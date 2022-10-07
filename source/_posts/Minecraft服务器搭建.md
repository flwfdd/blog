---
title: Minecraft服务器搭建
date: 2019-12-10 01:08:46
img: https://pic2.superbed.cn/item/5dee812c1f8f59f4d6b2e4ec.gif
categories: 网
tags:
- 网
- 服务器
- 游戏
---

在很久很久以前，当我还在上小学的时候，不知何时认识到了`Minecraft`，刚开始大概玩的还是手机版，版本号貌似是8，总之那时候物品都还没有分类目，创造模式打开全部物品都同时展示。再后来，小区的的小伙伴们就会在晚上聚到一起联机玩，那时候用的还是葫芦侠、多玩等等，还有一些什么两个物品就可以无限刷的bug。

事实上，我们玩的并不好，没有玩建筑，没有玩红石。但是记忆中铭刻下的是每个夜晚的欢声笑语。

小学一个寒假吧，我回老家，看见一个姐姐在玩电脑的`Minecraft`，我就把她当时玩的拷回了家。那个文件夹名叫`1.64简单整合`。是的，这次开服我用的依然是这一个版本，五六年来，一直是它，我的每台电脑上都有它的副本。我也对`Minecraft`本身产生了很大的兴趣，还看了些`MOJANG`的纪录片。

小学？初中？反正当我了解到开服这件事情之后，曾经几度尝试，但是无一成功，最成功的一次就是在`WiFi`下可用了，但是...我为什么不用局域网共享呢？那时候对于服务器、公网IP什么的压根没有什么了解，所以也当然没发成功。

初一初二的时候，我和另外两个小伙伴每个星期六会骑车到郊区的水库玩，每次去之前我们都在我家联机玩一会电脑版的`Minecraft`，那时候玩的就是`IC2`，这次这个服务器配置基本就是复制了当时组的这一个整合。

# 基本配置
使用的是腾讯云学生机，`1核 2G 1M`，系统`Ubuntu 18.04 LTS`，本来跑的是给下一届搭的OJ，现在也没人用了，关了就也没什么。服务器闲置没事做，便想着捡起一个远古的梦想：MC开服。

`Minecraft : 1.6.4`
Mod列表：
* 工业2
* 血量显示
* 耐久度显示
* G键合成
* 更多背包
* 小地图

## 安装JAVA
`sudo apt install default-jdk`
开始就是这么装的，后来炸了。因为只支持到`JAVA8`
所以先卸载，然后又安装`sudo apt install openjdk-8-jdk`

## 配置纯净版
```bash
mkdir minecraft
cd minecraft
sudo wget https://s3.amazonaws.com/Minecraft.Download/versions/1.6.4/minecraft_server.1.6.4.jar
sudo java -Xms512m -Xmx1024m -jar minecraft_server.1.6.4.jar nogui
```
还要去改一下配置文件`server.properties`，更改配置`online-mode=false`，不使用正版服务器了。
然后再运行几次`sudo java -Xms512m -Xmx1024m -jar minecraft_server.1.6.4.jar nogui`
显示`2019-12-07 15:21:09 [INFO] Done (15.580s)! For help, type "help" or "?"`就成功了。

因为我的版本启动器是`忘却的旋律`，只能使用`forge`启动，所以这时候下载一个纯净版的`1.6.4`，就可以使用`服务器IP地址:25565`成功登陆了。

## 配置forge
```bash
sudo chmod 777 -R ./
sudo wget https://files.minecraftforge.net/maven/net/minecraftforge/forge/1.6.4-9.11.1.916/forge-1.6.4-9.11.1.916-universal.jar
java -Xms512m -Xmx1024m -jar forge-1.6.4-9.11.1.916-universal.jar nogui
```
下载的`forge`版本要和客户端的一样。下载链接从`https://files.minecraftforge.net/`获取。

要使用`ftp`把客户端的`libraries`、`mods`、`config`文件夹全部复制到服务器文件夹下。这一步弄了好久，只要有一点不同步就可能产生错误。

添加了新Mod以后保险起见我把`world`文件夹（世界存档）删除了重新运行。

还是有些问题，`Minimap`和`VoxelMap`都会报错，暂时没有找到解决办法，就找了一个极其鸡肋的小地图Mod`mapwriter`。
