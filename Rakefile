rule ".html" => [".markdown", 'presentations.yml', 'presentations.erb', "layout.erb"] do |t|
  require 'kramdown'
  require 'erb'
  require 'yaml'
  markdown = Kramdown::Document.new(File.read(t.source))
  template = ERB.new(File.read("layout.erb"))
  content = markdown.to_html
  presentations = YAML.load_file('presentations.yml')
  presentation_html = ERB.new(File.read('presentations.erb')).result(binding)
  html = template.result(binding).gsub('PRESENTATIONS_INSERT', presentation_html)
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
