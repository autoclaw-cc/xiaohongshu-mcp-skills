---
name: setup-xhs-mcp
description: |
  安装部署 xiaohongshu-mcp 服务并配置 MCP 连接，引导用户完成从零到可用的全流程。
  当用户第一次使用小红书功能、提到安装/部署/配置小红书、环境搭建、MCP 服务连接失败、或 check_login_status 等 MCP 工具不可用时使用。
---

项目仓库：https://github.com/xpzouying/xiaohongshu-mcp

## 执行流程

### 1. 检测服务状态

检查 xiaohongshu-mcp 是否已在运行：

```bash
curl -sf http://localhost:18060/mcp -o /dev/null && echo "running" || echo "not running"
```

- 已运行 → 记录地址 `http://localhost:18060/mcp`，跳到步骤 3
- 未运行 → 询问用户：服务是否部署在其他地址/端口？
  - 用户提供地址 → 验证可达后跳到步骤 3
  - 未部署 → 进入步骤 2

### 2. 部署服务

确认操作系统（macOS / Linux / Windows）和是否已安装 Docker。

#### 方式一：Docker Compose（推荐）

Docker 镜像内置 Chrome 和中文字体，无需额外配置。

```bash
# 下载 docker-compose.yml
wget https://raw.githubusercontent.com/xpzouying/xiaohongshu-mcp/main/docker/docker-compose.yml

# 启动服务
docker compose up -d

# 查看日志
docker compose logs -f
```

镜像源：
- Docker Hub（默认）：`xpzouying/xiaohongshu-mcp`
- 阿里云（国内推荐）：`crpi-hocnvtkomt7w9v8t.cn-beijing.personal.cr.aliyuncs.com/xpzouying/xiaohongshu-mcp`

切换方法：编辑 docker-compose.yml，注释默认 image 行，取消注释阿里云 image 行。

数据持久化：
- `./data` — cookies 登录状态
- `./images` — 发布图片时的挂载目录

#### 方式二：下载二进制

从 GitHub Releases 下载：https://github.com/xpzouying/xiaohongshu-mcp/releases/latest

```bash
curl -s https://api.github.com/repos/xpzouying/xiaohongshu-mcp/releases/latest | grep browser_download_url
```

注意：二进制方式需要本机已安装 Chrome 或 Chromium。

#### 方式三：源码编译

不推荐，仅适合 Go 开发者。参考项目仓库 README。

部署完成后用 curl 确认服务已启动，然后进入步骤 3。

### 3. 检测 MCP 连接配置

检查当前客户端是否已配置 xiaohongshu MCP 连接。

**Claude Code**：读取 `~/.claude/settings.json` 和项目级 `.claude/settings.json`，查找 `mcpServers` 中是否有 `xiaohongshu` 配置。

- 已配置且地址正确 → 跳到步骤 5
- 已配置但地址不匹配 → 修正地址
- 未配置 → 进入步骤 4

### 4. 配置 MCP 连接

询问用户：

**连接地址**：如果步骤 1 已确认可达的地址，用该地址作为默认值，否则默认 `http://localhost:18060/mcp`。

**使用的客户端**：

**Claude Code**：

```bash
claude mcp add xiaohongshu --transport http <地址>
```

或写入配置文件：
- 全局：`~/.claude/settings.json`
- 项目级：`.claude/settings.json`

```json
{
  "mcpServers": {
    "xiaohongshu": {
      "url": "http://localhost:18060/mcp"
    }
  }
}
```

**Cursor**（`.cursor/mcp.json`）：

```json
{
  "mcpServers": {
    "xiaohongshu": {
      "url": "http://localhost:18060/mcp"
    }
  }
}
```

**其他客户端**：告知用户 MCP 服务地址，让用户按客户端文档自行配置。

### 5. 验证与提示

1. **提示用户重启当前会话** — MCP 配置变更后需重启客户端才能加载新的 MCP 工具
2. 重启后调用 `check_login_status` 验证连接正常
3. 验证成功 → 引导用户使用 `/xhs-login` 完成扫码登录

## 环境变量（可选）

- `XHS_PROXY` — HTTP/HTTPS/SOCKS5 代理地址
- `ROD_BROWSER_BIN` — 自定义 Chromium 路径
- `HEADLESS` — 无头模式开关

## 失败处理

| 场景 | 处理 |
|---|---|
| Docker 未安装 | 建议安装 Docker 或改用二进制方式 |
| 国内拉取镜像慢 | 切换到阿里云镜像源 |
| 端口 18060 被占用 | 检查已有进程，或更换端口 |
| Chrome 未安装（二进制方式） | 引导安装 Chrome 或改用 Docker 方式 |
| 配置写入后工具仍不可用 | 提示重启客户端会话 |
| 已有配置但地址错误 | 修正地址并重启 |
