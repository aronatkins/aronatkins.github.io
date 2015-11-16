---
title: 'go: tell me about slow channels'
date: '2015-11-15'
layout: post
tags: go golang channel deadlock
---

Go channels are a powerful construct for passing information between different
go routines, each with distinct responsibilities. Writing to an unbuffered
channel is a blocking operation which does not complete until a reader is
available at the other end.

The blocked write is not guaranteed to complete -- if the channel responsible
for the read is somehow blocked itself, you may have deadlock.

Here is a simple wrapper around a channel write that helps detect long-blocked
operations.

Some things you might want to change for own application:

* Writes taking longer than one second are identified as a stall. This is a
  constant in the code and not configurable.
* The `time.After` triggers each second of the stall but only does something
  the first time through. Maybe you'll want to log every so often that the
  send is still stalled.
* You might want to do something other than log a message when a stall is
  detected. Some ideas:
  * Print a stack trace.
  * Update a counter (maybe external to this function) so a health check fails.
  
```go
import (
	"log"
	"time"
)

func Emit(out chan<- interface{}, data interface{}) (ok bool) {
	var stallStart time.Time
	// If we stalled, print the duration of the stall as we exit.
	defer func() {
		if !stallStart.IsZero() {
			diff := time.Now().Sub(stallStart)
			log.Printf("Stall sending data %+v recovered after %0.2f seconds",
				data, diff.Seconds())
		}
	}()
	ok = true
	sendStart := time.Now()
	for {
		select {
		case out <- data:
			return
		case <-time.After(1 * time.Second):
			if stallStart.IsZero() {
				log.Printf("Stall sending data %+v", data)
				stallStart = sendStart
				ok = false
			}
		}
	}
}
```

Here is a simple `main` that shows this wrapper being used:

```go
func main() {
	out := make(chan interface{})
	go func() {
		<-time.After(2 * time.Second)
		<-out
	}()
	Emit(out, "slow-data")
}
```

When run, it outputs:

```
$ go run stall.go
2015/11/15 19:38:40 Stall sending data slow-data
2015/11/15 19:38:41 Stall sending data slow-data recovered after 2.00 seconds
```
