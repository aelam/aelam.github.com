---
layout: post
title: "用cocoapods创建模块"
description: "用cocoapods创建模块"
category: "iOS Development"
tags: [iOS, CocoaPods]

---



## 用pod创建一个库

pod提供了创建lib的工具集，如下操作即可。里面包括了测试项目和lib项目便于你在同一个Xcode项目里面完成lib代码，podspec以及demo测试

```ruby
pod lib create EMClick
```

## 模块的开发

### Xcode里面代码以及对应的资源文件都有相应的文件夹放置. 自己也要根据自身模块的特性做好文件管理

### 切到Example目录 pod install 安装Example的依赖. 当然也会包括你当前开发的苦

```shell

#tree -L 3  
.
├── EMClick.podspec
├── Example
│   ├── EMClick
│   │   ├── EMAppDelegate.h
│   │   ├── EMAppDelegate.m
│   │   └── main.m
│   ├── EMClick.xcodeproj
│   │   ├── project.pbxproj
│   │   ├── project.xcworkspace
│   ├── EMClick.xcworkspace
│   ├── Podfile
│   ├── Podfile.lock
│   ├── Pods  # example的dependency
│   │   ├── AFNetworking
│   │   ├── SFHFKeychainUtils
│   │   └── Target\ Support\ Files
│   └── Tests
│       ├── Tests-Info.plist
│       ├── Tests-Prefix.pch
│       ├── Tests.m
│       └── en.lproj
├── LICENSE
├── Pod
│   ├── Assets  # lib的资源目录
│   └── Classes # lib的代码目录
│       ├── EMCReportPolicy.h
│       ├── EMClick.h
│       ├── EMClick.m
│       └── model
├── README.md
└── _Pods.xcodeproj -> Example/Pods/Pods.xcodeproj

```

### podspec的配置

EMClick文件夹里面会有podspec。

* Dependency 使用的依赖对应的版本，根据情况指定版本
* Resource 最终会生成一个bundle
* Source 代码放在对应的位置, 比如github

> 注意: 开发初期可以不用管source的位置。podfile里面配置path的方式会查找本地相对路径，算是开发模式。


## 模块的上传
 参考[私有库搭建](http://aelam.github.io/2015/06/29/cocoapods-private-private-repo-usage/)

### 上传方式
1. `pod repo push`

```ruby
pod repo push EMSpecs EMSpeed.podspec
```
2. 手动编辑EMSpecs仓库然后`git push`

> 保证库能正常使用，要不要需要代码仓库和配置仓库的都需要更改

### 模块的版本
git仓库里面打好tag，podspec指定好版本和source的version，这里version一般是git的tag。做到直观有规可循心里有谱

* 在配置库里面不同的版本是如下结构

```shell
.
├── 0.0.1
│   └── EMSpeed.podspec
├── 0.0.2
│   └── EMSpeed.podspec
└── 0.1.0
    └── EMSpeed.podspec
```
> EMSpeed.podspec里面的版本对应EMSpeed仓库里面对应的tag



## 模块的使用

### 自定义配置库需要在podfile里面指定source

```ruby
source 'http://ph.benemind.com/diffusion/SPEC/emspecs.git'
source 'https://github.com/CocoaPods/Specs.git'

pod 'EMClick', '~> 0.1.0' # 也可以不指定版本

```

## pod自定义库开发的常见方式

### 指定podspec
这样你可以灵活的定制使用哪一个repo，repo的branch/tag/revision,  甚至本地目录也没有关系

```ruby
    pod 'EMSpeed', :podspec => '/path/to/podsepc'
```

### 直接指定仓库
这样的前提条件是仓库里面必须有一个podspec作为lib的配置文件。（这种情况你配置的tag和仓库里面的podspec指定的source版本可能不一样就有可能看到podinstall的时候detach head的情况）

```ruby
pod 'EMSpeed', :git="https://github.com/aelam/emspeed.git", :tag => "1.0.0" #:commit=>
```

### 自定义私有配置库
将EMSpeed.podspec按照配置库的规则放入配置库 参考[私有库搭建](http://aelam.github.io/2015/06/29/cocoapods-private-private-repo-usage/)

```ruby
source 'https://github.com/CocoaPods/Specs.git'
pod 'EMSpeed'
```

### pod开发模式，指定path
即我们开篇使用的方式

```ruby
pod 'EMSpeed', :path => '/path/to/repo'
```
