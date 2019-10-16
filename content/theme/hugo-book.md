---
date: 2018-09-09T21:07:13+01:00
title: hugo-book主题
menu:
  main:
    parent: "theme"
weight: 301
keywords:
- theme
- hugo-book
- hugo-book主题
description : "介绍hugo的hugo-book主题，使用方式和订制化"
---

## 主题介绍

hugo-book官网：

https://themes.gohugo.io/theme/hugo-book/

https://github.com/alex-shpak/hugo-book

优点：

- 类 gitbook
- 有 menu 。再也不用过度思考weight应该怎么设置了。[hugo-theme-learn](https://themes.gohugo.io/hugo-theme-learn/)就是这个地方不方便。
- 左边的导航栏支持多级菜单

缺点：

- [搜索不支持中文](https://github.com/alex-shpak/hugo-book/issues/80)
- [移动端不支持搜索](https://github.com/alex-shpak/hugo-book/issues/74)

这个主题非常适合用于文档，左边的导航栏支持多级菜单，样式也简单美观，而且对移动设备支持很好。

我主要用它来做学习笔记，替代之前长期使用的gitbook（gitbook的问题主要是本地生成内容速度太慢，而gitbook官方网站访问速度又不好）。当前这个hugo学习笔记用的就是此主题。

## 使用者示例

- https://github.com/solareenlo/blog-hugo  这里的 [.gitmodules](https://github.com/solareenlo/blog-hugo/blob/master/.gitmodules) 文件，可以方便切换 themes , 以及 book-ori 与 book-fork 区分 hugo-book 主题 的定制问题

## 准备工作

### 安装 hugo_extended 版本

如果不安装，会报下面错误

```
DESKTOP-APB1HCJ% hugo serve
Building sites … WARN 2019/10/16 17:41:53 found no layout file for "HTML" for "page": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
ERROR 2019/10/16 17:41:53 Transformation failed: TOCSS: failed to transform "book.scss" (text/x-scss): this feature is not available in your current Hugo version, see https://goo.gl/YMrWcn for more information
WARN 2019/10/16 17:41:53 found no layout file for "HTML" for "page": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
WARN 2019/10/16 17:41:53 found no layout file for "JSON" for "home": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
Total in 26 ms
Error: Error building site: logged 1 error(s)
DESKTOP-APB1HCJ%
```

https://github.com/alex-shpak/hugo-book/issues/9#issuecomment-447267714

```
wget -O ./hugo_extended_0.58.3.tar.gz https://github.com/gohugoio/hugo/releases/download/v0.58.3/hugo_extended_0.58.3_Linux-64bit.tar.gz
tar -zxvf ./hugo_extended_0.58.3.tar.gz
mv ./hugo_extended_0.58.3/hugo /usr/local/bin/hugo
```
