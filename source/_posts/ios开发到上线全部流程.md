---
title: ios开发到上线流程
date: 2019-03-19 12:12:24
tags: ios
---


下面教程介绍了如何利用web技术从零开始开发到上线一个IOS应用。采用cordova框架初始化项目，放入需要打包的页面，再用XCode运行项目，构建打包，就得到一个IOS应用。

## 1、安装xcode

开发IOS前提是要有一台mac电脑，然后在app store下载安装最新版Xcode

![xcode](/img/xcode.png)

## 2、安装node

进入node官网下载安装，建议安装最新稳定版
终端运行结果如下，证明安装成功

```
$node -v
v8.1.2

```

## 3、安装cordova

在终端运行，全局安装cordova，保证兼容性问题，最好安装7.0版本

```
$sudo npm install -g cordova@7.0
$cordova -v
7.0.0
```


## 新建一个cordova项目
进入一个目录，运行

```
$cordova create hello com.example.hello HelloWorld
$cordova platforms add ios
```

将需要打包的html代码放置到 www下，或者可以将项目用svn导入，然后执行下面命令可以将代码自动复制到 /platforms/ios/www中（为了配合安卓项目一块使用）

```
$cordova prepare
```

## 5、离线打包IOS应用

使用xcode打开项目/platforms/ios/xx.xcodeproj，连接苹果手机（也可以打开模拟器）,右上角device选择当前手机或者ipad，点开general，填写signing，到[苹果官网](http://developer.apple.com/)注册一个苹果开发者账号，添加到signing中，点击右上角运行按钮，或者快捷键cmd + R，即可在手机上看到打包好的app，已经成功了一大半了。
修改/platforms/ios/www/index.html内容，再打包试下

## 6、页面上下滑动出现黑边bug

打开当前根目录下config.xml，在最下面添加下面代码即可解决页面滑动时，页面上下会脱离APP视图。
然后再执行$cordova prepare命令，使配置生效（相当于覆盖ios目录下面的config.xml文件）

```
<preference name="WebViewBounce" value="false" />
<preference name="DisallowOverscroll" value="true" />
```

## 7、IOS状态栏往下顶20像素

把文件MainViewController.m中的方法viewWillAppear改成下面这样，如果要往上顶20像素，可以把20改为-20

```
 - (void)viewWillAppear:(BOOL)animated
{
// View defaults to full size.  If you want to customize the view's size, or its subviews (e.g. webView),
// you can do so here.
//Lower screen 20px on ios 7
if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 7) {
    CGRect viewBounds = [self.webView bounds];
    viewBounds.origin.y = 20;
    viewBounds.size.height = viewBounds.size.height - 20;
    self.webView.frame = viewBounds;
}
[super viewWillAppear:animated];
}
```

## 8、启动界面延迟加载

执行命令，添加插件cordova plugin add cordova-plugin-splashscreen，然后在/platforms/ios/app名字/config.xml中，添加

```
<preference name="AutoHideSplashScreen" value="true" />
<preference name="SplashScreenDelay" value="1000" />
```


## 9、app中嵌入外部链接

如果要在app中嵌入外链显示，需要在/platforms/ios/app名字/config.xml中，添加

```
<allow-navigation href="*"/>
<allow-intent href="*"/>
```

## 10、启动图标路劲

系统所在图片地址/platforms/ios/xxx/Images.xcassets/，AppIcon.appiconset里面放着所有不同大小启动图标图片，LaunchImage.launchimage里面放着为所有启动图片，最好全部替换

## 11、根据需要安装插件

根据具体需求安装对应插件，比如QQ，微信，微博登录分享，调用支付宝支付，调用系统自带相机，消息
推送，获取设备信息等，下面介绍几个常用的插件使用办法

极光推送

```
cordova plugin add jpush-phonegap-plugin --variable APP_KEY=your_jpush_appkey
```

插件配置：
1、进入[官网](https://www.jiguang.cn/)，注册账号密码，申请一个app，并且通过企业认证
2、极光推送中需要安装证书，参考
http://www.jianshu.com/p/6f8233ece1b2
3、启动极光推送，项目js中添加以下代码，具体设置[文档地址](https://github.com/jpush/jpush-phonegap-plugin/blob/master/doc/iOS_API.md)

```
window.plugins.jPushPlugin.startJPushSDK()  js启动极光推送服务
window.plugins.jPushPlugin.getRegistrationID(function(data) {   获取用户Registration ID
  console.log("JPushPlugin:registrationID is " + data)
})
```

QQ分享，微信分享，微博分享

进入[cordova](https://cordova.apache.org/plugins/)官网，找到对应插件查看文档安装（cordova-plugin-wechat cordova-plugin-qqsdk cordova-plugin-weibosdk jpush-phonegap-plugin参考api安装）

常用插件安装，可以参考常用[插件列表](https://www.cnblogs.com/huazai/p/5439640.html)


## 12、本地热更新调试

让app直接运行外部服务器下的网址，方便调试。
直接在/platforms/ios/app名字/config.xml中，修改content配置信息，比如我们的web应用运行在本机3000端口上，地址可以改为本机ip加端口号方式，并且手机跟电脑连在同一wifi下

```
<content src="http://172.16.20.58:3000/" />
```

## 13、打包上线

1、填好相关信息


![ios1](/img/ios1.png)


2、注册苹果证书，信息太长，参考博文

[参考链接](https://www.jianshu.com/p/01224fc523d4)


3、往苹果商店打包：product -> archive -> upload to app store -> 上传成功 -> 进入苹果官网填好相关基本信息，介绍图片等，再点击发布app，等待审核结果，一般首次3~7天，再次审核2天左右。

注意：如果官网找不到上传的APP，请检查证书是否有问题，或者app信息是否填写有误，或者App不合规范，一般会给你发邮件告知具体问题。









