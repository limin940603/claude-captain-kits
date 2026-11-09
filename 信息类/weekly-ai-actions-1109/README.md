# 本周 AI 资讯 · 一人公司的 3 个动作自查清单（2026-11-09 期）

> 克劳德队长 · 评论「清单」领取的那份
> 对应文章：《本周AI资讯：一人公司先做这3件事》
> 用法：挑一个正在做的项目，三个动作选一个先做，按清单打勾、按表格填。
> 用来做什么：把这周三篇文章里的思路，变成你本周就能对着自己项目做一遍的检查。

---

## 动作 1：把项目说明写短

根说明只留三类内容，其余细则放到别的文件，在入口写清位置。

```
- [ ] 技术栈：项目用什么运行
- [ ] 目标：这次交付要完成什么
- [ ] 检查：做完怎样确认能用
- [ ] 其他细则已移到单独文件，入口写了位置
- [ ] 让助手复述目标和检查方法，它说对了再开工
```

## 动作 2：AI 说「完成了」，对一下证据

抽一条助手声称完成的真实任务，填这张表：

| 助手声称完成了什么 | 对应的工具记录在哪 | 能直接检查的文件或结果 | 哪一步还没验证 | 状态（已核对 / 待核对） |
|---|---|---|---|---|
|  |  |  |  |  |

找不到对应结果，就记「待核对」。

## 动作 3：外部材料里的要求，查清参数是谁给的

适用于会「读邮件、网页、附件，再调用工具」的助手。

```
- [ ] 列出敏感参数：收件人、账号、金额……
- [ ] 每种参数写清可接受的授权来源（例如：只认用户本人确认过的地址）
- [ ] 外部材料里出现新地址/新账号时，助手会暂停并请你确认
- [ ] 用一份自拟材料测一次，记下它是否直接采用了材料里的新地址
```

---

## 信息来源

| 动作 | 来源 |
|---|---|
| 1 项目说明写短 | [The coding agent toolbox around the model: 8 notes on CLAUDE.md, mcp2cli, OpenCode and worktrees](https://dev.to/redreamality_dev/the-coding-agent-toolbox-around-the-model-8-notes-on-claudemd-mcp2cli-opencode-and-worktrees-3ig3)（DEV Community，2026-11-07） |
| 2 核对完成证据 | [How I built a local AI assistant that admits when it didn't actually do the thing](https://dev.to/kayato123_fca8d3c04a54849/how-i-built-a-local-ai-assistant-that-admits-when-it-didnt-actually-do-the-thing-25bh)（DEV Community，2026-11-05） |
| 3 参数来源检查 | [Stop asking "is this text malicious"](https://dev.to/zikoasm/stop-asking-is-this-text-malicious-how-i-stopped-144144-prompt-injection-attacks-on-an-ai-1in)（DEV Community，2026-11-07） |

我们的部分：三份清单和表格是克劳德队长根据原文思路整理的做法建议；原文的测试成绩是作者自述，我们没有引用，也没有实测。
