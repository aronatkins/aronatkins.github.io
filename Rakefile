require 'html/proofer'
require 'highline/import'

require 'json'
require 'open-uri'
require 'github-pages'

task :default => :server

desc 'Check github-pages version'
task :version do
  latest = JSON.parse(open('https://pages.github.com/versions.json').read)['github-pages']
  if (GitHubPages::VERSION).to_s != latest
    puts "You have github-pages #{GitHubPages::VERSION} but the latest is #{latest}. Update your Gemfile."
  else
    puts "github-pages #{GitHubPages::VERSION} is up-to-date."
  end
end

desc 'Build site with Jekyll.'
task :build do
  jekyll 'build --config _config.yml,_config_dev.yml'
end
 
desc 'Start server with --auto.'
task :serve do
  jekyll 'serve --drafts --config _config.yml,_config_dev.yml'
end
 
desc 'Remove all built files.'
task :clean do
  sh 'rm -rf _site'
end
 
def jekyll(opts = '')
#  Rake::Task['clean'].execute
  sh 'bundle exec jekyll ' + opts
end

desc 'test that HTML is well-formed.'
task :test => [ :clean, :build ] do
  opts = {
    :href_ignore => [ "#" ],
    :disable_external => true
  }
  # example of other options:
  # https://github.com/wet-boew/wet-boew/blob/master/Rakefile
  HTML::Proofer.new("./_site", opts).run
end

task :post do
  date = Time.now.strftime("%Y-%m-%d")
  date = ask("Date: ") { |q| q.default = date }
  title = ask("Title: ")

  under_title = title.downcase.tr('/,.!?.:;`"\'+',' ').strip.tr_s(' ','-')
  filename = "#{date}-#{under_title}.markdown"
  filename = ask("Filename: ") { |q| q.default = filename }

  options = {
    'title' => title.to_s,
    'date' => date.to_s,
    'layout' => 'post',
    'tags' => nil
  }
  make_article('_posts', 'article', filename, options)
end

task :draft do
  title = ask("Title: ")

  under_title = title.downcase.tr(',.!?.:;',' ').strip.tr_s(' ','-')
  filename = "#{under_title}.markdown"
  filename = ask("Filename: ") { |q| q.default = filename }

  # A little annoying. Wanted to have a nil 'date' placeholder in the YAML,
  # but Jekyll couldn't handle an empty date specification.
  options = {
    'title' => title.to_s,
    'layout' => 'post',
    'tags' => nil
  }
  make_article('_drafts', 'draft article', filename, options)
end

def make_article(directory, thing, filename, options)
  article = options.to_yaml
  article << "---\n\n"

  path = "#{directory}/#{filename}"
  unless File.exist?(path)
    File.open(path, "w") do |file| 
      file.write article
    end
    
    puts "A new #{thing} was created at #{path}."
    # TODO: push path to an editor.
  else
    puts "There was an error creating the #{thing}, #{path} already exists."
  end
end

