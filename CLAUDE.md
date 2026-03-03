# CLAUDE.md

Skills 层不包含业务实现代码，所有功能通过调用 xiaohongshu-mcp 的 MCP 工具完成。

## MCP 工具映射表

| MCP 工具 | 类型 | 对应 Skill | 说明 |
|---|---|---|---|
| `check_login_status` | ReadOnly | xhs-login | 检查登录状态 |
| `get_login_qrcode` | ReadOnly | xhs-login | 获取登录二维码 |
| `delete_cookies` | Destructive | xhs-login | 删除 cookies 重置登录 |
| `publish_content` | Destructive | post-to-xhs | 发布图文笔记 |
| `publish_with_video` | Destructive | post-to-xhs | 发布视频笔记 |
| `list_feeds` | ReadOnly | xhs-explore | 获取推荐流 |
| `search_feeds` | ReadOnly | xhs-search | 搜索笔记 |
| `get_feed_detail` | ReadOnly | xhs-explore | 获取笔记详情和评论 |
| `user_profile` | ReadOnly | xhs-profile | 获取用户主页 |
| `like_feed` | Destructive | xhs-interact | 点赞/取消点赞 |
| `favorite_feed` | Destructive | xhs-interact | 收藏/取消收藏 |
| `post_comment_to_feed` | Destructive | xhs-interact | 发表评论 |
| `reply_comment_in_feed` | Destructive | xhs-interact | 回复评论 |

## SKILL.md 编写规范

每个 SKILL.md 包含 YAML frontmatter（name + description）和 Markdown 正文。

正文必须包含：输入判断、约束条件、执行流程（含 MCP 工具调用）、失败处理。

编写原则：
- 控制在 200 行以内
- Destructive 操作需用户确认
- 工具名和参数必须与 xiaohongshu-mcp 源码一致

## 参考资源

- **xiaohongshu-mcp 源码**：`~/src/zy/xiaohongshu-mcp`（Go MCP 服务）
