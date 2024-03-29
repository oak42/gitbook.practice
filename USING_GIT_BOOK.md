# Using GitBook

# gitbook.practice.provisioning

# typora  Markdown Reference(https://support.typora.io/Markdown-Reference/)

# Typora无限试用

## Typora好用，但是收费了，于是乎参考[吾爱破解](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.52pojie.cn%2Fthread-1553967-1-1.html)分享给大家

![img](README.assets/webp-165380436478263.webp)

image.png



![img](README.assets/webp-165380436478264.webp)

image.png

打开注册表，输入**计算机\HKEY_CURRENT_USER\Software\Typora**
随意修改如下图

![img](README.assets/webp-165380436478265.webp)

image.png



然后右键**Typora**

![img](README.assets/webp-165380436478266.webp)

image.png


点**权限**，找到当前登录人和管理员，执行下图操作

![img](README.assets/webp-165380436478267.webp)

image.png



这样操作以后的效果如图就算成了

![img](README.assets/webp-165380436478268.webp)

image.png


再次打开还是 **您的试用还有** 15 **天到期**



作者：Ruining101
链接：https://www.jianshu.com/p/bb0ddf5fbd5d
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





==========================================================================================



# Gitbook教程（小白入坑gitbook全过程）



\# 前言

最近想找一个记笔记的软件，试了几款都觉得差点意思，后来看到别人用gitbook写的笔记就想试试，接触以后发现很多大佬讲的其实不适合我这种啥都不懂的人，于是我就想自己写一遍文章，再加上一直想写篇博客玩，于是就有了这篇，我的第一篇博客。

[TOC]

# Gitbook的安装

## node.js的安装

gitbook是一个基于Node.js的命令行工具，所以要先安装Node.js(下载地址：[https://nodejs.org/en/](https://links.jianshu.com/go?to=https%3A%2F%2Fnodejs.org%2Fen%2F)，找到对应平台的版本安装即可)。

Node.js都会默认安装npm（node包管理工具），所以不需要单独安装npm，打开命令行，执行以下命令安装GitBook:



```undefined
npm install -g gitbook-cli
```

如果提示npm不是内部会外部命令，可以cd到node.js的安装目录在执行或将npm的目录添加到系统变量path中。



## 编辑工具的安装

这里给大家介绍两个编辑工具：gitbook editer和typora

### gitbook editor

这个编辑工具对新手来说是个不错的选择，它集成了gitbook，git，Markdown等功能，可以将书籍同步到gitbook.com网站和github。但是gitbook editor的注册和登录需要翻墙，而且这东西的官网进不去，所以还下不到。

这里给大家分享一下这个工具，会翻墙的可以尝试一下这个工具

[https://pan.baidu.com/s/1LEf2b9QwIRNgsoXJVBV90g](https://links.jianshu.com/go?to=https%3A%2F%2Fpan.baidu.com%2Fs%2F1LEf2b9QwIRNgsoXJVBV90g)

提取码：vtpv

### typora

这个工具是我目前在使用的，推荐这个也没有啥特别的原因，就是我自己感觉挺好用的

下载地址:[https://www.typora.io/](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.typora.io%2F)

直接进官网下载对应平台的版本就可以了

# Gitbook的使用

## 创建笔记文件夹

在你想要的位置新建一个文件夹，然后打开命令行，cd到这个文件夹下。

接着执行以下命令



```kotlin
gitbook init
```

执行完后，文件夹里会多两个文件



![img](README.assets/webp-165380430158045.webp)

在这里插入图片描述

README.md（书籍的介绍在这个文件里）

SUMMARY.md（书籍的目录结构在这里配置）

## 写目录

后面我介绍了一个生成目录的插件，所以不一定要自己弄目录，但这里还是说下怎么写

上面说到SUMMRAY.md文件就是整个文件夹的目录，写目录其实就是编辑这个文件，刚打开时这个文件里什么都没有，我先在给他编写一下:



![img](README.assets/webp-165380430158046.webp)

在这里插入图片描述

在给大家看看源代码



```bash
# Summary

* [Introduction](README.md)
* [前言](readme.md)
* [第一章](part1/README.md)
    * [第一节](part1/1.md)
    * [第二节](part1/2.md)
    * [第三节](part1/3.md)
    * [第四节](part1/4.md)
* [第二章](part2/README.md)
* [第三章](part3/README.md)
* [第四章](part4/README.md)
```

大家可以复制源代码过去试一下

简单给大家说一下语法：中括号里是这个目录的名字，小括号里是路径。

写完目录后再次执行`gitbook init` Gitbook会查找SUMMARY.md中描述的目录和文件，如果没有则会创建。上面的目录运行后是这样的

![img](README.assets/webp-165380430158047.webp)

在这里插入图片描述



## 写笔记

在这里给大家介绍一些typora比较常用的功能

### 各级标题

在typora中使用crtl + 1~6可以创建1-6级标题，1级最大，6级最小。同时typora的大纲会显示所有标题，如图：



![img](README.assets/webp-165380430158048.webp)

在这里插入图片描述

### 表格

可以在段落--->表格中插入表格，或者按快捷键crtl+T

### 代码块

这个功能用的很频繁，可以在段落--->代码块中插入，也可以使用快捷键(```+空格)

### 专注模式和打字机模式

打字机模式就是让当前输入行保持在屏幕中间，专注模式就是输入行正常颜色，其他行变灰，可以在视图中打开

### 源代码模式

源代码模式就是展示Markdown的文件本来的样子，如图



这个模式会展示出Markdown文件的标识符

### 插入图片

这个不多说，在格式--->图像里，快捷键crtl+shift+i

### 插入链接

用两个尖括号括起来就行，如下：

<[www.baidu.com](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.baidu.com)>



### 更换主题

直接点主题就好，typora内置了6个主题，值得一提的是typora的主题是css格式，所以会css的小伙伴可以自己写主题，或者调整已有的主题

### 内容目录

就是你们在本文上面看到的那个目录，简单点说就是把大纲放到了文章里，在段落--->内容目录里可以找到（有标题时才能用）

### 常见的格式

就比如加粗，斜体，下划线什么的。在格式里有，快捷键如下：



### 我的使用方法

其实单个md文件可以存储很多笔记，我现在是把一块笔记存在一个md文件里，有一定规模后再装文件夹，然后放在gitbook中，再使用我介绍的插件生成目录。

## 插件的使用方法

在gitbook中有很多有用的插件，但是使用起来会有一点麻烦，这里给大家讲解一下怎么用。

gitbook的插件大部分都在npm上，可以访问npm官网查看搜索插件[https://www.npmjs.com/](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.npmjs.com%2F)

这里用一个自动生成目录的插件给大家介绍使用方法

名字叫:[gitbook-plugin-summary](https://links.jianshu.com/go?to=https%3A%2F%2Fplugins.gitbook.com%2Fplugin%2Fgensum)

1. 首先打开你笔记文件夹下的book.json文件，没有就自己创建一个

2. 将以下代码复制进去



   ```json
   {
     "plugins": ["summary"]
   }
   ```

3. 打开命令行，在这个文件夹中执行命令`gitbook install`安装插件

4. 执行命令`gitbook serve`然后在查看的时候就会发现，之前明明没有写目录，现在却有了目录

大家想要用别的插件可以直接去npm上找，都有使用方法的

## 生成电子书

写完后我们可以执行`gitbook serve`来预览这本书，执行后会把Markdown格式的文档转换为html格式，最后提示"Serving book on [http://localhost:4000](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost%3A4000)"此时用浏览器打开" [http://localhost:4000](https://links.jianshu.com/go?to=http%3A%2F%2Flocalhost%3A4000)"即可预览书本

如图：



![img](README.assets/webp-165380430158049.webp)

在这里插入图片描述

执行完后你的笔记文件夹里会多一个_book文件夹，里面是转化后的html文件

# 笔记的同步

## 同步单个的md文件

对于单个的md文件，我建议同步到石墨文档，可以在网页版，桌面版，移动版三个版本上查看和编辑。操作方法就是打开然后带入就行了。当然，在博客上发布也不失为一种好的同步方法。

## 同步整个笔记文件

可以使用git同步到github，由于我对git不太熟悉，就不误导大家了，大家可以去网上找找

最后，这是我第一次写博客，而且我从接触gitbook到写完这篇博客只有短短三天，如果有哪里不对，请各位指正



作者：Broken故城
链接：https://www.jianshu.com/p/0388d8bb49a7
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





==========================================================================================

# 教你如何使用github+jsDelivr搭建免费图床



## 前言

这里我用的是七牛图床，七牛图床有一定的免费使用量（没记错的话应该是10个G），如果你的存储量超过这个大小就需要付费使用了。除此之外，还需要维护一个备案过的域名，绑定一台云服务器。这些都需要一定的费用。

因此，对于白嫖党来说非常不友好。

今天，我就教大家用 “全球最大同性交友网站” github 并搭配 jsDelivr 开源 CDN 来搭建一个免费图床。全程不需要任何费用哦，白嫖党欢呼吧~

## 正文

本文内容包括：

- 创建一个 github 仓库
- 使用 jsDelivr 免费 CDN 加速图片访问速度
- 创建 Token
- 使用 PicGo 配置 github 图床

### 创建 github 仓库

这里就跳过怎么注册 github 账号的步骤了，做技术的都晓得。

1、登录你的 github 账号，创建一个新的仓库。

![img](README.assets/L3Byb3h5L2h0dHBzL3M1LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NDhfNjE4YjhkODhiMWQwNjQzMTgxLnBuZw==.jpg)

2、然后填写仓库的资料，主要是仓库名，其他一般默认。

![img](README.assets/L3Byb3h5L2h0dHBzL3M0LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NDhfNjE4YjhkODhlYTk5NTQ4NjEwLnBuZw==.jpg)

3、点击 create repository 后，跳到这个页面，就说明创建成功了。

![img](README.assets/L3Byb3h5L2h0dHBzL3M4LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NDlfNjE4YjhkODkyMTg5NTI3NTUxLnBuZw==.jpg)

然后可以上传一张图片试一下。不过，有可能你会遇到在 github 上看到的图片是裂开的情况。

只需要在电脑的 hosts 文件中添加以下代码即可。 windows 下的 hosts文件 目录在 `C:\Windows\System32\drivers\etc` 。（注意要以管理员权限打开） mac 下为 `/etc/hosts`。

```
# GitHub Start52.74.223.119 github.com192.30.253.119 gist.github.com54.169.195.247 api.github.com185.199.111.153 assets-cdn.github.com151.101.76.133 raw.githubusercontent.com151.101.108.133 user-images.githubusercontent.com151.101.76.133 gist.githubusercontent.com151.101.76.133 cloud.githubusercontent.com151.101.76.133 camo.githubusercontent.com151.101.76.133 avatars0.githubusercontent.com151.101.76.133 avatars1.githubusercontent.com151.101.76.133 avatars2.githubusercontent.com151.101.76.133 avatars3.githubusercontent.com151.101.76.133 avatars4.githubusercontent.com151.101.76.133 avatars5.githubusercontent.com151.101.76.133 avatars6.githubusercontent.com151.101.76.133 avatars7.githubusercontent.com151.101.76.133 avatars8.githubusercontent.com
```



然后回到你的图片仓库，刷新一下页面即可正常显示图片。

### 使用 jsDelivr 免费加速

其实，此时已经可以正常访问你仓库中的图片了。我这里以我创建好的仓库 myImages 为例。

![img](README.assets/L3Byb3h5L2h0dHBzL3MzLjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NDlfNjE4YjhkODk1NDc3MTM5MzQ2LnBuZw==.jpg)

要想访问仓库中的这个 test.png 图片，需要把链接地址中的 blob 改为 raw。即 `https://github.com/starry-skys/myImages/raw/main/test.png` 。或者在地址后拼接一段 `?raw=true`，即 `https://github.com/starry-skys/myImages/blob/main/test.png?raw=true` 。

但是，我们发现，通过 github 直接访问图片，速度不是特别理想，毕竟服务器在国外。

因此，我们可以使用 jsDelivr 进行 CDN 加速。这是完全开源免费的。

使用方法，非常简单，即把图片地址链接域名改为 CDN 的域名。格式如下：

```
https://cdn.jsdelivr.net/gh/<你的github用户名>/<你的图床仓库名>@<仓库版本号>/图片的路径
```

还是以上边的 test.png 图片为例，仓库版本号直接用分支名，由于现在 github 主分支名字都叫 main 了，因此版本号写 main 。图片路径，是在仓库中的相对路径，因为我这里就在根目录，因此就是 test.png 。

最终地址为 `https://cdn.jsdelivr.net/gh/starry-skys/myImages@main/test.png`。

其他说明，可参考 jsDelivr 官网介绍，[jsDelivr 官网](https://www.jsdelivr.com/?docs=gh)

### 配置 typora 自动上传到 github 图床

接下来，如果需要在 typora 中设置自动上传到 gtihub 图床，还需要做一些配置。

**一、首先，在 github 上创建一个 token。**

1、点击右上角账号上的 settings

![img](README.assets/L3Byb3h5L2h0dHBzL3M3LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NDlfNjE4YjhkODk3Mzg2ZTM0MjY1LnBuZw==.jpg)

2、然后左侧点击 developer settings ，再点击 personal access tokens ，然后点击 generate new token。

![img](README.assets/L3Byb3h5L2h0dHBzL3M2LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NDlfNjE4YjhkODlhNjk4OTY4MTgzLnBuZw==.jpg)

3、Note 用来说明你创建 token 的用途，然后 scopes 只需要选 repo 的所有选项即可。

![img](README.assets/L3Byb3h5L2h0dHBzL3M4LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NDlfNjE4YjhkODllZGY3NTY3OTY1LnBuZw==.jpg)

4、最后拉到底部，点击 generate token ，即可成功。

![img](README.assets/L3Byb3h5L2h0dHBzL3M1LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NTBfNjE4YjhkOGExOThiZDkzMjgucG5n.jpg)

5、找个地方记下这一串 token，等会需要用到。（如果没有记住，等再查看时就只能重新生成了）

![img](README.assets/L3Byb3h5L2h0dHBzL3M2LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NTBfNjE4YjhkOGE0NDAyZDUzMzI5LnBuZw==.jpg)

**二、打开 PicGo 配置 github 图床**

在 PicGo 中，找到图床设置 -> GitHub图床。

- 仓库名即为你的github账号/图片仓库名
- 分支名就用默认的 main
- Token 就填写刚才我们生成的 Token
- 存储路径如果需要指定子目录可以填写例如 img/ 。我这里没有填，就会上传到我图片仓库的根目录。
- 自定义域名就填写 jsDelivr 的域名，即图片访问地址，不包括图片路径的前半部分，我这里就是 `https://cdn.jsdelivr.net/gh/starry-skys/myImages@main`。
- 最后设为默认图床，下次在 typora 上传图片就会自动上传到 github 图床了。

![img](README.assets/L3Byb3h5L2h0dHBzL3M0LjUxY3RvLmNvbS9pbWFnZXMvYmxvZy8yMDIxMTEvMTAxNzE0NTBfNjE4YjhkOGE3YjkzNjEzODMucG5n.jpg)

至此，所有步骤就已经完成了，赶紧去尝试一下吧。

https://www.365seal.com/y/0NpOgo6jpz.html

https://blog.csdn.net/weixin_42875245/article/details/108554926    （验证图片上传选项） typora使用端口36677

==========================================================================================



# gitbook官网文档

https://docs.gitbook.com/integrations/git-sync/content-configuration

gitbook config ( {% em type="green" %}book.json{% endem %} (powered by the 'emphasize' plugin)] doc:
https://github.com/GitbookIO/gitbook/blob/master/docs/config.md