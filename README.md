# project-bootstrap

Cursor Agent Skill：**新建任意项目**时自动生成 AI 协作文档，并支持**只描述要做什么、技术栈由 AI 一步步帮你补全**。

| 生成文件 | 别名 | 回答的问题 |
|----------|------|------------|
| [`CLAUDE.md`](templates.md) | **how** | 怎么协作、怎么改、怎么交付 |
| [`CODEMAP.md`](templates.md) | **what/where** | 代码在哪、数据怎么流 |

默认开发哲学：**胶水代码、不造轮子、只编排依赖**（规范内嵌于模板，不依赖任何特定项目文件夹）。

---

## 快速开始（3 步）

1. **安装 skill**（见下方「安装」）到 `~/.agents/skills/project-bootstrap/`
2. **新开 Cursor Agent 会话**
3. **发送下面任一指令**

```
@project-bootstrap 我想做一个 [用自然语言描述要做什么]
```

不确定技术栈时用上面这句；已确定栈时用：

```
@project-bootstrap 新建 my-app，技术栈 React + Vite + Express + TypeScript
```

---

## 使用方法

### 模式 A：探索式立项（推荐，不用懂技术栈）

你只描述**目标和场景**，AI 分 **5 个阶段**推进，**每阶段你确认后再继续**：

| 阶段 | AI 做什么 | 你要做什么 |
|------|-----------|------------|
| 1/5 意图 | 提炼：做什么、给谁用、什么形态 | 确认或纠正 |
| 2/5 约束 | 一次问一个：离线？登录？部署？ | 选 1 / 2 / 3 或补充 |
| 3/5 技术栈 | 给 2～3 套方案 + **推荐** | 选一套或说「你推荐」 |
| 4/5 脚手架 | 建最小可跑项目 | 可选：`npm run dev` 试一下 |
| 5/5 双文档 | 生成 `CLAUDE.md` + `CODEMAP.md` | 完成 |

**示例指令：**

```text
@project-bootstrap 我想做一个旅行记账工具，手机电脑都能用，最好能离线
```

```text
@project-bootstrap 帮我把 Excel 清洗流程做成小工具，只有我自己用
```

**过程中你可以说：**

| 你说 | 效果 |
|------|------|
| `你推荐` | 采用 AI 推荐的方案 |
| `先别写代码` | 停在技术栈讨论，不出代码 |
| `用 React + Vite` | 跳过提案，按指定栈搭建 |
| `stop adhd mode` | 关闭 i-have-adhd 输出格式（若已安装） |

---

### 模式 B：已知技术栈

技术栈已定时，直接 scaffold + 双文档：

```text
@project-bootstrap 新建 voyage-notes，技术栈 React 19 + Vite 6 + Express + TypeScript
```

---

### 模式 C：已有代码，只补文档

项目已存在，缺少协作文档：

```text
@project-bootstrap 扫描当前项目，初始化 CLAUDE.md + CODEMAP.md
```

---

## 会生成什么

### 必生成

**`CLAUDE.md`**（九节，节名固定）

1. Code Map 维护（开局必读 `CODEMAP.md` + i-have-adhd skill）
2. 核心原则（中文、极简、复用依赖）
3. 开发与测试（依赖必须真实调用）
4. pd.md / changelog 协议
5. Git 提交与交付
6. AI 输出约束 + 规则优先级
7. TypeScript 类型安全（非 TS 项目会替换或标注不适用）
8. 设计原则（KISS、职责边界）
9. 架构概要（按本项目填写）

**`CODEMAP.md`**（按项目裁剪）

- 速览（≤10 行）
- 常用脚本 + 环境变量 + 测试情况
- 根目录树
- 路由 / 模块入口
- 模块分层、数据流
- API 端点（如有后端）

### 可选（默认会创建，除非你说不要）

- `changelog/` 空目录
- `.cursor/rules/i-have-adhd.mdc`（配合 [i-have-adhd](https://github.com/search?q=i-have-adhd+cursor+skill&type=repositories) skill）

---

## 安装

Skill 必须放在 Cursor 个人 skills 目录（**所有项目共用**）：

| 系统 | 路径 |
|------|------|
| Windows | `%USERPROFILE%\.agents\skills\project-bootstrap\` |
| macOS / Linux | `~/.agents/skills/project-bootstrap/` |

### 首次安装（从 GitHub）

将 `YOUR_USER` 换成你的 GitHub 用户名：

```bash
# macOS / Linux
git clone https://github.com/YOUR_USER/project-bootstrap.git ~/.agents/skills/project-bootstrap

# Windows PowerShell
git clone https://github.com/YOUR_USER/project-bootstrap.git "$env:USERPROFILE\.agents\skills\project-bootstrap"
```

安装后**新开 Agent 会话**，对话里输入 `@project-bootstrap` 应能引用到本 skill。

### 更新

```bash
cd ~/.agents/skills/project-bootstrap    # Windows: cd $env:USERPROFILE\.agents\skills\project-bootstrap
git pull
```

### 多设备同步

```text
设备 A：改 skill → git commit → git push
设备 B：git pull（同一路径）
```

每台设备的安装路径必须一致，Cursor 才能识别。

---

## 仓库文件说明

| 文件 | 给谁看 | 作用 |
|------|--------|------|
| `SKILL.md` | **Cursor Agent** | 主 workflow（**勿改名**） |
| `discovery.md` | Agent | 探索式立项 5 阶段细则 |
| `templates.md` | Agent | `CLAUDE.md` / `CODEMAP.md` 完整模板 |
| `README.md` | **人** | 安装与用法（本文件） |
| `LICENSE` | 人 | MIT |

---

## 前置条件

- [Cursor](https://cursor.com) IDE（支持 Agent Skills）
- 推荐搭配 **i-have-adhd** skill（`CLAUDE.md` 开局必读会引用；未安装也可跳过）

---

## 常见问题

**@project-bootstrap 没反应？**

1. 确认存在 `~/.agents/skills/project-bootstrap/SKILL.md`
2. 新开 Agent 会话
3. 写完整：`@project-bootstrap 我想做 …`

**必须和 voyage-ledger 在同一台机器吗？**

不需要。规范已内嵌在 `templates.md`，任意新项目可用。

**能否只装在某个项目里？**

可以复制到 `.cursor/skills/project-bootstrap/`，但个人目录安装可在**所有项目** `@project-bootstrap`。

**非 Web / 非 TypeScript 项目？**

探索模式会按 CLI、库、纯后端等调整 `CODEMAP.md` 结构；`CLAUDE.md` 第七节按语言替换。

---

## 发布到 GitHub（维护者）

在 skill 目录已 `git init` 时：

```bash
cd ~/.agents/skills/project-bootstrap

git add SKILL.md discovery.md templates.md README.md LICENSE
git commit -m "docs: project-bootstrap skill"

# GitHub 上新建空仓库 project-bootstrap 后：
git remote add origin https://github.com/YOUR_USER/project-bootstrap.git
git push -u origin master
```

其他设备按「首次安装」`git clone` 即可。

---

## License

MIT — 见 [LICENSE](LICENSE)
