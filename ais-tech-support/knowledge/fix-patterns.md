# Fix Patterns — `/ais-tech-support` Skill Source-of-Truth (index)

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

**This file is an index, not the corpus.** The patterns live in the ten category files below. Load the one that matches the problem — do not load all of them, and do not load a file "just in case". If you are unsure which one holds a pattern, grep the `knowledge/` directory for the symptom rather than reading files end to end.

| File | Routes here when the problem is… |
|---|---|
| `knowledge/fix-patterns-claude-code.md` | Claude Code install / PATH, VS Code extension, Anthropic outages & API errors, regression spirals, cross-machine sync |
| `knowledge/fix-patterns-context.md` | context windows, `/compact`, `/clear`, 5-hour & weekly limits, 1M context, MCP eating context |
| `knowledge/fix-patterns-config-skills.md` | CLAUDE.md placement, skills folders, permissions & settings.json, MCP-vs-CLI, client credential handling |
| `knowledge/fix-patterns-remote-runs.md` | remote/mobile/scheduled Claude Code runs, giving it eyes on n8n, voice input, Cowork/Design/Chrome |
| `knowledge/fix-patterns-n8n.md` | one-off n8n node issues — agents & AI nodes, data flow & expressions, credentials/triggers/reliability, Cloud limits & scaling |
| `knowledge/fix-patterns-hosting.md` | Hostinger, Render, Vercel, Trigger.dev, VPS choice, webhooks dying, FFmpeg in Docker, workflow observability |
| `knowledge/fix-patterns-course-access.md` | long-form backing detail for course access, unlocks, recordings, perks and classroom playback |
| `knowledge/fix-patterns-models.md` | when to switch models, prompt engineering, Antigravity/OpenClaw, RAG & knowledge bases |
| `knowledge/fix-patterns-integrations.md` | Google OAuth (Testing mode, DWD, redirect URIs) and other third-party auth; voice agent telephony (Vapi/Retell carriers, country coverage, SIP, where the voice lessons live) |
| `knowledge/fix-patterns-business.md` | pricing, client delivery ownership, selling websites, design & media output quality |

Thread URLs cited by the skill's "no invented links" rule live in these files (`**Example thread**:` lines), not in this index.
