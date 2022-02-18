---
title: 多终端使用hexo上传
date: 2022-02-18 16:30:23
tags: 教程
---

# 一、git分支进行多终端工作

现在我在我的笔记本上建立的hexo项目博客，回到家之后想在我的台式电脑上继续上传笔记，不知道怎么移动文件了，怎么办呢？

在网上查阅之后了解了几种办法，这里采用git的分支系统进行多终端工作，以后在某个设备上如果想要继续写东西，只要进行简单的配置和在github上同步下来文件，就可以无缝操作了。



# 二、hexo机制

hexo的机制就是，`hexo d`会将编译后的文件上传部署到github（也就是.depoloy_git文件内的内容），使用来生成网页的静态资源并不包含源文件。

![main分支](%E5%A4%9A%E7%BB%88%E7%AB%AF%E4%BD%BF%E7%94%A8hexo%E4%B8%8A%E4%BC%A0/image-20220218164144970.png)

其他文件都没有上传到github

![所有源文件](%E5%A4%9A%E7%BB%88%E7%AB%AF%E4%BD%BF%E7%94%A8hexo%E4%B8%8A%E4%BC%A0/image-20220218164301379.png)

这里就可以利用github的分支管理，将源文件上传到另外一个分支即可。

# 三、上传分支

## ①在github上新建一个hexo分支

如图

![hexo分支](%E5%A4%9A%E7%BB%88%E7%AB%AF%E4%BD%BF%E7%94%A8hexo%E4%B8%8A%E4%BC%A0/image-20220218164507686.png)

在本仓库的setting中选择默认分支为hexo分支，方便以后同步不用指定分支。

## ②本地新建一个目录用于clone分支

在新建的文件夹中打开git bash

```js
git clone git@github.com:burnlee1994/burnlee1994.github.io.git
```

将其克隆到本地，因为此前默认分支已经设置成了hexo，所以克隆的时候只克隆了hexo分支。

此时克隆到新建的文件夹的文件就是和main分支中一模一样的，就是.depoloy_git中的文件。

接下来在burnlee1994.github.io中，把除了.git的文件全部删掉。

然后将之前写的博客的全部源文件，除了.depoly_git，全部复制过来。

PS：这里需要注意的是，复制过来的源文件中又一个.gitignore，用来忽略一些不需要的文件，如果没有就新建一个，在里面写上如下，表示这类型的文件不需要被git

```js
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

注意，如果之前克隆过theme中的主题文件，那么应该把所有子文件下的.git文件夹删掉。

然后

```js
git add .
git commit -m "add branch"
git push
```

这样就上传完了，可以去github上看看hexo分支有没有上传成功，其中的`node_modules`、`public`、`db.json`已经被忽略，但是没关系这些不需要上传，只需要输入命令安装即可。

![hexo分支的源文件](%E5%A4%9A%E7%BB%88%E7%AB%AF%E4%BD%BF%E7%94%A8hexo%E4%B8%8A%E4%BC%A0/image-20220218165452738.png)

# 四、更换电脑操作

和之前的环境搭建一样

## ①安装git

## ②设置git全局邮箱和用户名

```js
git config --global user.name "yourgithubname"
git config --golbal user.email "youremail"
```

## ③设置ssh key

```js
ssh-keygen -t ras -C "youremail"
```

生成成功后填入到github上

## ④安装node.js

## ⑤安装hexo

```js
npm install hexo cli -g
```

但是不需要初始化了，直接在任意文件夹下

```js
git clone git@.....
```

然后进入到克隆的文件夹

```js
npm install
npm install hexo-deployer-git --save
```

生成、部署

```js
hexo g
hexo d
```

然后就可以开始写新博客了



## PS：

1.注意不要忘了每次写完都把源文件上传一下

```js
git add .
git commit -m "xxx"
git push
```

2.如果是在已经编辑过的电脑上，已经有了克隆文件夹，那么每次只要和远端同步一下就可以了

```js
git pull
```

