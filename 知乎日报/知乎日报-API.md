# 知乎日报-API


* [http://news-at.zhihu.com/api/3/news/latest](http://news-at.zhihu.com/api/3/news/latest)

```
{
"date":"20140621",
"stories":[{
"title":"眼花缭乱的时装周上，眼睛不能只盯着模特",
"share_url":"http://daily.zhihu.com/story/3980042",
"ga_prefix":"062120",
"images":["http://p3.zhimg.com/24/7b/247bcebba402d03c48a036f97f65c7de.jpg"],
"type":0,
"id":3980042},, ... ], "top_stories":[{
"title":"为什么今天很多人照相喜欢摆 V 手势？",
"image":"http://p2.zhimg.com/55/c3/55c3296b5fb9bcc68c938242e4da8994.jpg",
"share_url":"http://daily.zhihu.com/story/3983436",
"ga_prefix":"062118",
"type":0,
"id":3983436},, ... ]
}

date : 日期
stories : 当日新闻
title : 新闻标题
image : 图像地址
share_ur: 供在线查看内容与分享至 SNS 用的 URL
ga_prefix : 供 Google Analytics 使用
内容获取
```

* [http://news.at.zhihu.com/api/3/news/before/20140621](http://news.at.zhihu.com/api/3/news/before/20140621)

```
{ 
  date: "20140620", 
  stories: [ 
  { 
    title: "深夜食堂 · 两小无猜", 
    share_url: "http://daily.zhihu.com/story/3985504", 
    ga_prefix: "062022", 
    images: [ "http://p3.zhimg.com/61/87/61879fc578db3f8f61d18c55d3dc8984.jpg" ], 
    type: 0, 
    id: 3985504 
  }, ... ]
}
```

* [http://news-at.zhihu.com/api/3/news/3983436](http://news-at.zhihu.com/api/3/news/3983436)

```
{
    "body": ...,
    "image_source": "SIGitas MATulis / CC BY-SA",
    "title": "为什么今天很多人照相喜欢摆 V 手势？",
    "image": "http://p2.zhimg.com/55/c3/55c3296b5fb9bcc68c938242e4da8994.jpg",
    "share_url": "http://daily.zhihu.com/story/3983436",
    "js": 
    [
      "http://news-at.zhihu.com/js/story.js?v=97942"
    ],
    "ga_prefix": "062118",
    "type": ​0,
    "id": ​3983436,
    "css": 
    [
        "http://news-at.zhihu.com/css/news_qa.auto.css?v=77778"
    ]
}
```

* [http://news-at.zhihu.com/api/3/news/hot](http://news-at.zhihu.com/api/3/news/hot)

```
{
  "recent": [
  {
      "news_id": ​8095363,
      "url": "http://news-at.zhihu.com/api/2/news/8095363",
      "thumbnail": "http://pic1.zhimg.com/fee362f30fe8740e5b6cf7acb9b1d520.jpg",
      "title": "公务员体检为什么要检查肛门？"
  }, ...]
}
```

