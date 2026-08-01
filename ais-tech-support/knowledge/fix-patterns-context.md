# Fix Patterns — Claude Code — context & usage limits

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

See also `diagnostics/claude-code-context.md`, which is the routed entry point for context and usage-limit problems.

---

## CLAUDE CODE — Context & Limits

## Context window already at 8-10% on empty new project
**Symptom**: "I haven't done anything but I'm already at 10% context usage."

**Root cause**: Claude Code's hidden system prompts + any globally-loaded MCPs/skills get counted into the resting baseline.

**Fix steps**:
1. This is normal — 6-10% baseline.
2. To reduce: run `claude mcp list` and identify MCPs you don't need for the current project.
3. Disable per-session: `claude mcp disable <name>` (note: disabling alone may not stop tool definitions from loading; removing is more reliable).
4. Remove project-scoped MCP: `claude mcp remove <name> -s project`.
5. Run `/context` inside Claude Code to see the breakdown of where the tokens are going.

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/issue-with-claude-code-context-window-in-vs-code
- https://www.skool.com/ai-automation-society-plus/disabling-unused-mcp-servers

---

## n8n-mcp eating 10K+ tokens at session start
**Symptom**: "n8n MCP is showing 10k tokens used in tools." But the actual context bloat is usually somewhere else.

**Root cause**: The n8n-mcp itself only uses ~2K. The 10K seen is usually Claude Code's built-in system tools (10.6K is normal). Real context bloat usually comes from large tool outputs (e.g., reading email HTML, large file reads).

**Fix steps**:
1. Run `/context` and read the breakdown — distinguish "System tools" from "MCP tools" from "Messages".
2. If MCP tools are the bloat: turn off non-essential MCPs (`claude mcp disable`).
3. If Messages are the bloat: filter heavy outputs (HTML→plain text), `/clear` between gathering and writing phases, spill bodies to disk for analysis instead of holding in conversation.
4. Long-term: switch to a model with a larger context window. ⚠️ Context sizes change with every model release — have the student run `/model` in Claude Code and read the current lineup rather than quoting a number. A 1M variant may require "extra usage" enabled on the Anthropic account.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/i-have-10k-tokens-in-tools-with-my-n8n-mcp-running

---

## Stuck in the `/compact` loop (fill→compact→fill→repeat)
**Symptom**: "Hit 0% context, /compact summarizes, I resume, fills up immediately, no progress."

**Root cause**: Wrong model (Haiku is the small-context option), too many MCPs loaded, debugging the wrong node, /compact destroying important nuance.

**Fix steps**:
1. **Switch model**: run `/model` and read the current lineup — Haiku is the small-context option, so moving off it is usually the biggest single jump. ⚠️ Don't quote a specific model's context size from memory; sizes change with every release, and a 1M variant may require "extra usage" enabled on the Anthropic account.
2. **Kill unused MCPs — and mind the scope.** Run `claude mcp list` to see every server and the scope it is registered at. For anything registered at project scope, run `claude mcp remove <server-name> -s project` — replace `<server-name>` with the exact name shown by the list command. Removing without the scope flag only touches user/local scope, which is why servers you thought you removed (Notion, Trigger.dev) come back every session and eat tens of thousands of tokens before you type anything.

   **Remove rather than disable.** A disabled MCP server still loads its tool definitions into your context window.
3. **Spawn a subagent** for the specific bug (subagents get their own context window).
4. **Stop using /compact as a habit** — support-team guidance is at most once per session, used deliberately mid-task; otherwise /clear + handoff.md:
   - Before /clear, ask Claude: "Update CLAUDE.md, roadmap.md, change-log.md, handoff.md with current state and next steps."
   - `/clear` → new session reads those docs to start with the right context.
- **Give every task an exit ritual.** End the task by having Claude Code update the project docs (CLAUDE.md, roadmap, changelog, handoff notes), then run `/clear` so the next task starts in a fresh session. Working prompt: ask Claude Code to create docs capturing the current state of the project, the changes made, and the planned next steps.
- **If you are already too close to the limit** for a full doc update (it would trigger auto-compact), have Claude Code write just a handoff file, then `/clear` and pick up from it.
- **Keep CLAUDE.md slim** — around 200 lines maximum. Past that, use it as an index pointing at your other project docs.

**Confidence**: high — team-verified; the scope-flag detail is the part students most often miss

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/context-window-max-capacity
- https://www.skool.com/ai-automation-society-plus/claude-code-context

---

## When to use `/clear` vs `/compact`
**Symptom**: "Conversation is long but only 15% context, should I reset?"

**Root cause**: Conflating message count with context usage.

**Fix steps** (community-verified playbook):
- `< 50%` context + same task → keep going.
- `50–80%` context → consider `/compact`.
- `> 80%` context → `/clear` or `/compact` before next major step.
- New unrelated task → `/clear` regardless of context %.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-code-context

---

## "Extra usage required for 1M context" error on every new chat
**Symptom**: Opening a new chat in VS Code Claude Code throws an API error; switching model and typing "go" makes it work.

**Root cause**: Extension defaults to 1M-context model, which requires extra-usage credits enabled on the account.

**Fix steps**:
1. Enable extra usage at https://claude.ai/settings/usage.
2. OR pin a standard-context model as your default via `/model` (pick a variant without the 1M tag — check `/model` for the current labels).
3. If still failing, roll back the VS Code extension version (known intermittent bug).

**Confidence**: medium

**Example thread**: https://www.skool.com/ai-automation-society-plus/api-error-in-vs-code-switching-model-helps

---

## "API Error 400 — Accept Consumer Terms" infinite loop
**Symptom**: Every prompt returns "You'll need to accept them in claude.ai with the email in /status to continue", but logging in doesn't prompt for acceptance.

**Root cause**: SSO login (Google) silently skips ToS prompt. The email under `/status` may not match the account that needs to accept.

**Fix steps**:
1. Inside Claude Code, run `/status` to see which email it's using.
2. Log into claude.ai with **that exact email**, not whichever SSO is auto-logging.
3. If ToS prompt still doesn't appear, try logging in via `console.anthropic.com` instead.

**Confidence**: medium

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-error

---

## "Failed to start Claude's Workspace" on Windows
**Symptom**: Claude Code or Claude Cowork shows "Failed to Start Workspace", reinstall doesn't fix.

**Root cause**: Either Anthropic outage, or Hyper-V/WSL conflict.

**Fix steps**:
1. Check https://status.claude.com / https://status.anthropic.com first — if there's an incident, just wait.
2. If not an outage and on Windows: enable Hyper-V via "Turn Windows features on or off".
3. Solved-post template walkthrough at https://www.skool.com/ai-automation-society-plus/solved-claude-failing-to-start-workspace

**Confidence**: medium

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/claude-failing-to-start-workspace
- https://www.skool.com/ai-automation-society-plus/solved-claude-failing-to-start-workspace

---

## Hitting Pro/Max usage limits too fast — usage hygiene checklist (and when it isn't you)
**Symptom**: Students burn through the 5-hour or weekly limit mid-course, or hit it after one or two prompts with no change in how they work.

**Root cause**: Usually baseline bloat plus long sessions: every message reprocesses the whole conversation, and a bloated CLAUDE.md plus always-on MCP servers can consume a large share of the budget before you type anything. But not always — the community has also seen genuine platform-side changes to session/usage windows that hit users who changed nothing, so rule that in before assuming it's the student.

**Fix steps**:
1. Run /context on a fresh session first. If it already shows a large chunk consumed before you type, the problem is your baseline, not your prompts.
2. Prune MCP servers to the ones the current project actually needs — each connected server loads its full tool list whether you call it or not, and the popular ones are heavy. Remove and re-add per project rather than leaving everything on.
3. Turn on Tool Search so MCP tool descriptions load lazily instead of all at startup (the setting a team member named is enable_tool_search set to auto, shipped in Claude Code 2.1.7). This reclaims a large share of startup context for most people.
4. Keep CLAUDE.md concise — the team's stated ceiling is about 200 lines.
5. New session per task: /clear when switching to unrelated work, and if you do compact, do it manually once around the halfway mark rather than waiting for auto-compact (still at most once per session).
6. Plan inside Claude Code's plan mode rather than shuttling prompts through other LLM chats — plan mode takes no heavy actions, and Claude Code already has your project context, so the small extra usage buys much better output.
7. Watch Nate Herk's video '18 Claude Code Token Hacks in 18 Minutes' for the full list.
8. If usage collapsed overnight with no change on your side, check whether others are reporting the same before upgrading — plan changes have been rolled out platform-wide before. Only consider upgrading after all of the above is applied.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/claude-limit
- https://www.skool.com/ai-automation-society-plus/claude-suddenly-eating-up-your-usage
- https://www.skool.com/ai-automation-society-plus/whats-your-best-practice-for-planning-with-claude-code-vs-claude-mobile-app
- https://www.skool.com/ai-automation-society-plus/context-window-filling-quick-af

<!-- pattern: /claude-limit -->

---

## Claude Design now draws from the same usage pool as Claude Code
**Symptom**: Claude Code limits hit much earlier while also using Claude Design; the separate Design usage bar disappeared.

**Root cause**: Anthropic merged Design's separate weekly bucket into the shared pool with chat and Claude Code; design prompts are heavy.

**Fix steps**:
1. Budget Design and Code together; build the design system once and reuse it.
2. Don't stack heavy Design and Code pushes back to back; schedule big design work right after a limit reset.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/is-claude-design-using-my-normal-session-tokens

<!-- pattern: /is-claude-design-using-my-normal-session-tokens -->

---

## Rate limits when bulk-processing PDFs — OCR + pdftotext + batched fresh sessions
**Symptom**: Claude Code hits API rate limits analyzing a large PDF set even on Max, even 'one file at a time with pauses'.

**Root cause**: Token volume, not request frequency: PDFs process as text AND image (~1.5-3K tokens/page), the file reader inflates tokens ~70%, and stateless requests re-send the whole conversation; 'wait 15 seconds' does nothing (no internal clock).

**Fix steps**:
1. OCR scanned PDFs first, then pre-extract text locally with pdftotext (poppler-utils/xpdf) and point Claude Code at the .txt files.
2. Process in batches of 10-15 with a report per batch and a FRESH session per batch; compile the master report from the per-batch reports in a final session.
3. Run /compact manually once if context grows mid-batch (not repeatedly — start a fresh session instead).

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/api-rate-limit-error-issue

<!-- pattern: /api-rate-limit-error-issue -->

---

## Task bigger than the context window (error code 3) — sub-agent fan-out with checkpoint file
**Symptom**: A skill processing a large document dies with error code 3 / context limit before finishing.

**Root cause**: The whole job runs through one context window and all intermediate output piles into the main conversation.

**Fix steps**:
1. Main agent only extracts the work items; create a focused agent file at .claude/agents/<agent-name>.md (replace <agent-name> with a task-specific name) that processes one batch and writes results to a file.
2. Spawn one sub-agent per BATCH (each spawn costs ~12-16K tokens of overhead), returning only summaries.
3. Set model: haiku in the agent frontmatter for cost (keep its tool list minimal), and append to a checkpoint file (e.g. verified_batches.json) so failed runs resume.
4. Anthropic's Batch API (flat 50% discount, async) suits very large offline jobs.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/how-do-you-handle-tasks-that-blow-past-the-context-window

<!-- pattern: /how-do-you-handle-tasks-that-blow-past-the-context-window -->

---

## Claude Code hangs ingesting multiple GitHub repos at once
**Symptom**: Asked to load several repos into CLAUDE.md in one prompt, Claude sits 'loading' 45+ minutes.

**Root cause**: Multiple repositories in one request overload context and stall the session; normal processing takes seconds to a minute.

**Fix steps**:
1. Close and reopen the session, then clone ONE repo, update CLAUDE.md to reference it, verify, and repeat per repo.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claudemd-help

<!-- pattern: /claudemd-help -->

---

## 'Claude seems dumber' — product-layer bugs; update the CLI and check announcements
**Symptom**: Claude/Claude Code feels dumber, lazier, or more forgetful over a period of weeks.

**Root cause**: Anthropic published a postmortem (April 23) confirming three separate product-layer bugs in Claude Code — a lowered default reasoning effort, a cache optimisation that wiped thinking blocks between turns, and a system-prompt instruction that capped response length too aggressively. All three were fixed in Claude Code v2.1.116; model weights were not changed. Context rot from very long single sessions compounds the impression.

**Fix steps**:
1. Update Claude Code to the latest version (`npm install -g @anthropic-ai/claude-code`, then `claude --version` to confirm you are on v2.1.116 or newer) — the three bugs are fixed as of that build.
2. Start fresh sessions more often instead of marathoning one long chat; context rot on long conversations is the part an update won't fix.
3. Before concluding your own setup broke, check Anthropic's engineering blog and their @ClaudeDevs account on X for product-layer announcements.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-seems-dumber

<!-- pattern: /claude-seems-dumber -->

---
