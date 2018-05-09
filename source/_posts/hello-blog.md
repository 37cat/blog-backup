---
title: 记录本blog搭建历程 --- hexo配合github pages搭建
date: 2018-05-03 10:22
---

一直都希望能够搭建一个属于自己的blog，来满足自己~~装逼~~记录学习经历的需求。

但是一直由于~~各种原因~~(懒)，一直拖了下来，最近终于懒癌缓解，把这个blog搭了起来，那就赶紧记录下搭建过程，以及其中踩到的坑，算是对这一通折腾的证明。

<!--more-->

### 确定思路

其实搭建博客由很多种实现方法，据我所查资料发现的blog实现方式就有几种：

+ 直接使用现有平台，包括但不限于CSDN、稀土掘金等
+ 使用独立域名+VPS+blog系统，搭建个人独立blog

本着折腾的精神自然要选择第二种看上去就很费劲的思路了，好处是可定制的程度很高，真正是属于码农的自留地。

### 使用hexo生成blog框架

首先说明下其实hexo是属于上面所说的blog系统的一种，它是基于javaScript的，其他的基于python的Django应该也有类似的blog系统。

选择好了方向自然需要装环境，

- node.js
  >http://nodejs.cn/download/

  注意下版本，我一开始装的是4.几的版本，之后的过程中报了个各种兼容性问题，从新安装8.11.1稳定版本就一切OK了

node.js正确安装之后会自动添加到环境变量PATH路径中，所以可以直接使用，接下来就是安装hexo了。

打开命令行窗口，推荐使用git bash,据其他教程说使用Windows cmd会出现“意想不到”的问题。

- 安装hexo
    >`npm install -g hexo`

    npm是node的包管理工具类似于python的pip，估计全称就是node package manage，由于众所周知的墙的存在npm有时候会很慢，可以尝试国内镜像cnpm。

- 安装依赖包
    >`npm install`

- 在想要放blog的文件夹下
    >`hexo init`

- hexo生成gengerate
    >`hexo g`

- hexo打开本地服务器server,本地调试用 localhost:4000
    >`hexo s`

- hexo部署deploy
    >`hexo d`

    部署到GitHub pages上还需要一个扩展
    >`npm install hexo-deployer-git --save`

### 把blog放到github上


### 关联独立域名到github主页


参考链接：

[如何搭建一个独立博客——简明Github Pages与Hexo教程](https://www.jianshu.com/p/05289a4bc8b2)

[手把手教你使用Hexo + Github Pages搭建个人独立博客 ](https://segmentfault.com/a/1190000004947261)

[github怎么绑定自己的域名？](https://www.zhihu.com/question/31377141)
