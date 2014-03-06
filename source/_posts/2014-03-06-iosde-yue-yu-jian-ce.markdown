---
layout: post
title: "iOS的越狱检测"
date: 2014-03-06 22:47:58 +0800
comments: true
categories: iOS开发
---
在许多应用场景中，基于安全或其他考虑，都需要检测用户的iOS设备是否已经越狱。由于iOS沙箱机制的存在，许多在普通的系统上很容易能够做到的事情在iOS系统上都变的不可能了；而如果设备越狱，沙箱机制完全被干掉，许多原先不敢想的事情都能做到了，这也是目前大多数越狱检测的思路。

##方法一：判断是否安装越狱专有应用

检测手机上是否安装有越狱手机专属应用是最常见的越狱检测方式，比如Cydia，检测的方法包括路径检测与URL Schema检测。

###路径检测

```objective-c
    if([[NSFileManager defaultManager] fileExistsAtPath: @"/Applications/Cydia.app"]) {
        return YES;
    }
```
###URL Schema检测

```objective-c
if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"cydia://"]]) {
    return YES;
}
```
除了Cydia，在天朝手机上的一些特色手机应用也可以作为越狱检测的一个因子，比如baidu输入法，sougou输入法等，例如检测sougou输入法是否安装。
###Sougou输入法检测

```objective-c
if([[NSFileManager defaultManager] fileExistsAtPath:@"/Applications/SogouSettings.app"]) {
    return YES;
}
```

##方法二：检测越狱后特有工具或库

某些工具，例如sshd, gdb及bash等，只在越狱后的手机上才会存在，检测这些工具也是检测越狱的一种有效手段。
###gdb检测
```objective-c
if([[NSFileManager defaultManager] fileExistsAtPath:@"/usr/bin/gdb"]) {
    return YES;
}
```
###sshd检测
```objective-c
if([[NSFileManager defaultManager] fileExistsAtPath:@"/usr/sbin/sshd"]) {
    return YES;
}
```
除了特定工具，一些特殊的lib也是越狱手机专用，比如Cydia下众多插件都依赖的MobileSubstrate.dylib。
###MobileSubstrate检测
```objective-c
if([[NSFileManager defaultManager] fileExistsAtPath:@"/Library/MobileSubstrate/MobileSubstrate.dylib"]) {
    return YES;
}
```
**PS:** Mobile Substrate从0.9.5之后加入64位处理器支持，并更名为Cydia Substrate，所以检测路径相应更改为"/Library/Frameworks/ CydiaSubstrate.framework/CydiaSubstrate.framework"
