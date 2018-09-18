---
date: 2018-09-09T21:07:13+01:00
title: academic
weight: 30
---

academic官网：

https://themes.gohugo.io/academic/

## 安装

采用fork方式，方便未来更新。

1. fork下面的项目

	https://github.com/sourcethemes/academic-kickstart

2. 修改项目名称为`skyao.io`
3. git clone这个fork的项目到本地
4. 执行git submudule命令初始化theme

	```bash
   cd skyao.io
   git submodule update --init --recursive
	```

5. 运行服务器

	```bash
	hugo server
	```

6. 浏览器访问 [localhost:1313](http://localhost:1313)

## 套用demo的内容

为了快速开始，直接套用demo的内容：

```bash
cp -av themes/academic/exampleSite/* .
```

## quickstart

按照[Getting started](https://sourcethemes.com/academic/docs/get-started/)文档的建议

### 修改config.toml文件

- title
- params下的

	- name
	- role