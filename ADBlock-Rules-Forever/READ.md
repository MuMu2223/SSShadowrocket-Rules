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

在 Shadowrocket 中添加模块时，请使用以下 URL 链接：

```text
https://raw.githubusercontent.com/MuMu2223/SSShadowrocket-Rules/refs/heads/main/ADBlock-Rules-Forever/ADBlock-Rules.conf
