---
layout: post
title: "Octopress 安装 &amp; 主题配置"
date: 2016-06-02 09:39:15 +0800
comments: true
categories: 
---
Octopress install
---
```
git clone git://github.com/imathis/octopress.git octopress
cd octopress
bundler install
rake install #安装默认主题
```
####部署到Github Pages
---
在Github上创建一个新的仓库，仓库名的格式应按照username.github.io。其中，username是你的Github的user name。
####配置octpress的git仓库地址
`rake setup_github_pages`
####写blog
`rake new_post['blog title']`
这条rake命令会自动在source/_post/目录下生成一个以YYYY-MM-DD-title为格式，以.markdown为扩展名的文件。这个文件是使用Markdown进行编写，会被转换成静态HTML文件。

如果这篇文章没有完成，可以加一行published: false。在生成新内容，部署到远程时，这篇文章将不会被发表。
####发布blog
```
rake generate #根据source目录下的内容生成新的内容到_deploy目录下
rake deploy #提交到远程的master分支下
```
这时候已经可以通过username.github.io访问
####发布源码
```
git add .
git commit -m "source for blog page"
git push origin source
```
配置Octopress
---
目录下的_config.yml就是Octopress的主配置文件。这里包含Main Configs，Jekyll & Plugins，3rd Party Settings三个模块。
Main Configs
```
url: http://jeluomsy.github.io #这个由rake setup_github_pages时自动获取，不需要手动输入
title: Jeluomsy's Blog
subtitle: 记录学习生活的点滴收获
author: Jeluomsy
```
一般情况下，我们只需要修改title，subtitle，author。
Jekyll & Plugins
---
这个模块中可以修改的设置比较多。
####文件目录配置，保持默认即可。
```
root: /
permalink: /blog/:year/:month/:day/:title/
source: source
destination: public
plugins: plugins
code_dir: downloads/code
category_dir: blog/categories
```
####文章在博客中如何展示的一些配置
```
paginate: 2          # 每一页显示的博文数量
paginate_path: "posts/:num"  # 分页路径
recent_posts: 10       # 侧边栏‘最近文章’中显示的博文数量
excerpt_link: "Read on &rarr;"  # 类似于'阅读全文'按钮的功能
excerpt_separator: "<!--more-->" # 文章只预显示这条语句之前的内容
```
文章标题格式
`titlecase: false`

如果为true，就会默认首字母大写以及改变一些特殊字符的显示。默认为true。
####侧边栏配置
```
default_asides: [asides/recent_posts.html, asides/github.html, asides/delicious.html, asides/pinboard.html, asides/googleplus.html, custom/asides/about.html]
```
这里的路径是在source/_includes目录之下。用来显示在侧边栏显示那些内容，基本上是一些第三方社交网站。最后的about.html可以写一些个人介绍。
####3rd Party Settings
这里可以配置Github的信息，评论系统disqus，以及Google Analytics。这三个是我认为有必要配置的部分。 Github只需要填上github_user:设置项。disqus和Google Analytics需要获取对应的disqus_short_name:和google_analytics_tracking_id:才可以使用。

评论系统可以使用国内的多说，访问速度更快。

统计可以使用国内的百度分析。
####更换主题

Octopress的主题文件存放在source/.themes/隐藏目录下。在第一次成功执行rake install后，就会安装默认的主题。

如果希望更换主题，可以在Github上搜索关键字Octopress Theme。以我当前使用的主题Slash为例，按顺序执行以下命令。
```
$ cd octopress
$ git clone git://github.com/tommy351/Octopress-Theme-Slash.git .themes/slash
$ rake install['slash']
$ rake generate
```
如果在执行rake install['slash']时，出现
`Build Warning: Layout 'nil' requested in atom.xml does not exist.`

是因为无法识别nil这个值。

将source/atom.xml和source/_includes/custom/category_feed.xml两个文件中
`layout: nil`

改为
`layout: null`

####分类名称永远小写的解决方法

在使用过程中最让我头疼的还是分类名称一直小写的问题。我把源代码中关于分类的部分都看了一遍，没有发现什么地方让分类名变成了小写。但是在网站上显示的时候却直接变成了小写。为了解决这个问题，我只能采取最笨的方法，直接修改source/_plugins目录下的categories_generate.rb插件，来控制分类名称的输出。
```
case "#{category}"
    when "ios"
        self.data['title']       = "#{title_prefix}iOS"
        self.data['description'] = "#{meta_description_prefix}iOS"
    else
        self.data['title']       = "#{title_prefix}#{category}"
        self.data['description'] = "#{meta_description_prefix}#{category}"
end
```
这个方法在每一次新增分类时都可能需要修改，添加新的when语句。
Octopress themes
---
| Name	| URL	|
| --- 	|  --- 	|
| Octopress Flat | https://github.com/alexharris/octopress-flat/ |
|	a	|	http://sofreshandsogreen.herokuapp.com/	|
| Slash | https://tommy351.github.io/Octopress-Theme-Slash/index_tw.html |
| | http://jser.it/ |
| | https://syui.github.io/blog/archives/ |
| Oct2 | http://bijumon.github.io/oct2/ |
| Octoplate | https://github.com/mjhea0/noLogo#screenshots |
| minimized | https://github.com/lmm/minimized#screenshots |
| | http://blog.justin.kelly.org.au/octopress-theme/ |
| | https://www.lijinma.com/blog/archives |
| Terminal | http://ivanjovanovic.com/ |
| | https://mmistakes.github.io/hpstr-jekyll-theme/theme-setup/ |
|| https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes |
Haroopad
---
跨平台，代码高亮，Vim 键绑定，多列模式，行号，折叠， Github Flavored Markdown 等功能~  
Ubuntu14 从官网下载deb包双击安装  


Reference
---
http://jeluomsy.github.io/blog/2016/01/03/build-octopress-on-github-pages/
