# Interview Helper

[English](README.md) | 简体中文

一个 Claude skill，帮你在口头代码面试（viva / code walkthrough / 面试式作业检查）之前，真正吃透自己写的代码作业。

很多课程（UQ、USYD 等）通过面试来检查编程作业。
导师会针对你自己的代码提出刁钻的问题，答不上来就挂。
让 AI "给我讲讲我的项目"只会带来虚假的掌握感。
这个 skill 反其道而行：让你自己产出答案，然后精确指出你的薄弱点。

## 功能

**学习模式（study mode）** - 引导你五层递进地理解自己的项目：
整体概览、模块、函数与代码行、设计决策、边界情况。
你先解释，AI 只负责纠错和补漏。每一层结束都有一道测验关卡。

**面试模式（interview mode）** - 针对你的代码进行全英文模拟面试，分三个难度：

| 难度 | 考察点 | 风格 |
|---|---|---|
| Easy | "是什么" | 可以看代码，表层问题，态度温和。可用任意语言回答。 |
| Hard | "为什么" | 设计决策、边界情况、"如果把 X 改掉会怎样"。有追问。 |
| Hell | "这真是你写的吗" | 学术诚信式拷问、陷阱问题、现场写代码、追问到底。必须用英语回答。 |

没有提示，没有中途纠正，没有标准答案 - 和真实面试一模一样。
每轮结束后你会收到一份书面报告：每题评分、映射到你代码的薄弱点、总体判定，以及接下来该补什么。

## 安装

一条命令，无需手动复制（支持 Claude Code、Codex、Cursor、OpenCode 等 [70+ 种 agent](https://github.com/vercel-labs/skills#supported-agents)）：

```bash
npx skills add lewiswang0516/interview-helper-skill
```

加 `-g` 可全局安装（所有项目可用），否则只安装到当前项目：

```bash
npx skills add lewiswang0516/interview-helper-skill -g
```

后续更新：`npx skills update`。
卸载：`npx skills remove interview-helper`。

Claude Code 手动安装（如果你更喜欢手动方式）：

```bash
git clone https://github.com/lewiswang0516/interview-helper-skill
cp -R interview-helper-skill/skills/interview-helper ~/.claude/skills/interview-helper
```

claude.ai：在 capabilities 设置里把 `skills/interview-helper` 文件夹作为 skill 上传。

## 使用

直接告诉 Claude 你要面试就行 - 这个 skill 会被这类话触发：
"I have a code interview on my assignment"、"考考我"、"拷打我"、"quiz me on my project"。
也可以直接调用：`/interview-helper`。

你还可以提供这些额外材料（全部可选）：

- 作业说明或评分标准（rubric）- 问题会瞄准评分点来出。
- 一份题目清单 - 其他同学的往届面试题、课程题库，或你自己担心的问题列表。这些会被优先提问。

## 正当使用

这个 skill 的目的是让你理解自己的作品，这正是面试考核想要检验的东西。
它不会替你写作业或改作业，也拒绝提供用来背诵的标准答案 - 反正背下来的答案在第一个追问上就会露馅。
