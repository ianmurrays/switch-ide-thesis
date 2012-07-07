guard :shell, :all_on_start => true do
  watch %r{(.+)/(.+).md} do |m|
    `pandoc #{m[1]}/#{m[2]}.md -o #{m[1]}/#{m[2]}.pdf`
    "Parsed #{m[1]}/#{m[2]}.md"
  end
end
