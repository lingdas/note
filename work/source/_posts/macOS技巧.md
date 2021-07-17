---
title: macOS技巧
author: lovelves
avatar: 'https://cdn.jsdelivr.net/gh/lingdas/note/img/avactor.jpeg'
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
tags: macOS技巧
keywords: 关闭更新
photos: 'https://cdn.jsdelivr.net/gh/lingdas/note/img/36.jpg'
date: 2021-07-17 12:27:31
authorLink:
description: macOS关闭更新
---

## 关闭macOs 中烦人的更新
步骤 1
到「应用程式」>「工具程式」内开启「终端机」。
步骤 2
开启终端机（Terminal）后，复制底下指令后按「Enter」执行，过程中会需要你输入管理者密码才能执行，输入完毕后重新开机一次即可。
```bash
sudo softwareupdate --ignore "macOS Catalina"
sudo softwareupdate --ignore "macOS big sur"
```
步骤 3
继续复制底下指令，直接贴上终端机后按「Enter」执行。
```bash
defaults delete com.apple.preferences.softwareupdate
```
步骤 4
最后执行底下指令，执行结果会显示macOS Catalina 10.15.5 不建议关闭系统更新，不过其实输入指令后也同样是可以强制关闭。
```bash
softwareupdate --list
```
步骤 5
完成以上步骤后，最后还是会发现有红色提示macOS 更新讯息，最后要更新缓存。
```bash
defaults delete com.apple.systempreferences AttentionPrefBundleIDs; killall Dock
```