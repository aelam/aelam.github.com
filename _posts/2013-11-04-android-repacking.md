---
layout: post
title: "apk解包,修改,重新打包"
description: "apk解包,修改,重新打包"
category:
tags: [Android, apk, repacking]
---

# apk解包,修改,重新打包

### 环境 shell/bat(此处用shell)
### 需要工具 apktool, sed(或者其他替代), jarsigner

```shell

	#!/bin/bash
	referral_id="pinggege"
	origin_apk="jifen520-appChina-release-1.01.apk"
	apk_temp="apk.temp"

	keystore_file="android.keystore"
	keystore_alias="android.keystore"
	storepass="4541973"
	keypass="4541973"

	#step 1 解包
	apktool d $origin_apk $apk_temp

	#step 2 替换
	sed -e s/referral_place_holder/$referral_id/g "$apk_temp/AndroidManifest.xml" > "$apk_temp/AndroidManifest.xml.tmp"
	mv $apk_temp/AndroidManifest.xml.tmp $apk_temp/AndroidManifest.xml

	#step 3 打包
	apktool b -f "$apk_temp" "$apk_temp.apk"


	#step 4 重新签名
	jarsigner -sigalg MD5withRSA -digestalg SHA1 -keystore android.keystore -storepass $storepass -	keypass $keypass "$apk_temp.apk" android.keystore -signedjar "$referral_id.apk"

	#step 5 清理
	rm -rf "$apk_temp"
	rm "$apk_temp.apk"

	echo "the resigned apk is $referral_id.apk!!"
```
