#
# Adapted from https://github.com/progit/progit/blob/master/latex/makepdf
#
require "rubygems"
require "bundler/setup"
require "guard"
require "guard/shell"
require "terminal-notifier-guard"

def replace(string, &block)
  string.instance_eval do
    alias :s :gsub!
    instance_eval(&block)
  end
  string
end

def pre_pandoc(string)
  replace(string) do
    # s %r{\#\#\#\# (.*?)}, '\subsubsection \1'
    # s %r{\#\#\# (.*?)}, '\subsection \1'
    # s %r{\#\# (.*?)}, '\section \1'
    # s %r{\# (.*?)}, '\chapter \1'
  end
end

def typeset(output = "switch-ide-thesis.tex")
  $root = File.expand_path(File.dirname(__FILE__))

  # Join all the markdown files first
  markdown = Dir["#$root/*.md"].sort.map do |file|
    File.read(file)
  end.join("\n\n\\clearpage\n\\newpage\n\n") # Join each file with a "newpage".

  # Run pandoc on the final markdown
  arguments = ["pandoc", 
               "--smart", 
               "--bibliography=#{$root}/references.bib", 
               "--csl=#{$root}/ieee-w-url.csl", "--number-sections",
               "--template=#{$root}/template.tex",
               "--toc",
               "-o #{output}"].join(" ")

  print "Typesetting... "
  IO.popen(arguments, 'w+') do |pipe|
    pipe.write(pre_pandoc(markdown))
  end
  puts "done."
end

guard :shell, :all_on_start => false do
  watch(%r{.+\.md}) { typeset }
  watch("template.tex") { typeset }
  watch("references.bib") { typeset }
end
