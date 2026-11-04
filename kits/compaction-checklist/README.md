# 压缩会话防失忆清单（Claude Code / Codex）

> 克劳德队长 · 评论「清单」领取的那份
> 用来做什么：长会话被压缩（compact）时，AI 不报错，只是悄悄忘掉没写进文件的东西。这份清单让你压缩前点名要留什么、压缩后自动把状态塞回去、再问一遍确认没丢。
> 来源：Mayur Agnani《How to Tell What Claude Code and Codex Forget When They Compact a Session》（DEV Community，2026-10-31）中的配置和命令，我们加了中文用法和注意事项。命令参数以你本机 `claude --help`、Codex 官方文档为准，版本不同可能有出入。

---

## 先记住一条规律

压缩后的上下文从三条路拼回来：

| 路 | 包含什么 | 压缩后 |
|---|---|---|
| 磁盘重读 | 根目录 CLAUDE.md、自动记忆、计划文件、最近读过的几个小文件 | 还在 |
| 按额度保留 | 最近的对话原话（Codex 约 20,000 token 以内） | 还在 |
| 只靠摘要 | 其余所有：只读过的值、一次性的命令输出、日志、网页 | 看摘要写没写 |

**结论：找不回来的东西，压缩前写进文件。**

---

## 1. 压缩前保留指令（Claude Code，零配置）

```
/compact Keep the exact failing test names, error strings and ids.
```

用法：手动压缩时用。把后半句换成你这次真正要留的东西，越具体越好，比如：

```
/compact 保留：失败的测试名原文、报错原文、改过的文件清单、还没做完的下一步。
```

注意：一次别点太多，摘要越长，后面每轮都多花 token。只点「丢了就找不回」的。

## 2. Codex 压缩提示词（配一次，每次压缩都生效）

写进 `~/.codex/config.toml`：

```toml
compact_prompt = "Summarize this session. Keep exact error strings, ids, file paths and test names."
```

注意：这个配置项出自原作者对 Codex 开源代码的阅读，作者没实际跑过 Codex。改前先备份配置文件，改后压一次看效果。

## 3. 根规则文件里的压缩说明（Claude Code）

放进仓库根目录的 `CLAUDE.md`（不要放子目录，子目录的规则压缩后不一定在）：

```markdown
# Compact Instructions
Keep exact error strings, failing test names, ids and every file edited.
```

用法：每次压缩时摘要都会读到它，等于把第 1 条固化下来。

## 4. 压缩后自动恢复钩子（Claude Code）

写进项目的 `.claude/settings.json`（已有 hooks 的话合并进去，别整段覆盖）：

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "compact",
        "hooks": [
          {
            "type": "command",
            "command": "git status --short; cat notes/session.md 2>/dev/null"
          }
        ]
      }
    ]
  }
}
```

用法：每次压缩结束，自动把「改了哪些文件」和你的笔记文件内容塞回上下文。`notes/session.md` 换成你自己的进度文件，比如我们用的 `进度.md`：

```
git status --short; cat 进度.md 2>/dev/null
```

注意：笔记文件别太长，钩子输出每次都占上下文。

## 5. 压缩后探针命令（确认到底丢没丢）

```bash
claude -p --resume "$SESSION" --fork-session --tools "" \
"From memory, no tools. Which files did you edit? What was the exact error? Reply UNKNOWN if unsure."
```

用法：压缩刚结束时跑。`$SESSION` 换成会话 ID；`--fork-session` 分叉出一份，不污染原会话；`--tools ""` 关掉工具，逼它只凭记忆答。问题换成你最在意的 3–5 个事实，允许它答 UNKNOWN，漏了一眼就能看出来。

注意：参数以本机 `claude --help` 为准。改设置前跑一次记下漏了几个，改完再跑一次对比。

还有一个不用跑命令的信号：压缩后它马上把同一个文件又读一遍，说明摘要漏了它。

## 6. 加钩子前三问

```
1. 哪个事实一旦丢了，AI 自己找不回来？
2. 这个事实是从文件进来的，还是只在对话里出现过？
3. 压缩后用探针问，它能原样答出来吗？
```

（原文：Which fact, if lost, could the agent not get back? / Does that fact reach the model through a file, or only through a message? / Would a probe after compaction return it exactly?）

用法：三问都答完再决定要不要加第 4 条的钩子。答案是「能重新读到」「短任务不会压缩」的，就不必折腾。

---

## 什么时候不用管压缩

- 任务很短，根本填不满上下文。
- 信息在磁盘上、重读一次很便宜。
- 下一件事和当前无关：直接 `/clear` 开新会话，比任何摘要都干净。
- 大量调研：交给子代理去读，别塞进主对话。

## 我们自己的做法

每个项目一份 `进度.md`：每交付一件事就更新现状、已验证的、下一步；新窗口从「下一步」第 1 条接着干。我们写这份清单时，会话本身就压缩过一次，靠摘要 + 进度.md 接上了。
