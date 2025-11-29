---
title: ICP Filing
date: 2025-09-27 11:33:27
tags: [Server, Website Launch,ICP filing]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/ICP%20filing.png?raw=true
category: Tutorial
category_bar: true
description: This article details the entire process from purchasing a server to successfully completing the filing, including server selection, domain name purchase, filing procedures, and precautions.
---


## 提醒
{%note danger %}
备案的流程较为复杂，等待时间也较长（尤其是当你的申请被打回时），如果你还在观望，没有购买服务器，那么笔者建议在预算充足的情况下可以考虑香港及海外的服务器，无需备案避免耽误时间，且当目标群体主要在国外时，访问更为方便，网络连接速度和稳定性可能会更好。
{%endnote%}

这里我尝试的部署电商平台的教程如下所示，内容很清楚，即使零基础2h也可完成速通！
[跨境电商独立站搭建教程，WordPress外贸建站指南，网站搭建教程](https://blog.vpszj.cn/archives/2615.html?amp=1)

## 域名备案流程及注意事项

1. 选择合适的**服务器提供商**和**域名注册商**进行购买，可以直接选择**大厂**进行购买 [百度智能云](https://cloud.baidu.com/) 、 [阿里云](https://www.aliyun.com/)、 [腾讯云](https://cloud.tencent.com/)、 [华为云](https://www.huaweicloud.com/)、 [京东云](https://www.jdcloud.com/)，同时如果可以选择，注意服务器的所在地，物理距离会影响访问速度，建议根据目标客户的所在地选择如华北、华东等（无法选择也无需太过在意，影响不大）。
2. 在域名注册商处**域名实名认证**，过程很快，但系统进行**数据同步**需要差不多两个工作日
3. **发起 ICP 备案**（主体 + 域名 + 服务器绑定）：此过程最为繁琐，在服务器提供商的备案系统操作（约一个工作日），需要上传域名证书、打印并签署互联网信息服务负责人责任告知书、互联网信息服务负责人真实性承诺书
4. 收到短信 24h 内进行 “**短信核验**”
5. 收到备案号 30天 内进行**公安备案**（月两个工作日）

在**页脚版权处**添加公安备案编号图案及公安备案号和ICP备案号，如下所示：
```text
© 2025 农粮运输装置竞赛展示平台|
  <img src="http://sighingfield.tech/wp-content/uploads/2025/10/备案图标.png" alt="备案图标" style="width: 16px;height: 16px;vertical-align: middle;margin-right: 4px" /> <a href="https://beian.mps.gov.cn/#/query/webSearch?code=32010402002278" rel="noreferrer" target="_blank">苏公网安备32010402002278号</a>
 <a href="https://beian.miit.gov.cn/">苏ICP备2025210964号-1 </a>
 ```
{%note danger %}
未成功备案获取备案号前，域名不可进行**解析**，进行网站部署！
{%endnote%}

{%fold into@对于网站用途、开办内容的陈述如下（避免被打回耽误时间）%}
**基本格式**为：本网站为个人网站......网站开通后的主要内容为......
```text
本网站为个人网站，做参加学校三创赛等竞赛等成果展示界面使用；
网站开通后的主要内容为个人的学习心得与笔记和对学校所做竞赛项目进度的跟进及优势介绍与现有成果展示。
```
{%note info %}
如果注册多个域名，用途相似，记得进行修改，网站开办内容填写时不可和之前的相同，否则会被初审退回！
{%endnote%}
{%endfold%}


{%note danger %}
注意**个人建站需要符合个人性质**，不能出现**企业**、**平台**、**项目**等字样，不可出现电话“随时致电”等字样，且要在首页底部展示**ICP备案号**，点击可以直接**跳转到工信部**，否则后续注册的域名会被打回，填写个人建站方案书。
{%endnote%}

中间可能需要完成**个人网站建设方案书**


{%fold into@相关问题与回答模版%}

{%fold into@一、网站服务内容介绍（包含网站内容截图或者设计图、网站栏目及内容介绍、多网站/域名用途和域名拓展使用情况）：%}
&emsp;&emsp;这是个人网站，专为学校竞赛项目打造的展示平台，仅用于竞赛展示，无任何盈利目的，平台内所有商品均为虚构系学生成果展示，以下是网站建设主页面的封面图。
{%endfold%}

{%fold into@二、组网方案（包含设备配置、组网结构、使用技术和部署情况）：%}
&emsp;&emsp;本网站建站的云服务器BCC由百度智能云提供，域名由阿里云提供，云服务器位于华北 - 北京，可用区D ，系统采用的是Debian / 12.0.0 amd64 (64bit)，采用wordpress建站。

{%note info %}
相关内容再服务器提供商处进行查询！
{%endnote%}
{%endfold%}

{%fold into@三、网站安全与信息安全管理制度（包含网络安全防御措施、信息安全管控制度和应急处理方案）：%}
&emsp;&emsp;用百度智能云的安全中心服务，建立主机安全防御体系，防止病毒勒索，以保证网站的正常运行，网站日志留存时间设置为180天。
{%endfold%}

{%fold into@四、承诺书%}
&emsp;&emsp;本网站是个人网站，本人承诺网站不含有企业、单位等非个人网站的信息，网站实际开办内容与备案信息一致，如后续发现有违反以上承诺的行为，或者域名有交易行为、网站内容涉及互联网信息服务管理“九不准”等违法违规内容，愿意接受接入服务商关闭网站、主管部门注销备案并列入黑名单等处罚。
{%endfold%}

{%endfold%}

在域名注册商控制台进行解析即可，解析到服务器IP地址，即可进行访问！
{%note info%}
#### 使用二级域名搭建多个网站
**DNS解析配置（在域名注册商处操作）** 
为 domain-a.com 设置二级域名 app.domain-a.com
操作步骤（以阿里云/腾讯云等常见控制台为例）：
- 登录你的域名注册商管理后台。
- 找到域名解析管理页面。
- 选择你要解析的域名（例如 domain-a.com），点击“添加记录”。
  - 记录类型：选择 A。
  - 主机记录：填写你的二级域名前缀。比如想要 app.domain-a.com，这里就填 app。
  - 记录值：填写你的服务器公网IP地址 111.222.333.444。
  - TTL：默认即可（通常10分钟）。
{%endnote%}
为保障网络访问的安全性，可在域名注册商处签发SSL证书（2min），将网站升级为HTTPS协议，然后在1Panel面板手动导入`key``pem`文件！




2025.10.25-2025.11.18进入审查阶段

2025.11.18小程序备案完成

2025.11.19完成微信认证工作 缴纳30r