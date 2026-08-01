# Fix Patterns — Course navigation, access & content

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

**Read `diagnostics/course-access-navigation.md` first.** That diagnostic is the routed entry point for the "where is X / it's locked / progress stuck" family and carries the decision tree. This file is the long-form backing detail behind it — come here for a specific pattern the diagnostic points at, not as the first stop.

---

## COURSE / CONTENT

## Can't find md files / lesson moved after classroom restructure
**Symptom**: "Where are the assets from 'Every Level of Claude Code Explained in 21 Minutes'?"

**Root cause**: The AIS 2.0 restructure moved content. YouTube video resources were removed from video descriptions and centralized in the **Nate Herk - Video Database** lesson (a lesson at Community Resources top level, not a section); templates and agent skills were consolidated under Community Resources; some older content moved to the Archived course.

**Fix steps**:
1. Look in: Claude Code → Phase 1 (1.1-1.8 — n8n MCP setup track) and Phase 2 (agentic workflows, MCP servers, skills). Most numbered Claude Code lessons live in these two phases.
2. Some video templates only live in the Archived classroom (e.g., AI Voice Receptionist Vapi template).
3. The confirmed template resource: the **"Pre-Flight Setup Checklist" PDF** attached to lesson 1.4 (the lesson text itself references it). A **"Tips & Best Practices" PDF** on `Claude Code → Phase 1 → 1.1 INTRODUCTION` is reported in-thread but not confirmed against the lesson — tell the student to check that lesson's resources section rather than promising it's there. Beyond those, point students at the actual lesson — drive folder URLs floated in the community have not been verifiable against current lesson content.
4. **WAT framework**: `Claude Code → Phase 2 → 1.3 The WAT Framework`. WAT = **Workflows, Agents, Tools** — a conceptual framework for agentic automations (not a folder structure). This is the canonical reference now; don't point students at the older YouTube Resources post.
5. **Permanent fix**: students should keep their personal CLAUDE.md templates and custom skills in their own GitHub gist or private repo so restructures don't kill access.
- **YouTube video resources** (skills folders, CLAUDE.md files): Classroom > Community Resources > 'Nate Herk - Video Database'.
- **Free templates from a specific Nate Herk YouTube video**: search the community feed for the video title prefixed 'New Video: ...' — the resources are attached to that post. Some of these posts live in the free AI Automation Society community rather than AIS+, so check both.
- **n8n templates**: Classroom > Community Resources > n8n Templates. Some older templates may have moved to the Archived course.
- **Agent skills** (Skill Builder, Excalidraw Diagrams, Excalidraw Style Images, Nano Banana 2, Nate's Frontend Design, Video to Website): Classroom > Community Resources > Agent Skills. Each comes with its SKILL.md and install steps. **Community Shared Skills** is a separate lesson at Community Resources top level, not inside the Agent Skills section.
- **The WAT-framework CLAUDE.md**: it is attached to Nate Herk's 'Master 95% of Claude Code in 36 Mins' community post. The classroom lesson that teaches the framework is `Claude Code → Phase 2 → 1.3 The WAT Framework`, but the downloadable file itself lives on that post. If the download link does not work on mobile, long-press the file and choose the download option, or copy the text into a new file and save it with a `.md` extension.
- **Missing or broken lesson resources** (puzzle files, presentation slides, 404 resource links, permission-locked Google Sheets): post it in Support Needed‼️. The team re-attaches files and repairs broken links directly — several of these were fixed inside the thread.

**Confidence**: high — team-verified across 15 threads

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/cant-find-claude-md-for-wat-framework
- https://www.skool.com/ai-automation-society-plus/documents-missing-from-mastering-claude-code
- https://www.skool.com/ai-automation-society-plus/where-are-the-md-files-discussed-in-every-level-of-claude-code-explained-in-21-minutes
- https://www.skool.com/ai-automation-society-plus/i-cannot-find-the-resources-from-the-yt-videos
- https://www.skool.com/ai-automation-society-plus/where-are-the-receptionist-templates
- https://www.skool.com/ai-automation-society-plus/where-can-i-find-this-workflow-template
- https://www.skool.com/ai-automation-society-plus/n8n-templates-2
- https://www.skool.com/ai-automation-society-plus/where-do-i-find-agent-skills-classroom
- https://www.skool.com/ai-automation-society-plus/excalidraw-skills
- https://www.skool.com/ai-automation-society-plus/wat-md
- https://www.skool.com/ai-automation-society-plus/watmd
- https://www.skool.com/ai-automation-society-plus/where-did-subs-to-sales-course-go
- https://www.skool.com/ai-automation-society-plus/n8n-masterclass-puzzles
- https://www.skool.com/ai-automation-society-plus/presentation-slides
- https://www.skool.com/ai-automation-society-plus/issue-with-classroom-resources-link
- https://www.skool.com/ai-automation-society-plus/need-help-669c1785
- https://www.skool.com/ai-automation-society-plus/agent-skills-ressources
- https://www.skool.com/ai-automation-society-plus/22-masterclass-starting-the-convo

---

## How to cancel AIS+ subscription
**Symptom**: User wants to cancel, Skool support not responding. Also covers: members who joined via a discount/promo link and find no cancel option at all in their Skool profile, and unresolved billing errors such as double charges.

**Root cause**: Skool itself handles billing, but the community has internal contacts. Promo-link subscriptions are not always cancellable from the Skool profile UI, so the self-serve route silently does not exist for those members.

**Fix steps**:
1. **Email the official billing channel — nate@aiautomationsociety.ai** (`START HERE → Help With Billing & Your Account`). This is the documented route for charges, invoices, failed payments, plan changes, refunds and cancellations, including promo-link subscriptions with no self-serve cancel option. Include the email tied to the AIS+ account, a brief description, and screenshots. Response within ~24 hours on business days.
2. Try Settings → Subscriptions on Skool first if the student hasn't — self-serve works for most standard subscriptions.
3. If Skool's own billing system is the blocker, contact Skool support directly through their help system.
4. **Get it in writing.** A member who arranged a cancellation by DM had it confirmed and was then charged again — the email route leaves a paper trail. ⚠️ Do not route billing to a named individual; the skill's naming policy forbids it and the email is the documented channel. For account access/entitlements (not billing), the **AIS Support** account on Skool is the right destination.
- For general account access problems (locked classrooms, entitlements) rather than billing, message the **AIS Support** account on Skool. That account does not handle technical inquiries.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/cancellation-help
- https://www.skool.com/ai-automation-society-plus/auto-payments-help
- https://www.skool.com/ai-automation-society-plus/subscription-from-january-discount-how-to-cancel
- https://www.skool.com/ai-automation-society-plus/program-discount-67

---

## Your build doesn't match the lesson video — Claude Code is non-deterministic; verify function, not structure
**Symptom**: The file tree looks nothing like the lesson screenshots; the AI plan produced one workflow where the demo shows two (no vector store); MCP verification output differs from the video — students assume a broken setup.

**Root cause**: Claude Code is non-deterministic: it organizes files and picks architectures on the fly, so the exact commands, folder layout and output will rarely match a recording step for step. Separately, a small static knowledge set legitimately gets inlined into the agent's prompt instead of a vector store. Structural mismatch is not a failure.

**Fix steps**:
1. Verify the end state, not the file tree: the MCP tools respond when called, .env exists with the n8n instance URL and API key, Claude Code can list and edit your workflows, and the required behaviors pass the lesson's tests.
2. If the architecture differs (e.g. policies inlined into the agent instead of a separate Knowledge Base Indexer workflow), check it meets the requirements first. Only ask Claude Code to build the vector-store variant if you specifically want to match the lesson.
3. Rule of thumb given by the team: inline knowledge is fine when the documents are small and static; a vector store starts to matter when they are large or change often.
4. Quick way to tell which build you got: open the support agent and see whether the policy text sits inside the agent's prompt, or whether there is a vector store node with nothing loaded into it.

**Lesson reference**: `Claude Code → Phase 1: Setup & First n8n Workflow → 1.6 Verify All Tools Connected`; lessons 2.2/2.3 (agent + Knowledge Base Indexer) — verify the 2.x titles against the classroom

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/16-verify-all-tools-are-connected
- https://www.skool.com/ai-automation-society-plus/module-22-my-result-from-ai-plan-is-entirely-different-from-guide
- https://www.skool.com/ai-automation-society-plus/something-is-missing
- https://www.skool.com/ai-automation-society-plus/claude-code-mcp-doesnt-look-like-the-video

<!-- pattern: /16-verify-all-tools-are-connected -->

---

## Screen doesn't match the videos — Visual Studio vs VS Code, CLI vs extension, wrong CLAUDE.md
**Symptom**: The interface looks nothing like the one in Nate Herk's videos; or Claude Code immediately scaffolds a whole Node/Trigger.dev project with GitHub wiring that the course never mentioned at that point.

**Root cause**: Three separate mixups, all confirmed by the team: (1) the full Visual Studio (purple icon) was installed instead of Visual Studio Code (blue icon) — the Claude Code extension only works in VS Code; (2) typing 'claude' in the bottom terminal launches the CLI, which is a completely different interface from the extension panel the videos use; (3) the Trigger.dev CLAUDE.md was grabbed instead of the WAT CLAUDE.md — a CLAUDE.md carries setup instructions Claude Code executes immediately, so the wrong one scaffolds the wrong project.

**Fix steps**:
1. Confirm you installed Visual Studio Code (blue icon), not Visual Studio (purple icon). They are different products.
2. In VS Code open the Extensions panel (the four-squares icon on the left), search 'Claude Code', install the Anthropic one, and sign in with your Claude subscription. A Claude icon appears in the left sidebar; clicking it gives the two-panel layout from the videos.
3. Know that typing 'claude' in the terminal at the bottom of VS Code starts the CLI instead — it works, but nothing will line up with the video.
4. Claude Desktop is a third, separate app. It is simpler to start with, but the course steps will not match it one-to-one.
5. Use the WAT CLAUDE.md, not the Trigger.dev one. The downloadable file is attached to Nate Herk's 'Master 95% of Claude Code in 36 Mins' community post — check there first (the classroom lesson that teaches the framework is `Claude Code → Phase 2 → 1.3 The WAT Framework`, but the file itself lives on the post). Do not look for a "Classroom > Claude Code > CLAUDE.md Files" folder; that path no longer exists and links pointing there are stale. Don't drop any CLAUDE.md into a project until the course tells you to.
6. If the wrong CLAUDE.md already scaffolded a project, don't try to unpick it: open a fresh empty folder and restart from where the course starts.

**Lesson reference**: 10-hour Claude Code course 'Getting Set Up' section; the WAT framework is taught in `Claude Code → Phase 2 → 1.3 The WAT Framework` (the CLAUDE.md file itself is attached to the 'Master 95% of Claude Code in 36 Mins' community post)

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/re-claude-code-skills
- https://www.skool.com/ai-automation-society-plus/support-needed-2755b332

<!-- pattern: /re-claude-code-skills -->

---
## COURSE NAVIGATION & ACCESS

## OPAA, Scale and Get Your First Clients — where they live and when they unlock
**Symptom**: Members cannot find One Person AI Agency (OPAA), Subs to Sales, or business-formation content in the classroom, or want to know what the locked Get Your First Clients and Scale folders contain before deciding to upgrade.

**Root cause**: The AIS 2.0 restructure folded OPAA and Subs to Sales into the Scale course as sections rather than leaving them as standalone courses, so searching the classroom for their old names finds nothing. Scale and Get Your First Clients are also time-gated, so newer members may not see them at all.

**Fix steps**:
1. OPAA is a section inside the Scale course ('One Person AI Automation Agency'), not a standalone course. Subs to Sales is likewise a section inside Scale.
2. Unlock schedule: Get Your First Clients opens at 1 month of membership, Scale at 90 days — and time you have already been a member counts toward it. Annual and Premium members get both immediately.
3. OPAA is also sold as a standalone course outside the membership, for anyone who wants only that content.
4. Get Your First Clients is short and practical, around 17 lessons: why the first client is the hardest, the ways to get clients, warm versus cold outreach, building your outreach list, what to say in DMs, getting people onto discovery calls, and a full Upwork section covering profile optimisation, finding high-ROI jobs, a proposal framework, and submitting your first proposals. (One team member also described a Claude skill for drafting Upwork proposals; that could not be confirmed against the classroom listing, so treat it as reported rather than certain.)
5. Scale is the large one, around 84 lessons, covering the business behind an agency: mindset, the AI opportunity, business formation (LLC, banking and taxes, contracts and legal protection, invoicing), niche selection, online presence and branding, packaging and pricing (pricing models, the ROI formula, anchoring, retainers), outbound systems including a cold email case study, discovery and sales, client onboarding and QA, sustainable lead flow, and a section on finding constraints and hiring.
6. If you want to start building right now regardless of gating, the build curriculum including the Claude Code walkthrough is 'Build Your Portfolio', which is open from day one.

**Lesson reference**: Scale module contents and unlock schedule per Nate Herk's 'AIS+ 2.0' announcement post — verify counts/gating against the live classroom

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/opaa
- https://www.skool.com/ai-automation-society-plus/opaa-access
- https://www.skool.com/ai-automation-society-plus/access-to-one-person-ai-agency-not-recieved
- https://www.skool.com/ai-automation-society-plus/accessing-one-person-ai-automation-agency
- https://www.skool.com/ai-automation-society-plus/aspiring-ai-implementation-consultant
- https://www.skool.com/ai-automation-society-plus/seeking-curriculum-details-for-get-your-first-clients-and-scale

<!-- pattern: /opaa -->

---

## Skool course progress stuck below 100% — lessons, sections, AND 'End Of' trophy pages need checkmarks
**Symptom**: Progress percentage won't move (or sticks at e.g. 89%) despite watching everything; cache clearing and reboots don't help.

**Root cause**: Skool never auto-completes lessons; the completion checkmark must be clicked — including on chapter/section items and the 'End Of ...' trophy lessons; restructures can leave whole sections unvisited.

**Fix steps**:
1. Click the checkmark at the top right of every finished lesson.
2. Also open and checkmark each section header and every 'End Of ...' trophy lesson.
3. Scan the outline for entire sections you never opened after a classroom restructure.
4. If an item looks checked but progress won't move, uncheck and re-check to force a refresh.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/course-progress-not-updating-after-troubleshooting
- https://www.skool.com/ai-automation-society-plus/portfolio-stuck-at-89

<!-- pattern: /course-progress-not-updating-after-troubleshooting -->

---

## Learning path — Agent Zero fundamentals first, then n8n and Claude Code (two valid orders)
**Symptom**: New members are overwhelmed, paralyzed by 'n8n is dead' clickbait, or unsure whether to finish the n8n masterclass before starting Claude Code.

**Root cause**: Tool-churn anxiety plus curriculum growth. Fundamentals (LLMs, APIs, JSON, triggers, data passing, webhooks, error handling) transfer across tools and are what let you direct — and debug — an agent.

**Fix steps**:
1. Treat 'X is dead' video titles as engagement bait; pick one orchestrator and commit rather than tool-hopping.
2. Start with the Agent Zero module for fundamentals, then '10 Hours to 10 Seconds V2'. Both are in the Build Your Portfolio course.
3. From there the team has endorsed two orders, both fine: (a) build a handful of real n8n workflows first so triggers, data passing, webhooks and error handling click, then layer Claude Code on top; or (b) go straight into Claude Code after Agent Zero and pick up n8n as a complement. Members with a coding background are usually pointed at (b); complete beginners at (a). Running both in parallel also works.
4. Whichever order, build real projects from day one and prompt Claude Code to explain every step and decision as it goes — a team member's stated fastest-learning technique.
5. Nate Herk's free YouTube course 'Build & Sell with Claude Code (10+ Hour Course)' is the usual starting point for the Claude Code side.

**Lesson reference**: Agent Zero module, '10 hours to 10 seconds', and 'Build & Sell with Claude Code (10+ Hour Course)'

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/i-need-help-where-to-start
- https://www.skool.com/ai-automation-society-plus/course-workflow
- https://www.skool.com/ai-automation-society-plus/beginner-here-anxious-to-take-the-right-direction
- https://www.skool.com/ai-automation-society-plus/having-4-questions-for-immersed-people-in-ai-automations
- https://www.skool.com/ai-automation-society-plus/claude-code-classroom
- https://www.skool.com/ai-automation-society-plus/im-lost-help

<!-- pattern: /i-need-help-where-to-start -->

---

## "Is n8n dead / graduated?" — the standing answer
**Symptom**: Recurring question after Nate Herk's stack-tier video placed n8n in his 'graduated' tier and the classroom went Claude Code-first (n8n masterclass in Archived).

**Root cause**: 'Graduated' means no longer his primary content tool, not obsolete; n8n remains the 24/7 runtime layer and stays in enterprise/client demand; Archived is housekeeping, not deprecation.

**Fix steps**:
1. Point to Nate Herk's 'Is n8n Dead?' YouTube video as the canonical answer.
2. Mental model: n8n is the always-on runtime; Claude Code is the senior dev that builds/modifies workflows (via the n8n MCP) — complements, not competitors.
3. The full n8n masterclass lives in the Archived classroom module (Module 1 Prerequisite).
4. For clients requesting 'n8n', deliver n8n built faster with Claude Code — a competitive advantage, not an argument.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/is-it-still-worth-it-to-learn-n8n
- https://www.skool.com/ai-automation-society-plus/is-n8n-still-recommended-or-is-it-graduated
- https://www.skool.com/ai-automation-society-plus/question-about-the-learning-path-claude-code-vs-n8n

<!-- pattern: /is-it-still-worth-it-to-learn-n8n -->

---

## Where to learn building websites/apps (login, payments) — Building Frontends lessons
**Symptom**: Students want to build a business website with secure login and a webshop, or a customer-facing app, and can't find where the course teaches it.

**Root cause**: The content lives in the Claude Code module's app-building section (Building Frontends lessons), easy to miss.

**Fix steps**:
1. Classroom > Claude Code module > app-building section, starting at '#1.1 Intro: Building Frontends' (covers real site design, login systems, and Stripe payments).
2. Go through the Claude Code module from the start first, then describe your specific business to Claude Code and let it build.

**Lesson reference**: '#1.1 Intro: Building Frontends' and following lessons in the Claude Code module

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/hi-f87ff14c
- https://www.skool.com/ai-automation-society-plus/help-building-an-application-with-claude-code

<!-- pattern: /hi-f87ff14c -->

---

## Following the course with Codex — AGENTS.md swap and what Codex is
**Symptom**: Member on a ChatGPT/Codex plan asks if they can follow the Claude Code course; or creates CLAUDE.md on Codex and nothing happens.

**Root cause**: Codex is OpenAI's Claude Code counterpart; it reads AGENTS.md, not CLAUDE.md (which it silently ignores), and asks fewer permission questions by default.

**Fix steps**:
1. Wherever a lesson says create CLAUDE.md, create AGENTS.md (run /init in Codex to scaffold it); contents are identical markdown.
2. Run /plan before builds and add an ask-before-edit line to AGENTS.md to match the videos' interactivity.
3. Running both tools side by side: keep one AGENTS.md and reference it from CLAUDE.md with '@AGENTS.md' (or a symlink).
4. Any paid ChatGPT tier includes Codex, as Claude Pro includes Claude Code; skills/subagents/plugins all exist in Codex too. To conserve credits on either: stay in plan mode as long as possible.
5. For dedicated Codex learning, the team points to Nate Herk's one-hour Codex course post.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/will-codex-do-as-claude-code-in-the-course
- https://www.skool.com/ai-automation-society-plus/codex-question
- https://www.skool.com/ai-automation-society-plus/i-need-some-advice-where-should-i-learn-about-codex

<!-- pattern: /will-codex-do-as-claude-code-in-the-course -->

---

## Where AIS Live, seminar and weekly call recordings land, and when
**Symptom**: Members who missed AIS Live, a weekend seminar, or a weekly live call cannot find the recording or the session materials afterwards — or one session is missing from an otherwise complete drop.

**Root cause**: Recordings are edited after the event rather than published live, and the drop is announced by email to the address on your account. Individual sessions can lag further when the live recording itself had problems. The weekly community call is handled differently again — it gets a community recap post rather than an email drop.

**Fix steps**:
1. Give it a day or two. Recordings and the accompanying session materials are put together after the event, and you will get an email at the address on your Skool/registration account when they go live.
2. Also check the Live Call Recordings classroom, which is where recorded calls are collected.
3. If one session is missing from an otherwise complete drop, it most likely had technical problems during the live event (a dropped connection is the confirmed cause in one case) and is being re-recorded. Ask in Support Needed for status rather than assuming it was skipped.
4. For the weekly AI & Chill call, search the community feed for the 'AI and Chill recap' post — a community member publishes the recording and a tool guide each Monday or Tuesday. Follow that poster to get notified.
5. If nothing has arrived after a couple of days, post in the community to flag it — every one of these cases was resolved that way.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/ais-live-and-the-materials
- https://www.skool.com/ai-automation-society-plus/ais-live-event
- https://www.skool.com/ai-automation-society-plus/ais-recordings
- https://www.skool.com/ai-automation-society-plus/ais-live-vip-recordings
- https://www.skool.com/ai-automation-society-plus/recording-from-this-weekend-seminar
- https://www.skool.com/ai-automation-society-plus/last-week-chill-chat

<!-- pattern: /ais-live-and-the-materials -->

---

## How AIS+ support works — what you get, response times, and who to contact
**Symptom**: Members ask what AIS+ adds over the free community, expect one-to-one screen-share or welcome calls, wonder how long support takes to reply, or do not know who to contact for account and billing matters.

**Root cause**: Support is thread-based rather than scheduled, and the entry points are not obvious from the Skool UI — technical help, membership help and account access each go to a different place.

**Fix steps**:
1. For technical help, make a post in the Support Needed category and tag the support team. Describe the specific blocker — what you are trying to do, what is happening instead, and what you have already tried. This is the route with the highest resolution rate.
2. Response time is typically within 24 hours, though volume and time zones can stretch it. This figure comes from the team directly.
3. There are no one-to-one screen-share sessions. The team's stated alternative is to post the specific thing you are stuck on and they work it with you in the thread, which they note moves faster than it sounds once the question is specific.
4. There are also live tech support calls hosted by Nate Herk — check the community calendar for timings.
5. For membership, billing, cancellations and anything non-technical, DM Yash Chauhan.
6. For account access and entitlements — locked classrooms, perk approvals — message the AIS Support account on Skool. That account explicitly does not handle technical inquiries.
7. Other AIS+ benefits named by the team: unlimited tech support, a structured learning path through the classroom, extra templates and build walkthroughs beyond what the free community gets, live Q&A calls with Nate Herk and community calls (both recorded), special events, and the member network itself.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/benefits-or-difference-between-plus-and-free
- https://www.skool.com/ai-automation-society-plus/review-set-up-support-needs
- https://www.skool.com/ai-automation-society-plus/support-response-time
- https://www.skool.com/ai-automation-society-plus/admin-contact
- https://www.skool.com/ai-automation-society-plus/locked-out

<!-- pattern: /benefits-or-difference-between-plus-and-free -->

---

## Course modules that call the Anthropic API need API credits — Pro/Max doesn't cover them
**Symptom**: The Phase 3 Scheduled Research Agent module fails with low-credit errors; students assume their Claude Pro/Max subscription covers it.

**Root cause**: That module's agent calls the Anthropic API directly, which is billed separately through the Anthropic Console. Consumer subscriptions (Pro/Max) include zero API usage, so the agent has no credits to spend.

**Fix steps**:
1. Top up a small amount at console.anthropic.com under Billing — the team's guidance was that $5 is plenty to complete the module (verify current minimums on the billing page).
2. No-payment alternative: follow the same lesson but substitute the Gemini API, which still has a free tier. Create the key at aistudio.google.com. ⚠️ Per-model daily limits change often and are shown per-project in AI Studio — check there rather than quoting a fixed number and tell Claude to use Gemini instead of the Anthropic API when prompting.

**Lesson reference**: `Claude Code → Phase 3: Hosting & Deployment → 1.6 Masterclass - Scheduled Research Agent` (give the phase — the Claude Code course has three different lessons numbered 1.6)

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/issue-with-module-16-scheduled-research-agent-api-credits

<!-- pattern: /issue-with-module-16-scheduled-research-agent-api-credits -->

---

## Secret perks / AI Discount Vault access stuck on pending
**Symptom**: Registering directly on the AIS perks site (ai-automation-society.joinsecret.com) does nothing, or the signup sits on 'pending' indefinitely even after registering several times.

**Root cause**: Access is approval-gated on the AIS+ side. Signing up on the perks site directly does not create a request the team can see or approve — the request has to be submitted from the dedicated classroom post. The team periodically clears the pending queue, so a member who is still locked out after that has almost certainly not submitted a request through the right route.

**Fix steps**:
1. Submit the membership request from the classroom: Classroom > Member Perks > $3M Savings Vault > 'How to Unlock Your AI Discount Vault'. Follow the request instructions on that post rather than registering on the perks site alone.
2. Wait for a team member to approve it. Approvals are done in batches.
3. If access still does not appear after that, DM the AIS Support account on Skool to have the approval queue checked.
4. Note: neither source thread ended with a confirmed resolution, so treat this route as the mechanism the team described rather than a verified fix.

**Confidence**: low — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/ai-discount-vault-approval
- https://www.skool.com/ai-automation-society-plus/credits-for-additional-cloud-accounts

<!-- pattern: /ai-discount-vault-approval -->

---

## Finding the member discounts for Hostinger and self-hosted n8n
**Symptom**: Students hear about a member discount on Hostinger or self-hosted n8n in a YouTube video and cannot find where it lives in the community.

**Root cause**: Discounts are split across two places and neither is where students look first: a general partner-deal section in Community Resources, and a separate annual-member perks vault. Some codes also circulate only in threads.

**Fix steps**:
1. Check Classroom > Community Resources > Discount Codes first — it has a dedicated Hostinger VPS entry along with other partner deals, and it is not membership-tier gated.
2. Also check Classroom > Member Perks > $3M Savings Vault > 'Unlock $3M in AI Tools'. This is the larger deals collection and it is tied to annual membership, so if you are on monthly and it appears locked, that is the gating rather than a bug.
3. A community discount code for Hostinger, NATEH15 for 15% off the annual plan, was mentioned by the support team in a VPS thread. It was never confirmed working by anyone in-thread and codes expire, so treat it as worth trying rather than guaranteed.
4. Separately, when buying a Hostinger VPS: the advertised low price applies only to the first term paid upfront and roughly doubles on renewal, so choose the longest term you are comfortable with. This point was acted on by a member in-thread.
5. Note: in both discount threads the support team's answer was a hedged best guess and the students never replied to confirm, so verify the location rather than assuming.

**Confidence**: low — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/n8n-discount
- https://www.skool.com/ai-automation-society-plus/n8n-discount-code-2
- https://www.skool.com/ai-automation-society-plus/vps-recommendation

<!-- pattern: /n8n-discount -->

---

## Classroom videos will not play or skip chunks of the timeline
**Symptom**: Classroom videos fail to load at all, or the timeline does not load fully and the player skips past the gaps during playback. Incognito mode makes no difference.

**Root cause**: Two unrelated causes. Classroom videos are Loom-hosted, so a platform-wide Loom outage takes many videos down at once and there is nothing to fix on your side. Separately, a Chrome-specific playback problem produces the timeline gaps — sometimes caused by hardware acceleration.

**Fix steps**:
1. If many videos are failing at once and other members are reporting the same thing, it is a Loom outage. Refresh and come back later. Repeated refreshes sometimes get an individual video to load in the meantime.
2. Incognito mode does not help with the timeline-gap problem — do not spend time on it. Clearing cache is worth one try.
3. For timeline gaps in Chrome, open the same video in Firefox or Edge. This is the confirmed fix — a student with gaps on every video reported no problems at all after switching, including jumping around and refreshing.
4. If you want to stay in Chrome, go to Settings, search 'hardware acceleration', turn off 'Use hardware acceleration when available', and restart Chrome. This was suggested by the support team but was never tested by the student, so treat it as unverified.
5. If exactly one video fails in every browser while everything else plays, the problem is that specific video file — report it to the support team.

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/agent-zero-not-working
- https://www.skool.com/ai-automation-society-plus/loom-video-loading-problem

<!-- pattern: /agent-zero-not-working -->

---

## Getting text transcripts out of classroom videos
**Symptom**: Students want text transcripts of Masterclass or classroom videos to feed into NotebookLM or an AI helper, but 'Save Video As' and link copying are restricted in the classroom player, and YouTube transcript tools do not work because these are not YouTube videos.

**Root cause**: Classroom videos are Loom-hosted and the embedded classroom player restricts downloading and link copying. Loom's own player does expose a transcript panel, but you have to get to it through the player UI.

**Fix steps**:
1. Put the lesson video into full screen first.
2. Pause the video. A 'Get Loom for free' popup appears — click it.
3. Close the login window that opens. The transcript panel appears on the right-hand side, with a Copy button.
4. This works one video at a time; there is no batch export.
5. This route depends entirely on Loom's player UI and may break if Loom changes it. A fallback raised in-thread: play the video (you can increase playback speed) and capture the audio with a transcription tool, then use the resulting text.
6. Note: these steps were provided by the support team but the student never confirmed back that they worked, so expect to adapt if the UI has shifted.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/request-does-anyone-have-the-masterclass-transcripts-for-notebooklm

<!-- pattern: /request-does-anyone-have-the-masterclass-transcripts-for-notebooklm -->

---

## Scam/phishing DMs targeting community members
**Symptom**: Member receives an unexpected DM/notification (prizes, offers) connected to the community and asks if it's real.

**Root cause**: A recurring scam campaign targets members; the official team does not solicit via unsolicited DMs.

**Fix steps**:
1. Do not reply, click links, or provide information.
2. Post in Support Needed to verify — the team confirms known scams quickly; then report/block the sender.

**Confidence**: low — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/receive-notification

<!-- pattern: /receive-notification -->

---
