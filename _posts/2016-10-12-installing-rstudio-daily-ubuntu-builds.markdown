---
title: Installing RStudio daily builds (Ubuntu)
date: '2016-10-12'
layout: post
tags: rstudio linux ubuntu engineering
---

The
[script that I previously published]({{site.url}}/2015/06/03/installing-rstudio-daily-builds.html) to
handle OSX installs has been updated to also handle installing RStudio daily
desktop builds on Ubuntu Linux hosts.

You need to install either `curl` or `wget` in order to use this script. It
only understands Ubuntu installation and uses `dpkg` to perform the install.
If run as a non-`root` user, `sudo` is used when running `dpkg`. 

You may need to follow this script with a `apt-get install -f` to install
RStudio package dependencies (using `sudo` if appropriate).

You can always find the latest version [as a GitHub Gist](https://gist.github.com/aronatkins/ac3934e08d2961285bef) 
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
Downloading daily build from: https://www.rstudio.org/download/latest/daily/desktop/ubuntu64/rstudio-latest-amd64.deb
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   335  100   335    0     0    642      0 --:--:-- --:--:-- --:--:--   642
100 94.1M  100 94.1M    0     0  3383k      0  0:00:28  0:00:28 --:--:-- 3599k
Installing /tmp/rstudio-latest-amd64.deb
Selecting previously unselected package rstudio.
(Reading database ... 8837 files and directories currently installed.)
Preparing to unpack /tmp/rstudio-latest-amd64.deb ...
Unpacking rstudio (1.0.39) ...
Setting up rstudio (1.0.39) ...
Processing triggers for shared-mime-info (1.5-2ubuntu0.1) ...
```

<script src="https://gist.github.com/aronatkins/ac3934e08d2961285bef.js"></script>
