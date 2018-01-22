# 本地加速

使用中，发现网页转载速度不是很好，而且有时无法访问。

## 问题所在

发现问题出现在页面上的一些静态文件下载，使用的路径居然是`https://cdnjs.cloudflare.com/××`。

```xml
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha512-6MXa8B6uaO18Hid6blRMetEIoPqHf7Ux1tnyIQdpt9qI5OACx7C+O3IVTr98vwGnlcg0LOLa02i9Y1HpVhlfiw==" crossorigin="anonymous">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/academicons/1.8.1/css/academicons.min.css" integrity="sha512-NThgw3XKQ1absAahW6to7Ey42uycrVvfNfyjqcFNgCmOCQ5AR4AO0SiXrN+8ZtYeappp56lk1WtvjVmEa+VR6A==" crossorigin="anonymous">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css" integrity="sha512-SfTiTlX6kk+qitfevl/7LibUOeJWlt9rbyDn92a1DqWOw9vWG2MFoays0sgObmWazO5BQPiFucnnEAjpAB+/Sw==" crossorigin="anonymous">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.2.5/jquery.fancybox.min.css" integrity="sha256-ygkqlh3CYSUri3LhQxzdcm0n1EQvH2Y+U5S2idbLtxs=" crossorigin="anonymous">
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

### 更新submodule

`theme/academic`是git的submodule，默认是指向

https://github.com/gcushen/hugo-academic

我们需要修改为指向到我们fork并修改了的仓库。

没有找到git中更新submodule地址的命令，只能用笨办法，先删除再添加。

1. 删除现有submodule

	```bash
    git rm --cached themes/academic
	rm -rf themes/academic
	rm .gitmodules
    ```

	然后修改`.git/config`文件，删除对应的submodule信息：

	```bash
	[submodule "themes/academic"]
        active = true
        url = git://github.com/skyao/hugo-academic.git
	```

	然后commit/push到远程git仓库。

2. 删除本地仓库，重新clone

3. 添加新的submodule

    将我们fork的theme仓库添加进来：

    ```bash
    git submodule add git://github.com/skyao/hugo-academic.git themes/academic
    ```

    然后执行：

    - `git submodule init`： 来初始化本地配置文件，

    - `git submodule update`： 从某个项目拉取所有数据并检出你上层项目里所列的合适的提交：

4. 重新构建hugo

	用新的主题重新构建hugo。

## 更新jenkins build

登录jenkins所在机器，进入`/var/lib/jenkins/workspace/`，直接删除对应job的目录。

然后在脚本中，在build之前，增加一行`git submodule update --init --recursive`来初始化submodule。（备注：build成功之后，以后可以不用再次初始化，可以注释掉这行）


