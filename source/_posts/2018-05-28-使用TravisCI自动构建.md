---
title: 使用TravisCI自动构建
date: 2018-05-28 10:54:13
---

## 前言

最近发现了个很好用的东西：`TravisCI`，能自动构建项目。其实，持续集成我早就有所了解，不过没怎么操作过。以前也知道`TravisCI`，但没有相关需求，所以就没怎么接触。这几天在找一个适合做笔记的静态网站程序，发现了`MkDocs`挺不错的，也看了一些静态博客程序，比如`纸小墨`。不过，感觉方便的不容易定制外观，因为考虑到`Hexo`还是用的人比较多，且我对`Hexo`已经有所了解，就懒得去折腾新的静态博客了。但是，在看`纸小墨`时有了重大发现。

<!-- more -->

在看`纸小墨`时，找到了使用`TravisCI`自动构建博客并部署，这样，用户只需要写博客，然后Push就行了，`TravisCI`会自动完成静态博客的构建与部署，只需要一份`TravisCI`配置（`.travis.yml`）就行了！

想想在写代码时，提交后自动编译、运行测试，不需要手动编译测试，真的很方便。我以为这样的服务应该是收费的，就很长时间里没有去了解。最近了解一下，才发现解决了我多年来的痛点。难怪很多项目的README里会显示编译状态，原来如此。。。

`TravisCI`和GitHub结合的比较好，另外还有其它的持续集成工具，如`Jenkins`。

## 配置

`TravisCI`的配置也不难，采用的是YAML格式，文件名是`.travis.yml`，放在项目的根目录。每次Push后会触发`TravisCI`的启动运行。

以下是构建`MkDocs`的配置：

``` YAML
language: python
python: 
  - "3.5"
install: 
  - pip install mkdocs
  - pip install python-markdown-math
script: 
  - mkdocs build --clean
  - mkdocs build
after_success:  |
  if [ -n "$REPO_TOKEN" ]; then
    cd "$TRAVIS_BUILD_DIR"
    cd site
    git init
    git add .
    git -c user.name=$GITHUB_NAME -c user.email=$GITHUB_EMAIL commit -m "Auto Deployment"
    git push -f -q https://$GITHUB_NAME:$REPO_TOKEN@$REPO master:gh-pages
    cd "$TRAVIS_BUILD_DIR"
  fi
```

`GITHUB_NAME`、`GITHUB_EMAIL`、`REPO_TOKEN`和`REPO`都是配置的环境变量，在`TravisCI`的项目设置里可以进行配置：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180528111946.png)

其中，`REPO_TOKEN`可以在[GitHub Token管理](https://github.com/settings/tokens)生成，需要repo权限，这样`TravisCI`才能push代码。

可以看到，其实就是配置了一些脚本，注意命令别写错了，特别是git push。
