---
name: xhs-explore
description: |
  浏览小红书推荐流、查看笔记详情和评论。
  当用户想看推荐内容、刷首页、查看某条笔记的详情/评论、或已有 feed_id 想获取完整内容时使用。
---

## 输入判断

- 用户想浏览推荐 → 步骤 1
- 用户提供了 feed_id → 步骤 2

## 执行流程

### 1. 获取推荐流

调用 `list_feeds`（无参数），返回首页推荐笔记列表。

展示每条笔记的标题、作者、互动数据，附带 `feed_id` 和 `xsec_token`。

### 2. 查看笔记详情

调用 `get_feed_detail`：
- `feed_id`（string，必填）
- `xsec_token`（string，必填）
- `load_all_comments`（bool，可选，默认 false，仅返回前 10 条评论）
- `limit`（int，可选，load_all_comments=true 时生效，默认 20）
- `click_more_replies`（bool，可选，是否展开二级回复）
- `reply_limit`（int，可选，跳过回复数超过此值的评论，默认 10）
- `scroll_speed`（string，可选：slow | normal | fast）

展示：笔记内容、图片、作者信息、互动数据、评论列表。

提示用户可以：
- 点赞/收藏（使用 xhs-interact）
- 发表评论（使用 xhs-interact）
- 查看作者主页（使用 xhs-profile）

## 失败处理

| 场景 | 处理 |
|---|---|
| 未登录 | 引导使用 xhs-login |
| 笔记已删除或不可见 | 告知用户该笔记无法访问 |
