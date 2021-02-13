# techacademynagazine
require "twitter"
require "active_support/time"

client = Twitter::REST::Client.new do |config|
  # pitで管理するとよいですね
  config.consumer_key = "zWcXdvc68hOEKw90yufcB60BI"
  config.consumer_secret = "5vpW21oQ1Ym4220Fvtg2qOqIDy7cU3wWmh9qT1CiNSf9PDxQYG"
  config.access_token = "1330816117053353984-MJmBH9pduqLeQE5kfPFyfH7lq12LVS"
  config.access_token_secret = "JYFKBLWxTAhvypX7TwJVBLeeaV9xK1108a6MLPLaTYwtx"
end

count = 0
total_followers_count = 0
max_id = nil
query = "きたけー" # キーワード

begin
  client.search(query, lang: :ja, locale: :ja, max_id: max_id).each do |tweet|
    next if tweet.created_at >= 1.day.since.beginning_of_day
    if tweet.created_at < Time.now.beginning_of_day
      max_id = nil
      break
    end
    count += 1
    total_followers_count += tweet.user.followers_count
    max_id = tweet.id
  end
end while max_id != nil

puts "ツイート数 #{count}"
puts "フォロワー数のトータル #{total_followers_count}"
puts "平均フォロワー数 #{total_followers_count / count.to_f}"
