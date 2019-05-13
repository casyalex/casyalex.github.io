---
title: Cordova开发踩过的坑与记录
date: 2019-05-13 00:31:03
tags:
---

Q1. 我的 cordova 项目很卡，响应慢，如何解决性能问题？

A：项目只要求 ios，以下经验都只针对 IOS.

1. 首先内核很重要, cordova 默认用的是 UIWebView。这个内核非常古老了，但是因为各类兼容性好，有大量的老插件可以用，所以官方还是推荐这个内核。而原生的 IOS APP，他们用 Webview 都已经不选它了。新的叫 WKWebView，转换到这个内核基本就能提升至流畅的体验了。  
   Cordova 官方[WKWebview](https://github.com/apache/cordova-plugin-wkwebview-engine)：这个官方放出来基本就没更新了，兼容性差。不推荐！  
   Ionic 团队改进的[WKWebview](https://github.com/ionic-team/cordova-plugin-ionic-webview)：这个魔改版本，Ionic 持续维护，我用了，效果棒棒的，推荐！

   需要注意的是，用了 WKWebview，所有请求都要求 CORS 跨域，以及 file 协议的路径都需要调用 `window.Ionic.WebView.convertFileSrc()`转换一下。其他的怎么用，Readme 都写得很清楚了。

2. 关于动画。
