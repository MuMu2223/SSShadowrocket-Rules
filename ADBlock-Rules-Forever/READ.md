# 🛡️ Shadowrocket 纯去广告模块 (AdBlock Only Module)

本项目基于 [Shadowrocket-ADBlock-Rules-Forever](https://github.com/johnshall/Shadowrocket-ADBlock-Rules-Forever) 进行 Fork 与个人日常维护，专为 **Shadowrocket（小火箭）** 提供独立、纯粹的广告拦截模块。

---

## 📌 项目特点

* 🎯 **纯粹去广告**：仅包含域名拦截规则（`REJECT`），不含任何节点策略或翻墙分流。
* 🧩 **无缝叠加使用**：作为**模块（Module）**引入，能够完美叠加在现有的主配置文件（如 `lazy_group.conf`）之上。
* 🔒 **安全且原生**：仅基于 DNS/域名匹配过滤广告与追踪脚本，**无需开启 MITM，无需安装 CA 根证书**，零隐私安全风险。
* ⚡ **低耗能无感**：本地高效算法匹配，拦截广告反而能节省流量与提升页面加载速度。

---

## 🔗 模块订阅链接

在 Shadowrocket 中添加模块时，下载一下 URL 链接：

https://raw.githubusercontent.com/MuMu2223/SSShadowrocket-Rules/refs/heads/main/ADBlock-Rules-Forever/ADBlock-Rules.conf

优化版

https://raw.githubusercontent.com/MuMu2223/SSShadowrocket-Rules/refs/heads/main/ADBlock-Rules-Forever/SS-ADBlockRules.conf


```text

---

🛠️ 安装与使用教程
为了保证分流规则和防泄漏策略正常工作，请严格按照以下步骤将本模块叠加至当前的主配置文件上：

第一步：添加去广告模块
打开 Shadowrocket（小火箭） App。

点击底部导航栏的 「配置」。

找到并点击 「模块」（部分版本位于「配置」页面上方或右上角）。

点击右上角的 + 号。

选择 「从 URL 下载」。

粘贴上面的模块订阅链接，点击 「下载」。

下载完成后，在模块列表中勾选/开启下载好的ADBlock-Rules模块。

💡 工作原理：
该模块会直接叠加在你当前生效的主配置（例如 lazy_group.conf）之上。原本的分流节点、防泄漏策略依然保持生效，模块仅额外拦截广告。

第二步：重启服务使模块生效
返回 Shadowrocket 「首页」。

将顶部的 小火箭开关关闭，再重新打开（完成一次断开重连）。

确保首页的 「全局路由」 依然保持为 「配置」 模式。

❓ 常见问题 FAQ
⚠️ 免责声明
本项目规则收集自网络开源去广告列表，仅供个人研究、测试与学习使用。

请勿将本项目用于任何商业用途。

---

优化版补充说明：
共删除：23 条（含1条重复的 google-analytics.com）域名

影响的功能
1	ota.pay.mobile.sina.cn	新浪支付
2	pay.mobile.sina.cn	新浪支付
3	cm.passport.iqiyi.com	爱奇艺登录
4	im-x.jd.com	京东IM
5	hotchat-im.iqiyi.com	爱奇艺聊天
6	location.ximalaya.com	喜马拉雅定位
7	static.tieba.baidu.com	百度贴吧静态资源
8	besc.baidustatic.com	百度静态资源
9	dup.baidustatic.com	百度静态资源
10	ubmcmm.baidustatic.com	百度静态资源
11	ubmcvideo.baidustatic.com	百度静态资源
12	push.m.youku.com	优酷推送
13	dev-push.m.youku.com	优酷开发版推送
14	cloudpush.iqiyi.com	爱奇艺推送
15	mobilemsg.youku.com	优酷消息
16	msg.youku.com	优酷消息
17	newspush.sinajs.cn	新浪新闻推送
18	google-analytics.com	Google统计（重复2条）
19	hm.baidu.com	百度统计
20	zhihu-analytics.zhihu.com	知乎统计
21	gorgon.youdao.com	有道验证码
22	impservice.youdao.com	有道图片服务
