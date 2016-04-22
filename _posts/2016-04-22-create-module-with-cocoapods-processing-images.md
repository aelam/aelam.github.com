---
layout: post
title: "Processing Image Names in CocoaPods"
description: ""
category: iOS
tags: [iOS, CocoaPods]
---

# Images in Cocoapods-based Module.

## Resources config in podspec

### 1. Config `resource_bundles`

CocoaPods will create a bundle which will be copied in MainBundle(or iOS8.0+ bundle of your Dynamic Framework). then you have to find the bundle, then find the resource_bundle, then access the Images.

```
NSBundle *bundle = [NSBundle bundleForClass:[AClassInYourModule class];
// then get the image from the bundle

```

### 2. Config `resources`.
Just `s.resources = /path/to/your/assets`. when you build your app. CocoaPods script will copy your resouces of this module to MainBundle. you needn't change your origin code at all.
It's not clear as the first one. but it's currently the best solutions for my apps.

# Trouble?

After iOS7+, engineers have tended to use xcassets rather than folders to manage @2x, @3x images.
xcassets is much easier to maintain. you just need to care the names of the imageset. when you drag the images in a xcassets, the Content.json will map the image to your imageset automatically.
BUT when you are doing your module you will find the real name of the images are different from the names of imagesets. When you run your app, you can't load your images at all just because the image name are different from its origin imageset.

Then you need to rename all images, make them same as the imagesets name. Then no matter you are using a xcassets or CocoaPods-managed resources, it can work!

Attached is the script can help you to rename the imageset's names

```
./image_rename.py your.xcassets 2.xcassets

```


<script src="https://gist.github.com/aelam/b22a072aee24e835ed3202749e1ee21b.js"></script>
