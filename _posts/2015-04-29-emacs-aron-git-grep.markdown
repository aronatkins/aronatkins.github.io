---
title: 'emacs: aron/git-grep'
date: '2015-04-29'
layout: post
tags: emacs git grep
---

There are
[lots of suggestions](https://www.google.com/search?q=emacs+git+grep) how to
expose `git grep` in emacs.

[Here is mine](https://github.com/aronatkins/emacs.d/blob/master/lisp/aron-grep.el),
inspired by a recent blog post to
[oremacs](http://oremacs.com/2015/04/19/git-grep-ivy/). You are interactively
asked for a search term and a pathspec; accepting the defaults will search for
a symbol at your current cursor position across the entire git repository.

```elisp
(defvar aron/git-grep-symbol-history nil
  "Internal variable for aron-grep; do not modify")
(defvar aron/git-grep-pathspec-history nil
  "Internal variable for aron-grep; do not modify")
(defun aron/git-grep (search-for pathspec)
  "Grep for a string in the current git repository."
  (interactive (list 
                (read-from-minibuffer "search for: "
                                      (thing-at-point 'symbol)
                                      nil nil
                                      'aron/git-grep-symbol-history)
                (read-from-minibuffer "pathspecs: "
                                      ""
                                      nil nil
                                      'aron/git-grep-pathspec-history)
                ))
  (let ((default-directory (locate-dominating-file default-directory ".git")))
    ; We don't use the grep function directly because it doesn't offer much
    ; besides adding /dev/null in unpredictable ways. We have a fully runnable
    ; compilation command; just apply grep-mode to the result.
    (compilation-start
     (format "git --no-pager grep -nH --full-name --no-color -i -e '%s' -- %s"
             search-for pathspec)
     'grep-mode)))
```

If I just want to search Go files, use a pathspec of `*.go`; Javascript with
`*.js`. I am occasionally unhappy that the search is case insensitive and may
rewrite that bit.

My personal emacs
[key bindings](https://github.com/aronatkins/emacs.d/blob/master/lisp/aron-keys.el)
often use `C-z` as a prefix (how is `C-z` useful to anyone in these modern times?).
I have `aron/git-grep` bound to `C-z j`.

```elisp
(defvar ctl-z-map (make-sparse-keymap) "Keymap for user extensions.")
(define-key ctl-z-map "j" 'aron/git-grep)

;; since we're changing the default meaning of C-z, add suspend back into the
;; fray with C-z C-z.
(define-key ctl-z-map "\C-z" 'suspend-emacs)

(global-set-key "\C-z" ctl-z-map)
```
