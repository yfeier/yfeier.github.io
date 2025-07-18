# 利用Flutter做成应用11：应用市场发布


### 将应用发布到Google应用市场

进入[Google Play控制台](https://play.google.com/console/developers)，创建一个应用。




### 在网站填写相关内容

***政策->应用内容***
* 隐私权政策  
  添加隐私权政策网站。
  可通过下面的网站制作隐私文件。  
  https://app-privacy-policy-generator.firebaseapp.com/
  
* 广告  
  确认应用是否包含了广告。在 Google Play 中，包含广告的应用旁边会显示“包含广告”标签。
* 应用访问权限  
  判断某些部分需要用户满足特定条件（例如，提供登录凭据、取得会员资格、在特定位置或进行其他形式的身份验证）才能访问。
* 内容分级  
  从官方分级机构获得内容分级。您的内容分级会显示在 Google Play 中。
* 目标受众群体和内容  
  声明应用的目标年龄段以及与应用内容相关的其他信息。
* 新闻应用  
  声明您的应用是否为新闻应用。
* 新冠肺炎 (COVID-19) 接触者追踪应用和感染状况应用  
  声明您的应用是否为新冠肺炎接触者追踪应用或新冠肺炎健康状态应用。
* 数据安全  
  声明您的应用在隐私权和安全性方面的做法。
* 广告 ID  
  是否使用了广告 ID。您必须先完成此部分，然后才能提交以 Android 13 为目标平台的版本。
  针对Android 13或更高版本并使用广告ID的应用程序必须在清单中包含com.google.android.gms.permission.AD_ID权限。
  
***商店发布->商店设置***
* 应用类别  
  选择与应用的内容或主要功能最相符的应用类型、类别和标签。
* 商品详情中的详细联系信息  
  设置邮件网站等，这些信息会在 Google Play 上向用户显示。

***商店发布->主要商品详情***
* 应用详情  
  * 应用名称  
  * 简短说明
  * 完整说明
* 图片
  * 应用图标  
  应用图标必须是透明 PNG 或 JPEG 格式的文件，大小不得超过 1 MB，尺寸为 512 x 512 像素。
  * 置顶大图  
  置顶大图必须是 PNG 或 JPEG 格式，大小不得超过 15 MB，并且尺寸为 1,024 x 500 像素。
  * 手机屏幕截图  
  上传 2 到 8 张手机屏幕截图。相应屏幕截图必须是 PNG 或 JPEG 格式的文件，每张屏幕截图的大小不得超过 8 MB，宽高比为 16:9 或 9:16，各条边的尺寸介于 320 像素和 3840 像素之间。
  * 7 英寸平板电脑屏幕截图  
  可省略
  * 10英寸平板电脑屏幕截图  
  可省略
  
  为了获取屏幕截图，可使用截屏工具[snapmod](https://play.google.com/store/apps/details?id=cn.gavinliu.snapmod)方便的制作屏幕截图。

### 在网站上发布应用
***发布->正式版***  
如不需要发布测试版，可直接在正式版中创建新的发布版本。

* 添加国家/地区  
一般全部选择。
* 创建正式版本  
上传app bundle。检查发布版本，没问题的话点击开始发布正式版。  
发布后，需要等待审核，审核没有问题的话就可以正式上线了！

应用下载地址[Download](https://avgle.top/download/)









---

> Author:   
> URL: http://localhost:1313/tech/build-flutter-app11/  

