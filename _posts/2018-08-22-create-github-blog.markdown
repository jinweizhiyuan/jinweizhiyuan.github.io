---
layout: page
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