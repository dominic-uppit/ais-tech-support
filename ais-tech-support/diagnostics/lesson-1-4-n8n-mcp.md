# Diagnostic — Lesson 1.4 / n8n MCP Server

Use this when a student is stuck setting up the n8n MCP server (course lesson 1.4) or anything in its neighborhood: "the video doesn't match what I see", "I have 100x more files than Kodi", "no n8n API menu", "/mcp doesn't show n8n", "multiple .env files". This is still one of the highest-volume setup topics in the AIS+ corpus (15+ threads), though course access/navigation now outpaces it by volume.

**Default to what Nate's lesson actually teaches.** Lesson 1.4 has you ask Claude Code to install n8n-mcp **at the project level** (not user-level), configure credentials via a `.env` file (never paste them in chat), and verify with `/mcp` + a "List my n8n workflows" health check. The video walks through the install but doesn't lock you to a single command path — Claude may pick `npm install` inside a cloned repo, `claude mcp add`, or something else.

The two things lesson 1.4 IS strict about:
1. **Project-level scope** ("configuring the MCP server at the project level to avoid common security warnings" — direct from the lesson summary).
2. **Credentials in `.env`**, never in chat.

⚠️ **But a correct `.env` alone does NOT register the MCP server.** This is the trap that produces "I did everything and `/mcp` still shows nothing" (team-verified in a solved thread): **Claude Code does not read `.env` files for MCP server configuration.** The credentials have to be supplied at the moment the server is registered — that's the `-e` flags on `claude mcp add`. Keeping secrets in `.env` is still the right habit for everything else in the project (and it's what the lesson means), but if the student's `.env` is perfect and `/mcp` is empty, the registration step is what's missing, not the file.

**"npx" means three different things here — keep them straight.** Kodi's warning in the video only covers one of them, and a student who heard a flat "don't use npx" will refuse the support team's own recommended command:

1. **Registering the MCP server so it launches via the npx package** — `claude mcp add n8n-mcp ... -- npx n8n-mcp` — is the route the support team recommends in recent solved threads. It's the cleanest path because it avoids cloning the repo entirely. Not what the video shows (the video clones), but what the team recommends now. **This is fine to run.**
2. **Running `npx <package>` ad-hoc as the install command** — Claude proposing "let me just npx this" instead of doing a proper install — is what Kodi's warning targets. **Decline that.**
3. **`npm install` / `npm run build`** are a third, different thing: they populate a cloned repo's dependencies. **Approve those** if a clone already happened. Students wrongly declining `npm install` because they heard "don't use npx" is a top-5 cause of failed setups.

If a student is refusing the registration command in (1) because of the warning about (2), explaining this distinction IS the fix.

**How to use this diagnostic — gate-first, not dump-the-tree.** Do NOT respond with all 11 steps at once. The corpus shows the best answerers (support team included) ask ONE question, get ONE answer, prescribe ONE fix, then loop. Match that pattern:

1. **Read the student's post for which step they're already stuck at.** Often you can skip Step 1 (n8n flavor) or Step 2 (which MCP) because they've already told you.
2. **Answer with the SINGLE most-likely fix** based on what they described. Keep it tight — 3-5 numbered steps.
3. **End with one check question** ("does `/mcp` show n8n-mcp connected now?") rather than dumping the next branch.
4. **Only expand to additional branches if they come back stuck.** A long answer signals "I'm not sure" and a non-technical student will glaze past the right fix.

The exception: if the student is genuinely starting from zero ("I don't know where to begin"), then Steps 1-4 in order is the right walk-through.

**Before you start: the escape hatch.** The support team has publicly told students they can skip 1.4 and keep learning if they get stuck — section 2 doesn't depend on a working n8n-mcp. Mention this if the student sounds frustrated, but only after at least one fix attempt. Don't lead with it.

---

## Step 1 — Which n8n flavor are they running?

Ask (or infer from the transcript):

- **n8n Cloud (paid plan)** → has API menu, can generate keys. Skip to Step 2.
- **n8n self-hosted (Hostinger / VPS / Docker / Railway)** → has API menu, can generate keys. Skip to Step 2. Their `N8N_API_URL` will be the same URL they browse to (e.g. `https://n8n.yourdomain.com`).
- **n8n free trial** → likely NO "n8n API" menu visible. Jump to Step 1a below before doing anything else.
- **No n8n yet / "starting from scratch"** → they need to pick a hosting option first. Recommend:
  - Quickest for learning: n8n Cloud free trial (note the API limitation below) or paid starter
  - Cheap + your own server: Hostinger 1-click n8n VPS template
  - Local-only: Docker on their machine
  Then come back to Step 2.

### Step 1a — Free trial has no API menu

If they say "Settings → n8n API doesn't exist" or "Kodi shows an API option but I don't have it":

- The API key endpoint is gated to **paid Cloud plans or self-hosted**. Free trial doesn't surface it.
- **Newer n8n UI**: the same info now lives under `Settings → Instance-level MCP → Connection details`. Have them check there first.
- If it's genuinely missing: their options are (a) upgrade to a paid Cloud plan, (b) self-host (Hostinger 1-click template is the path of least resistance), or (c) skip 1.4 and continue with section 2.

<!-- pattern: /14-n8n-mcp-server-cant-find-api-n8n-key, /not-seeing-the-n8n-the-menu-bar -->

---

## Step 2 — WHICH n8n MCP do they think they're installing?

This is the single biggest source of confusion. This framing is the clearest in the corpus — use it verbatim:

> There are two different tools called "n8n MCP".
>
> 1. **n8n's official instance-level MCP** (built into n8n itself, under Settings → Instance-level MCP). This **exposes your existing workflows TO Claude** so Claude can run them. Useful for Claude Desktop chat.
> 2. **czlonkowski/n8n-mcp** (community, separate server). This **gives Claude knowledge of every n8n node + validation tools** so Claude can BUILD workflows for you. The AIS+ course uses this one.
>
> Which are you setting up?

- 99% of students need **czlonkowski/n8n-mcp**. The course is about Claude Code building workflows, not Claude Desktop running them.
- If they only want Claude Desktop to trigger existing workflows → that's the native one, configured in n8n itself, not in Claude Code. Different lesson.

<!-- pattern verified in /n8n-mcp-connection -->

---

## Step 3 — Confirm the install path

**What lesson 1.4 actually says**: have Claude install n8n-mcp at the project level, with `.env`-stored credentials. The lesson video walks through the install (via a clone) but doesn't pin you to one specific command. Two paths work: the **clone path** the video shows, and the **npx registration path** the support team now recommends (below).

Common confusion: Kodi warns against Claude running an ad-hoc `npx <package>` as its install command. Students hear "don't approve npx" and then **also reject `npm install`** — that's wrong, and it's a top-5 cause of failed setups. Three different things:
- `claude mcp add ... -- npx n8n-mcp` = registering the server so it launches via the npx package. **Team-recommended. Fine to run.**
- Claude proposing ad-hoc `npx <package>` as the install step = what Kodi's warning targets. **Decline.**
- `npm install` / `npm run build` = populate a cloned repo's dependencies. **Approve** if a clone already happened.

**Rule of thumb**: approve `npm install`, `npm run build`, `git clone`, Node.js install prompts, and the `claude mcp add ... -- npx n8n-mcp` registration. Decline only an ad-hoc `npx <package>` offered as the install command itself.

### If they're stuck mid-install

Don't prescribe a from-scratch install path — walk through the actual symptom:

- **Claude is asking for `npm install` and they're not sure**: approve it. (See above — different from the ad-hoc `npx` Kodi warns about.)
- **They cloned the repo and got "100x more files than the video"**: that's normal for a clone. They need to finish whatever install Claude started. Ask them to tell Claude "finish the n8n-mcp setup", and approve every command Claude asks for — decline only an ad-hoc `npx <package>` offered as the install step. (If they'd rather not deal with the clone at all, the npx registration route below drops it entirely.)
- **Multiple `.env` files in the cloned folder**: `.env.example` and `.env.docker` are templates. Only the plain `.env` matters. Have Claude copy `.env.example` → `.env` and fill in the values.
- **They haven't started yet**: have them ask Claude to install n8n-mcp at the project level. Point them at the Pre-Flight Setup Checklist PDF attached to lesson 1.4 first — it covers prereqs (Node.js, n8n API key, Homebrew on Mac).

<!-- pattern: /mpc-server-set-up, /trouble-with-setting-up-n8n-mcp-server-in-vs-claude-code -->

### Recommended path: register via the npx package (no clone)

This is what the support team recommends in recent solved threads, and it's the cleanest route because it skips the clone entirely. Two ways to get there:

- **In Claude Code**: "add the n8n-mcp server as an MCP connection using the npx n8n-mcp package, do not clone or download anything." That wording leaves nothing to misread — Claude will ask for the n8n URL and API key and write the config itself.
- **From a normal terminal** (not inside Claude Code chat), run it directly:

```
claude mcp add n8n-mcp --scope project -e MCP_MODE=stdio -e LOG_LEVEL=error -e DISABLE_CONSOLE_OUTPUT=true -e N8N_API_URL=https://your-n8n-instance.com -e N8N_API_KEY=your-api-key -- npx n8n-mcp
```

⚠️ **Placeholder alert — never show this command verbatim.** `https://your-n8n-instance.com` and `your-api-key` are placeholders. Substitute the student's real n8n URL (the exact address they type in their browser to open n8n) and their real API key (n8n → Settings → n8n API → Create API Key) before presenting it — or ask for those two values first. A beginner will paste the placeholders as-is and get a connection failure.

Be honest about the provenance: **this is not what the lesson video walks through** (the video clones), but it is what the support team recommends now. Say it that way — "not what the video shows, but what the team recommends now." It is a primary path, not a fallback.

And if the student pushes back with "but Kodi said no npx": this is the registration form, not the ad-hoc install command. See the three-way distinction at the top of this file.

---

## Step 4 — Set the install command up correctly

**This step applies regardless of which install command path Claude went through** — clone + `npm install`, `claude mcp add`, or something else. All paths still need the API URL set correctly and the right scope.

For the **clone path**: if Claude cloned the repo and ran `npm install` + `npm run build`, the server is usually wired up via the cloned repo's own build — they don't need to run a separate `claude mcp add` command.

For the **npx registration route**: the `claude mcp add` command from Step 3 already includes the URL and scope flags. They run it ONCE in their PowerShell/terminal (not inside Claude Code chat).

Two things to verify with them BEFORE they verify the install:

### 4a. `N8N_API_URL` format — the silent killer

The single most common silent failure. The MCP appends `/api/v1/...` internally, so the env var must be the **base URL only**.

| What they have | What it should be |
|---|---|
| `https://yourname.app.n8n.cloud/api/v1` | `https://yourname.app.n8n.cloud` |
| `https://yourname.app.n8n.cloud/` (trailing slash) | `https://yourname.app.n8n.cloud` |
| `https://n8n.mydomain.com/api/v1/` | `https://n8n.mydomain.com` |
| `yourname.app.n8n.cloud` (no scheme) | `https://yourname.app.n8n.cloud` |

Rule: paste exactly the URL they use to open n8n in their browser. No `/api/v1`. No trailing slash. With `https://`.

<!-- pattern: /connect-n8n-mcp-to-claude-code-issue, /using-n8n-via-hostinger-connection-issue -->

### 4b. Scope: `--scope project` matters

- `--scope project` writes config to `.mcp.json` in the project root. Per-project, recommended for the course.
- Without it, the default writes user-scope → the MCP loads everywhere, AND it won't show under `/mcp` if Claude Code's working directory doesn't match expectations.
- If they already installed without `--scope project` and `/mcp` doesn't show n8n-mcp, ask them to remove it (`claude mcp remove n8n-mcp`) and re-add with `--scope project`.

<!-- pattern verified in /connect-n8n-mcp-to-claude-code-issue -->

---

## Step 5 — Restart Claude Code + approve the project MCP prompt

After running the add command:

1. **Fully quit Claude Code** (not just close the VS Code window — close all VS Code windows, then reopen). On terminal CLI, exit the session and start a new one.
2. On first launch in the project, Claude Code shows: **"Allow this project's MCP servers?"** They must approve. If they accidentally denied:
   - Open `~/.claude.json` (Mac/Linux) or `%USERPROFILE%\.claude.json` (Windows).
   - Find the project's path entry.
   - Remove `n8n-mcp` from the `disabledMcpjsonServers` list.
   - Restart Claude Code — the prompt will reappear.
3. Run `/mcp` inside Claude Code → `n8n-mcp` should show as **connected**.
4. End-state check (this is the one that matters, not the visual match to the video): ask Claude in chat "List my n8n workflows." If it returns workflows, the connection is real.

**Normalize this for them**: "Your terminal output won't look exactly like Kodi's video. Claude Code is non-deterministic. The video frame is not the success criterion — `/mcp` showing connected + Claude listing workflows IS." (Covered in corpus-notes.)

---

## Step 6 — Behind a corporate / enterprise proxy?

If the student mentions corporate machine, firewall, "can't reach npm", or `npm install` failing with network errors:

1. **Documentation-only mode** works offline — set `MCP_MODE=stdio` and skip the `N8N_API_URL`/`N8N_API_KEY` env vars. Claude still gets node knowledge and validation, just can't introspect a live n8n.
2. For the install itself behind a proxy:
   - Mirror czlonkowski/n8n-mcp to internal Bitbucket / GitLab.
   - Point `.npmrc` at internal Artifactory / Nexus.
   - Or get IT to whitelist `registry.npmjs.org` for the developer's user.
3. **No-admin Windows path**: have them install Node via Scoop instead of fighting the MSI:
   ```
   iwr -useb get.scoop.sh | iex
   scoop install nodejs-lts
   ```
   Then verify with `node --version` and re-run the `claude mcp add` command.

<!-- pattern: /n8n-mcp-server-not-able-to-add-due-to-enterprise-restriction, /install-mcp-server-problem-with-nodejs -->

---

## Step 7 — Windows-specific gotchas

If they're on Windows:

- **PowerShell execution policy** can block scripts during install. If they hit "running scripts is disabled on this system": in a **normal** (non-admin) PowerShell window run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`, type `Y`. `-Scope CurrentUser` does not need Administrator — and installing from an elevated window is itself a cause of "claude: command not found" later, because the PATH entry lands on the wrong profile. ⚠️ On a work-managed laptop, don't change execution policy without asking IT first.
- **PATH after install**: if `claude` or `npx` is "not found" in a fresh terminal, manually add `%USERPROFILE%\.local\bin` to User PATH via Environment Variables, then open a brand new PowerShell window.
- Use Shift+Tab inside Claude Code to cycle permission modes — they might be hitting confirmations they could bypass.
- Backslash line continuations from the standard command don't work in PowerShell — use the one-line PowerShell version in Step 4.

<!-- pattern: /need-help-connecting-n8n-mcp-server-on-hostinger -->

---

## Step 8 — Hostinger / self-hosted specifics

If they're on Hostinger or another self-hosted setup:

- `N8N_API_URL` = the URL you browse to. Same rule as Cloud — no `/api/v1`, no trailing slash.
- Generate the API key in n8n: `Settings → n8n API → Create an API key`. Copy it to `N8N_API_KEY`. (The menu is **n8n API**, not "API" — students hunting for a plain "API" entry is a recurring dead end.)
- After editing `.env` or running `claude mcp add`, **restart Claude Code** so env vars reload.
- If `/mcp` shows connected but Claude gets a 404 / "resource not found" / can't find workflows → known Hostinger/self-hosted quirk. Two verified fixes, in order:
  1. **Base-URL path mismatch** (team-verified): czlonkowski n8n-mcp wants the bare root in `N8N_API_URL` (it appends `/api/v1` itself); other tools, like the n8n node's own API credential, need `/api/v1` spelled out. Confirm each field has the form that tool expects. (Current n8n-mcp versions strip trailing slashes and detect an existing `/api/v1`, so they can't double it — if the URL is already the bare root, move to fix 2 rather than toggling the format.)
  2. **API key from the wrong place** (community-verified): the key must be generated inside n8n (Settings → n8n API), NOT in the Hostinger panel. Make a fresh key, no expiration, default scopes, update `.env`, restart.
  If both fail → Escape Hatch B (Support Needed post); the support team maintains workaround knowledge for Hostinger edge cases.
- Hostinger HTTPS struck-through? That's not the MCP — that's their DNS. A record needs to point at the VPS IP; SSL issues automatically once DNS resolves (5-60 min).

<!-- pattern: /using-n8n-via-hostinger-connection-issue, /need-help-connecting-n8n-mcp-server-on-hostinger, /new-hostinger-vps-shows-https-not-secure -->

---

## Step 9 — Connecting self-hosted n8n MCP to Claude Desktop (NOT Claude Code)

If their goal turned out to be exposing self-hosted n8n to Claude Desktop chat (different from the course path):

1. Edit `claude_desktop_config.json`:
   - Mac: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Windows: `%APPDATA%\Claude\claude_desktop_config.json`
2. Claude Desktop **does not support** `"type": "http"` for remote MCPs. Use `mcp-remote` as a bridge:
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
3. Node.js must be installed (for `npx`).
4. **Fully quit Claude Desktop** (right-click tray icon → Quit, not just close window), then reopen.
5. Behind nginx (Hostinger reverse proxy), add to the MCP location block:
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
   `proxy_buffering off`, `proxy_cache off`, and `gzip off` are the critical ones — without them nginx buffers the stream and the connection fails silently. Keep `chunked_transfer_encoding on`: turning it **off** forces nginx to buffer the whole response to compute a Content-Length, which re-creates the exact problem you're fixing.
6. Behind Cloudflare Zero Trust → set up a CF Service Token, add as an additional auth header.

<!-- pattern: /anyone-get-hostinger-self-hosted-n8n-working-with-desktop-claude-via-mcp -->

---

## Step 10 — Conceptual confusion: "Where does the MCP actually run?"

If they ask "I thought the MCP would be in the cloud but Claude said it's local":

- The **MCP server is a local helper** on their machine. It only runs while Claude Code is running. Local is correct.
- The **n8n instance** (where workflows actually live and execute) is what they'd host in the cloud or on a VPS if they need 24/7 uptime, webhooks, scheduled triggers.
- For learning + portfolio, n8n local or n8n free trial is fine. They only need cloud n8n for always-on production work.

<!-- pattern: /n8n-mcp-server-building-portafolio, /mcp-server-2 -->

---

## Step 11 — Final verification checklist

Before declaring done, walk them through:

- [ ] `claude --version` returns a version (CLI is installed)
- [ ] `/mcp` inside Claude Code shows `n8n-mcp` as connected (not "failed" or missing)
- [ ] In chat, "List my n8n workflows" returns actual workflows from their n8n instance
- [ ] Project-scope `.mcp.json` exists in the project root (for project-scope installs)

If any of these fail and you've gone through all branches — Escape Hatch B.

---

## If none of the branches matched

Switch to drafting a Support Needed post (see SKILL.md Escape Hatch B). Pre-fill the template with:

- **OS** (Mac / Windows / Linux + version)
- **n8n hosting**: Cloud paid / Cloud free / Hostinger / self-hosted Docker / other
- **Install command Claude proposed**: which command(s) did Claude ask permission for, and which did they approve/reject?
- **Exact `N8N_API_URL` they used** (mask the subdomain if sensitive — but include the path/trailing-slash structure)
- **Exact error message or `/mcp` output**
- **What they've tried so far** from this diagnostic

No need to tag anyone — the support team watches Support Needed‼️ and a well-formed post gets picked up.

And remind them: 1.4 is optional. They can move to section 2 anytime and circle back.
