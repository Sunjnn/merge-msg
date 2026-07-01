---
name: merge-msg
description: Generate a merge/PR title and description from branch diff. Use when the user says "生成合并信息", "generate merge message", or "生成 PR 描述".
---

# Generate Merge Message

## Steps

1. **Ask the user** for the base branch or base commit. Do NOT guess or infer — always ask first, even if `main` or `master` exists.
2. Run `git branch --show-current` to get the current branch.
2. Run `git log <base>..<current> --oneline` to list commits on this branch.
3. Run `git diff <base>...<current>` to read the full diff.
4. If no commits or diff are found, tell the user there is nothing to merge.
5. Generate a **title** and a **description**, then output both. Do NOT run any git merge or PR creation command.

## Title format

- Same convention as commit messages: start with a conventional prefix (`feat`, `fix`, `refac`, `style`, `doc`, `test`, `perf`, `revert`, `chore`, `ci`, `build`, etc.)
- English, one line, as short as possible
- Use backticks for code references when helpful, but not required
- No trailing period

### Title examples

```
feat: add user avatar upload
fix: resolve race condition in payment callback
refac: split `OrderService` into command and query
```

## Description format

- Markdown, written in Chinese
- Structure:
  1. **概述**: 1–2 sentences summarizing the purpose of this merge.
  2. **主要变更**: a bullet list of key changes, grouped by topic if needed.
  3. **影响范围**: which modules / services / pages are affected.
  4. **注意事项** (optional): anything reviewers should pay attention to — breaking changes, migration steps, performance implications, etc.

### Description example

````markdown
## 概述

实现用户头像上传功能，支持裁剪和压缩后存储至 OSS。

## 主要变更

- 新增 `AvatarUploader` 组件，支持拖拽和点击上传
- 后端新增 `/api/user/avatar` 接口，处理图片校验与上传
- 用户表增加 `avatar_url` 字段，附带数据库迁移脚本

## 影响范围

- 前端：个人设置页
- 后端：用户服务、OSS 工具模块
- 数据库：`users` 表结构变更

## 注意事项

- 需要先执行数据库迁移 `migrate_20260701_add_avatar_url.sql`
- OSS bucket 需配置跨域规则
````
