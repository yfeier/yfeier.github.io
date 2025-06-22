# Hugo网站搭建1：环境


## 安装go

hugo依赖go环境，下载地址：

http://docs.studygolang.com/doc/install
或
https://golang.google.cn/

## 安装hugo

下载地址：
https://github.com/gohugoio/hugo/releases

在网站可找到两个版本  
[hugo_0.96.0_Windows-64bit.zip](https://github.com/gohugoio/hugo/releases/download/v0.96.0/hugo_0.96.0_Windows-64bit.zip)和[hugo_extended_0.96.0_Windows-64bit.zip](https://github.com/gohugoio/hugo/releases/download/v0.96.0/hugo_extended_0.96.0_Windows-64bit.zip)

推荐使用extended版本，否则修改下载的主题有的会不起作用。

## 建站

1. 生成hugo网站的基本文件。  
```
hugo new site blog
```

生成的文件夹结构如下：

```
blog
├── archetypes
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```

#### archetypes
在通过hugo new xxx 创建内容页面的时候，默认情况下hugo会创建date、title等front matter，可以通过在archetypes目录下创建文件，设置自定义的front matter。

#### config.toml
网站使用的配置文件。

#### content
网站的内容文件放在此处，可在里面建立新的文件夹代表不同的分类。

#### data
data目录用来存储网站用到一些配置、数据文件。文件类型可以是yaml|toml|json等格式。

#### layouts
存放用来渲染content目录下面内容的模版文件，模版.html格式结尾，layouts可以同时存储在项目目录和themes/<THEME>/layouts目录下。

#### static
用来存储图片、css、js等静态资源文件。

#### themes
用来存储主题文件。  



2. 在blog文件夹下执行  
```
hugo server
```

在本地启动网站，默认访问地址：
[http://localhost:1313/](http://localhost:1313/)


3. 生成发布文件
```
hugo
```

将在public目录下生成hugo编译后的网站所有文件，可用于发布。

---

> Author:   
> URL: https://yfeier.github.io/tech/build-hugo-site1/  

