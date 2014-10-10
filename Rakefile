$:.unshift File.expand_path('../lib', __FILE__)
$:.unshift File.expand_path('../build_tools', __FILE__)
require "moj_template/version"
require "gem_publisher"

desc "Compile template and assets from ./source into ./app"
task :compile do
  require 'compiler/asset_compiler'
  puts "Compiling assets and templates into ./app"
  Compiler::AssetCompiler.compile
end

desc "Build all versions"
task :build => ["build:gem", "build:tar", "build:play", "build:mustache", "build:mustache_inheritance", "build:liquid", "build:django", "build:jinja"]

namespace :build do
  desc "Build moj_template-#{MojTemplate::VERSION}.gem into the pkg directory"
  task :gem => :compile do
    puts "Building ./pkg/moj_template-#{MojTemplate::VERSION}.gem"
    require 'packager/gem_packager'
    Packager::GemPackager.build
  end

  desc "Build moj_template-#{MojTemplate::VERSION}.tgz into the pkg directory"
  task :tar => :compile do
    puts "Building ./pkg/moj_template-#{MojTemplate::VERSION}.tgz"
    require 'packager/tar_packager'
    Packager::TarPackager.build
  end

  desc "Build play_moj_template-#{MojTemplate::VERSION}.tgz into the pkg directory"
  task :play => :compile do
    puts "Building ./pkg/play_moj_template-#{MojTemplate::VERSION}.tgz"
    require 'packager/play_packager'
    Packager::PlayPackager.build
  end

  desc "Build mustache_moj_template-#{MojTemplate::VERSION} into the pkg directory"
  task :mustache => :compile do
    puts "Building ./pkg/mustache_moj_template-#{MojTemplate::VERSION}"
    require 'packager/mustache_packager'
    Packager::MustachePackager.build
  end

  desc "Build mustache_inheritance_moj_template-#{MojTemplate::VERSION} into the pkg directory"
  task :mustache_inheritance => :compile do
    puts "Building ./pkg/mustache_inheritance_moj_template-#{MojTemplate::VERSION}"
    require 'packager/mustache_inheritance_packager'
    Packager::MustacheInheritancePackager.build
  end

  desc "Build liquid_moj_template-#{MojTemplate::VERSION} into the pkg directory"
  task :liquid => :compile do
    puts "Building ./pkg/liquid_moj_template-#{MojTemplate::VERSION}"
    require 'packager/liquid_packager'
    Packager::LiquidPackager.build
  end

  desc "Build django_moj_template-#{MojTemplate::VERSION} into the pkg directory"
  task :django => :compile do
    puts "Building ./pkg/django_moj_template-#{MojTemplate::VERSION}"
    require 'packager/django_packager'
    Packager::DjangoPackager.build
  end

  desc "Build jinja_moj_template-#{MojTemplate::VERSION} into the pkg directory"
  task :jinja => :compile do
    puts "Building ./pkg/jinja_moj_template-#{MojTemplate::VERSION}"
    require 'packager/jinja_packager'
    Packager::JinjaPackager.build
  end
end

desc "Build and release gem to gemfury if version has been updated"
task :release => :build do
  p = GemPublisher::Publisher.new('moj_template.gemspec')
  if p.version_released?
    puts "moj_template-#{MojTemplate::VERSION} already released. Not pushing."
  else
    puts "Pushing moj_template-#{MojTemplate::VERSION} to rubygems"
    p.pusher.push "pkg/moj_template-#{MojTemplate::VERSION}.gem", :rubygems
    p.git_remote.add_tag "v#{MojTemplate::VERSION}"
    puts "Done."
  end

  require 'publisher/django_publisher'
  q = Publisher::DjangoPublisher.new
  if q.version_released?
    puts "django_moj_template-#{MojTemplate::VERSION} already released. Not pushing."
  else
    puts "Publishing django_moj_template-#{MojTemplate::VERSION}..."
    q.publish
    puts "Done."
  end

#   require 'publisher/play_publisher'
#   q = Publisher::PlayPublisher.new
#   if q.version_released?
#     puts "moj_template_play #{MojTemplate::VERSION} already released. Not pushing."
#   else
#     puts "Pushing moj_template_play #{MojTemplate::VERSION} to git repo"
#     q.publish
#   end

#   require 'publisher/mustache_publisher'
#   q = Publisher::MustachePublisher.new
#   if q.version_released?
#     puts "moj_template_mustache #{MojTemplate::VERSION} already released. Not pushing."
#   else
#     puts "Pushing moj_template_mustache #{MojTemplate::VERSION} to git repo"
#     q.publish
#   end
end

require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec)
task :spec => :compile
task :default => :spec
