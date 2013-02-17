rule ".html" => [".markdown", "layout.erb"] do |t|
  require 'kramdown'
  require 'erb'
  markdown = Kramdown::Document.new(File.read(t.source))
  template = ERB.new(File.read("layout.erb"))
  content = markdown.to_html
  html = template.result(binding)
  puts "#{t.source} => #{t.name}"
  File.open(t.name, "w") { |f| f.write html }
end

rule ".css" => ".scss" do |t|
  sh "bundle exec sass #{t.source} #{t.name}"
end

task :build => ["index.html", "style.css"]

task :clean do
  sh "rm *.html *.css"
end

task :upload do
  sh "rsync -t * lazyatom@lazyatom.com:sites/lazyatom.com/www"
end

task :publish => [:build, :upload]

task :default => :publish
