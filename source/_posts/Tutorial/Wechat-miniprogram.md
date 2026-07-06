---
title: Wechat miniprogram
date: 2026-03-27 17:05:55
tags:
archive: true
---

微信小程序实际上和网页差不了太多，会前端三件套HTML、CSS、Javascript基本上小程序就可以写个七七八八了，会Vue就更好了，我们写小程序主要使用的是两个软件：[HBuilder X](https://dcloud.io/hbuilderx.html) 和 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)，前者是相当于小程序编辑器，后者相当于是小程序运行的模拟器。

之前一直使用 claude code 辅助开发，对代码结果并不很理解，后来要在 windows 和 mac 上切换开发才成功慢慢明白，小程序的文件夹一般是这样组织的：
```bash
.
├── App.vue
├── components
│   └── custom-tabbar
│       └── custom-tabbar.vue
├── index.html
├── main.js
├── manifest.json
├── pages
│   ├── about
│   │   └── about.vue
│   └── ai
│       └── ai.vue
├── pages.json
├── project.config.json
├── project.private.config.json
├── static
├── store
├── uni.promisify.adaptor.js
├── uni.scss
├── unpackage
│   └── dist
│       └── dev
│           └── mp-weixin
└── utils
```
- `Pages`页面中，每个文件夹 = 一个独立页面，页面必须在 `pages.json` 里注册才能访问

当我们在HBuilder X 中将代码编写完成后，点击上面的`运行`:arrow_right:`运行到小程序模拟器`:arrow_right:`微信开发者工具`，后面应该会自动跳转到微信开发者工具并打开。如果在 mac 上可能需要手动配置一下，但是注意我们在微信开发者工具中进入的文件夹不是我们编写的`uni-app` 源码目录，而是编译后生成的`unpackage/dist/dev/mp-weixin`，因此我们后续只需修改「源代码」，不要碰 `unpackage`，如果我们修改`unpackage/` 里的任何文件，后面重新编译生成后也会被重新覆盖而无效！当我们改完代码后`保存`:arrow_right: `自动编译` :arrow_right: `自动刷新小程序`，不用手动刷新运行！