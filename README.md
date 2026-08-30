# XS-RulesPlus — Shadowrocket 规则集

开箱即用的 Shadowrocket 分流配置，导入后添加节点即可使用。

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

## License

MIT
