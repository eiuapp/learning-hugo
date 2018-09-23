---
date: 2018-09-09T21:07:13+01:00
title: academic主题
weight: 300
keywords:
- theme
- academic
- academic主题
description : "介绍hugo的academic主题，使用方式和订制化"
---

## 主题介绍

academic是一个特别适合搭建内容相对比较丰富的网站的主题，如果我们hugo网站的内容不仅仅是博客，还有其他好几种样式的内容，那么academic会是一个很不错的选择。此外，academic主题简洁大方，也适合作为一个稍有规模的正式网站。

- 官网介绍：https://themes.gohugo.io/academic/
- 我用academic主题搭建的个人技术博客网站: https://skyao.io

## 准备工作

### git仓库准备

以建立skyao.io这个网站为例，fork 两个github项目：

1. https://github.com/gcushen/hugo-academic: 修改仓库名为hugo-academic-cn，这是自行订制的主题仓库，加cn后缀名以示区别。
2. https://github.com/sourcethemes/academic-kickstart: 修改仓库名为skyao.io，这是存放站点内容的仓库，为了方便起见，从官方的kickstart仓库开始改起，也方便未来保持更新。

### 不使用git的submodule

clone下来skyao.io的仓库到本地：

```bash
git clone git@github.com:skyao/skyao.io.git
```

在`skyao.io`目录下，删除`themes`

```bash
cd skyao.io.git
rm -rf themes/
```

修改`.gitignore`文件内容如下：

```bash
.*
!.gitignore
public/
themes/
content/post/images/
content/publication/images/
```

修改`update_academic.sh`文件内容如下：

```bash
#!/bin/bash

if [ ! -d "themes" ];then
  echo "No themes directory, create it"
  mkdir themes
fi

if [ -d "themes/academic" ];then
  echo 'Find directoy "themes/academic", update by "git pull"'
  cd themes/academic
  git pull
  cd ../../
else
  echo 'Directoy "themes/academic" not found, do "git clone"'
  git clone https://github.com/skyao/hugo-academic-cn.git themes/academic
  #git clone git@github.com:skyao/hugo-academic-cn.git themes/academic
fi
```

执行命令 `sh update_academic.sh` 获取 academic 主题文件。此时`themes/academic`是我们订制的主题内容，此时两个git仓库都可以分别提交内容，非常方便修改。

## 创建网站

###### 使用 exampleSite 初始化

删除｀skyao.io｀目录下的config.toml文件和content/static目录，然后从｀themes/academic/exampleSite｀下的这三个文件和目录复制到｀skyao.io｀目录下。执行`hugo server`命令，就可以通过浏览器访问 http://localhost:1313/ 看到示例站点。

我们从这个基础开始进行修改和订制。

### 修改网站的基本信息

修改config.toml文件

```toml
title = "敖小剑的博客"
copyright = "skyao.io &copy; 2018"
paginate = 50 # 默认10太小
googleAnalytics = "17048c9d4209e5d08a9ae5b0b4160808" # 开始百度统计，注意不是google统计，后面会详细讲
hasCJKLanguage = true

name = "敖小剑"
role = "资深码农"
organizations = [ { name = "蚂蚁金服", url = "https://www.antfin.com/" }, { name="中间件团队", url="" } ]
avatar = "portrait.jpg"  # 内容不用改，需要找一张400*400的图片，覆盖掉`static/img/portrait.jpg`文件
email = "aoxiaojian@hotmail.com"
address = "广州市天河区广电平云广场B塔18楼"
office_hours = ""
phone = ""
skype = ""
telegram = ""

  map = 0 # 最后还是决定关闭地图现实，太影响页面打开速度
  map_api_key = "AIza××" # 需要在google cloud platform申请
  latitude = "23.1186075" # 在google地图上找到目的地址，浏览器地址栏上会显示详细的坐标数值
  longitude = "113.3510883"
  zoom = 14
  
  color_theme = "1950s"
  font = "default"
  
  
  twitter = "SkyAoXiaojian"
  date_format = "2006-01-02"
  time_format = "15:04"
  reading_time = false # 阅读时间预测，不准，不开启
  highlight_languages = ["java", "bash","go", "xml", "json","yaml"] # 具体支持哪些语言见 https://cdnjs.com/libraries/highlight.js/
  
    publication_types = [
    '未分组',  # 0
    '会议记录',  # 1
    '期刊文章',  # 2
    '手稿',  # 3
    '技术报告',  # 4
    '书籍',  # 5
    '书籍摘抄'  # 6
  ]
  
  [params.publications]
  date_format = "2006-01-02"
  
  ## 图标的代码需要在 https://fontawesome.com 网站上查找
  [[params.social]]
    icon = "envelope"
    icon_pack = "fas"
    link = "mailto:aoxiaojian@hotmail.com"

  [[params.social]]
    icon = "twitter"
    icon_pack = "fab"
    link = "//twitter.com/SkyAoXiaojian"

  [[params.social]]
    icon = "github"
    icon_pack = "fab"
    link = "//github.com/skyao"

  [[params.social]]
    icon = "git"
    icon_pack = "fab"
    link = "//legacy.gitbook.com/@skyao"

  [[params.social]]
    icon = "weibo"
    icon_pack = "fab"
    link = "//weibo.com/aoxiaojian"

  [[params.social]]
    icon = "youtube"
    icon_pack = "fab"
    link = "//www.youtube.com/channel/UCKeIYzzIeOtVR1iSfAdRBqQ"



[[menu.main]]
  name = "首页"
  url = "#about"
  weight = 1

[[menu.main]]
  name = "演讲分享"
  url = "#publications_selected"
  weight = 2

[[menu.main]]
  name = "技术博客"
  url = "#posts"
  weight = 3

[[menu.main]]
  name = "开源项目"
  url = "#projects"
  weight = 4

#[[menu.main]] ## 教程这块要研究一下具体怎么用
#  name = "Tutorials"
#  url = "/tutorial/"
#  weight = 5

[[menu.main]]
  name = "和我联系"
  url = "#contact"
  weight = 6
  
  
```

### 修改首页内容

`skyao.io/content/home` 目录下，修改 about.md ：

```toml
[interests]
  interests = [
    "微服务/Micro Service",
    "服务网格/Service Mesh",
    "无服务器架构/Serverless",
    "云原生/Cloud Native",
    "敏捷/DevOps"
  ]

[[education.courses]] # 删除

# 删除原有的内容，直接覆盖
# 简介 （这里也是可以改的）

增加个人简介
```

修改 contact.md:

```tom
active = true	# 不想显示的话可以在这里关闭
title = "和我联系"
subtitle = ""
```

修改 hero.md:

```tom
title = "SOFAMesh"
overlay_img = "projects/sofamesh-wide.jpg" # 记得把图片放到static/img/下

# 其他内容酌情修改
```

hero_carousel.md 是另一个版本的hero，横屏，可以多个话题滑动，效果更好的感觉。后面再整理。

修改 posts.md:

```tom
title = "技术博客"
count = 10
```

修改 projects.md

```to
title = "开源项目"
subtitle = ""

# filter根据实际情况调整
[[filter]]
  name = "All"
  tag = "*"

[[filter]]
  name = "Service Mesh"
  tag = "servicemesh"

[[filter]]
  name = "Serverless"
  tag = "serverless"
```

修改 publications.md

```to
title = "最新动态"
subtitle = "演讲/文章/分享"
count = 5
```

修改 publications_selected.md 

```tom
title = "特别推荐"
```

修改 search.md:

```tom
title = "站内搜索"
```

修改 skills.md

```tom
# 关闭，中国人还是习惯性谦虚一些好，详细列技能感觉怪怪
active = false
```

修改 tags.md

```tom
title = "标签"
```

修改 talks.md

```tom
title = "演讲计划"
subtitle = "技术大会日程安排"
```

修改talks_selected.md:

```tom
title = "下一站"
subtitle = "近期活动，欢迎关注"
```

修改teaching.md:

```tom
# 暂时关闭，没有需要
active = false
```

### 修改post

修改 content/posts/_index.md

```to
title = "技术博客"

# 在页面 http://localhost:1313/post/ 显示一张图片
[header]
image = ""
caption = ""
```

### 修改project

project没有要修改的内容。 

### 修改publications

修改 content/publication/_index.md

```tom
title = "最近发表"
[header]
image = ""
caption = ""
```

### 增加 learning

新版本提供了 content/tutorials 的实例，不过没有挂在首页，不容易发现。

官方说明是适合用于：

- 项目或者软件文档
- 在线课程
- 教程

我这次用它来记录我的学习笔记，因为数量繁多不好整理，一直都没有放在个人网站上。

具体修改内容如下：

1. 重命名 `content/tutorials` 为 `content/learning`，通过 http://localhost:1313/learning/ 可以访问

2. 由于目录被改名，所以左边的菜单报错，修改修改`example.md`中

	```toml
	[menu.learning]	# 将menu.tutorial修改为menu.learning
	  parent = "Example Topic"
	  weight = 1
	```

	修改`_index.md`:

	```toml
	[menu.learning] # 将menu.tutorial修改为menu.learning
	  name = "Overview"
	  weight = 1
	```

	菜单恢复正常

3. 在顶部的导航条中增加链接

	修改`config.toml`，增加菜单：

	```toml
	[[menu.main]]
	  name = "学习笔记"
	  url = "/learning/"
	  weight = 5
	```

4. 修改`themes/academic/layouts/partials/docs_layout.html`，注释掉一下内容：

	```html
	<!--
	<div class="body-footer">
	{{ i18n "last_updated" }} {{ $.Lastmod.Format $.Site.Params.date_format }}
	</div>
	-->
	```

	不直接显示最后修改时间在页面上，这样界面清爽一些。

### slides功能

在content/slides下发现一个 example-slides.md 文件，用浏览器打开下面的地址：

http://localhost:1313/slides/example-slides/

发现原来是一个用markdown书写slides的功能，很有意思，稍后研究。

## 优化

### 关闭google地图

首页的google地图现实，太影响页面打开速度。最后选择关闭。修改config.toml:

```toml
  map = 0 # 最后还是决定关闭地图现实，太影响页面打开速度
```

### 去掉github项目的star显示

在hero页面上，默认设置会去取github项目的star数量，同样非常的拖累整体速度。还是去掉吧，修改文件`content/home`，删除以下内容：

```html
  <a id="academic-release" href="https://github.com/alipay/sofa-mesh/releases" data-repo="alipay/sofa-mesh">
  Latest release <!-- V -->
  </a>
<div class="mt-3">
  Star
</div>
<script async defer src="/local/buttons.github.io/buttons.js"></script>
```

### 改google analytics为百度统计

修改 `academic/layouts/partials/header.html`，将下面原来用于google analytics的内容：

```go
  {{ if not .Site.IsServer }}
  {{ if .Site.GoogleAnalytics }}
    <script>
      window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
      ga('create', '{{ .Site.GoogleAnalytics }}', 'auto');
      {{ if .Site.Params.privacy_pack }}ga('set', 'anonymizeIp', true);{{ end }}
      ga('require', 'eventTracker');
      ga('require', 'outboundLinkTracker');
      ga('require', 'urlChangeTracker');
      ga('send', 'pageview');
    </script>
    <script async src="//www.google-analytics.com/analytics.js"></script>
    {{ if ($scr.Get "use_cdn") }}
    {{ printf "<script async src=\"%s\" integrity=\"%s\" crossorigin=\"anonymous\"></script>" (printf $js.autotrack.url $js.autotrack.version) $js.autotrack.sri | safeHTML }}
    {{ end }}
  {{ end }}
  {{ end }}
```

修改为百度统计的内容：

```toml
  {{ if not .Site.IsServer }}
  {{ if .Site.GoogleAnalytics }}
	<script>
	var _hmt = _hmt || [];
	(function() {
	  var hm = document.createElement("script");
	  hm.src = "https://hm.baidu.com/hm.js?{{ .Site.GoogleAnalytics }}";
	  var s = document.getElementsByTagName("script")[0];
	  s.parentNode.insertBefore(hm, s);
	})();
	</script>
  {{ end }}
  {{ end }}
```

### 本地文件加速

使用中，发现网页装载速度不是很好，而且有时无法访问。检查后发现问题出现在页面上的一些静态文件下载上，使用的路径居然是`https://cdnjs.cloudflare.com/××`。

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha512-6MXa8B6uaO18Hid6blRMetEIoPqHf7Ux1tnyIQdpt9qI5OACx7C+O3IVTr98vwGnlcg0LOLa02i9Y1HpVhlfiw==" crossorigin="anonymous">
```

cloudflare网站是在国外，访问速度慢，而且非常不稳定：经常被墙。导致页面加载的速度慢，有时还会出现无法打开页面的问题（被墙）。解决的方式就是将这些文件放在网站本地，从而避开这个问题。

具体做法：

1. 修改 `themes/academic/data/assets.toml`，将类似`https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js` 的地址修改为 `/local/cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js`
2. 修改 `fonts.googleapis.com/css.css` 文件，用 `/local/fonts` 替换 `https://fonts` ，将原来访问`https://fonts.gstatic.com`的地址指到本地。
3. 修改 `themes/academic/` 下的文件，将类似 `//cdnjs.cloudflare.com` 的地址替换为 `/local/cdnjs.cloudflare.com`
	- layouts/partials/header.html
	- layouts/partials/footer.html
	- layouts/partials/cookie_consent.html
	- layouts/slides/baseof.html
4. 然后将上述这些地址所指向的文件存放在`static/img/local`目录下，最大的工作量在这里。

最终的目标是实现下面第一个地址修改为第二个地址。

- https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js
- https://127.0.0.1:1313/local/cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js

小技巧：

1. 用chrome浏览器的开发工具，看network，就知道有哪些文件是走remote访问了
2. `find -type f -exec grep -H "//cdn" {} \;` 这样的命令可以方便的查找存在问题的文件

## 待解决的问题

1. 站内搜索只能搜索英文，使用中文搜索会无法找到任何信息。