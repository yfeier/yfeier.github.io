# Hugo网站搭建6：统计



如果希望统计网站的访问量等数据，可以在网页中加入统计代码。  

## 百度统计

https://tongji.baidu.com/web/homepage/index

在该网站注册并获取统计代码。
```html
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?***********";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>

```

需要将代码添加到网站全部页面的\</head>标签前。因此将代码放到themes\timeline\layouts\partials\head.html中即可。


## 谷歌统计

https://analytics.google.com/

```html
<script data-ad-client="********" async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
```
将统计代码加入head.html中。


如有不明处，参看下面的帮助文档。  
[为网站设置 Google Analytics（分析）(Universal Analytics)](https://support.google.com/analytics/answer/10269537?hl=zh-Hans)


## Cloudflare

https://dash.cloudflare.com/


从该网站添加自己的站点后，可查看网站的访问数据。  

本网站使用申请了域名，在域名管理中，
修改DNS服务器为
```
carol.ns.cloudflare.com
elmo.ns.cloudflare.com
```


在Cloudflare端设置DNS
添加类型A，名称为上面申请的域名， IPv4地址为上面域名分配的ip。  
添加类型CNAME，名称为www，目标为实际发布的网站地址。  


Cloudflare端还可以打开SSL/TLS加密模式，使网站访问网址支持https。  


---

> Author:   
> URL: http://localhost:1313/tech/build-hugo-site6/  

