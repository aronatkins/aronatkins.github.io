# Driving [http://aronatkins.github.io](http://aronatkins.github.io)

This repository contains the code used to generate my blog at
[http://aronatkins.github.io](http://aronatkins.github.io).

## Installation

Some notes from setting this all up. 

### install rbenv & ruby-build
```
cd ~/dev/git
git clone https://github.com/sstephenson/rbenv.git
ln -s ~/dev/git/rbenv ~/.rbenv

git clone https://github.com/sstephenson/ruby-build.git
mkdir -p ~/.rbenv/plugins
ln -s ~/dev/git/ruby-build ~/.rbenv/plugins/ruby-build
```

### Add the following to ~/.bash_profile
```
# rbenv
export PATH=$HOME/.rbenv/bin:$PATH
eval "$(rbenv init -)"
```

### configure .ruby-version
```
echo "2.1.5" > .ruby-version
```

### install ruby
```
rbenv install 2.1.5
```

### install bundler
```
gem install bundler
```

### configure Gemfile

After first using the jekyll gem directly, I found the `github-pages` gem and
shifted to using that.

```
source 'https://rubygems.org'

require 'json'
require 'open-uri'
versions = JSON.parse(open('https://pages.github.com/versions.json').read)

gem 'github-pages', versions['github-pages']
```

### fetch ruby-build-github

I installed `ruby-build-github` to try and keep a very similar stack to what
github promotes in their docs, but the 2.1.0 version did not build due to
well-documented problems compiling the openssl `cms.pod` documentation file.

Continued using 2.1.5 as that compiles cleanly; leaving these instructions
so I can occasionally revisit this step.
```
git clone git://github.com/parkr/ruby-build-github.git
mkdir -p ~/.rbenv/plugins
ln -s ~/dev/git/ruby-build-github ~/.rbenv/plugins/ruby-build-github
```

### Make the blog

Force jekyll to create a new blog hierarchy. This let me have the rbenv
`.ruby-version` file and the dependencies for building the blog in a `Gemfile`
present. Otherwise, `jekyll new` requires an empty directory.

```
jekyll new --force .
```

## License

The following directory and its contents are Copyright Aron Atkins. You may
not reuse anything therein without my permission.
* _posts/

All other directories are MIT Licensed. Feel free to use/adapt the HTML/CSS as
you please. A link back to
(https://github.com/aronatkins)[https://github.com/aronatkins] would be
appreciated, but is not required.
