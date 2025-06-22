# Hugo网站搭建7：发布



网站做好后，需要发布到网络上，github和gitlab都提供了免费的Pages功能。

github的Pages功能：
* 项目必须是公开属性
* 必须上传hugo编译后的网站文件

gitlab的Pages功能：
* 项目可以是私有  
* 可以上传hugo网站原始文件，gitlab提供了自动编译功能。


本网站选用gitlab的Pages。  

https://gitlab.com/

新建一个项目，项目名必须为【用户名.gitlab.io】。 
将hugo文件提交到项目后，配置CI/CD自动构建脚本，如果新建项目时选的是hugo项目，该脚本会自动生成。
```makefile
# All available Hugo versions are listed here: https://gitlab.com/pages/hugo/container_registry
image: registry.gitlab.com/pages/hugo:latest

variables:
  GIT_SUBMODULE_STRATEGY: recursive

test:
  script:
  - hugo
  except:
  - master

pages:
  script:
  - hugo
  artifacts:
    paths:
    - public
  only:
  - master
```

每次提交代码到gitlab后，该流水线会自动执行，运行hugo生成网站文件。  


也可以设置定时计划，如果上传的网页需要在之后的时间发布，在hugo的md文件中设置好publishdate字段，如果该字段是未来时间，则自动运行的流水线计划将不会将该md文件编译成网页，设置好的定时计划将在未来某一时间里编译该文件。  


此时访问【用户名.gitlab.io/项目名】即可访问你的网站。  
如本网站 https://yfeier.gitlab.io/my-blog/


如需要要自定义域名，可申请域名后绑定该地址。
申请域名后，在gitlab的Pages域名中绑定该域名。  

域名解析的设置中，设置主机记录www，记录类型CNAME，记录值为【用户名.gitlab.io】。



---

> Author:   
> URL: https://yfeier.github.io/tech/build-hugo-site7/  

