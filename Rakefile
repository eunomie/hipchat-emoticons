require "open-uri"
require "pp"
require "rubygems"
require "json"

BOOKMARKLET = 'prompt("Copy this:", JSON.stringify( { token: app.token_info.token, group_id: util.jid.group_id(app.current_user_jid), user_id: util.jid.user_id(app.current_user_jid) } ))'

desc "Updates emoticons.json."
task :default do

  `echo '#{BOOKMARKLET}' | pbcopy`
  puts "Copied bookmarklet to clipboard."

  `open -g https://hipchat.com/chat`
  puts "Opened chat in browser."
  puts " * Log in if necessary."
  puts " * Run the bookmarklet, copy its output."
  print " * Paste bookmarklet output: "
  raw_json = gets()

  options = JSON.parse(raw_json)
  base_url = "https://www.hipchat.com/api/get_emoticons?group_id=%s&user_id=%s&token=%s&format=json"
  url = base_url % options.values_at("group_id", "user_id", "token")

  data = open(url).read
  json = JSON.parse(data)
  pretty = JSON.pretty_generate(json)

  file = "emoticons.json"
  puts pretty
  File.open(file, "w") { |f| f.puts pretty }
  puts "Wrote to #{file}."
end
