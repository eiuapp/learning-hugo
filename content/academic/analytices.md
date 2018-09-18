---
date: 2018-09-10T21:07:13+08:00
title: 百度统计
weight: 302
menu:
  main:
    parent: "academic"
---

由于google统计容易被墙，不适合国情，只好放弃。

替代品选择的是百度统计。

## 修改模板

简单起见，修改`hugo-academic`项目下的`layouts/partials/header.html`文件，将原有google统计的内容替代为百度统计，但是继续使用原来的变量GoogleAnalytics：

```bash
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
```

## 修改配置

修改`config.toml`,设置googleAnalytics为百度统计的id：

```bash
# Enable analytics by entering your Google Analytics tracking ID
googleAnalytics = "17048c9d4209e5d08a9ae5b0b4160808"
```
