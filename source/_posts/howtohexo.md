---
title: HEXO + GIT 建立blog
date: 2017-02-15 20:21:57
tags:
---
**你的电脑安装node.js**
[node.js安装包下载](https://nodejs.org/)

**你的电脑安装git**
linux mac自带，windows安装git bash

**hexo准备:**
  安装		sudo npm install -g hexo
  初始化	hexo init

**github准备:**
  在自己的github下建立yourname.github.io的repository
  添加SSH Keys（这个是github的使用，不多说，总之ssh-keygen后，把公钥复制到github中）

**新建添加自己的blog：**
  hexo本地大概的目录结构：
  
>  _config.yml    node_modules    public      source  
   db.json        package.json    scaffolds  themes
 
  修改_config.yml
```
deploy:

     type: git

     repo: https://github.com/[github_account]/[github_account].github.io.git

     branch: master
```
  1 执行命令，npm install hexo-deployer-git --save
  2 新建一篇文章hexo n "article_name"
  3 编辑生成的md文件，markdown格式
  4 生成静态页面hexo g
  5 部署到github，hexo d
  6 帮助hexo h
  7 想本机预览 hexo s

**文章中使用图片**
  修改_config.yml 
``` 
    post_asset_folder:true
```
  执行npm install https://github.com/CodeFalling/hexo-asset-image --save
  在.md文件中插入图片  
```
![logo](artical_name/logo.jpg)
```

**主题**
  我用的theme叫next
  [next主题作者的github](https://github.com/iissnan/hexo-theme-next)
  [next主题作者的blog](http://notes.iissnan.com/)
  [next主题的使用文档](http://theme-next.iissnan.com/)
**提醒自己：**
  用了hexo clean，别忘了把CNAME copy 到publish目录下
  CNAME的内容:
  guoxin.site
