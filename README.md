# gitbook.practice



## Typora好用，但是收费了，于是乎参考[吾爱破解](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.52pojie.cn%2Fthread-1553967-1-1.html)分享给大家

![img](https:////upload-images.jianshu.io/upload_images/2730138-7057348b41d9a16c.png?imageMogr2/auto-orient/strip|imageView2/2/w/537/format/webp)

image.png



![img](https:////upload-images.jianshu.io/upload_images/2730138-21d92b3142d847ea.png?imageMogr2/auto-orient/strip|imageView2/2/w/525/format/webp)

image.png

打开注册表，输入**计算机\HKEY_CURRENT_USER\Software\Typora**
 随意修改如下图

![img](https:////upload-images.jianshu.io/upload_images/2730138-efbc98aed03613c1.png?imageMogr2/auto-orient/strip|imageView2/2/w/553/format/webp)

image.png



然后右键**Typora**

![img](https:////upload-images.jianshu.io/upload_images/2730138-c0f32c0042d861c0.png?imageMogr2/auto-orient/strip|imageView2/2/w/172/format/webp)

image.png


 点**权限**，找到当前登录人和管理员，执行下图操作

![img](https:////upload-images.jianshu.io/upload_images/2730138-290ba567afe6829c.png?imageMogr2/auto-orient/strip|imageView2/2/w/411/format/webp)

image.png



这样操作以后的效果如图就算成了

![img](https:////upload-images.jianshu.io/upload_images/2730138-27c9e19df45bfc96.png?imageMogr2/auto-orient/strip|imageView2/2/w/1004/format/webp)

image.png


 再次打开还是    **您的试用还有** 15 **天到期**



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



![img](https:////upload-images.jianshu.io/upload_images/20760577-441ec8985e5116ab?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

在这里插入图片描述

README.md（书籍的介绍在这个文件里）

SUMMARY.md（书籍的目录结构在这里配置）

## 写目录

后面我介绍了一个生成目录的插件，所以不一定要自己弄目录，但这里还是说下怎么写

上面说到SUMMRAY.md文件就是整个文件夹的目录，写目录其实就是编辑这个文件，刚打开时这个文件里什么都没有，我先在给他编写一下:



![img](https:////upload-images.jianshu.io/upload_images/20760577-3ddc5c272c9e9671?imageMogr2/auto-orient/strip|imageView2/2/w/1099/format/webp)

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

![img](https:////upload-images.jianshu.io/upload_images/20760577-f47d7aa347831f03?imageMogr2/auto-orient/strip|imageView2/2/w/1099/format/webp)

在这里插入图片描述



## 写笔记

在这里给大家介绍一些typora比较常用的功能

### 各级标题

在typora中使用crtl + 1~6可以创建1-6级标题，1级最大，6级最小。同时typora的大纲会显示所有标题，如图：



![img](https:////upload-images.jianshu.io/upload_images/20760577-f7f5dc70573ad11f?imageMogr2/auto-orient/strip|imageView2/2/w/1074/format/webp)

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



![img](https:////upload-images.jianshu.io/upload_images/20760577-b4ef201685ee1bf2?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

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

# git官网文档

https://docs.gitbook.com/integrations/git-sync/content-configuration

