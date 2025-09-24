---
date: '2025-09-24T16:14:40+08:00'
draft: false
title: '从Ananke主题切换为PaperMod主题'
summary: '默认的Ananke主题自定义项太少了，只好切换为社区评价比较好的PaperMod主题'
categories: '更新维护日志'
---
（写完才发现作为更新维护日志来说有点啰嗦了_(:з」∠)_ ）

之前跟着Ananke主题的文档配置了一天，最后发现还是有点过于毛胚房了，于是在ai的推荐下，我尝试切换为PaperMod主题。又跟着PaperMod的文档配置了两天，主要功能已经完善的差不多了，起码可以正常开始进行文章写作了。

现在要做的事情应该还剩，在博客文章中插入图片（要学习使用Hugo文档中的Page Bundle页面捆绑包部分）、文章评论功能（学习配置Giscus评论系统）、完善博客文章的预生成模板、学习Markdown语法。


看了一些有关eu.org域名审核的帖子，基本都说现在好像已经停止审核发放新的域名了，估计我申请的域名也没什么希望了。现在用的是Cloudflare Pages默认的pages.dev域名，好在没像Vercel那样完全无法访问。

不过众所周知Cloudflare在大陆并没有专门的服务器，使用ITDOG对博客域名进行Ping测试（运营商DNS），江苏的电信、联通、移动节点全部超时，怎么测都是一片红，更换为公共DNS（如阿里云223.5.5.5）就一切正常了，偶有几个节点显示超时；进行Tcping测试，运营商DNS，结果全绿，偶有几个节点显示超时，不过延迟还是有点高的，几乎都是浅绿色（延迟150ms+）；使用ITDOG的“网站测速”功能（官网描述为“该应用可从各地区、各线路发起HTTP速度测试；可分析：解析/连接/重定向/SSL/状态码/Head信息等”）测试博客域名，全国各地所有的中国移动测试节点全部为红色超时，但我让身边的人使用移动流量卡访问博客，是可以打开的，加载速度也正常；


PaperMod虽然自定义程度比默认的Ananke要高一些，但有些细节还是不够完善。问了GPT和Gemini，想要按照自己的想法进行自定义，需要学习一些前端开发的知识，起码要会写HTML和CSS代码。我不怎么会编程，想想就头大……

说实话，没有计算机专业知识，尤其是计算机网络相关的，上面提到的这些名词我都不理解是什么意思……


要继续使用Cloudflare Pages的默认域名，还是自己去买一个域名呢？看了下NameSilo上.top域名的报价，续费一年要35元，Spaceship上的.top域名续费一年要27元。一涉及到钱的问题果然就很纠结……而且问了一下群友，说.top的管局是国内的一家公司，会无缘无故停你域名的，他之前有个域名就被搞了，不知道是真是假。

当初只是想在互联网小角落拥有自己的一处小角落，可以随心所欲地表达自己。现在想想看，要折腾的东西其实还挺多的，感觉对于我来说都比较麻烦，而且这些知识还不算是目前我计划的未来发展方向能用到的。有一种从种小麦开始做面包的既视感（）


附：文档中有许多我不太明白的特性、一些不适用于全局配置文件的配置项、以及后续再做完善的功能，这里简单做一下记录备忘，以后再慢慢研究。

1.文章封面图/禁用响应式封面图片/启用封面图链接 https://github.com/adityatelange/hugo-PaperMod/wiki/Features#post-cover-image

2.从搜索结果中隐藏特定页面/搜索页快捷键 https://github.com/adityatelange/hugo-PaperMod/wiki/Features#search-page

3.Page Bundle页面捆绑包 https://gohugo.io/content-management/page-bundles

4.社交平台链接 https://github.com/adityatelange/hugo-PaperMod/wiki/Icons#social-icons

5.显示文章目录 https://github.com/adityatelange/hugo-PaperMod/wiki/Features#show-table-of-contents-toc-on-blog-post

6.在特定页面禁用面包屑导航 https://github.com/adityatelange/hugo-PaperMod/wiki/Features#breadcrumb-navigation

7.页面快捷键 https://github.com/adityatelange/hugo-PaperMod/wiki/Features#accesskeys

8.社交媒体卡片 https://github.com/adityatelange/hugo-PaperMod/wiki/Features#twitter-cards-support

9.置顶文章 https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#pin-a-post

10.在Markdown中居中图片 https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#centering-image-in-markdown

11.主页上只显示一个文件夹或部分的文章 https://github.com/adityatelange/hugo-PaperMod/wiki/FAQs#posts-from-only-one-foldersection-visible-on-home-page

12.PaperMod可用变量列表 https://github.com/adityatelange/hugo-PaperMod/wiki/Variables

13.Canonical URL（标准网址）

14.优化配置文件和Markdow的换行


**搭建博客时用到的参考资料（算是个感谢名单吧hhh）**

Hugo Getting started https://gohugo.io/getting-started/

Ananke Wiki https://github.com/theNewDynamic/gohugo-theme-ananke/wiki

PaperMod Wiki https://github.com/adityatelange/hugo-PaperMod/wiki

B站UP主二叉树树大佬的文章 https://2x.nz/posts/hugo

LMArena上的gpt-5-search和gemini-2.5-pro-grounding（ai真是太好用了！感觉离开ai要寸步难行力。LMArena也是个非常不错的平台，虽然功能性相比各家ai自己的网站有些简陋……）