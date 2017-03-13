---
title: Octopress to Hexo
date: 2017-03-12 21:24:58
categories: 
tags: [Hexo, Octopress]
comments: true
keywords: 
description:
header-img: ""
---
三年前，从Wordpress转到了Octopress。Octopress是一个不错的平台，不足就是官方文档少，版本更新缓慢。原本写博客都是在家中的Macbook Air上面，这次本想在默默耕耘的老黑T420上配置Octopress，转而一想，何不趁此机会改朝换代。

Hexo是之前经常看到的平台之一，基于Node.js，文档丰富，社区也比较活跃，所以就决定搬家到Hexo。基本框架和文件格式都近似，迁移不是很困难。有关Hexo配置的文章很多，这里就也不详细记录了。

主要命令:

```
$ hexo new "postName"
$ hexo new page "pageName"
$ hexo g	//生成静态文件
$ hexo s 	//启动服务器
$ hexo d 	//部署网站
$ hexo g -d //生成并部署
```

文章迁移。只要把 Octopress `source/_posts` 文件夹内的所有文件转移到 Hexo 的 `source/_posts` 文件夹，并修改 _config.yml 中的 new_post_name 参数: `new_post_name: :year-:month-:day-:title.md`。

网站继续利用github pages，把原来网站进行备份并重建名为`username.github.io`的项目。

安装以下插件： `$ npm install hexo-deployer-git --save`

配置文件`_config.yml`需要加入git的地址，如
```
deploy:
  type: git
  repo: git@github.com:username/username.github.io.git
  branch: master
  message: "Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }}"
```

部署过程出现`Permission denied (publickey)`的错误，输入`$ ssh -T git@github.com`之后也是错误，然后发现之前没把老黑的SSH Key加到Github上。虽然之前有一对Key，不过为了方便，从新生成了Key并成功连接了Github。具体见[Connecting to github with ssh](https://help.github.com/articles/connecting-to-github-with-ssh/)。

对于版本管理，许久没用，有一些生疏了，就把[远程操作](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)这篇翻出来看。因为是用了别人的主题，所以有必要通过`$ git branch`和`$ git remote -v`来查看一下当前本地和远程分支并进行管理和删减。

hexo部署时会将产生的静态网页推送到username.github.io项目中默认的master分支。为了给网站备份，需要在远程添加另一个分支。首先添加远程主机，并命名为origin：`$ git remote add origin <git repo address>`。
`$ git add .`和`$ git commit -m "msg"`之后，运行`$ git push origin master:source`，即将本地master分支推送到远程origin的source分支。

绑定域名。因为之前已经绑定过了，只需在github的username.github.io项目的Settings -> Custom domain里添加网址即可。