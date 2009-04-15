$:.unshift 'lib'
require 'rubygems'
require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'
require 'spec/rake/spectask'

REDIS_RAILS_VERSION = "0.0.2"

task :default => :spec

desc 'Build and install the gem (useful for development purposes).'
task :install do
  system "gem build redis-rails.gemspec"
  system "sudo gem uninstall redis-rails"
  system "sudo gem install --local --no-rdoc --no-ri redis-rails-#{REDIS_RAILS_VERSION}.gem"
  system "rm redis-rails-*.gem"
end

Spec::Rake::SpecTask.new do |t|
  t.spec_files = FileList['spec/**/*_spec.rb']
  t.spec_opts = %w(-fs --color)
end

desc 'Show the file list for the gemspec file'
task :files do
  puts "Files:\n #{Dir['**/*'].reject {|f| File.directory?(f)}.sort.inspect}"
  puts "Test files:\n #{Dir['spec/**/*_spec.rb'].reject {|f| File.directory?(f)}.sort.inspect}"
end

namespace :redis do
  desc 'Start the Redis cluster'
  task :start => :clean do
    system "redis-server spec/config/single.conf"
    system "redis-server spec/config/master.conf"
    system "redis-server spec/config/slave.conf"
  end

  desc 'Stop the Redis cluster'
  task :stop do
    # TODO replace with:
    # system "kill -9 `tmp/redis-single.pid`"
    # system "kill -9 `tmp/redis-master.pid`"
    # system "kill -9 `tmp/redis-slave.pid`"
    system "ps -eo pid,comm | grep redis | xargs kill -9"
  end

  desc 'Clean the tmp/ directory'
  task :clean do
    system "rm tmp/*"
  end
end
