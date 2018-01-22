# Require bundler
#
#require 'bundler'
#begin
#  Bundler.setup(:default, :development)
#rescue Bundler::BundlerError => e
#  $stderr.puts e.message
#  $stderr.puts "Run `bundle install` to install missing gems"
#  exit e.status_code
#end
#
#task :default => :build
#
#task :build do
#   system "bundle exec jekyll build"
#end 
#
#
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
   options = {}
   options = Jekyll.configuration(options)
   root = options['source']
   # Set the destination to temp.
   Dir.each_child(examples) { |d|
      src = root + '/' + examples + '/' + d + '/'
      dest = TEST_DIR + '/' + d
      puts "directory: " + d + ", src: " + src 
      if Dir.exist?(src)
         run_options = options
         prepend_test_dir(run_options, 'source')
         prepend_test_dir(run_options, 'destination')
         run_options['source'] = src
         run_options['destination'] = dest
         print_options(run_options)
         before(dest, src)
         Jekyll::Commands::Build.process(options)
      end
   }
end 

