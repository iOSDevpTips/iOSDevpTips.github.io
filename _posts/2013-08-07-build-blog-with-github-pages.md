---
layout: post
title:  "用Jekyll在GitHub Pages上搭建博客"
date:   2013-08-07 14:11:15
categories: Tutorial Jekyll
---

本教程将会教你如何用[Jekyll](http://jekyllrb.com)在[GitHub Pages](https://pages.github.com/)上搭建一个博客（注：适用OS X环境）。
需要指出来的是还可以用[Jekyll Bootstrap](http://jekyllbootstrap.com/)来完成，而且Jekyll Bootstrap还集成了评论等功能，主要步骤不变，多了几条配置。

## 搭建博客  

### 本地安装Jekyll  
1. 检查[Ruby](https://www.ruby-lang.org/zh_cn/)环境

      Mac默认有Ruby环境，打开Terminal，运行`ruby --version`，确保Ruby的版本号大于2.0.0。
      修改gem的源，如果不修改则需翻墙。

          gem sources --remove https://rubygems.org/  
          gem sources -a https://ruby.taobao.org/  
          gem sources -l
          gem update --system  

2. 安装[Bundler](http://bundler.io/)

      Bundler是一个包管理程序，能大大的简化手动安装jekyll。如果没有安装Bundler，运行`gem install bundler`来安装。

3. 安装jekyll

      在项目的根目录创建一个`Gemfile`文件，输入

        source 'https://ruby.taobao.org'
        require 'json'
        require 'open-uri'
        versions = JSON.parse(open('https://pages.github.com/versions.json').read)
        gem 'github-pages', versions['github-pages']

    然后运行`bundle install`即可安装。

    如果遇到RedCloth报错，检查Xcode的command line tools是否安装；

    如果nokogiri报错libiconv is missing，运行：

              gem uninstall nokogiri  
              xocde-select --install  
              gem install nokogiri  

4. 运行
    可以用bundle运行`bundle exec jekyll serve`，也可以直接用jekyll运行`jekyll serve --watch --drafts`。

5. 更新
    Jekyll更新很频繁，为了确保本地环境和GitHub同步，运行`bundle update`来更新。

### [GitHub](https://github.com/)配置
1.  新建repository，命名为username.github.io（username为你GitHub的用户名）。
2.  clone该项目到本地。
3.  'jekyll new .'安装Jekyll到当前目录。

## 配置博客

### 配置_config.yml
项目根目录内的_config.yml即为jekyll的配置文件，按照需要修改即可。
值得注意的是，要把markdown解析器换成redcarpet，因为GitHub Pages就是用的这个。

```
# Build settings
markdown: redcarpet
redcarpet:
  extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "tables", "strikethrough", "superscript", "with_toc_data", "footnotes","space_after_headers","highlight","underline","quote","lax_spacing"]

```

### 配置SEO
- 避免无效链接
_config.yml的permalink字段，默认是：`/:categories/:year/:month/:day/:title`，如果修改了category或者时间，所有的内部链接都要修改，否则就造成了死链接。另外之前搜索引擎收录的页面也全部失效了。  
所以我把permalink修改成：`permalink: /:title`  

- 去[Google站长工具](http://www.google.com/webmasters/)和[百度站长工具](http://zhanzhang.baidu.com/)提交你的网址等。

### 配置Analytics(选配)
我采用的是[Google Analytics](https://www.google.com/analytics/)，也有其他的可供选择。
把提供的JS放在head.html里面就可以了。

## 写博客
运行`rake post title="Hello World"`，会在posts目录中自动生成一个year-month-day-hello-world.md文件，编辑它即可。
编辑完之后运行jekyll`jekyll serve --watch`，在浏览器中打开 http://0.0.0.0:4000 可在本地预览博客。  
如果只想打个草稿，暂时不想发布，可以在drafts目录中新建一个Title.md文件，运行`jekyll serve --watch -drafts`，jekyll会自动连草稿一起编译，草稿的日期为当前的日期。
写完草稿想发布了，把这个文件改成year-month-day-title.md格式放到_posts目录即可。  
最后commit&push到GitHub上去即可看到博客内容拉。

### 结束
至此，整个搭建博客的过程就结束了。
最后推荐用[GitHub for Mac](https://mac.github.com/)和[Atom](https://www.atom.io/)
管理和编辑项目，用Atom来编辑Markdown真是太方便了。

### 参考：
1.http://jekyllrb.com/docs/home/

2.https://help.github.com/articles/using-jekyll-with-pages/
