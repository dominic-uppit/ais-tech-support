# Fix Patterns — CLAUDE.md, skills & permissions

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

---

## CLAUDE CODE — CLAUDE.md & Skills

## Where CLAUDE.md goes and how the hierarchy works
**Symptom**: "I already have a Claude.md from before, will my new one overwrite it?" / "Do I need one per project?" Also: "the CLAUDE.md Claude Code generated for me feels generic yet oddly personalised — it references tools I use even in an empty folder. Where did that content come from and should I trust it?"

**Root cause**: Students don't know CLAUDE.md is hierarchical and stacks. A *generated* CLAUDE.md is assembled from four sources at once: whatever is already in the folder, what you said in your prompt, Claude Code's own general defaults for what a CLAUDE.md looks like, and your personal/global layer — a CLAUDE.md in your home folder plus the auto-memory feature that saves preferences and corrections across sessions. In the lesson you start from an empty folder, so there is nothing to scan and you get mostly prompt-plus-defaults, which is why it reads generic. The "personalised" parts come from the global layer.

**Fix steps**:
1. Claude Code walks up from the current directory and stacks every CLAUDE.md it finds into context — they don't fight, they combine.
2. **User-level** (loads in every project): `~/.claude/CLAUDE.md`
3. **Project-level** (loads only in that project, wins on conflicts): `./CLAUDE.md` in project root.
4. **Per-folder** (subdirectory-specific rules): drop a CLAUDE.md in any subdir.
5. Run `/memory` inside Claude Code to see exactly which CLAUDE.md files are loaded and where they're from.
6. Name must be `CLAUDE.md` in caps for autoload on Linux (case matters there).
7. **Keep it slim** — 200 lines max; aim well under 100 lines for a fresh project; 200 is the ceiling, not the target. If it grows, use it as an index that points to other docs (roadmap.md, change-log.md, handoff.md).
8. **Don't split with @import expecting context savings** — imports expand inline and still count.
- **Treat a generated file as version one, never a finished product.** Apply a rules-not-documentation filter: if a line does not change what Claude Code actually does, cut it.
- **Improve it through use, not up front.** The moment Claude Code does something you did not want, correct it and tell it to add that rule to CLAUDE.md. After a productive session, ask Claude to review and update the file based on what it learned — then periodically ask it to tighten the file, because captured learnings make it sprawl.
- **A structure that works**: project purpose and stack, folder structure, coding and workflow preferences, always-do rules, never-do rules, debugging and testing expectations.
- **On an existing project, bootstrap with `/init`** — Claude scans the folder and generates a starter file you then edit.
- **One CLAUDE.md per project, one project per folder.** Different projects have different rules and goals.

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/question-about-setting-up-claude-in-vs-code
- https://www.skool.com/ai-automation-society-plus/where-to-keep-memory-md-files-for-new-projects-to-pick-them-up
- https://www.skool.com/ai-automation-society-plus/claudemd-file
- https://www.skool.com/ai-automation-society-plus/claude-code-architecture-folder-structure
- https://www.skool.com/ai-automation-society-plus/creating-a-better-claudemd
- https://www.skool.com/ai-automation-society-plus/the-claudemd-file
- https://www.skool.com/ai-automation-society-plus/question-about-existing-project-wrap-up-and-starting-new-project

---

## Where to drop community skills
**Symptom**: "Course tells me to install n8n-skills, where do they go?"

**Root cause**: Documentation gap.

**Fix steps**:
1. **User-level skills** (loaded into every project): `~/.claude/skills/<skill-name>/SKILL.md`
2. **Project-level skills** (only this project): `.claude/skills/<skill-name>/SKILL.md`
3. Minimum file is `SKILL.md` — supporting Python/JSON files come later.
4. **Subagents** go in `.claude/agents/` (or `~/.claude/agents/` for global).
5. Commands go in `.claude/commands/` but skills are the newer preferred format.
6. **Quick install when /plugin is broken**: prompt Claude — "Clone https://github.com/czlonkowski/n8n-skills and copy the contents of its skills folder into my Claude Code skills directory."

**Lesson references**:
- `Claude Code → Phase 2 → 1.7 Skills` — teaches what skills are (reusable markdown-based workflows stored as slash commands) and the structure of a skill file.
- `Claude Code → Phase 2 → 1.8 Skills in Action` — building, testing, and iterating on skills from scratch.

Point students who ask "what even IS a skill" at these two before the install instructions.

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/about-skills
- https://www.skool.com/ai-automation-society-plus/where-are-the-md-files-discussed-in-every-level-of-claude-code-explained-in-21-minutes
- https://www.skool.com/ai-automation-society-plus/n8n-skills-in-claude-code

---

## `/plugin` command does nothing
**Symptom**: Typing `/plugin` or `/plugins` produces nothing, **or** the Manage Plugins panel opens with no plugins and no marketplace to browse — so "Skills 2.0" / skill-creator is nowhere to be found.

**Root cause**: Several distinct things produce the same empty panel: the official Anthropic marketplace has not been loaded into Claude Code yet; the plugin marketplace needs Claude Code v1.0.33 or later; the command name differs by interface; and VS Code's own Extensions marketplace is a completely separate system that will never list Claude Code plugins. Separately, "Skills 2.0" is video branding for the upgraded skills system, not an installable product — the thing being installed in that video is the **skill-creator plugin**.

**Fix steps**:
1. Use the right command for your interface: in the terminal CLI it is `/plugin` (singular); in the VS Code extension it is `/plugins` (plural).
2. Check your version with `claude --version`. The plugin marketplace needs v1.0.33 or later. Update with `npm update -g @anthropic-ai/claude-code` if you installed via npm, or `brew upgrade claude-code` if you used Homebrew.
3. Load the official marketplace: run `/plugin marketplace add anthropics/claude-code` in Claude Code, or open the Marketplaces tab in the Manage Plugins panel and add it there.
4. If the panel is still empty on a current version — which happens — fully restart Claude Code, and if needed reinstall it, then re-add the marketplace. In one confirmed case this restart/refresh cycle was what actually made the marketplace appear.
5. Once the marketplace loads, go to the Plugins tab, search for `skill-creator`, install it for the project, and restart Claude Code for it to take effect.
6. Do not search the VS Code Extensions sidebar for these — that is a different marketplace and will not have them.
7. Fallback for installing czlonkowski's n8n-skills without `/plugin` at all, prompt Claude Code: "Install czlonkowski/n8n-skills manually by cloning https://github.com/czlonkowski/n8n-skills and copying the contents of its skills folder into my Claude Code skills directory."

**Confidence**: medium — team-verified; several distinct causes share one symptom, so expect to work through the list

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/need-help-connecting-n8n-mcp-server-on-hostinger
- https://www.skool.com/ai-automation-society-plus/claude-marketplace
- https://www.skool.com/ai-automation-society-plus/no-plugins-or-marketplace-in-my-vs-code

---

## Skills don't learn on their own — feedback-log.md plus periodic consolidation
**Symptom**: Corrections to a skill's output never carry over to future runs.

**Root cause**: Skills are static instruction files; nothing persists feedback unless written into the skill or a referenced file; dumping every correction into SKILL.md bloats it until rules get ignored.

**Fix steps**:
1. After a correction, explicitly instruct Claude to edit the skill file with a rule preventing that mistake class.
2. Keep the main skill file to permanent high-level rules; create feedback-log.md beside it for case corrections, and add one skill line: 'Before generating output, check feedback-log.md for mandatory overrides.'
3. Periodically have Claude compress the log into a handful of generalized rules.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/how-do-skills-improve-over-time-in-claude-code

<!-- pattern: /how-do-skills-improve-over-time-in-claude-code -->

---

## claude.ai Projects per business function vs Claude Code skills
**Symptom**: A non-coder feeds hours of business context into one long chat and still gets shallow results; the classroom skill templates look too code-heavy to be the answer.

**Root cause**: Context stored in a chat thread dies with the thread, and a single 'knows everything about me' brief is broad but shallow. Separately, Claude Code SKILL.md files are a developer surface — the wrong tool for a claude.ai business workflow.

**Fix steps**:
1. Create one claude.ai Project per business function (Sales, Lead Gen, Appointment Setting, ...). Custom instructions carry the durable stuff — ICP, offer, pricing, tone of voice. The knowledge base carries the evidence — real messages that converted, the objections you actually hear, your qualification criteria.
2. Don't throw away the long interview chats: pull the function-specific parts out of each and move them into the matching Project, so every new conversation starts with that context already loaded.
3. Narrow and deep beats broad and shallow — separate focused Projects outperform one Project that tries to know everything.
4. Mental model the team confirmed: Skills are universal standards that follow you everywhere (brand voice, formatting, communication style); Projects are the function-specific knowledge base and instructions. Leave SKILL.md files for coded workflows in Claude Code.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/how-to-build-skills-relevant-for-me-and-my-business-in-claude

<!-- pattern: /how-to-build-skills-relevant-for-me-and-my-business-in-claude -->

---

## Where to find quality skill collections (Claude Code / Antigravity)
**Symptom**: Students ask where people share high-quality, productivity-boosting skills for Claude Code or Antigravity.

**Root cause**: Collections are scattered across GitHub repos, standalone directories, and Reddit, with no single index.

**Fix steps**:
1. Claude Code: the 'awesome-claude-skills' repositories on GitHub (published under the ComposioHQ and karanb192 accounts) are the two the team points at most; awesomeclaude.ai is a browsable directory; Anthropic's own prompt library and their Claude Code skills documentation are worth reading as a baseline; r/ClaudeAI is active for what people are actually building.
2. Antigravity: the sickn33/antigravity-awesome-skills repository is the largest collection, with several smaller repos leaning toward software and business ops.
3. Many skills are cross-compatible — a skill built for Claude Code will often run in Antigravity and vice versa, so don't restrict your search to one tool's ecosystem.
4. Vet any third-party skill before running it in a project that has live credentials or production access — read what it actually does first.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claudeantigravity-skills

<!-- pattern: /claudeantigravity-skills -->

---

## claude-mem plugin eating context at session start — what to check first
**Symptom**: After installing the claude-mem plugin, sessions start with roughly 10% of context already consumed, sometimes appearing to show observations from an unrelated project.

**Root cause**: claude-mem injects prior-session observations at SessionStart, which costs context by design. Apparent cross-project leakage is usually something else: in the source thread the folders had distinct names and the real cause was the VS Code multi-window agent view surfacing another window's running session. A genuine collision is possible when two project folders share the same basename, since claude-mem keys projects on the folder basename.

**Fix steps**:
1. Confirm what is actually being injected before changing anything. Open the local viewer at http://127.0.0.1:<PORT> — replace <PORT> with the value of CLAUDE_MEM_WORKER_PORT found in ~/.claude-mem/settings.json. The settings modal there shows exactly which observations load at session start.
2. If the viewer refuses to connect, the worker daemon is probably not running. Check the readiness endpoint at that same port, and if it fails, restart Claude Code — it usually self-recovers on next launch.
3. If the observations are from the right project but stale or too many, shrink the payload: set CLAUDE_MEM_CONTEXT_OBSERVATIONS=10 and CLAUDE_MEM_CONTEXT_SESSION_COUNT=3 before launching (the defaults are 50 and 10).
4. If you genuinely have two project folders with the same basename (e.g. /work/api and /personal/api), either rename one so the basenames differ, or set CLAUDE_MEM_DATA_DIR=<path> — replace <path> with a per-client directory such as $HOME/.claude-mem-work — to get a fully separate memory database for that shell.
5. If sessions in VS Code keep landing in another project's context, that is the multi-window agent view listing tasks across all open windows, not memory leakage. Force-quit and restart VS Code; this is what actually resolved it in the source thread.
6. Note the limitation honestly: as of the thread there was no supported flag to make claude-mem load memory only on demand. Don't hand-edit the plugin's hook configuration to force it — that is unsupported and gets overwritten on update.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-mem-eating-up-10-context-at-start-up

<!-- pattern: /claude-mem-eating-up-10-context-at-start-up -->

---

## One folder per project + hub-and-spoke executive assistant
**Symptom**: Students don't know whether every build needs its own folder, or cram every project and client credential into one executive-assistant folder like the simple version shown in Nate Herk's EA video.

**Root cause**: A Claude Code project is a folder with its own .claude/ (CLAUDE.md, skills, MCP config) loaded each session — oversized shared folders mean more tokens, slower responses, hallucinations, and secrets exposure.

**Fix steps**:
1. Default to one folder per project (group them per business, e.g. Claude Code/Business 1/<project>); one GitHub repo per project; universal skills go in ~/.claude/skills/ while project-specific ones stay scoped to that project's .claude/skills/.
2. Hub-and-spoke EA: the EA's CLAUDE.md is a MAP (where each project lives, what it's for) — never a copy of project files. Point it at ONE shared roster/status file that the agent reads on demand rather than pulling it in with @import, which loads the whole file every session.
3. Credentials stay in each project's own .env; the EA knows who a client is and who handles them, but never holds their keys. Vet any third-party skill before it runs next to live client accounts.
4. Save working n8n workflow JSONs into the relevant project folder so Claude Code has context without re-pulling from n8n; put 24/7 chat-facing agents (e.g. a Telegram planner) in n8n or another hosted runtime rather than local Claude Code, which only runs while your machine is on.
5. Scale EA autonomy gradually: read-and-suggest with your approval first, releasing one task at a time; anything that writes to a live client account stays behind approval.
6. Known gotcha raised by the support team in one thread: a global skill in ~/.claude/skills/ is ignored in any project that has its own .claude/skills/ folder, with no warning — run /doctor inside that project to see what actually loaded.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/executive-assistant-herk-2-related-need-help
- https://www.skool.com/ai-automation-society-plus/laying-out-folder-skills-claudemd-etc-between-my-business-and-side-projects
- https://www.skool.com/ai-automation-society-plus/local-setup
- https://www.skool.com/ai-automation-society-plus/vs-code
- https://www.skool.com/ai-automation-society-plus/when-to-create-different-projects-vs-using-same-project
- https://www.skool.com/ai-automation-society-plus/how-do-you-structure-projects

<!-- pattern: /executive-assistant-herk-2-related-need-help -->

---

## Documenting builds and SOPs — docs live with the build; generate from the system
**Symptom**: Builders can't remember what their builds do, ask which SOP tool to buy, and hand-written docs drift stale.

**Root cause**: Documentation kept apart from the build drifts; hand-writing first drafts is unnecessary when the system itself can be read.

**Fix steps**:
1. n8n: document on the canvas, not in a side journal — Sticky Notes for what each section does and why, plus per-node Settings > note with 'Display note in flow' turned on for the small specifics. Export workflow JSONs to a folder or repo for real version history, because free/self-hosted n8n only keeps the last 24 hours.
2. Claude Code: run /init on an existing build to have it read the project and draft CLAUDE.md as a first draft, then correct anything off. Also ask it for a short non-technical README per project — that is the doc a client or portfolio reviewer actually reads.
3. SOPs: export the n8n workflow JSON, hand it to Claude Code and have it turn the node chain into a written SOP — then review it, because it will get some things wrong or invent them. Keep SOPs in plain markdown so they move between Notion, a repo, or an SOP tool without lock-in.
4. To stop SOP drift, treat the doc as generated rather than authored: n8n's API can list and export every workflow, so a scheduled job can pull the JSONs, regenerate the docs and commit them.
5. Auto-capture tools when you need them: Scribe, Tango or Guidde for click-through processes; Excalidraw or draw.io for system diagrams.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/help-a-noobie
- https://www.skool.com/ai-automation-society-plus/workflow-sop

<!-- pattern: /help-a-noobie -->

---

## Private GitHub repo per project — .gitignore before first push; rotate leaked keys
**Symptom**: Non-coders ask whether to connect projects to GitHub and how .gitignore fits.

**Root cause**: Without version control there's no rollback when Claude breaks something; a first push before .gitignore permanently embeds secrets in history.

**Fix steps**:
1. Every project gets its own private repo; tell Claude Code to 'set up git, write a .gitignore covering .env and key files, and push to a new private repo' — verify the .gitignore before the first push.
2. If a key slips into a commit: rotate it at the provider; don't try to scrub git history.
3. Commit after each working state so rollback targets are fresh.

**Lesson reference**: Build Your Portfolio — GitHub lesson and the Secrets Management lesson after it

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/should-i-connect-to-github

<!-- pattern: /should-i-connect-to-github -->

---
## CLAUDE CODE — Permissions & Security Posture

## 'Can Claude Code read all my files?' — the containment ladder
**Symptom**: Students discover Claude Code reads files outside the opened folder and fear it can change anything on the machine.

**Root cause**: The Bash tool runs with the user's own OS permissions; parent-folder CLAUDE.md reads are by design; supervision (permission prompts) is what gates risk — bypass mode is the dangerous switch.

**Fix steps**:
1. Keep bypass-permissions off (no --dangerously-skip-permissions) on any machine holding sensitive files — supervision, not raw capability, is what separates Claude Code's risk profile from a fully autonomous agent. Read the short purpose note Claude Code prints above each bash command before approving it; even scanning for keywords tells you what it is about to do.
2. Use plan mode (Shift+Tab) for read-only exploration, deny rules in .claude/settings.json for sensitive paths, and /sandbox for OS-level Bash restrictions.
3. For unattended runs or any session where you are skipping permissions, use Anthropic's official reference devcontainer from the anthropics/claude-code repo — it adds an egress firewall plus filesystem isolation, and is cleaner than a full VM.
4. Reassurance on the specific trigger for this question: Claude Code reading a CLAUDE.md in a parent folder is by design, not a leak — that is how it gathers context.

**Confidence**: high — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/security-concerns

<!-- pattern: /security-concerns -->

---

## Permission prompts during guided setup are normal — modes, Auto mode, notifiers
**Symptom**: Students following setup videos get ~10-30 'Allow this bash command?' prompts the video never showed, worry 'localhost' means broken, or miss that Claude is waiting on approval.

**Root cause**: Instructor sessions run different permission settings and routes vary per run; localhost:3000 is the MCP server correctly running on the student's machine; no built-in notification exists when Claude blocks on approval.

**Fix steps**:
1. Getting 10-30 approval prompts where the instructor got none is normal: instructor sessions run different permission settings, and Claude does not take the same route twice. Shift+Tab cycles permission modes (ask-before-edits -> edit automatically -> plan). Higher-risk commands still prompt even in edit-automatically mode — that is the design, read them rather than trying to eliminate them.
2. To reduce prompts further: Auto permission mode uses an AI classifier so safe actions run and risky ones still prompt (`claude --permission-mode auto`, or via VS Code settings) — note it is only on Max, Team, Enterprise or API plans, not Pro. Otherwise use the allow/deny lists in settings.json; you can literally ask Claude Code to edit its own permissions config, e.g. 'auto-approve python commands and deny curl, sudo and rm -rf'.
3. If you keep missing that Claude is blocked waiting on approval, install a notifier — the 'Claude Notifier' VS Code extension plays different sounds per event including waiting-on-permission, or build your own with hooks (the poster in this thread built one with hooks and confirmed it working).
4. 'Connected to http://localhost:3000' during n8n MCP setup is correct and expected — localhost just means your own machine, which is where the MCP process runs. The separate 'n8n connection: configured' line is the one confirming your n8n instance is reachable.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/allow-this-bash-command
- https://www.skool.com/ai-automation-society-plus/claude-code-balance-between-security-and-autonomy

<!-- pattern: /allow-this-bash-command -->

---

## Client credential handling playbook — OAuth2, secret env vars, scoped tokens, handoff calls
**Symptom**: Freelancers hold raw client passwords/API keys, don't know what access to request, or how clients should share keys safely.

**Root cause**: Credentials collected directly instead of delegated auth and client-owned secret storage.

**Fix steps**:
1. Email/Google: OAuth2 only — never the client's password and never app passwords (Google has deprecated basic password auth for Gmail). On n8n Cloud the client just clicks 'Sign in with Google'; the client authorises and can revoke at any time without breaking anything else.
2. Self-hosted n8n with your own Google Cloud app is the trap: Gmail scopes are sensitive/restricted, so a consent screen left in Testing mode expires the refresh token in about 7 days and the client's connection silently dies. Have the client own the Google project in their own Workspace with the consent screen set to Internal, or use n8n Cloud's already-verified app. Request narrow scopes (read + compose) rather than full mailbox access. (Raised by a community member, not the support team.)
3. The client creates their own LLM/API accounts on their own billing and generates project-scoped keys — an Airtable PAT limited to that one base, Stripe/HubSpot restricted keys — so you never carry their billing or their liability.
4. Code reads secrets from environment variables or a secret store the client owns. On Trigger.dev the client is invited to the team and enters their keys as Secret environment variables (once marked Secret the value is hidden in the dashboard and can't be viewed there after creation — UI-level protection, not a cryptographic guarantee, so don't promise the client it's beyond your reach); you reference them in code by variable name only. For one-off handoffs use a 1Password or Bitwarden single-view expiring link.
5. Build against your own dummy credentials, then do a short live handoff call where the client shares their screen and pastes their keys or completes the OAuth flow themselves — you never touch a raw secret. Decide up front who owns the n8n instance; building inside the client's own instance keeps the credentials with them from day one.
6. Claude Code app routines specifically: keys go in the routine's environment variables, because the .env is not part of the repo that gets pulled and there is no secrets manager in that environment. Move to the platform's secrets manager when you deploy anywhere else.
7. At roughly 5-10+ clients, graduate from encrypted env vars (e.g. SOPS with the keys on a separate host) to a managed vault such as Doppler, 1Password or HashiCorp Vault.
8. If you are already holding a client's password or key, migrate: swap the email to OAuth2 first (highest risk), then move the API keys, then rotate everything you previously held.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/clients-api-keys
- https://www.skool.com/ai-automation-society-plus/clients-api-keys-and-password-management
- https://www.skool.com/ai-automation-society-plus/need-helpadvice-on-client-creds-for-n8n
- https://www.skool.com/ai-automation-society-plus/credential-mgmt-in-claude-routines
- https://www.skool.com/ai-automation-society-plus/help-needed-4722bd0f

<!-- pattern: /clients-api-keys -->

---

## MCP vs CLI — decision framework
**Symptom**: Both a CLI and an MCP exist for the same service; students don't know which to use.

**Root cause**: MCP servers inject their full tool schemas into the context window at session start (the GitHub MCP alone exposes 90+ tools), while agents can already drive well-known CLIs from training. Benchmarks cited in-thread put MCP at roughly 30-35x the tokens of the CLI equivalent for the same task, though the exact multiplier varies.

**Fix steps**:
1. Default to the CLI for well-known tools (git, gh, npm, docker, curl, aws) and for simple operations — Claude already knows the commands. For a CLI it doesn't know, paste the docs and it picks it up.
2. Reach for MCP when you need organisational standardisation across a team, enterprise auth and permissions, custom internal tools the model has never seen, or environments where the CLI isn't installed.
3. Beginners: CLI-first for faster results, lower token cost and fewer moving parts; add MCP only once you hit a case where it genuinely earns its place.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/difference-between-cli-and-mcp

<!-- pattern: /difference-between-cli-and-mcp -->

---

## Build an MCP server for software that has an API but no MCP
**Symptom**: No existing MCP server for a needed tool.

**Root cause**: Capability gap — MCP generation tooling automates this when an OpenAPI spec exists.

**Fix steps**:
1. If the vendor publishes an OpenAPI/Swagger spec, generate from it: Python — FastMCP's `from_openapi` constructor, passing the spec you downloaded from the vendor's docs; TypeScript — Speakeasy or Stainless.
2. No spec published? Have Claude Code read the vendor's API documentation and generate an OpenAPI spec first, then feed that into the generator.
3. Curate afterwards — auto-generated servers work, but models do meaningfully better once you prune endpoints you don't need and rewrite the tool descriptions. Test with MCP Inspector (ask Claude Code to set it up via npm).

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/creating-an-mcp

<!-- pattern: /creating-an-mcp -->

---

## Deployed workflows don't need MCP — production uses native function calling
**Symptom**: Students think Trigger.dev/Modal-deployed agentic workflows need MCP servers attached, and worry a coded deployment will be 'less adaptive' than an n8n agent node.

**Root cause**: MCP is a dev-time layer: it standardizes how Claude Code/Desktop discovers and talks to APIs while you build. In production the MCP layer goes away — your code makes direct API calls and the agentic behaviour comes from the LLM SDK's native function calling. The n8n agent node is the same pattern under the hood (n8n turns each attached tool into a JSON schema, sends it to the LLM, and executes whatever the LLM picks).

**Fix steps**:
1. Use MCPs during development so Claude Code can discover the APIs and write the tool functions for you.
2. Deploy plain functions plus the LLM SDK: pass JSON descriptions of those functions to the Anthropic/OpenAI API and let the model choose which to call per input.
3. Runtime adaptability is preserved — the LLM can still call something like list_folders() mid-run, see new state, and adapt. Nothing becomes hardcoded just because MCP is gone.
4. Treat the WAT-framework 'tools' in production as the concrete scripts/API calls you wrote during development.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/mcp-dilemma-needed-for-triggerdev

<!-- pattern: /mcp-dilemma-needed-for-triggerdev -->

---

## GitHub plugin auth fails: 'does not support dynamic client registration'
**Symptom**: GitHub (and reportedly Slack) plugin/MCP install fails with 'SDK auth failed: Incompatible auth server: does not support dynamic client registration'.

**Root cause**: Known bug: GitHub's OAuth server doesn't support Dynamic Client Registration (RFC 7591), which Claude Code's plugin auth flow expects. GitHub requires manual app registration before it issues a fixed client ID. This is not user error.

**Fix steps**:
1. Confirmed working: use the GitHub CLI instead — run `gh auth login` and Claude Code manages repos through it. In the source thread this fully replaced the plugin.
2. Untested fallback reported in-thread (the original poster had already tried a token env var without success, so treat this as unverified): create a fine-grained PAT at GitHub Settings > Developer Settings > Personal Access Tokens > Fine-grained tokens with the repo permissions you need, then set GITHUB_PERSONAL_ACCESS_TOKEN to that token value (GitHub shows it once at creation — copy it then) as a system environment variable, and restart Claude Code.
3. Don't keep debugging the plugin OAuth flow — the fix has to come from Anthropic's side.

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/github-plugin-error-for-claude-code

<!-- pattern: /github-plugin-error-for-claude-code -->

---

## Office 365 files with Claude — add-ins to edit, M365 Connector to read, ms-365 MCP to automate
**Symptom**: 'File incompatible' when uploading a .docx to Claude chat; ad-hoc generated conversion scripts burn tokens every session.

**Root cause**: Claude's chat interface only accepts PDFs, plain text and images (.docx is a ZIP of XML, so it is rejected outright). Without a proper integration, Claude regenerates a python-docx/JS conversion approach from scratch each session. The right tier of Microsoft integration depends on the job.

**Fix steps**:
1. Editing documents interactively: install the official 'Claude by Anthropic' add-ins for Word/Excel/PowerPoint from the Microsoft Marketplace. They require a paid Claude plan (Pro, Max, Team or Enterprise) at no extra cost, and read/write the open document natively.
2. Read-only research across your tenant: enable the free Microsoft 365 Connector in Claude settings under Customize > Connectors (SharePoint, OneDrive, Outlook, Teams).
3. Automation from Claude Code (documented in the thread but not confirmed working by the poster): use the Softeria ms-365-mcp-server. FIRST set persistent token-cache paths in your shell profile, otherwise tokens are wiped on every npm update — set MS365_MCP_TOKEN_CACHE_PATH to $HOME/.config/ms365-mcp/.token-cache.json and MS365_MCP_SELECTED_ACCOUNT_PATH to $HOME/.config/ms365-mcp/.selected-account.json, and create that directory first.
4. Then register it: `claude mcp add ms365 -s user -- npx -y @softeria/ms-365-mcp-server --org-mode` (use --org-mode only for enterprise/tenant accounts; on Windows wrap the command as `cmd /c "npx -y @softeria/ms-365-mcp-server --org-mode"` or the flags won't pass through). Restart Claude Code, run the login tool, and complete the device-code sign-in at microsoft.com/devicelogin.

**Confidence**: low — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/rw-office-365-files-with-claude

<!-- pattern: /rw-office-365-files-with-claude -->

---

## Google Drive access inside Antigravity — Google Workspace CLI or manual MCP config
**Symptom**: Antigravity's connector store has no Google Drive integration.

**Root cause**: No store connector exists, but Antigravity supports manual MCP entries via its raw config, and a CLI can bypass MCP entirely.

**Fix steps**:
1. Confirmed-working: install the Google Workspace CLI (github.com/googleworkspace/cli) and let the agent drive it.
2. Manual MCP: agent side panel dropdown > MCP Servers > Manage > View raw config (mcp_config.json) — add Rube MCP (Composio token from their site) or the hardened Google Workspace MCP (needs a Google Cloud OAuth Desktop-app client with Drive/Docs/Sheets APIs enabled).

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/how-to-access-google-drive-data-in-claude-code-via-antigravity

<!-- pattern: /how-to-access-google-drive-data-in-claude-code-via-antigravity -->

---
