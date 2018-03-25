---
title: Hexo+GithubPages&Coding搭建博客
date: 2018-03-11 22:22:04
photos: http://p4r569uff.bkt.clouddn.com/hexo.PNG
tags: 
- Hexo
- Github
- Coding
categories: 博客
copyright: true
top:
---

>我是如何搭建博客的

<!--more-->

## 前言： ##
>关于我的博客自然也是看了很多网上的教程才一步一步搭建成功的，多多少少都会免不了一番折腾，基本上是跟着网上各式各样的教程一步一步搭建成功的。搭建博客并不难，但是为了丰富个人的博客页面（瞎折腾），陆陆续续的也花了不少时间，总算完工了，所以决定写一篇文章来记录下大致的搭建过程。

## 搭建博客的原因： ##
>记录生活，记录学习上的点滴（啧啧）。每个人都有自己的理由，无论你是码农，还是其他职业，你都值得并且有能力去搭建一个博客。

-----------------------------------------------------------------------

## 你需要了解的知识： ##
>搭建博客的方法：

- 博客平台注册，如博客园，CSDN,新浪博客等。  
- 博客框架搭建。  
- 代码手写。 

如标题所言，我用的是hexo博客框架搭建的，并利用GitHubPages和CodingPages作为托管平台。
### HEXO ###
Hexo是一个简单地、轻量地、基于Node的一个静态博客框架，可以方便的生成静态网页托管在github和Heroku上。
### GitHubPages ###
GitHubPages可以被认为是用户编写的、托管在github上的静态网页。由于它的空间免费稳定,可以用于介绍托管在github上的Project或者搭建网站。
### CodingPages ###
CodingPages是一个静态网页托管和演示服务，其实和GitHubPages功能一样，用于托管博客、项目官网等静态网页，也可以部署开源博客、CMS等动态网页。
### GitHub ###
可作为免费的远程仓库，托管自己的代码。还是一个开源协作社区，通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。

关于这些知识，如果你想了解的更多，建议自行百度。

## 搭建过程 ##
**搭建步骤：**

   1.   获得个人网站域名
   2.   GitHub创建个人仓库
   3.   安装Git
   4.   安装Node.js
   5.   安装Hexo
   6.   推送网站
   7.   绑定域名
   8.   更换主题

>其实网上搭建博客的方法很多，不同框架，不同平台，不同主题。搭建的方法也是大同小异，不一定哪一篇教程就适合你，完全照搬某个教程也不一定能成功，还是得多参考，多尝试，自己理解了大致方法就明白自己要做哪一步该做哪一步。这里我给上主要的教程链接。

首先看这个：[GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)。有些地方并不详细，比如搭建个人仓库的部分，可以自行百度下详细教程。在安装hexo的时候，我也出现了问题，可以参考这个教程的做法：[使用github+Hexo人人都能拥有一个美美的博客](https://blog.csdn.net/working_harder/article/details/52437783)。接着是绑定域名的部分，实践发现只要添加两条解析记录就可以了，看图：

![Alt text](http://p4r569uff.bkt.clouddn.com/ali.jpg)

一些其它问题你都可以参照这个博客的做法来解决：[...Miss.j BlogDiary...](http://jmyblog.top/)。

博客搭建完成后，如果在浏览器中显示错误，那么可能是域名设置和绑定部分出现了错误。编写好文章后，可以通过先输入hexo s命令，在浏览器中输入： http://localhost:400/ 在本地预览效果，修改完成后，再推送到github上。

想要把网站弄得**个性化**一点，参考这几个教程(**针对next主题**)：[hexo的next主题个性化教程:打造炫酷网站](https://zhuanlan.zhihu.com/p/28128674)、[2小时还你一个集打赏、评论、RSS功能于一身的个人博客](https://www.jianshu.com/p/5973c05d7100)、[Hexo博客搭建及NexT主题个性化设置](https://www.jianshu.com/p/e75f208b5290)。如果你也想为自己的网站添加宠物，经个人实践，之前教程中的方法已经行不通了，你可以参考这两个教程：[hexo-helper-live2d
](https://github.com/EYHN/hexo-helper-live2d)、[ohyhello](https://www.ohyhello.com/889/)。

## 博客备份 ##
当你搭建好一个博客后，最好还是给自己的博客备份一下，万一哪天不小心把博客文件删了，或者是换电脑了，没有hexo的源文件还是很麻烦的。你可以选择拷贝一份到你的u盘上或者是云端的办法来备份，这里我使用的是备份到github上。老规矩，链接附上：[怎么去备份你的Hexo博客](https://www.jianshu.com/p/baab04284923)、[使用Hexo搭建博客，备份至GitHub过程 ](https://blog.csdn.net/u012195214/article/details/72721065)。

## THE END ##
最后，我想说，看完这篇文章，你可能会说怎么一大堆链接啊，看的心累。呃.....这个嘛......事实上我搭建博客的时候，看的教程自然不止这上面分享的这么多，以上分享的教程个人觉得还是比较实用的。搭建博客其实就是在考验你自己解决问题的能力和耐心，方法很多，教程更是多不胜数，这些教程在网上都能找到，我相信只要你有耐心，建好自己的博客是迟早的事。如果能靠自己的能力完成，那一定是满满的成就感。所以说，像我这种渣渣都可以，你肯定也是可以的！O__O



