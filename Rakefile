require 'rubygems'
require 'rake'
require 'rake/testtask'
require 'rake/packagetask'
require 'rubygems/package_task'
require 'rspec/core/rake_task'
require 'spree_core/testing_support/common_rake'

RSpec::Core::RakeTask.new

desc "Default Task"
task :default => [:spec]

spec = eval(File.read('spree_batch_products.gemspec'))

Gem::PackageTask.new(spec) do |p|
  p.gem_spec = spec
end


desc "Release to gemcutter"
task :release => :package do
  require 'rake/gemcutter'
  Rake::Gemcutter::Tasks.new(spec).define
  Rake::Task['gem:push'].invoke
end

desc "Regenerates a rails 3 app for testing"
task :test_app do
  ENV['LIB_NAME'] = 'spree_batch_products'
  Rake::Task['common:test_app'].invoke
end

namespace :test_app do
  desc 'Rebuild test database'
  task :rebuild_dbs do
    system("cd spec/test_app && bundle exec rake db:drop db:migrate RAILS_ENV=test")
  end
end
