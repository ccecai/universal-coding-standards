# 通用代码规范

[English](README.md) | [简体中文](README.zh-CN.md)

供 Codex、Cursor 在不同项目中使用的通用编码约定。实际指令保留在英文
[SKILL.md](SKILL.md) 中，回答语言按用户要求。这些是默认规则，用户明确要求和
适用的项目规则优先。

## 包含哪些内容

- 控制改动范围，明确源码归属、模块边界和平台差异。
- 管理共享状态、并发、输入检查、资源生命周期和关键操作。
- 明确初始化、验证限制、命名、常量、日志和文档要求。
- 检查编译警告、构建、最终链接、验证证据和提交内容。
- 补充 C/C++ 的头文件、声明、函数复杂度、宏、分支及数据安全要求。

C/C++ 和硬件要求仅在相关场景下适用。安装 skill 不会增加工具、仓库访问权限，
也不代表获得提交或发布授权。

## 必须配套的两个 skill

将你已有、可信来源的 **Ponytail** 和 **Caveman** 放在同级目录：

```text
~/.agents/skills/
├── universal-coding-standards/
│   ├── SKILL.md
│   └── agents/openai.yaml
├── ponytail/SKILL.md
└── caveman/SKILL.md
```

Ponytail 负责最小实现、根因分析、必要保障和可运行检查，默认使用 `full` 模式。
Caveman 负责简洁、完整地沟通。本仓库补充其余工程要求。
两个配套 skill 都是必需的，本仓库不包含它们，也不会自动安装。

## 安装

以下命令需要 Git 和 POSIX shell，例如 macOS 终端或 Linux shell。本仓库是私有仓库，
需要使用有访问权限的账号认证，不要把令牌写进克隆地址。目标目录已存在时，
先检查并备份，不要直接覆盖。

### Codex、Cursor 共用的个人安装

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/ccecai/universal-coding-standards.git \
  "$HOME/.agents/skills/universal-coding-standards"
```

使用前，将 Ponytail、Caveman 放入上面列出的同级目录。当前 Codex、Cursor 官方文档
均列出 `~/.agents/skills` 作为用户级技能发现位置。没有看到新 skill 时，重新开一个
会话或重启客户端。本地安装不会自动安装到远程执行环境或 ChatGPT 账号。

如果已经安装在 `~/.codex/skills`，继续使用实际路径，避免重复克隆。Cursor 也支持
`~/.cursor/skills`；使用该位置时，三个 skill 仍需放在同一级。
只想在某个项目中使用，可以将三个文件夹放进该项目的 `.agents/skills/`。

## 让每次编码任务都默认遵守

能够被发现、允许自动选择，不等于每次任务都必然使用。在 Codex 中，将下面的要求
合并到已有的全局 `~/.codex/AGENTS.md`，不要覆盖其他指令。自定义了 `CODEX_HOME`
时，修改对应目录下的规则文件；同一层存在有效的 `AGENTS.override.md` 时，它优先。

```markdown
For every task that writes, changes, fixes, refactors, reviews, or designs code,
read and follow these skills unless already loaded:
- ~/.agents/skills/universal-coding-standards/SKILL.md
- ~/.agents/skills/ponytail/SKILL.md
- ~/.agents/skills/caveman/SKILL.md
Default to Ponytail full mode. Higher-priority instructions, explicit user
requirements, and applicable project rules take precedence over these defaults.
```

将路径改成实际安装位置。

### Cursor：让所有项目默认遵守

打开 **Customize → Rules**，新增或编辑一条 **User Rule（用户规则）**，将上面的要求
合并进去。用户规则适用于各项目的 Agent；仅添加项目规则或项目 `AGENTS.md`，
不能达到全局默认启用的效果。保留其他已有用户规则，并补充下面的执行要求：

```text
When I ask to "规范代码", "规范", "规范一下", or "按规范整理" in a coding task,
execute the Code normalization requests workflow in universal-coding-standards.
```

Cursor 也能发现已有的 `~/.codex/skills` 安装。在用户规则中引用真实文件路径，
不必再复制一份。检查该规则列在 **User** 下，且 Customize 的 **Skills** 中能看到技能。
这些是本地 Agent 的默认规则，远程环境需要单独配置。

## 使用方法

被客户端发现后，编码、修复、重构、设计、审查和依赖选择等任务都可以选用此 skill。
在 Codex 中也可以明确调用：

```text
$universal-coding-standards 按通用代码规范完成这次改动。
```

在 Cursor 中输入 `/`，从命令列表选择 `universal-coding-standards`。
检查安装时，可以让助手列出当前生效的全局和项目规则，并确认两个配套文件都能读取。
本 skill 不增加单独的审批流程。

### 整理改动的代码

```text
规范代码
规范
规范一下这次改动的代码
按规范整理 src/example.c，保持功能不变
```

这些编码请求会实际整理代码，不是只列检查清单。在正在处理代码的上下文中，
单独输入“规范”也会执行。指定了文件、选区或差异，就按指定范围处理；未指定时，
检查暂存、未暂存及相关新增代码，只整理改动行和必要的周边代码。
遵守项目工具和风格，保留原有功能及用户改动，并汇报实际检查结果。
没有改动代码也没有目标时，会请用户指定范围，不会直接整理整个仓库。
整理过程不会自动暂存或提交。引用示例、明确说不要整理、询问规范含义，或要求修改
这个 skill 本身，都不会被当成修改业务代码的授权。

## 文件与更新

- `SKILL.md`：英文指令和适用范围。
- `agents/openai.yaml`：显示信息和 `allow_implicit_invocation: true` 配置。
- `README.md` / `README.zh-CN.md`：英文、中文安装及使用说明。
- `.gitignore`：本地环境、凭据文件和缓存的忽略规则。

通过克隆本仓库得到、且没有本地改动的安装，可以在安装目录运行 `git pull --ff-only`
更新。存在本地改动或历史分叉导致更新失败时，先检查原因，不要重置或覆盖。
两个配套 skill 分别更新。

## 相关技能与参考

- [代码审查技能](https://github.com/ccecai/code-review-skill)
- [新增功能与 BUG 修复技能](https://github.com/ccecai/feature-bugfix-skill)
- [Codex 技能文档](https://learn.chatgpt.com/docs/build-skills)
- [Codex 全局和项目规则](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- [Cursor 技能文档](https://cursor.com/docs/skills)

这是独立维护的 skill，不是 OpenAI 或 Cursor 官方产品。
