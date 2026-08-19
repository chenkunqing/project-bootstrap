# project-bootstrap

Cursor Agent Skill：新建项目时自动生成 **双文档**协作规范，并支持「只描述要做什么、技术栈一步步补全」。

| 生成文件 | 别名 | 职责 |
|----------|------|------|
| `CLAUDE.md` | **how** | 怎么协作、怎么改、怎么交付 |
| `CODEMAP.md` | **what/where** | 代码在哪、数据怎么流 |

开发哲学默认对齐：**胶水代码、不造轮子、只编排依赖**。

---

## 前置条件

- [Cursor](https://cursor.com) IDE（支持 Agent Skills）
- 推荐搭配 [i-have-adhd](../i-have-adhd) skill（`CLAUDE.md` 开局必读会引用它；没有也能用，跳过即可）

---

## 安装（任意设备）

Skill 需放在 Cursor 可读的个人 skills 目录：

| 系统 | 路径 |
|------|------|
| Windows | `%USERPROFILE%\.agents\skills\project-bootstrap\` |
| macOS / Linux | `~/.agents/skills/project-bootstrap/` |

### 首次安装

```bash
# macOS / Linux
git clone https://github.com/YOUR_USER/project-bootstrap.git ~/.agents/skills/project-bootstrap

# Windows (PowerShell)
git clone https://github.com/YOUR_USER/project-bootstrap.git $env:USERPROFILE\.agents\skills\project-bootstrap
```

克隆后目录结构应为：

```text
~/.agents/skills/project-bootstrap/
├── SKILL.md          # Agent 主指令（勿改名）
├── discovery.md      # 探索式立项流程
├── templates.md      # CLAUDE.md / CODEMAP.md 模板
└── README.md         # 本文件（给人看）
```

重启 Cursor 或新开 Agent 会话后生效。

### 更新

```bash
cd ~/.agents/skills/project-bootstrap   # Windows 用 $env:USERPROFILE\.agents\skills\project-bootstrap
git pull
```

### 多设备同步

1. 本机 push 到 GitHub
2. 其他设备 `git clone` 或 `git pull` 到同一路径

---

## 使用方法

在 Cursor Agent 对话中 **@ 引用 skill** 或直接说明意图。

### 模式 A：探索式立项（不确定技术栈）— 推荐

只描述要做什么，AI 分 5 阶段逐步确认：意图 → 约束 → 技术栈方案 → 脚手架 → 双文档。

```
@project-bootstrap 我想做一个旅行记账工具，手机电脑都能用，最好能离线
```

```
@project-bootstrap 帮我把 Excel 清洗流程做成一个小工具，只有我自己用
```

每轮会显示 `阶段 X/5`，你确认后再进入下一步。可以说：

- `你推荐` — 采用 AI 推荐方案
- `先别写代码` — 停在技术栈讨论
- `用 React + Vite` — 跳过提案，直接按指定栈搭建

### 模式 B：已知技术栈

```
@project-bootstrap 新建 my-app，技术栈 React 19 + Vite + Express + TypeScript
```

### 模式 C：已有代码，只补文档

```
@project-bootstrap 扫描当前项目，初始化 CLAUDE.md + CODEMAP.md
```

---

## 会生成什么

### 必生成

- **`CLAUDE.md`** — 九节规范（Code Map 维护、核心原则、测试、changelog、Git、AI 约束、类型安全、设计原则、架构概要）
- **`CODEMAP.md`** — 速览、常用脚本、目录树、路由/模块分层、数据流、API（按项目实际裁剪）

### 可选（AI 未反对时默认创建）

- `changelog/` 目录
- `.cursor/rules/i-have-adhd.mdc` — 若你本机已装 i-have-adhd skill

---

## 双文档分工（给 AI 看）

| 文档 | AI 何时读 | 内容 |
|------|-----------|------|
| `CODEMAP.md` | 每个新会话开局 | 结构、路径、数据流 |
| `CLAUDE.md` | 协作规则 | 怎么改、怎么交付、禁止什么 |

`CLAUDE.md` 第一节规定开局顺序：先 `CODEMAP.md`，再 `i-have-adhd` skill。

---

## 仓库内文件说明

| 文件 | 读者 | 说明 |
|------|------|------|
| `SKILL.md` | Cursor Agent | 主 workflow，**发布时必需** |
| `discovery.md` | Agent | 探索式立项 5 阶段细则 |
| `templates.md` | Agent | 双文档完整模板 |
| `README.md` | 人 | 安装与用法（本文件） |

---

## 常见问题

**Q：@project-bootstrap 没反应？**

- 确认路径为 `~/.agents/skills/project-bootstrap/SKILL.md`
- 新开 Agent 会话再试
- 对话里写全：`@project-bootstrap 我想做 …`

**Q：和 voyage-ledger 有什么关系？**

- 无硬依赖。规范从 voyage-ledger 的 `CLAUDE.md` / `CODEMAP.md` 抽象而来，可在任意新项目复用。

**Q：非 TypeScript 项目？**

- Agent 会按 `templates.md` 将第七节替换为对应语言约束，或标注不适用。

**Q：能否只装在某一个项目里？**

- 可以复制到项目内 `.cursor/skills/project-bootstrap/`，但个人目录安装可在所有项目 `@project-bootstrap`。

---

## 发布到 GitHub（维护者）

```bash
cd ~/.agents/skills/project-bootstrap
git init
git add SKILL.md discovery.md templates.md README.md LICENSE
git commit -m "docs: initial project-bootstrap skill"
git remote add origin https://github.com/YOUR_USER/project-bootstrap.git
git push -u origin main
```

将 README 中 `YOUR_USER` 替换为你的 GitHub 用户名。

---

## License

MIT — 见 [LICENSE](LICENSE)
