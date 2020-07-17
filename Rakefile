# This is free and unencumbered software released into the public domain.

require 'rake'
require 'yaml'

VERSION = File.read('VERSION').chomp

task default: %w(stub)

task :stub do
  puts "#!/bin/bash"
  classes = YAML.load(File.read('etc/classes.yaml'))
  classes.each do |class_name, class_info|
    class_info = class_info.transform_keys(&:to_sym)
    puts
    puts "# #{class_name}"
    puts "touch c/dogma/#{class_name.downcase}.h"
    puts "touch cpp/dogma/#{class_name.downcase}.hpp"
    puts "touch dart/lib/src/#{class_name.downcase}.dart"
    puts "touch python/src/dogma/#{class_name.downcase}.py"
    puts "touch python/test/test_#{class_name.downcase}.py"
    puts "touch ruby/lib/dogma/#{class_name.downcase}.rb"
    puts "touch ruby/spec/#{class_name.downcase}_spec.rb"
    puts "touch zig/src/#{class_name.downcase}.zig"
  end
  puts
  puts "echo OK"
end
