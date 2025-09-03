---
title: Offload Your Hexo Images
date: 2025-08-21 16:59:18
tags: [tutorial, hexo,image hosting]
index_img:  https://github.com/Richard110206/Blog-image/blob/main/cover/ImageHosting.png?raw=true
category: Tutorial
category_bar: true
description: Using GitHub in conjunction with PicGo to establish an image hosting service.
---

{%note info%}之前刚开始进行`Hexo`博客撰写，图片都保存在本地`Hexo`源文件目录（`source/images/`）文件夹，随着图片增多，管理起来压力增大，于是产生了使用图床，引入外链进行图片存储的想法！{%endnote%}

## `Pros and Cons`
{%note info%}
- **提升部署速度**：当你执行 `hexo g -d `部署时，`Hexo` 需要将你博客的所有源文件，包括文章、主题、以及几十上百张图片，全部处理并拷贝到`public`文件夹，然后再将这个巨大的文件夹推送到`GitHub`，这个过程会非常缓慢。
- **跨设备写作与协作更方便**：本地存储时，如果你想在另一台电脑上写文章，必须把整个包含所有图片的庞大仓库克隆下来。而使用图床，你只需要克隆**轻量级的博客源码仓库**。文章里的图片都是绝对路径（`URL`），在任何设备上打开都能正常显示。
- **版本控制体验极佳**：使用图床后，你的博客源码仓库非常干净，`Git` 可以高效地管理文本差异。图片仓库独立出去，两者互不干扰，管理起来清晰明了。
{%endnote%}

&emsp;&emsp;正因为有以上优势，因而我强烈建议将图片放在图床中进行存储，常见的的**图床服务**有[阿里云OSS](https://oss.console.aliyun.com)、[腾讯云 COS](https://console.cloud.tencent.com/cos5)、[七牛云](https://portal.qiniu.com/kodo)、[SM.MS](https://sm.ms/)、[Github](https://github.com/)等。
&emsp;&emsp;考虑到前几者涉及到注册、实名认证、存储收费等问题，再加上认为`Github`大概率不会倒闭，图片存储较为稳定，且已有账号注册等原因，最终选择使用[PicGo插件](https://picgo.github.io/PicGo-Doc/zh/guide/#picgo-is-here)和`Github`实现**图床**功能！

{%note info%}
- 更简单的图床功能网站：[IMG.TG](https://img.tg/richard_11_02_06)（但是感觉像`start up company`界面有点简陋，担心有**存储不稳定**的问题）（后来发现有国内百度云 CDN 节点加速，口碑还不错，那大抵是我手拙无福消受了:cry:）

- 不想折腾的可以看看这篇骚操作:grinning:：[图片外链方法大全： 免费的图床！ 告别新浪图床 和 CDN](https://blog.csdn.net/hotdog233/article/details/119380498?ops_request_misc=&request_id=&biz_id=102&utm_term=%E5%8F%AF%E7%9B%B4%E6%8E%A5%E8%AE%BF%E9%97%AE%E7%9A%84%E5%9B%BE%E7%89%87%E7%9B%B4%E9%93%BE&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-119380498.142^v102^pc_search_result_base2&spm=1018.2226.3001.4187)
{%endnote%}


下面分别介绍 `PicGo` **图形化桌面版**（适合新手，操作直观）和 `PicGo-Core` **命令行版**（适合终端用户，轻量化）的完整配置流程，实现`GitHub`图床功能。
### 一、准备工作
以 `GitHub` 图床 为例，配置步骤如下：
1. 创建 `GitHub` 仓库
2. 新建一个公开仓库（如 `blog-images`），用于存储图片
3. 记住仓库路径：用户名/仓库名（如 `username/blog-images`）
4. 生成 `GitHub` 访问令牌
- 打开 `GitHub` → 点击头像 → `Settings` → `Developer settings` → `Personal access tokens` → `Generate new token`
- **勾选 `repo` 权限**（**必须**），生成后复制令牌（仅显示一次，需在记事本中保存）
选择并安装**图床插件**
根据需求安装对应图床的插件，以常用的`GitHub`图床 为例：
```bash
picgo install github-plus  # GitHub 增强插件，支持自定义路径等
```
其他常用插件：
- 阿里云 OSS：`picgo install aliyun-oss`
- 腾讯云 COS：`picgo install tencent-cos`
- 七牛云：`picgo install qiniu`

### 二、配置图床参数
安装 `Node.js`（命令行版必装，图形化版可选）
下载地址：[Node.js 官网](https://nodejs.org/zh-cn)（推荐 LTS 版本）。
验证安装：终端输入 `node -v` 和 `npm -v`，能显示版本号即成功。

### 三、`PicGo` 图形化桌面版配置（新手推荐）
图形化界面**操作直观**，无需记忆命令，适合首次配置图床的用户。

1. 安装`PicGo`桌面版
下载地址：[PicGo GitHub Releases](https://github.com/Molunerfinn/PicGo/releases)。
选择对应系统版本（`Windows` 选 `exe`，`Mac` 选 `dmg`），安装后打开。
2. 配置 GitHub 图床参数
左侧菜单栏点击 「`图床设置`」 → 选择 「`GitHub图床`」）。
依次填写参数：
- `repo`：用户名/仓库名（如 `username/blog-images`）
- `branch`：分支名（默认 `main`）
- `token`：刚才生成的 `GitHub` 令牌
- `path`：图片在`github`的存储路径（可选，如 `images/2024/`）
- `customUrl`：自定义 CDN 域名（可选，如 `https://cdn.jsdelivr.net/gh/用户名/仓库名`）
填写完成后，点击 「`设为默认图床`」，配置生效。
3. 验证配置（上传测试）
点击 `PicGo` 主界面的 「`上传区`」，**直接拖入本地图片**，或**粘贴剪贴板截图**（如微信截图后直接粘贴或电脑快捷键截图）。上传成功后，进入`Github`，进入图片右键`复制图片链接`即可！
### 三、`PicGo-Core` 命令行版配置（终端用户推荐）
**轻量化**、**无界面**，适合习惯用终端操作的用户，**可集成到脚本或编辑器**中。
1. 安装 `PicGo-Core`
打开终端（`Windows` 用 `CMD/PowerShell`，`Mac/Linux` 用 `Terminal`），执行以下命令全局安装：
```bash
npm install picgo -g
```
- 验证安装：输入 `picgo -v`，显示版本号即成功。

2. 安装 `GitHub` 图床插件
```bash
picgo install github-plus
```
其他常用图床插件（按需安装）：
- 阿里云 OSS：`picgo install aliyun-oss`
- 腾讯云 COS：`picgo install tencent-cos`
- 七牛云：`picgo install qiniu`

3. 配置 `GitHub` 图床参数
```bash
picgo set uploader
```
依次**填写参数**（参考前面图形化版的参数说明）

4. 配置完成后，设置 `github-plus` 为默认上传器：
```bash
picgo use uploader  # 再次选择 github-plus 并回车
```

5. 验证配置（上传测试）
```bash
# 替换为你图片的本地路径
# Windows 示例
picgo upload C:\Users\Legion\Pictures\test.jpg
# Mac/Linux 示例
picgo upload ~/Desktop/test.jpg
```
{%note info%}
- 成功：终端会输出图片直链，复制链接到浏览器可打开图片。
- 失败：检查 `token`、`repo`权限是否开通、图片存储路径是否正确，或网络是否通畅。
{%endnote%}

### 四、与 `Hexo` 集成
在 `Hexo`文章中直接使用上传后的图片链接，例如：
```markdown
![示例图片](Github直链地址)
```

### 链接在csdn无法显示问题
&emsp;&emsp;当我们在`github`配置好图床服务后，想要在`CSDN`进行引用 ，会发现编辑时显示正常，但是发布后会显示图片转存失败，如下图所示。这是因为`CSDN` 为了防止其他网站直接引用（消耗 CSDN 的服务器流量和带宽）本站的图片资源，会设置**防盗链功能**。
![编辑器中显示正常](https://github.com/Richard110206/Blog-image/blob/main/article/Tutorial/ImageHosting/editor.png?raw=true)

![发布后显示转存失败](https://github.com/Richard110206/Blog-image/blob/main/article/Tutorial/ImageHosting/imagefail.png?raw=true)

&emsp;&emsp;当在外站中插入一个来自 `github.com` 的图片链接时，用户的浏览器会向 `GitHub` 的服务器请求这张图片。`GitHub` 服务器在响应时，可能会检查请求的来源（`Referer`）。如果来源是 `csdn.net`，而 `GitHub` 并未将 `CSDN` 加入白名单，`GitHub` 可能会**拒绝这个请求或返回一个错误**（如 `403 Forbidden`）。反过来，如果 `CSDN` 检测到图片不是来自自己的服务器，也可能会拦截显示。

&emsp;&emsp;在 `CSDN` 博客编辑器中，点击图片上传按钮。选择图片进行上传。`CSDN` 会**自动将图片上传到自己的图床**，并生成一个新的、稳定的 `CSDN` **内部链接**，这时候就可以正常引用图片了！

封面来源：[Imagen AI](https://x.com/Imagen_Network/status/1957955746492215713/photo/1)


