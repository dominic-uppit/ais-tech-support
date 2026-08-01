# Diagnostic — Claude Code Context Window & Token Limits

Use this when a student is hitting context/token problems: "out of context", "stuck in /compact loop", "hit my 5h or weekly limit", "10K tokens used before I even started", "extra usage required for 1M context", "should I switch to Opus / Qwen / OpenRouter to save money", "MCP eating all my context". Covers taxonomy sections 3 (Context Window) plus the related model-switching pieces from 6.1 and 6.3.

**Key insight to anchor on**: most "out of context" problems come from one of three things — being on the small-context model tier (Haiku), too many MCPs loaded at session start, or aggressive `/compact` shredding the conversation. Diagnose by running `/context` and reading the breakdown BEFORE doing anything else. ⚠️ Check `/model` for the current lineup rather than quoting context sizes — they change with every release.

**Related lessons** (point students here when they're missing the underlying concepts, not just the fix):
- `Claude Code → Phase 2 → 1.10 Token Management` — five essential prompting patterns for staying within token limits.
- `Build Your Portfolio → RAG & Vector DBs → Context Windows` — basic concept of what a context window is, token-to-word ratio, why larger windows matter. (Counterintuitively filed under RAG, not at the top level — students won't find it by browsing.)
- `Build Your Portfolio → RAG & Vector DBs → Tokenization & Text Processing` — what a token actually is.

---

## Step 1 — Identify the specific symptom

Match the student's wording to one of these branches:

| What they're saying | Branch |
|---|---|
| "Hitting 5h / weekly limit too fast", "out of credits", "session ended early" | Step 2 — Rate limits |
| "Out of context", "Claude getting confused mid-task", "context % at 95" | Step 3 — Mid-task context exhaustion |
| "/compact keeps running but I'm stuck", "fill→compact→fill→repeat" | Step 4 — Compact loop |
| "Extra usage required for 1M context", "API error on every new chat" | Step 5 — 1M context error |
| "10K tokens in tools before I even start", "MCP eating context" | Step 6 — Resting baseline bloat |
| "Should I use Opus instead of Sonnet?", "switching models" | Step 7 — Model switching |
| "Want to use Qwen / OpenRouter / DeepSeek for free" | Step 8 — OpenRouter setup |
| "Should I /clear or /compact?" | Step 9 — `/clear` vs `/compact` decision rule |

If the symptom doesn't match any of these, Escape Hatch B at the bottom.

---

## Step 2 — Hitting rate limits too fast (5h / weekly)

Symptom: "My session got to its limit way earlier than expected", "Why am I burning credits so fast".

Things to check, in order:

1. **Claude Design now shares the same token pool as Claude Code.** This is recent — used to be separate buckets. If they've been using Claude Design, that draws from the same limit.
2. **MCPs at rest cost tokens before any chat happens.** Run `/context` inside Claude Code → look at "MCP tools" line. If it's >5K, they have too many MCPs loaded. See Step 6 to trim.
3. **CLAUDE.md size**: keep it under ~200 lines. If it's grown into a multi-thousand-line dump, every turn pays that cost. Trim it; use it as an index pointing at separate `roadmap.md`, `handoff.md` etc.
4. **Skills loaded everywhere**: each loaded skill costs context per session. Put skills in `.claude/skills/` (project-scope) instead of `~/.claude/skills/` (user-scope) so they only load in projects that need them.
5. **Heavy tool outputs in conversation history**: reading large emails, HTML pages, big files keeps that in the running context for the rest of the session. Filter to plain text, spill big outputs to disk, `/clear` between gather and write phases.
6. **Production traffic on a Max subscription**: a personal Claude subscription is meant for that person's own use, not for serving a client's users. Production belongs on API keys from `console.anthropic.com`, billed separately. Community members report Anthropic tightening enforcement against third-party tools that routed user requests through subscription OAuth. ⚠️ Don't quote a specific enforcement date or assert what the terms say — point the student at Anthropic's current [Usage Policy](https://www.anthropic.com/legal/aup) and [Consumer Terms](https://www.anthropic.com/legal/consumer-terms) and let them read the current wording.

<!-- pattern: /is-claude-design-using-my-normal-session-tokens, /running-into-claude-limits-whats-your-token-saving-strategy, /no-running-out-of-credit-with-claude-code, /helpdesk-ai-agent, /claude-code-20-plan-vs-claude-api-costing -->

---

## Step 3 — Mid-task context exhaustion

Symptom: "Context is at 90%+ and Claude is getting confused / losing the plot."

1. Run `/context`. Read the breakdown — distinguish:
   - **System tools** (Claude's built-ins, ~10.6K is normal — can't reduce)
   - **MCP tools** (each loaded MCP)
   - **Messages** (conversation history including tool outputs)
   - **Files in context** (anything pinned with `@filename`)
2. **If Messages is bloated**: classic case. Time to checkpoint and clear:
   - Ask Claude: "Update `CLAUDE.md`, `roadmap.md`, `change-log.md`, `handoff.md` with current state and next steps."
   - Commit to git: `git add . && git commit -m "wip"` (keeps the work safe).
   - Run `/clear`.
   - In the new fresh session, tell Claude "Read handoff.md and continue from there" — it picks up with the right context but a clean slate.
3. **If MCP tools is bloated**: trim MCPs. See Step 6.
4. **If Files in context is bloated**: they pinned too many files with `@`. Unpin via `/context` controls.
5. **Don't `/compact` aggressively** — see Step 4 + Step 9.

<!-- pattern: /context-window-max-capacity, /claude-code-context -->

---

## Step 4 — The `/compact` loop

Symptom: "I hit context limit, /compact summarizes, I resume, fills up immediately, I /compact again, no progress."

The corpus is clear: **`/compact` is the wrong tool when you're in a loop.** Antipatterns to call out:

- `/compact`: **at most once per session**, used deliberately mid-task — not as a habit (support-team guidance). Prefer `/clear` + handoff.md between tasks. Repeated `/compact` is what shreds a session.
- Repeated `/compact` calls strip out nuance and create an increasingly stale summary — the model loses important context with each pass.

The actual fix:

1. **Check the model.** Run `/model` and read what the student is actually on. Haiku is the small-context tier, so if they're on Haiku, moving to Sonnet or Opus is usually the biggest single jump.
   - ⚠️ Model context windows change with every release, and a 1M variant may require "extra usage" enabled on the Anthropic account (see Step 5). **Never quote a context size from memory — have the student run `/model` and read the current lineup**, or check the model docs. Don't tell a student a specific model is "200K" or "1M" without checking.
   - Use Opus when you've hit a reasoning ceiling, not just a context one.
2. **Trim MCPs**. `claude mcp list` → identify unused. `claude mcp remove <name> -s project` (or `-s user`). Restart Claude Code.
3. **Spawn a subagent for the specific bug.** Subagents get their own context window — your main thread doesn't have to absorb the debug spiral. Use the Task tool or `Spawn a subagent` prompt.
4. **Switch to `/clear` + handoff.md**:
   - Before clearing, ask: "Update CLAUDE.md, roadmap.md, change-log.md, handoff.md with where we are and what's next."
   - Commit to git.
   - `/clear`.
   - New session: "Read handoff.md and continue."

<!-- pattern: /context-window-max-capacity, /claude-code-context -->

---

## Step 5 — "Extra usage required for 1M context" error

Symptom: "Every new chat in VS Code throws an API error. Switching model and typing 'go' makes it work."

- **Cause**: The extension is defaulting to a 1M-context model variant. 1M context requires "Extra Usage" credits enabled on the account.
- **Fix options (pick one)**:
  1. **Enable extra usage** at `https://claude.ai/settings/usage`. Then 1M variants work.
  2. **Pin a standard-context model** as default via `/model` — pick the non-1M variant. Check `/model` for the current labels in your lineup.
  3. **If it still errors after enabling extra usage**: there's a known intermittent VS Code extension bug. Roll back the extension per `diagnostics/vscode-extension-broken.md`.

<!-- pattern: /api-error-in-vs-code-switching-model-helps -->

---

## Step 6 — Resting baseline too high (10K+ before chatting)

Symptom: "I haven't done anything but I'm already at 10% context."

Normalize first: **6-10% baseline is normal**. Claude Code's built-in system tools alone are ~10.6K tokens. That's not a bug.

But if it's >15% before any work:

1. Run `/context` and read the breakdown.
2. **If "MCP tools" is the bloat**:
   - `claude mcp list` → see what's loaded.
   - Disable per session: `claude mcp disable <name>`.
   - Better: remove project-scope ones you don't need: `claude mcp remove <name> -s project`.
   - Note: disabling alone may still load tool definitions for some MCPs. Removing is more reliable.
3. **Known offenders**:
   - **Smartlead MCP** loads 60+ tools by default. Quickest fix: in Claude Desktop click `+` → Connectors → toggle Smartlead off for general chats. In Claude Code: `claude mcp disable smartlead`. For per-tool control, untick individual tools in connector settings (painful at 100+ tools).
   - **n8n-mcp** itself is only ~2K — if students report it as the bloat, they're usually misreading `/context` (the 10K is Claude's system tools, not n8n-mcp).
4. **If "Messages" is bloated on a fresh session**: probably an autoloaded `CLAUDE.md` that's gotten too big. Trim it to <200 lines, move details into separate files.

<!-- pattern: /i-have-10k-tokens-in-tools-with-my-n8n-mcp-running, /issue-with-claude-code-context-window-in-vs-code, /quick-question-smartlead-claude-mcp, /disabling-unused-mcp-servers -->

---

## Step 7 — When to switch Sonnet → Opus

Symptom: "Sonnet keeps fixing one thing and breaking another", "Should I use Opus?"

Switch to Opus when you see:

1. **Logic loops** — fix A breaks B, fix B breaks A.
2. **Quality drops** — output not improving after multiple correction rounds.
3. **Band-aid architecture** — Claude taking shortcuts or building fragile workarounds.
4. **Time-to-value getting worse** — each turn helps less than the previous.

How: `/model` → pick Opus tier. Switch back to Sonnet for normal building once you're unstuck (Opus is more expensive).

Switch to Opus for a **reasoning** ceiling. For a **context** ceiling, check `/model` for which variants are on offer and whether a 1M option needs extra usage enabled (Step 5) — ⚠️ don't assert a specific model's context size from memory; the lineup changes with every release.

**Don't switch to Opus reactively for every issue** — the current Sonnet handles most work fine. Opus is for when you've actually hit Sonnet's ceiling.

<!-- pattern: /switching-between-models-claude-code -->

---

## Step 8 — OpenRouter + Qwen / DeepSeek for free or cheap

Symptom: "Trying to use Claude Code with free models", "may not have access" error, "want to save money".

The corpus-verified setup (community-verified pattern from a detailed solved thread):

1. **OpenRouter privacy setting**: Settings → Privacy → enable **"free endpoints that may train on inputs"**. Without this, `:free` endpoints reject your requests.
2. **Use `ANTHROPIC_AUTH_TOKEN`, NOT `ANTHROPIC_API_KEY`** for the OpenRouter key. This is the single most common mistake.
3. **Explicitly null the other**: `ANTHROPIC_API_KEY=""` so Claude Code doesn't silently fall back to it.
4. **Base URL**: `openrouter.ai/api` — **NOT** `/api/v1`. The `/v1` is appended by Claude Code.
5. **Background calls**: also set `ANTHROPIC_DEFAULT_HAIKU_MODEL` to a valid free slug. Background tasks (commit messages, etc.) need a working model or they error.
6. **Verify slugs are still live** at `https://openrouter.ai/models` — slugs change frequently, especially for free Qwen/DeepSeek variants.

Example `.env` setup (replace with current valid slugs):
```
ANTHROPIC_AUTH_TOKEN=sk-or-v1-...your-openrouter-key...
ANTHROPIC_API_KEY=
ANTHROPIC_BASE_URL=https://openrouter.ai/api
ANTHROPIC_MODEL=qwen/qwen-2.5-coder-32b-instruct:free
ANTHROPIC_DEFAULT_HAIKU_MODEL=qwen/qwen-2.5-coder-32b-instruct:free
```
⚠️ `sk-or-v1-...your-openrouter-key...` is a placeholder — the student's real key comes from openrouter.ai → Keys. Tell them explicitly to replace it (in the `.env` file, never pasted into chat), and verify the model slugs are current before showing this block.

**Honest limitation**: free Qwen gets flaky on agentic tool-use loops. The free tier caps daily requests low; buying a small amount of credit on OpenRouter lifts it substantially (roughly 50 → 1000/day when this was written), which is the difference between "toy" and "actually usable". Verify current limits at openrouter.ai/docs — quotas change.

<!-- pattern: /using-claude-code-in-vs-code-via-openrouter-and-qwen-free -->

---

## Step 9 — `/clear` vs `/compact` decision rule

The cleanest decision rule in the corpus (community-verified playbook):

| Context % | Same task? | Action |
|---|---|---|
| < 50% | Yes | Keep going, no action needed |
| 50–80% | Yes | Consider `/compact` (still rarely — try checkpoint+clear first) |
| > 80% | Yes | `/clear` (with handoff.md) OR `/compact` before next major step |
| Any % | No (new unrelated task) | `/clear` regardless of context % |

Antipatterns to push back on (from corpus-notes):

- **"I'll `/compact` to manage context"** as a habit → support-team guidance: max once per session. Use `/clear` + handoff.md.
- **"I'll dump all my skills into every project"** → each loaded skill costs context, load only relevant.
- **Long CLAUDE.md as a fix for everything** → keep it slim, point to other files. Imports expand inline; they don't save tokens.

<!-- pattern: /claude-code-context -->

---

## Step 10 — General context hygiene to teach the student

If they're hitting these problems repeatedly, the underlying habits need work. Top recommendations from the corpus:

1. **Commit to git at every working point.** Cheap backup; lets you `/clear` freely without losing work.
2. **Always write handoff.md before clearing** — ask Claude: "Write `.claude-handoff.md` with current state, what's next, open questions."
3. **Use Plan mode (Shift+Tab) for big tasks** — Claude lays out the plan before doing edits, you approve, edits get gated.
4. **Use subagents for isolated tasks** — debugging, research, summarization. They get their own context.
5. **Slim CLAUDE.md** (<200 lines). Use it as an index pointing at `roadmap.md`, `change-log.md`, `architecture.md`.
6. **Project-scope skills**, not user-scope, unless a skill is genuinely needed everywhere.
7. **For long-running token savers**, use prompt caching with the API where possible.
8. **For production work, use API keys** (`console.anthropic.com`), not your personal Claude Code subscription. Different billing, no shared rate limit.

---

## If none of the branches matched

Switch to drafting a Support Needed post (see SKILL.md Escape Hatch B). Pre-fill with:

- **Exact symptom** (verbatim error or behavior)
- **Output of `/context`** (the breakdown is the most useful data point)
- **Output of `/model`** (which model + variant they're on)
- **Output of `claude mcp list`** (so answerers can spot bloat)
- **Operating system + Claude Code version** (`claude --version`)
- **CLAUDE.md size** (rough line count is fine)

No need to tag anyone — the support team watches Support Needed‼️ and a well-formed post gets picked up.
