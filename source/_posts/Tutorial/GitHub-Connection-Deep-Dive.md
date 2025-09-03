---
title: GitHub Connection Deep Dive
date: 2025-08-27 14:25:40
tags: [Github,updating]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/GithubConnection.png?raw=true
category: Tutorial
category_bar: true
description: Triggered by an SSH connection failure to GitHub, this article explores and explains the methods for connecting to GitHub—including via HTTPS and SSH.
---

今天在将hexo博客进行部署的时候突然发生以下报错：

```bash
ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
Error: Spawn failed
    at ChildProcess.<anonymous> (E:\Hexo-backup\node_modules\hexo-deployer-git\node_modules\hexo-util\lib\spawn.js:51:21)
    at ChildProcess.emit (node:events:518:28)
    at cp.emit (E:\Hexo-backup\node_modules\cross-spawn\lib\enoent.js:34:29)
    at ChildProcess._handle.onexit (node:internal/child_process:293:12)
```

在CSDN找到相应问题文档，才成功解决！[git报错ssh: connect to host github.com port 22: Connection timed out](https://blog.csdn.net/nightwishh/article/details/99647545?fromshare=blogdetail&sharetype=blogdetail&sharerId=99647545&sharerefer=PC&sharesource=m0_53058983&sharefrom=from_link)

## HTTPS
## SSH
