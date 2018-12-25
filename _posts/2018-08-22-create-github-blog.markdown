---
layout: post
title: "create github blog"
date: 2018-08-22 16:17:22 +0800
categories: [github]
---

***本地配置jekyll***
1. macos 或 linux 参考 [installation](https://jekyllrb.com/docs/installation/)
2. window 安装 参考 [jekyll on windows](https://jekyllrb.com/docs/windows/)

***生成博客***
1. 首先创建github账号，并登录github
2. 创建一个资源库名称和你的github账号相同
3. git clone 资源库名称 到本地
4. cd 本地资源库
5. jekyll new .
6. 修改 gemfile主题，替换为gem "minima", "~> 2.0"，命令行进入项目，执行bundle install
7. 修改_config.yml主题，theme: minima替换为theme: jekyll-theme-cayman-blog
8. git push origin master

***github与本地环境区别注意事项***
1. syntax highlight：上面搭建的酵环境中，语法高亮在本地正常，但push到github上时，语法高亮插件失败，经过排查，发现创建的酵环境与实际的github环境还是区别，为了和github环境相同，在`_config.yml`中加配置`gem "github-pages", group: :jekyll_plugins`， 然后bundle install，到此本地环境和github环境同步，highlight 标签正常
2. 和githb环境同步后，发现首页的**Lastest Post**的摘要(excerpt)消失了，经过查问题搜索，找到问题，excerpt默认的摘要的是第一个段落，但github环境不识别段落，就此在`_config.yml`中加入`exceprt_separator: "\n"`，摘要正常显示