---
title: 'go: email with attachments'
date: '2015-04-30'
layout: post
tags: go golang gomail email attachment
---

Here is an example showing how to use the
[gomail](https://godoc.org/gopkg.in/gomail.v1) package to produce email
containing plain-text content, alternative html content with an embedded
image, and an HTML attachment.

It doesn't worry about trying to actually send mail but only tries to create a
well-composed message. See the
[documentation](https://godoc.org/gopkg.in/gomail.v1) or
[repository](https://github.com/go-gomail/gomail) on GitHub for more details.

```go
package main

import (
    "encoding/base64"
    "fmt"
    "net/smtp"

    "gopkg.in/gomail.v1"
)

func main() {
    m := gomail.NewMessage()
    m.SetHeaders(map[string][]string{
        "From":    {"alice@foobar.com"},
        "To":      {"bob@foobar.com"},
        "Subject": {"inbound crap"},
    })
    m.SetBody("text/plain", "hello. i am sending you crap.")
    // from: http://probablyprogramming.com/2009/03/15/the-tiniest-gif-ever
    blankEncoded := "R0lGODlhAQABAIABAP///wAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw=="
    // i'm a bad person. ignoring errors!
    blankContent, _ := base64.StdEncoding.DecodeString(blankEncoded)
    blank := gomail.CreateFile("blank.gif", blankContent)
    m.Embed(blank)
    m.AddAlternative("text/html", `<p>hello. i am sending you <img border="2" src="cid:blank.gif" alt="blank" /> crap.</p>`)
    crap := gomail.CreateFile("crap.html", []byte("<!doctype html>\n<html><title>crap</title><body>this is crap, y'know?</body></html>"))
    m.Attach(crap)

    mailDumper := func(addr string, a smtp.Auth, from string, to []string, msg []byte) error {
        // This dumps out the mail message so it can piped straight to `sendmail -t`
        // and proves that this is a parsable message.
        fmt.Print(string(msg))
        return nil
    }
    mailer := gomail.NewMailer("localhost", "user", "pass", 465, gomail.SetSendMail(mailDumper))
    mailer.Send(m)
}
```

Here is the resulting message:

```nohighlight
Date: Wed, 29 Apr 2015 16:33:56 -0400
Content-Type: multipart/mixed; boundary=21ee3da964c7bf70def62adb9ee1a061747003c026e363e47231258c48f1
From: alice@foobar.com
To: bob@foobar.com
Subject: inbound crap
Mime-Version: 1.0

--21ee3da964c7bf70def62adb9ee1a061747003c026e363e47231258c48f1
Content-Type: multipart/related; boundary=76a1282373c08a65dd49db1dea2c55111fda9a715c89720a844fabb7d497

--76a1282373c08a65dd49db1dea2c55111fda9a715c89720a844fabb7d497
Content-Type: multipart/alternative; boundary=4186c39e13b2140c88094b3933206336f2bb3948db7ecf064c7a7d7473f2

--4186c39e13b2140c88094b3933206336f2bb3948db7ecf064c7a7d7473f2
Content-Type: text/plain; charset=UTF-8
Content-Transfer-Encoding: quoted-printable

hello. i am sending you crap.
--4186c39e13b2140c88094b3933206336f2bb3948db7ecf064c7a7d7473f2
Content-Type: text/html; charset=UTF-8
Content-Transfer-Encoding: quoted-printable

<p>hello. i am sending you <img border=3D"2" src=3D"cid:blank.gif" alt=3D"bl=
ank" /> crap.</p>
--4186c39e13b2140c88094b3933206336f2bb3948db7ecf064c7a7d7473f2--

--76a1282373c08a65dd49db1dea2c55111fda9a715c89720a844fabb7d497
Content-Type: image/gif; name="blank.gif"
Content-Transfer-Encoding: base64
Content-Disposition: inline; filename="blank.gif"
Content-ID: <blank.gif>

R0lGODlhAQABAIABAP///wAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
--76a1282373c08a65dd49db1dea2c55111fda9a715c89720a844fabb7d497--

--21ee3da964c7bf70def62adb9ee1a061747003c026e363e47231258c48f1
Content-Type: text/html; charset=utf-8; name="crap.html"
Content-Transfer-Encoding: base64
Content-Disposition: attachment; filename="crap.html"

PCFkb2N0eXBlIGh0bWw+CjxodG1sPjx0aXRsZT5jcmFwPC90aXRsZT48Ym9keT50aGlzIGlzIGNy
YXAsIHkna25vdz88L2JvZHk+PC9odG1sPg==
--21ee3da964c7bf70def62adb9ee1a061747003c026e363e47231258c48f1--
```
