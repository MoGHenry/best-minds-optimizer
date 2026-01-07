# Best Minds

> "Don't think of LLMs as entities but as simulators."
> — Andrej Karpathy

一种思维方法论：不问 AI "你怎么看"，而是问"世界上谁最懂这个？TA 会怎么说？"，然后模拟那个人。

## 核心理念

不要问 AI "你怎么看"。

要问：**这个问题，世界上谁最懂？TA 会怎么说？**

然后模拟那个人。

## 使用场景

- **技术问题** → 找该领域的顶级工程师
- **商业策略** → 找成功的企业家或投资者
- **创意写作** → 找该类型的大师作家
- **学术研究** → 找该领域的顶尖学者
- **人生建议** → 找该领域的权威专家

## 核心原则

1. **问题决定人数** — 一个人够就一个，需要碰撞才多个
2. **找真正最懂的** — 不是找"合适的"，是找"最强的"
3. **基于真实** — 模拟要基于 TA 公开的思想、著作、言论
4. **引用原话** — 尽可能用 TA 说过的话

## 与其他方法的对比

| ai-coaches | best-minds |
|------------|------------|
| 从 13 个预设智者选 | 从全世界找 |
| 基于关键词匹配 | 基于问题本质 |

## 触发词

- 最强大脑
- 顶级专家
- 世界级
- best minds
- 谁最懂这个

## 使用示例

### 示例 1: 技术架构
```
问：我想设计一个大规模分布式系统，谁最懂？怎么设计？
答：模拟 Werner Vogels (Amazon CTO)...
```

### 示例 2: 人生建议
```
问：我想创办一个公司，需要什么建议？
答：模拟 Paul Graham (Y Combinator 创始人)...
```

### 示例 3: 写作
```
问：怎样写出吸引人的故事？
答：模拟 Neil Gaiman (著名作家)...
```

## 安装

如果你使用 Claude Code 或其他支持 Skill 的 AI 工具：

1. Clone 本仓库到 `~/.claude/skills/best-minds`
```bash
git clone https://github.com/YOUR_USERNAME/best-minds ~/.claude/skills/best-minds
```

2. 配置你的工具使其支持该 skill

3. 在提示中使用触发词即可激活

## 文件结构

```
best-minds/
├── README.md          # 本文件
├── SKILL.md           # Claude Code Skill 配置
├── LICENSE            # MIT License
├── EXAMPLES.md        # 使用示例集
└── .gitignore         # Git 忽略配置
```

## 工作原理

1. **识别问题** — 理解用户的核心问题
2. **定位专家** — 找出世界上最懂这个问题的人
3. **研究背景** — 基于该人的公开言论和著作
4. **模拟思维** — 按照该人的思维方式来回答
5. **引用支持** — 尽可能引用原话和例子

## 注意事项

- 模拟是基于公开信息，不是真实对话
- 尽可能引用该人说过的话
- 指出特定的代表著作或言论
- 承认 AI 模拟的局限性

## 贡献

欢迎提交改进建议、新的使用示例或反馈！

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个 Pull Request

## License

MIT License - 详见 [LICENSE](LICENSE) 文件

## 相关资源

- [Claude 官方文档](https://claude.ai)
- [Andrej Karpathy - LLM Simulator](https://karpathy.substack.com/)
- [Paul Graham Essays](http://paulgraham.com/essays.html)

## 作者

Created by [Your Name]

## 反馈

如有任何问题或建议，欢迎在 [Issues](issues) 中讨论！
