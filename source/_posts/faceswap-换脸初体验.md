---
title: faceswap 换脸初体验
date: 2020-01-05 22:22:10
mathjax: true
categories: 技
tags:
- 技
- github
- faceswap
- 深度学习
---

冷雨夜，与某龙闲聊中得知，便来一试。

所谓深度学习神马的，实质上就是利用显卡善于大量重复运算的特性，不断试错，最终一步步训练得到更优秀的模型。

真棒！`￥250`捡垃圾捡来的`750 Ti`终于不用吃灰了！

# 安装

先找到`github`上的[这个项目](https://github.com/deepfakes/faceswap)。

开始阅读[安装文档](https://github.com/deepfakes/faceswap/blob/master/INSTALL.md)。

其中提到要求`GPU`而不是`CPU`。`GPU`数小时，`CPU`可能要以周为单位。查了下，我捡来的`750 Ti`应该还是支持的。

自动安装程序用不了（因为所下载的资源应该是墙外的，下载不下来）。

手动安装了`Anaconda`和`Git`。然后根据文档走就行了。注意操作要在`Anaconda`环境下的终端进行。

运行`python setup.py`，没有选择`Docker`。就开始自动安装各种包了。

（那些包一个几百MB太可怕了啊啊啊）

`python faceswap.py gui`就可以了。

![界面截图](https://s2.ax1x.com/2020/01/05/lBXODs.png)

至此，安装就算是完成咯。

# 初步使用

开始阅读[使用文档](https://github.com/deepfakes/faceswap/blob/master/USAGE.md)。


> So here's our plan. We want to create a reality where Donald Trump lost the presidency to Nic Cage; we have his inauguration video; let's replace Trump with Cage.

???Smg

## Extract 提取训练数据

程序还得从`github`下载模型，下不下来还得科学上网...

这一步是将视频切割成图片，然后将图片中的人脸提取出来，方便之后的训练。

第一次用命令行运行，想摸一摸显卡的温度，然后一摸，，，就报错了。emmm虽然不能说一定有必然的联系，但大概就是原因。

另外注意配置输出的分辨率，默认`256*256`太大了。

## Train 训练模型

报错啦，原因是GPU内存不够...（我的`750 Ti`只有`2G`显存太渣了...）

```
01/05/2020 16:54:19 CRITICAL Error caught! Exiting...
01/05/2020 16:54:19 ERROR    Caught exception in thread: '_training_0'
01/05/2020 16:54:19 ERROR    You do not have enough GPU memory available to train the selected model at the selected settings. You can try a number of things:
01/05/2020 16:54:19 ERROR    1) Close any other application that is using your GPU (web browsers are particularly bad for this).
01/05/2020 16:54:19 ERROR    2) Lower the batchsize (the amount of images fed into the model each iteration).
01/05/2020 16:54:19 ERROR    3) Try 'Memory Saving Gradients' and/or 'Optimizer Savings' and/or 'Ping Pong Training'.
01/05/2020 16:54:19 ERROR    4) Use a more lightweight model, or select the model's 'LowMem' option (in config) if it has one.
```

无论如何都不行...太糟糕了。

试着改成了`64*64`然后再来运行`Lightweight`模型，理论上是最节能的方法了，然而这时候就会莫名退出，没有报错提示，只有一个错误是`return 3221225620`。google都不起作用。`issue`里有人提交这个问题但是并没有回答。

最后一通乱搞，开了所有模型的`LowMem`选项，勾选了那三个内存节省方案，以这样的设置跑起来了。

![Training](https://s2.ax1x.com/2020/01/05/lrSa5R.png)

大概每个小时可以跑`5000`次迭代。

![约五千次](https://s2.ax1x.com/2020/01/06/lsRxWn.png)

![约十万次](https://s2.ax1x.com/2020/01/06/lsRzzq.png)

效果也还不错了，只是因为原图数量和质量都不够高，训练时间也没有特别长，所以有点糊，吻合也不够好。但效果已经出来了。

# Convert 导出

直接选择输入文件，模型目录就行。

注意输入文件需要经过`Extract`，确定一个脸部的路径文件。
