---
layout: post
title: "Cocoapods Private Repo Usage"
description: ""
category:
tags: [Cocoapods]
---


## 更新项目中新的第三方库

```shell
  pod install --verbose --no-repo-update
  pod update --verbose --no-repo-update  # --no-repo-update 表示可以不更新配置库
```

## 创建EMSpecs repo

* 创建git仓库，结构保持和Specs.git一致
* 提交到git server

```shell
  mkdir EMSpecs
  cd EMSPecs
  git init
  # 添加podspecs 省略
  git commit
  git remote add origin http://ph.benemind.com/diffusion/SPEC/emspecs.git
  git push origin master
```

## 私有Cocoapods Specs的使用
  * 指定[EMSpec](http://ph.benemind.com/diffusion/SPEC/emspecs.git)到电脑上
  * 搜索可用性
  * 添加到Podfile中

```shell
  pod repo add  EMSpecs http://ph.benemind.com/diffusion/SPEC/emspecs.git
  pod search EMSpeed #  查看是否有结果
```


## Cocoapods 进阶
 Framework作者需要自行维护好代码，代码的tag，以及对应到EMSpecs或Specs的配置文件

  * 创建podspec(第一次)
  * 检查可用性
  * 提交到EMSpecs 或 https://github.com/cocoapods/specs

~~~shell
pod spec create EMSpeed
pod spec lint EMSpeed.podspec
pod repo push EMSpecs EMSpeed.podspec
~~~

## 项目中实际使用
在`Podfile`中加入， pods会从两个仓库中找最新的库，但是上面的优先，所以顺序不同会影响到使用的库的版本

```shell
Source http://ph.benemind.com/diffusion/SPEC/emspecs.git
Source https://github.com/CocoaPods/Specs.git
```

## Ref
[Cocoapods.org](https://cocoapods.org)

## FAQ
#
