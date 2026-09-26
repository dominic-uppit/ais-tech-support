# Corpus Notes — Meta-Observations for `/ais-tech-support` Skill Design

## Gauge the student's level before answering (do this first, every time)

Most people here came from Make, Zapier, or no automation background at all, and a large share
are building with Claude without knowing what the pieces underneath are called. A correct answer
pitched two levels above the person is a wasted answer: they cannot act on it, and they usually
will not say so. They go quiet, or come back with "sorry, what is a Node app?", which costs a
round trip.

**Read the signals in what they wrote, pick a level, then write to it.** Nobody states their
level, so infer it.

| Signal | Reads as |
|---|---|
| "I built everything with Claude" and cannot say what the stack is | **Beginner.** They prompted their way here. They own the result without a model of the parts. |
| Names a product but not the layer it occupies ("my hosting provider", "normal hosting") | **Beginner.** |
| Asks "do I need X or Y" where X and Y are not comparable (a host vs a framework) | **Beginner.** The categories have not separated yet. |
| Mentions Make/Zapier/Airtable background, "non-technical", "new to this", "first project" | **Beginner.** |
| Correct tool names in the right slots (repo, deploy, DNS, webhook, env var) | **Intermediate.** |
| Describes what they already tried and why it failed | **Intermediate**, at least. |
| Pastes an error and names the node/file/line it came from | **Intermediate.** |
| Asks about tradeoffs, architecture, scale, multi-tenancy, cost per run | **Advanced.** |
| Discusses versions, self-hosting, Docker, CI, RLS, queues unprompted | **Advanced.** |

Mixed signals are normal. **When a post reads between two levels, write to the lower one.** An
advanced reader skims past an explanation costlessly; a beginner stalls on an unexplained term.

### What changes per level

- **Beginner.** Lead with the decision and what to do, not the mechanism. Every unavoidable
  technical term gets a plain-language handle in the same breath, the way the team already does
  it: "Tailscale is basically a lightweight VPN", "think of it as Vercel for the front,
  Trigger.dev for the back", "a system prompt is basically the agent's job training". Do not
  explain *why* the infrastructure behaves that way unless they asked. Prefer "your client's
  hosting can't run this kind of page" over process recycling and memory caps. Give them the
  words for the thing so they can search it later, just do not make understanding the words a
  prerequisite for acting.
- **Intermediate.** Normal technical register. Name products and mechanisms directly, skip the
  analogies, keep one line of rationale so they can adapt the fix rather than paste it.
- **Advanced.** Lead with the tradeoff, not the recommendation. Assume the vocabulary. They are
  usually choosing between options they already know, so the useful content is the constraint
  that decides it.

**This is about register, not depth.** Do not withhold the real answer, do not water down the
recommendation, and never be condescending. Beginner framing means the same answer in words that
land. The failure this prevents is the correct answer nobody can use.

### State the answer, do not guess at their setup

Two habits creep into beginner-pitched replies and both need cutting.

**No assumptions about what they have.** "Your site is almost certainly static files", "you're
probably on shared hosting", "I'm guessing you used Next.js". If it was not stated in the thread,
do not assert it, and do not hedge your way into asserting it. "Almost certainly" and "probably"
are guesses wearing a hedge. A guess that lands wrong makes the whole reply feel written for
someone else, and a beginner cannot tell which parts still apply.

Write the instruction instead of the diagnosis. **"Posts live in a database instead of in the
website's code"** tells them what to do without claiming to know what they built. **"Your posts
are almost certainly in the code, so..."** is the same advice resting on a guess. If a fact
genuinely changes the answer and it is not in the thread, ask for it at the end, and do not block
the fix on it.

**No tee-ups.** Do not announce that a point is coming, then make it. "But before that, there's
one thing worth sorting out first, because it decides everything else" carries no content: a
sentence that builds anticipation for the next sentence instead of carrying its own is wasted on
someone who is stuck. Lead with the answer.

If a step needs justifying, put the reason **after** the step, tied to it: "The reason for step 1:
if posts are saved in the website's code, every new one needs the site rebuilt, which your client
can't do themselves." That explains without asserting anything about their build, and without
making them read a preamble to reach the fix.

## Vocabulary students actually use

| What students say | What it actually is |
|---|---|
| "section 1.4" / "module 1.4" / "lesson 1.4" | Ambiguous as of 2026-09-30. In the corpus it almost always meant the old n8n MCP Server lesson, the most-referenced section number in support, which is gone from the classroom with its whole section. The classroom now has a 1.4 in Phase 2 (First Agentic Workflow), Phase 3 (Trigger Dev) and Phase 4 (Build My First AI Lead Qualifier App). Route on the rest of the message: with n8n or MCP it is the n8n MCP connection diagnostic, with Trigger.dev it is Phase 3, and on its own ask which phase. Never send anyone back to the old lesson. |
| "claude md" / "the md file" / "claude.md" / "CLAUDE.md" / "Claude.MD" | The CLAUDE.md context file. Casing is all over the map; the skill should handle any. |
| "VS code" / "VSC" / "VS Claude Code" / "Claude in VS" | VS Code with the Claude Code extension. |
| "the extension" vs "the CLI" | Many students don't realize these are separate; they install the extension and assume `claude` works in terminal. |
| "claude design" / "Claude Cowork" / "Claude Code" / "Claude desktop" / "Claude.ai" | Five distinct products students confuse constantly. |
| "the WAT framework" / "WAT prompt" / "Nate's template" | **Workflows, Agents, Tools** — a conceptual framework for designing agentic automations. Taught in `Claude Code → Phase 2 → 1.3 The WAT Framework`. (NOT a folder structure — that's a separate community pattern; see taxonomy 7.3.) |
| "the checklist" (in an n8n MCP thread) | A setup checklist PDF that was attached to the old n8n MCP lesson and went away with it. Do not point anyone at it. Its content was the prerequisites (Node.js, an n8n API key, Homebrew on Mac), which the n8n MCP diagnostic lists directly. |
| "the health check" | The n8n MCP verification step: `/mcp` shows n8n-mcp connected, and asking Claude to list n8n workflows returns them. Output rarely matches any recording, which trips people up. |
| "Kodi" | Kodi Zene, the AIS+ AI Instructor. In n8n MCP threads, "Kodi said" or "Kodi's video" almost always refers to the old n8n MCP lesson video, which is removed from the classroom on 2026-09-30. Treat it as n8n MCP context and answer from the diagnostic; do not send them back to that video. |
| "the n8n MCP" | Almost always means czlonkowski's `n8n-mcp` (NOT n8n's native instance-level MCP — even though both share the name). |
| "the n8n skills" / "skills" | czlonkowski's `n8n-skills` repo. |
| "n8n cloud" vs "self-host" vs "hostinger" | Three hosting options that students debate constantly. |
| "the .env" | API keys / secrets file. Many students don't know what it is or where it goes. |
| "10h10s" / "Agent Zero" / "Build Your Portfolio" | Specific courses. Students use the abbreviations. |
| "MPC server" / "Cloud Code" | Letter-swap for "MCP server" and misspelling of "Claude Code". Both are extremely common — don't treat them as different products. |
| "it creates something huge" / "a massive folder" | Claude Code cloned the full czlonkowski/n8n-mcp GitHub source instead of registering the MCP server via the npx package. The clone is not wrong, and the npx registration route skips it. |
| "hundreds of pending changes in Source Control" | Git noise from the cloned n8n-mcp repo's own files. Normal, safe to ignore, and NOT the student's work to commit. Reassure before anything else. |
| "it says connected to localhost — is that ok?" | Yes. The n8n-mcp server process runs on the student's own machine. The separate "n8n connection: configured" line is the one that confirms their n8n instance is reachable. |
| "my build doesn't match the video" / "mine doesn't look like the video" | Claude Code is non-deterministic. Verify the END STATE (tools respond, `.env` exists, workflows list), never the file tree. This phrase appears constantly and almost never indicates a real fault. |
| "Visual Studio" / "VS Studio" | Almost always VS Code. But check — some students genuinely installed the full Visual Studio (purple icon); the Claude Code extension only works in VS Code (blue icon). |
| "the checkmark" | Skool's lesson-complete button, top right of each classroom lesson. Clicking it is the ONLY thing that advances course progress. |
| "the trophy" / "End Of section" | The "End Of ..." completion lessons at the end of each classroom section. They also need checkmarking for progress to reach 100%. |
| "OPAA" / "One Person AI Agency" | Not a standalone course any more — a section inside the **Scale** course after the AIS 2.0 restructure. Also sold standalone outside the membership. |
| "the savings vault" / "AI Discount Vault" | Classroom > Member Perks > $3M Savings Vault. Approval-gated, and the request must be submitted from the classroom post — registering on the perks site directly does nothing. |
| "graduated" (as in "Nate graduated n8n") | From the stack-tier video: no longer his primary content tool. NOT obsolete, NOT removed from the course. |
| "the n8n course is archived — is that a clue?" | No. The AIS 2.0 restructure archived courses for housekeeping. Archived ≠ deprecated. |
| "Skills 2.0" | Video branding for the upgraded skills system, not an installable product. What the student wants to install is the **skill-creator plugin** from the Anthropic marketplace inside Claude Code. |
| "the marketplace" / "plugins" | Claude Code's own plugin marketplace — `/plugin` in the terminal CLI, `/plugins` in the VS Code extension. Needs Claude Code 1.0.33+. Nothing to do with VS Code's Extensions sidebar. |
| "skills" (from a claude.ai user) | Often means claude.ai's no-code Skills, not Claude Code `SKILL.md` files. Clarify which product before troubleshooting. |
| "agent view" | Claude Code's multi-session dashboard. CLI-only (`claude agents` in a fresh terminal), not a VS Code panel, and needs v2.1.139+. |
| "channels" | Claude Code's preview feature for chatting with a running session from Telegram. Limited command set; permission relay only from v2.1.81. |
| "zombie processes" | Orphaned Claude Code background processes that keep running after a session ends. They compete for the single Telegram polling slot and silently drop channel messages. |
| "error code 3" | Claude Code terminating a session because the task exceeded the context window. |
| "AGENTS.md" | The cross-tool equivalent of CLAUDE.md. Codex and most non-Claude coding agents read this and silently ignore CLAUDE.md. |
| "the WAT file" / "WAT md" | The Workflows-Agents-Tools CLAUDE.md used to START a build. Distinct from the trigger.dev md, which only enters at DEPLOY time. Students grab the wrong one and Claude scaffolds a whole project. |
| "the API page URL" | The n8n REST API endpoint — what MCP **Management** tools need — as opposed to the dashboard URL that loads the human-facing UI. Pointing Management tools at the dashboard is the classic silent failure. |
| "the circle node" / "the OpenAI node under my agent" | An n8n **sub-node** holding only model configuration. The rectangular ROOT node performs the call — retry and error settings belong there, not on the circle. |
| "Set Automatically" (tool description) | n8n's default for an AI Agent tool's description. A leading cause of "my agent never uses its tools" — it needs a written description saying when the tool MUST be called. |
| "publish/unpublish vs activate/deactivate" | The same n8n on/off toggle. Older versions say Active/Deactivate, newer say Publish/Unpublish. Students think they're different things. |
| "the 0/2 next to my workflow" | n8n's Production Checklist badge (suggestions like error handling). Cosmetic. Unrelated to execution problems. |
| "pin a node / can't pin" | n8n data pinning. Only works on nodes with a single main output — never Text Classifier, IF or Switch. What looks pinned in videos is execution output. |
| "No workspace here" | The message n8n **Cloud** shows when an account has no cloud workspace. Self-hosted users land there by accident and think their instance was deleted. Their instance is untouched at their own domain/IP. |
| "publish the app" / "my token keeps getting revoked" | Google Cloud OAuth consent screen publishing status. "Testing" mode expires refresh tokens every 7 days; "In production" stops it (and is separate from verification). |
| "Kodee" | Hostinger's built-in AI assistant in the VPS panel — a product, not a person. Often the thing that finishes an n8n VPS fix. |
| "walkie-talkie tools" | Community shorthand for Claude Code's mobile surfaces (`/remote`, Dispatch, Channels). All research previews, all remote views into a session — none is a terminal replacement. |
| "Claude is getting dumber" | Usually product-layer bugs (Anthropic has published postmortems), an outdated CLI, or context rot from marathon chats — not a silent model downgrade. Check `claude --version` first. |

## Common confusions / things students conflate

1. **Two different "n8n MCP" tools**: The official n8n instance-level MCP (built into n8n, exposes workflows TO Claude) vs the community czlonkowski n8n-mcp (gives Claude knowledge to BUILD workflows). Students connecting Claude Code to build workflows want the second, and they often follow guides for the first.
2. **Claude Code CLI vs VS Code extension**: Students install the extension and assume `claude` works in their terminal. They are separate; the extension wraps the CLI.
3. **Claude Code vs Claude Chat vs Claude.ai vs Claude Cowork vs Claude Desktop vs Claude Design**: Five overlapping products with different runtimes and pricing models. Claude Design now shares the token pool with Claude Code (used to be separate).
4. **Claude Code subscription vs Claude API**: People expect a personal Max plan to power their production workflows. A personal subscription covers that person's own use, not serving a client's end users, and includes zero API usage — production belongs on API keys from console.anthropic.com, separately billed. ⚠️ Don't assert what the terms legally say or cite an enforcement date; link Anthropic's current [Usage Policy](https://www.anthropic.com/legal/aup) and [Consumer Terms](https://www.anthropic.com/legal/consumer-terms).
5. **Cloning vs the npx package, and two different meanings of "npx"**: Students clone czlonkowski/n8n-mcp expecting a small install and get massive node_modules, without knowing the server can be registered via the npx package with no clone at all. The trap is that "npx" names two different things, and a student who has heard "don't use npx" (the old n8n MCP lesson carried that warning) will otherwise reject the fix that works best:
   - **`npx <package>` run ad-hoc as an install command** is what that warning was about.
   - **Registering the MCP server so it launches via the npx package** — `claude mcp add n8n-mcp ... -- npx n8n-mcp` — is what the support team recommends in the 2026-07 threads, and is what avoids the clone entirely.
   Say the distinction out loud rather than "npx is bad" or "npx is fine". Separately, `npm install` is a third, different command: it is required inside a cloned repo and declining it guarantees failure. See the `n8n MCP setup` fix-pattern in `fix-patterns-claude-code.md` and `diagnostic-n8n-workflow-quality.md` — all three should state this the same way.
6. **MCP scope confusion**: User scope vs project scope. `/mcp` doesn't show MCPs at the wrong scope. Default `claude mcp add` writes user-scope, not what most students want.
7. **N8N_API_URL format**: A recurring silent-failure mode on self-hosted/Hostinger setups. Some tools want the bare root (`https://...your-domain.com`) and some want `/api/v1` appended, so check each field is in the form that tool expects (team-verified fix; see `fix-patterns-claude-code.md`). Caveat: current czlonkowski n8n-mcp versions normalise trailing slashes and detect an already-present `/api/v1`, so they cannot produce a doubled path — if the URL is already the bare root, the 404 is something else (wrong-place API key, or a version-specific bug) rather than a format typo.
8. **AIS vs AIS+**: AIS = free community. AIS+ = paid with classroom. A few students join the wrong one or don't realize content is locked behind AIS+.
9. **AI OS vs AIOS vs WAT framework**: Different names floating around. Some refer to the same thing, some don't.
10. **CLAUDE.md as enforcement**: Students think instructions in CLAUDE.md will prevent destructive actions in bypass mode. They won't — CLAUDE.md is advisory. Only `permissions.deny` rules in settings.json are enforcement.
11. **OneDrive/Google Drive for code sync**: Constantly suggested; constantly the wrong answer (corrupts git repos). GitHub is the right answer.
12. **`/compact` as a magic fix for context limits**: People use it aggressively, but it discards nuance. Best practice is `/clear` + handoff.md, not repeated `/compact`.
13. **Plan mode taking 2 hours**: Plan mode "spinning" is almost always too-broad prompts; not a tool bug.
14. **Skill vs MCP**: Students sometimes ask "is there a skill to give Claude credential editing in n8n?" — no, that's an MCP/API permissions question, not a skill question.
15. **Adding `:1m` model to use 1M context**: The 1M version requires "extra usage" credits enabled; many fail silently.
16. **Documentation tools vs Management tools in the n8n MCP**: Documentation tools serve static reference data and work regardless of your instance; Management tools make live REST calls. "Connected but nothing works" almost always means the URL is wrong, not the key. This split is the fastest diagnostic in the whole n8n MCP surface — use it before anything else.
17. **`.env` vs MCP registration**: Claude Code does NOT read `.env` when configuring an MCP server. Credentials must be passed at registration time (`claude mcp add ... -e KEY=value`). A perfectly correct `.env` leaves the server unregistered and invisible, which is why "my keys are right but Claude says it has no connection" is so common.
18. **Claude Code CLI vs extension vs Desktop vs Design vs Cowork vs Chat** — the product line has grown to six surfaces with different runtimes, different state, and (for Design) a now-shared usage pool. Design and Code share NO state; Design can only read from GitHub, never push.
19. **"Skills 2.0" vs the skill-creator plugin**: video branding vs the actual installable. Students search the VS Code Extensions marketplace for it and find nothing, because Claude Code plugins live in a completely separate marketplace system.
20. **Session-scoped vs persistent scheduling**: three mechanisms share the word "scheduled" — CLI/extension tasks (die with the session, expire), Claude Desktop tasks (persist via OS scheduler, but only while the machine is awake and the app is open), and Cloud tasks (persist, but run on a fresh clone with no local file access). Selling a client automation on the wrong one guarantees a broken handoff.
21. **Retry/error settings on the sub-node vs the root node**: in n8n, the circular AI model sub-node only holds configuration. Retry On Fail and On Error set there never fire. They belong on the rectangular executing root node.
22. **"Always Output Data" vs an IF node**: enabling Always Output Data on a lookup or delete node is the right fix for a dead branch. Enabling it on an IF node can cause a loop. Students apply it everywhere.
23. **Publishing an OAuth app vs verifying it**: publishing the consent screen to "In production" is instant and is what stops the 7-day refresh-token expiry. Google's app-verification review is a separate, much longer process. Conflating them makes students think they're blocked for weeks when they aren't.

## Who answers in Support Needed (and what that means for the skill)

- The **Automation Support Specialists** (Mustafa Tawfiq and Dominic Ibarra — see `community-map.md` for the full official team) watch the Support Needed category. Students never need to tag anyone specific to get help; a well-formed post gets picked up.
- **Yash Chauhan** (Community Manager) handles community/orientation matters and escalations. Billing itself goes to email, not the channel (see `community-map.md`).
- **Nate Herk** rarely answers in the support channel — his teaching is in the lessons, not in threads. Don't tell students to expect answers from him directly.
- **Ais Tech Assistant** is a bot that nudges OPs to mark posts SOLVED and add a rating.
- Many excellent answers in the corpus come from regular community members. Their fixes are folded into the `fix-patterns-*.md` files as "community-verified" entries — **the skill must not name or suggest tagging any non-team member**, no matter how strong their track record. Refer to them as "a community member" if provenance ever comes up.

## Recurring antipatterns the team and community push back on

- **"I'll let Claude run the MCP install command from chat with my API key in the message"** → NO, run terminal commands in terminal; keys belong in `.env`, not in chat history. Lesson reference: `Claude Code → Phase 3 → 1.5 Secrets Management` covers env vars, why API keys can never go to GitHub, and managing secrets across local + Trigger.dev cloud.
- **"I'll put 'don't delete files' in my CLAUDE.md to be safe"** → CLAUDE.md is advisory, not enforcement. Use `permissions.deny` in settings.json.
- **"I'll refactor the bloated repo in place"** → Claude Code preserves old code by default (trained non-destructive). For real cleanup, regen into a fresh tree from spec.
- **"Run `npm test` on PostToolUse hook"** → too slow at scale. Use `Stop` hook instead.
- **"I'll let Claude Max power my client's helpdesk"** → a personal subscription isn't the right vehicle for serving a client's users; production belongs on API keys. Community-reported that Anthropic has tightened enforcement here. Don't cite a specific date or paraphrase the terms — link Anthropic's current Usage Policy / Consumer Terms instead.
- **"I'll sync my Claude Code project via OneDrive"** → corrupts git repos. Use GitHub.
- **"I'll use `/compact` to manage context"** → support-team guidance: max once per session. Prefer `/clear` with handoff.md.
- **"I'll dump all my skills into every project"** → each loaded skill costs context; load only relevant ones.
- **"Cloudflare Enterprise blocking my scraper — I'll add more headers"** → Enterprise tier checks TLS fingerprint at handshake; headers don't help. Use Camoufox/nodriver or managed unlockers, or stop scraping that target.
- **"DM automation via n8n + Meta unofficial API"** → gets accounts banned. Use official Meta Business Partner (ManyChat) or Graph API with a proper app.
- **"I'll put the MCP credentials in `.env` and Claude Code will pick them up"** → it won't. Claude Code only learns about an MCP server through `claude mcp add` with `-e` flags. This is the single most common "everything looks right but nothing works" in the n8n MCP corpus.
- **"I'll hand Claude Code the n8n-mcp GitHub URL"** → it reads that as "download and build this project" and clones the whole source. Say instead: "add the n8n-mcp server as an MCP connection using the npx n8n-mcp package, do not clone or download anything."
- **"I'll decline `npm install` because I was warned about `npx`"** → they are different commands. `npm install` is required and declining it guarantees the setup fails. Use plan mode to review what will be installed instead of blanket-refusing.
- **"A trailing slash on the URL can't matter"** → a trailing slash (or a doubled/missing `/api/v1`) on `N8N_API_URL` breaks API calls silently. Check this character-by-character before anything else.
- **"I'll reinstall the extension"** → for a known VS Code extension regression, reinstalling pulls the same bad build. The fix is downgrading to the last working version and disabling auto-update. Same logic applies to reinstalling Homebrew when the real problem is PATH: verify with `brew --version` in a NEW terminal first.
- **"I'll upgrade Pro → Max to escape this rate-limit error"** → if the error only appears inside VS Code while the terminal CLI works fine, it's the extension's credentials bug and the error survives the upgrade. Also check for a stale `ANTHROPIC_API_KEY` env var, which silently overrides subscription login onto API billing.
- **"I'll tell Claude to wait 15 seconds between files"** → it has no internal clock. It agrees and immediately continues. Rate limits from bulk file processing are a token-volume problem: pre-extract text, batch 10-15 per FRESH session.
- **"I'll marathon one long session"** → every message reprocesses the whole conversation, so late messages can cost several times more than early ones. One task per session, `/clear` between unrelated tasks.
- **"I'll set up the client's Stripe/Supabase/n8n on my own accounts and transfer later"** → transferring after the fact is painful, AI providers require usage attributable to the business actually using it, and **n8n's terms do not permit hosting or reselling access to others on non-enterprise plans**. Have the client create those accounts from day one and invite you as admin.
- **"I'll leave the Google OAuth consent screen in Testing mode, it's just a small tool"** → refresh tokens expire every 7 days. This is why handed-off client builds "just stop working" about a week after go-live.
- **"I'll let Claude Code create the n8n credentials while it builds"** → n8n then locks those nodes as not-yours and the student can only duplicate the node. Create credentials manually in the n8n UI first and have Claude reference them by name.
- **"I'll test against the production webhook URL"** → on n8n Cloud that burns the monthly execution quota. The Test URL plus Execute workflow gives unlimited free test executions. (Inverse trap: n8n **error workflows only fire on production executions**, so those genuinely do need a production run to test.)
- **"I'll ask the AI if it's happy with its answer"** → sycophancy makes it "improve" forever regardless of quality. Ask forced-analysis questions instead: "what specific information is missing from this response?", "spot any logical weaknesses in your answer".

## Surprising findings that should shape the skill

1. **n8n MCP setup is THE setup friction point.** Roughly 15+ threads are about it, almost all from the old lesson 1.4 (n8n MCP Server), which is removed from the classroom on 2026-09-30. No surviving lesson sets it up, so the diagnostic is now the only guide a member gets, and it has to work without a lesson behind it. A flowchart approach: ask cloud vs hostinger vs free trial vs corporate proxy first.

2. **VS Code extension regressions happen every few weeks.** When students report "Claude suddenly broke", the answer is almost always "roll back the VS Code extension to the previous version + disable auto-update". The skill should know this is a recurring pattern, not a one-time bug.

3. **[SOLVED] titles are official practice.** The `START HERE → Getting Help with Automations` lesson explicitly asks students to retitle solved posts with [SOLVED] — that's why so many titles carry it, and it's why searching for [SOLVED] threads is a legitimate first move. When the skill drafts a Support Needed post, remind the student to mark it [SOLVED] once resolved.

4. **Students who can't get the n8n MCP connected can keep learning without it.** In the corpus, several skipped the old lesson 1.4 and moved on, and the support team endorsed it publicly ("you can bypass n8n if needed"). That escape hatch still holds, since no lesson in Claude Code Phases 1 to 4 uses the n8n MCP. The skill should say "you don't need this connection to keep going with the course", never "skip to section 2" (that section is gone too).

5. **The course shuffled its module structure recently.** Claude Code lessons live in the **Claude Code** course, by phase: Phase 1: AI Operating System (lessons numbered 1 to 15), then Phases 2, 3 and 4 (numbered 1.1, 1.2 and so on, so the same number repeats across phases). Community-floating references that say "Claude Code material moved to Build Your Portfolio" are stale: Build Your Portfolio is a separate course with its own intro/Agent Zero/portfolio content. Some assets only exist in the Archived classroom. The skill needs to handle "I can't find lesson X" gracefully, and the answer is usually "check the Claude Code course first (by phase), then Archived".

6. **Two-product confusion (`czlonkowski/n8n-mcp` vs n8n native instance-level MCP) is poorly explained anywhere.** The cleanest framing in the corpus: **one EXPOSES your workflows to Claude, the other teaches Claude to BUILD workflows**. The skill should lead with this distinction whenever n8n+MCP comes up.

7. **Claude Code is non-deterministic** — students panic when their setup doesn't look like the video. The skill should normalize "your output won't match the video, that's fine, what matters is the end state". The end-state check is `/mcp` shows n8n-mcp as connected, and asking it to list n8n workflows actually works.

8. **The `.env` file confuses non-technical students badly.** Many come from Make/Zapier and have never edited a config file. The skill needs an ELI5 path: "open Claude and say 'create an empty .env file', then add your secrets in the format `KEY=value`".

9. **Hostinger + n8n-mcp has team-maintained workaround knowledge** that isn't in any official doc — the 404 / "can't find workflows" bug and its `/api/v1` base-URL fix are captured in `fix-patterns-claude-code.md`. If a Hostinger case doesn't match the captured patterns, the Support Needed channel is the right escalation; the support team knows this terrain.

10. **The `.claude` folder username path matters** for cross-machine sync — when copying `.claude` between machines, the project subfolder name includes the username and must be renamed. This is in zero official documentation.

11. **Plan mode + too-broad prompts = 2 hours spinning** — plan mode itself isn't broken; students just give it too-broad goals.

12. **Skills are markdown — the structure IS the lesson.** Community consensus: stop hunting for the perfect pre-made skill; let Claude build skills for your specific workflow. The skill should encourage skill-authoring over skill-collecting.

13. **Course/access questions are now the single largest category (74 threads), and business/client questions the second (56).** The v1 skill was built as a Claude-Code-and-n8n troubleshooter; the majority of incoming volume is now navigation ("where is X", "when does Y unlock", "what do I charge"). Escape Hatch A ("that's a business question, go to Agency Talk") is no longer sufficient on its own — the skill should give the substantive answer the threads actually contain, THEN route for follow-up discussion.

14. **"My build doesn't match the video" is a category, not a bug.** Across many threads the answer is identical: Claude Code is non-deterministic, verify the end state rather than the file tree. Students arrive convinced their setup is broken. Lead with normalisation before any diagnostic step.

15. **The AIS 2.0 restructure invalidated a large slice of the v1 location knowledge.** OPAA and Subs to Sales became sections inside Scale; YouTube resources moved into the **Nate Herk - Video Database** lesson (a single lesson at Community Resources top level, not a section — a student told to look for a "module" will hunt for a collapsible section that doesn't exist); templates and agent skills consolidated under Community Resources; the WAT CLAUDE.md lives on a community post, not in a lesson. Any location the skill states should be framed as "check here first" rather than asserted as permanent — this has now moved twice.

16. **Provenance quality varies more than v1 assumed, and the skill should say so.** Several high-value patterns end with the poster never confirming the fix, or the team offering a hedged best guess. Those entries carry explicit "reported, not confirmed" language in the `fix-patterns-*.md` files. Preserve it when answering — telling a student "this is the team's suggestion but nobody confirmed it worked" is more useful than false confidence, and it is what the corpus actually supports.
