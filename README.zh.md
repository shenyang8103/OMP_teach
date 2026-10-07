# teach

> 🇬🇧 [English version](README.md) · 🇨🇳 这里是简体中文版。

这是 **OMP**（OMP 智能体框架）的一个教学技能。它教会用户任何东西，让知识真正*扎根*、被*理解*，而不只是记住——基于两条经过验证的教学原理，并具备方案持久化、模块级检查点、学习笔记、可选参考资料支持，以及每个测验前的答疑环节。

本技能移植/改写自 [amosblomqvist/learn](https://github.com/amosblomqvist/learn)（原为 `pi` 框架所写），并适配到 OMP 的工具/技能/智能体模型。**非常感谢 [amosblomqvist](https://github.com/amosblomqvist) 提供的原版系统与创意。**

## 功能特性

- **探针 → 规划 → 教学 循环。** 定位用户的理解边界（地板与天花板），构建依赖关系图式的方案，经确认后逐节点教学。
- **两条原理。** 连接型知识优于孤立知识；无条件真理优先。每个节点流程：动机 → 建立 → 连接 → 答疑 → 测验核对。
- **方案持久化。** 方案确认后写入 `learn-plan.md`（mermaid 依赖图 + 模块清单 + 进度指针）。日后续学时会询问"继续还是调整"，而非重新探针。
- **学习笔记。** 每次会话追加一份复习笔记，命名为 `目录名_yymmdd_x.md`（内容目录名 + 日期 + 当次序号）。技能本身不会回读它——仅供用户复习。
- **可选参考资料。** 首次运行会询问用户是否有参考资料目录（当前工作目录下的子目录）。若有，那些文件即为**权威来源**；网络检索仅作补充，且在冲突时以资料为准。该选择会记入 `learn-plan.md`，且不再重复询问。
- **答疑环节。** 教学完一个节点后，弹出一个二选一选择题：**马上测试（默认）** / **你有疑问**。仅当用户选择"你有疑问"时才可提问；循环往复，直到用户选择测试为止。
- **随机化测验答案。** 正确选项位置由当前系统时间秒数决定（`秒数 % 选项数`），因此不会总在第一位；且该计算过程绝不向用户展示。
- **中文优先。** 所有面向用户的教学内容均为中文；英文术语保留括号附原文（如 `质数 (prime)`）。
- **研究者子智能体。** 内置 `researcher` 任务型智能体，负责网络调研并返回带出处的研究简报，以保证方案准确性。

## 目录结构

```
teach/
├── package.json              # OMP 技能清单
├── README.md                 # 英文版
├── README.zh.md              # 简体中文版（本文件）
├── skills/
│   └── teach/
│       └── SKILL.md          # 技能定义（核心内容）
└── agents/
    └── researcher.md         # 内置研究者任务型智能体
```

> 说明：`agents/researcher.md` 被 `SKILL.md` 引用，必须与技能一同安装（见下方安装步骤）。

## 安装（OMP）

OMP 从用户级智能体目录加载技能。请将本技能复制（或软链）到该目录：

```powershell
# Windows（PowerShell）
$agent = "$env:USERPROFILE\.omp\agent"
Copy-Item -Recurse teach\skills\teach         $agent\skills\teach
Copy-Item -Recurse teach\agents\researcher.md  $agent\agents\researcher.md
```

其他 shell 的等价写法：

```sh
AGENT="$HOME/.omp/agent"
cp -r teach/skills/teach         "$AGENT/skills/teach"
cp -r teach/agents/researcher.md "$AGENT/agents/researcher.md"
```

安装后，从任意工作目录执行 `omp read skill://teach` 都应能解析到本技能。日后更新时，请重新执行复制（或通过 OMP 的 `skills.customDirectories` 将技能指向本仓库）。

## 使用方法

在任意 OMP 会话中，直接让它教你东西即可，例如：

> 用 teach 技能教我质数

技能会先探测你的水平，给出方案（含 mermaid 关系图），等待你确认，然后逐模块教学——每个模块结束后提供检查点（继续下一模块 / 保存并退出），并写入 `learn-plan.md` 与 `目录名_yymmdd_x.md` 笔记。日后在同一工作目录重新打开，会从已保存的方案继续。

## 许可证

精神归属请参见原项目；本移植版按"原样"提供，供个人使用。若你进行再分发，请保留对 [amosblomqvist/learn](https://github.com/amosblomqvist/learn) 的署名。
