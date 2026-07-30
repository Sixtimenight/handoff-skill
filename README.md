# Handoff Context Compression

一个用于 Codex 的轻量上下文交接技能：把长对话整理成可直接粘贴到新对话的状态快照。

## 安装

将 `SKILL.md` 放入 Codex 技能目录：

```text
~/.codex/skills/handoff/SKILL.md
```

也可以放在项目级技能目录：

```text
.agents/skills/handoff/SKILL.md
```

## 使用

在对话中输入：

```text
/handoff-context
```

默认输出约 3000–5000 tokens。另支持 `极限版` 和 `详细版`。
