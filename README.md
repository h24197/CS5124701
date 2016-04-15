# 巨量資料與分析 HW1
作業一：利用Twitter或Github的爬蟲，將爬下來的資料以下來格式儲存(擇一):  
  - JSON
  - CSV

> 在此使用上課所教的Logstash的方式去抓資料 
> 原先預設為資安相關keyword, 但資料量太少  
> keyword以資料量多的為主 => "a"  (/data)  
> 留下爬下來的JSON所有欄位  
> 下列以原先資安相關keywords做介紹
> 檔案過大,故以每100M為單位壓縮成data.zip

### Data Format
JSON file:  
```sh
    {
        "favorited": false, 
        "in_reply_to_user_id": null, 
        "contributors": null, 
        "truncated": false, 
        "text": "#cybersecurity When it comes to security standards, one size doesn't fit all #databreach\nhttps://t.co/kbIvWOtMuk via @computerworld", 
        "@timestamp": "2016-04-14T19:27:12.000Z", 
        "is_quote_status": false, 
        "in_reply_to_status_id": null, 
        "user": {
            "follow_request_sent": null, 
            "profile_use_background_image": true, 
            "time_zone": "London", 
            "id": 57601610, 
            "description": "Specialist recruitment services for Legal In House Counsel Worldwide, Data Scientists and difficult hires.", 
            "verified": false, 
            "profile_image_url_https": "https://pbs.twimg.com/profile_images/378800000642278754/68cebc79eeb522a31229e4292a587e48_normal.jpeg", 
            "profile_sidebar_fill_color": "000000", 
            "is_translator": false, 
            "geo_enabled": false, 
            "profile_text_color": "000000", 
            "followers_count": 180, 
            "protected": false, 
            "id_str": "57601610", 
            "default_profile_image": false, 
            "listed_count": 648, 
            "utc_offset": 3600, 
            "statuses_count": 11780, 
            "profile_background_color": "DBE9ED", 
            "friends_count": 67, 
            "profile_link_color": "CC3366", 
            "profile_image_url": "http://pbs.twimg.com/profile_images/378800000642278754/68cebc79eeb522a31229e4292a587e48_normal.jpeg", 
            "notifications": null, 
            "profile_background_image_url_https": "https://pbs.twimg.com/profile_background_images/667043743225237504/527yYZQr.jpg", 
            "profile_background_image_url": "http://pbs.twimg.com/profile_background_images/667043743225237504/527yYZQr.jpg", 
            "screen_name": "CERGraham", 
            "lang": "en", 
            "profile_background_tile": false, 
            "favourites_count": 335, 
            "name": "CERGlobal", 
            "url": "http://www.cerglobalrecruitment.co.uk", 
            "created_at": "Fri Jul 17 09:40:22 +0000 2009", 
            "contributors_enabled": false, 
            "location": "London", 
            "profile_sidebar_border_color": "000000", 
            "default_profile": false, 
            "following": null
        }, 
        "timestamp_ms": "1460662032376", 
        "geo": null, 
        "id": 720694915500519424, 
        "possibly_sensitive": false, 
        "source": "<a href=\"http://mobile.twitter.com\" rel=\"nofollow\">Mobile Web</a>", 
        "lang": "en", 
        "entities": {
            "user_mentions": [
                {
                    "indices": [
                        117, 
                        131
                    ], 
                    "screen_name": "Computerworld", 
                    "id": 14539324, 
                    "name": "Computerworld", 
                    "id_str": "14539324"
                }
            ], 
            "symbols": [], 
            "hashtags": [
                {
                    "indices": [
                        0, 
                        14
                    ], 
                    "text": "cybersecurity"
                }, 
                {
                    "indices": [
                        77, 
                        88
                    ], 
                    "text": "databreach"
                }
            ], 
            "urls": [
                {
                    "url": "https://t.co/kbIvWOtMuk", 
                    "indices": [
                        89, 
                        112
                    ], 
                    "expanded_url": "http://www.computerworld.com/article/3054556/security/when-it-comes-to-security-standards-one-size-doesnt-fit-all.html", 
                    "display_url": "computerworld.com/article/305455\u2026"
                }
            ]
        }, 
        "created_at": "Thu Apr 14 19:27:12 +0000 2016", 
        "retweeted": false, 
        "coordinates": null, 
        "in_reply_to_user_id_str": null, 
        "filter_level": "low", 
        "in_reply_to_status_id_str": null, 
        "in_reply_to_screen_name": null, 
        "favorite_count": 0, 
        "place": null, 
        "retweet_count": 0, 
        "@version": "1", 
        "id_str": "720694915500519424"
    }

```

### Data Source

Use Logstash to be a crawler:

* API - Instead of using Twitter API, I use logstash.
* Topic - About Information Security
* Keywords - cybersecurity, malware, infosec, kaspersky, trendmicro, symantec


#### Logstash

Type these commands:

```sh
$ cd logstash-2.3.1
```

```sh
$ ./logstash -f twitter.config
```

##### twitter.config
Using Logstash needs to configure it. So, this is my configuration file:

* Input field - my twitter key & keywords
* Output field - Elasticsearch & export a JSON file 
```sh
 input{
     twitter{
         consumer_key => "..."
         consumer_secret => "..."
         oauth_token => "..."
         oauth_token_secret => "..."
         keywords => ["some about information securyty"]
         full_tweet => true
     }
 }
 output{
     elastcsearch{
         index => "twitter1"
     }
     file{
         codec => "json"
         path => ["path/filename.json"]
     }
 }
```

```sh
$ ./logstash -f twitter.config
```

