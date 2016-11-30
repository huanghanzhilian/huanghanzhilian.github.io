task :default => :new

require 'fileutils'

desc "创建新 post"
task :new do
    puts "请输入要创建的 post URL："
	@url = STDIN.gets.chomp
	
	
	
	@slug = "#{@url}"
	@slug = @slug.downcase.strip.gsub(' ', '-')
	@date = Time.now.strftime("%F")
	@post_name = "_posts/#{@date}-#{@slug}.md"
	if File.exist?(@post_name)
			abort("文件名已经存在！创建失败")
	end
	FileUtils.touch(@post_name)
	open(@post_name, 'a') do |file|
			file.puts "---"
			file.puts "layout: post"
			file.puts "title: 请输入 post 标题"
			file.puts "subtitle: 请输入 post 子标题"
			file.puts "author: 继小鹏"
			file.puts "date: #{Time.now}"
			file.puts "category: html"
			file.puts "tag: html"
			file.puts "---"
	end
	exec "vi #{@post_name}"
end