---
title: jekyll, markdown, and source highlighting
date: '2015-04-30'
layout: post
tags: jekyll markdown highlight.js
---

Here is how I added code styling to this blog. Unfortunately, the code is only
styled when you visit the site; the RSS feeds will have unstyled code blocks.

I decided to use [highlight.js](https://highlightjs.org/) after struggling
with Jekyll configuration/extensions. In particular, I either couldn't get the
highlighting to work at all, or didn't find support for one or more languages
I've been touching.

The highlight.js package contains a number of default languages and many more
that can be added in. If you use the default set, you can use a CDN-hosted
copy. I wanted R, Go, and Lisp which demanded a
[custom build](https://highlightjs.org/download/).

The downloaded bundle contains a `highlight.pack.js` which I copied into a
`js/` directory within my Jekyll blog. I am using the sunburst theme, which I
copied into `css/highlight.css`.

In `_includes/head.html`, add the following:

```
<link rel="stylesheet" href="{{ "/css/highlight.css" | prepend: site.baseurl }}">
<script src="{{ "/js/highlight.pack.js" | prepend: site.baseurl }}"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

That's it! Your code blocks now have syntax highlighting.

If you want to force language-specific code highlighting of a block, say what
language you are using:

~~~markdown
```lisp
(defun aron/git-grep () ...)
```
~~~

which is rendered like:

```lisp
(defun aron/git-grep () ...)
```

If you don't specify a lanuage identifier, highlight.js will still apply some
formatting to your block. If you don't want that, use the `nohighlight` tag:

~~~markdown
```nohighlight
this shall not be formatted
```
~~~

which is rendered like:

```nohighlight
this shall not be formatted
```
