require 'rake'
require 'rake/testtask'
require 'fileutils'

desc "Clean the generated build files"
task :clean do |t|
  Dir.chdir('ext') do
    sh 'make distclean' rescue nil
    rm 'proc/wait3.' + Config::CONFIG['DLEXT'] rescue nil
    rm_rf 'conftest.dSYM' if File.exists?('conftest.dSYM') # OS X
  end
end

desc "Build the source (but don't install it)"
task :build => [:clean] do |t|
  Dir.chdir('ext') do
    ruby 'extconf.rb'
    sh 'make'
    FileUtils.mv 'wait3.' + Config::CONFIG['DLEXT'], 'proc'
  end
end

desc "Install the proc-wait3 library"
task :install => [:build] do |t|
  sh 'make install'
end

desc "Build the gem"
task :gem do
  spec = eval(IO.read('proc-wait3.gemspec'))
  Gem::Builder.new(spec).build
end

namespace :example do
  desc 'Run the Process.getrusage example program'
  task :getrusage => [:build] do
    ruby '-Iext examples/example_getrusage.rb'
  end

  desc 'Run the Process.pause example program'
  task :pause => [:build] do
    ruby '-Iext examples/example_pause.rb'
  end

  desc 'Run the Process.wait3 example program'
  task :wait3 => [:build] do
    ruby '-Iext examples/example_wait3.rb'
  end

  desc 'Run the Process.wait4 example program'
  task :wait4 => [:build] do
    ruby '-Iext examples/example_wait4.rb'
  end

  desc 'Run the Process.waitid example program'
  task :waitid => [:build] do
    ruby '-Iext examples/example_waitid.rb'
  end
end

Rake::TestTask.new do |t|
  task :test => [:build]
  t.libs << 'ext'
  t.warning = true
  t.verbose = true
end

task :default => :test
