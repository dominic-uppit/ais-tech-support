# Diagnostic — Course Access & Classroom Navigation

Use this when a student can't find something in the classroom, can't access a course, can't get their progress to move, or doesn't know what to do next in the curriculum. Trigger phrases: "where is X", "I can't find the template", "the course is locked", "which course should I do next", "my progress is stuck at 89%", "where are the recordings", "is n8n dead", "AIS vs AIS+".

**This is now the highest-volume category in the corpus — 74 threads (47 of them newly ingested).** It overtook n8n MCP setup. Treat it as a first-class diagnostic, not an afterthought.

**Lead with this**: the classroom was restructured (the "AIS 2.0" reorganisation) and a large share of these questions are the restructure, not the student. Content moved; some of it moved twice. **State locations as "check here first", never as permanent fact** — and if the student reports the location is wrong, believe them and route to Support Needed‼️ rather than insisting.

**Second thing to know**: Skool never auto-completes lessons. A startling number of "my access is broken" reports are actually "I didn't click the checkmark."

---

## Step 1 — Triage: which of five questions is this?

Read the transcript first. Almost every thread in this category is one of five:

| What they're asking | Go to |
|---|---|
| "I can't FIND a file/template/resource/lesson" | **Step 2** — the resource-location map |
| "I can't ACCESS a course / it's locked" | **Step 3** — the gating rules |
| "My PROGRESS won't move / stuck at N%" | **Step 4** — the checkmark mechanics |
| "What should I do NEXT / where do I start" | **Step 5** — the learning-path decision tree |
| "The video won't PLAY / I want a transcript" | **Step 6** — playback and transcripts |

If it's "my build doesn't match the video", that is **not** this diagnostic — that's `diagnostics/lesson-1-4-n8n-mcp.md` or the non-determinism pattern in `knowledge/fix-patterns-course-access.md` ("Your build doesn't match the lesson video").

If it's "where do I POST" or "how do I contact billing", that's `knowledge/community-map.md`.

---

## Step 2 — "I can't find X" — the resource-location map

Ask ONE clarifying question if you can't tell what X is: **"Is it a template, a CLAUDE.md file, an agent skill, a lesson, or a recording?"** Then answer from the matching branch. Don't dump the whole map.

### 2a — Resources from one of Nate Herk's YouTube videos

Two places, in this order:
1. **Classroom > Community Resources > "Nate Herk - Video Database"** — this is where video resources (skills folders, CLAUDE.md files) were centralised when they were removed from the video descriptions.
2. **Search the community feed for the video title prefixed `New Video: ...`** — free templates from a specific video are attached to that announcement post. **Some of those posts live in the free AI Automation Society community rather than AIS+**, so check both.

Never paste or reconstruct a resource download link — send them to the post or the lesson.

### 2b — n8n templates

**Classroom > Community Resources > n8n Templates.** Some older templates may have moved to the Archived course.

### 2c — Agent skills

**Classroom > Community Resources > Agent Skills.** That section contains six lessons: Skill Builder, Excalidraw Diagrams, Excalidraw Style Images, Nano Banana 2, Nate's Frontend Design, Video to Website. Each ships with its own SKILL.md and install steps. **Community Shared Skills** is a separate lesson at Community Resources *top level* — not inside Agent Skills — so send students there directly rather than telling them to open Agent Skills to find it.

### 2d — The WAT CLAUDE.md

This one trips people repeatedly:
- The **concept** is taught in the classroom: `Claude Code → Phase 2 → 1.3 The WAT Framework`.
- The **downloadable file** is attached to Nate Herk's **"Master 95% of Claude Code in 36 Mins"** community post — not to the lesson.
- If the download doesn't work on mobile: long-press the file and choose the download option, or copy the text into a new file and save it with a `.md` extension.

⚠️ **Do not tell students to look under "Classroom > Claude Code > CLAUDE.md Files".** That path does not exist in the current classroom map, and community links pointing there are stale.

⚠️ **Warn them off the wrong CLAUDE.md.** Students routinely grab the Trigger.dev CLAUDE.md instead. A CLAUDE.md carries setup instructions Claude Code executes immediately, so the wrong one scaffolds an entire Node/Trigger.dev project the course never asked for. If that already happened: don't unpick it — open a fresh empty folder and restart from where the course starts.

### 2e — OPAA / Subs to Sales / business content

They are no longer standalone courses. **OPAA ("One Person AI Automation Agency") and Subs to Sales are both sections inside the Scale course.** Searching the classroom for their old names finds nothing. See Step 3 for whether Scale is unlocked for them yet.

### 2f — Building websites / apps with login and payments

`Classroom > Claude Code module > app-building section`, starting at **"#1.1 Intro: Building Frontends"** — it covers real site design, login systems, and Stripe payments. Advice from the threads: go through the Claude Code module from the start first, then describe the specific business to Claude Code and let it build.

### 2g — Call and event recordings

- **AIS Live / weekend seminars**: recordings and session materials are edited AFTER the event and announced by **email to the address on your account**. Give it a day or two. Also check the **Live Call Recordings** classroom.
- **One session missing from an otherwise complete drop**: that session most likely had technical problems during the live event (a dropped connection was the confirmed cause in one case) and is being re-recorded. Ask in Support Needed‼️ for status rather than assuming it was skipped.
- **The weekly "AI & Chill" call**: search the community feed for the "AI and Chill recap" post — it goes up each Monday or Tuesday with the recording and a tool guide.

### 2h — The resource is genuinely broken (404, permission-locked, missing file)

This includes puzzle files, presentation slides, dead resource links, and Google Sheets that ask for access. **Post it in Support Needed‼️.** The team re-attaches files and repairs broken links directly — several of these were fixed inside the thread. This is a real, fast escalation path, not a brush-off.

<!-- pattern: /i-cannot-find-the-resources-from-the-yt-videos, /where-do-i-find-agent-skills-classroom, /n8n-templates-2, /wat-md, /issue-with-classroom-resources-link, /hi-f87ff14c -->

---

## Step 3 — "It's locked / I can't access it"

Check the gating rules BEFORE treating it as a bug. Most "locked" reports are the schedule working as designed.

| Course | When it unlocks |
|---|---|
| START HERE | Immediately |
| Build Your Portfolio (includes the Claude Code walkthrough) | Immediately |
| The AI Partner Model | At Level 3 (~20 likes from other members) |
| **Get Your First Clients** | **1 month of membership** |
| **Scale** (contains OPAA and Subs to Sales) | **90 days of membership** |

Two details from the threads that resolve most of these:
- **Time already spent as a member counts toward the gate.** Someone who joined four months ago and just noticed Scale doesn't need to wait 90 more days.
- **Annual and Premium members get both immediately.** Monthly members wait.

**If they want to know what's inside before deciding to upgrade** (a recurring ask), you can describe it:
- *Get Your First Clients* — short and practical, around 17 lessons: why the first client is the hardest, the ways to get clients, warm vs cold outreach, building an outreach list, what to say in DMs, getting people onto discovery calls, and a full Upwork section (profile optimisation, finding high-ROI jobs, a proposal framework, submitting first proposals).
- *Scale* — the large one, around 84 lessons on the business behind an agency: mindset, business formation (LLC, banking and taxes, contracts, invoicing), niche selection, online presence and branding, packaging and pricing (pricing models, the ROI formula, anchoring, retainers), outbound systems including a cold email case study, discovery and sales, client onboarding and QA, sustainable lead flow, and finding constraints and hiring.

*(Counts and section names come from an announcement post — say "around" and suggest they verify against the live classroom.)*

**OPAA is also sold as a standalone course outside the membership**, for anyone who only wants that content.

**If it's genuinely not the gate** — they're an annual member and still locked, or the entitlement looks wrong — that is an account-access issue: **message the AIS Support account on Skool.** That account handles locked classrooms and perk approvals and explicitly does not handle technical inquiries.

**Member perks / AI Discount Vault stuck on "pending"**: signing up on the perks site directly does not create a request anyone can approve. The request has to be submitted from **Classroom > Member Perks > $3M Savings Vault > "How to Unlock Your AI Discount Vault"**. Approvals are done in batches. Still nothing after that → DM the AIS Support account. *(Neither source thread ended with a confirmed resolution — present this as the mechanism the team described, not a guaranteed fix.)*

<!-- pattern: /opaa, /opaa-access, /access-to-one-person-ai-agency-not-recieved, /seeking-curriculum-details-for-get-your-first-clients-and-scale, /ai-discount-vault-approval, /locked-out -->

---

## Step 4 — "My progress is stuck"

Short branch. The cause is almost always the same.

**Skool never auto-completes a lesson.** Watching the video to the end does nothing. Cache clearing and reboots do nothing.

1. Click the **checkmark at the top right** of every finished lesson.
2. Also open and checkmark **each section header** and every **"End Of ..." trophy lesson**. These are the ones people miss.
3. Scan the course outline for **entire sections never opened** — a classroom restructure can insert sections into a course someone already "finished".
4. If an item looks checked but the percentage won't move, **uncheck and re-check it** to force a refresh.

<!-- pattern: /course-progress-not-updating-after-troubleshooting, /portfolio-stuck-at-89 -->

---

## Step 5 — "Where do I start / what's next"

This is a decision-tree conversation, not a fix. Ask one question: **"Do you have any coding background, and what do you want to have built in a month?"**

### The path the team endorses

1. **Agent Zero module** — fundamentals (LLMs, APIs, JSON, triggers, data passing, webhooks, error handling). These transfer across every tool and are what let you *direct and debug* an agent rather than just follow a video.
2. **"10 Hours to 10 Seconds V2"**.

Both live in the **Build Your Portfolio** course, which is open from day one.

### After that — two valid orders, both endorsed

- **(a) n8n first**: build a handful of real n8n workflows so triggers, data passing, webhooks and error handling click, then layer Claude Code on top. Usually the right call for **complete beginners**.
- **(b) Claude Code first**: straight into Claude Code after Agent Zero, picking up n8n as a complement. Usually the right call for people with a **coding background**.

Running both in parallel also works. Don't present one as correct.

### Standing answers to the two anxiety questions

**"Is n8n dead / did Nate graduate it?"** — "Graduated" in the stack-tier video means *no longer his primary content tool*, not obsolete. n8n remains the always-on runtime layer and stays in client and enterprise demand. The masterclass moving to the Archived classroom is housekeeping, not deprecation. The mental model: **n8n is the always-on runtime; Claude Code is the senior dev that builds and modifies those workflows via the n8n MCP.** They're complements. For a client who asks for "n8n", delivering n8n built faster with Claude Code is a competitive advantage, not an argument to have. Nate Herk's "Is n8n Dead?" video is the canonical answer to send.

**"Should I use Codex instead?"** — Yes, you can follow the course on Codex. The one substitution that matters: **Codex reads `AGENTS.md`, not `CLAUDE.md`** — it silently ignores CLAUDE.md. Wherever a lesson says create CLAUDE.md, create AGENTS.md (`/init` in Codex scaffolds it); the contents are identical markdown. Codex also asks fewer permission questions by default, so run `/plan` before builds and add an ask-before-edit line to AGENTS.md to match the videos' interactivity. Running both tools side by side: keep one AGENTS.md and reference it from CLAUDE.md with `@AGENTS.md`. For dedicated Codex learning the team points to Nate Herk's one-hour Codex course post.

### General advice that appears in every one of these threads

- Treat "X is dead" video titles as engagement bait. Pick one orchestrator and commit; tool-hopping is the actual thing costing them months.
- **Build real projects from day one** and prompt Claude Code to explain every step and decision as it goes. A team member named this as the fastest-learning technique.

<!-- pattern: /i-need-help-where-to-start, /beginner-here-anxious-to-take-the-right-direction, /course-workflow, /im-lost-help, /is-it-still-worth-it-to-learn-n8n, /is-n8n-still-recommended-or-is-it-graduated, /will-codex-do-as-claude-code-in-the-course -->

---

## Step 6 — Video playback and transcripts

### "The video won't play" or "the timeline skips chunks"

Two unrelated causes — separate them first:

1. **Many videos failing at once, others reporting the same** → a Loom outage. Classroom videos are Loom-hosted; there is nothing to fix on the student's side. Refresh and come back later. Repeated refreshes sometimes get an individual video to load in the meantime.
2. **Timeline doesn't load fully and playback skips the gaps** → this is Chrome-specific. **Open the same video in Firefox or Edge.** This is the confirmed fix — a student with gaps on every video reported no problems at all after switching.
   - To stay in Chrome: Settings → search "hardware acceleration" → turn off "Use hardware acceleration when available" → restart Chrome. *(Suggested by the team but never tested by the student — offer it as unverified.)*
   - **Incognito mode does not help** with the gap problem. Don't let them spend time on it. Clearing cache is worth one try.
3. **Exactly one video fails in every browser while everything else plays** → that specific video file is the problem. Report it in Support Needed‼️.

### "I want a text transcript" (for NotebookLM etc.)

"Save Video As" and link copying are restricted in the classroom player, and YouTube transcript tools don't apply because these aren't YouTube videos. The route through Loom's own player:

1. Put the lesson video into **full screen**.
2. **Pause** it. A "Get Loom for free" popup appears — click it.
3. **Close the login window** that opens. The transcript panel appears on the right with a Copy button.

One video at a time; there is no batch export. This depends entirely on Loom's player UI and may break if Loom changes it. Fallback raised in-thread: play the video at increased speed and capture the audio with a transcription tool.

*(These steps came from the support team but the student never confirmed back that they worked — say so.)*

<!-- pattern: /agent-zero-not-working, /loom-video-loading-problem, /request-does-anyone-have-the-masterclass-transcripts-for-notebooklm -->

---

## Step 7 — Adjacent things students ask in the same breath

- **"What does AIS+ actually get me?"** — unlimited tech support via Support Needed‼️, a structured learning path, extra templates and build walkthroughs beyond the free community, live Q&A calls with Nate Herk plus community calls (both recorded), special events, and the member network. **There are no one-to-one screen-share sessions** — the team's stated alternative is to post the specific blocker and work it in the thread. Typical response time is within 24 hours (team-stated; volume and time zones can stretch it).
- **"Who do I contact for what?"** — technical → Support Needed‼️. Membership, billing, cancellations, refunds, promo-link subscriptions → the official billing email **nate@aiautomationsociety.ai** (`START HERE → Help With Billing & Your Account`); include the email tied to the account, a short description, and screenshots. Account access and entitlements → the **AIS Support** account on Skool. ⚠️ Do not tell a student to DM a specific team member about billing — the documented email is the route that produces a paper trail, and at least one member's DM-based cancellation was confirmed and then billed again anyway.
- **"I got a weird DM about a prize"** — a recurring scam campaign targets members. The official team does not solicit via unsolicited DMs. Don't reply or click; verify in Support Needed‼️, then report and block.
- **"Is there a member discount for Hostinger / n8n?"** — check **Classroom > Community Resources > Discount Codes** first (not tier-gated), then **Classroom > Member Perks > $3M Savings Vault** (tied to annual membership, so a locked view on monthly is the gating, not a bug). Also flag the Hostinger renewal trap: the advertised price applies to the first term paid upfront and roughly doubles on renewal, so pick the longest term they're comfortable with. *(Both discount threads ended with hedged team answers and no student confirmation — tell them to verify the location.)*

<!-- pattern: /benefits-or-difference-between-plus-and-free, /support-response-time, /admin-contact, /receive-notification, /n8n-discount, /vps-recommendation -->

---

## If none of the branches matched

Switch to drafting a Support Needed‼️ post (see SKILL.md Escape Hatch B). For course-access issues, pre-fill:

- **What exactly they're looking for** — lesson title, file name, video title, or as much as they have
- **Where they've already looked** — which courses, which sections
- **What made them think it exists** — a lesson reference, a video, a community post
- **Membership type and how long** — monthly vs annual, join date (this alone resolves most "it's locked" cases)
- **Screenshot of what they see** — especially for locked courses and stuck progress

No need to tag anyone — the support team watches Support Needed‼️. Remind them to retitle the post **[SOLVED]** once it's resolved.

For **membership, billing or cancellation** specifically: do NOT send them to Support Needed‼️. Route per `knowledge/community-map.md`.
