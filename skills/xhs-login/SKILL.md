---
name: xhs-login
description: |
  管理小红书登录状态：检查是否已登录、二维码扫码登录、重置登录切换账号。
  当用户提到登录、扫码、账号、切换账号、退出登录、登录状态检查，或其他 skill 报告"未登录"需要先登录时使用。
---

## 执行流程

### 1. 检查登录状态

调用 `check_login_status`（无参数），返回是否已登录及用户名。

- 已登录 → 告知用户当前登录账号
- 未登录 → 进入步骤 2

### 2. 扫码登录

调用 `get_login_qrcode`（无参数）。MCP 工具返回两部分内容：
- 文本：超时提示（含截止时间）
- 图片：PNG 格式二维码（MCP image content type，Base64 编码）

**展示二维码**：MCP 返回的图片会通过客户端渲染给用户。如果客户端无法直接展示图片（如纯文本终端），则将 Base64 数据保存为临时 PNG 文件，告知用户文件路径让其手动打开：
```bash
# fallback: 保存二维码到临时文件
echo "<base64_data>" | base64 -d > /tmp/xhs-qrcode.png
open /tmp/xhs-qrcode.png   # macOS
xdg-open /tmp/xhs-qrcode.png  # Linux
```

提示用户：
- 打开小红书 App 扫描二维码
- 二维码有效期有限，过期需重新获取

扫码完成后，调用 `check_login_status` 确认登录成功。

### 3. 重新登录 / 切换账号

当用户要求重新登录或切换账号时：

1. 调用 `delete_cookies`（⚠️ 需用户确认）— 清除当前登录状态
2. 调用 `get_login_qrcode` — 获取新二维码
3. 引导用户扫码

## 约束

- `delete_cookies` 会清除登录状态，执行前必须确认
- 登录需要用户手动用手机 App 扫码，无法自动完成

## 失败处理

| 场景 | 处理 |
|---|---|
| MCP 工具不可用 | 引导用户使用 `/setup-xhs-mcp` 完成部署和连接配置 |
| 二维码超时 | 重新调用 `get_login_qrcode` |
