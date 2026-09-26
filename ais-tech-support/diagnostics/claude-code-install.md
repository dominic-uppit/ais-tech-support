# Diagnostic — Claude Code Install & PATH

Use this when a student can't get the `claude` CLI to run: `command not found`, `permission denied`, PATH errors, install fails, "I have the VS Code extension but `claude` doesn't work in terminal", or "I have no admin rights on this machine". Covers everything in taxonomy section 1 EXCEPT connecting n8n to Claude Code with the n8n MCP server (that lives in `diagnostics/lesson-1-4-n8n-mcp.md`).

**Key thing to know going in**: many students install the VS Code Claude Code extension and assume `claude` is also installed in their terminal. **The extension and the CLI are separate things.** The extension actually depends on the CLI underneath. If `claude --version` doesn't return a version in a normal terminal, the CLI itself needs installing — even if the extension is sitting there in VS Code.

---

## Step 1 — What OS are they on?

Different OS, different installer, different failure modes. Ask (or infer from the transcript: PowerShell / `~/.zshrc` / `Set-ExecutionPolicy` / `%USERPROFILE%` are tells).

- **Mac or Linux** → go to Step 2.
- **Windows** → go to Step 3.
- **WSL** → treat as Linux for installs; use the Mac/Linux branch.

---

## Step 2 — Mac / Linux install path

### 2a. Run the native installer

In a regular terminal (not inside Claude chat, not the VS Code integrated terminal initially):

```
curl -fsSL https://claude.ai/install.sh | bash
```

This drops the `claude` binary into `$HOME/.local/bin`.

### 2b. Add to PATH

The installer usually edits the shell RC file, but doesn't always. Add it manually to be safe:

```
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

If they're on bash instead of zsh, use `~/.bashrc`. (Check with `echo $SHELL`.)

### 2c. Open a fresh terminal window

This is the step that trips most people up. PATH changes don't refresh in the current terminal session. **Close the terminal window completely and open a brand new one.** Do not just `source` and assume it's done.

### 2d. Verify

```
claude --version
```

Should return a version. If it does → done. If it says `command not found`:
- Confirm the install actually placed the binary: `ls $HOME/.local/bin/claude` — file should exist.
- Confirm PATH includes it: `echo $PATH | tr ':' '\n' | grep -i local` — should show `$HOME/.local/bin`.
- If both are fine but it's still missing → fully quit and reopen the terminal app (not just the tab).
- If `ls $HOME/.local/bin/claude` shows nothing, re-run the installer from Step 2a.

<!-- pattern: /introduction-to-mcp-servers-in-cloud-code-problem, /failed-to-setup-md-file -->

### 2e. `EACCES` / permission denied on Mac/Linux

If the install or `claude` itself errors with permission denied:

- They may have an old npm-based Claude Code install left over that's owned by root. Check: `which claude` — if it points at `/usr/local/bin/claude` or somewhere under `/opt`, that's the old npm install.
- Clean up: `sudo rm /usr/local/bin/claude` (or wherever `which` pointed), then re-run the native installer from 2a.
- Don't `chmod 777` anything to "fix" it — that's a common but bad suggestion that leaves them with broken permissions.

---

## Step 3 — Windows install path

### 3a. Decide: admin rights or not?

Ask: "Do you have admin rights on this machine, or is it a corporate laptop?"

- **Has admin** → Step 3b (standard PowerShell installer).
- **No admin / corporate** → Step 3c (Scoop or portable Node).

### 3b. Standard PowerShell installer (admin path)

In PowerShell:

```
irm https://claude.ai/install.ps1 | iex
```

Then:

1. **Close PowerShell completely** and open a fresh PowerShell window. PATH won't refresh otherwise.
2. Run `claude --version`. Should return a version.
3. If still `command not found` or "not recognized as the name of a cmdlet":
   - Manually add `%USERPROFILE%\.local\bin` to your **User PATH** via Environment Variables (Search → "Edit environment variables for your account" → Path → New).
   - Open another fresh PowerShell window. Run `claude --version` again.

Also run:
```
Get-Command claude
```
This is the Windows equivalent of `which claude` — confirms the path the shell will resolve.

<!-- pattern: /failed-to-setup-md-file -->

### 3c. No admin / corporate Windows machine

The Node.js MSI installer wants an admin password they don't have. Two options:

**Option 1: Scoop (recommended — no admin needed)**

```
iwr -useb get.scoop.sh | iex
scoop install nodejs-lts
```

Then run the Claude Code installer from 3b.

**Option 2: Portable Node + walk through with Claude itself**

Have them open Claude Code (chat, claude.ai, or Claude Desktop — any of them) and say:

> I'm on a corporate Windows machine, no admin access, need Node.js installed for the Claude Code CLI. Walk me through a portable install that doesn't need admin.

It will grab the Node binary from nodejs.org, drop it in a user-writable folder, add it to User PATH, verify with `node --version`. Then they go back to 3b for the Claude install.

Verify Node first: `node --version` should return v18+ before Claude install.

<!-- pattern: /install-mcp-server-problem-with-nodejs, /how-to-sync-claude-work-across-2-laptops-no-admin-on-one -->

### 3d. PowerShell execution policy blocking install

If the install script errors with "running scripts is disabled on this system":

1. Open PowerShell **as Administrator** (Right-click PowerShell → Run as Administrator).
2. Run:
   ```
   Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
   ```
3. Type `Y` to confirm.
4. Close that PowerShell window.
5. Open a regular (non-admin) PowerShell and re-run the install from 3b.

This is also the fix if they later see Claude Code asking for permission on every PowerShell command despite bypass-permissions being on — Windows blocks unsigned scripts by default, independent of Claude's permission system.

<!-- pattern: /bypass-permission-does-not-bypass-powershell -->

---

## Step 4 — They installed via npm instead of the native installer

If they say "I ran `npm install -g @anthropic-ai/claude-code`" — that path still works but the native installer is now preferred:

- Their existing npm install isn't broken — just older. They can update it: `npm install -g @anthropic-ai/claude-code@latest`.
- If `/plugin` doesn't work, this update is also the fix for that (outdated CLI).
- If they want to switch to the native installer cleanly:
  1. `npm uninstall -g @anthropic-ai/claude-code` first.
  2. Confirm `which claude` returns nothing.
  3. Then run the native installer from Step 2 or 3.

<!-- pattern: /need-help-connecting-n8n-mcp-server-on-hostinger -->

---

## Step 5 — Specific errors and their fixes

### `claude: command not found` (Mac/Linux/WSL)

- Either CLI was never installed, OR PATH doesn't include `$HOME/.local/bin`.
- Run `ls $HOME/.local/bin/claude`:
  - File missing → run installer (Step 2a).
  - File exists → PATH issue (Step 2b/2c).

### `claude` not recognized (Windows PowerShell)

- Either CLI was never installed, OR PATH doesn't include `%USERPROFILE%\.local\bin`.
- Run `Get-Command claude` — if it errors, install missing.
- If `Test-Path "$env:USERPROFILE\.local\bin\claude.exe"` returns `True`, it's a PATH issue (Step 3b last bullet).

### "permission denied" (Mac/Linux)

- Leftover root-owned install from old npm. See Step 2e.

### "running scripts is disabled on this system" (Windows)

- Execution policy. See Step 3d.

### "Failed to start Claude's workspace" on Windows

- Not usually an install issue.
- Check `https://status.claude.com` first — if there's an incident, wait.
- On Windows, enable Hyper-V via "Turn Windows features on or off". Reboot. Try again.

<!-- pattern: /claude-failing-to-start-workspace, /solved-claude-failing-to-start-workspace -->

### "API Error 400 — Accept Consumer Terms"

- Install is fine. Their account hasn't accepted updated ToS.
- Inside Claude Code: `/status` shows the email it's using.
- They need to log into `claude.ai` with that exact email (not whatever SSO is auto-logging them in elsewhere).
- If ToS prompt still doesn't appear, try `console.anthropic.com` instead.

<!-- pattern: /claude-error -->

---

## Step 6 — Non-technical / no-prior-dev-setup students

If the student is clearly new (e.g., "I've never used terminal before", "what's a command line", "I came from Make/Zapier"):

- Don't dump install instructions on them. Walk through it step by step:
  1. **Mac**: "Open Spotlight (Cmd+Space) → type 'Terminal' → press Enter. A black window opens. That's the terminal."
  2. **Windows**: "Press Win key → type 'PowerShell' → click Windows PowerShell. A blue/black window opens. That's PowerShell."
  3. "Copy the install command exactly. Right-click in that window to paste (left-click doesn't always paste). Press Enter."
- Verify each step lands before moving on.
- They may also need:
  - **Git** (for any course that involves repos): `brew install git` (Mac), or installer from git-scm.com (Windows).
  - **Node.js** (for n8n MCP): node v18+ — installer from nodejs.org, or Scoop on no-admin Windows.
  - **VS Code** (if course uses it): direct download from code.visualstudio.com.
  - **A GitHub account** (only needed if they want cross-machine sync — see corpus pattern). They can install Claude Code without one.

**Don't make them install everything up front.** Install just what they need for the current lesson. Many students get overwhelmed by an up-front list of tools they don't yet understand.

<!-- pattern: /installing-claude-with-no-github-account, /im-lost-help -->

---

## Step 7 — Post-install verification

Have them run all of these in a fresh terminal:

| Command | Expected result |
|---|---|
| `claude --version` | A version number (not "command not found") |
| Mac/Linux: `which claude` | A path under `$HOME/.local/bin/` |
| Windows: `Get-Command claude` | Shows source path |
| `claude` then type a prompt and Enter | A response — confirms auth too |

If `claude --version` works but `claude` (interactive) errors:
- Account/auth issue, not install. Run `/login` from within Claude Code, or check `/status`.

If all of the above work → install is good. Now they can:
- Connect n8n to Claude Code with the n8n MCP server, if that is their goal: `diagnostics/lesson-1-4-n8n-mcp.md`.
- Or work on whatever they were originally trying to do.

---

## If none of the branches matched

Switch to drafting a Support Needed post (see SKILL.md Escape Hatch B). Pre-fill with:

- **OS** + version (e.g., "macOS 14.5", "Windows 11 Pro", "Ubuntu 22.04 WSL")
- **Admin rights** (yes / no / corporate-managed)
- **What they ran** (paste the exact command)
- **What happened** (exact error message — verbatim, not paraphrased)
- **Output of**: `which claude` (Mac/Linux) or `Get-Command claude` (Windows)
- **Output of**: `echo $PATH` (Mac/Linux) or `$env:PATH` (Windows)
- **Node.js installed?** `node --version`

No need to tag anyone — the support team watches Support Needed‼️ and a well-formed post gets picked up.
