require 'html/proofer'
require 'highline/import'

task :default => :server
 
desc 'Build site with Jekyll.'
task :build do
  jekyll 'build'
end
 
desc 'Start server with --auto.'
task :serve do
  jekyll 'serve'
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

  under_title = title.downcase.tr(',.!?.:;',' ').strip.tr_s(' ','-')
  filename = "#{date}-#{under_title}.markdown"
  filename = ask("Filename: ") { |q| q.default = filename }

  options = {
    'title' => title.to_s,
    'date' => date.to_s,
    'layout' => 'post',
    'categories' => nil
  }
  article = options.to_yaml
  article << "---"

  path = "_posts/#{filename}"
  unless File.exist?(path)
    File.open(path, "w") do |file| 
      file.write article
    end
    
    puts "A new article was created at #{path}."
    # TODO: push path to an editor.
  else
    puts "There was an error creating the article, #{path} already exists."
  end
end
