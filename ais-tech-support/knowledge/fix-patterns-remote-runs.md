# Fix Patterns — Remote, mobile & scheduled runs; Cowork, Design & Chrome

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

---

## CLAUDE CODE — Remote, Mobile & Scheduled Runs

## Remote Control won't attach — subscription login required, /login via claude.ai, workspace trust
**Symptom**: Claude Code's Remote Control feature won't start or attach to a session.

**Root cause**: Unmet requirements — most commonly auth: Remote Control needs a claude.ai subscription login, and API-key auth is not supported.

**Fix steps**:
1. Sign in via /login through claude.ai (API keys don't count). A `setup-token` / `CLAUDE_CODE_OAUTH_TOKEN` long-lived token also fails — it can only make model requests, so use a full-scope login.
2. Unset `ANTHROPIC_API_KEY` if it's set, and check no `ANTHROPIC_BASE_URL` / Bedrock / Vertex variable is routing the session away from api.anthropic.com — Remote Control is disabled in that case.
3. Run claude in the project directory once and accept the workspace trust dialog before attaching remotely (home-directory trust doesn't persist).
4. On Team/Enterprise, an Owner must enable the Remote Control toggle in Claude Code admin settings — it's off by default there.
5. ⚠️ Don't assert a plan tier from memory. If it still won't attach, run `claude doctor` to see which eligibility check failed and confirm the current requirements at code.claude.com/docs/en/remote-control.

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-code-remote-control-not-working

<!-- pattern: /claude-code-remote-control-not-working -->

---

## Claude Code from a phone — VPS + tmux + SSH with setup-token (and the lighter options)
**Symptom**: Students want to control Claude Code from a phone; official mobile surfaces (/remote, Dispatch, Channels) drop connections, lock commands, or can't run coding skills; headless VPS logins fail without a browser.

**Root cause**: Mobile surfaces are research previews (remote views, not terminals); a durable session must live on an always-on machine; default OAuth needs a browser.

**Fix steps**:
1. Reliable setup: a small cloud VPS (or an always-on home mini PC reached over Tailscale), Claude Code running inside tmux or screen so the session survives disconnects, and SSH in from the phone with Termius or Termux.
2. To bill against a Max subscription instead of API tokens: run `claude setup-token` on a machine that HAS a browser — it prints a long-lived OAuth token. On the VPS run `export CLAUDE_CODE_OAUTH_TOKEN="<paste the exact token setup-token printed>"`. Make sure ANTHROPIC_API_KEY is NOT also set on that VPS, because the API key takes priority and silently switches you to pay-per-token billing.
3. Alternative headless auth with no token: run `claude` on the VPS, copy the auth URL it prints, open it in a browser on any machine where you're already logged into your Claude account, then copy the `code` parameter out of the localhost redirect URL and paste it back into the VPS terminal.
4. If you'd rather use an API key on the VPS, create one in the Anthropic Console (console.anthropic.com) and `export ANTHROPIC_API_KEY=<your console key>` in .bashrc/.profile — this is pay-per-token, not subscription.
5. Lighter options: VS Code Remote Tunnels opened from vscode.dev in the phone browser; Web Sessions at claude.ai/code. Third-party clients also exist (remoteCC — QR code plus Expo Go; Happy at happy.engineering — MIT open source, NOT an Anthropic product, E2E-encrypted relay, self-host the relay for sensitive client work).
6. Set expectations on the official mobile surfaces: /remote is a research preview that can drop the connection with no auto-recover and locks some commands to the local machine, and Dispatch is built around Cowork tasks rather than coding — neither is a terminal replacement today.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/claude-code-on-android
- https://www.skool.com/ai-automation-society-plus/claude-code-on-mobile
- https://www.skool.com/ai-automation-society-plus/remote-use-of-claude-code
- https://www.skool.com/ai-automation-society-plus/how-to-set-up-claude-code-on-a-vps-for-paperclip

<!-- pattern: /claude-code-on-android -->

---

## Claude Code scheduled tasks — session-scoped vs Desktop vs Cloud, OS cron, keep-awake, OAuth
**Symptom**: Scheduled tasks expire, die with the session, stop when the laptop sleeps, or can't touch local files; client automations on Desktop /schedule stop when the app closes.

**Root cause**: Three scheduling modes differ by design: CLI/VS Code extension tasks are session-scoped and expire (the support team cited ~3 days, and they die with the session); Claude Desktop tasks persist via the OS scheduler but only run while the machine is awake and the app is open; Cloud tasks persist but run against a fresh clone with no local file access. Separately, cron does not inherit your shell's environment variables, and a background run cannot complete an interactive OAuth flow.

**Fix steps**:
1. For local persistent scheduling, either use Claude Desktop scheduled tasks, or drive headless runs from an OS-native scheduler (cron / launchd / Windows Task Scheduler) calling `claude -p "<the exact task instruction you want run>"`.
2. Cron cannot see variables set in .bashrc or .profile. Put ANTHROPIC_API_KEY directly in the crontab line, or source an env file inside the cron command (Docker: pass it through the container environment config). Create the key in the Anthropic Console (console.anthropic.com).
3. For logs and post-mortem debugging, have cron launch the run inside tmux with tee, e.g. `tmux new-session -d -s claude-task 'claude -p "<your prompt>" 2>&1 | tee -a /var/log/claude-task.log'` — you get a log file plus a session you can attach to if it hangs. This is the step the poster confirmed 'made all the difference'.
4. Keep the machine awake: Mac — System Settings > Battery/Energy > 'Prevent automatic sleeping when the display is off' and sleep set to Never on power, or the Amphetamine app; Windows — Settings > System > Power & Sleep set to Never when plugged in, or PowerToys Awake. On a laptop, closing the lid still forces sleep unless you change the lid-close behaviour.
5. Background OAuth breaks because the flow tries to open a browser. Switch those services to API-key auth where possible, or run the OAuth flow manually once before scheduling so the token is already cached.
6. For client-facing or rock-solid unattended runs, move off a laptop entirely: VPS cron, GitHub Actions cron, Trigger.dev, or claude.ai cloud scheduled tasks.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/claude-code-local-persistent-scheduled-tasks
- https://www.skool.com/ai-automation-society-plus/claudecode-scheduled-task
- https://www.skool.com/ai-automation-society-plus/i-want-to-build-a-content-system
- https://www.skool.com/ai-automation-society-plus/hosting-client-automations-claude-desktop-vs-triggerdev

<!-- pattern: /claude-code-local-persistent-scheduled-tasks -->

---

## Telegram channel flaky — zombie processes, v2.1.81+ approvals, VS Code terminal loop
**Symptom**: Telegram channel replies inconsistently; approvals only appear on desktop; terminal stuck in a loop.

**Root cause**: Zombie Claude Code processes compete for the single Telegram polling slot; permission relay needs v2.1.81+; channels expose only basic messaging by design; running in the VS Code integrated terminal triggers an auto-install loop.

**Fix steps**:
1. Kill ALL Claude Code processes (Task Manager on Windows) and start one fresh session before reconnecting Telegram. Zombie processes compete for the single Telegram polling slot and messages get dropped silently. This is the fix the poster confirmed resolved her problem.
2. Approvals only appearing on the desktop: run `claude --version` and update if you are below v2.1.81 — permission relay to Telegram did not exist before that. Once relayed, reply in Telegram with `yes ABCDE` or `no ABCDE`, where ABCDE is the 5-character request ID shown with the prompt.
3. Run Claude Code from a regular terminal window, not the VS Code integrated terminal — the CLI's extension auto-install triggers a loop there.
4. Set expectations: Channels is a preview that exposes only basic messaging, not the full Claude Code toolset, and it is effectively single-user — several people chatting to the same bot about the same project folder will conflict.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/channels-in-claude-code

<!-- pattern: /channels-in-claude-code -->

---

## Connecting separately built agents — subagents are session-scoped; use an orchestrator service
**Symptom**: Several agents built as separate Claude Code projects 'don't talk to each other'; brute-forcing the whole system in one session burns limits.

**Root cause**: Claude Code's subagents/agent teams only work within a single session — they cannot connect separately deployed projects.

**Fix steps**:
1. Deploy each agent as its own service with its own API endpoint (Claude Agent SDK pattern). A central orchestrator receives every trigger — webhook, form, call-end event — routes it to the right agent, gets the result back and logs it. Agents never call each other directly.
2. At roughly 2-5 agents prefer the orchestrator over a message bus; a bus only earns its keep with many independently-developed agents reacting to unpredictable events. Rebuild one agent at a time into a standalone service, then wire it into the orchestrator's routing.
3. The orchestrator project's CLAUDE.md (keep it under ~200 lines) holds the routing rules, the endpoint contract for each agent, and the credential structure — so you stop re-explaining the architecture every session.
4. Related token hygiene from the same answer, since brute-forcing the whole system in one session is what burns the limits: plan mode before building, one task per session, /clear between unrelated tasks, one deliberate /compact around 60% context if needed (not repeatedly), and disconnect MCP servers you are not using.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/help-with-mission-control

<!-- pattern: /help-with-mission-control -->

---

## Claude Code in dev containers — persist ~/.claude AND ~/.claude.json
**Symptom**: In a VS Code dev container the extension reinstalls every start, skills show but don't fire, and login resets.

**Root cause**: Container filesystem resets while Claude Code state lives in TWO places (~/.claude and ~/.claude.json); a stale plugin cache also delays skill loading ~30s after start.

**Fix steps**:
1. Mount persistent named volumes for BOTH ~/.claude and ~/.claude.json (per Anthropic's devcontainer guide), plus ~/.vscode-server for extensions.
2. If skills show but don't fire, wait ~30s then /reload-plugins or restart the session.
3. Keep dev-only files out of the deployed repo: .gitignore .devcontainer/, .vscode/, dev docker-compose files and .env.dev, and use a separate production Dockerfile or a multi-stage build that copies only what production needs.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/how-are-you-guys-using-dev-environments

<!-- pattern: /how-are-you-guys-using-dev-environments -->

---

## Claude Code keeps asking YOU to check n8n — give it eyes via the n8n MCP/API
**Symptom**: Student feels like Claude Code's assistant, relaying execution logs and screenshots by hand while debugging n8n workflows.

**Root cause**: Claude Code has no access to the system it's debugging, so it uses the human as its eyes.

**Fix steps**:
1. Give Claude the eyes it is missing: with the n8n MCP connected (or n8n API access), tell it explicitly 'use the n8n MCP tools to inspect the most recent execution and find where it failed; if MCP is unavailable, use the API'. If your n8n is on a VPS, tell Claude how to reach it (e.g. over SSH) so it can fetch the logs itself.
2. Cheaper alternative when you don't want it crawling the whole workflow JSON: open the failed node, copy its execution JSON and paste it into the chat — that is often faster and fewer tokens than having Claude read the execution itself.
3. To stop repeating the instruction every session, have Claude Code add a line to CLAUDE.md making 'check the n8n execution log via MCP yourself before asking me' the default.
4. General habit from the support team's answer: whenever Claude asks you to do something manually, ask 'can you do this yourself?' and then 'is there a CLI or MCP you could be given so you can handle this on your own?'.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/im-claude-codes-assistant

<!-- pattern: /im-claude-codes-assistant -->

---

## Talking to your agent instead of typing — Wispr Flow and alternatives
**Symptom**: Students see Nate Herk speaking to Claude Code in videos and can't find the tool in the classroom.

**Root cause**: It's third-party system-wide dictation, not a Claude Code feature.

**Fix steps**:
1. Install Wispr Flow — system-wide dictation: hit a hotkey, talk, and clean text is pasted wherever your cursor is. This is the tool identified in-thread as what Nate Herk uses in his videos.
2. Alternatives: SuperWhisper (on-device, Mac) or a free DIY stack (whisper.cpp + Ollama + Hammerspoon).

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/talking-to-your-agent-instead-of-typing

<!-- pattern: /talking-to-your-agent-instead-of-typing -->

---
## CLAUDE PRODUCT LINE — Cowork, Design, Chrome

## Claude Code vs Cowork vs Chat — which tool for which job
**Symptom**: Students shuttle code between Claude Chat and Cowork by hand, or ask which is 'more powerful' as an executive assistant, or map elaborate n8n pipelines for on-demand office work.

**Root cause**: Product-line confusion: Claude Code is the file/terminal dev agent; Cowork is the desktop assistant for apps/files/browser; Chat is conversational only.

**Fix steps**:
1. Building software or automations: open Claude Code in the project folder with a CLAUDE.md — it reads the whole directory, edits across files, runs things and sees the errors, which removes the describe-it/copy-the-code-back/paste-it-in loop. For design, give it reference screenshots of layouts you like and say what appeals to you about them rather than describing a visual from scratch.
2. EA and desktop work (Excel, decks, PDFs, folders, web portals with no API): Cowork — it drives apps, files and the browser on your machine. Save repeatable instruction sets as Cowork skills or routines.
3. Chat is conversational only: use it to ideate and architect, but stop using it as the middleman that hands you code to paste into files by hand.
4. Deciding between Cowork and an n8n pipeline for messy multi-file office work — questions raised by a community member in-thread: does anything need to happen while you're away from your computer? what volume of changes per week? are several people editing in parallel? where do the files live? does a human review every update anyway? Low volume with mandatory human review favours Cowork; high volume or parallel editors push toward a shared source of truth and a pipeline.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/claude-code-vs-cowork-executive-assistant
- https://www.skool.com/ai-automation-society-plus/chat-vs-cowork-vs-code
- https://www.skool.com/ai-automation-society-plus/first-client-acdb92c3

<!-- pattern: /claude-code-vs-cowork-executive-assistant -->

---

## Cowork sessions can't be deleted — archive only; email Anthropic for hard deletion
**Symptom**: No delete button for Cowork sessions/tasks anywhere.

**Root cause**: Cowork's UI only offers Archive; deletion is handled by Anthropic on request.

**Fix steps**:
1. There is no delete button — archive is the only in-app option for cleanup. For actual deletion of session data, email privacy@anthropic.com and request a hard deletion.
2. Scheduled tasks are different from sessions: those CAN be paused or deleted from Cowork settings.
3. A local-cleanup workaround (deleting the session folder under .claude/projects plus the matching Cowork logs) was raised in-thread by a community member from an external blog post — it was not tested or endorsed by the support team, so treat it as unverified.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/cowork

<!-- pattern: /cowork -->

---

## Cowork skills fail with proxy errors on external sites — sandbox allowlist
**Symptom**: A Cowork skill fetching an external URL (e.g. YouTube) errors with a proxy/network error even though the same task worked in plain chat.

**Root cause**: Cowork skills run in a network-restricted sandbox ('bubblewrap') with a strict proxy allowlist; plain chat uses the normal web-search tool outside the sandbox.

**Fix steps**:
1. No supported bypass exists. Run the task conversationally in Cowork chat instead of as a skill, or have an n8n workflow fetch the content (e.g. YouTube transcript) to a local file the skill can read.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-cowork-skill-proxy-error-towards-youtube

<!-- pattern: /claude-cowork-skill-proxy-error-towards-youtube -->

---

## Claude Design ↔ Claude Code — no shared state, handoff gaps, GitHub read-only, tweaks reverting
**Symptom**: Design work (especially mobile/responsive) doesn't carry into Claude Code; Design refuses to push to GitHub; tweaks revert on reload.

**Root cause**: Claude Design and Claude Code are separate products with no shared state — Claude Code cannot see a Design session. The Handoff export can also omit responsive CSS/breakpoint classes even though Design's mobile preview looks right, and Claude Design can only READ from GitHub, never commit or push.

**Fix steps**:
1. Never tell Claude Code 'as I already did in Claude Design' — it cannot see that session. Carry the work across as files: use the 'Handoff to Claude Code' export (a tar bundle with the design files, tokens, component structure and a README) or export standalone HTML, drop it in the project folder, and point Claude Code at the file.
2. If mobile styles are still missing after the handoff, that is a known export gap, not your mistake. Two things worked in the thread: give Claude Code explicit breakpoint instructions (e.g. nav collapses to a burger below 768px, hero text scales down, sections stack on tablet and below, CTA full width on mobile), or paste screenshots of the intended layout — the student reported the screenshot route 'worked perfectly'.
3. Deploying: Claude Design cannot push to GitHub. Export the code, push it to a repo once yourself, connect that repo to Vercel, and from then on make every edit in Claude Code (terminal or VS Code extension) so Vercel auto-deploys. An externally-bought domain is pointed at Vercel via the DNS records Vercel gives you.
4. If tweaks appear to revert on reload: try a hard refresh first (Ctrl+F5, or Cmd+Shift+R on Mac) in case it is a cache issue; what actually resolved it for the student was telling Claude Design to make those tweaks the default.

**Confidence**: medium — community-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/claude-design-mobile-optimisation-having-issues
- https://www.skool.com/ai-automation-society-plus/claude-design-website-question
- https://www.skool.com/ai-automation-society-plus/how-to-save-tweaks-on-claude-design

<!-- pattern: /claude-design-mobile-optimisation-having-issues -->

---

## Claude in Chrome crashing the browser — conflicting blocker software
**Symptom**: The Claude in Chrome extension repeatedly crashes, sometimes taking all browser windows down.

**Root cause**: Locally-installed app/site blockers (e.g. Cold Turkey Blocker) conflict with the extension; whole-browser crashes indicate a local software conflict.

**Fix steps**:
1. Start by clearing cache and cookies and retrying in an incognito window (extensions off) to isolate whether an extension is involved.
2. If the ENTIRE browser goes down, not just the extension, that points at a local software conflict rather than the extension itself. Uninstall or disable app/site-blocker and monitor software — removing Cold Turkey Blocker is what fixed the confirmed case in this thread.
3. Separate known limit reported by a community member in the same thread: a single wait step longer than about 10 seconds errors out and the extension loses its connection. Break long waits into ~10-second chunks and check page state in between.

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-in-chrome-crashing

<!-- pattern: /claude-in-chrome-crashing -->

---
