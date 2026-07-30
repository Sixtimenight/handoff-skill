# Handoff Context Compression

## Purpose

Create a compact, self-contained context snapshot for continuing the current task in a new Codex conversation.

## Trigger

Use this skill when the user asks for any of the following:

- `鐢熸垚 handoff`
- `鐢熸垚浜ゆ帴`
- `鍘嬬缉涓婁笅鏂嘸
- `缁欐柊瀵硅瘽鍑嗗涓婁笅鏂嘸
- `鎬荤粨缁欎笅涓璇漙
- `/handoff`

## Core behavior

1. Summarize the work state, not the conversation history.
2. Preserve facts needed to continue immediately.
3. Prefer evidence from the current workspace and tool results over memory or assumptions.
4. If this is a code task, inspect the relevant files and, when available, check:
   - current branch
   - changed files
   - recent commits
   - relevant test or build results
   - current error output
5. Do not modify project files merely to create a handoff.
6. Do not claim that a test, command, or file inspection happened unless it actually happened.
7. Mark uncertain, stale, or unverified information as `鏈‘璁 or `鏈煡`.
8. Omit greetings, dead ends, repeated explanations, and unrelated background.
9. Never include secrets, tokens, passwords, private keys, or sensitive personal data. Replace them with `[宸查殣钘廬`.

## Output requirements

Return only a copyable Markdown document beginning with `# Handoff`.

Default target length: approximately 3000鈥?000 tokens. Use the lower end when the task is simple; use the upper end only when details are needed to resume safely. Do not pad the document to reach the target.

Use this structure:

# Handoff

## 1. 浠诲姟鐩爣

- What must ultimately be achieved.
- Why it matters, if relevant.
- The user's explicit success criteria and constraints.

## 2. 褰撳墠杩涘害

- What phase the work is in.
- What is currently being worked on.
- The last known state before handing off.

## 3. 宸插畬鎴愬伐浣?
List completed work and verification results. For code, include relevant absolute or workspace-relative file paths and the important changes.

## 4. 鍏抽敭鍐崇瓥涓庣害鏉?
Record confirmed technical choices, rejected approaches, user preferences, compatibility requirements, and any instructions that must continue to be followed.

## 5. 褰撳墠鐜板満

Include useful workspace facts, existing resources, relevant configuration, and鈥攚hen applicable鈥攖he following Git snapshot:

```text
Branch:
Working tree:
Changed files:
Last relevant commit:
Tests/build:
```

Include exact error messages only when they help the next session. Keep logs short and retain the key location or cause.

## 6. 鏈畬鎴愪簨椤逛笌椋庨櫓

- Remaining TODOs.
- Blockers and their evidence.
- Unverified assumptions.
- Potential regressions or risks.

## 7. 涓嬩竴姝ヨ鍔?
Give the next 1鈥? actions in priority order. Make the first action concrete enough for the next Codex session to begin immediately.

## 8. 缁欎笅涓€娆?Codex 鐨勬彁绀?
Include pitfalls, things not to repeat, files or commands to inspect first, and any user preference that affects the next response.

## Quality checklist

Before returning the handoff, verify that it answers:

- What is the goal?
- What is already done?
- What is true right now?
- What remains?
- What should happen first next?

If a section has no relevant information, write `鏃燻 rather than inventing content. Keep the result self-contained and directly pasteable into a new conversation.

## Optional modes

If the user says `鏋侀檺鐗坄, target no more than 800 tokens and keep only the goal, current state, blockers, changed files, and next actions.

If the user says `璇︾粏鐗坄, remain under approximately 7000 tokens and include more implementation context, but still omit conversation history and repetition.

If the user asks to save the handoff to a file, follow the requested path and preserve the same Markdown structure. Otherwise, return it inline and do not create a file.

