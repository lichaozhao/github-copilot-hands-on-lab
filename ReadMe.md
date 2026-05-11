# GitHub Copilot Hands-on Lab

这是一个用于演示和练习 GitHub Copilot 使用方式的教学仓库。初始来源 [copilot-agent-and-mcp](https://github.com/ps-copilot-sandbox/copilot-agent-and-mcp)

示例项目源码位于 [bookstore](https://github.com/lichaozhao/bookstore)，它是一个前后端分离的图书收藏应用。

实验用到的用户需求以及实现需求的示例提示词位于 [user-requirement-cn.md](./hands-on/user-requirement-cn.md)。

实验过程是利用 GitHub Copilot 按照用户需求完成对现存 `bookstore` 项目的修改和扩展，并在整个过程中熟悉 GitHub Copilot 的各种使用方法，并对如何与 AI 协作有更深入的理解。

## 实验环境准备
- 安装最新版的 VS Code
- 向管理员申请了 GitHub Copilot 的账号
- 本地安装了 Node.js 和 npm
- 克隆仓库 [bookstore](https://github.com/lichaozhao/bookstore) 到本地，并在 VS Code 中打开了 `bookstore` 文件夹

## 建议的实验过程
- 阅读 `bookstore/README.md`，理解应用的功能和运行方式
- 运行 `bookstore` 项目，测试它的功能
- 阅读 `hands-on/user-requirement-cn.md`，了解需要实现的用户需求
- 利用 GitHub Copilot 按照用户需求逐步修改和扩展 `bookstore` 项目，在过程中了解以下功能：
   - 使用Agent实现简单的功能
   - 修复当前仓库中的错误
   - 了解如何生成长时间运行的需求
   - 了解如何设置Agent的目标和约束
   - 了解如何构建适合自己的AI Coding基建


## 实验材料说明

`bookstore` 项目中 GitHub Copilot用到的个性化配置和作用如下：
```text
.claude/
└── skills/
    ├── bug-fix/SKILL.md                    # 修复 bug 的操作规范与约束，强调根因分析和最小化修改
    ├── provision-test-env/SKILL.md         # L4/L5 级测试环境准备流程与日志记录要求
    ├── run-test/SKILL.md                   # 各级测试执行手册，包含 lint、type-check、build、E2E 测试等流程
    ├── teardown-test-env/SKILL.md          # L4/L5 级测试环境销毁流程
    └── understand-project-info/SKILL.md    # 项目信息汇总与了解流程，强调先查阅结构文档

.github/
├── copilot-instructions.md                 # 项目开发、计划、测试、代码修改等重要约定
├── agents/
│   ├── CodeReviewer.agent.md               # 代码审查 agent 说明，职责、输出格式、关注点
│   ├── ProductManager.agent.md             # 产品经理 agent 说明，需求澄清、规范化流程
│   └── TestPlaner.agent.md                 # 测试规划 agent 说明，测试计划与覆盖率要求
├── hooks/
│   ├── archive-status.sh                   # 归档 docs/status.md 的脚本
│   └── hook.json                           # 配置 SessionStart 时自动归档 status.md 的钩子
├── instructions/
│   ├── memory-externalization.instructions.md   # 跨 session 状态外化与维护规范
│   ├── project-structure.instructions.md        # 项目结构与主要文件说明
│   └── test-defination.instructions.md          # 各级测试定义与分层标准
└── prompts/
    ├── sample.prompt.md                         # 示例 prompt，主要用于 code review 场景
    ├── test-scenario-generation.prompt.md       # 测试场景生成相关 prompt
    └── update-architecture-doc.prompt.md        # 更新架构文档的 prompt，指导文档内容
```

其中`.claude`和`.github`目录下的文件及文件夹作用是：

| 文件夹/路径                  | 作用说明                                                                 |
|-----------------------------|--------------------------------------------------------------------------|
| `.claude/`                  | 存放与 Claude 相关的技能定义文件，规范操作流程与约束。                     |
| `.claude/skills/`           | 定义具体技能（如 bug 修复、测试环境管理等）的操作手册与流程。               |
| `.github/`                  | 存放 GitHub 配置文件，包括开发约定、钩子脚本、测试规划等。                  |
| `.github/copilot-instructions.md` | 默认加载的指令文件，类似 AGENTS.md、CLAUDE.md                  |
| `.github/agents/`           | 定义不同 Agent 的职责与输出格式（如代码审查、产品经理、测试规划）。          |
| `.github/hooks/`            | 配置 GitHub 钩子脚本（如自动归档 status.md）。                             |
| `.github/instructions/`     | 定义项目开发、测试、结构说明等重要约定与规范。                              |
| `.github/prompts/`          | 存放与特定场景相关的 Prompt 文件（如测试场景生成、架构文档更新）。           |


## `bookstore`仓库里目前存在的两个问题

这两个问题需要借助GitHub Copilot来修复。

### 1. ESLint 不认识 Playwright 测试环境

相关文件：

- [bookstore/frontend/eslint.config.js](https://github.com/lichaozhao/bookstore/frontend/eslint.config.js)
- [bookstore/frontend/playwright/e2e/book_favorites.spec.js](https://github.com/lichaozhao/bookstore/frontend/playwright/e2e/book_favorites.spec.js)

这个案例用于说明“工具配置问题”和“业务代码问题”是两回事。

如果 ESLint 配置里没有声明这些测试环境相关的全局变量，ESLint 就会把它们误判成“未定义变量”，然后报出一系列 `no-undef` 错误。

这个案例适合教学以下内容：

- 什么是静态检查
- 什么是运行环境提供的全局对象
- 为什么“代码能运行”和“lint 能通过”是两件不同的事
- 遇到错误时，应该先判断是代码错了，还是工具不知道当前上下文

### 2. Redux reducer 参数声明了但没有使用

相关文件：

- [bookstore/frontend/src/store/favoritesSlice.js](https://github.com/lichaozhao/bookstore/frontend/src/store/favoritesSlice.js)

这个案例用于说明“未使用变量”为什么会被当作一个值得修复的问题。

在 Redux Toolkit 的 reducer 或 `addCase` 回调中，常见写法会声明两个参数：

- `state`
- `action`

但如果某一段逻辑里把参数写出来了，却完全没有使用，就会触发 ESLint 的 `no-unused-vars` 报错。

这个案例适合教学以下内容：

- 函数参数为什么也算变量
- 什么叫“声明了但没有使用”
- 为什么这类问题虽然不一定导致运行失败，但仍然是代码质量问题
- 如何判断是应该删掉无用参数，还是应该补上缺失的逻辑

## 许可证

本项目采用 MIT License，完整内容见仓库根目录的 [LICENSE](./LICENSE) 文件。


