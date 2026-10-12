# qu-ai-wei 去AI味改写技能

> 把文本从「像模型在表演写作」拉回「像具体人在当前场景下表达」。

`qu-ai-wei` 是中文优先、兼容中英混排的 Agent Skill，遵循 Agent Skills 开放标准（`SKILL.md` + `references/`），Claude Code / Codex / Cursor / OpenCode 通用。它检测并清理文本里的 AI 生成痕迹，同时保住事实、术语、责任主体和作者语气。

它**不判断文本是不是 AI 写的**，不输出「AI 含量」，也不承诺通过任何检测器——它清理的是写作痕迹，不是作者身份。

## 设计原则

四条硬约束，优先级高于任何规则、场景或用户加码：

1. **白名单改写**——只改能定位到触发标记的地方，其余逐字保留，不顺手润色。
2. **信息守恒**——改写后的每个实词都要能在原文指出出处；指不出来就是新增，撤销。
3. **结构不动**——标题层级、章节顺序、段落数、列表/表格/引用/代码块位置一律保持。
4. **作者风格优先**——有风格文档就以它为准，作者的写法不是 AI 痕迹。

外加一条**实证防误杀的禁区**：句长参差、被动句、名词化、正文设问、句内排比、比喻等特征，看着像 AI 味但实测站不住（有的方向还相反），一律不当问题处理。

## 功能

- **五步工作流**：读清任务 → 划保护域 → 找 AI 味 → 最小改写 → 两遍回读
- **42 条标记目录**，按「触发标记 → 改法 → 边界」组织，分内容拔高 / 句式节奏 / 中文特有 / 用词与权威 / 交流残留五组
- **高收益标记速查**：翻案腔、本质拔高、助手路标、揭露式破折号、提示语冒号、顿号密集罗列、无动机结尾问句等，命中率最高的一批直接放在 SKILL.md
- **保护机制**：protected spans + 事实账本，防止「为了像人而失真」
- **实证否证表**：带频率数据的「不该改什么」清单，硬约束而非参考
- **分场景规则**：聊天 / 站会汇报 / 技术文档 / 公众号小红书 / 随笔 / 小说正文，加 README / release-note / 论坛帖 / issue 回复子场景包
- **两种输出模式**：默认单一改写稿、annotation mode（只标问题不改写），用户要求时给 50 分评分
- **两遍回读**：保真回读（信息不漂）+ 残留审计（AI 味清零）

## 安装

技能目录是 `SKILL.md` + `references/` 的自包含结构，放到任意 Agent Skills 兼容位置即可。

```bash
# 跨 agent 共享目录（Codex / Cursor / OpenCode 等原生读取）
git clone https://github.com/Bluetooth503/qu-ai-wei.git ~/.agents/skills/qu-ai-wei

# Claude Code 只认自己的目录，软链过去即可（避免两份副本不同步）
mkdir -p ~/.claude/skills
ln -sfn ../../.agents/skills/qu-ai-wei ~/.claude/skills/qu-ai-wei
```

也可以直接把 `qu-ai-wei/` 复制进 `~/.claude/skills/`、`~/.codex/skills/` 或项目内的 `.agents/skills/`。

## 使用

```text
帮我把这段话改自然点，别那么 AI：……
这段文字哪里像 AI 写的？先标问题，别改：……
帮我润色这篇公众号文章，最后给个评分。
帮我改这份 release note，但技术信息一个都不能丢。
```

## 目录结构

```
qu-ai-wei/
├── SKILL.md                    # 决策层：四条硬约束 + 五步工作流 + 高收益标记 + 输出合同 + 自检
└── references/
    ├── patterns.md             # 完整标记目录（42 条，触发标记/改法/边界）
    ├── structures.md           # 形状层：段落、节奏、标点、强调、列表
    ├── protected-spans.md      # 保护清单 + 事实账本 + 实证否证表
    ├── scenes.md               # 场景判定、子场景包、默认范围与力度
    └── examples.md             # 完整示例、评分演示、诊断输出、bounded 删除清单
```

SKILL.md 本身足以完成基础改写；遇到拿不准的模式、场景或边界，再按需读对应文件。

## 来源与致谢

本技能是三份去 AI 味方法的融合（均取最新版本）：

| 来源 | 贡献 |
|------|------|
| [op7418/humanizer-zh](https://github.com/op7418/humanizer-zh)（汉化自 [blader/humanizer](https://github.com/blader/humanizer)，参考 [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop)） | 模式分类、边界示例、交付前核对 |
| [MrGeDiao/shuorenhua](https://github.com/MrGeDiao/shuorenhua)（说人话） | 决策流程、保护机制（protected spans / 事实账本）、无源引用模式、annotation mode、bounded 删除清单 |
| [larashero3-dotcom/lieflat-less-ai-tone](https://github.com/larashero3-dotcom/lieflat-less-ai-tone) | 白名单纪律、实证频率数据、翻译腔五种结构、标点类规则 |

部分模式与示例整理自维基百科 [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)（WikiProject AI Cleanup 维护）。

> 早期版本曾参考 `B1lli/remove-ai-flavor-writing-skill`，该仓库已下线，相关规则由 `lieflat-less-ai-tone` 的实证结果替代。

## License

MIT © Enlai503
