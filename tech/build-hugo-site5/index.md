# Hugo网站搭建5：评论



在静态网页中添加评论功能，可使用第三方评论系统。

## Valine评论系统

Valine是一款基于[LeanCloud](https://leancloud.app/)的快速、简洁且高效的无后端评论系统。  

Valine详细可参见下面的网站。  
https://valine.js.org/  
https://github.com/xCss/Valine  


注册LeanCloud账号后，登录管理界面，添加一个应用。  
https://console.leancloud.app/apps  
免费版提供256M容量。  
在该应用的设置->应用凭证中可以找到AppID和AppKey等，用于配置文件中。


在hugo的配置文件中添加如下配置。  
```toml
# Valine.
# You can get your appid and appkey from https://leancloud.cn
# more info please open https://valine.js.org
[params.valine]
  enable = true
  appId = '**********'
  appKey = '*********'
  notify = false  # mail notifier , https://github.com/xCss/Valine/wiki
  verify = false # Verification code
  avatar = 'monsterid'
  placeholder = 'Say Something...'
  visitor = true
  lang = 'en'
```

在需要添加评论的页面添加下面的代码，即可使用评论功能。  
```html
<!-- valine -->
{{- if .Site.Params.valine.enable -}}
<!-- id 将作为查询条件 -->
<span id="{{ .Permalink }}" class="leancloud_visitors" data-flag-title="{{ .Title }}">
<p></p>
</span>

<div id="vcomments"></div>
<script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
<script src='//unpkg.com/valine/dist/Valine.min.js'></script>

<script type="text/javascript">
  new Valine({
      el: '#vcomments' ,
      appId: '{{ .Site.Params.valine.appId }}',
      appKey: '{{ .Site.Params.valine.appKey }}',
      notify: '{{ .Site.Params.valine.notify }}', 
      verify: '{{ .Site.Params.valine.verify }}', 
      avatar:'{{ .Site.Params.valine.avatar }}', 
      placeholder: '{{ .Site.Params.valine.placeholder }}',
      visitor: '{{ .Site.Params.valine.visitor }}',
      lang: '{{ .Site.Params.valine.lang }}'
  });
</script>
{{- end -}}

```


## Waline评论系统

一款基于 Valine 衍生的简洁、安全的评论系统。

https://waline.js.org/


1. 首先在[LeanCloud国际板](https://leancloud.app/)注册一个应用。  
   进入应用，选择左下角的 设置 > 应用 Key。你可以看到你的 APP ID,APP Key 和 Master Key。请记录它们，以便后续使用。  


2. 在[Vercel](https://vercel.com/)使用Github或Gitlab账号登录。
   [创建](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fwalinejs%2Fwaline%2Ftree%2Fmain%2Fexample)一个项目。  

   此时 Vercel 会基于 Waline 模板帮助你新建并初始化仓库，仓库名为你之前输入的项目名。  


   部署成功后，点击【Go to Dashboard】跳到应用的控制台。  
   点击顶部的 Settings - Environment Variables 进入环境变量配置页，并配置三个环境变量 LEAN_ID, LEAN_KEY 和 LEAN_MASTER_KEY 。它们的值分别对应上一步在 LeanCloud 中获得的 APP ID, APP KEY, Master Key。


   环境变量配置完成之后点击顶部的 Deployments 点击顶部最新的一次部署右侧的 Redeploy 按钮进行重新部署。该步骤是为了让刚才设置的环境变量生效。


   此时会跳转到 Overview 界面开始部署，等待片刻后 STATUS 会变成 Ready。此时请点击 Visit ，即可跳转到部署好的网站地址，此地址即为你的服务端地址。  


3. HTML引入 (客户端)

在hugo的配置文件中添加如下配置。  
```toml
[params.waline]
  enable = true
  lang = 'en'
  serverURL = 'https://********.vercel.app/'
  avatar = 'monsterid'
```

在需要添加评论的页面添加下面的代码，即可使用评论功能。  
```html
<!-- Waline -->
{{- if .Site.Params.waline.enable -}}
<p></p>
<script src="//cdn.jsdelivr.net/npm/@waline/client"></script>

<div id="waline"></div>
<script>
  Waline({
    el: '#waline',
    serverURL: '{{ .Site.Params.waline.serverURL }}',
    avatar:'{{ .Site.Params.valine.avatar }}', 
    lang: '{{ .Site.Params.waline.lang }}',
    visitor: true,
  });
</script>
{{- end -}}
```

评论服务此时就会在你的网站上成功运行。  


#### 评论管理 (管理端)
部署完成后，请访问 \<serverURL>/ui/register 进行注册。首个注册的人会被设定成管理员。
管理员登陆后，即可看到评论管理界面。在这里可以修改、标记或删除评论。
用户也可通过评论框注册账号，登陆后会跳转到自己的档案页。

---

> Author:   
> URL: https://yfeier.github.io/tech/build-hugo-site5/  

