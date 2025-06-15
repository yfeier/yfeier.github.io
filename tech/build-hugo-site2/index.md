# Hugo网站搭建2：主题


## 安装主题

可从该网站选择合适的主题。  
https://themes.gohugo.io/


[本网站](https://avgle.top/pages/)使用下面的主题  
https://themes.gohugo.io/themes/hugo-theme-timeline/

将下载后的主题文件夹放到自己的网站目录的themes文件夹下面，主题文件中的exampleSite是主题提供的示例网站，可删除。

设置网站配置文件config.toml中的主题
```
theme = "timeline"
```

themes文件夹下面可放置不同的主题，在配置中设置当前使用的主题即可。只有配置中设置的主题起作用。  


此时所有的板块将使用该主题。由于hugo只能使用一个主题，如果在某一板块想使用别的主题layout，可以在layouts文件夹下定义该板块的layout模板文件。

如本网站blog模块，在layouts文件夹下新建blog文件夹，在blog文件夹中新建list.html列表文件模板和single.html内容模板。




---

> Author:   
> URL: http://example.org/tech/build-hugo-site2/  

