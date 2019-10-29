require 'active_support/core_ext/string'

desc "Begin a new post in _posts"
task :post, :filename do |t, args|
  args.with_defaults(:filename => 'new-post')
  file_name = args.filename.gsub(/[ _]/, '-')
  open("_posts/#{Time.now.strftime('%Y-%m-%d')}-#{file_name}.md", 'w') do |post|
    post.puts "---"
    post.puts "title: \"#{args.filename.gsub(/[-_]/, ' ').titlecase}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %T %Z')}"
    post.puts "---"
  end
end

desc "Start a new draft in _drafts"
task :draft, :filename do |t, args|
  args.with_defaults(:filename => 'new-post')
  file_name = args.filename.gsub(/[ _]/, '-')
  open("_drafts/#{file_name}.md", 'w') do |post|
    post.puts "---"
    post.puts "title: #{args.filename}"
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %T %Z')}"
    post.puts "---"
  end
end
