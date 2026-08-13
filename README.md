# qu-ai-wei 去AI味改写技能

> 把文本从「像模型在表演写作」拉回「像具体人在当前场景下表达」。

`qu-ai-wei` 是一个 ZCode / Claude Code 兼容的 Agent Skill，用于检测并清理中英文文本中的 AI 生成痕迹，同时保住事实、术语、责任主体和作者语气。

## 功能

- **8 步决策工作流**：场景判定 → 划保护域 → 判 Tier → 判档位 → 判 scope → 改写 → 两步回读 → 输出
- **7 类高危句式壳**：二元对比壳（不是A而是B）、阶段推进壳（先A再B）、本质论断壳（真正重要的是）、助手路线标记、收窄与易答框架、模板化段落形状、无动机结尾问句
- **保护机制**：protected spans + 事实账本，防止「为了像人而失真」——不新增事实、不删核心事实、不改变责任主体
- **分场景规则**：聊天 / 站会汇报 / 技术文档 / 公众号小红书 / 随笔 / 小说正文 / README / release-note / 论坛帖 / issue 回复
- **三种输出模式**：默认单一改写稿、annotation mode（只标问题不改写）、50 分质量评分
- **两种回读检查**：保真回读（信息不漂）+ 残留审计（AI 味清零）

## 安装

```bash
# 复制到技能目录（以 ZCode 为例）
cp -r qu-ai-wei ~/.agents/skills/
# 或 ~/.zcode/skills/
```

## 使用

```text
帮我把这段话改自然点，别那么 AI：……
这段文字哪里像 AI 写的？先标问题，别改：……
帮我润色这篇公众号文章，最后给个评分。
```

## 目录结构

```
qu-ai-wei/
├── SKILL.md                    # 核心流程 + 7 类高危句式壳 + 质量门 + 输出合同
└── references/
    ├── patterns.md             # 完整模式目录（五类 18+ 模式，含中英文词表）
    ├── scenes.md               # 场景判定、子场景包、场景禁改
    ├── protected-spans.md      # 保护清单与事实账本规则
    ├── structures.md           # 结构反模式与格式类问题
    └── examples.md             # 完整示例、评分演示、诊断标注示例
```

## 来源与致谢

本技能是以下三份去 AI 味方法的融合：

| 来源 | 贡献 |
|------|------|
| [humanizer-zh](https://github.com/blader/humanizer)（翻译自 blader/humanizer，参考 [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop)） | 完整模式目录、50 分评分体系、「注入灵魂」原则 |
| shuorenhua（本地技能，无公开仓库） | 8 步决策工作流、保护机制（protected spans / 事实账本）、无源引用三模式、annotation mode、bounded 删除清单 |
| [remove-ai-flavor](https://github.com/B1lli/remove-ai-flavor-writing-skill) | 中文句式壳目录、分场景规则（含小说对话豁免、小红书专项）、质量门检查 |

部分模式与示例整理自维基百科 [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)（WikiProject AI Cleanup 维护）。

## License

MIT © Enlai503
