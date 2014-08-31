require 'rubygems'
require "bundler/setup"
require 'stringex'

#Environment variables

source_dir    = "."
posts_dir     = "_posts"
new_post_ext  = "md"
new_page_ext  = "md"

task :default do
  system "jekyll build && jekyll serve --watch"
end

task :start do
  system "jekyll build && jekyll serve --watch"
end

task :new_post, :title do |t, args|
  puts "#{args}"
  if args.title
    title = args.title
  else
    title = get_stdin("Enter a title for your post: ")
  end
  raise "### You haven't set anything up yet. First run `rake install` to set up an Octopress theme." unless File.directory?(source_dir)
  mkdir_p "#{source_dir}/#{posts_dir}"
  filename = "#{source_dir}/#{posts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title.to_url}.#{new_post_ext}"
  puts "create file #{filename}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/&/,'&amp;')}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M:%S %z')}"
    post.puts "comments: true"
    post.puts "categories: "
    post.puts "---"
  end
  
end

task :new_page, :filename do |t, args|
  puts "#{args}"
  raise "### You haven't set anything up yet. First run `rake install` to set up an Octopress theme." unless File.directory?(source_dir)
  args.with_defaults(:filename => 'new-page')
  page_dir = [source_dir]
  
  if args.filename.downcase =~ /(^.+\/)?(.+)/
    filename, dot, extension = $2.rpartition('.').reject(&:empty?)         # Get filename and extension
    title = filename
    page_dir.concat($1.downcase.sub(/^\//, '').split('/')) unless $1.nil?  # Add path to page_dir Array
    if extension.nil?
      page_dir << filename
      filename = "index"
    end

    puts "Create file at :#{page_dir}"
    extension ||= new_page_ext
    # page_dir = page_dir.map! { |d| d = d.to_url }.join('/')                # Sanitize path
    page_dir = page_dir.join("/")

    filename = filename.downcase.to_url

    puts "Create file at :#{page_dir}"

    mkdir_p page_dir
    file = "#{page_dir}/#{filename}.#{extension}"

    if File.exist?(file)
      abort("rake aborted!") if ask("#{file} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
    end
    puts "Creating new page: #{file}"
    open(file, 'w') do |page|
      page.puts "---"
      page.puts "layout: page"
      page.puts "title: \"#{title}\""
      page.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M')}"
      page.puts "comments: true"
      page.puts "sharing: true"
      page.puts "footer: true"
      page.puts "---"
    end
  else
    puts "Syntax error: #{args.filename} contains unsupported characters"
  end
end

task :list_posts do
  system "ls -ls " + source_dir + "/" + posts_dir
end
