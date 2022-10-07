---
title: 小苏上网——live2d网页博客看板娘
date: 2020-01-08 19:34:11
categories: 网
tags:
- 网
- live2d
---

“看板娘”。

一直想让她上网...想了很久了，模型还没做好的时候就想着了...但是苦于轮子只有`SDK2.1`的，而小苏是`SDK3.3`的，所以一直都没有成功。本来应该可以通过`live2d 3`软件转码，但一直都无法试用会员。

终于，我重装完系统，立即下载`live2d`，一通乱搞，科学上网什么的，总之会员也试用了，错位什么的也修复了，`SDK2.1`的模型和展示文件都没问题了。这时候没有找到好的轮子，就又暂时搁置了。

其间看过一个博客叫[猫与向日葵](https://imjad.cn/)，Ta的博客也是`hexo`搭的，真的感觉超棒，就像是奶油一样的感觉。点击特效像是散开的糖果，像是奶油上缤纷的糖针啊……（虽然那个模板叫做活字印刷w）

这两天youyou迷上了`steam`，买了个`live2d ex`，可以把`live2d`模型放到桌面上。我把小苏模型给他了，总有种不祥的感觉...

# Web版

去`github`一搜`live2d`，就出来一堆。找到了[这个](https://github.com/galnetwen/Live2D)。

然后就试着把之前做好的`SDK2.1`的展示文件（主要包括`model.json`、`model.moc`和贴图，主要引用`model.json`，其中定义了其他文件的路径。如果有动作文件什么的路径也包含在这里面）搞进去。然后...奇迹般地就好了喔。

有呼吸、一言、眼神跟随，不错哒。

很大的精力都投入到了调整大小上面。这个脚本是在`canvas`上面绘制，调整起来有点诡异...另外分辨率感觉比较低，后面`hexo`的轮子里面用了超采样什么的，在`Web`版里面至少我是没有找到接口，就只能这样子了。

另外把贴图的地址改成了阿里云的`OSS`，倒是直接在`model.json`里面更改就奇迹般地可以了。

![网页小苏](https://s2.ax1x.com/2020/01/08/lR9OTx.jpg)

# 博客版

如果一切安好，那么你现在应该可以在右下角看到小苏！

使用了星星最多（`2.6K`）的[hexo-helper-live2d](https://github.com/EYHN/hexo-helper-live2d)。这个东西貌似也是套了好多好多层轮子233。

到底编辑哪里的配置，博客还是主题，模型放哪里，给我弄了好半天。最后的结论：
* 编辑博客的`_config.yml`文件
* 模型文件夹放在`根目录/live2d_models/`下
* 模型的`json`文件名必须为`文件夹名.model.json`
* 在`_config.yml`文件中更改的模型名称要和文件夹名一致
* 每次重新开`hexo s`才有效果

更多的配置项在[这里](https://l2dwidget.js.org/docs/class/src/index.js~L2Dwidget.html)

最后的配置：
```yaml
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: su
  display:
    position: right
    width: 150
    height: 300
    vOffset: -55
    hOffset: -30
  mobile:
    show: true
  react:
    opacity: 0.8
  dialog:
    enable: false
    hitokoto: true
```