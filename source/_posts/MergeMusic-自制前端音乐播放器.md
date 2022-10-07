---
title: MergeMusic-自制前端音乐播放器
date: 2020-04-04 16:55:34
categories: 网
tags:
- 网
- Vue
- html
- javascript
---

> 聚合网易云、QQ音乐、bilibili的单页面音乐播放器。

`20200331`打开半个月之前写的，这都什么玩意儿？六百多行两万字，乱七八糟后背发凉。昨天经`manim`群友提醒发现b站现在是音视频分离的`m4s`格式，而且无需转码，直接改后缀`mp3`即可。只是会限制`referer`，所以必须要过服务器的缓存，这点上算不上真正的“单页面”了。不过还是想要加。

所以现在想了想需要的更改有：
* 统一数据格式，把数据处理尽量移到函数服务上，不在前端做了，否则前端代码一堆特判整不成。
* 增加专辑、歌单、b站收藏夹的支持。
* 把音频可视化删了吧，效果也不大看得出来。
* 增加b站。

需要考虑b站分p应该以什么样的形式呈现，或许应该作为歌单处理，另外b站收藏夹需要通过搜索用户才能得到？

`20200403`emmm一言的网易云api貌似炸了，换一个吧。找到个[好东西](https://api.imjad.cn/)，主域名是之前弄`live2d`的时候见过的那个猫与向日葵的博客，我还特别喜欢它的那个`hexo`主题。欸不对这个也挂掉了，莫不是全部搜索都挂了？又找到[一个](https://www.alapi.cn/wymusic.html)，貌似可用。

`20200404`是一个哀悼的日子。

做完了，用起来还是挺爽的。b站的层级有点多，`music`是分P的，`p`是一个稿件，`fav`是收藏列表，`up`是up主，`user`是用户，和`up`的区别是`user`是用来检索`fav`的。网易云用了两个`api`，b站和QQ音乐都是原生的。QQ和网易托管在阿里云函数计算上，b站的跑在阿里云ECS上，用了个`flask`。

# 函数计算API

## qqsearch/cloudsearch/bilisearch
搜索歌曲。

* keyword
搜索关键字
* type
搜索类型:`music`|`lrc`|`list`。默认`music`。
* limit
每次搜索条数，默认`20`。
* offset
搜索偏移，默认`0`。

返回格式
```json
[{
    "type": "格式",
	"mid": "音乐ID，有前缀C/Q/B",
	"name": "音乐名称",
	"album": {
		"name": "专辑名称"
	},
	"artist": [{
		"name": "艺术家名字"
    }]
}]
```
对于`type`,可能的取值有: `music` | `list` | `p`。

## qqmusic/cloudmusic/blimusic
获取歌曲详细信息。

* type
类型:`music` | `list` | `p` | `fav`。
* mid

返回格式
对于`music`
```json
{
    "src": "音乐直链",
    "lyrics": "",
    "img": "图片链接"
}
```

对于`list`
返回一个音乐列表，格式同搜索。