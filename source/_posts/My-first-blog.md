---
title: My first blog
date: 2017-03-19 17:05:58
tags:
     - blog
     - hexo

---

这是我的第一篇博客，也代表着我走向**前端**的起点

本文记录了搭建此博客的过程（windows）

<!--more-->

### 配置环境

首先git bash、node.js、github该安装的安装，该申请账号的申请账号，没得问题。



### github

先创建一个github仓库，名字随意，如Blog

然后git clone到本地

```{bash}
C:\Users\Administrator\Desktop\MyProject>git clone git@github.com:jer0701/Blog.git
```



### hexo 

npm安装 hexo

```{bash}
C:\Users\Administrator\Desktop\MyProject\Blog>npm install -g hexo
```

执行初始化

```{bash}
C:\Users\Administrator\Desktop\MyProject\Blog>hexo init
```

好啦，到这里所有的安装工作都完成啦！



### 生成静态页面

```{bash}
C:\Users\Administrator\Desktop\MyProject\Blog>hexo generate
```

### 本地启动

```{bash}
C:\Users\Administrator\Desktop\MyProject\Blog>hexo server
```

然后在浏览器输入[http://localhost:4000](http://localhost:4000)，可以预览

我当时怎么也查看不了，最后发现把VPN代理关闭就可以看了





### 部署到github

需要配置 _config.yml文件

​	deploy:
  		type: git
 		repository: git@github.com:jer0701/Blog.git
  		branch: gh-pages



然后部署

```{bash}
C:\Users\Administrator\Desktop\MyProject\Blog>npm install hexo-deployer-git --save
C:\Users\Administrator\Desktop\MyProject\Blog>hexo deployranjo
```

然后通过访问[http://jer0701.github.io/Blog/](http://jer0701.github.io/Blog/)就可以了,至此，一个完整的博客搭建好了，你可以自己配置主题和域名๑乛◡乛๑







##### 问题及解决

* hexo deploy 显示Permission denied (publickey).

```{bash}

debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Trying private key: /c/Users/Administrator/.ssh/id_rsa
debug1: Trying private key: /c/Users/Administrator/.ssh/id_dsa
debug1: Trying private key: /c/Users/Administrator/.ssh/id_ecdsa
debug1: Trying private key: /c/Users/Administrator/.ssh/id_ed25519
debug1: No more authentication methods to try.
Permission denied (publickey).

Administrator@EJ24HRCEX0UU263 MINGW64 ~
$ ls ~/.ssh
github_rsa  github_rsa.pub  known_hosts

###将github换成id开头

Administrator@EJ24HRCEX0UU263 MINGW64 ~
$ ssh -T git@github.com
Hi jer0701! You've successfully authenticated, but GitHub does not provide shell access.
```





* git add ./ 的时候显示filename too long    

```{bash}
C:\Users\Administrator\Desktop\MyProject\Blog>git config --global core.longpaths true
```

