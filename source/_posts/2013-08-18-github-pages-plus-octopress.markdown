---
layout: post
title: "Github Pages + Octopress"
date: 2013-08-18 20:21
comments: true
categories: [github, octopress]
published: true
---
发现现在很多git用户都开始使用[github pages](http://pages.github.com/) + [octopress](http://octopress.org/)来建立自己的blog。Octopress宣扬的是blogging framework for hacker,如果懂点shell commands和基本的git用法就可以使用。所以自己也打算尝试一下并且开启自己的blog。
<!--more --->
##Octopress Setup
###Requirement
* Install [Git](http://git-scm.com/downloads),强烈推荐使用[Homebrew](http://brew.sh/)来安装git
* Install Ruby 1.9.3 using [RVM](https://rvm.io/)
{% codeblock %}
$ \curl -L https://get.rvm.io | bash -s stable  --ruby =1.9.3
{% endcodeblock %}
###Setup Octopress
{% codeblock %}
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress    # If you use RVM, You'll be asked if you trust the .rvmrc file (say yes).
$ ruby --version  # Should report Ruby 1.9.3
{% endcodeblock %}
下面安装依赖文件和安装octopress
{% codeblock %}
$ gem install bundler
$ bundle install
$ rake install
{% endcodeblock %}
##Deoploying to Github pages
###Create github repository
在你的github下面创建一个符合`username.github.io`. 创建好之后就可以在本地Octopress配置github了
{% codeblock %}
$ rake setup_github_pages
{% endcodeblock %}
这个命令将会帮你创建以下几点:
1 .询问你的github pages的repositiory url，这里可以填ssh地址比如：`git@github.com:username/username.github.io.git`.
2. 重新命名origin到octopress.
3. 设置origin remote为自己的github pages的url.
4. 从当前master branch切换到source.
5. 根据repository来配置blog地址.
6. 在_deploy下面建立master branch用作deployment.
下面可以运行
{% codeblock %}
$ rake generate
$ rake deploy
{% endcodeblock %}
这将会创建你的blog，copy到_deploy/下面，然后add,commit,push到master branch。几秒钟之后你就会在你的blog上看到你刚才更新的内容
也不要忘记把source下面的内容commit到github
{% codeblock %}
$ git add .
$ git commit -m 'commit message'
$ git push origin source
{% endcodeblock %}
