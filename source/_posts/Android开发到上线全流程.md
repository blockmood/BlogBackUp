---
title: Android开发到上线全流程
date: 2019-03-24 13:10:24
tags: Android
---


下面教程介绍了如何利用web技术从零开始开发到上线一个Android应用。采用cordova框架初始化项目，放入需要打包的页面，再用Eclipse运行项目，构建打包，就得到一个Android应用。

## 1、安装Eclipse

目前开发环境为mac，所以下面方法以mac为准，window用户暂不支持。

2017最新Eclipse破解版，可以到我个人[网盘下载：](https://pan.baidu.com/s/1c4liJwW) 密码: 2dkx

## 2、配置java环境

由于是java开发，所以得安装java环境
请到[java官网](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载，mac用户下载macOS版本，本地执行javac -version打印版本号，即安装成功。

## 3、安装ADT

默认安装的Eclipse是没有安卓环境的，需要还安装ADT，一个Eclipse工具用于安卓开发的插件。
ADT百度网盘地址：[链接:](https://pan.baidu.com/s/1pM6QJTx) 密码: 6ezy
安装ADT步骤如下：
1、启动Eclipse,然后在菜单栏上选择 Help > Install New Software
2、单击 Add 按钮，在右上角，选择下载好的ADT压缩包
3、下面全部勾选，点击finish
4、重启Eclipse

## 4、安装SDK

下载最新版安卓[SDK工具](https://www.androiddevtools.cn/)，解压放到本地一个目录
然后打开Eclipse -> 偏好设置 -> Android，选择SDK所在路径，点提交，再重启Eclipse
然后再点击Window -> Android SDK Manager 启动SDK工具，下载安装对应SDK包
需要下载的地方，如果网络慢就需要翻墙
/Tools/Android SDK Tool
/Tools/Android SDK Platforms-tools
/Tools/Android SDK Build-tools 25(建议下载API25，25.0.1，25.0.2，25.0.3，cordova7.0只能支持到API25，谨记！)
/Android 7.1.1（API 25）全部下载
/Extras 全部下载

只需要…安装好上面的，即可启动Cordova项目了!

## 5、生成安卓应用

安装node，cordova教程可以看上一篇[IOS开发到上线全流程](https://blockmood.github.io/2019/03/19/ios%E5%BC%80%E5%8F%91%E5%88%B0%E4%B8%8A%E7%BA%BF%E5%85%A8%E9%83%A8%E6%B5%81%E7%A8%8B/)。
进入之前IOS开发一个目录，添加安卓平台代码

```
$cordova platforms add android
```

然后打开Eclipse -> File -> New -> other -> Android（如果没安装ADT是看不到这个选项的） -> Android Project from Existing Code -> 目录选择cordova安卓目录 -> 点击确定打开了cordova项目 -> 电脑连接上安卓手机（手机处于开发者模式）-> 点击菜单run -> run -> 手机上提示要安装应用 -> 安装成功 -> 电脑上生成/platforms/android/bin/MainActivity.apk文件

修改/www/index.html内容，终端执行cordova prepare，再重新打包，会看到手机上的APP内容也跟着变了。

生成的MainActivity.apk文件即为安卓应用，可以用来发布到各大安卓市场，如果要加密，可以使用[爱加密](http://www.ijiami.cn/)

## 6、安装插件

插件安装跟苹果同步，苹果装好了安卓就能同步使用

## 7、发布应用到市场

安卓不像苹果，有很多应用市场，需要一个一个发布，平台列举如下，可根据具体情况推广：

360手机助手：http://dev.360.cn
百度手机助手： http://app.baidu.com
腾讯应用宝：http://open.qq.com
豌豆荚：http://developer.wandoujia.com
小米开放平台：http://dev.xiaomi.com
联想乐商店：http://open.lenovo.com/developer
搜狗手机助手：http://zhushou.sogou.com/open
OPPO应用商店:http://open.oppomobile.com
华为应用市场：http://developer.huawei.com
魅族应用中心：http://developer.meizu.com
三星应用商店：http://seller.samsungapps.com/join/joinNow.as
应用汇：http://dev.appchina.com
机锋市场：http://dev.gfan.com
锤子应用商店：http://dev.smartisan.com






















