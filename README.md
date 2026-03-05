# 隐信

一个纯客户端、单文件实现的文本内容伪装工具，用于在受限或不可信通道中传输敏感信息。

### 设计目标

在当前网络环境下，许多平台对加密特征、异常编码、特定模式有越来越严格的实时检测与拦截。  
隐信的核心思路是：**将任意文本加密后伪装成自然中文序列**，让内容在表面上看起来像日常对话记录，从而大幅降低被自动识别和拦截的概率。

### 核心机制

- 输入：任意文本 + 共享密钥  
- 处理流程（全部在浏览器本地完成）：
  1. PBKDF2 + SHA-256 高迭代密钥派生  
  2. AES-256-GCM 认证加密（机密性 + 完整性保护）  
  3. deflate 压缩  
  4. base85 编码 + 汉字映射（85 个常用汉字，无填充符）  
- 输出：一串自然中文字符序列  
- 接收方：相同密钥 + 相同流程逆向还原，GCM 标签校验任何篡改

### 安全属性

- 端到端本地运算，无服务器、无上传、无日志  
- 密钥匹配 → 原文可还原  
- 密钥不匹配或内容被修改 → 解析失败  
- 伪装强度：输出为常见汉字组合，无明显加密特征（如 、+、/、base64 模式等）

### 安全边界与使用要求

- 本工具采用**静态对称密钥**，无前向保密：密钥泄露将导致历史消息可追溯  
- 密钥强度直接决定安全性：推荐使用密码管理器生成 25+ 字符随机字符串，或 6–8 词高熵 passphrase  
- 弱密钥（短密码、常见词、模式化组合）存在暴力破解风险  
- 建议：重要通信后立即更换密钥，仅在可信设备手动输入，避免剪贴板嗅探、键盘记录等侧信道攻击

### 适用场景

- 在国内主流即时通讯工具中传输需低调的内容  
- 需要内容表面伪装成“正常中文对话”的场景  
- 不信任第三方加密应用本身可能带来的行为标记或留痕  

隐信不是要取代专业加密工具（如 Signal、Threema），而是提供一种“看起来不像加密”的补充方式。

### 获取与使用

- 在线版本： https://kylerx.ca6.pw/

安全依赖密钥强度与用户正确使用，而非任何隐形信任。

我的Telegram频道：https://t.me/LYLERX_Tech  
**Summary:**
English Version


# YinXin

A pure client-side, single-file text content camouflage tool for transmitting sensitive information over restricted or untrusted channels.

### Design Goal

In the current internet environment, many platforms have increasingly strict real-time detection and blocking of encryption features, abnormal encodings, and specific patterns.  
The core idea of YinXin is: **encrypt any text and camouflage it into natural Chinese sequences**, making the content appear as everyday conversation logs on the surface, thereby significantly reducing the probability of automatic identification and interception.

### Core Mechanism

- Input: arbitrary text + shared key  
- Processing flow (all performed locally in the browser):
  1. PBKDF2 + SHA-256 high-iteration key derivation  
  2. AES-256-GCM authenticated encryption (confidentiality + integrity protection)  
  3. deflate compression  
  4. base85 encoding + hanzi mapping (85 common Chinese characters, no padding)  
- Output: a natural Chinese character sequence  
- Receiver: same key + reverse process, GCM tag verifies any tampering

### Security Properties

- End-to-end local computation, no server, no upload, no logs  
- Correct key → original text can be restored  
- Incorrect key or modified content → parsing fails  
- Camouflage strength: output consists of common hanzi combinations, no obvious encryption features (such as `=`, `+`, `/`, `base64` patterns, etc.)

### Security Boundaries and Usage Requirements

- This tool uses **static symmetric key** scheme, no forward secrecy: key leakage will allow tracing of historical messages  
- Key strength directly determines security: recommend using a password manager to generate 25+ character random strings, or 6–8 word high-entropy passphrases  
- Weak keys (short passwords, common words, patterned combinations) are vulnerable to brute-force attacks  
- Recommendation: immediately change key after important communications, input only on trusted devices, avoid clipboard sniffing, shoulder surfing, and other side-channel attacks

### Applicable Scenarios

- Transmitting low-profile content on mainstream domestic instant messaging tools  
- Scenarios requiring surface disguise as “normal Chinese conversation”  
- Users who distrust third-party encryption applications due to potential behavioral tagging or logging  

YinXin is not intended to replace professional encryption tools (such as Signal, Threema), but provides a supplementary method that “doesn't look like encryption at all.”

### Access and Usage

- Online version: https://kylerx.ca6.pw/
- Offline version: download `index.html` and double-click to open  
- Self-hosting: upload to any server supporting static files  

The project is fully open-source, and the code can be viewed in the browser developer tools.  
Security relies entirely on key strength and correct user practice, not on any hidden trust.

My Telegram channel: https://t.me/LYLERX_Tech
