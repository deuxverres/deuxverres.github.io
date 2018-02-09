require 'rake'
require 'colorize'
require 'neocities/client'
require 'chamber'

# Default task is compiling
task :default => :compile

files = nil
minify = true
port = 8081

# Prerequisite tasks
task :stylus do
	# Look .stylus files
	sF = Rake::FileList.new('**/content/*.stylus') do |f|
		f.exclude('~*')
		f.exclude('.*')
	end

	if sF.length==0 then len = 'no' else len = sF.length end
	len = sF.length
	puts "Compiling #{len} .stylus file(s)".cyan.bold

	sF.each do |file|
		output = 'output/'+File.basename(file,'.stylus')+'.css'
		if minify then
			min = '-c '
		else 
			min = ''
		end
		command = "stylus #{min}< #{file} > #{output}"
		`#{command}`
	end
end

task :blade do
	# Look .blade files
	sF = Rake::FileList.new('**/content/*.blade') do |f|
		f.exclude('~*')
		f.exclude('.*')
	end

	if sF.length==0 then len = 'no' else len = sF.length end
	puts "Compiling #{len} .blade file(s)".cyan.bold
		
	sF.each do |file|
		output = 'output/'+File.basename(file,'.blade')+'.html'
		if minify then
			min = '-m '
		else
			min = ''
		end
		command = "blade #{min}#{file} #{output}"
	 `#{command}`
	end
end

task :coffee do
	sF = Rake::FileList.new('**/content/*.coffee/') do |f|
		f.exclude('~*')
		f.exclude('.*')
	end

	if sF.length==0 then len = 'no' else len = sF.length end
	puts "Compiling #{len} .coffee file(s)".cyan.bold

	sF.each do |file|
		output = 'output/'+File.basename(file,'.coffee')+'.js'
		command = "coffee -p #{file} > #{output}"
		`#{command}`
		if minify then
			`minify --output #{output} #{output}`
		end
	end		
end

task :server do
	puts "Opening server at localhost:#{port}".cyan.bold
	`http-server ./output -p #{port} -c-1 `
end

task :push do
	system "cp -r assets output"
	system "git add ."
	system "git commit"
	system "git push origin master"
end

# Normal tasks
task :compile => [:stylus, :blade, :coffee] do
end

task :deploy => [:compile, :server] do
end

task :publish => [:compile, :push] do
end