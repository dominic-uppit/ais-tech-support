# Problem Taxonomy — AIS+ Support-Needed Channel (780+ distinct threads)

This taxonomy is grep-able and structured for maintaining the `/ais-tech-support` skill. Categories are roughly ordered by priority (Claude Code + n8n/MCP first). Counts are approximate; threads can match more than one category.

**This is a coverage index, not a fix corpus — it records *what the corpus has seen*, not *how to fix it*.** Entries carry a symptom, a thread count, a `Verified:` verdict and example thread URLs; the fix steps live in the `knowledge/fix-patterns-*.md` files and `diagnostics/`. Do not load this file to route a problem — SKILL.md's routing table already does that. Load it (or better, grep it) only to check whether the corpus has seen a problem that matches no routing bucket, or for the two topics with no fix-pattern coverage: **§ 11 Voice Agents** and **§ 13 Web Scraping**, whose `Verified:` lines do carry the technical substance.

Key for `verified`:
- **yes** = at least one thread in the corpus shows a working fix that the OP confirmed
- **partial** = community gave plausible fixes but OP either didn't confirm or no fix shown
- **no** = no fix in corpus (mostly questions / discussions)

---

## 1. CLAUDE CODE — Setup & Installation

### 1.1 `claude: command not found` after install (Mac/Linux)
- Symptom: "I typed claude and it says command not found", "terminal doesn't know what claude is"
- Count: ~8
- Verified: yes
- Examples:
  - https://www.skool.com/ai-automation-society-plus/introduction-to-mcp-servers-in-cloud-code-problem
  - https://www.skool.com/ai-automation-society-plus/failed-to-setup-md-file

### 1.2 PATH not picking up `claude` binary after install
- Symptom: "claude --version still says not found in a new terminal"
- Count: ~5
- Verified: yes (`export PATH="$HOME/.local/bin:$PATH"`)
- Example: https://www.skool.com/ai-automation-society-plus/introduction-to-mcp-servers-in-cloud-code-problem

### 1.3 Windows install — no admin rights / corporate machine
- Symptom: "Node install requires admin and I don't have it"
- Count: ~4
- Verified: yes (Scoop, portable Node install)
- Examples:
  - https://www.skool.com/ai-automation-society-plus/install-mcp-server-problem-with-nodejs
  - https://www.skool.com/ai-automation-society-plus/how-to-sync-claude-work-across-2-laptops-no-admin-on-one

### 1.4 Windows install — using the PowerShell installer
- Symptom: "I have VS Code extension but no CLI", VS Code extension errors at the bottom-right
- Count: ~3
- Verified: yes (`irm https://claude.ai/install.ps1 | iex`)
- Example: https://www.skool.com/ai-automation-society-plus/failed-to-setup-md-file

### 1.5 "Install Claude with no GitHub account" / no-prior-dev-setup
- Symptom: "I'm not technical, terminal terrifies me, can someone look at my screen"
- Count: ~6
- Verified: partial
- Examples:
  - https://www.skool.com/ai-automation-society-plus/installing-claude-with-no-github-account
  - https://www.skool.com/ai-automation-society-plus/im-lost-help

### 1.6 n8n MCP setup confusion / "the video doesn't match what I see"
- Symptom: "I followed the video and got 100x more files", "the menu doesn't show n8n API option". Almost all of these threads came from the old Claude Code lesson 1.4 (n8n MCP Server), which is removed from the classroom on 2026-09-30; no surviving lesson covers this setup. Fix steps: `diagnostics/lesson-1-4-n8n-mcp.md`
- Count: ~16 (biggest single setup pain point in the corpus) <!-- count refreshed 2026-07 -->
- Verified: yes (combination: install Node, install Homebrew on Mac, n8n needs paid Cloud or self-host to expose API)
- Examples:
  - https://www.skool.com/ai-automation-society-plus/help-with-section-14-n8n-mcp-server
  - https://www.skool.com/ai-automation-society-plus/section-14-in-the-claude-code-set-up
  - https://www.skool.com/ai-automation-society-plus/14-n8n-mcp-server-cant-find-api-n8n-key
  - https://www.skool.com/ai-automation-society-plus/n8n-mcp-connection
  - https://www.skool.com/ai-automation-society-plus/trouble-with-setting-up-n8n-mcp-server-in-vs-claude-code

### 1.7 Free-tier-of-n8n issue (no API menu visible)
- Symptom: "Video says go to settings → n8n API but I don't have that"
- Count: ~3
- Verified: yes — need paid Cloud plan, self-host, or use the newer "instance-level MCP" menu
- Example: https://www.skool.com/ai-automation-society-plus/not-seeing-the-n8n-the-menu-bar

---

## 2. CLAUDE CODE — VS Code Extension Bugs

### 2.1 Extension stops loading after auto-update (recurring known regression)
- Symptom: "All of a sudden my Claude icon doesn't work", "Cannot activate Claude Code extension", error at the bottom-right
- Count: ~7 (high-impact — every few weeks)
- Verified: yes — roll back to previous version, disable auto-update
- Examples:
  - https://www.skool.com/ai-automation-society-plus/all-of-a-sudden-claude-code-extension-in-vs-code-doesnt-work
  - https://www.skool.com/ai-automation-society-plus/cannot-use-the-claude
  - https://www.skool.com/ai-automation-society-plus/command-claude-vscodeeditoropenlast-not-found

### 2.2 `Bypass permissions` still asks for permission after recent update
- Symptom: "Bypass mode is on but Claude still asks every edit"
- Count: ~3
- Verified: yes — rollback to v2.1.77 (known issue #36168)
- Example: https://www.skool.com/ai-automation-society-plus/claude-code-keeps-asking-permission-to-edit-files-even-though-settings-are-correct

### 2.3 "Extra usage required for 1M context" API error in VS Code
- Symptom: "Every time I open a new chat I get an API error and have to switch model and type 'go'"
- Count: ~2
- Verified: partial — pin a non-1M model, enable extra usage, or wait for fix
- Example: https://www.skool.com/ai-automation-society-plus/api-error-in-vs-code-switching-model-helps

### 2.4 VS Code welcome message keeps appearing
- Symptom: "Every time I open VS Code, Claude's welcome page reappears"
- Count: ~1
- Verified: yes — open project folder via right-click → "Open with VS Code"
- Example: https://www.skool.com/ai-automation-society-plus/claude-code-issue

### 2.5 Claude Code dark panel / theme issue
- Count: ~1
- Verified: yes
- Example: https://www.skool.com/ai-automation-society-plus/claude-code-dark-panel

### 2.6 False "Rate limit reached" inside the extension on a Pro/Max subscription
- Symptom: "API Error: Rate limit reached" only in VS Code; the same account works fine in the terminal CLI; upgrading the plan doesn't help
- Count: ~2
- Verified: yes — the extension silently resets subscription info in its credentials file and bills you as free/API tier; `claude /logout` then `claude /login` from a system terminal, and unset any stale `ANTHROPIC_API_KEY`
- Examples:
  - https://www.skool.com/ai-automation-society-plus/api-error-rate-limit-reached
  - https://www.skool.com/ai-automation-society-plus/api-limit-reached-claude-code

### 2.7 Extension hangs "not responding" mid-response
- Symptom: a response stalls indefinitely; sometimes the stop button itself is stuck
- Count: ~1
- Verified: yes — known extension bug; stop and retry, or `Developer: Reload Window`; run the CLI if it is constant
- Example: https://www.skool.com/ai-automation-society-plus/no-responding

### 2.8 "Error while loading view: claudeVSCodePanel" after a folder rename
- Symptom: the chat panel errors in every chat and reloading VS Code doesn't help
- Count: ~1
- Verified: yes — the webview lost its workspace path; `Developer: Reload Webviews`, or File > Open Folder at the new path
- Example: https://www.skool.com/ai-automation-society-plus/vs-code-error-while-loading-view

### 2.9 Past sessions vanish after restart
- Symptom: all previous sessions disappear after a hard shutdown; new history stops being stored
- Count: ~1
- Verified: partial — reopen the project folder (not a blank window), set "restore windows" to all; a separate reported bug loses session storage on mapped network drives whose IP changes
- Example: https://www.skool.com/ai-automation-society-plus/vs-code-not-storing-past-sessions-any-longer

---

## 3. CLAUDE CODE — Context Window & Token Limits

### 3.1 Hitting weekly/5h limits too fast
- Symptom: "Session got to its limit super early"
- Count: ~6
- Verified: yes — Claude Design now shares pool, prompt caching tips, slim CLAUDE.md, use CLIs instead of MCPs
- Examples:
  - https://www.skool.com/ai-automation-society-plus/is-claude-design-using-my-normal-session-tokens
  - https://www.skool.com/ai-automation-society-plus/running-into-claude-limits-whats-your-token-saving-strategy
  - https://www.skool.com/ai-automation-society-plus/no-running-out-of-credit-with-claude-code

### 3.2 MCP eating context budget / "10K tokens in tools at rest"
- Symptom: "My context is at 10% before I even start"
- Count: ~6
- Verified: yes — Claude Code's built-in system tools = baseline ~6-10%, plus loaded MCPs/skills; turn off unused MCPs (`claude mcp disable <name>`), use ToolSearch / on-demand mode
- Examples:
  - https://www.skool.com/ai-automation-society-plus/i-have-10k-tokens-in-tools-with-my-n8n-mcp-running
  - https://www.skool.com/ai-automation-society-plus/issue-with-claude-code-context-window-in-vs-code
  - https://www.skool.com/ai-automation-society-plus/quick-question-smartlead-claude-mcp
  - https://www.skool.com/ai-automation-society-plus/disabling-unused-mcp-servers

### 3.3 Context-loop stuck on debugging — `/compact` not helping
- Symptom: "Fill up, compact, resume, fill up again, can't make progress"
- Count: ~4
- Verified: yes — switch to a larger-context model (run `/model` for the current lineup; don't quote a size from memory), kill unused MCPs, spawn a subagent for the bug, use `/clear` not `/compact`, write a handoff.md
- Examples:
  - https://www.skool.com/ai-automation-society-plus/context-window-max-capacity
  - https://www.skool.com/ai-automation-society-plus/claude-code-context

### 3.4 When to `/clear` vs `/compact`
- Count: ~3
- Verified: yes — context % matters more than message count; `/clear` for new task, `/compact` only mid-task and rarely
- Example: https://www.skool.com/ai-automation-society-plus/claude-code-context

### 3.5 Bulk-processing files blows the token budget (PDFs especially)
- Symptom: rate limits on Max even "one file at a time with pauses"; a skill dies with error code 3 mid-document
- Count: ~3
- Verified: yes — PDFs process as text AND image; pre-extract with pdftotext, batch 10-15 per FRESH session; for oversized single jobs, fan out to sub-agents with a checkpoint file
- Examples:
  - https://www.skool.com/ai-automation-society-plus/api-rate-limit-error-issue
  - https://www.skool.com/ai-automation-society-plus/how-do-you-handle-tasks-that-blow-past-the-context-window
  - https://www.skool.com/ai-automation-society-plus/claudemd-help

### 3.6 "Claude seems dumber" over a period of weeks
- Symptom: model feels lazier or more forgetful; students assume a silent model downgrade
- Count: ~1
- Verified: yes — Anthropic's April 23 postmortem confirmed three product-layer bugs (lowered default reasoning effort, a cache optimisation wiping thinking blocks, an over-aggressive response-length cap), all fixed in Claude Code v2.1.116; weights unchanged. Context rot from marathon sessions compounds the impression
- Example: https://www.skool.com/ai-automation-society-plus/claude-seems-dumber

---

## 4. CLAUDE CODE — CLAUDE.md / Skills / Subagents

### 4.1 Where to put CLAUDE.md / how the hierarchy works
- Symptom: "I already have a CLAUDE.md from before joining, will Claude Code overwrite it?"
- Count: ~9 <!-- count refreshed 2026-07 -->
- Verified: yes — user-level `~/.claude/CLAUDE.md` + per-project `./CLAUDE.md` stack; project wins on conflict; `/memory` lists all loaded
- Examples:
  - https://www.skool.com/ai-automation-society-plus/question-about-setting-up-claude-in-vs-code
  - https://www.skool.com/ai-automation-society-plus/where-to-keep-memory-md-files-for-new-projects-to-pick-them-up
  - https://www.skool.com/ai-automation-society-plus/claudemd-file
  - https://www.skool.com/ai-automation-society-plus/claude-code-architecture-folder-structure

### 4.2 "Where do md files go" / md folder structure
- Symptom: "Where's the link to Nate's md files mentioned in his video"
- Count: ~5
- Verified: yes — Google Drive folder, plus structural rules: CLAUDE.md at root, `.claude/skills/<name>/SKILL.md`, `.claude/agents/`
- Examples:
  - https://www.skool.com/ai-automation-society-plus/where-are-the-md-files-discussed-in-every-level-of-claude-code-explained-in-21-minutes
  - https://www.skool.com/ai-automation-society-plus/documents-missing-from-mastering-claude-code
  - https://www.skool.com/ai-automation-society-plus/cant-find-claude-md-for-wat-framework

### 4.3 What are skills, when do you need them, are they safe?
- Symptom: "Course says install n8n skills but I'm not sure what they do" (that was the old Claude Code lesson 1.5, removed from the classroom on 2026-09-30; no surviving lesson installs the n8n skills, so answer from the fix patterns)
- Count: ~10 <!-- count refreshed 2026-07 -->
- Verified: yes — use czlonkowski's n8n-skills (trusted source), skills don't replace CLAUDE.md
- Examples:
  - https://www.skool.com/ai-automation-society-plus/n8n-skills-in-claude-code
  - https://www.skool.com/ai-automation-society-plus/about-skills
  - https://www.skool.com/ai-automation-society-plus/n8n-skills
  - https://www.skool.com/ai-automation-society-plus/adding-skills-to-claude
  - https://www.skool.com/ai-automation-society-plus/claude-code-skills

### 4.4 `/plugin` command not working
- Symptom: "I want to install a plugin but `/plugin` does nothing"
- Count: ~3
- Verified: yes — outdated Claude Code; `npm install -g @anthropic-ai/claude-code@latest`, OR manually clone skill repo into `~/.claude/skills/`
- Example: https://www.skool.com/ai-automation-society-plus/need-help-connecting-n8n-mcp-server-on-hostinger

### 4.5 Multi-project / cross-project skill sharing
- Count: ~3
- Verified: yes — put skills in `~/.claude/skills/` for user-scope; project-scope = `.claude/skills/`

### 4.6 Auto-memory (MEMORY.md, feedback_*) leaking into wrong project
- Symptom: "Memory MD files from one project, do I copy them?"
- Count: ~2
- Verified: yes — auto-memory is per-project intentionally; graduate keepers manually into user CLAUDE.md
- Example: https://www.skool.com/ai-automation-society-plus/where-to-keep-memory-md-files-for-new-projects-to-pick-them-up

### 4.7 Skills don't improve on their own
- Symptom: corrections to a skill's output never carry over to the next run
- Count: ~1
- Verified: yes — skills are static files; keep permanent rules in SKILL.md, put case corrections in a sibling feedback-log.md the skill is told to check, and periodically consolidate
- Example: https://www.skool.com/ai-automation-society-plus/how-do-skills-improve-over-time-in-claude-code

### 4.8 claude.ai Projects vs Claude Code skills (non-coder confusion)
- Symptom: a non-coder feeds hours of business context into one long chat, gets shallow results, and finds the classroom SKILL.md templates too code-heavy
- Count: ~1
- Verified: yes — one claude.ai Project per business function; skills are universal standards, Projects are function-specific knowledge; SKILL.md is a developer surface
- Example: https://www.skool.com/ai-automation-society-plus/how-to-build-skills-relevant-for-me-and-my-business-in-claude

### 4.9 Third-party memory plugins eating startup context
- Symptom: after installing claude-mem, sessions start ~10% consumed and appear to show another project's observations
- Count: ~1
- Verified: partial — injection at SessionStart is by design; apparent cross-project leakage is usually the VS Code multi-window agent view, not memory. Genuine collisions happen only when two folders share a basename
- Example: https://www.skool.com/ai-automation-society-plus/claude-mem-eating-up-10-context-at-start-up

### 4.10 Where to find quality skill collections
- Symptom: "where do people share good skills for Claude Code / Antigravity?"
- Count: ~1
- Verified: partial — scattered across GitHub repos, directories and Reddit; many skills are cross-compatible between the two tools; vet before running next to live credentials
- Example: https://www.skool.com/ai-automation-society-plus/claudeantigravity-skills

---

## 5. CLAUDE CODE — Permissions / Security / Settings

### 5.1 Bypass permissions doesn't bypass PowerShell on Windows
- Count: ~2
- Verified: yes — `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`; better: keep bypass but add deny list in settings.json for destructive commands
- Examples:
  - https://www.skool.com/ai-automation-society-plus/bypass-permission-does-not-bypass-powershell

### 5.2 Where to draw the security line (curl/network)
- Count: ~3
- Verified: yes — deny list is advisory; real fix = egress allowlist at network layer (Squid proxy); use Doppler for secrets
- Examples:
  - https://www.skool.com/ai-automation-society-plus/where-to-draw-the-line-on-security-with-claude-code
  - https://www.skool.com/ai-automation-society-plus/claude-code-balance-between-security-and-autonomy
  - https://www.skool.com/ai-automation-society-plus/security-concerns

### 5.3 Asking Claude to never delete files via CLAUDE.md
- Symptom: "I'll put a rule in claude.md to not delete anything"
- Count: ~2
- Verified: yes — CLAUDE.md is advisory, not enforcement; deny rules in settings.json are the only enforcement
- Example: https://www.skool.com/ai-automation-society-plus/bypass-permission-does-not-bypass-powershell

### 5.4 Client API keys / credentials management
- Count: ~4
- Verified: yes — separate per-client n8n/CC accounts, .env per project, never commit, Doppler/Infisical for prod
- Examples:
  - https://www.skool.com/ai-automation-society-plus/clients-api-keys
  - https://www.skool.com/ai-automation-society-plus/clients-api-keys-and-password-management
  - https://www.skool.com/ai-automation-society-plus/client-credentials-2

### 5.5 "Can Claude Code read all my files?" — the containment ladder
- Symptom: students discover Claude Code reads files outside the opened folder and fear it can change anything on the machine
- Count: ~1
- Verified: yes — the Bash tool runs with the user's own OS permissions and parent-folder CLAUDE.md reads are by design; supervision is what gates risk. Ladder: keep bypass off → plan mode for read-only → deny rules → /sandbox → the official devcontainer for unattended runs
- Example: https://www.skool.com/ai-automation-society-plus/security-concerns

### 5.6 Permission prompts during guided setup are normal
- Symptom: 10-30 "Allow this bash command?" prompts the lesson video never showed; "connected to localhost" read as a broken setup
- Count: ~2
- Verified: yes — instructor sessions run different permission settings and Claude never takes the same route twice; Shift+Tab cycles modes; Auto mode is Max/Team/Enterprise/API only; localhost:3000 is the MCP process on your own machine and is correct
- Examples:
  - https://www.skool.com/ai-automation-society-plus/allow-this-bash-command
  - https://www.skool.com/ai-automation-society-plus/claude-code-balance-between-security-and-autonomy

### 5.7 Client credential handling playbook
- Symptom: freelancers holding raw client passwords/API keys, unsure what access to request or how clients should hand keys over
- Count: ~5
- Verified: yes — OAuth2 for email (never passwords/app passwords), client-owned API accounts with project-scoped keys, secrets in the platform's own secret store, build against dummy creds then do a live handoff call
- Examples:
  - https://www.skool.com/ai-automation-society-plus/clients-api-keys
  - https://www.skool.com/ai-automation-society-plus/clients-api-keys-and-password-management
  - https://www.skool.com/ai-automation-society-plus/need-helpadvice-on-client-creds-for-n8n
  - https://www.skool.com/ai-automation-society-plus/credential-mgmt-in-claude-routines

---

## 6. CLAUDE CODE — Model Switching / Costs

### 6.1 When to switch Sonnet → Opus
- Count: ~3
- Verified: yes — switch on logic loops, quality drops, band-aid architecture
- Example: https://www.skool.com/ai-automation-society-plus/switching-between-models-claude-code

### 6.2 Claude Code Plan vs API costs
- Count: ~5 <!-- count refreshed 2026-07 -->
- Verified: yes — a personal subscription covers that person's own Claude Code usage and includes zero API usage; production workflows belong on API keys from console.anthropic.com. Link Anthropic's current Usage Policy / Consumer Terms rather than asserting what the terms say
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-20-plan-vs-claude-api-costing
  - https://www.skool.com/ai-automation-society-plus/helpdesk-ai-agent

### 6.3 OpenRouter / Qwen / DeepSeek free model setup
- Count: ~3
- Verified: yes — `ANTHROPIC_AUTH_TOKEN`, OpenRouter privacy toggle for free endpoints, base URL `/api` not `/api/v1`
- Example: https://www.skool.com/ai-automation-society-plus/using-claude-code-in-vs-code-via-openrouter-and-qwen-free

### 6.4 Claude Design now shares the same pool as Code
- Count: ~2
- Verified: yes — confirmed by community (Design uses same token bucket since merger)
- Example: https://www.skool.com/ai-automation-society-plus/is-claude-design-using-my-normal-session-tokens

### 6.5 Claude outage / API errors all over
- Count: ~3
- Verified: yes — check status.claude.com / status.anthropic.com
- Example: https://www.skool.com/ai-automation-society-plus/what-do-you-do-when-claude-code-gives-you-error-message

### 6.6 "Failed to start workspace" error in Claude Code/Cowork
- Count: ~3
- Verified: yes — usually an Anthropic outage, or Hyper-V/WSL conflict on Windows
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-failing-to-start-workspace
  - https://www.skool.com/ai-automation-society-plus/solved-claude-failing-to-start-workspace

### 6.7 Consumer-terms re-acceptance loop
- Symptom: "API Error: 400 We've updated our Consumer Terms..."
- Count: ~2
- Verified: yes — log into claude.ai with the email shown in `/status`; if SSO doesn't prompt, try console.anthropic.com
- Example: https://www.skool.com/ai-automation-society-plus/claude-error

---

## 7. CLAUDE CODE — Workflow Patterns

### 7.1 Claude breaking previously-fixed features ("the regression spiral")
- Count: ~6
- Verified: yes — write tests, commit to git at every working point, scope changes ("only touch this"), use PostToolUse/Stop hooks
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-best-practices
  - https://www.skool.com/ai-automation-society-plus/is-it-me-or-claude
  - https://www.skool.com/ai-automation-society-plus/claude-code-duplicating-nodes

### 7.2 Cross-machine sync — pick up where I left off
- Count: ~5
- Verified: yes — GitHub, NOT OneDrive/Google Drive (they corrupt git repos); use handoff.md
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-session-from-pc-and-picking-up-where-i-left-off-on-my-laptop
  - https://www.skool.com/ai-automation-society-plus/copy-claude-code-chat-in-vs-code-to-another-computer
  - https://www.skool.com/ai-automation-society-plus/how-to-sync-claude-work-across-2-laptops-no-admin-on-one
  - https://www.skool.com/ai-automation-society-plus/transfer-claude-desktop-from-a-windows-to-mac

### 7.3 Folder structure / multi-project organization (WAT framework)
- Count: ~6
- Verified: yes — one folder per use case, each with its own CLAUDE.md
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-architecture-folder-structure
  - https://www.skool.com/ai-automation-society-plus/claude-code-setup-new-folder-for-every-project
  - https://www.skool.com/ai-automation-society-plus/wat-question

### 7.4 Claude Code vs Claude Chat vs Cowork workflow
- Count: ~5
- Verified: yes — Chat for planning, Code for building; Cowork for file/task mgmt
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-cowork-claude-code-openclaw
  - https://www.skool.com/ai-automation-society-plus/whats-your-best-practice-for-planning-with-claude-code-vs-claude-mobile-app

### 7.5 Plan mode getting stuck spinning for hours
- Count: ~1
- Verified: partial — too-broad prompts cause this; break it down
- Example: https://www.skool.com/ai-automation-society-plus/claude-code-spinning

### 7.6 GitHub workflow — branches, multi-machine, push/pull
- Count: ~5
- Verified: yes — standard git workflow; commit + push from CC; many users new to git
- Examples:
  - https://www.skool.com/ai-automation-society-plus/warning-pushing-to-github-as-a-newbie
  - https://www.skool.com/ai-automation-society-plus/setting-up-collaborative-workflow-in-claude-code-github-need-advice
  - https://www.skool.com/ai-automation-society-plus/safe-use-of-gibhub

### 7.7 MCP vs CLI — which to reach for
- Symptom: both a CLI and an MCP exist for the same service and students don't know which to use
- Count: ~1
- Verified: yes — MCP servers inject full tool schemas at session start (GitHub MCP alone exposes 90+ tools); default to CLI for well-known tools, reach for MCP for org standardisation, enterprise auth, or custom internal tools
- Example: https://www.skool.com/ai-automation-society-plus/difference-between-cli-and-mcp

### 7.8 Claude Code vs Cowork vs Chat — which tool for which job
- Symptom: students shuttle code between Chat and Cowork by hand, or ask which is "more powerful" as an executive assistant
- Count: ~3
- Verified: yes — Code for software/automations in a project folder, Cowork for desktop/app/browser work, Chat for ideation only
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-vs-cowork-executive-assistant
  - https://www.skool.com/ai-automation-society-plus/chat-vs-cowork-vs-code

### 7.9 One folder per project + the hub-and-spoke executive assistant
- Symptom: students cram every project and client credential into one EA folder like the simple version in the EA video
- Count: ~6
- Verified: yes — one folder per project; the EA's CLAUDE.md is a MAP not a copy; credentials stay in each project's own .env; scale EA autonomy gradually. Gotcha: a global skill in `~/.claude/skills/` is silently ignored in any project that has its own `.claude/skills/` — run `/doctor` to see what loaded
- Examples:
  - https://www.skool.com/ai-automation-society-plus/executive-assistant-herk-2-related-need-help
  - https://www.skool.com/ai-automation-society-plus/how-do-you-structure-projects
  - https://www.skool.com/ai-automation-society-plus/when-to-create-different-projects-vs-using-same-project

### 7.10 Documenting builds and SOPs
- Symptom: builders can't remember what their builds do; hand-written docs drift stale
- Count: ~2
- Verified: yes — n8n: sticky notes + per-node notes on the canvas, export JSONs for real history (self-hosted prunes execution data on a default schedule — `EXECUTIONS_DATA_MAX_AGE`, docs.n8n.io/deploy/host-n8n/configure-n8n/scaling/manage-execution-data — and workflow-version history is plan-gated); Claude Code: `/init` drafts CLAUDE.md and a non-technical README; treat SOPs as generated from the workflow JSON, not authored
- Examples:
  - https://www.skool.com/ai-automation-society-plus/help-a-noobie
  - https://www.skool.com/ai-automation-society-plus/workflow-sop

### 7.11 Claude Code in dev containers
- Symptom: in a VS Code dev container the extension reinstalls every start, skills show but don't fire, login resets
- Count: ~1
- Verified: yes — state lives in TWO places; mount persistent volumes for BOTH `~/.claude` and `~/.claude.json` (plus `~/.vscode-server`); a stale plugin cache delays skill loading ~30s
- Example: https://www.skool.com/ai-automation-society-plus/how-are-you-guys-using-dev-environments

### 7.12 Connecting separately built agents
- Symptom: several agents built as separate Claude Code projects "don't talk to each other"
- Count: ~1
- Verified: yes — subagents are session-scoped and cannot connect deployed projects; deploy each agent as its own service behind a central orchestrator
- Example: https://www.skool.com/ai-automation-society-plus/help-with-mission-control

### 7.13 "I'm Claude Code's assistant" — give it eyes
- Symptom: student relays n8n execution logs and screenshots by hand while Claude debugs
- Count: ~1
- Verified: yes — connect the n8n MCP/API (or SSH access) and instruct Claude to inspect executions itself; cheaper alternative is pasting the failed node's execution JSON
- Example: https://www.skool.com/ai-automation-society-plus/im-claude-codes-assistant

---

## 8. N8N + MCP — Setup & Connection

### 8.1 czlonkowski/n8n-mcp clone vs npx (single biggest n8n confusion)
- Symptom: "I cloned the repo and got 100x more files", "claude wants to npm install, do I let it?", "multiple .env files"
- Count: ~15 (the #1 setup-failure pattern in the whole corpus) <!-- count refreshed 2026-07 -->
- Verified: yes — use the npx route (`claude mcp add n8n-mcp -e MCP_MODE=stdio -- npx n8n-mcp`) instead of cloning the repo. If you DO clone, you MUST run `npm install`.
- Examples:
  - https://www.skool.com/ai-automation-society-plus/help-with-section-14-n8n-mcp-server
  - https://www.skool.com/ai-automation-society-plus/mpc-server-set-up
  - https://www.skool.com/ai-automation-society-plus/trouble-with-setting-up-n8n-mcp-server-in-vs-claude-code
  - https://www.skool.com/ai-automation-society-plus/n8n-mcp-connection
  - https://www.skool.com/ai-automation-society-plus/connect-n8n-mcp-to-claude-code-issue

### 8.2 Two different "n8n MCP" tools confusion (official vs czlonkowski)
- Count: ~6 <!-- count refreshed 2026-07 -->
- Verified: yes — n8n native instance-level MCP exposes existing workflows TO Claude; czlonkowski's gives CC node knowledge to BUILD workflows. Course uses czlonkowski's.
- Example: https://www.skool.com/ai-automation-society-plus/n8n-mcp-connection

### 8.3 N8N_API_URL formatting — `/api/v1` confusion
- Count: ~3
- Verified: yes — base URL only, no trailing slash, no /api/v1 (the MCP appends it itself). Hostinger users: use the same URL as you browse to.
- Examples:
  - https://www.skool.com/ai-automation-society-plus/connect-n8n-mcp-to-claude-code-issue
  - https://www.skool.com/ai-automation-society-plus/using-n8n-via-hostinger-connection-issue

### 8.4 MCP set up at wrong scope (user vs project)
- Symptom: "I see it set up but /mcp doesn't show it"
- Count: ~3
- Verified: yes — `claude mcp add --scope project ...` writes `.mcp.json` to project root; without that flag it's user-scoped; restart Claude Code and approve the "Allow project MCP servers?" prompt
- Example: https://www.skool.com/ai-automation-society-plus/connect-n8n-mcp-to-claude-code-issue

### 8.5 Hostinger / self-hosted n8n — Claude Desktop config + reverse proxy
- Symptom: "claude_desktop_config.json says n8n-mcp is invalid"
- Count: ~3
- Verified: yes — Claude Desktop doesn't support remote HTTP MCPs directly; use `mcp-remote` npm bridge; nginx needs `proxy_buffering off` and `gzip off`
- Examples:
  - https://www.skool.com/ai-automation-society-plus/anyone-get-hostinger-self-hosted-n8n-working-with-desktop-claude-via-mcp
  - https://www.skool.com/ai-automation-society-plus/need-help-connecting-n8n-mcp-server-on-hostinger

### 8.6 n8n free trial — no "n8n API" menu visible
- Count: ~2
- Verified: yes — need paid plan or self-host. Newer UI: Settings → Instance-level MCP → Connection details
- Example: https://www.skool.com/ai-automation-society-plus/14-n8n-mcp-server-cant-find-api-n8n-key

### 8.7 Corporate firewall / enterprise proxy blocking MCP install
- Count: ~1
- Verified: partial — mirror repo to internal Bitbucket, point .npmrc at internal Artifactory; documentation-only MCP mode works offline
- Example: https://www.skool.com/ai-automation-society-plus/n8n-mcp-server-not-able-to-add-due-to-enterprise-restriction

### 8.8 "MCP runs locally?" — confusion about what runs where
- Count: ~3
- Verified: yes — MCP server is a local helper; the n8n instance is what runs the workflows (and what should be hosted)
- Examples:
  - https://www.skool.com/ai-automation-society-plus/n8n-mcp-server-building-portafolio
  - https://www.skool.com/ai-automation-society-plus/mcp-server-2

### 8.9 Permissions prompts during MCP install (Windows PowerShell)
- Count: ~2
- Verified: yes — normal; use Shift+Tab to cycle permission modes
- Example: https://www.skool.com/ai-automation-society-plus/need-help-connecting-n8n-mcp-server-on-hostinger

### 8.10 "Multiple .env files" generated by clone
- Count: ~3
- Verified: yes — they are templates (.env.example, .env.docker); only the plain `.env` matters
- Example: https://www.skool.com/ai-automation-society-plus/mpc-server-set-up

---

## 9. N8N — Building Workflows / Quality Issues

### 9.1 Claude hallucinating node names / parameters in n8n JSON
- Count: ~6 <!-- count refreshed 2026-07 -->
- Verified: yes — install czlonkowski's n8n-skills + n8n-mcp (validation built in)
- Example: https://www.skool.com/ai-automation-society-plus/setting-n8n-mcp-server-up-with-claude-code

### 9.2 n8n Code node 60-second timeout
- Count: ~2
- Verified: yes — split external HTTP calls into native HTTP Request nodes, don't put them in Code nodes
- Example: https://www.skool.com/ai-automation-society-plus/challenges-with-scraping-websites

### 9.3 ServiceM8 / verified-node trigger silent failure on trial accounts
- Count: ~1
- Verified: yes — triggers go silent on demo/trial; build against live paid account
- Example: https://www.skool.com/ai-automation-society-plus/n8n-use-case

### 9.4 Gmail → Google Sheets dedup loop / append-every-run
- Count: ~1
- Verified: partial
- Example: https://www.skool.com/ai-automation-society-plus/n8n-gmail-google-sheets-dedup-not-working-same-emails-appended-on-every-run

### 9.5 AI node returning 429 (rate limit)
- Count: ~4 <!-- count refreshed 2026-07 -->
- Verified: partial — switch model, add backoff, check API quota
- Example: https://www.skool.com/ai-automation-society-plus/help-customer-support-email-auto-draft-workflow-ai-node-returning-429-every-execution-no-drafts-created-hours-of-debugging-still-stuck

### 9.6 Telegram bot burning n8n Cloud execution quota
- Count: ~1
- Verified: partial — move to self-host or batch
- Example: https://www.skool.com/ai-automation-society-plus/telegram-bot-burning-n8n-cloud-limits

### 9.7 Google Sheets parsing data wrong
- Count: ~1
- Verified: partial
- Example: https://www.skool.com/ai-automation-society-plus/google-sheet-data-isnt-parsing

### 9.8 Skip n8n entirely — when n8n vs Claude Code
- Count: ~18 (recurring philosophical thread, not a bug) <!-- count refreshed 2026-07 -->
- Verified: yes — n8n for predictable flows + visual demos to clients; Claude Code agentic for messier judgment work
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-vs-n8n-trying-to-understand-the-real-difference
  - https://www.skool.com/ai-automation-society-plus/is-n8n-still-recommended-or-is-it-graduated
  - https://www.skool.com/ai-automation-society-plus/n8n-vs-claude-code-which-is-more-reliable-to-sell

### 9.9 AI Agent never calls its tool
- Symptom: the agent replies conversationally but never calls the attached tool; "None of your tools were used in this run"
- Count: ~2
- Verified: yes — set a custom Tool Description (not "Set Automatically"); keep `{{ }}` expressions out of the prompt's Tools section, since they pre-resolve and the model thinks it already has the data
- Examples:
  - https://www.skool.com/ai-automation-society-plus/ai-agent-not-using-the-tools
  - https://www.skool.com/ai-automation-society-plus/supabase-vector-store-doesnt-seem-to-work

### 9.10 Zero items = dead branch
- Symptom: a workflow silently stops after a lookup/delete that matched nothing; a form without an optional upload never reaches the downstream agent
- Count: ~2
- Verified: yes — enable "Always Output Data" on lookup/delete nodes (never on IF — it loops); route explicitly with IF; a multi-input node waits for ALL branches, so route the empty branch through a No Operation node
- Examples:
  - https://www.skool.com/ai-automation-society-plus/submission-form-is-not-passing-through-information-if-binary-file-is-not-attached
  - https://www.skool.com/ai-automation-society-plus/help-with-dynamic-rag

### 9.11 Expression and data-reference failures ($json, dates, JSON bodies, whitespace)
- Symptom: the wrong item moves, only one of many items processes, date filters error, "JSON parameter needs to be valid JSON", a node fails while looking correct
- Count: ~5
- Verified: yes — `{{ $json.x }}` reads the immediately-previous node (pin with `{{ $('Node Name').item.json.x }}`); a single-item node collapses the loop (insert Merge); drop `.format()` before date comparisons and enable "Convert types where required"; use `JSON.stringify()` without adding your own quotes; check for stray whitespace around `/` and `{{ }}`
- Examples:
  - https://www.skool.com/ai-automation-society-plus/drive-node-move-file-issue
  - https://www.skool.com/ai-automation-society-plus/google-drive-issue-move-files
  - https://www.skool.com/ai-automation-society-plus/fast-track-challenge-agent-2
  - https://www.skool.com/ai-automation-society-plus/error-message

### 9.12 Loop Over Items wired backwards
- Symptom: phantom rows written to the CRM sheet, IDs that don't line up, an execution that never finishes
- Count: ~1
- Verified: yes — per-item work was connected to the "done" branch (fires once, after everything) instead of "loop" (fires per item)
- Example: https://www.skool.com/ai-automation-society-plus/issue-faced-whilst-building-sales-bot-in-live-sales-bot-tutorial

### 9.13 Triggers firing wrong: twice, never, or only in production
- Symptom: a daily schedule fires exactly twice; a Gmail trigger returns nothing; an error workflow never fires during manual tests; a production webhook returns 200 with an empty body
- Count: ~5
- Verified: yes — duplicate/disconnected triggers and a second container on the same DB both double-fire; unread-only Gmail triggers starve themselves when the flow marks mail read, and the workflow may simply never have been published; error workflows only fire on production executions; toggle Active off/on to clear a stale webhook registration
- Examples:
  - https://www.skool.com/ai-automation-society-plus/help-with-dual-executions-in-n8n
  - https://www.skool.com/ai-automation-society-plus/google-sheet-data-isnt-parsing
  - https://www.skool.com/ai-automation-society-plus/n8n-self-healing-need-help
  - https://www.skool.com/ai-automation-society-plus/interactive-live-avatar-support-needed

### 9.14 Credentials and imported templates
- Symptom: nodes in a Claude-built workflow are locked and uneditable; imported course templates only run manually and their generation nodes error
- Count: ~3
- Verified: yes — create credentials yourself in the n8n UI first and have Claude reference them by name; for imported templates: activate, re-credential every generation node, unpin leftover test data
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-duplicating-nodes
  - https://www.skool.com/ai-automation-society-plus/n8n-marketing-automation

### 9.15 HTTP Request header auth
- Symptom: "Header name must be a non-empty string"; a valid fal.ai key rejected; template calls to image endpoints failing on auth
- Count: ~3
- Verified: yes — the credential's Name field must be exactly `Authorization`; schemes differ by provider (`Bearer <key>` for OpenAI/Kie.ai, `Key <key>` for fal.ai); prefer the predefined credential type for well-known APIs
- Examples:
  - https://www.skool.com/ai-automation-society-plus/the-ai-marketing-team-42625-template-but-im-stuck-on-an-error-i-cant-resolve
  - https://www.skool.com/ai-automation-society-plus/kie-connection-instruction

### 9.16 Retry / error handling set on the wrong node
- Symptom: an LLM step times out, retries never happen, the wired error branch never executes
- Count: ~1
- Verified: yes — retry was set on the circular Chat Model sub-node (config only); it belongs on the rectangular executing root node, with On Error set to "Continue (Using Error Output)"
- Example: https://www.skool.com/ai-automation-society-plus/open-ai-node-request-time-out

### 9.17 MCP-created workflows: nodes snap back / edits don't stick
- Symptom: dragging a node on an MCP-built workflow snaps it back; manual edits revert
- Count: ~1
- Verified: partial — n8n issue #27638 (nodes saved under `settings.nodes` instead of the root `nodes` array), closed by PR #32341 after affecting 2.13.3 stable / 2.14.2 beta; check their version and upgrade first, and only then rebuild manually or move the node definitions in the exported JSON and reimport
- Example: https://www.skool.com/ai-automation-society-plus/dragging-an-n8n-node-and-then-jumps-back-to-its-original-position

### 9.18 Data pinning greyed out
- Symptom: pin unavailable on Text Classifier / IF / Switch even though videos appear to pin nodes
- Count: ~1
- Verified: yes — pinning requires a single main output; pin the trigger instead and re-run
- Example: https://www.skool.com/ai-automation-society-plus/cant-pin-text-classifier-node

### 9.19 AI-pipeline cost and latency optimization
- Symptom: high-volume workflow burns API credits and hits timeouts
- Count: ~2
- Verified: partial — dedup BEFORE classification, batch lookups outside the loop with Compare Datasets, fix erroring Structured Output Parsers (every failed parse is a paid retry), and don't use an AI Agent node for tool-less classification
- Examples:
  - https://www.skool.com/ai-automation-society-plus/optimise-n8n-system
  - https://www.skool.com/ai-automation-society-plus/quick-question-ef67236e

---

## 10. HOSTING / DEPLOYMENT

### 10.1 Hostinger VPS — HTTPS not secure on first n8n setup
- Count: ~2
- Verified: yes — DNS A record not pointed yet; SSL issues automatically once DNS resolves
- Example: https://www.skool.com/ai-automation-society-plus/new-hostinger-vps-shows-https-not-secure

### 10.2 Hostinger security baseline checklist
- Count: ~2
- Verified: yes — SSH key only, fail2ban, firewall, backup N8N_ENCRYPTION_KEY off-server, reverse proxy w/ Let's Encrypt
- Example: https://www.skool.com/ai-automation-society-plus/hostinger-safety

### 10.3 Render free tier sleeping on WhatsApp webhook
- Count: ~2
- Verified: yes — Render free spins down after 15 min; upgrade off the free tier to the paid Starter tier, which stays up 24/7 (current price at render.com/pricing)
- Example: https://www.skool.com/ai-automation-society-plus/whatsapp-bot-by-claude-code-and-currently-living-on-render

### 10.4 Self-host vs Cloud decision
- Count: ~5
- Verified: yes — for beginners, Cloud; for >10K ops/mo or compliance, self-host
- Examples:
  - https://www.skool.com/ai-automation-society-plus/self-host-vs-not
  - https://www.skool.com/ai-automation-society-plus/how-to-host-n8n-properly
  - https://www.skool.com/ai-automation-society-plus/hosting-options

### 10.5 Trigger.dev as scheduler / hosting for Claude Code agents
- Count: ~5
- Verified: yes — built-in observability, use for code-first agents, n8n for visual-first
- Lesson reference: `Claude Code → Phase 3 → 1.4 Trigger Dev` (teaches what Trigger.dev is, account creation, installing the Trigger.dev MCP server, first deploy)
- Examples:
  - https://www.skool.com/ai-automation-society-plus/about-github-and-triggerdev
  - https://www.skool.com/ai-automation-society-plus/triggerdev
  - https://www.skool.com/ai-automation-society-plus/hosting-on-triggerdev-agent-sdk

### 10.6 Vercel custom domain on a Claude Code-built site
- Count: ~2
- Verified: yes — buy domain at registrar, add to Vercel, paste DNS records back
- Example: https://www.skool.com/ai-automation-society-plus/building-frontend

### 10.7 Observability for vibe-coded background workflows
- Count: ~3
- Verified: yes — structured logging table in Postgres/SQLite + push-on-failure alerts; OpenTelemetry for enterprise (`CLAUDE_CODE_ENABLE_TELEMETRY=1`)
- Examples:
  - https://www.skool.com/ai-automation-society-plus/vibe-coding-with-claude-code-how-do-i-get-observability-into-my-background-workflows
  - https://www.skool.com/ai-automation-society-plus/claude-code-programm-oversight

### 10.8 n8n Cloud vs self-hosting for beginners
- Symptom: brand-new students stall on server setup assuming self-hosting is the "proper" path
- Count: ~2
- Verified: yes — start on Cloud, move to self-hosting later for cost or execution-volume reasons; local free is fine for practice but your machine must be running
- Examples:
  - https://www.skool.com/ai-automation-society-plus/n8n-510128ea
  - https://www.skool.com/ai-automation-society-plus/how-to-host-n8n-properly

### 10.9 Choosing a VPS for always-on agents
- Symptom: "cheapest reliable box for n8n / an always-on agent?"; weighing a spare Mac Mini
- Count: ~3
- Verified: yes — Oracle free tier only for throwaway tests (idle reclamation, account wipes); Hetzner/Hostinger as solid picks; verify ARM image availability before buying ARM; promo pricing roughly doubles at renewal; 2GB minimum, 4GB comfortable
- Examples:
  - https://www.skool.com/ai-automation-society-plus/vps-recommendation
  - https://www.skool.com/ai-automation-society-plus/hosting-options

### 10.10 Local/ngrok → VPS migration
- Symptom: after migrating, external services can't reach webhooks; after a VPS failure, restored credentials are locked forever
- Count: ~2
- Verified: yes — set `WEBHOOK_URL` to the public https domain BEFORE activating anything (otherwise n8n advertises localhost:5678); set `N8N_ENCRYPTION_KEY` explicitly on first spin-up and save it off-server; pin the Docker image tag, never `:latest`
- Examples:
  - https://www.skool.com/ai-automation-society-plus/how-to-choose-a-hostinger-plan
  - https://www.skool.com/ai-automation-society-plus/n8n-or-claude-routines-or-claude-managed-agents

### 10.11 Hostinger n8n — custom domain 404s and "No workspace here"
- Symptom: after pointing a custom domain at the VPS the URL 404s; separately, a self-hosted user lands on app.n8n.cloud and fears the workflows are gone
- Count: ~2
- Verified: yes — the Docker router still serves the old domain (update the domain variables in `/root/n8n/.env`, then `docker compose down && up -d`); and app.n8n.cloud is a different product entirely from your own instance
- Examples:
  - https://www.skool.com/ai-automation-society-plus/self-hosting-on-hostinger
  - https://www.skool.com/ai-automation-society-plus/my-self-hosted-by-hostinger-dashboard-disappeared

### 10.12 n8n Cloud ceilings and degradation
- Symptom: memory errors approaching execution caps; 502/524 on publish; Schedule Triggers silently not firing with nothing in the log
- Count: ~3
- Verified: yes — Split In Batches and sub-workflows for headroom; stop testing via manual executions; degradation is usually execution-history bloat, and the confirmed fix is copying the workflow into a brand-new one and deleting the bloated original
- Examples:
  - https://www.skool.com/ai-automation-society-plus/advice-on-n8n-scalingalternatives
  - https://www.skool.com/ai-automation-society-plus/struggling-with-n8n-cloud-triggers-not-firing-502524-errors-anyone-else
  - https://www.skool.com/ai-automation-society-plus/telegram-bot-burning-n8n-cloud-limits

### 10.13 Self-hosted n8n at scale — Postgres lock contention
- Symptom: queue-mode instance with many workers overloads Postgres; top lock waits on `workflow_statistics`
- Count: ~1
- Verified: yes — `SKIP_STATISTICS_EVENTS=true` on ALL instances (`N8N_DISABLED_MODULES=insights` is a different code path and does nothing here); tune execution-data saving and raise the pool size
- Example: https://www.skool.com/ai-automation-society-plus/workflowstatistics-locking-db

### 10.14 Trigger.dev — dev mode vs production deploy
- Symptom: tasks only run while `npm run trigger:dev` is open; otherwise they queue and expire; npm throws ENOENT
- Count: ~3
- Verified: yes — dev mode runs a local worker that dies with the terminal; deploy with `npm run trigger:deploy`; ENOENT means npm ran outside the project folder
- Examples:
  - https://www.skool.com/ai-automation-society-plus/triggerdev
  - https://www.skool.com/ai-automation-society-plus/question-about-triggerdev
  - https://www.skool.com/ai-automation-society-plus/mcp-dilemma-needed-for-triggerdev

---

## 11. VOICE AGENTS (Vapi / ElevenLabs / Retell)

### 11.1 Vapi voice agent workflow misfires
- Count: ~7 <!-- count refreshed 2026-07 -->
- Verified: partial
- Example: https://www.skool.com/ai-automation-society-plus/support-needed-vapi-voice-agent-workflow-issue

### 11.2 Twilio Mexican number + WhatsApp Business verification
- Count: ~2
- Verified: no — open question
- Example: https://www.skool.com/ai-automation-society-plus/twilio-mexican-number-whatsapp-business-verification-for-ai-receptionist

### 11.3 Telnyx vs Twilio carrier support
- Count: ~2
- Verified: partial
- Example: https://www.skool.com/ai-automation-society-plus/twilio-vs-telnyx
- **Fix**: `knowledge/fix-patterns-integrations.md` § VOICE AGENT TELEPHONY (covers "my country isn't supported")

### 11.4 VAPI voice quality / which voices to use
- Count: ~1
- Example: https://www.skool.com/ai-automation-society-plus/vapi-voices-what-are-you-actually-using

### 11.5 Unable to proceed with Vapi call node in n8n
- Count: ~2
- Verified: yes (one case)
- Example: https://www.skool.com/ai-automation-society-plus/support-request-unable-to-proceed-with-vapi-call-node

### 11.6 AI Voice receptionist cost/stack architecture
- Count: ~8 <!-- count refreshed 2026-07 -->
- Verified: yes (recurring architecture pattern post)
- Example: https://www.skool.com/ai-automation-society-plus/ai-voice-receptionist-setup-stack-cost

---

## 12. RAG / VECTOR / KNOWLEDGE BASE

### 12.1 Karpathy LLM Wiki approach vs RAG
- Count: ~2
- Verified: yes — for <100-200 docs, plain markdown in Git beats vector DB; show direct quote in answer to kill hallucination complaints
- Example: https://www.skool.com/ai-automation-society-plus/best-wiki-ai-agent-rag-karpathys-llm-wiki-obsidian

### 12.2 Chunking strategy for legal/large PDFs
- Count: ~3 <!-- count refreshed 2026-07 -->
- Example: https://www.skool.com/ai-automation-society-plus/chunking-vector-storage-choices-for-a-150-page-law-firm-rag

### 12.3 WhatsApp sales agent losing context (RAG + Supabase + Airtable)
- Count: ~1
- Verified: partial
- Example: https://www.skool.com/ai-automation-society-plus/eed-advice-on-a-complex-whatsapp-sales-agent-rag-supabase-airtable-losing-context-and-missing-tasks

### 12.4 Supabase setup help
- Count: ~3 <!-- count refreshed 2026-07 -->
- Example: https://www.skool.com/ai-automation-society-plus/need-help-with-course-materials

### 12.5 RAG agent skips the vector store entirely
- Symptom: answers look plausible but never come from your documents; the execution log shows the vector-store tool was never called
- Count: ~2
- Verified: yes — add an explicit "always check the knowledge base" instruction to the agent's system prompt, and confirm the RETRIEVAL-side vector store node has an embedding model connected (matching ingestion)
- Examples:
  - https://www.skool.com/ai-automation-society-plus/supabase-vector-store-doesnt-seem-to-work
  - https://www.skool.com/ai-automation-society-plus/prompt-engineering

### 12.6 RAG chatbot slow and token-hungry
- Symptom: ~30s and ~8K tokens per answer; more knowledge makes it slower
- Count: ~1
- Verified: yes — lower the vector-store `limit` (try 2), re-chunk so Q&A pairs stay together (1500/200 worked), and replace the retrieval sub-workflow's AI Agent with a Question & Answer Chain on a small model
- Example: https://www.skool.com/ai-automation-society-plus/rag-more-info-better-answers-less-money-and-takes-double-time

### 12.7 Agent hallucinates after a bare "yes"
- Symptom: multi-agent chatbot works until a short confirmation reply, then hallucinates or skips the knowledge base
- Count: ~1
- Verified: yes — the orchestrator passes the literal "yes" as the retrieval query; either deliver data with the answer instead of asking a follow-up, or force full-context query synthesis in the prompt
- Example: https://www.skool.com/ai-automation-society-plus/prompt-engineering

### 12.8 Dynamic RAG refresh — Supabase metadata deletes
- Symptom: delete-then-re-embed stops working; Delete Rows outputs zero items and file_id filters match nothing
- Count: ~1
- Verified: yes — the Supabase Vector Store writes the file ID into a `metadata` JSONB column the standard Delete node can't filter on; use an HTTP DELETE via PostgREST against `metadata->>file_id`
- Example: https://www.skool.com/ai-automation-society-plus/help-with-dynamic-rag

---

## 13. WEB SCRAPING

### 13.1 Cloudflare Enterprise blocking Firecrawl/Apify
- Count: ~4 <!-- count refreshed 2026-07 -->
- Verified: yes — Enterprise tier uses TLS fingerprinting (JA3/JA4); only Camoufox / nodriver / curl_cffi or managed unlockers (Bright Data, Scrapfly) get through
- Example: https://www.skool.com/ai-automation-society-plus/challenges-with-scraping-websites

### 13.2 Workday/SuccessFactors ATS scraping
- Count: ~1
- Verified: yes — use the public CXS endpoint `myworkdayjobs.com/wday/cxs/...`; SuccessFactors has hidden RSS feed at `/sitemal.xml`
- Example: https://www.skool.com/ai-automation-society-plus/challenges-with-scraping-websites

### 13.3 Reddit API access denied / policy loop
- Count: ~1
- Example: https://www.skool.com/ai-automation-society-plus/anyone-get-reddit-api-access-in-2026-stuck-in-a-policy-loop

### 13.4 SPA / JavaScript-rendered page scraping
- Count: ~4 <!-- count refreshed 2026-07 -->
- Verified: yes — use DevTools → Fetch/XHR → sort by size → Copy as cURL → Claude turns into request
- Example: https://www.skool.com/ai-automation-society-plus/challenges-with-scraping-websites

---

## 14. INTEGRATION ISSUES (Third-party APIs)

### 14.1 Meta WhatsApp Cloud API / business account verification
- Count: ~8 <!-- count refreshed 2026-07 -->
- Verified: partial — many independent reports of restrictions/lockouts
- Examples:
  - https://www.skool.com/ai-automation-society-plus/whatsapp-bot-by-claude-code-and-currently-living-on-render
  - https://www.skool.com/ai-automation-society-plus/meta-business-account-restricted
  - https://www.skool.com/ai-automation-society-plus/issue-with-whatsapp-number-in-europe

### 14.2 Meta Business / Instagram automation account banned
- Count: ~1
- Verified: yes — Meta bans unofficial DM automation; switch to official partner (ManyChat or Graph API)
- Example: https://www.skool.com/ai-automation-society-plus/meta-business-account-restricted

### 14.3 Gmail node problems in n8n
- Count: ~2
- Example: https://www.skool.com/ai-automation-society-plus/gmail-node-problem

### 14.4 Google Workspace API connection
- Count: ~4 <!-- count refreshed 2026-07 -->
- Verified: yes — google-workspace MCP / CLI exists
- Example: https://www.skool.com/ai-automation-society-plus/google-workspaces-apis

### 14.5 Smartlead MCP loading 60+ tools (token bloat)
- Count: ~1
- Verified: yes — toggle Smartlead connector off per chat; or use Smartlead "On demand access" mode
- Example: https://www.skool.com/ai-automation-society-plus/quick-question-smartlead-claude-mcp

### 14.6 ManyChat / Instagram automation
- Count: ~2

### 14.7 Fireflies trigger help
- Count: ~1
- Example: https://www.skool.com/ai-automation-society-plus/fireflies-trigger-help

### 14.8 Concurrent WhatsApp messages handling
- Count: ~1
- Verified: yes — queue/dedup
- Example: https://www.skool.com/ai-automation-society-plus/handling-concurrent-incoming-messages-in-a-whatsapp-automation

### 14.9 AWS SNS to n8n webhook
- Count: ~1
- Example: https://www.skool.com/ai-automation-society-plus/how-to-connect-aws-sns-to-n8n-webhook

### 14.10 GoHighLevel skill
- Count: ~1
- Example: https://www.skool.com/ai-automation-society-plus/gohighlevel-skill

### 14.11 Google OAuth consent screen left in "Testing" mode
- Symptom: Gmail/Drive/Sheets automations "just stop working" about weekly; a handed-off client build dies a week after go-live
- Count: ~3
- Verified: yes — Testing-status apps issue refresh tokens that expire after 7 days; set User type to Internal if every account is in one Workspace, otherwise Publish App ("In production", instant, separate from verification). Caveat: restricted Gmail scopes against external accounts may still need a Google security review
- Examples:
  - https://www.skool.com/ai-automation-society-plus/urgent-google-oauth-refresh-token-keeps-revoking-sheetsdrivegmail-need-a-permanent-fix
  - https://www.skool.com/ai-automation-society-plus/building-an-ai-email-agent-for-a-friend-google-cloud-functions-vs-n8n-what-would-you-do
  - https://www.skool.com/ai-automation-society-plus/need-helpadvice-on-client-creds-for-n8n

### 14.12 Per-user OAuth vs Domain-Wide Delegation
- Symptom: a student sets up a DWD service account on an AI's advice and asks whether it's safe
- Count: ~1
- Verified: yes — DWD lets the service account impersonate ANY domain user; default to per-user OAuth, reserve DWD for unattended bulk work, and include `access_type=offline` + `prompt=consent` or no refresh token is issued
- Example: https://www.skool.com/ai-automation-society-plus/google-workspaces-apis

### 14.13 Google OAuth client setup overwhelm (Phase 3)
- Symptom: stuck creating the OAuth client ID/secret; "I followed the steps but it doesn't work"
- Count: ~1
- Verified: yes — four distinct pieces (client ID/secret, redirect URIs, scopes, test users); the top failure is a redirect URI that doesn't byte-match the platform's callback, including http/https, trailing slash and port
- Example: https://www.skool.com/ai-automation-society-plus/trouble-with-phase-3

### 14.14 Course modules that call the Anthropic API need API credits
- Symptom: the Scheduled Research Agent module fails with low-credit errors on an active Pro/Max subscription
- Count: ~1
- Verified: yes — consumer subscriptions include zero API usage; top up ~$5 at console.anthropic.com, or substitute the Gemini free tier
- Example: https://www.skool.com/ai-automation-society-plus/issue-with-module-16-scheduled-research-agent-api-credits

### 14.15 GitHub plugin auth — "does not support dynamic client registration"
- Symptom: GitHub (and reportedly Slack) plugin/MCP install fails with an SDK auth error
- Count: ~1
- Verified: yes — GitHub's OAuth server doesn't support RFC 7591 DCR, which Claude Code's plugin auth flow expects; use the GitHub CLI (`gh auth login`) instead, which fully replaced the plugin in the source thread
- Example: https://www.skool.com/ai-automation-society-plus/github-plugin-error-for-claude-code

### 14.16 Office 365 files with Claude
- Symptom: "file incompatible" on a .docx upload; conversion scripts regenerated every session
- Count: ~1
- Verified: partial — Word/Excel/PowerPoint add-ins to edit interactively, the free M365 Connector to read across a tenant, the ms-365 MCP server to automate (documented in-thread but not confirmed working by the poster)
- Example: https://www.skool.com/ai-automation-society-plus/rw-office-365-files-with-claude

---

## 15. COURSE / CONTENT ACCESS

### 15.1 Locked / can't find a specific lesson or md file
- Count: ~11 (recurring — classroom got restructured) <!-- count refreshed 2026-07 -->
- Verified: partial — numbered Claude Code lessons live in the **Claude Code** course (Phase 1-4); some assets only in Archived. Old community Drive links pointing to "Build Your Portfolio" for Claude Code content are stale — verify in the Claude Code course first.
- Examples:
  - https://www.skool.com/ai-automation-society-plus/documents-missing-from-mastering-claude-code
  - https://www.skool.com/ai-automation-society-plus/cant-find-claude-md-for-wat-framework
  - https://www.skool.com/ai-automation-society-plus/where-are-the-md-files-discussed-in-every-level-of-claude-code-explained-in-21-minutes
  - https://www.skool.com/ai-automation-society-plus/finding-the-aios-repo
  - https://www.skool.com/ai-automation-society-plus/download-the-ai-voice-receptionist-with-vapi-and-n8n-mcp-template

### 15.2 Which course to do next
- Count: ~6
- Example: https://www.skool.com/ai-automation-society-plus/which-class-to-do-next

### 15.3 AIS vs AIS+
- Count: ~2
- Verified: yes — AIS is the free community, AIS+ is paid with the classroom
- Example: https://www.skool.com/ai-automation-society-plus/ais-vs-ais

### 15.4 Cancellation / refunds
- Count: ~5
- Verified: yes — contact Yash / Skool support
- Examples:
  - https://www.skool.com/ai-automation-society-plus/cancellation-help
  - https://www.skool.com/ai-automation-society-plus/cancel-membership
  - https://www.skool.com/ai-automation-society-plus/yesterday-my-subscription-got-automatically-renewed-but-i-would-like-to-cancel-it-support-is-not-reacting

### 15.5 Portfolio progress stuck at X%
- Count: ~2
- Example: https://www.skool.com/ai-automation-society-plus/portfolio-stuck-at-89

### 15.6 Codex (not Claude Code) — where to learn
- Count: ~4
- Verified: partial — Nate has a 1-hour Codex video; use the same patterns
- Example: https://www.skool.com/ai-automation-society-plus/i-need-some-advice-where-should-i-learn-about-codex

### 15.7 Course progress stuck below 100%
- Symptom: percentage won't move despite watching everything; cache clearing and reboots don't help
- Count: ~2
- Verified: yes — Skool never auto-completes; the checkmark must be clicked, including on section headers and every "End Of ..." trophy lesson. Restructures can leave whole sections unvisited
- Examples:
  - https://www.skool.com/ai-automation-society-plus/course-progress-not-updating-after-troubleshooting
  - https://www.skool.com/ai-automation-society-plus/portfolio-stuck-at-89

### 15.8 OPAA / Scale / Get Your First Clients — where they live and when they unlock
- Symptom: "I can't find One Person AI Agency / Subs to Sales anywhere", or "what's inside the locked folders?"
- Count: ~6
- Verified: yes — the AIS 2.0 restructure folded OPAA and Subs to Sales into the **Scale** course as sections; Get Your First Clients opens at 1 month, Scale at 90 days (existing membership time counts); annual/Premium get both immediately; OPAA is also sold standalone outside the membership
- Examples:
  - https://www.skool.com/ai-automation-society-plus/opaa
  - https://www.skool.com/ai-automation-society-plus/opaa-access
  - https://www.skool.com/ai-automation-society-plus/access-to-one-person-ai-agency-not-recieved
  - https://www.skool.com/ai-automation-society-plus/seeking-curriculum-details-for-get-your-first-clients-and-scale

### 15.9 Where the resources actually live after the AIS 2.0 restructure
- Symptom: can't find YouTube video resources, n8n templates, the WAT CLAUDE.md, agent skills, puzzle files, or slides
- Count: ~15 (the largest course-access cluster)
- Verified: yes — Community Resources > "Nate Herk - Video Database" for YouTube resources; "New Video: ..." community posts for a specific video's templates (check the free AIS community too); Community Resources > n8n Templates; Community Resources > Agent Skills; the WAT CLAUDE.md is attached to the "Master 95% of Claude Code in 36 Mins" community post. Broken/missing lesson resources get re-attached when reported in Support Needed
- Examples:
  - https://www.skool.com/ai-automation-society-plus/i-cannot-find-the-resources-from-the-yt-videos
  - https://www.skool.com/ai-automation-society-plus/where-do-i-find-agent-skills-classroom
  - https://www.skool.com/ai-automation-society-plus/n8n-templates-2
  - https://www.skool.com/ai-automation-society-plus/wat-md
  - https://www.skool.com/ai-automation-society-plus/issue-with-classroom-resources-link

### 15.10 Learning path — what to do first
- Symptom: new members overwhelmed, paralysed by "n8n is dead" clickbait, or unsure whether to finish the n8n masterclass before Claude Code
- Count: ~6
- Verified: yes — Agent Zero for fundamentals, then "10 Hours to 10 Seconds V2" (both in Build Your Portfolio); two valid orders after that (n8n-first for beginners, Claude-Code-first for people with a coding background); build real projects from day one
- Examples:
  - https://www.skool.com/ai-automation-society-plus/i-need-help-where-to-start
  - https://www.skool.com/ai-automation-society-plus/beginner-here-anxious-to-take-the-right-direction
  - https://www.skool.com/ai-automation-society-plus/course-workflow
  - https://www.skool.com/ai-automation-society-plus/im-lost-help

### 15.11 "Is n8n dead / graduated?"
- Symptom: recurring after the stack-tier video placed n8n in the "graduated" tier and the masterclass moved to Archived
- Count: ~3
- Verified: yes — "graduated" means no longer the primary content tool, not obsolete; Archived is housekeeping, not deprecation; n8n is the always-on runtime layer that Claude Code builds into
- Examples:
  - https://www.skool.com/ai-automation-society-plus/is-it-still-worth-it-to-learn-n8n
  - https://www.skool.com/ai-automation-society-plus/is-n8n-still-recommended-or-is-it-graduated

### 15.12 Following the course with Codex instead of Claude Code
- Symptom: on a ChatGPT/Codex plan; or created CLAUDE.md in Codex and nothing happened
- Count: ~3
- Verified: yes — Codex reads AGENTS.md, not CLAUDE.md (which it silently ignores); run `/init` to scaffold it; contents are identical markdown
- Examples:
  - https://www.skool.com/ai-automation-society-plus/will-codex-do-as-claude-code-in-the-course
  - https://www.skool.com/ai-automation-society-plus/i-need-some-advice-where-should-i-learn-about-codex

### 15.13 "My build doesn't match the video"
- Symptom: file tree looks nothing like the lesson screenshots; the AI plan produced one workflow where the demo shows two; MCP verification output differs
- Count: ~4
- Verified: yes — Claude Code is non-deterministic; verify the END STATE (tools respond, .env exists, workflows list, lesson tests pass), not the file tree. Inline knowledge instead of a vector store is a legitimate architecture for small static doc sets
- Examples:
  - https://www.skool.com/ai-automation-society-plus/16-verify-all-tools-are-connected
  - https://www.skool.com/ai-automation-society-plus/module-22-my-result-from-ai-plan-is-entirely-different-from-guide
  - https://www.skool.com/ai-automation-society-plus/claude-code-mcp-doesnt-look-like-the-video

### 15.14 "My screen doesn't match the videos" — wrong product installed
- Symptom: the interface looks nothing like the videos; or Claude Code immediately scaffolds a whole Node/Trigger.dev project the course never mentioned
- Count: ~2
- Verified: yes — three mixups: full Visual Studio (purple) installed instead of VS Code (blue); typing `claude` in the bottom terminal launches the CLI rather than the extension panel; the Trigger.dev CLAUDE.md grabbed instead of the WAT one (a CLAUDE.md carries instructions Claude executes immediately)
- Examples:
  - https://www.skool.com/ai-automation-society-plus/re-claude-code-skills
  - https://www.skool.com/ai-automation-society-plus/support-needed-2755b332

### 15.15 Classroom video playback problems
- Symptom: videos won't load at all, or the timeline doesn't load fully and playback skips gaps
- Count: ~2
- Verified: yes — classroom videos are Loom-hosted, so a Loom outage takes many down at once; timeline gaps are Chrome-specific and switching to Firefox/Edge is the confirmed fix. Incognito does not help
- Examples:
  - https://www.skool.com/ai-automation-society-plus/agent-zero-not-working
  - https://www.skool.com/ai-automation-society-plus/loom-video-loading-problem

### 15.16 Getting text transcripts out of classroom videos
- Symptom: wanting transcripts for NotebookLM; "Save Video As" and link copying are restricted and YouTube transcript tools don't apply
- Count: ~1
- Verified: partial — full-screen the video, pause, click the "Get Loom for free" popup, close the login window, and the transcript panel with a Copy button appears. Steps came from the team but the student never confirmed back
- Example: https://www.skool.com/ai-automation-society-plus/request-does-anyone-have-the-masterclass-transcripts-for-notebooklm

---

## 16. BUSINESS / CLIENT-FACING

### 16.1 First client pricing / what to charge
- Count: ~18 <!-- count refreshed 2026-07 -->
- Example: https://www.skool.com/ai-automation-society-plus/closing-the-loop-from-my-last-post

### 16.2 How to give client visibility / dashboards
- Count: ~8 <!-- count refreshed 2026-07 -->
- Verified: yes — Airtable as control plane, plus per-run logs
- Example: https://www.skool.com/ai-automation-society-plus/how-do-your-clients-see-what-their-whatsapp-ai-agent-is-saying

### 16.3 Where client owns the n8n vs you owning it
- Count: ~11 <!-- count refreshed 2026-07 -->
- Verified: yes — always set up in client's name, hand over keys
- Example: https://www.skool.com/ai-automation-society-plus/self-host-vs-not

### 16.4 Contract / scope creep concerns
- Count: ~6 <!-- count refreshed 2026-07 -->
- Verified: yes — "tool API changes that break flows are billable"
- Example: https://www.skool.com/ai-automation-society-plus/hostinger-safety

### 16.5 Pricing an AI automation build
- Symptom: "what do I charge for a chatbot / RAG assistant / voice agent?"; how to structure a retainer without hourly billing
- Count: ~7 (the single largest business cluster)
- Verified: partial — no single correct price. Route to **Agency Talks**; point at the canonical resources by title (the community "AI Agent & Workflow Pricing Framework" post, and Nate Herk's two pricing videos). The 10x framing: estimate the leaked revenue per month and quote roughly a tenth of it. Frame the offer around the system and outcome, not the tool
- Examples:
  - https://www.skool.com/ai-automation-society-plus/how-should-i-price-a-rag-based-ai-assistant-for-a-client-2
  - https://www.skool.com/ai-automation-society-plus/how-do-you-scope-ai-automation-retainers
  - https://www.skool.com/ai-automation-society-plus/how-much-are-chatbots-sellable-for
  - https://www.skool.com/ai-automation-society-plus/outcome-based-pricing-retainers

### 16.6 Client delivery ownership — who owns which accounts
- Symptom: builders set up Stripe/Supabase/Resend/n8n/hosting on their own accounts, plan to resell n8n hosting, or ask who pays for what at handoff
- Count: ~10
- Verified: yes — the client owns anything touching money, customer data, or outbound sending, and creates those accounts themselves from day one. **Hard constraint**: n8n's terms do not permit hosting or reselling access to others on non-enterprise plans, so the client must own their instance. AI providers require usage attributable to the business using it, making client-owned API keys a terms requirement rather than a preference
- Examples:
  - https://www.skool.com/ai-automation-society-plus/handover-hosting-automations
  - https://www.skool.com/ai-automation-society-plus/n8n-self-hosted-vs-cloud-for-clients
  - https://www.skool.com/ai-automation-society-plus/thoughts-on-hosting-everything-for-the-client
  - https://www.skool.com/ai-automation-society-plus/self-host-vs-not

### 16.7 Selling websites to local businesses
- Symptom: can produce Claude Code sites but doesn't know how to structure repos/domains/payments/bookings per client, or which businesses to approach
- Count: ~2
- Verified: yes — one repo per client under YOUR GitHub (small businesses won't manage GitHub), one Vercel project each, domains at a registrar pointed at Vercel; build one template repo and clone it; check the client's POS before reaching for Stripe; prospect on no-site/outdated/broken-UX signals
- Examples:
  - https://www.skool.com/ai-automation-society-plus/questions-about-website-implementation
  - https://www.skool.com/ai-automation-society-plus/questions-regarding-website-buildingselling

---

## 17. ANTIGRAVITY / CODEX / OTHER IDEs

### 17.1 Antigravity 2.0 broke Claude Code extension
- Count: ~1
- Verified: yes — Antigravity 2.0 removed VS-Code-style editor; rollback per Google docs
- Example: https://www.skool.com/ai-automation-society-plus/antigravity-update

### 17.2 Codex + Claude Code dual setup
- Count: ~4
- Verified: yes — CC for planning, Codex for review/grunt work; CLAUDE.md → AGENTS.md
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-vs-codex-2
  - https://www.skool.com/ai-automation-society-plus/claude-code-in-conjunction-with-codex
  - https://www.skool.com/ai-automation-society-plus/claude-codecodex-with-self-learning-and-context-engine-is-there-something-that-already-exists

### 17.3 PaperClip / Multica / agent-team UIs
- Count: ~2
- Example: https://www.skool.com/ai-automation-society-plus/how-can-i-get-visibility-into-what-my-ai-agents-are-doing-and-manage-their-progress

### 17.4 OpenClaw — recommendation, hardening, and OAuth ban risk
- Symptom: students running or considering OpenClaw ask how it compares; or a provider switch empties auth-profiles.json
- Count: ~5
- Verified: yes — the team recommendation is Claude Code (official, local, stable, cheaper). If running OpenClaw anyway: sandbox in Docker, restrict permissions, never expose to the public internet, vet ClawHub skills. **Use API keys, not consumer-subscription OAuth** — Anthropic and Google have issued account bans for that path
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-vs-openclaw
  - https://www.skool.com/ai-automation-society-plus/openclaw-with-oauth
  - https://www.skool.com/ai-automation-society-plus/openclaw-disconnect
  - https://www.skool.com/ai-automation-society-plus/looking-for-implementation-details-of-nates-klaus-openclaw-dashboard

### 17.5 Google Drive access inside Antigravity
- Symptom: Antigravity's connector store has no Google Drive integration
- Count: ~1
- Verified: yes — install the Google Workspace CLI and let the agent drive it; or add an MCP entry manually via the raw `mcp_config.json`
- Example: https://www.skool.com/ai-automation-society-plus/how-to-access-google-drive-data-in-claude-code-via-antigravity

### 17.6 PaperClip on a VPS
- Symptom: "can a Max login power PaperClip 24/7?"; Claude Code's browser login blocks a headless VPS deploy
- Count: ~2
- Verified: partial — prefer an API key (Commercial terms) with `budgetMonthlyCents` set per agent; headless login via the printed auth URL and the `code` parameter from the localhost callback; Hostinger VPSes are LXC containers whose restricted kernel access breaks PaperClip's embedded Postgres, so use external Postgres or a KVM provider
- Examples:
  - https://www.skool.com/ai-automation-society-plus/is-it-possible-to-use-claude-max-plan-subscription-in-paperclip
  - https://www.skool.com/ai-automation-society-plus/how-to-set-up-claude-code-on-a-vps-for-paperclip

---

## 18. COMMUNITY NAVIGATION & SUPPORT PROCESS

### 18.1 How AIS+ support works
- Symptom: "what does AIS+ add over the free community?", expectations of one-to-one screen-share or welcome calls, "how long until someone replies?"
- Count: ~5
- Verified: yes — post in Support Needed and tag the support team; typical response within 24 hours (team-stated); there are no one-to-one screen-shares, the team works the problem in-thread; email nate@aiautomationsociety.ai for membership/billing (the documented channel — don't route billing to a named individual); message the AIS Support account for account access and entitlements
- Examples:
  - https://www.skool.com/ai-automation-society-plus/benefits-or-difference-between-plus-and-free
  - https://www.skool.com/ai-automation-society-plus/support-response-time
  - https://www.skool.com/ai-automation-society-plus/admin-contact
  - https://www.skool.com/ai-automation-society-plus/locked-out

### 18.2 Where call and event recordings land, and when
- Symptom: missed AIS Live, a weekend seminar, or a weekly call and can't find the recording
- Count: ~6
- Verified: yes — recordings are edited after the event and announced by email to the address on your account; also check the Live Call Recordings classroom. A single missing session usually had technical problems live and is being re-recorded. The weekly "AI & Chill" call gets a community recap post instead of an email drop
- Examples:
  - https://www.skool.com/ai-automation-society-plus/ais-live-and-the-materials
  - https://www.skool.com/ai-automation-society-plus/ais-recordings
  - https://www.skool.com/ai-automation-society-plus/recording-from-this-weekend-seminar
  - https://www.skool.com/ai-automation-society-plus/last-week-chill-chat

### 18.3 Member perks — AI Discount Vault and partner discounts
- Symptom: perks-site registration sits on "pending" indefinitely; can't find the Hostinger/n8n member discount
- Count: ~5
- Verified: partial — the vault request must be submitted from Classroom > Member Perks > $3M Savings Vault, not by registering on the perks site directly; approvals are batched. Partner discounts also live under Community Resources > Discount Codes. Neither source thread ended with a confirmed resolution
- Examples:
  - https://www.skool.com/ai-automation-society-plus/ai-discount-vault-approval
  - https://www.skool.com/ai-automation-society-plus/n8n-discount
  - https://www.skool.com/ai-automation-society-plus/vps-recommendation

### 18.4 Scam / phishing DMs
- Symptom: an unexpected DM about prizes or offers, apparently connected to the community
- Count: ~1
- Verified: yes — a recurring scam campaign targets members; the official team does not solicit via unsolicited DMs. Don't reply or click; verify in Support Needed, then report and block
- Example: https://www.skool.com/ai-automation-society-plus/receive-notification

---

## 19. CLAUDE PRODUCT LINE — Cowork / Design / Chrome / Channels

### 19.1 Cowork — sandbox limits and session deletion
- Symptom: a Cowork skill fetching an external URL errors with a proxy error though the same task works in plain chat; separately, no delete button for Cowork sessions
- Count: ~2
- Verified: yes — Cowork skills run in a network-restricted sandbox with a strict proxy allowlist and there is no supported bypass (run it conversationally instead). Session cleanup is archive-only in-app; email privacy@anthropic.com for hard deletion. Scheduled tasks CAN be paused/deleted
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-cowork-skill-proxy-error-towards-youtube
  - https://www.skool.com/ai-automation-society-plus/cowork

### 19.2 Claude Design ↔ Claude Code handoff
- Symptom: design work (especially mobile/responsive) doesn't carry into Claude Code; Design refuses to push to GitHub; tweaks revert on reload
- Count: ~3
- Verified: yes — they are separate products with NO shared state; carry work over as files via the Handoff export or standalone HTML. The export can omit responsive CSS even when Design's mobile preview looks right — give Claude Code explicit breakpoint instructions or paste screenshots. Design can only READ from GitHub
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-design-mobile-optimisation-having-issues
  - https://www.skool.com/ai-automation-society-plus/claude-design-website-question
  - https://www.skool.com/ai-automation-society-plus/how-to-save-tweaks-on-claude-design

### 19.3 Claude in Chrome crashing the browser
- Symptom: the extension repeatedly crashes, sometimes taking all browser windows down
- Count: ~1
- Verified: yes — whole-browser crashes point at a local software conflict; removing app/site-blocker software (Cold Turkey Blocker in the confirmed case) fixed it. Separate reported limit: a single wait step longer than ~10 seconds errors out
- Example: https://www.skool.com/ai-automation-society-plus/claude-in-chrome-crashing

### 19.4 Claude Design shares the Claude Code usage pool
- Symptom: Code limits hit much earlier while also using Design; the separate Design usage bar disappeared
- Count: ~1
- Verified: yes — Anthropic merged Design's separate weekly bucket into the shared pool with chat and Claude Code
- Example: https://www.skool.com/ai-automation-society-plus/is-claude-design-using-my-normal-session-tokens

---

## 20. CLAUDE CODE — Remote, Mobile & Scheduled Runs

### 20.1 Remote Control won't attach
- Symptom: the Remote Control feature won't start or attach to a session
- Count: ~1
- Verified: yes — requires a **claude.ai subscription login** (not API-key auth, not a `setup-token` long-lived token), `/login` through claude.ai, and one prior local run in the project directory to accept workspace trust. ⚠️ Don't assert a plan tier from memory — check code.claude.com/docs/en/remote-control
- Example: https://www.skool.com/ai-automation-society-plus/claude-code-remote-control-not-working

### 20.2 Driving Claude Code from a phone
- Symptom: official mobile surfaces drop connections, lock commands, or can't run coding skills; headless VPS logins fail without a browser
- Count: ~4
- Verified: yes — the durable setup is a small VPS (or an always-on home box over Tailscale) running Claude Code inside tmux, SSH'd from the phone. Bill against Max via `claude setup-token` on a machine that HAS a browser, then export `CLAUDE_CODE_OAUTH_TOKEN` on the VPS — and make sure `ANTHROPIC_API_KEY` is NOT also set, or it silently switches to pay-per-token
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-on-android
  - https://www.skool.com/ai-automation-society-plus/claude-code-on-mobile
  - https://www.skool.com/ai-automation-society-plus/remote-use-of-claude-code

### 20.3 Scheduled tasks — three different mechanisms
- Symptom: scheduled tasks expire, die with the session, stop when the laptop sleeps, or can't touch local files
- Count: ~4
- Verified: yes — CLI/extension tasks are session-scoped and expire; Claude Desktop tasks persist via the OS scheduler but only while the machine is awake and the app is open; Cloud tasks persist but run against a fresh clone with no local files. **Cron does not inherit your shell environment** and background runs cannot complete interactive OAuth
- Examples:
  - https://www.skool.com/ai-automation-society-plus/claude-code-local-persistent-scheduled-tasks
  - https://www.skool.com/ai-automation-society-plus/claudecode-scheduled-task
  - https://www.skool.com/ai-automation-society-plus/hosting-client-automations-claude-desktop-vs-triggerdev

### 20.4 Telegram channel flaky
- Symptom: the Telegram channel replies inconsistently; approvals only appear on desktop; the terminal loops
- Count: ~1
- Verified: yes — zombie Claude Code processes compete for the single Telegram polling slot (kill all, start one fresh session); permission relay needs v2.1.81+; run from a regular terminal, not the VS Code integrated one
- Example: https://www.skool.com/ai-automation-society-plus/channels-in-claude-code

---

## 21. DESIGN, MEDIA & PROMPTING QUALITY

### 21.1 Frontend design degrades with each iteration
- Symptom: the first build looks decent; every "make it look like this screenshot" pass produces worse, more generic output
- Count: ~1
- Verified: yes — stop sending screenshots; prompt with design principles and specific CSS deltas; save a good version before iterating further; start a fresh session rather than iterating inside a degraded one
- Example: https://www.skool.com/ai-automation-society-plus/claude-code-frontend-chatbot

### 21.2 Professional marketing collateral from agents
- Symptom: agent-generated flyers/posters look amateur; Canva API output stays basic
- Count: ~1
- Verified: yes — Canva's Autofill API requires Enterprise for developer and end user (paid non-Enterprise plans get a limited dev trial only) and brand kits don't influence API output; use Recraft for brand-lockable assets plus HTML/CSS-to-Image for final composition
- Example: https://www.skool.com/ai-automation-society-plus/claude-code-design-output

### 21.3 Image pipelines lose quality when the theme changes
- Symptom: a pipeline tuned for one theme produces worse images once the subject switches
- Count: ~1
- Verified: yes — split every prompt into a fixed style-lock block (first, never changes) and a swappable subject block; image models weight leading tokens more heavily
- Example: https://www.skool.com/ai-automation-society-plus/claude-content-prompting

### 21.4 Programmatic slides / diagrams look flat
- Symptom: deterministic slide output is technically correct but flat; parameter tweaks don't help
- Count: ~1
- Verified: partial — hand-design ONE ideal slide, extract its rules (font-size ratios, white-space proportions, contrast, focal point), and encode those as opinionated defaults
- Example: https://www.skool.com/ai-automation-society-plus/built-a-full-slidediagram-generator-but-the-output-still-looks-terrible

### 21.5 Sycophancy — "are you happy with your answer?" triggers endless rebuilds
- Symptom: asking the model if it's satisfied produces "I can improve it" forever, regardless of quality
- Count: ~1
- Verified: yes — feelings-framed questions elicit agreeable responses. Ask forced-analysis questions instead: "what specific information is missing?", "spot logical weaknesses in your answer"
- Example: https://www.skool.com/ai-automation-society-plus/complete-beginner-but-very-eager-to-learn

### 21.6 Prompt over-engineering
- Symptom: freezing while hand-crafting the "perfect prompt"; fear that encoding rules will corner the model's creativity
- Count: ~2
- Verified: yes — always allow clarifying questions; use a second LLM as prompt writer for complex tasks; specific structured rules improve output where vague cautions ("be careful") change nothing
- Examples:
  - https://www.skool.com/ai-automation-society-plus/analysis-paralysis
  - https://www.skool.com/ai-automation-society-plus/claude-code-logic-experts-opinion

### 21.7 Quality ceiling — test the prompt outside the pipeline first
- Symptom: output quality feels capped and the builder plans an infrastructure migration
- Count: ~1
- Verified: yes — run the exact system prompt directly against the API for an hour. A quality jump means the pipeline degrades it; identical output means it's a prompt problem. Order of operations: prompt, then platform, then infrastructure
- Example: https://www.skool.com/ai-automation-society-plus/can-i-replace-n8n-with-pure-code-for-an-ai-video-ad-pipeline-and-is-it-worth-it

---

## 22. APP & FRONTEND DELIVERY (Lovable / Supabase / Vercel)

### 22.1 Escaping Lovable Cloud lock-in
- Symptom: a Lovable project with AI-generated DB/auth is stuck on Lovable Cloud; the agent calls it "not transferable"
- Count: ~1
- Verified: yes — no in-place switch exists, but only runtime data lives in Cloud; GitHub sync already carries code, migrations, RLS policies and edge functions. Auth passwords never export. Click "Remove Lovable Cloud" only after the new backend fully works
- Example: https://www.skool.com/ai-automation-society-plus/anyone-else-frustrated-by-the-lovable-cloud-supabase-lock-in

### 22.2 Client production apps built in Lovable
- Symptom: "is Lovable Cloud hosting reliable enough for a paid client app?"
- Count: ~1
- Verified: yes — no uptime SLA, a history of outages, and the app stops if credits run out. Keep building in Lovable but sync to GitHub and deploy through Vercel; use Supabase's own hosted service; the client owns the Vercel/Supabase accounts
- Example: https://www.skool.com/ai-automation-society-plus/lovable-cloud-hosting-frontend-backend

### 22.3 Supabase RLS without policies — silent empty reads
- Symptom: an AI-generated app's writes "succeed" but queries return nothing
- Count: ~1
- Verified: yes — code generators enable Row Level Security on new tables and forget the policies, so every query filters to zero rows
- Example: https://www.skool.com/ai-automation-society-plus/creating-real-artifacts

### 22.4 Supabase Google sign-in shows the raw project domain
- Symptom: the Google consent screen shows `<project>.supabase.co`; Supabase's custom domain is a paid per-project add-on (current price at supabase.com/pricing)
- Count: ~1
- Verified: yes — on mobile, switch to native Google sign-in and pass the ID token to Supabase with `signInWithIdToken`; the OS account picker shows your app name at no extra cost
- Example: https://www.skool.com/ai-automation-society-plus/google-sign-in-showing-supabase-domain

### 22.5 From throwaway artifact to persistent app
- Symptom: Claude artifacts break — no database, no cross-device state; students keep a parallel md file that drifts
- Count: ~1
- Verified: partial — use the artifact's built-in `window.storage` API as the ONLY data layer for simple cases; graduate to Vite/Next + Supabase + Vercel when you need multi-user, auth, sync or querying
- Example: https://www.skool.com/ai-automation-society-plus/creating-real-artifacts

### 22.6 Atomic transactional logic belongs in the database
- Symptom: a builder wants n8n as a mobile app's loyalty-points backend because it's familiar
- Count: ~1
- Verified: yes — read-check-write must be atomic; n8n adds HTTP hops and can't wrap DB transactions, so concurrent scans double-count. Points math goes in a Postgres function called via `supabase.rpc()`
- Example: https://www.skool.com/ai-automation-society-plus/for-the-shop-mobile-app-would-i-use-n8n

### 22.7 WordPress vs GitHub+Vercel for content-heavy sites
- Symptom: "should I migrate my content site to the course stack?"
- Count: ~1
- Verified: yes — different jobs. Keep WordPress for regular publishing and SEO; use GitHub+Vercel for interactive demos and dashboards linked from it
- Example: https://www.skool.com/ai-automation-society-plus/wordpress-vs-github-vercel-for-personal-site-whats-better-for-regular-content-updates

---

## 23. UNRESOLVED / OPEN PROBLEM TYPES (corpus has no fix)

- Salsify / niche PIM API integration with Claude Code
- Phenom People, iCIMS, Oracle Taleo ATS scraping
- Cloudflare Enterprise wall — managed unlockers required
- Concurrent WhatsApp message ordering at scale
- Production agent observability beyond OTel (cost-aware)
- Cross-machine Claude Code session history sync (no native solution)
- Routine scheduling under 1-hour minimum
- Claude Code plugin OAuth against providers without dynamic client registration (GitHub, reportedly Slack) — needs an Anthropic-side fix
- Merge-node behaviour with conditional retry branches in parallel media generation — no confirmed pattern
- claude-mem on-demand loading — no supported flag exists to make memory load only when asked
- Multi-account Messenger monitoring on browser-automation agents — no low-risk approach found
- FFmpeg into the n8n v2 Docker image — the pre-v2 `apk` method fails and no verified v2 command sequence was established in the corpus
- n8n Simple Memory with two triggers in one workflow — resolved by a version update in one case; the underlying platform behaviour was never characterised
