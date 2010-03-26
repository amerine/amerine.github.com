require 'active_support'

desc "Begin a new post in _posts"
task :post, :filename do |t, args|
  args.with_defaults(:filename => 'new-post')
  open("_posts/#{Time.now.strftime('%Y-%m-%d_%H-%M')}-#{args.filename.gsub(/[ _]/, '-')}.markdown", 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{args.filename.gsub(/[-_]/, ' ').titlecase}\""
    post.puts "---"
  end
end

desc "Start a new draft in _drafts"
task :draft, :filename do |t, args|
  args.with_defaults(:filename => 'new-post')
  open("_drafts/#{args.filename.gsub(/[ _]/, '-')}.markdown", 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{args.filename.gsub(/[-_]/, ' ').titlecase}\""
    post.puts "---"
  end
end