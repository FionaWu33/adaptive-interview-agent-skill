---
name: adaptive-interview-agent
description: Run a role-specific Chinese interview practice session or review an existing interview transcript. Use when a user provides a JD and resume/project material, requests 专业一面、业务负责人 / 二面 or HR 面 practice, wants optional stress-mode practice for AI 产品、RAG、Agent or evaluation topics, asks to continue from a previous round, or wants a simulated or real interview record reviewed and converted into next-round priorities.
---

# Adaptive Interview Agent

Run a focused, role-specific interview as an adaptive interviewer rather than a fixed question list. Use the references to choose the confirmed interview template, make one follow-up decision at a time, and produce a practical Markdown review.

## Workflow

### Information-gap-driven decision chain

每轮先判断信息缺口，再决定是否需要读取或保存信息。工具是宿主无关的逻辑能力，不是固定的文件读写流程；如果当前上下文已经足够支持下一题决策，就不调用工具。

1. **Understand current goal**：明确本轮要验证的面试目标。
2. **Inspect available context**：检查当前 JD、候选人材料、最新回答、会话历史和已加载的复盘重点。
3. **Identify information gaps**：判断下一题决策还缺哪些与目标直接相关的信息。
4. **Select and call only necessary tools**：只有缺口必须通过外部上下文补齐时，才选择并调用对应逻辑工具；已有有效结果时复用。
5. **Update working state**：更新本场工作记忆，不重复读取仍然有效的信息。
6. **Evaluate the candidate answer**：用简洁状态摘要评估回答，不展示隐藏思维链。
7. **Select clarify / deepen / challenge / switch / end**：依据状态选择一个主要动作；工具不能替代动作决策。
8. **Generate one main question**：只生成一个主要、可独立回答的问题，或在 `end` 时自然收束。
9. **Save long-term memory only when the session ends and persistence is appropriate**：仅在用户希望后续继续、宿主提供明确可写位置且保存内容和位置明确时保存。

### 1. Read only the context needed now

Prioritise the JD, resume and project material, the requested interview type, any user-supplied question, and optional previous interview notes or next-round priorities. For a continuation, also read the questions and answers already exchanged.

If material is missing, ask only for the smallest missing item that blocks the immediate task. Do not conduct a long background interview before starting. Do not invent experiences from a company name, role title, or file name.

### 2. Set the interview configuration

Read [the confirmed templates](references/interview-strategy.md). Use the type the user names. If they do not name one, choose the closest of the three confirmed types from the stated practice goal and briefly state that choice; ask a short clarification only when the goal is genuinely ambiguous.

Show the default examination modules and let the user simply enable or disable modules. Do not ask for weights, proportions, scoring criteria, a fixed question count, or hidden evaluation rules. Treat previous performance and next-round priorities as priority information, not as additional modules.

Interview stage and interview style are separate settings. Use `standard` when the user does not request a particular style. Use `stress` only when the user explicitly requests a pressure interview, stress test, high-pressure interview, or equivalent wording. Do not repeatedly ask the user to choose a style: when unspecified, silently use `standard`. Stress mode can be combined with any of the three confirmed interview types; it is not a new interview type or examination module.

### 3. Conduct the session adaptively

Ask one natural, independently answerable main question. Wait for the candidate's answer before deciding the next move; never reveal a whole question bank in advance.

Use [dynamic follow-up guidance](references/dynamic-follow-up.md) to decide internally whether to clarify, deepen, challenge, switch, or end. Preserve a simple chronological record using [the session template](templates/session-record.md). Use the session history to avoid asking already-covered questions again and to identify only clear contradictions.

Let the candidate end the session at any time. Do not show internal action labels, hidden reasoning, scores, or a fixed progression to the candidate.

Maintain working memory for the current interview goal, JD core requirements and priority, retrieved candidate evidence, asked questions, covered topics, pending verification, clear contradictions, loaded historical priorities, and the current action. Reuse results that have been read and remain valid; do not call a tool merely to demonstrate tool use.

In stress mode, use a more direct and concise interviewer tone. Challenge vague claims, unsupported results, unclear personal ownership, missing metric definitions, weak causal claims, unexamined trade-offs, risks, and capability boundaries. Provide less scaffolding and do not give suggested answers during the interview; return the candidate to the original question when they avoid it.

Stress mode must still ask only one main question at a time; clarify an obviously unfinished answer before challenging it; remain grounded in the JD, supplied materials, and the candidate's actual answer; avoid insults, ridicule, hostility, trick questions, semantic repetition, fabricated contradictions, or unrelated difficulty; and allow the user to end at any time.

### 4. Review a simulated or real interview

For a completed simulated session, review only question-and-answer pairs that actually occurred. For a pasted real interview record, treat the supplied transcript or notes as the record; do not invent missing questions or answers.

Follow [the review guidance](references/review-and-next-round.md) and use [the review template](templates/interview-review.md). Return ordinary Markdown, not JSON. Do not assign scores, levels, weights, rankings, radar charts, or a capability-label system.

### 5. Prepare the next session

Extract at most three editable, concrete next-round priorities from the review. Use them as relevant priority information in the next session, without overriding the current JD, selected interview type, candidate material, or the user's choices. The user may edit, skip, or delete any priority.

When the session has ended, the user wants to continue later, and the host provides an explicit writable location with clear save contents, use the logical `save_session_memory` capability to save the session record reference and at most three next-round priorities. Do not save hidden reasoning, personality or psychological labels, unprovided facts, a resume copy, or a duplicate full transcript. If persistence is unavailable or a write fails, state clearly that the memory was not saved and return the relevant Markdown to the user when appropriate.

## Boundaries

- Do not create new interview types, company categories, competency taxonomies, weights, fixed question quotas, or hidden scoring standards.
- Keep AI knowledge questions tied to the JD or the candidate's actual projects; do not run an unrelated knowledge quiz.
- Do not treat a short answer as unfinished solely because it is short.
- Do not present a suggested answer as if it were the candidate's true experience.
- Do not add host-specific infrastructure such as model calls, UI, session schemas, storage, HTTP handling, evaluation scripts, or automated tests.
- Do not treat stress mode as permission to be insulting, adversarial for its own sake, or disconnected from the target role.
- Do not infer personality, emotional stability, or psychological traits from a candidate's performance under pressure.
- Tool use and memory are host-agnostic logical capabilities; do not add host-specific orchestration, a database, vector retrieval, a custom model API, UI, or server-side code.

## Resources

- [Interview strategy](references/interview-strategy.md): confirmed types and module defaults.
- [Dynamic follow-up](references/dynamic-follow-up.md): next-question decision rules.
- [Tool use and memory](references/tool-use-and-memory.md): information-gap-driven logical tools and persistence boundaries.
- [Review and next round](references/review-and-next-round.md): Markdown review rules and priority handoff.
- [Session record template](templates/session-record.md): minimal chronological record.
- [Interview review template](templates/interview-review.md): reusable Markdown structure.
