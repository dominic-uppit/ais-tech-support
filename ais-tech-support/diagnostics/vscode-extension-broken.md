# Diagnostic — VS Code Claude Code Extension Broken

Use this when a student reports the Claude Code extension in VS Code suddenly stopped working, won't activate, throws errors at the bottom-right of VS Code, or has gone weird in some way that didn't exist yesterday. Also covers: bypass-permissions ignored, "extra usage required for 1M context" on every new chat, welcome page reappearing, dark panel/theme issues.

**Lead with this** — it's the most important insight from the corpus: **Anthropic ships VS Code extension updates every few weeks, and some versions break activation or other features. Rolling back is the first move, not the last.** Roll back first, then diagnose.

**Important fallback to mention up front**: the **terminal CLI (`claude` in a regular shell) is unaffected** by extension regressions. If the student needs to keep working, they can use the CLI today and fix the extension later.

---

## Step 1 — Triage by timing

Ask one question (or read it from the transcript):

- **"Worked yesterday, broken today"** → 90% chance it's an extension regression from auto-update. Skip to Step 3 (Rollback).
- **"Never worked"** → Treat as an install issue. Route to `diagnostics/claude-code-install.md`. The extension can't activate if the underlying CLI was never installed.
- **"Worked, then I changed something"** → If they changed model defaults / settings, check Step 6 (specific known bad states). If they changed nothing, treat as regression → Step 3.

---

## Step 2 — CLI vs Extension split-test

Before any rollback, confirm where the breakage actually is:

1. Have them open a **regular terminal** (Mac/Linux: Terminal; Windows: PowerShell or Windows Terminal — NOT the integrated VS Code terminal initially).
2. Run `claude --version`. If this errors with "command not found", the CLI itself isn't installed → route to `diagnostics/claude-code-install.md` instead.
3. If it returns a version, run `claude` and type a prompt. If the CLI works but the extension doesn't, it's definitely extension-side → continue to Step 3.
4. If the CLI also broke → not an extension issue. Check `https://status.claude.com` for an Anthropic outage, and/or check `/status` inside Claude for account/auth state.

---

## Step 3 — Roll back the extension (the canonical fix)

This is the procedure for any "worked yesterday, broken today" extension issue.

**First, check what version they're on** — this guides which version to roll back to:
- VS Code → Extensions panel → click **Claude Code** → version number shows at the top of the extension details.
- If they're on **anything past v2.1.128**, that's almost certainly the cause. Roll back to 2.1.128.
- If they're already on 2.1.128 or earlier and it's still broken, the fix is different (could be a stale CLI install or a config issue) — skip to Step 6 and check the specific symptoms instead.

Rollback procedure:

1. Open VS Code → Extensions panel (Ctrl+Shift+X / Cmd+Shift+X).
2. Find **Claude Code**.
3. Click the **gear icon** next to the Uninstall button.
4. Pick **"Install Another Version..."**.
5. Pick a known-good earlier version based on what feature matters most:
   - **Most recent reliable**: 2.1.128
   - **Older but stable**: 2.1.77, 2.1.52, 2.1.49
   - **Special case — bypass-permissions matters**: see Step 4 (2.1.77 is the safe pick)
6. VS Code prompts to reload window — say yes.
7. Click the gear icon again → **uncheck "Auto Update"** for this extension so it doesn't silently reinstall the broken version overnight.
8. Verify Claude Code launches and the chat panel loads.
9. Re-enable auto-update in 1-2 days — Anthropic usually ships a fix fast. If a newer post-fix version has shipped since the broken one (e.g., 2.1.130+), it's safe to update again.

**Known-bad versions to avoid** (from corpus reports):
- **v2.1.51** — early activation bug
- **v2.1.129** — activation regression
- **Anything past v2.1.77** — broke bypass-permissions mode (GitHub issue #36168)

If the version they need isn't in the dropdown, they can install from a VSIX file:
- Download from Anthropic's GitHub releases or VS Code Marketplace version history.
- In Extensions panel → `...` menu → "Install from VSIX..." → pick the file.

<!-- pattern: /all-of-a-sudden-claude-code-extension-in-vs-code-doesnt-work, /cannot-use-the-claude, /command-claude-vscodeeditoropenlast-not-found -->

---

## Step 4 — Specific symptom: bypass-permissions asking anyway

If their reported symptom is "Bypass permissions is ON but Claude Code still asks for confirmation on every edit":

- **Root cause**: Known Anthropic GitHub issue #36168. Bypass mode broken in versions newer than v2.1.77.
- **Fix**:
  1. Roll back the VS Code extension to **v2.1.77** specifically (use Step 3 procedure).
  2. Disable auto-update for the extension.
  3. Re-enable bypass-permissions inside Claude Code.

**Bonus** (worth mentioning): even on a working version, bypass-permissions does **not** bypass PowerShell on Windows — that's a separate Windows execution-policy thing. If they also report PowerShell commands needing confirmation:
- Quick fix: in a **normal** (non-admin) PowerShell window → `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` → `Y`. `-Scope CurrentUser` needs no elevation. ⚠️ On a work-managed machine, ask IT before changing execution policy.
- Better fix (community-verified): keep bypass on but add a deny list in `~/.claude/settings.json` or `%USERPROFILE%\.claude\settings.json`:
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
  Deny rules are evaluated BEFORE permission modes — they hard-block even in bypass.

<!-- pattern: /claude-code-keeps-asking-permission-to-edit-files-even-though-settings-are-correct, /bypass-permission-does-not-bypass-powershell -->

---

## Step 5 — Specific symptom: "Extra usage required for 1M context"

If they report: "Every time I open a new chat in VS Code I get an API error. Switching model and typing 'go' makes it work."

- **Root cause**: The extension is defaulting to a 1M-context model variant. 1M context requires "Extra Usage" credits enabled on the account. Without it, the model rejects the request silently on chat open.
- **Fix options** (any one works):
  1. **Enable extra usage** at `https://claude.ai/settings/usage`. Cheapest path if they want the 1M window.
  2. **Pin a non-1M model** as their default: `/model` → pick a standard-context variant (Sonnet without the 1M tag, or Opus). Check `/model` for the current labels in your lineup.
  3. **If both fail** — there's a separate known extension bug that causes this on some versions even with extra usage enabled. Roll back per Step 3.

<!-- pattern: /api-error-in-vs-code-switching-model-helps -->

---

## Step 6 — Other specific known-bad states

### "Welcome message keeps appearing every time I open VS Code"

- **Cause**: Opening VS Code without a project folder loaded — Claude Code shows the welcome page when there's no workspace.
- **Fix**: Right-click the project folder in Finder/Explorer → **"Open with VS Code"**. This opens VS Code with that folder as the workspace, and Claude Code skips the welcome page.

<!-- pattern: /claude-code-issue -->

### Dark panel / Claude Code theme broken

- **Cause**: Theme mismatch between VS Code theme and Claude Code's expected colors after an update.
- **Fix**:
  1. Try `Ctrl+Shift+P` / `Cmd+Shift+P` → "Developer: Reload Window".
  2. If still broken, toggle VS Code's color theme to a known good one (Dark+ or Light+) and reload.
  3. If still broken → Step 3 rollback.

<!-- pattern: /claude-code-dark-panel -->

### "command claude.vscode.editoropenlast not found" or similar internal command errors

- **Cause**: Extension activation partially failed — VS Code registered some commands but not others.
- **Fix**:
  1. Reload window (`Ctrl+Shift+P` → "Developer: Reload Window").
  2. If still broken → fully quit VS Code (all windows), reopen.
  3. If still broken → Step 3 rollback.

<!-- pattern: /command-claude-vscodeeditoropenlast-not-found -->

### "Failed to start Claude's workspace" / Claude Cowork won't start (Windows)

- This often isn't the extension at all. Check in this order:
  1. `https://status.claude.com` — if there's an incident, wait it out.
  2. On Windows, enable **Hyper-V** via "Turn Windows features on or off" — Claude Cowork needs the WSL/Hyper-V stack.
  3. Then reload VS Code.

<!-- pattern: /claude-failing-to-start-workspace, /solved-claude-failing-to-start-workspace -->

### Antigravity 2.0 ate the extension panes

- **Cause**: Google's Antigravity 2.0 removed the VS-Code-style editor entirely; it's now an agent-only standalone app.
- **Fix**:
  1. Roll back Antigravity to its previous version per Google's discussion post: https://discuss.ai.google.dev/t/antigravity-2-0-is-awful-heres-how-to-get-the-previous-version/145512
  2. Disable Antigravity auto-update + set profile to None.
  3. **Better long-term**: switch to plain Microsoft VS Code. The Claude Code extension works identically there, and you don't depend on a Gemini-first Google product for your editor.

<!-- pattern: /antigravity-update -->

---

## Step 7 — Keep working while the extension is broken (fallback)

If they need to keep shipping today and the rollback didn't stick:

1. Open a regular terminal (NOT the VS Code integrated terminal — start fresh outside VS Code).
2. `cd` into their project folder.
3. Run `claude`. They get the full Claude Code CLI experience — chat, edits, MCPs, everything. Just no VS Code panel UI.
4. Bonus: `claude -c` resumes the most recent session. `claude -r` lets them pick a specific past session.
5. When the extension is fixed, sessions started in CLI are visible in VS Code too (same project, same `.claude/` folder).

This is the corpus-verified "don't get blocked by the extension" move. Multiple students have shipped entire projects on CLI while waiting for extension fixes.

---

## Step 8 — Confirm the fix held

After rollback + auto-update disabled, have them:

1. Close all VS Code windows.
2. Reopen the project folder via right-click → "Open with VS Code".
3. Claude Code panel should activate without errors.
4. Run `/status` inside Claude Code → confirms account/auth is connected.
5. Run `/mcp` → MCPs from earlier should still be connected.
6. Make a small test edit to confirm bypass-permissions / permissions work as expected.

If any of these fail after rollback → there's something deeper (account/auth, OS, network). Escape Hatch B.

---

## If none of the branches matched

Switch to drafting a Support Needed post (see SKILL.md Escape Hatch B). Pre-fill with:

- **OS** + version
- **VS Code version** (Help → About)
- **Claude Code extension version** (Extensions panel → Claude Code → look at version number)
- **Exact error message or where it appears** (bottom-right notification? Chat panel? Output panel under "Claude Code"?)
- **Has the CLI been verified to work?** (`claude --version` + a short `claude` chat session)
- **What's been tried**: rollback attempted? Which versions tried?

No need to tag anyone — the support team watches Support Needed‼️ and a well-formed post gets picked up.
