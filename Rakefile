# This is free and unencumbered software released into the public domain.

require 'pathname'
require 'rake'
require 'yaml'

VERSION = File.read('VERSION').chomp

LANGUAGES = {
  :c        => %w(C .c),
  :cpp      => %w(C++ .cpp),
  :dart     => %w(Dart .dart),
  :protobuf => %w(Protobuf .pb),
  :python   => %w(Python .py),
  :ruby     => %w(Ruby .rb),
  :zig      => %w(Zig .zig),
}.freeze

GITHUB_ORG_URL = "https://github.com/dogmatists"

README_H2 = %w(Prerequisites Installation Examples Reference) + ['See Also']

def classes(&block)
  classes = YAML.load(File.read('etc/classes.yaml'))
  classes.each do |class_name, class_info|
    class_name = class_name.to_sym
    class_info = class_info.transform_keys(&:to_sym)
    block.call(class_name, class_info)
  end
end

task default: %w(stub)

task :langs do
  LANGUAGES.each do |k, vs|
    puts [k, vs].flatten.join("\t")
  end
end

task :stub do
  puts "#!/bin/bash"
  classes do |class_name, class_info|
    puts
    puts "# #{class_name}"
    puts "touch c/dogma/#{class_name.downcase}.h"
    puts "touch cpp/dogma/#{class_name.downcase}.hpp"
    puts "touch dart/lib/src/#{class_name.downcase}.dart"
    puts "touch dart/test/#{class_name.downcase}_test.dart"
    puts "touch protobuf/src/#{class_name.downcase}.proto"
    puts "touch python/src/dogma/#{class_name.downcase}.py"
    puts "touch python/test/test_#{class_name.downcase}.py"
    puts "touch ruby/lib/dogma/#{class_name.downcase}.rb"
    puts "touch ruby/spec/#{class_name.downcase}_spec.rb"
    puts "touch zig/src/#{class_name.downcase}.zig"
  end
  puts
  puts "echo OK"
end

namespace :check do
  task :readmes do
    LANGUAGES.each do |lang_id, _|
      Readme.load(lang_id) do |readme|
        readme.check_badges!
        readme.check_h2_sections!
        readme.check_see_also!
      end
    end
  end
end

class Readme < Struct.new(:lang_id, :path, :text)
  def self.load(lang_id, &block)
    path = Pathname("#{lang_id}/README.md")
    abort "#{path}: missing file" unless path.exist?
    block.call(self.new(lang_id, path, path.read))
  end

  def check_badges!
    # TODO
  end

  def check_h2_sections!
    readme_h2 = self.text.scan(/^## (.*)/).map { |matches| matches.first }
    warn "#{self.path}: TOC discrepancy (<h2> sections)" if readme_h2 != README_H2
  end

  def check_see_also!
    _, footer_actual = self.text.split("\n## See Also\n\n")
    titles, links = [], []
    LANGUAGES.each do |lang_id, (lang_title, lang_ext)|
      titles << "[#{lang_title}][]" unless lang_id == self.lang_id
      links  << "[#{lang_title}]:".ljust(12) + "#{GITHUB_ORG_URL}/dogma#{lang_ext}"
    end
    last_title = titles.pop
    footer_expected = "Dogma for " + titles.join(", ") + ", and "
    footer_expected << last_title + ".\n\n" + links.join("\n") + "\n"
    warn "#{self.path}: footer discrepancy (See Also section)" if footer_actual != footer_expected
  end
end
