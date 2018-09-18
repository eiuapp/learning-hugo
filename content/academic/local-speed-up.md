---
date: 2018-09-10T21:07:13+08:00
title: 本地加速
weight: 31
menu:
  main:
    parent: "academic"
---

使用中，发现网页转载速度不是很好，而且有时无法访问。

## 问题所在

发现问题出现在页面上的一些静态文件下载，使用的路径居然是`https://cdnjs.cloudflare.com/××`。

```html
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha512-6MXa8B6uaO18Hid6blRMetEIoPqHf7Ux1tnyIQdpt9qI5OACx7C+O3IVTr98vwGnlcg0LOLa02i9Y1HpVhlfiw==" crossorigin="anonymous">
```

cloudflare网站是在国外，访问速度慢，而且非常不稳定：经常被墙。导致页面加载的速度慢，有时还会出现无法打开页面的问题。

检查后发现，定义这些css/js等文件路径的地方是`themes/academic/layouts/partials/`下的`header.html`和`footer.html`，属于主题仓库。而这个仓库目前是以git的submodule出现。

## 解决方式

### Fork主题

为了方便日后升级，我们fork academic，修改修改的文件有`themes/academic/layouts/partials/`下的`header.html`和`footer.html`。

类似的，将下面的原有内容：

```xml
  <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/{{- $sri.css.bootstrap.version -}}/css/bootstrap.min.css">
```

修改href中的地址为本地地址：

```xml
  <link rel="stylesheet" href="/local/ajax/libs/twitter-bootstrap/{{- $sri.css.bootstrap.version -}}/css/bootstrap.min.css">
```

在`themes/academic/static`目录下建立`local`目录，然后将前面涉及到的所有js/css文件都保存好。

### 更新academic插件

默认是采用git的submodule来载入academic 的代码，但是实际使用中发现submodule非常的不方便，因此放弃submodule，采用直接复制文件的方式。

初始方法：

```bash
cd skyao
git clone git@github.com:skyao/hugo-academic.git
git clone git@github.com:skyao/skyao.io.git
cd skyao.io
sh update_academic.sh
hugo server
```

## 更新academic版本

当academic版本更新时，需要跟着更新。

### 更新fork的仓库

```bash
git remote add upstream git://github.com/gcushen/hugo-academic.git
git fetch upstream
git merge upstream/master
```

### 测试新版本

将`themes/academic`目录下的内容删除，然后复制hugo-academic下的所有内容到`themes/academic`目录下。

然后边修改边确认，等待修改完成测试通过，再将修改好的内容同步回fork的hugo-academic git仓库。

