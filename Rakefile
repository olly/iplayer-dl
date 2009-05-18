require 'rake/testtask'
require 'rake/packagetask'
require 'rake/rdoctask'
require 'rake'
require 'find'
require 'lib/iplayer/version'

# Globals

PKG_NAME = 'iplayer-dl'

PKG_FILES = %w[ COPYING README setup.rb Rakefile ]
Find.find('lib/', 'test/', 'bin/', 'share/') do |f|
  if FileTest.directory?(f) and f =~ /\.svn/
    Find.prune
  else
    PKG_FILES << f
  end
end

EXE_FILES = PKG_FILES + %w[ application.ico init.rb ]

# Tasks

task :default => :test

Rake::TestTask.new do |t|
  t.libs << "test" 
  t.test_files = FileList['test/test_*.rb']
end

Rake::PackageTask.new(PKG_NAME, IPlayer::VERSION) do |p|
  p.need_tar_gz = true
  p.package_files = PKG_FILES
end

desc "Build a Windows executable. Needs rubyscript2exe.rb in the current path and the wx gem installed."
task :exe do |t|
  mkdir_p 'tmp'
  build_dir = File.join('tmp', "ipdl-#{IPlayer::GUI_VERSION}")
  rm_rf build_dir
  mkdir build_dir
  EXE_FILES.each do |file|
    next if File.directory?(file)
    loc = File.join(build_dir, File.dirname(file))
    mkdir_p loc
    cp_r file, loc
  end
  sh "ruby rubyscript2exe.rb #{build_dir} --rubyscript2exe-verbose --rubyscript2exe-rubyw"
  rm_rf build_dir
  mkdir_p 'pkg'
  mv "ipdl-#{IPlayer::GUI_VERSION}.exe", "pkg"
end

begin
  require 'rubygems'
  require 'jeweler'
  Jeweler::Tasks.new do |gemspec|
    gemspec.name = "iplayer-dl"
    gemspec.summary = "Downloads DRM-free video (h.264) and audio (MP3) files from the BBC iPlayer service by pretending to be an iPhone."
    gemspec.homepage = "http://po-ru.com/projects/iplayer-downloader/"
    gemspec.description = "Downloads DRM-free video (h.264) and audio (MP3) files from the BBC iPlayer service by pretending to be an iPhone."
    gemspec.authors = ["Paul Battley"]
  end
rescue LoadError
  puts "Jeweler not available. Install it with: sudo gem install technicalpickles-jeweler -s http://gems.github.com"
end