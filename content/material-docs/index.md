---
date: 2018-09-09T21:07:13+01:00
title: material-docs
weight: 400
---

## 主题介绍

material-docs官网：

http://themes.gohugo.io/theme/material-docs/

这个主题非常适合用于文档，左边的导航栏支持多级菜单，样式也简单美观，而且对移动设备支持很好。

我主要用它来做学习笔记，替代之前长期使用的gitbook（gitbook的问题主要是本地生成内容速度太慢，而gitbook官方网站访问速度又不好）。当前这个hogo学习笔记用的就是此主题。

## 准备工作

### Fork并定制

material-docs的github仓库已经有很长时间没有更新，基本停留在两年前。从实践中看，有很多问题，对新的hugo版本的支持也很不好。

因此，采用fork的方式，建立自己的仓库：

https://github.com/skyao/hugo-material-docs

期间做了很多了更新：

1. PR那边有很多已经被发现而且修订提交的问题，但是一直没有merge，我手工合并到自己的仓库
2. 弃用google统计，改用百度统计，具体做法和 [academic](../academic/analytices/) 中的做法是一样的
3. 同样进行了本地加速，参照  [academic](../academic/local-speed-up/) 中的做法
4. 修改了layout文件，主要是删除了版权申明，作者信息，下载修改为意见反馈，现在的版面非常的干净，没有任何多余的东西
5. 汉化，没有做标准的i18n，因为是给自己定制，因此直接在layout文件中修改为中文。
6. 修改了exampleSite的内容，删除原有的页面和图片，改为学习笔记的常见页面如介绍，安装等，方便后续使用
7. 提供了简单的gitbook模版文件支持，可以生成一个简单带有文字说明和链接的页面，以便将原有gitbook用户导流到新的hugo笔记

## 使用方式

以新建一个学习笔记为例，详细描述需要的步骤和命令。

### 新建学习笔记

1. 在github中新建仓库，名为 **learning-cilium**的Cilium学习笔记

2. 在本地新建一个hugo站点

    ```bash
    hugo new site learning-cilium
    cd learning-cilium
    ```

3. 下载更新主题的脚本并执行

    ```bash
    wget https://raw.githubusercontent.com/skyao/hugo-material-docs/master/exampleSite/update_theme.sh
    chmod +x update_theme.sh
    ./update_theme.sh
    ```

	命令完成之后，`themes/hugo-material-docs`目录下就有我们自己的主题文件内容。

4. 从`exampleSite`目录复制需要的站点初始化文件到新站点

    ```bash
    cd themes/hugo-material-docs
    # 复制config.toml文件
    cp config.toml ../../../config.toml
    # 复制content目录和static目录
    cp -r content/ ../../../content/
    cp -r static/ ../../../static/
    # 复制gitignore文件
    cp gitignore-example ../../../.gitignore
    ```

5. 修改新站点的`config.toml`文件

    以下内容必须修改：

    ```toml
    title = "学习笔记"
    repo_url = "https://github.com/skyao/learning-hugo"
    ```

6. 此时已经开始运行`hogo server`命令来查看新站点的内容了

7. 继续修改站点内容

	主要是content目录下的`index.md`以及`installation`和`introduction`两个目录下的文件，也可以继续修改config.toml文件，可以从浏览器上实时看到修改后的效果

8. 复制`content/index.md`文件到根目录下，作为README.md文件

    ```bash
    cp content/index.md README.md
    ```

    然后删除文件头部的hugo元数据。

9. 提交第一个初始版本到github

    ```bash
    git init
    # 检查一下要提交的文件是否如预期
    git status
    git add -A
    git commit -m "first version"
    git remote add origin git@github.com:skyao/learning-cilium.git
    git push -u origin master
    ```
10. 和gitbook共存
	
	由于原来的读书笔记是gitbook格式，因此为了让原来gitbook那边的老用户有机会找回到新的基于hugo的笔记，需要在gitbook那边好歹留一个简单页面说明。

	为此，保留gitbook的最少文件，如`book.json`和`README.md`即可。是的，SUMMARY.md文件都是可以不用的，而且README.md文件本来也可以用来作为在github仓库的首页文档，因此实质上只是多了一个`book.json`文件。

	内容示意如下：
	
	```json
	{
	  "gitbook": ">=3.2.0",
	  "description": "Hugo学习笔记",
	  "language": "zh-hans",
	  "author": "敖小剑",
	  "extension": null,
	  "generator": "site",
	  "links" : {
	    "sidebar" : {
	        "敖小剑的博客": "https://skyao.io"
	    }
	  },
	  "structure" : {
	    },
	  "plugins": [
	    "-search",
	    "-sharing"
	  ],
	  "pluginsConfig": {
	  },
	  "title": "Hugo学习笔记",
	  "variables": {
	  }
	}
	```

11. 最后，准备好`.gitignore`文件

	```bash
	.*
	!.gitignore
	_book/
	node_modules/
	public/
	themes/
	update_theme.sh
	```

## 待解决的问题

存在以下问题：

1. 主题已经不再更新
	
    作者已经消失，一年多没有更新，包括PR也没有人合并。
	
2. 在移动端，底部上一页显示有问题
	
    只显示箭头，不显示文字内容，但是链接是存在的，点击可以去往上一个。

    测试过多个浏览器，包括chrome，搜狗浏览器，UC浏览器，都是如此。

    在PC上一般没有问题，但是如果将窗口的宽度缩小到一定程度，也会出现这个现象。猜测是为了避免上一页和下一页的文字同时现实会放不下，所以估计隐藏了上一页的文字只保留箭头。如果是这样，那么就是一种特别精妙的设计而不是缺陷了。
    
希望后面有人继续维护这个主题。