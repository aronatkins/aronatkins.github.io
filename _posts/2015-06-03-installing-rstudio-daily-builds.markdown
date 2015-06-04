---
title: Installing RStudio daily builds
date: '2015-06-03'
layout: post
tags: rstudio mac osx engineering
---

As a member of the team at [RStudio](http://rstudio.com), I install the daily
build of our
[open source RStudio IDE](http://www.rstudio.com/products/rstudio/) almost
every morning.

There are published ways to download and install these builds
[for Linux](http://stackoverflow.com/a/15046636) but that doesn't work for me.

My main development machine is a Macbook Pro, and, well, 
[we're different](http://en.wikipedia.org/wiki/Think_different).

After far too many iterations of "*google for 'rstudio daily mac' then
download the DMG then mount it then copy the application to `/Applications`*",
I put on my "Let's write a script for that!" hat and ... wrote a script.

You can find the latest version [as a GitHub Gist](https://gist.github.com/aronatkins/ac3934e08d2961285bef) 
and embedded below (the embed will not appear in RSS or in non-Javascript
viewers). It fetches the latest daily RStudio IDE build from our download
site, replacing any existing install.

To use this script, download it to your own machine, make it executable, and
run it. Use it every day!

```nohighlight
# Mark executable after downloading.
$ chmod +x install-rstudio-daily.sh
# Every day!
$ ./install-rstudio-daily.sh
Downloading daily build from: https://s3.amazonaws.com:443/rstudio-dailybuilds/RStudio-0.99.579.dmg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 57.5M  100 57.5M    0     0  3374k      0  0:00:17  0:00:17 --:--:-- 3561k
Installed RStudio-0.99.579 to /Applications
```

<script src="https://gist.github.com/aronatkins/ac3934e08d2961285bef.js"></script>
