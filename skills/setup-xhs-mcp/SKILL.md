---
name: setup-xhs-mcp
description: |
  安装部署 xiaohongshu-mcp 服务，引导用户完成从零到可用的全流程（安装、配置 MCP 连接、验证）。
  当用户第一次使用小红书功能、提到安装/部署/配置小红书、环境搭建、MCP 服务连接失败、或任何小红书工具尚未就绪的场景时使用。
---

项目仓库：https://github.com/xpzouying/xiaohongshu-mcp

## 执行流程

### 1. 检查环境

确认当前系统：
- 操作系统（macOS / Linux / Windows）
- 是否已安装 Docker（优先推荐）
- 是否已有 xiaohongshu-mcp 服务在运行（检查端口 18060）

### 2. 安装 xiaohongshu-mcp

#### 方式一：Docker Compose（推荐）

Docker 镜像内置了 Chrome 和中文字体，无需额外配置。

```bash
# 下载 docker-compose.yml
wget https://raw.githubusercontent.com/xpzouying/xiaohongshu-mcp/main/docker/docker-compose.yml

# 启动服务
docker compose up -d

# 查看日志
docker compose logs -f

# 停止服务
docker compose stop

# 更新到最新版本
docker compose pull && docker compose up -d
```

docker-compose.yml 中的镜像源默认为 Docker Hub，国内用户如果拉取慢，可以切换到阿里云镜像：

- Docker Hub：`xpzouying/xiaohongshu-mcp`
- 阿里云镜像（国内推荐）：`crpi-hocnvtkomt7w9v8t.cn-beijing.personal.cr.aliyuncs.com/xpzouying/xiaohongshu-mcp`

切换方法：编辑 docker-compose.yml，注释默认 image 行，取消注释阿里云 image 行。

数据持久化：
- `./data` — cookies 登录状态
- `./images` — 发布图片时的挂载目录

#### 方式二：下载二进制

从 GitHub Releases 页面下载对应平台的可执行文件：
https://github.com/xpzouying/xiaohongshu-mcp/releases/latest

或用命令行查看最新版本：
```bash
curl -s https://api.github.com/repos/xpzouying/xiaohongshu-mcp/releases/latest | grep browser_download_url
```

下载后直接运行即可。注意：二进制方式需要本机已安装 Chrome 或 Chromium。

#### 方式三：源码编译

不推荐，仅适合有 Go 开发经验的用户。请参考项目仓库 README。

### 3. 配置 MCP 连接

根据用户使用的客户端配置 MCP：

**Claude Code（~/.claude/settings.json）**：
```json
{
  "mcpServers": {
    "xiaohongshu": {
      "url": "http://localhost:18060/mcp"
    }
  }
}
```

**OpenClaw**：按 OpenClaw 的 MCP 配置方式添加上述 URL。

### 4. 验证

调用 `check_login_status` 验证 MCP 服务已连接并正常工作。

验证成功后，引导用户按 xhs-login 完成扫码登录。

## 环境变量（可选）

- `XHS_PROXY` — HTTP/HTTPS/SOCKS5 代理地址
- `ROD_BROWSER_BIN` — 自定义 Chromium 路径
- `HEADLESS` — 无头模式开关

## 失败处理

| 场景 | 处理 |
|---|---|
| Docker 未安装 | 建议安装 Docker 或改用二进制方式 |
| 国内拉取镜像慢 | 切换到阿里云镜像源 |
| 端口 18060 被占用 | 检查已有进程，或在 docker-compose.yml 中更换端口 |
| Chrome 未安装（二进制方式） | 引导安装 Chrome 或改用 Docker 方式 |
