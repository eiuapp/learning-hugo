---
date: 2018-09-10T21:07:13+08:00
title: 安装
weight: 20
---

安装hugo之前，先安装好golang。

## Linux安装

在[Hugo Releases](https://github.com/spf13/hugo/releases)页面下载对应操作系统版本的安装包，如：

- hugo_0.48_Linux-64bit.deb

deb文件直接安装即可。

```bash
sudo dpkg -i hugo_0.48_Linux-64bit.deb 
```

验证安装：

```bash
$ hugo version
Hugo Static Site Generator v0.48 linux/amd64 BuildDate: 2018-08-29T06:33:51Z
```

## 自动发布

以下是jenkins自动生成并发布到nginx的简单脚本：

```bash
#git submodule update --init --recursive

# clean 
cd /var/www/skyao/
rm -rf 404.html home index.xml project sitemap.xml tags categories img js publication site.webmanifest talk files index.html post publication_types styles.css
cd /var/lib/jenkins/workspace/skyao.io/

hugo --theme=academic --baseUrl="http://skyao.io/" -d "/var/www/skyao/"
```

