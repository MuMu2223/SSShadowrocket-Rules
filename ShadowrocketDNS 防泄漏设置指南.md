---

# 🛡️ Shadowrocket (小火箭) DNS 防泄漏设置指南

在使用代理网络时，传统的 DNS 解析请求可能会以明文形式发送至本地运营商（ISP），从而暴露你的上网域名轨迹，甚至造成分流异常。本指南将帮助你通过优化 Shadowrocket 设置，彻底切断 DNS 泄漏通道。

---

## 🛠️ 设置步骤

### 一、 开启并配置加密 DNS（DoH / DoT）

传统的 53 端口 UDP DNS 是明文传输的，极易被运营商劫持或监听。改用加密 DNS（DoH）可确保传输过程安全。

1. 打开 **Shadowrocket**，进入 **「设置」 (Settings)** ➔ **「DNS」**。
2. 开启 **「DNS 转发」 (DNS Forwarding)**。
3. 清空或删除原有的默认 DNS，添加以下标准的加密 DNS（DoH）节点：
* `[https://dns.alidns.com/dns-query](https://dns.alidns.com/dns-query)` （阿里 DoH，用于国内域名解析）
* `[https://doh.pub/dns-query](https://doh.pub/dns-query)` （腾讯 DoH，用于国内域名解析）
* `[https://1.1.1.1/dns-query](https://1.1.1.1/dns-query)` （Cloudflare DoH，用于海外域名解析）
* `[https://dns.google/dns-query](https://dns.google/dns-query)` （Google DoH，用于海外域名解析）



---

### 二、 开启远程解析与 DNS 覆盖（核心防泄漏）

确保被分流为 `PROXY`（走代理）的域名**绝对不在本地设备发起解析**，直接交给远端代理节点处理。

1. 返回 **「设置」 (Settings)** ➔ 进入 **「高级」 (Advanced)**（部分版本位于 DNS 子菜单下方）。
2. 将 **「DNS 覆盖」 (Override DNS)** 或 **「根据规则解析 DNS」** 开关设置为 **开启 (ON)**。
3. 确保 **「远程 DNS 解析」 (Remote DNS / Remote Resolve)** 开关保持 **开启 (ON)**。

---

### 三、 禁用 QUIC 协议（防止 UDP 旁路泄漏）

部分浏览器或 App（如 Chrome、YouTube）会尝试使用 QUIC (HTTP/3) 协议绕过系统代理规则，导致请求直接从本地 UDP 泄漏。

1. 进入 **「设置」 (Settings)** ➔ **「UDP」**。
2. 将 **「禁用 QUIC」 (Disable QUIC)** 开关设置为 **开启 (ON)**。

---

### 四、 重载生效

配置完成后：

1. 返回 Shadowrocket **首页**。
2. 确保 **全局路由** 模式为 **「配置」**。
3. 将顶部**总开关关闭，等待 3 秒后重新开启**，完成规则与 DNS 引擎重载。

---

## 🧪 验证 DNS 是否存在泄漏

完成上述设置后，请使用以下方法测试防泄漏效果：

1. 保持 Shadowrocket 连接状态。
2. 使用手机浏览器访问国际通用测试网站：
* 🌐 [BrowserLeaks DNS Test](https://browserleaks.com/dns)
* 🌐 [DNSLeakTest](https://www.dnsleaktest.com/)


3. 点击 **「Standard Test」** 或 **「Extended Test」** 运行测试。

### 🎯 结果判定标准

* **✅ 完全安全（无泄漏）**：
测试结果中显示的 **IP 地址** 和 **Country/ISP（国家与运营商）** 仅包含你当前连接的代理节点信息（如香港、日本、美国等），**列表中没有任何中国大陆（China Telecom / Unicom / Mobile）的记录**。
* **⚠️ 存在泄漏**：
若列表中出现了你所在地区的本地运营商信息，请重新核对“第二步”中的远程解析开关是否正常开启。
