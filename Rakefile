require 'active_support'

desc "Begin a new post in _posts"
task :post, :filename do |t, args|
  args.with_defaults(:filename => 'new-post')
  #system "touch #{source}/_posts/#{Time.now.strftime('%Y-%m-%d_%H-%M')}-#{args.filename}.markdown"
  open("_posts/#{Time.now.strftime('%Y-%m-%d_%H-%M')}-#{args.filename.gsub(/[ _]/, '-')}.markdown", 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{args.filename.gsub(/[-_]/, ' ').titlecase}\""
    post.puts "---"
  end
end