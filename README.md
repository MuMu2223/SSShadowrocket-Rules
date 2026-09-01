# XS-RulesPlus — Shadowrocket 规则大直男

## 1、开箱即用的 Shadowrocket 分流配置，导入后添加节点即可使用。

## 配置文件

- **主配置**: `XS-RulesPlus.conf`
- **规则目录**: `Rules/`（Google、AI、Apple、券商等本地规则）
- **自动更新**: 配置内置 `update-url`，Shadowrocket 可直接拉取最新内容

## 快速开始

1. 复制配置链接：
   ```
   https://raw.githubusercontent.com/MuMu2223/SSShadowrocket-Rules/main/XS-RulesPlus.conf
   ```
2. Shadowrocket → 配置 → 右上角 `+` → 粘贴链接 → 下载
3. 设为使用中（✔️），添加节点或订阅
4. 连通性测试，选择可用节点连接

## 策略组一览

| 策略组 | 默认出口 | 说明 |
|--------|----------|------|
| 🚀 节点选择 | 手动 | 主入口，可选地区分组或 PROXY/DIRECT |
| 🇭🇰 香港节点 | url-test | 自动测速，匹配 HK / 香港等关键词 |
| 🇹🇼 台湾节点 | url-test | 自动测速，匹配 TW / 台湾等关键词 |
| 🇯🇵 日本节点 | url-test | 自动测速，匹配 JP / 日本等关键词 |
| 🇺🇸 美国节点 | url-test | 自动测速，匹配 US / 美国等关键词 |
| 🇸🇬 新加坡节点 | url-test | **新增**，匹配 SG / 新加坡等关键词 |
| 🌐 其他节点 | url-test | 兜底地区分组 |
| 🧱 DNS 防泄露 | REJECT | 拦截 HTTPDNS / 硬编码 DNS |
| 🤖 AI 服务 | 美国节点 | ChatGPT、Claude、Grok 等 |
| 📹 油管视频 | 新加坡节点 | YouTube、Netflix |
| 🔍 谷歌服务 | 日本节点 | Google 搜索、API、Gemini |
| Ⓜ️ 微软服务 | 节点选择 | Office、Azure、Copilot |
| 🍏 苹果服务 | DIRECT | iCloud、App Store、Apple CDN |
| 🍎 苹果推送 | 节点选择 | APNs 推送通知 |
| 📲 电报消息 | 节点选择 | Telegram |
| 🐱 代码托管 | 节点选择 | GitHub、GitLab、Atlassian |
| 🏦 汇丰香港 | 香港节点 | HSBC HK + Reward+ |
| 🏦 香港银行 | DIRECT | 渣打、中银、众安等港银 |
| 📈 券商服务 | 香港节点 | 富途、长桥、老虎、雪盈、盈透、Schwab |
| 🔒 国内服务 | DIRECT | B站、腾讯云、国内 CDN |
| 🌍 非中国 | PROXY | 境外流量兜底 |
| 🐟 漏网之鱼 | PROXY | 最终兜底 |

## 分流规则优先级

规则自上而下匹配，先命中先执行：

| 优先级 | 规则 | 默认策略 |
|--------|------|----------|
| 1 | DNS 防泄露（BlockHttpDNS + hijack-dns） | REJECT |
| 2 | 广告拦截（Advertising 规则集） | REJECT |
| 3 | Netflix | 油管视频 |
| 4 | TikTok | 节点选择 |
| 5 | Twitter / X | 节点选择 |
| 6 | 谷歌服务（含 Gemini） | 日本节点 |
| 7 | AI 服务（ChatGPT / Claude / Grok） | 美国节点 |
| 8 | YouTube | 新加坡节点 |
| 9 | 哔哩哔哩 | DIRECT |
| 10 | 局域网 / 私有网络 | DIRECT |
| 11 | Telegram | 节点选择 |
| 12 | 代码托管（GitHub / GitLab / Atlassian） | 节点选择 |
| 13 | 微软服务 | 节点选择 |
| 14 | 汇丰香港 | 香港节点 |
| 15 | 香港银行（非汇丰） | DIRECT |
| 16 | 券商服务 | 香港节点 |
| 17 | 苹果推送（TCP 5223 + Push 域名） | 节点选择 |
| 18 | 苹果服务（iCloud / App Store / CDN） | DIRECT |
| 19 | 国内服务（China 规则 + GEOIP CN） | DIRECT |
| 20 | 非中国（Global 规则） | PROXY |
| 21 | 漏网之鱼 | PROXY |

## 核心功能

### DNS 防泄露（多层防护）

- **多源 DoH 并发**：主 DNS 三源同时探测（Cloudflare / 1.1.1.1 / security.cloudflare-dns.com），备用双源（Google / 8.8.8.8）
- **硬编码 DNS 劫持**：拦截 `:53` 通配及常见公共 DNS（8.8.8.8、1.1.1.1、Quad9、OpenDNS、阿里、腾讯、114）
- **HTTPDNS 拦截**：通过 BlockHttpDNS 规则集阻止 App 内置 HTTPDNS 绕过系统解析
- **DNS 隔离**：`dns-fallback-system = false`，代理解析不回退系统 DNS；直连域名使用系统 DNS，保障国内 CDN 调度准确
- **防 DNS Rebinding**：`private-ip-answer = true` 阻止私有地址泄露

### 地区分组与自动测速

- 6 个地区分组（港/台/日/美/新/其他），基于节点名称关键词自动归类
- `tolerance = 50`：避免延迟差过小导致频繁切换，保持连接稳定
- `interval = 600s`：每 10 分钟自动测速一次

### 新加坡节点优化

- 新增 `🇸🇬 新加坡节点` 分组，AI 服务和流媒体默认优先使用
- 新加坡节点到亚洲用户延迟低，适合 Netflix、YouTube、Claude 等服务

### 广告与隐私拦截

- 引入 blackmatrix7 Advertising 规则集，拦截主流广告域名
- 可在 Shadowrocket 策略组中将「广告拦截」切换为节点选择以临时关闭

### 香港金融服务分流

- **汇丰香港**：默认香港节点，保障 Reward+ 及移动端体验
- **其他香港银行**：默认 DIRECT，避免代理 IP 变动触发风控
- **券商服务**：覆盖富途、长桥、老虎、雪盈、盈透、Schwab，本地规则收录完整交易域名和 IP 段

### 局域网保护

- `skip-proxy` 排除私有网段（192.168.x.x、10.x.x.x、172.16.x.x）
- `always-real-ip` 确保 *.local、*.lan、localhost 直连系统 DNS
- `*.apple-cloudkit.com`、`*.icloud.com` 等 Apple 服务强制使用系统解析

## 注意事项

- 节点名称需包含地区标识（如 🇭🇰、HK、🇸🇬、SG），否则无法自动归入对应分组
- 香港银行和券商服务对出口 IP 敏感，建议固定使用同一香港节点
- 如需 HTTPS 解密（MITM），请在 Shadowrocket 中生成并安装 CA 证书
- 广告拦截规则会拦截部分第三方资源，如遇页面异常可临时切换「🧱 DNS 防泄露」策略为 DIRECT

---

## 2、全场景去广告增强版 v2.2
SS-ADBlockRules/SSADBlockRulesPlus2.2.module

> 专为 ** iPhone / iPad** 日常使用优化的 Shadowrocket 去广告 + 安全防护模块  
> 低误杀 · 覆盖 各类开屏广告 · 海外广告 · 木马/钓鱼/诈骗拦截

---

## 一、模块简介

- ** 开屏广告拦截**
- **国内主流 App 开屏广告**
- **海外网站广告与追踪器**
- **木马 / 钓鱼 / 诈骗 / 恶意域名**安全拦截
- **严格控制误杀率**，不包含任何银行、证券、支付类规则
- 支持 Shadowrocket 模块导入或直接合并到现有配置

**当前版本**：v2.2  
**更新日期**：2026-09-01  
**适用客户端**：Shadowrocket（小火箭）

---

## 二、使用方法

### 1. 导入方式（推荐）

1. 打开 Shadowrocket → 底部 **「配置」** → **「模块」**
2. 点击右上角 **「+」**
3. 选择「从 URL 下载」或直接粘贴完整规则内容

https://raw.githubusercontent.com/MuMu2223/SSShadowrocket-Rules/refs/heads/main/SS-ADBlockRules/SSADBlockRulesPlus2.2.module

4. 下载 / 保存后，**开启模块开关**，并尽量将本模块放在模块列表靠前位置

> 也可将规则内容直接复制到当前配置文件的 `[Rule]`、`[URL Rewrite]`、`[MITM]` 部分。

### 2. 必须开启 HTTPS 解密（MitM）

去广告（尤其是开屏广告）依赖 HTTPS 解密，请按以下步骤操作：

1. 配置 → 当前配置文件右侧 **ⓘ** → **HTTPS 解密** → 打开开关
2. 点击 **生成新的 CA 证书** → **安装证书**
3. 系统「设置」→「已下载描述文件」→ 安装 Shadowrocket CA
4. 「设置」→「通用」→「关于本机」→「证书信任设置」→ 开启 Shadowrocket CA 信任

完成后重启 Shadowrocket，并彻底杀掉需要去广告的 App 再重新打开。

### 3. 可选远程规则集（安全强化）

在配置的 `[Rule]` 部分可取消注释以下 RULE-SET（建议优先开启安全类）：

```conf
# 木马 / 钓鱼 / 诈骗强化（推荐开启）
RULE-SET,https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/tif.txt,Reject
RULE-SET,https://raw.githubusercontent.com/jarelllama/Scam-Blocklist/main/lists/adblock/scams.txt,Reject

# 广告增强（可选，规则量较大）
# RULE-SET,https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/Advertising/Advertising.list,Reject
```

---

## 三、规则结构说明

### 1. [Rule] 部分

| 分类 | 说明 |
|------|------|
| **安全规则** | 木马、钓鱼、诈骗、恶意域名拦截（关键词 + 高风险后缀 + 精选域名） |
| **12306 专项** | 开屏与广告域名拦截 |
| **地图类开屏** | 百度地图、高德地图、腾讯地图专项 |
| **国内 App 开屏 / 广告** | 优酷（完整）、爱奇艺、腾讯视频、微博、喜马拉雅、美团、京东、抖音等 |
| **广告联盟 / SDK** | 国内 + 海外常见广告联盟与广告 SDK |
| **远程 RULE-SET** | 可选安全与广告增强列表 |

### 2. [URL Rewrite] 部分

针对开屏广告和信息流广告的 URL 路径拦截（**需开启 MitM 才生效**）：

- 通用开屏关键词（splash / startup / launchad 等）
- 12306、百度地图、高德地图专项开屏接口
- 爱奇艺、B站、微博、美团、优酷、喜马拉雅、知乎等具体路径

### 3. [MITM] 部分

已预置常用需要解密的 hostname（地图、视频、社交、电商、12306 等），支持 `%APPEND%` 追加。

---

## 四、设计原则与注意事项

### 设计原则

- **低误杀优先**：避免宽泛关键词，优先使用精确 DOMAIN / DOMAIN-SUFFIX + 必要 URL-REGEX
- **安全与广告分离**：安全规则放在最前，广告规则在后
- **不触碰敏感业务**：明确排除银行、证券、支付类域名
- **开屏优先**：针对国内高频 App 做专项路径拦截

### 使用注意

1. 开启后建议观察 3～7 天，重点检查地图导航、视频、社交、12306 等 App 是否正常
2. 如出现误杀，在规则最前面添加：
   ```conf
   DOMAIN-SUFFIX,被误杀的域名,DIRECT
   ```
3. 部分 App（尤其银行、证券）启用了证书固定（SSL Pinning），开启 MitM 后可能报错，可在 MITM hostname 中用 `-域名` 排除
4. 远程 RULE-SET 数量较大时对性能影响极小（现代 iPhone 几乎无感），但建议先开启安全类，广告类按需启用

### 性能说明

- 本地精选规则 + 2～3 个安全远程列表：日常使用几乎无延迟
- 同时开启大型广告列表（如 blackmatrix7 Advertising）：可能有轻微匹配开销，老旧设备更明显

---

## 五、无 HTTPS 解密时的规则生效情况

| 规则类型 | 是否需要 MitM | 说明 |
|----------|---------------|------|
| `[Rule]` 中的 DOMAIN / DOMAIN-SUFFIX / IP-CIDR | 不需要 | 完全生效 |
| 远程 RULE-SET | 不需要 | 完全生效 |
| `[URL Rewrite]` 路径拦截（开屏等） | **需要** | 全部失效 |
| 12306、百度/高德地图等开屏专项 | **需要** | 全部失效 |

> **结论**：不开启 MitM 只能拦截整个域名，开屏广告去除效果会明显下降。

---

## 六、测试方法

1. **基础测试**：彻底杀掉目标 App 后重新打开，观察开屏广告是否消失（重点测试 12306、百度地图、高德地图、爱奇艺、优酷、微博）
2. **日志测试**：Shadowrocket →「数据」→ 开启记录，查看是否出现大量 REJECT
3. **网页测试**：用 Safari 访问新闻网站，观察广告位是否减少

---

## 七、更新与反馈

- 国内 App 广告接口变化较快，建议每隔 1～2 个月检查开屏效果
- 发现漏拦或误杀，可自行在规则前部补充，或反馈具体域名
- 本规则仅供学习与个人使用，请遵守当地法律法规


## 3、🍎 APNs 苹果推送服务优化模块 (Apns.module)

本模块是专为 iOS / macOS 设备打造的底层网络优化补丁。通过对苹果官方推送服务（APNs）进行精准分流，解决在使用代理过程中常见的**消息延迟、后台断连以及设备异常耗电**等问题。

---

## 🌟 核心作用

### 1. 消除消息延迟，防止后台“假死”

iOS 系统的所有 App（如微信、QQ、网易邮箱等）后台通知均依赖苹果 APNs 服务器（通常基于 `5223` 端口维持长连接）。

* **未优化前**：若 APNs 请求误走代理节点，当节点波动、断流或切换时，推送连接会被切断，导致别人发消息时手机无提醒，必须点开 App 才能刷出消息。
* **挂载本模块后**：强制将 APNs 相关的域名与 IP 段解析为 **直连（DIRECT）**，绕过代理节点，始终通过本地最稳定的宽带/移动网络连接苹果服务器，确保通知秒级到达。

### 2. 降低小火箭后台耗电与发热

长连接如果走代理，代理软件需要频繁发送“心跳包”去维持跨国 TCP 连接，导致系统 CPU 被频繁唤醒。

* **优化效果**：APNs 直连后，长连接托管给 iOS 系统原生网络处理，有效降低 Shadowrocket 在后台的 CPU 占用与电量消耗。

---

## 🔒 安全性说明

* **100% 官方直连**：仅包含苹果官方 APNs 域名的路由分流规则（`DOMAIN-SUFFIX` / `IP-CIDR`）。
* **零解密风险**：无需开启 HTTPS 解密（MITM），不安装任何本地 CA 证书，完全不接触任何个人隐私与账号密码。

---

## 🛠️ 使用方法

### 方式一：直接通过 URL 链接导入（推荐）

1. 打开 **Shadowrocket（小火箭）**。
2. 点击底部导航栏 **「配置」 (Config)** ➔ 选择 **「模块」 (Module)**。
3. 点击右上角的 **`+`** 号。
4. 在弹出的输入框中粘贴以下模块 URL 链接并点击 **下载 / 保存**：
```text
https://raw.githubusercontent.com/MuMu2223/SSShadowrocket-Rules/refs/heads/main/Apns/Apns.module

```


5. 在模块列表中找到 **`Apns.module`**，点击使其处于勾选开启（绿色）状态。
6. 返回小火箭首页，**关闭总开关等待 3 秒后重新开启**即可生效。

---

### 方式二：本地文件导入

1. 将 `Apns.module` 文件下载保存至 iOS **「文件」 App** 的本地目录。
2. 打开 Shadowrocket ➔ 进入 **「配置」** ➔ **「模块」**。
3. 点击右上角 **`+`** 号，选择 **从本地文件导入**。
4. 选中下载好的 `Apns.module` 文件，并在列表中勾选启用。

---

## 💡 最佳配置搭配建议

为了保证最佳的上网体验与系统稳定性，建议将小火箭按以下结构组合使用：

* 📄 **主配置文件 (`.conf`)**：`XS-RulesPlus.conf`（负责全局节点路由与基础分流）
* 🧩 **功能模块 1 (`.module`)**：`Apns.module`（负责苹果推送防延迟与后台省电）
* 🧩 **功能模块 2 (`.module` / `.conf`)**：`SSADBlockRulesPlus2.2.module`（负责高强度 App/网页去广告）

> **📌 内部渲染机制**：小火箭的规则优先权为 **模块 (Module) > 主配置 (Config)**。挂载 `Apns.module` 后，无需修改主配置文件，推送优化规则将自动无缝覆盖并优先执行。



