# Fix Patterns — Claude Code — install, extension, outages & sync

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

See also `diagnostics/claude-code-install.md` and `diagnostics/vscode-extension-broken.md`, which are the routed entry points for install and extension problems.

---

## CLAUDE CODE — Install & PATH

## `claude: command not found` on Mac/Linux
**Symptom**: Student types `claude` in terminal, gets `command not found: claude`. They think Claude is installed because the Claude.ai desktop app and the VS Code extension are visible — but those are separate from the Claude Code CLI.

**Root cause**: They never installed the Claude Code CLI binary. The desktop app and the Claude.ai web UI are not Claude Code. Or it's installed but `$HOME/.local/bin` isn't in PATH.

**Fix steps**:
1. In a regular terminal (not inside Claude chat), run the official native installer:
   - Mac/Linux: `curl -fsSL https://claude.ai/install.sh | bash`
2. Add the install location to PATH:
   ```
   echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc
   ```
3. **Close the terminal window entirely and open a brand new one.** PATH won't refresh in the existing session.
4. Verify: `claude --version` — should return a version, not "command not found".
5. If still failing, manually add `$HOME/.local/bin` to PATH via your shell's RC file.

**Lesson reference**: Claude Code → Phase 1 → 1.1 INTRODUCTION

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/introduction-to-mcp-servers-in-cloud-code-problem (long community walkthrough — definitive)
- https://www.skool.com/ai-automation-society-plus/failed-to-setup-md-file

---

## `claude` command not found on Windows
**Symptom**: VS Code Claude Code extension errors with "command not found", or PowerShell rejects `claude`.

**Root cause**: Only the extension is installed, not the underlying CLI; or it's installed but not on PATH.

**Fix steps**:
1. In PowerShell, run: `irm https://claude.ai/install.ps1 | iex`
2. Close PowerShell completely and open a new window.
3. Run `claude --version`.
4. If still not found, manually add `%USERPROFILE%\.local\bin` to your User PATH via Environment Variables.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/failed-to-setup-md-file

---

## Windows install on a corporate machine without admin rights
**Symptom**: Standard Node.js installer wants admin password the student doesn't have.

**Root cause**: Node.js MSI installer needs admin.

**Fix steps**:
1. Use Scoop (no admin required) instead of fighting the MSI: `iwr -useb get.scoop.sh | iex`, then `scoop install nodejs-lts`.
2. Or ask Claude Code: "I'm on a corporate Windows machine, no admin access, need Node.js for the n8n MCP server. Walk me through a portable install that doesn't need admin." It will grab the binary from nodejs.org, set up user PATH, verify.
3. Confirm with `node --version`.

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/install-mcp-server-problem-with-nodejs
- https://www.skool.com/ai-automation-society-plus/how-to-sync-claude-work-across-2-laptops-no-admin-on-one

---
## CLAUDE CODE — VS Code Extension

## VS Code Claude Code extension stops loading after auto-update
**Symptom**: "Claude icon used to work, now clicking it shows an error in the bottom-right of VS Code", "Cannot activate extension". Variant error strings from the same regression class: the sidebar Claude icon disappears entirely, or the extension throws `command claude-vscode.editor.openLast not found`. Reinstalling does not help — reinstall pulls the same bad build.

**Root cause**: Anthropic ships VS Code extension updates very frequently; some versions break activation. This is recurring — happened with v2.1.51, v2.1.129, and versions after v2.1.77 (for the bypass-permissions bug). Each bad release produces a different error string, which is why the symptom looks new every time. Only downgrading to the last build that worked for that user clears it.

**Fix steps**:
1. Open VS Code Extensions panel.
2. Find Claude Code → click the gear icon next to Uninstall.
3. Click "Install Another Version".
   - The gear icon and a right-click on the extension both open the same menu. If **Install Another Version** does not appear at all, uninstall the extension, reload VS Code, reinstall it, then use the option.
4. Pick a known-good earlier version (most recent reliable as of corpus: 2.1.128, 2.1.49, 2.1.52, 2.1.77).
5. Reload window when prompted.
6. Click gear icon again → **uncheck Auto Update** so the broken version doesn't reinstall.
7. As a fallback, the terminal CLI (`claude` in a regular shell) is unaffected and you can keep working.
8. Re-enable auto-update once a working version ships (usually within 1-2 days).
- If the Claude icon vanished from the sidebar, that is the extension failing to activate rather than a separate bug. Once on a working version, restart VS Code and the icon returns.

**Confidence**: high — multiple independent threads converged on the same fix.

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/all-of-a-sudden-claude-code-extension-in-vs-code-doesnt-work
- https://www.skool.com/ai-automation-society-plus/cannot-use-the-claude
- https://www.skool.com/ai-automation-society-plus/claude-code-keeps-asking-permission-to-edit-files-even-though-settings-are-correct
- https://www.skool.com/ai-automation-society-plus/command-claude-vscodeeditoropenlast-not-found

---

## Bypass-permissions mode keeps asking permission anyway
**Symptom**: Bypass permissions is on, but Claude Code still asks for confirmation on every edit.

**Root cause**: Known Anthropic GitHub issue #36168 — bypass mode broken in versions newer than v2.1.77.

**Fix steps**:
1. Roll back the VS Code extension to v2.1.77 (see procedure above).
2. Disable auto-update.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-code-keeps-asking-permission-to-edit-files-even-though-settings-are-correct

---

## Bypass-permissions does NOT bypass PowerShell on Windows
**Symptom**: Bypass mode works for file edits but PowerShell commands still need explicit confirmation.

**Root cause**: Windows blocks unsigned scripts by default. Separate from Claude Code's permission system.

**Fix steps**:
1. Open a **normal** PowerShell window — `-Scope CurrentUser` does not require Administrator, and installing from an elevated window puts the PATH entry on the wrong profile, which later shows up as `claude: command not found`.
2. Run: `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`
3. Type `Y` to confirm. ⚠️ On a work-managed laptop, ask IT before changing execution policy — it may be set by device policy.

**The enforcement distinction that matters**: students frequently write "never delete files" into CLAUDE.md as a bypass-mode safety net. CLAUDE.md is advisory — it does not enforce anything. `permissions.deny` rules in `settings.json` are evaluated BEFORE permission modes and hard-block matching commands even in bypass mode. These are two separate layers: the ExecutionPolicy fix below addresses Windows refusing to run scripts, the deny list addresses what Claude Code is allowed to attempt at all.

**However**: Prefer the community-verified approach — keep bypass-permissions mode but add a deny list in `~/.claude/settings.json` for destructive verbs:
```json
{
  "permissions": {
    "deny": [
      "Bash(Remove-Item:*)",
      "Bash(rm:*)",
      "Bash(rmdir:*)",
      "Bash(del:*)",
      "Bash(Clear-Content:*)",
      "Bash(git push:*)",
      "Bash(git reset --hard:*)",
      "Bash(git rebase:*)"
    ]
  }
}
```
Deny rules are evaluated BEFORE permission modes, so they hard-block even in bypass mode.

**Confidence**: high — community-verified (the student confirmed the deny-list guardrail working)

**Example thread**: https://www.skool.com/ai-automation-society-plus/bypass-permission-does-not-bypass-powershell

---

## False 'Rate limit reached' in the VS Code extension on a Pro/Max subscription
**Symptom**: 'API Error: Rate limit reached' / 'API limit reached' only inside VS Code while the same account works in the terminal CLI; upgrading the plan doesn't help.

**Root cause**: Known extension bug: it silently resets subscription info in its credentials file and treats you as free/API tier; a leftover ANTHROPIC_API_KEY env var can also override subscription login onto API billing.

**Fix steps**:
1. From a system terminal run claude /logout then claude /login with the subscription account; verify with /status (should show your plan).
2. Check echo $ANTHROPIC_API_KEY (PowerShell: $env:ANTHROPIC_API_KEY); if set and unexplained, unset it and remove the export from your shell config — subscription login doesn't need it.
3. If it persists (known upstream bug), the community workaround is re-clicking the model in the picker at session start even if it looks selected, and keeping the extension updated (or rolling back one version).

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/api-error-rate-limit-reached
- https://www.skool.com/ai-automation-society-plus/api-limit-reached-claude-code

<!-- pattern: /api-error-rate-limit-reached -->

---

## VS Code extension hangs 'not responding' mid-response
**Symptom**: A response stalls indefinitely; sometimes even the stop button is stuck.

**Root cause**: Known extension bug, not user-caused.

**Fix steps**:
1. Stop and retry the prompt.
2. If the stop button is stuck: Ctrl+Shift+P > 'Developer: Reload Window'.
3. If constant, run Claude Code from the terminal CLI instead of the extension.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/no-responding

<!-- pattern: /no-responding -->

---

## 'Error while loading view: claudeVSCodePanel' after a folder rename
**Symptom**: The Claude Code chat panel errors in every chat; reloading VS Code doesn't help.

**Root cause**: The project folder was renamed/moved while Claude Code was open, so the extension's webview lost its workspace path.

**Fix steps**:
1. Try Ctrl+Shift+P > 'Developer: Reload Webviews'.
2. If you renamed/moved the folder: File > Open Folder at the new path, then start a chat — the panel recovers with a valid workspace path.

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/vs-code-error-while-loading-view

<!-- pattern: /vs-code-error-while-loading-view -->

---

## Past sessions vanish after restart — restore settings and the mapped-network-drive bug
**Symptom**: All previous VS Code / Claude Code sessions disappear after a hard shutdown; new history stops being stored.

**Root cause**: Hard shutdowns can corrupt VS Code session state; separately, a reported Claude Code bug loses session storage when a project lives on a mapped network drive whose mapping/IP changes (anthropics/claude-code issue #34125).

**Fix steps**:
1. Reopen the actual project folder (File > Open Folder), not a blank window.
2. Settings: set 'restore windows' to all; ensure 'startup editor' isn't none.
3. If projects live on a NAS/mapped drive: remap after IP changes, expect history lost this way to be unrecoverable, and prefer local drives for active projects.
4. Prevention: have Claude Code persist important context to md files before shutdowns.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/vs-code-not-storing-past-sessions-any-longer

<!-- pattern: /vs-code-not-storing-past-sessions-any-longer -->

---
## CLAUDE CODE — Errors & Anthropic outages

## Random API errors everywhere
**Symptom**: Claude.ai, Claude Code, everything erroring at once. Variant: the Claude desktop app download or installer fails for days on end.

**Root cause**: Usually an Anthropic incident.

**Fix steps**:
1. **Check `https://status.anthropic.com` / `https://status.claude.com` before debugging your own setup.** An Anthropic outage breaks API calls *and* the download/install flow at the same time — students routinely reinstall for hours against an outage.
2. Check https://status.claude.com (formerly status.anthropic.com).
3. Wait it out. Usually back in 5-30 minutes.
4. If sustained > 1 hour, try the Bedrock route (if you have access) by setting `ANTHROPIC_BEDROCK_BASE_URL`.
- **Download the desktop app only from `claude.com/download`** (Windows 10+ 64-bit required). It is not on the Microsoft Store. During an outage, just wait and retry.

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/what-do-you-do-when-claude-code-gives-you-error-message
- https://www.skool.com/ai-automation-society-plus/struggling-to-download-claude-on-desktop

---
## CLAUDE CODE — Regression Spiral

## Claude keeps breaking previously-working features
**Symptom**: "I fix feature A, it breaks feature B; I fix B, A breaks again. Hours of looping."

**Root cause**: No tests + no scope control + cleaning in place instead of greenfield. Claude is trained to be non-destructive so it wraps + preserves rather than deleting cleanly.

**Fix steps**:
1. **Write a tiny test the moment a fix works** — even one smoke test per fragile feature.
2. **Commit to git at every working point** — Esc/Esc or `/rewind` only tracks Claude's own edits (not bash-driven file changes). Git is the real net.
3. **Scope the fix in the prompt**: "Change only X. Don't touch or refactor the working code. Make the smallest change that fixes it." Use Plan mode (Shift+Tab) to gate the blast radius before edits.
4. **Add a Stop hook** (in `.claude/settings.json` or `~/.claude/settings.json`) to auto-run tests after every turn:
   ```json
   {
     "hooks": {
       "Stop": [{ "matcher": "", "hooks": [{ "type": "command", "command": "npm test" }] }]
     }
   }
   ```
   (Use `Stop`, not `PostToolUse` — `PostToolUse` fires after every edit and bogs the session down on bigger projects.)
5. **When refactoring, regen from spec into a clean tree** — don't refactor in place. Use a fresh git worktree, treat the spec as source of truth.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-code-best-practices

---
## CLAUDE CODE — Cross-machine sync

## Pick up Claude Code session on another laptop
**Symptom**: Started a project on PC, want to continue on laptop.

**Root cause**: Claude Code stores session history locally per machine. The repo travels but not the chat.

**Fix steps**:
1. **Sync code through GitHub, not through a cloud-sync folder.** Run `gh auth login` once in your terminal, then ask Claude Code to initialise a git repo, create a `.gitignore`, create a private GitHub repo and push. After that: commit and push before you stop on one machine, pull when you sit down at the other.

   **The risk with OneDrive/Google Drive is specific, not universal.** Small text files like CLAUDE.md and skills sync fine. What breaks is `.git` folders and `node_modules`, which thrash the sync process and can corrupt the repo. If you keep projects in a synced folder anyway, exclude `.git` and `node_modules` from sync.

   **Files On-Demand gotcha**: set any project folder you work in to **"Always keep on this device"**. If OneDrive marks a file online-only, Claude Code cannot read it and you get errors. On Windows the Desktop folder lives inside OneDrive by default, which is why some videos look like they are using OneDrive as a deliberate strategy when they are not.
2. Push the project to a private GitHub repo.
3. Before stopping on machine A: `git add . && git commit -m "wip" && git push`
4. On machine B: `git clone <repo>` then `git pull` when resuming.
5. `.env` files don't sync (gitignored) — keep them in a password manager (Roboform, 1Password, Doppler) and pull onto the new machine.
6. **For session continuity**, ask Claude before stopping: "Write a .claude-handoff.md with current state, next steps, open questions" — commit it; the new session reads it for context.
7. **Or use Claude Code Web** at `code.claude.com` — sessions run in Anthropic-managed sandboxes, no machine sync needed.
- **To move chat history**: copy the `.claude` folder (or just `.claude/projects/`) to the other machine. The project subfolder name is path-encoded from your username and project path — for example a folder ending `-Users-<old-username>-<project>` must be renamed to `-Users-<new-username>-<project>` using the new machine's actual username (`<old-username>` is the username on the machine you copied from, `<project>` is your project folder name; both are visible in the existing folder name). Then open it with `claude -c` for the last chat or `claude -r` to pick a specific one.
- **On a no-admin work laptop**, options raised by the community but *not* confirmed in-thread: Scoop installs tools (git, GitHub CLI, node) into your user folder without admin prompts; VS Code Settings Sync carries editor config through your GitHub or Microsoft account; a directory junction (`mklink /J`) can redirect a fixed `C:` path without admin where a symlink would need it. Treat these as leads to test, not a verified recipe.

**Lesson reference**: `Claude Code → Phase 3 → 1.3 GitHub` (teaches what GitHub is, why developers use it, account + private repo setup from scratch — point new students here when they're stuck on the git side).

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/claude-code-session-from-pc-and-picking-up-where-i-left-off-on-my-laptop
- https://www.skool.com/ai-automation-society-plus/copy-claude-code-chat-in-vs-code-to-another-computer
- https://www.skool.com/ai-automation-society-plus/how-to-sync-claude-work-across-2-laptops-no-admin-on-one
- https://www.skool.com/ai-automation-society-plus/working-locally-vs-in-the-cloud-with-claude-code-in-ides

---

## Transfer the entire `.claude/` folder between machines
**Symptom**: "I have all my custom skills, agents, settings on machine A — want them on machine B."

**Root cause**: `.claude/projects/<path>` contains session history. The path includes the username, which differs across machines.

**Fix steps**:
1. Copy the whole `.claude` folder (hidden, in home directory) from machine A to machine B.
2. **Rename the project subfolder** so its username portion matches the new machine. Example: `Users/laptopname/myproject` → `Users/othername/myproject`.
3. Open the project in the same path on machine B.
4. Run `claude -c` for last chat, or `claude -r` to pick a specific one.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/copy-claude-code-chat-in-vs-code-to-another-computer

---

## N8N + MCP — Setup

## n8n MCP setup creates massive folder vs the video shows tiny one
**Symptom**: "Cloned the repo and got 100x more files than the video"; "Multiple .env files". Also: Source Control floods with hundreds of pending changes, and — the part that actually blocks people — even with the API key and instance URL correctly in `.env`, Claude Code replies that it has no connection to your n8n instance.

**Root cause**: The folder size and multiple `.env` files are NORMAL if Claude went the clone route. The student is on the right path — they just need to FINISH whatever install Claude started. The most common reason they're stuck is that they rejected `npm install` when Claude Code asked (often confused by Kodi's warning about `npx`, which is a different command). Handing Claude Code the repo URL reads as "download and build this project", so it clones the full source. Separately and more importantly: **Claude Code does not read `.env` files when configuring an MCP server.** The credentials have to be supplied at the moment the server is registered, so a perfectly correct `.env` leaves the MCP server unregistered and invisible.

**What lesson 1.4 actually teaches**: Nate's lesson has you ask Claude Code to install n8n-mcp at the **project level**, configure credentials via `.env`, and verify with `/mcp` + a health check. The lesson video walks through the install but doesn't lock you to one specific command path — Claude may pick `npm install` inside a cloned repo, `claude mcp add`, or another command. The two things lesson 1.4 IS strict about:
1. **Configure at project level**, not user-level (avoids common security warnings).
2. **Credentials go in `.env`**, never in chat.

**Fix steps**:
1. **Cleanest path, no clone.** Tell Claude Code exactly this — "add the n8n-mcp server as an MCP connection using the npx n8n-mcp package, do not clone or download anything". That wording leaves nothing to misread. It will prompt you for your n8n instance URL and API key in chat and write the config itself.
2. **Alternative — register it directly from a normal terminal** (not inside Claude Code):
   ```
   claude mcp add n8n-mcp -e MCP_MODE=stdio -e LOG_LEVEL=error -e DISABLE_CONSOLE_OUTPUT=true -e N8N_API_URL=<your n8n URL> -e N8N_API_KEY=<your n8n API key> -- npx n8n-mcp
   ```
   ⚠️ `<your n8n URL>` is the address you type in your browser to open n8n — ask the student for it if you don't already have it. `<your n8n API key>` is generated inside n8n at Settings → n8n API → Create API Key. Never show this command with the angle brackets left in.
3. **Two different meanings of "npx" — keep them straight, because the lesson's warning only covers one of them.**
   - **Registering the MCP server so it launches via the npx package** (`claude mcp add ... -- npx n8n-mcp`, step 2 above) is the route the support team recommends in the 2026-07 threads. It is what avoids the clone entirely. This is fine.
   - **Running `npx <package>` ad-hoc as the install command** — Claude proposing "let me just npx this" instead of doing a proper install — is what the lesson cautions against. Decline that.
   - `npm install` and `npm run build` are a third, different thing: they populate a cloned repo's dependencies. **Approve those** if a clone already happened.
   - If a student has heard a flat "don't use npx" and is therefore refusing the team's own recommended registration command, this distinction is the thing to explain.
4. About "multiple .env files" — `.env.example` and `.env.docker` are templates, not active config. Only the plain `.env` matters. Have Claude copy `.env.example` → `.env` and fill in the values. Anything with a suffix (`.env.example`, `.env.docker`) is a blank template for a different hosting method — only the file named exactly `.env` matters. Ask Claude Code to locate or create it for you.
5. Inside the `.env` file (NOT in chat), set:
   - `N8N_API_URL=https://your-n8n-instance.com` (base URL only — see gotchas)
   - `N8N_API_KEY=your-api-key` (from n8n Settings → n8n API → Create an API key — the menu is "n8n API", not "API")
   - ⚠️ Both values above are placeholders — when giving this step to a student, swap in their real n8n URL (the exact address they type in their browser to open n8n) if known, or ask for it; never present `your-n8n-instance.com` / `your-api-key` bare, because beginners paste them literally.
6. Fully quit Claude Code (close all VS Code windows; on CLI exit session), then reopen the project. Approve the "Allow this project's MCP servers?" prompt.
7. Run `/mcp` inside Claude Code → `n8n-mcp` should show as connected.
8. Test (this is the "Health Check Confirmation" chapter of the lesson): ask Claude in chat "List my n8n workflows." Real workflows back = connection is real.
- **Source Control noise is not your work.** The hundreds of pending changes after a clone are the repo's own files. Do not commit them. If you switch to the npx route you can delete the cloned folder entirely.
- **Docker is a valid alternative** if you already run Docker locally: register the MCP server as a `docker run` against the maintainer's published image. Tradeoffs the team flagged — Docker Desktop must be running whenever you start a Claude Code session, and the image will not update itself, where npx pulls the latest each run.

**If stuck mid-install**: walk through the actual symptom (the exact error or step they're on) rather than starting from scratch. The Pre-Flight Setup Checklist PDF attached to lesson 1.4 covers the prerequisites (Node.js install, n8n API key generation, Homebrew on Mac) — point them there if they haven't read it.

**On the npx registration route vs the video**: `claude mcp add ... -- npx n8n-mcp` is not what the lesson video walks through (the video goes through a clone), but it is what the support team recommends in the 2026-07 threads and it is the cleanest path — see fix step 1. Be honest about that provenance when you offer it: "not what the video shows, but what the team recommends now." This is distinct from Claude proposing an ad-hoc `npx` as its install command, which is what the lesson cautions against.

**Important gotchas**:
- **`N8N_API_URL` is the base only** — paste the root URL you browse to, with no `/api/v1`. The MCP appends the API path itself. (Recent n8n-mcp versions normalise trailing slashes and detect an already-present `/api/v1`, so they won't double the path — don't assume a URL-format typo is the cause of a 404 without checking the version.)
- For browser URL `https://yourname.app.n8n.cloud/api/v1`, use `https://yourname.app.n8n.cloud`.
- **Project-level scope matters**: lesson 1.4 explicitly configures the MCP at the project level. If a student installed user-scope, `/mcp` may not show it where expected.
- If you denied the project-MCP prompt previously, edit `~/.claude.json` (or `%USERPROFILE%\.claude.json` on Windows), find your project's path entry, remove `n8n-mcp` from the disabled servers list to get re-prompted.

**Lesson reference**: Claude Code → Phase 1 → 1.4 n8n MCP Server (lesson has a "Pre-Flight Setup Checklist" PDF attached — covers Node.js install, Homebrew on Mac, n8n account + API key, and "watch out for this" moments. Point students there if they haven't read it).

**Confidence**: high — multiple solved threads converge.

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/connect-n8n-mcp-to-claude-code-issue
- https://www.skool.com/ai-automation-society-plus/setting-n8n-mcp-server-up-with-claude-code
- https://www.skool.com/ai-automation-society-plus/n8n-mcp-connection
- https://www.skool.com/ai-automation-society-plus/trouble-with-setting-up-n8n-mcp-server-in-vs-claude-code
- https://www.skool.com/ai-automation-society-plus/mpc-server-set-up
- https://www.skool.com/ai-automation-society-plus/help-with-section-14-n8n-mcp-server
- https://www.skool.com/ai-automation-society-plus/setup-first-n8n-workflow

---

## "Two different n8n MCPs" confusion
**Symptom**: Student asks why their official n8n MCP doesn't let Claude build workflows.

**Root cause**: There are two distinct tools both called "n8n MCP":
- **Official n8n instance-level MCP** (built into n8n itself, Settings → Instance-level MCP) — exposes existing workflows TO Claude for execution. Use this for Claude Desktop.
- **Community czlonkowski/n8n-mcp** — runs separately, gives Claude Code knowledge of every n8n node + validation tools so it can BUILD workflows.

**Fix steps**:
1. For the AIS+ course → install czlonkowski/n8n-mcp (the BUILD one).
2. For just exposing n8n workflows to Claude Desktop chat → use the n8n native instance-level MCP.

**Lesson reference**: `Claude Code → Phase 2 → 1.5 Introduction to MCP Servers in Cloud Code` covers what MCP servers are, how to find them, and how to connect them to Claude Code — good background for students who don't yet have the MCP concept.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/n8n-mcp-connection

---

## n8n free trial has no "API" menu — can't get API key
**Symptom**: "Kodi says go to Settings → n8n API but it doesn't exist."

**Root cause**: API key generation requires a paid Cloud plan OR self-hosted instance. Free trial doesn't surface it. New UI may put it under Settings → Instance-level MCP → Connection details. Note the common counter-claim: some community advice says no payment is needed at all. That is true only in the sense that self-hosting is free — the free Cloud trial genuinely cannot produce a key.

**Fix steps**:
1. Go to `Settings → Instance-level MCP → Connection details` — this is where the URL and API key live in the new UI.
2. **Option A — upgrade to a paid n8n Cloud plan.** The API menu then appears under Settings and you can create a key.
3. **Option B — self-host free.** The Community Edition is free and runs locally on your own machine, which is enough to get your first workflows built and connected to Claude Code at no cost. Tradeoff to state up front: your machine has to be powered on for any workflow to run, and performance depends on your hardware. Good starting point, not a long-term or client-facing solution.
4. **Option C — an always-on self-hosted instance on a low-cost VPS.** Hostinger offers a one-click n8n template; several members in these threads moved to it as a cheaper alternative to n8n Cloud.
5. The team's recommendation for anyone intending to keep going: move to a paid plan or a properly hosted instance sooner rather than later — you will need it eventually and switching later costs time.
6. Once you have an instance, generate the key at **Settings → n8n API → Create API Key**.
7. If learning only, skip section 1.4 and continue to section 2 (other students have done this without issue).

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/not-seeing-the-n8n-the-menu-bar
- https://www.skool.com/ai-automation-society-plus/14-n8n-mcp-server-cant-find-api-n8n-key
- https://www.skool.com/ai-automation-society-plus/n8n-api
- https://www.skool.com/ai-automation-society-plus/n8n-subscription-2

---

## n8n MCP on Hostinger — get the URL right
**Symptom**: "I'm on Hostinger but the field asks for n8n cloud URL — what goes there?" Sharper variant of the same failure: the MCP server shows as connected and the **Documentation tools work, but the Management tools fail**.

**Root cause**: The field is just asking for whatever URL you use to open n8n in your browser.

**Fix steps**:
1. In .env, set `N8N_API_URL=` to the student's Hostinger n8n URL — the exact address they type in the browser to open n8n (e.g. theirs might look like `https://n8n.yourdomain.com`) — no trailing slash, no /api/v1. `n8n.yourdomain.com` is a placeholder; use their real URL or ask for it, never present the placeholder bare.

**Important gotchas**:
- **Use the Documentation-vs-Management split as your diagnostic.** Documentation tools serve static reference data (node schemas, community templates) and work regardless of your instance. Management tools make live REST calls against your instance. If Documentation tools work and Management tools fail, the problem is the URL — not the API key.
- **Give `N8N_API_URL` as the bare root** (no `/api/v1`, no trailing slash) — that's the form the MCP expects. Two students in the 2026-07 threads fixed a 404 by correcting this field. Note current n8n-mcp versions strip trailing slashes and won't double `/api/v1`, so if the URL already looks right, check the installed version and the open issues at github.com/czlonkowski/n8n-mcp/issues rather than re-editing the URL.

2. In n8n, go to Settings → n8n API → Create an API key (the menu is "n8n API", not "API"). Copy into `N8N_API_KEY` in `.env`.
3. Install MCP via the canonical clone path (see "n8n MCP setup creates massive folder" above). Hostinger n8n is just a different URL — the install procedure is the same.
4. **Restart Claude Code** to pick up the .env.
5. `/mcp` should show n8n-mcp as connected.
6. **If `/mcp` shows connected but you get a 404 / "resource not found" / "can't find workflows"** — known Hostinger/self-hosted quirk with two verified fixes, in order:
   - **Base-URL path mismatch** (team-verified): the API path must appear exactly once. czlonkowski n8n-mcp wants the bare root in `N8N_API_URL` and appends `/api/v1` itself; other tools (e.g. the n8n node's own API credential) need `/api/v1` written out explicitly. A missing OR doubled `/api/v1` both produce a 404. If you're 404ing, toggle the URL to the other form and retry. Concrete forms: for a self-hosted instance (for example on Hostinger), `N8N_API_URL` is the address you use to open n8n in your browser plus `/api/v1` — the exact host is whatever your browser shows when n8n is open; ask the student for it rather than guessing. For n8n Cloud the instance URL is the plain workspace address with no path (the form the lesson demonstrates); if Management tools still fail on that, append `/api/v1`.
   - **API key generated in the wrong place** (community-verified): the key must come from inside n8n itself (Settings → n8n API → Create API Key) — NOT from the Hostinger panel. Create a fresh key with no expiration date and the default scopes, update `.env`, restart.
7. Still stuck after both → draft a Support Needed‼️ post (Escape Hatch B); the support team maintains workaround knowledge for Hostinger edge cases.
- **Before blaming the URL, confirm Node.js is installed** — run `node --version` in a terminal. One student in these threads spent multiple attempts on URL variations when the real blocker was that Node.js was never installed, so the MCP server could not run at all.

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/using-n8n-via-hostinger-connection-issue
- https://www.skool.com/ai-automation-society-plus/need-help-connecting-n8n-mcp-server-on-hostinger
- https://www.skool.com/ai-automation-society-plus/connecting-n8n-workflow-to-hostinger-instance-to-query-workflows
- https://www.skool.com/ai-automation-society-plus/phs-11-to-16
- https://www.skool.com/ai-automation-society-plus/unable-to-start-mcp-server-tutorial-14-n8n-mcp-server

---

## Self-hosted n8n + Claude Desktop "Some MCP servers could not be loaded"
**Symptom**: Putting `"type": "http"` in `claude_desktop_config.json` for remote n8n MCP, Claude Desktop says invalid config.

**Root cause**: Claude Desktop doesn't fully support remote HTTP/SSE servers via config — `"type": "http"` is rejected. Separately: **any security layer you placed in front of your own instance** — Cloudflare Zero Trust, a WAF, basic auth — will block the bridge, because the bridge is not sending the credential that layer demands. This is the failure mode people forget they created.

**Fix steps**:
1. **In n8n, go to Settings → Instance-level MCP and enable MCP access.** This generates your access token and shows your MCP server URL — copy both now, because the token is only shown in full once (it can be rotated from the same page if you miss it). These two values are what fill the placeholders in the config below.
2. Use `mcp-remote` as a bridge. Replace your config with:
   ```json
   {
     "mcpServers": {
       "n8n": {
         "command": "npx",
         "args": [
           "mcp-remote",
           "https://your-n8n-domain/mcp-server/http",
           "--header",
           "Authorization: Bearer YOUR_MCP_TOKEN"
         ]
       }
     }
   }
   ```
   ⚠️ `https://your-n8n-domain/...` and `YOUR_MCP_TOKEN` are placeholders — substitute the student's real n8n domain and their real MCP token (n8n → Settings → Instance-level MCP → Connection details) before showing this config, or ask for them first.
3. Config file location:
   - Mac: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`
4. Node.js must be installed (for npx).
5. **Fully quit Claude Desktop** (not just close window) and reopen.
6. On Hostinger / behind a reverse proxy, add to nginx config for the MCP path:
   ```
   location /mcp/ {
     proxy_http_version 1.1;
     proxy_buffering off;
     proxy_cache off;
     gzip off;
     chunked_transfer_encoding on;
     proxy_read_timeout 86400s;
     proxy_set_header Connection '';
   }
   ```
   (`proxy_buffering off`, `proxy_cache off`, and `gzip off` are the critical ones — without them nginx buffers the stream and the connection fails silently. Keep `chunked_transfer_encoding on`: turning it **off** makes nginx buffer the whole response to compute a Content-Length, re-creating the problem.)
7. If you're behind Cloudflare Zero Trust, set up a Service Token in CF and add it as an additional auth header.
- **If it connects but still fails, check for security layers you added yourself.** In the confirmed case here the student had put a Cloudflare Zero Trust gateway in front of the n8n instance and forgotten about it — the fix was issuing a Zero Trust **service token** and attaching it as additional auth in the Claude Desktop configuration. The same applies to any WAF or basic-auth layer you control.
- **Verify end to end rather than trusting the "connected" indicator**: build a small test workflow and have Claude actually trigger it.
- **Rotate any credentials that were exposed during troubleshooting** once it works.

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/anyone-get-hostinger-self-hosted-n8n-working-with-desktop-claude-via-mcp
- https://www.skool.com/ai-automation-society-plus/need-help-connecting-n8n-mcp-server-on-hostinger

---

## "Claude wants to npm install — should I let it?"
**Symptom**: During the n8n-mcp install, Claude asks to run `npm install`. Student denies (thinking it's the `npx` Kodi warned about), then everything fails.

**Root cause**: A mishearing of the lesson audio. Students recall a warning about letting Claude run `npx` or `npm` and generalise it into declining `npm install`. `npm install` is not the thing to avoid — it fetches the dependency packages the MCP server needs in order to run at all, so declining it guarantees the setup fails.

**Fix steps**:
1. **Approve `npm install`** when prompted. (This is separate from the `npx` warning.)
- If you want to see what will be installed before approving, switch to **plan mode** first, read the plan, then accept. This is the right response to "I don't want to blindly approve installs" — not declining.
2. Also approve `npm run build` if asked.
3. If Node.js wasn't installed first, Claude will ask to install it — approve that too. On Mac, also ensure Homebrew is installed (`brew --version` to check).
4. **Decline an ad-hoc `npx <package>` if Claude offers it as its install command** — that's the one the lesson calls out. This does NOT mean refusing `claude mcp add ... -- npx n8n-mcp`, which is the team's recommended registration command and a different thing entirely (see "n8n MCP setup creates massive folder" for the full distinction).
- **Verify end to end** rather than trusting the install output: ask Claude Code to list your n8n workflows, then ask it to make an edit to one. If both work, the MCP server is genuinely connected.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/trouble-with-setting-up-n8n-mcp-server-in-vs-claude-code

---

## n8n MCP runs locally vs n8n instance in cloud confusion
**Symptom**: "I asked Claude where the MCP is, it said locally — but I thought it should be in the cloud."

**Root cause**: Students conflate the MCP server with the n8n instance.

**Fix steps**:
1. The MCP server is a **local helper** on your machine. It only runs while Claude Code is running. It's correct that it's local.
2. The **n8n instance** (where workflows actually live and execute) is what you'd want in the cloud or self-hosted if you need 24/7 uptime, webhooks, scheduled triggers.
3. For learning/portfolio, n8n local is fine. Only need cloud n8n when something needs to be always-on.

**Confidence**: high

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/n8n-mcp-server-building-portafolio
- https://www.skool.com/ai-automation-society-plus/mcp-server-2

---

## Smartlead MCP loading 60+ tools and burning context
**Symptom**: MCP loads many tools into every conversation, even when you're not using it.

**Root cause**: All MCP tool schemas get loaded at session start by default.

**Fix steps**:
1. Quickest: in Claude Desktop, click the `+` button → Connectors → toggle Smartlead off for general chats; turn on only in outreach sessions.
2. For per-tool control: inside the Smartlead connector settings, untick individual tools you never use (painful for 100+ tools).
3. For 100+ tools: switch the MCP into "On demand access" mode if it supports it.
4. In Claude Code: `claude mcp disable smartlead`, then `/context` to confirm token drop.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/quick-question-smartlead-claude-mcp

---

## Claude Code doesn't read .env for MCP config — register with claude mcp add -e
**Symptom**: Claude Code says 'I don't have a direct connection to your n8n instance' even though N8N_API_URL and N8N_API_KEY are sitting in the project's .env file.

**Root cause**: Claude Code never reads .env for MCP server configuration. MCP credentials have to be passed at the moment you register the server; until you do, Claude Code doesn't know the server exists at all.

**Fix steps**:
1. From a regular terminal (not inside Claude Code), run: claude mcp add n8n-mcp -e MCP_MODE=stdio -e LOG_LEVEL=error -e DISABLE_CONSOLE_OUTPUT=true -e N8N_API_URL=<your-n8n-instance-url> -e N8N_API_KEY=<your-n8n-api-key> -- npx n8n-mcp
2. Replace <your-n8n-instance-url> with the base URL you see in your browser's address bar for n8n — on n8n Cloud that is https://<yourname>.app.n8n.cloud with NO trailing slash and no /api/v1 path appended (the client adds the API path itself). Replace <your-n8n-api-key> with a key generated in n8n under Settings > n8n API.
3. Restart Claude Code, then verify with /mcp inside a conversation or claude mcp list from the terminal.
4. Community-reported gotcha for project-scoped installs: the first time Claude Code sees a project's MCP config it asks 'Allow this project's MCP servers?' — if that prompt was ever denied, the server won't load even though the config is correct.
5. Prove real connectivity rather than trusting the status: ask 'list my n8n workflows'.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/setup-first-n8n-workflow

<!-- pattern: /setup-first-n8n-workflow -->

---

## n8n MCP server won't start — Node.js prerequisite missing
**Symptom**: Lesson 1.4 setup fails repeatedly even though the clone, .env, and config all look right; the MCP server reports failed or not connected. On Mac, Homebrew may also be absent.

**Root cause**: Node.js was never installed on the machine. The MCP server needs it to run, and students often assume Claude Code installed it for them when in fact it was asking permission to do so.

**Fix steps**:
1. Do the Pre-Flight Setup Checklist attached to lesson '1.4 n8n MCP Server' BEFORE starting the lesson — it covers exactly this (Node.js, Homebrew on Mac, n8n account and API key, and the common gotchas).
2. Run node --version in a terminal. If nothing comes back, that is the problem: install Node.js from nodejs.org, or ask Claude Code for OS-specific steps and actually approve the install when it asks.
3. On Mac, also check brew --version and install Homebrew if it is missing.
4. Rerun the setup in plan mode so you can review what will be installed before it runs — approving npm install is expected and required here.
5. Strip any trailing slash from N8N_API_URL in your .env. Use https://your-instance-host with no trailing slash — copy the host from your n8n browser address bar, and generate N8N_API_KEY under n8n Settings > n8n API.
6. Expect the process and the file tree to look different from the video — Claude Code is non-deterministic. Judge it by whether the server starts and your workflows list.

**Lesson reference**: Lesson 1.4 n8n MCP server setup — thread notes the lesson doesn't mention the Node.js prerequisite

**Confidence**: high — community-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/unable-to-start-mcp-server-tutorial-14-n8n-mcp-server
- https://www.skool.com/ai-automation-society-plus/mcp-server-connection-failed-on-mac-n8n-cloud-help

<!-- pattern: /unable-to-start-mcp-server-tutorial-14-n8n-mcp-server -->

---

## Homebrew installed but brew not found — login-shell PATH gotcha
**Symptom**: During preflight setup Homebrew installs fine, but Claude Code reports brew isn't on PATH and offers to reinstall.

**Root cause**: The brew shellenv line in ~/.zprofile only loads for login shells in a brand-new terminal window; Claude Code's shell often skips ~/.zprofile.

**Fix steps**:
1. Verify in a NEW macOS Terminal window: brew --version. If it prints, Homebrew is fine — it's only a PATH issue.
2. Let Claude Code add the PATH entry when it offers (safe), or add eval "$(/opt/homebrew/bin/brew shellenv)" to BOTH ~/.zprofile and ~/.zshrc so login and non-login shells see it.
3. Never reinstall to fix a PATH problem.

**Confidence**: high — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/preflight-set-up

<!-- pattern: /preflight-set-up -->

---

## Trigger.dev MCP half-dead — the mystery project folder and the missing login
**Symptom**: The Phase 3 Trigger.dev lesson starts inside a project folder the student doesn't have; and once the MCP is installed it shows as connected but only the docs-search tool responds.

**Root cause**: The project folder is created during the preceding GitHub lesson — one prompt has Claude create the local folder, link it to the repo, add a .gitignore and make the first commit — so students who skimmed that step have nothing to open. Separately, the Trigger.dev MCP's docs-search tool works before you log in while every other tool stays dead, which makes a half-installed server look connected.

**Fix steps**:
1. Locate that project folder on your machine; if it isn't there, re-run the GitHub lesson's prompt with your repo name and Claude will rebuild it. Open Claude Code inside that folder.
2. Run: npx trigger.dev@latest install-mcp --client claude-code
3. Fully quit and reopen Claude Code (not just a reload), and COMPLETE the Trigger.dev login when it prompts — that is the step that actually switches the server on.
4. Verify with /mcp: it lists the server's status and the tools it loaded. 'Connected but only docs-search works' means the install succeeded and you just need to finish the login. If /mcp shows nothing at all, you're likely in a different folder than where it installed.

**Lesson reference**: Build Your Portfolio — GitHub repo setup lesson and Phase 3 Trigger.dev MCP lesson (taught by Kodi Zene)

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/triggerdev-mcp

<!-- pattern: /triggerdev-mcp -->

---

## Claude Code suddenly inventing n8n nodes — reload the skills repo, CLAUDE.md, and MCP
**Symptom**: Workflows that built fine last week now contain made-up nodes, often right after a new model release.

**Root cause**: Without the n8n skills repo referenced in CLAUDE.md and the n8n MCP exposing real node schemas, Claude guesses; a just-released model plus stale context makes it worse.

**Fix steps**:
1. Confirm the n8n skills repo from lesson 1.5 n8n Skills is not just cloned but actually referenced in your CLAUDE.md — reviewing the CLAUDE.md was the step the student had missed, and fixing it resolved her case.
2. Connect the n8n MCP so Claude reads real node schemas instead of guessing; without it, invented nodes are expected behaviour.
3. If the problem started right after a new Opus release, try Sonnet for n8n workflow building and make sure the CLI itself is up to date — the CLI sometimes lags a model release.
4. Use /clear between unrelated tasks, and at most one deliberate /compact mid-task; the student credited this session hygiene as part of what fixed her sessions.

**Lesson reference**: Lesson #1.5 n8n Skills (skills repo clone + CLAUDE.md reference)

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/is-it-me-or-claude

<!-- pattern: /is-it-me-or-claude -->

---
