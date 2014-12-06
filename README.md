aronatkins.github.io
====================

Some notes from setting this all up. 

# install rbenv
```
cd ~/dev/git
git clone https://github.com/sstephenson/rbenv.git
ln -s ~/dev/git/rbenv ~/.rbenv
```

# install ruby-build
```
git clone https://github.com/sstephenson/ruby-build.git
mkdir -p ~/.rbenv/plugins
ln -s ~/dev/git/ruby-build ~/.rbenv/plugins/ruby-build
```

# update ~/.bash_profile:
```
# rbenv
export PATH=$HOME/.rbenv/bin:$PATH
eval "$(rbenv init -)"
```

# install ruby
```
rbenv install 2.1.5
```

# configure .ruby-version
```
echo "2.1.5" > .ruby-version
```

# install bundler
```
gem install bundler
```

# configure Gemfile
```
source 'https://rubygems.org'
gem 'jekyll', '2.5.2'
```

# rewrite Gemfile
Found the `github-pages` gem after reading through more of the jekyll docs.
Shifted to using that.

```
source 'https://rubygems.org'

require 'json'
require 'open-uri'
versions = JSON.parse(open('https://pages.github.com/versions.json').read)

gem 'github-pages', versions['github-pages']
```

# fetch ruby-build-github
```
git clone git://github.com/parkr/ruby-build-github.git
mkdir -p ~/.rbenv/plugins
ln -s ~/dev/git/ruby-build-github ~/.rbenv/plugins/ruby-build-github
```

Wasn't able to get the github releases to build due to syntax errors in the
cms.pod file. Went back to 2.1.5 not using the github ruby-build.

# Make the blog

```jekyll new --force .```

This let me have the rbenv `.ruby-version` file and the dependencies for
building the blog in a `Gemfile` present. Otherwise, `jekyll new` requires an
empty directory.
