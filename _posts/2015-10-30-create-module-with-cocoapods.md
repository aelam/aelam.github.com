---
layout: post
title: "用CocoaPods创建模块前传"
description: "创建pod模块的知识准备"
category: "iOS"
tags: [iOS, CocoaPods]
---

# pod lib create EMSpeed 然后怎么办?

1. 在ph.benemind.com或者github上创建创建好一个git仓库 拷贝仓库地址  http://ph.benemind.com/xxx.git
2. 终端切换到建好的EMSpeed文件夹下面, 然后执行`git remote add origin http://ph.benemind.com/xxx.git`  

完成上述步骤EMSpeed就与远端地址关联起来了

# podspec该如何配置?

### 配置好version, 和之前的仓库地址, tag会读取version, git仓库也要打一个对应的tag

```ruby
s.version          = "0.1.2"
s.source           = { :git => "http://ph.benemind.com/diffusion/EMSPEED/emspeed.git", :tag => s.version.to_s }
```

### 涉及到的git操作

```ruby

cd /path/to/repo

# 添加远端地址 (origin为远端别名, 后面的地址是远端仓库地址)
git remote add origin http://ph.benemind.com/diffusion/EMSPEED/emspeed.git

# commit代码
git add .
git commit -m "updated 头文件结构"

# 为本地仓库打tag
git tag 0.1.2

# 把代码推到远端仓库
git push origin master

# 把tag推到远端仓库
git push origin --tags # 把所有tags都推送上去
git push origin 0.1.2  # 把tag 0.1.2推送上去

```

# lib写完以后怎么办

1. 设置好版本, 这里以EMSpeed和版本0.1.0为例
2. 修改EMSpeed.podspec为0.1.0 里面的version设置为0.1.0
3. EMSpeed.podspec的source 地址设置为前面生成的远端仓库地址
4. pod lib lint EMSpeed.podspec 检查库的配置是否有问题
5. 检查通过以后commit一下仓库, 然后给仓库打上tag 0.1.0
6. 提交代码和tag到远端 git push origin master --tags
7. 准备就绪以后 切换到仓库里面EMSpeed.podspec所在目录 然后执行 `pod repo push EMSpecs EMSpeed.podspec` 关于EMSpecs的配置请看后一篇文章
8. pod search EMSpeed 检查是否能搜索到

# 当前开发的libA依赖于另一个开发的libB怎么办

1. 在libA.podspec里面设定dependency `s.dependency libB`
2. 在libA的demo项目里面找到Podfile 然后指定`pod libB, :git=>"http://ph.benemind.com/libB.git"`
