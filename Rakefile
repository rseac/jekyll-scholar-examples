# Require bundler
#
require 'bundler'
begin
  Bundler.setup(:default, :development)
rescue Bundler::BundlerError => e
  $stderr.puts e.message
  $stderr.puts "Run `bundle install` to install missing gems"
  exit e.status_code
end

#
task :default => :build

task :build do
   system "bundle exec jekyll build"
end 


# Task to build separate directories
require 'jekyll'
require 'jekyll/scholar'
require 'tmpdir'


examples = 'examples'
TEST_DIR = File.join(Dir.tmpdir, examples)

def prepend_test_dir(options, key)
  if options.key?(key)
    if Pathname(options[key]).relative?
      options[key] = File.join(TEST_DIR, options[key])
    end
  else
    options[key] ||= TEST_DIR
  end
end

def before(dest, src)
   puts dest + " \t " + src
   FileUtils.rm_rf(dest) if File.exist?(dest)
   FileUtils.mkdir_p(dest)
   FileUtils.cp_r(src, dest)
   Dir.chdir(dest)
end

def after(dest)
  FileUtils.rm_rf(dest) if File.exist?(dest)
end

def print_options(options)
   puts "# source      : " + options['source']
   puts "# destination : " + options['destination']
end 

task :build_all do 
   options = Jekyll.configuration({})
   root = options['source']
   # Set the destination to temp.
   Dir.each_child(examples) { |dir|
	  src = root + '/' + examples + '/' + dir + '/'
	  dest = TEST_DIR + '/' + dir
	  if Dir.exist?(src)
		 run_options = {}
		 run_options['source'] = src
		 run_options['destination'] = dest
		 print_options(run_options)
		 before(dest, src)
		 Jekyll.configuration(run_options)
		 Jekyll::Commands::Build.process(run_options)
	  end
   }
end 

