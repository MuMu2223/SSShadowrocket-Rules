# SSS全场景去广告增强版 v2.1

> 低误杀 · 覆盖开屏广告 · 海外广告 · 木马 / 钓鱼 / 诈骗拦截

---

## 一、模块简介

- **国内主流 App 开屏广告**（百度地图、高德地图、爱奇艺、优酷、微博、美团、抖音等）
- **海外网站广告与追踪器**
- **木马 / 钓鱼 / 诈骗 / 恶意域名**安全拦截
- **严格控制误杀率**，不包含任何银行、证券、支付类规则
- 支持 Shadowrocket 模块导入或直接合并到现有配置

**当前版本**：v2.1  
**更新日期**：2026-09-01  
**适用客户端**：Shadowrocket（小火箭）

---

## 二、使用方法

### 1. 导入方式（推荐）

1. 打开 Shadowrocket → 底部 **「配置」** → **「模块」**
2. 点击右上角 **「+」**
3. 选择「从 URL 下载」或直接粘贴完整规则内容
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
| **国内追踪器** | 友盟、百度统计、CNZZ 等常见国内追踪域名 |
| **国内 App 开屏 / 广告** | 百度地图、高德地图、爱奇艺、优酷、腾讯视频、微博、美团、抖音、喜马拉雅等 |
| **海外广告与 SDK** | Google Ads、AppLovin、Unity Ads、Criteo、Taboola 等 |
| **远程 RULE-SET** | 可选安全与广告增强列表 |

### 2. [URL Rewrite] 部分

针对开屏广告和信息流广告的 URL 路径拦截（需开启 MitM 才生效）：

- 通用开屏关键词（splash / startup / launchad 等）
- 百度地图、高德地图专项开屏接口
- 爱奇艺、B站、微博、美团、优酷等具体路径

### 3. [MITM] 部分

已预置常用需要解密的 hostname（地图、视频、社交、电商等），支持 `%APPEND%` 追加。

---

## 四、设计原则与注意事项

### 设计原则

- **低误杀优先**：避免宽泛关键词（如单纯 `ad`），优先使用精确 DOMAIN / DOMAIN-SUFFIX + 必要 URL-REGEX
- **安全与广告分离**：安全规则放在最前，广告规则在后
- **不触碰敏感业务**：明确排除银行、证券、支付类域名

### 使用注意

1. 开启后建议观察 3～7 天，重点检查地图导航、视频、社交类 App 是否正常
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

## 五、更新与反馈

- 国内 App 广告接口变化较快，建议每隔 1～2 个月检查开屏效果
- 发现漏拦或误杀，可自行在规则前部补充，或反馈具体域名
- 本规则仅供学习与个人使用，请遵守当地法律法规

---

```
