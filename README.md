# xiaohongshu-mcp-skills

基于 [xiaohongshu-mcp](https://github.com/xpzouying/xiaohongshu-mcp) 的 Agent Skills 集合，为小红书提供完整的自动化操作能力。兼容 [Agent Skills 开放标准](https://agentskills.io)，支持 OpenClaw、Claude Code 等平台。

## 功能

| Skill | 说明 |
|---|---|
| **setup-xhs-mcp** | 安装部署 xiaohongshu-mcp 服务 |
| **xhs-login** | 登录管理（扫码登录、状态检查、重新登录） |
| **post-to-xhs** | 发布图文/视频笔记 |
| **xhs-search** | 搜索笔记（支持多维度筛选） |
| **xhs-explore** | 浏览推荐流、查看笔记详情和评论 |
| **xhs-interact** | 互动操作（点赞、收藏、评论、回复） |
| **xhs-profile** | 查看用户主页和作品 |
| **xhs-content-plan** | 内容策划（热门分析、竞品研究、选题建议） |

## 前置条件

- 已安装并运行 [xiaohongshu-mcp](https://github.com/xpzouying/xiaohongshu-mcp) 服务
- 如未安装，使用 `setup-xhs-mcp` skill 引导完成

## 安装

### OpenClaw

下载本项目到本地后，解压到 OpenClaw 的 SKILLS 目录下重启会话生效。

### Claude Code

```bash
# 项目级别
cp -r skills/ .claude/skills/

# 或全局级别
cp -r skills/ ~/.claude/skills/
```

根 `SKILL.md` 可作为统一入口使用，也可以直接使用各子 skill。

## 使用示例

```
# 首次使用：安装 MCP 服务和登录
/setup-xhs-mcp
/xhs-login

# 搜索和浏览
/xhs-search 美食探店
/xhs-explore

# 发布内容
/post-to-xhs

# 互动
/xhs-interact
```

## 许可证

MIT
