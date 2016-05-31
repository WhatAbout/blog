---
layout:     post
<<<<<<< HEAD
title:      "jekyll+github+markdown搭载个人博客(未完待续)"
subtitle:   "jekyll+github+markdown搭载个人博客"
date:       2016-05-23 12:00:00
author:     "iMaple"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - 个人博客
    - jekyll 
    - github pages
    - markdown
---

>概要：关于 jekyll+github page 搭建个人博客的，网上已经有很多很不错的教程，本文作为第一次搭建博客过程中遇到问题的一点记录，当然如果本文对你有帮助，欢迎转载，转载时请标明出处~如果搭建过程有任何问题，也欢迎一起探讨学习。

***
注意：后文都是在window7-32位操作系统基础上搭建的，其他操作系统估计流程大同小异，请自行google~另外，搭载个人博客的方式也有很多种，你可以简单搭载之后自行设计界面，我这里选择的是最懒的方法--直接fork前人的代码，省去排版布局的时间，而且界面也确实很好看呢，文章最后会把参考的文章出处贴出来。
***

#### 搭建流程

1、购买域名（当然如果你已经有域名或者不需要独立的域名可以跳过这一步）；
2、开发环境配置（包括构建github博客库、git下载、jekyll开发环境相关软件下载及配置，评论、站长统计插件下载配置、免费图床等）
3、本地测试环境搭建，预览没问题即可上传至github
4、独立域名绑定（选择性忽略）

***

#### 1.1 购买域名

购买域名就很简单啦，有钱就行了。。。
去 [万网](https://wanwang.aliyun.com/)  查看想购买的域名是否已被注册，如果刚好没有而且价格也在接受访问内直接购买就行了哈。其实如果可用接受 username.github.com/name （username为你的github用户名，name为你博客库名，后面会提到）这种格式访问你的博客的话就无所谓啦~

#### 2.1 构建github博客库

##### 2.1.0 git 简介

通俗地讲：git是一个开源的分布式版本控制系统，而github则是git的代码托管站点
至于什么是github pages？
>What are GitHub Pages?
GitHub Pages are public webpages hosted and published through our site.
GitHub Pages is designed to host your personal, organization, or project pages directly from a GitHub repository. To learn more about the different types of GitHub Pages sites, see "[User, organization, and project pages](https://help.github.com/articles/user-organization-and-project-pages/)."
You can create and publish GitHub Pages online using the [Automatic Page Generator](https://help.github.com/articles/creating-pages-with-the-automatic-generator). If you prefer to work locally, you can use [GitHub Desktop](http://desktop.github.com/) or the [command line](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line).

大概的意思就是通过github站点创建的网页应用的一类统称吧？？？（这逼我是装定了！），它的好处就是免费、搭建简单。

##### 2.1.1 下载git

首先选择对应系统版本的git下载 [git下载](https://git-scm.com/download)，我这里下载的是window-32位的版本，下载完安装包之后，window的都是傻瓜式的，狂点下一步安装即可。。。
##### 2.1.2 构建github博客库
这里给个官网地址，注册跟代码仓库的创建流程不想说，直接跳过-->[猛戳这里](https://github.com/)

##### 2.1.3 git ssh配置和使用

a、初次使用git，需要设置username 跟 email
`$ git config --global user.name "imaple"`
`$ git config --global user.email "764915052@qq.com"`

b、紧接着我们生成秘钥（我输入miyao出来的第一个词居然是迷药？？？一看就知道搜狗不是啥正经的输入法。。。一定不关我事）。

`$ ssh-keygen -t rsa -C "764915052@qq.com"`

c、为方便操作接下来我们不设置密码，连续按3次回车，最后便得到两个文件：id_rsa 和 id_rsa.pub（生成过程会提示你文件生成的路径的，比如我的是在：C:\Users\asus\.ssh）

d、接下来我们把ssh_key添加到github站点

登陆github，点击右上角账号头像，选择下拉标，选中** Settings **->左侧选中** SSH and GPG keys **->选择** 新建SSH key **，title自由发挥吧，容易区分即可，然后把 id_rsa.pub 文件的内容复制过来，贴在 Key 这一栏，点击** Add SSH key ** 完成即可。

e、测试是否创建成功

`$ ssh -T git@github.com`

你将会看到：

`The authenticity of host 'github.com (207.97.227.239)' can't be established. RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48. Are you sure you want to continue connecting (yes/no)?`

输入`yes`，如看到以下提示即表明创建成功了

`Hi imaple! You've successfully authenticated, but GitHub does not provide shell access.`


<<<<<<< HEAD
#### 2.2 jekyll环境搭建

##### 2.2.0 jekyll 简介及相关

>jekyll是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，不需要数据库支持。但是可以配合第三方服务,例如Disqus。最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。

window上jekyll 的本地运行需要安装 [ruby](https://www.ruby-lang.org/zh_cn/downloads/) 跟 [Devkit for Ruby](https://github.com/oneclick/rubyinstaller/downloads/) ，下载完对应版本后，ruby 安装在默认c盘也可以，Devkit则是一个压缩包，假设我这里解压到c:/Devkit/，则打开cmd命令行界面，运行如下命令
`
cd Devkit //进到Devkit的解压路径
ruby dk.rb init
ruby dk.rb install
`
等待安装完成之后，运行 `ruby -v` 若可现实ruby对应的版本号即表明安装成功.

<<<<<<< HEAD
##### 2.2.1 更换GEM的source镜像

问：gem是什么鬼？？？
答：简单理解gem就是ruby的包管理器，类似node的npm吧。
问：为啥要切换掉原先的镜像？？？
答：要是要用为啥要切换掉呀，当然我也不确定是不是我这的网速问题，用原先镜像来安装一些插件的时候确实很慢（长城50M宽带，真特么坑爹啊）。

那么要切换成哪个镜像好一点呢，网上的教程包括官方大多都推荐淘宝的镜像，然而我试过都没有成功（人品问题？？？）。最后我选择了中山大学的镜像，还算ok吧

1. 首先确认一下当前source镜像地址（注意最后是-l，小写的L，不是-1.。。）
`gem sources -l`
2. 删除GEM的默认source镜像rubygems.org

`gem sources -r http://rubygems.org/`
3. 将 https://ruby.taobao.org 、http://ruby.taobao.org 设置为GEM的source镜像（你可以根据自己的实际情况，我这里选择的是中山大学提供的镜像路径 http://mirror.sysu.edu.cn/rubygems/ ）

`gem sources -a http://mirror.sysu.edu.cn/rubygems/`

##### 2.2.2 安装jekyll 及相关插件

1、通过开始菜单进入 ruby 的命令行界面
2、依次运行如下命令
`
gem install jekyll //安装jekyll
gem install kramdown //markdown语言解析包
gem install pygments.rb //代码高亮包
gem install liquid //有用。。。
//其他包看情况跟实际需要安装
`

附：其他几个可用的gem镜像：

– 中山大学：http://mirror.sysu.edu.cn/rubygems/
– 山东理工大学：http://ruby.sdutlinux.org/
– 淘宝网：http://ruby.taobao.org/


***


<<<<<<< HEAD
#### 文章参考

1、黄玄的博客模板 -> [猛戳这里](https://github.com/Huxpro/huxpro.github.io/blob/master/README.zh.md)

2、采用Jekyll + github + pygments构建个人博客的最终说明 ->[猛戳这里](http://www.jianshu.com/p/609e1197754c)