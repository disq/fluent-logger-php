require 'nokogiri'
require 'json'

namespace :fluent do

  desc "check testcase"
  task :test do
    `phpunit`
  end

  task :default do
  end
  
  desc "create pear package"
  task :package do
    log "# creating package...\n"
    
    io = open("|pear package")
    while !io.eof
      tmp = io.gets
      print tmp
    end
    io.close
    /^Package (.+?) done/ =~ tmp
    
    ENV['PACKAGE_NAME'] = $1
    FileUtils::mkdir_p("builds")
    FileUtils::mv($1, "builds")
  end
  
  desc "install fluent-logger-php via pear"
  task :install => [:package] do
    log "# install #{ENV['PACKAGE_NAME']} with pear\n"
    Dir::chdir("builds") do 
      io = open("|pear install #{ENV['PACKAGE_NAME']}")
      while !io.eof
        print io.gets
      end
      io.close
    end
  end
  
  desc "remove packaged files."
  task :clean do
    Dir::chdir("builds") do 
      Dir::foreach(".") do |file|
        if !FileTest.directory?(file) then
          File.delete(file)
        end
      end
    end
  end
  
  def log(message)
    print message
  end

end