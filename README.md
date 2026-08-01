# /ais-tech-support — AIS+ Tech Support Skill for Claude Code

A Claude Code skill for members of **Nate Herk's AI Automation Society Plus (AIS+)**. It troubleshoots the problems students actually hit — Claude Code setup, lesson 1.4 / n8n MCP, VS Code extension issues, hosting, context limits — and helps you navigate the community (where to post, how course unlocks work, where the live call recordings live). Built from 780+ real support threads — including hundreds solved directly by the AIS+ support team — the full AIS+ classroom, and Nate's YouTube video database.

When it can't solve your problem, it helps you write a great **Support Needed‼️** post instead.

---

## Requirements

- **Claude Code** installed and working (the AIS+ Claude Code course, lesson 1.2, covers this)
- That's it. No API keys, no configuration — the skill is just instructions and knowledge files.

## Install (2 minutes)

There are two ways to install the skill. **Both work exactly the same once installed** — the only difference is *where* the skill lives, which controls *which projects can see it*.

| | **Option A — User level** (recommended) | **Option B — Project level** |
|---|---|---|
| The skill works in… | **Every** Claude Code project on your computer | **One** project only (the one you install it into) |
| Choose this if… | You just want tech-support help available everywhere, always — install once and forget about it | You want to try the skill out in one sandbox project first, or you keep this computer/project locked down and don't want the skill following you into other projects |

Not sure? **Pick Option A.** You can always uninstall later (see below).

---

### Option A — User-level install (works in every project)

**Step 1** — Open Claude Code (in VS Code or the terminal — any project, it doesn't matter which).

**Step 2** — Copy this **entire prompt**, paste it into the Claude Code chat exactly as-is (nothing to fill in or change), and press Enter:

```
Install the ais-tech-support skill for me: download the "ais-tech-support" folder
from https://github.com/dominic-uppit/ais-tech-support and copy it
into my USER-LEVEL Claude Code skills directory (~/.claude/skills/ on Mac/Linux,
%USERPROFILE%\.claude\skills\ on Windows) so the final path is
.claude/skills/ais-tech-support/SKILL.md. If the folder already exists, replace it
with the new version. Then list the installed files to confirm it worked.
```

Claude Code will download the files, put them in the right place for your operating system, and show you the installed files. Approve any permission prompts it asks for along the way.

**Step 3** — Restart Claude Code (close VS Code completely and reopen, or exit the terminal session and start a new one). Then type:

```
/ais-tech-support
```

If it responds and asks what you're stuck on — you're done. It will now work in **every** project on this computer.

---

### Option B — Project-level install (works in one project only)

**Step 1** — Open Claude Code **inside the specific project you want the skill in**. (With this option, the project you're in when you install *is* the project that gets the skill — so this time it matters which one you open.)

**Step 2** — Copy this **entire prompt**, paste it into the Claude Code chat exactly as-is (nothing to fill in or change), and press Enter:

```
Install the ais-tech-support skill for me in THIS PROJECT ONLY: download the
"ais-tech-support" folder from https://github.com/dominic-uppit/ais-tech-support
and copy it into this project's .claude/skills/ directory (create it if it doesn't
exist) so the final path, relative to the project root, is
.claude/skills/ais-tech-support/SKILL.md. Do NOT install it to my user-level
~/.claude/skills directory. If the folder already exists, replace it with the new
version. Then list the installed files to confirm it worked.
```

**Step 3** — Restart Claude Code (close VS Code completely and reopen, or exit the terminal session and start a new one), open the **same project**, and type:

```
/ais-tech-support
```

If it responds and asks what you're stuck on — you're done. Remember: with this option the skill only exists in **this one project**. In any other project, `/ais-tech-support` won't appear — that's by design, not a bug.

## Updating

Same as installing: paste the same install prompt you used before (Option A's or Option B's) and it replaces the old version with the newest one.

## Uninstalling

- Installed with **Option A**? Ask Claude Code: `Delete the ais-tech-support folder from my user-level Claude Code skills directory (~/.claude/skills/ on Mac/Linux, %USERPROFILE%\.claude\skills\ on Windows).`
- Installed with **Option B**? Ask Claude Code (inside that project): `Delete the ais-tech-support folder from this project's .claude/skills/ directory.`

## Troubleshooting the install

| Problem | Fix |
|---|---|
| `/ais-tech-support` doesn't appear after install | Restart Claude Code fully — close **all** VS Code windows (or exit the CLI session) and reopen. Skills load at startup. |
| It works in one project but not others | You installed with **Option B (project level)** — that's exactly what it does. If you want it everywhere, paste the Option A prompt. |
| Claude says it can't find the skills directory | Tell it: "Create the directory first, then install." The `.claude/skills` folder doesn't exist until your first skill. |
| Claude put it somewhere else | For Option A the final path must be exactly `~/.claude/skills/ais-tech-support/SKILL.md` (Mac/Linux) or `%USERPROFILE%\.claude\skills\ais-tech-support\SKILL.md` (Windows). For Option B it's `.claude/skills/ais-tech-support/SKILL.md` inside your project folder. Paste the install prompt again — it self-corrects. |
| Still stuck | Post in **Support Needed‼️** on Skool with a screenshot of what happened — the support team watches the channel. |

## What's inside

```
ais-tech-support/
├── SKILL.md            ← how the skill behaves
├── knowledge/          ← classroom map, community map, video map, taxonomy, corpus notes,
│                         and the fix-pattern corpus split into 10 category files
│                         (fix-patterns.md is the index that routes to them)
└── diagnostics/        ← deep troubleshooting trees for the 8 highest-volume problem areas
```

The diagnostics cover: lesson 1.4 / n8n MCP setup, Claude Code install, context & token limits, VS Code extension breakage, n8n workflows that run but misbehave, third-party API & OAuth failures, course access & navigation, and client work & pricing.

No credentials, no personal data, no course video content — lesson references and links only.

## Terms of use

Free for **AIS+ members** to install, use, and adapt locally. Please don't redistribute, rehost, or resell it — the knowledge files are distilled from real community threads and reference paid classroom content, so they're a membership benefit rather than a public corpus. Full terms in [LICENSE](LICENSE).

---

*Maintained by the AIS+ support team. Found a problem with the skill itself? Post in Support Needed‼️ on Skool.*
