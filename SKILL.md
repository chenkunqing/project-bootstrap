---
name: project-bootstrap
description: >-
  Bootstrap new projects with CLAUDE.md (how — collaboration rules) and
  CODEMAP.md (what/where — code structure map) using the user's standard
  multi-AI workflow. Use when creating a new project, initializing a repo,
  or when the user mentions project-bootstrap, CLAUDE.md, CODEMAP.md, or
  "how/what" project docs. If the user describes goals but not tech stack,
  run Discovery Mode (see discovery.md) to propose stack step by step.
---

# project-bootstrap

为新项目生成 **双文档**协作规范，标准对齐用户 voyage-ledger 实践：

| 文件 | 别名 | 职责 |
|------|------|------|
| `CLAUDE.md` | how | 怎么协作、怎么改、怎么交付 |
| `CODEMAP.md` | what/where | 代码在哪、数据怎么流 |

**禁止**：空模板占位、引用其他项目文件夹、文档路径与代码不一致。

---

## 触发与输入

用户典型说法：

- `@project-bootstrap 新建 [项目名]，技术栈 [X]` — **已知栈**，直接执行
- `@project-bootstrap 我想做一个 [描述]` — **不确定栈**，走 [Discovery Mode](discovery.md)
- `初始化 CLAUDE.md + CODEMAP.md`

### 模式选择

| 条件 | 模式 |
|------|------|
| 用户未给 / 不确定技术栈 | **Discovery Mode** → 读 [discovery.md](discovery.md) |
| 用户已指定技术栈 | 下方「已知栈」流程 |

**Discovery Mode 概要**：意图 → 约束 → AI 提案 2～3 套栈（推荐放第一）→ 用户确认 → scaffold → CLAUDE.md + CODEMAP.md。每阶段确认后再下一步。

---

### 已知栈：开工前确认

缺则询问，有则直接用：

1. 项目名
2. 技术栈（前端/后端/语言/框架）
3. 开发哲学（默认：胶水代码、不造轮子、只编排依赖）
4. 是否使用 TypeScript（决定 CLAUDE.md 第七节是否保留）
5. 是否有 UI / 后端 API（决定 CODEMAP 路由/API 章节深度）

---

## 执行顺序

```
1. Scaffold 项目（或扫描已有目录）
2. 根据实际文件树生成 CODEMAP.md
3. 根据技术栈填写 CLAUDE.md 第九节「架构概要」
4. 校验两份文档路径与 package.json / 入口文件一致
5. 告知用户：开局 AI 会先读 CODEMAP + i-have-adhd skill
```

**CODEMAP 必须在代码存在后写**；先 scaffold 再文档。

---

## 生成 CLAUDE.md

按 [templates.md](templates.md) 中 **CLAUDE.md 模板** 生成，要求：

- 九节结构完整，节名与顺序不变
- 第一节「开局必读」固定：① `CODEMAP.md` ② `~/.agents/skills/i-have-adhd/SKILL.md`
- 开发哲学 blockquote 使用用户默认文案（除非用户指定其他）
- 第七节「TypeScript 类型安全」：非 TS 项目整节删除，改为对应语言的类型/ lint 约束（如有）
- 第九节「架构概要」：按本项目实际填写，5–10 行 bullet

---

## 生成 CODEMAP.md

按 [templates.md](templates.md) 中 **CODEMAP.md 模板** 生成，要求：

- 文首「速览」≤10 行：入口、持久化、后端、类型源、关键外部服务
- 「常用脚本」来自 `package.json` / `pyproject.toml` / `Makefile` 等真实脚本
- 环境变量只列项目中实际使用的
- 根目录树与仓库一致（每行带一行职责注释）
- 有 UI → 写路由/页面表；无 UI → 写 CLI 子命令或模块入口
- 有后端 → 写 API 端点表 + 数据流
- 复杂领域模块 → 可选一节「模块说明」（数据模型 + 初始化 + 入口），参照行李清单模式
- 测试：如实写「有/无」及框架名

**省略规则**：不存在的层（如 hooks/、services/）不写空表；改为本项目实际分层。

---

## 可选配套

用户未反对时，建议同时创建：

| 文件 | 用途 |
|------|------|
| `changelog/` | 空目录，配合 CLAUDE.md 第四节 |
| `.cursor/rules/i-have-adhd.mdc` | `alwaysApply: true`，指向 i-have-adhd skill |

`.cursor/rules/i-have-adhd.mdc` 内容：

```markdown
---
description: Always follow the i-have-adhd skill
alwaysApply: true
---

始终遵循已安装的 skill：`~/.agents/skills/i-have-adhd/SKILL.md`（`/i-have-adhd`）。用户说 `stop adhd mode` 或 `normal mode` 时关闭。
```

---

## 完成校验

交付前逐项检查：

- [ ] `CLAUDE.md` 九节齐全（或 TS 节已替换为对应语言节）
- [ ] `CODEMAP.md` 速览、脚本、目录树、数据流已填
- [ ] 文档中每条路径在仓库中存在（或标注「规划未实现」）
- [ ] 路由表无重复行、无已废弃路径
- [ ] 未复制其他项目的业务域名/路由/API

---

## 交付话术

完成后只告知：

1. 已创建的文件列表
2. 新项目 AI 开局流程：读 `CODEMAP.md` → 读 i-have-adhd skill
3. 后续结构性变更需同步更新 `CODEMAP.md`

---

## 模板

- 完整文件模板：[templates.md](templates.md)
- 探索式立项（不确定技术栈）：[discovery.md](discovery.md)
