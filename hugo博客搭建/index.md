# hugo博客搭建


> 最近把博客从hexo搬到了hugo，感觉确实比hexo快了不少，尤其是在部署的时候，直接push就好了，关于hugo的教程网上有很多，本篇文章也是自己(mac)的一份总结。


## hugo下载
* mac系统,下载可能会很慢
```js
brew install hugo
```
如果下载很慢，可以直接去GitHub下载
<https://github.com/gohugoio/hugo/releases>

* 查看版本
```php
hugo version
```
出现版本信息则安装成功。

## 创建site
* 创建一个site,名称最好使用你的GitHub名称，比如yourGithubName.github.io
```js
hugo new site yourGithubName.github.io
```
* 进入这个目录，创建一篇文章
```js
hugo new posts/test.md
```
此时会在content目录下创建一个目录posts，下面创建一篇文章test.md。
* test.md内会是这样,draft为true表示草稿状态，要发布时改为false即可
```md
---
title: test.md
date: xxxxxxxxx
draft: true
---
```

## 关于主题
* 关于主题，我使用的是这个[LeaveIt](https://github.com/liuzc/leaveit)主题，具体使用GitHub上有详细的教程。

## 本地浏览
* 使用hugo server本地浏览
```js
hugo server
```

## 发布到GitHub
* 删除原有hexo的仓库，重新创建一个新仓库格式仍然是 你的用户名.github.io。
* 执行hugo,这时候会在当前目录下生成public文件夹
* cd到public文件夹，将此文件夹的所有内容push到刚刚创建的仓库即可。








