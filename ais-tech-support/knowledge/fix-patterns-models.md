# Fix Patterns — Model switching, prompting, Antigravity & RAG

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

---

## MODEL SWITCHING

## When to switch from Sonnet to Opus
**Symptom**: "Sonnet keeps fixing one thing and breaking another."

**Root cause**: Sonnet is hitting a complexity ceiling.

**Fix steps**: Switch to Opus when you see:
1. **Logic loops**: fix A breaks B, fix B breaks A.
2. **Quality drops**: output not improving after multiple correction rounds.
3. **Band-aid architecture**: Claude taking shortcuts or building fragile workarounds.
4. **General principle**: time-to-value getting worse.

`/model` then pick the Opus tier. Switch back to Sonnet for normal building once unstuck.

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/switching-between-models-claude-code

---

## OpenRouter + free Qwen with Claude Code
**Symptom**: Trying to use Claude Code with free models; "may not have access" model error.

**Root cause**: OpenRouter `:free` endpoints are gated behind a privacy setting that lets the provider train on inputs.

**Fix steps**:
1. In OpenRouter → Settings → Privacy → enable "free endpoints that may train on inputs".
2. Use `ANTHROPIC_AUTH_TOKEN` (not `ANTHROPIC_API_KEY`) for the OpenRouter key.
3. Set `ANTHROPIC_API_KEY=""` explicitly so Claude Code doesn't fall back.
4. Base URL is `openrouter.ai/api` (NOT `/api/v1`).
5. Set `ANTHROPIC_DEFAULT_HAIKU_MODEL` to a valid free slug too (background calls need it).
6. Verify the model slug is still live at https://openrouter.ai/models — slugs change frequently.
7. Honest limitation: free Qwen gets flaky on agentic tool-use loops. Buying a small amount of credit lifts the free-tier daily request cap substantially (roughly 50 → 1000/day when this was written — verify current limits at openrouter.ai/docs).

**Confidence**: high (detailed community-verified answer)

**Related — local LLMs through Claude Code on a laptop with no GPU** *(confidence for this sub-section: medium — community-verified)*

**Symptom**: Ollama models in the 6–10GB class take minutes to answer even a bare "hi" through Claude Code on a 16GB laptop with integrated graphics — and are noticeably faster when talked to directly in Ollama's own chat.

**Root cause**: With no dedicated GPU there is no VRAM, so the model runs against system RAM. 4–5GB of a 16GB machine is already taken by the OS and background processes, so a 6–10GB model spills and crawls. Claude Code also adds overhead on top of talking to Ollama directly.

1. Without a dedicated GPU (ideally 6–8GB VRAM or more), stick to small models — in-thread guidance ranged from 1B to 4B parameters, with the support team specifically suggesting 1B–3B. Named examples: llama 3 and qwen 2 at those small sizes.
2. **Do not install a larger model to fix slowness.** A bigger model is slower and increases CPU load — the opposite of the fix.
3. Run bigger models in the cloud instead. Ollama Cloud and OpenRouter's free tier were both endorsed as the practical path; the student in that thread chose OpenRouter over troubleshooting local models further. Free-tier request quotas change over time — check the current limit rather than relying on a figure from a thread.
4. **Honest caveat**: the student never tested the smaller models, so the 1–4B sizing is expert consensus from the thread rather than a result confirmed on that hardware.

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/using-claude-code-in-vs-code-via-openrouter-and-qwen-free
- https://www.skool.com/ai-automation-society-plus/using-local-llm-for-claude-code

---
## ANTIGRAVITY

## Antigravity 2.0 broke my Claude Code setup
**Symptom**: After Antigravity auto-updated to 2.0, the editor + terminal panes disappeared. No more VS Code extension support.

**Root cause**: Google removed the VS Code-style editor in 2.0; it's now an agent-only standalone app.

**Fix steps**:
1. Roll back to the previous Antigravity version per Google's discussion post: https://discuss.ai.google.dev/t/antigravity-2-0-is-awful-heres-how-to-get-the-previous-version/145512
2. After rollback, disable auto-update + set profile to None.
3. **Better long-term fix**: decouple — Claude Code works fine in plain Microsoft VS Code with the same extension, or in any terminal. Stop depending on Antigravity (it's a Gemini-first Google product).

**Confidence**: high

**Example thread**: https://www.skool.com/ai-automation-society-plus/antigravity-update

---

## Claude Code vs OpenClaw — recommendation and OpenClaw hardening
**Symptom**: Students running or considering OpenClaw ask how it compares.

**Root cause**: OpenClaw (third-party, early 2026) had unstable configs between updates, very high token burn (~$5/hour on Sonnet reported), and serious security exposure (RCE exploits, 135K+ exposed instances, ~12% of ClawHub skills flagged malicious — team-stated figures, verify before quoting).

**Fix steps**:
1. Team recommendation: prefer Claude Code (official, local, stable, cheaper).
2. If running OpenClaw anyway: sandbox in Docker, restrict permissions, never expose to the public internet, and vet ClawHub skills.

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/claude-code-vs-openclaw
- https://www.skool.com/ai-automation-society-plus/claudecode-vs-openclaw

<!-- pattern: /claude-code-vs-openclaw -->

---

## OpenClaw + consumer-subscription OAuth = account-ban risk; provider switches wipe keys
**Symptom**: Students want to run OpenClaw against their Claude/Gemini subscriptions rather than paying per token; separately, switching providers inside OpenClaw empties auth-profiles.json and the agent fails with 'No API key found for provider ...'.

**Root cause**: Anthropic and Google have issued account bans for consumer-subscription OAuth used through OpenClaw; at the time of the threads only OpenAI's Codex OAuth appeared to be permitted natively. OpenClaw's provider-switch flow can drop the credentials for the provider you switched away from.

**Fix steps**:
1. Use API keys for Anthropic and Gemini models in OpenClaw. If you have a ChatGPT subscription, the native Codex OAuth path in OpenClaw's onboarding is the one route that was reported as allowed.
2. If a provider switch wipes your credentials: the confirmed fix in the thread was to have Claude Code audit the OpenClaw config and repair it. Manual fallback — open the auth-profiles.json path named in the error and restore a profile block for each provider you use, each with its type, provider name and key.
3. Re-check the current provider terms of service before relying on any of this; enforcement on subscription-driven automation has been changing.

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/openclaw-with-oauth
- https://www.skool.com/ai-automation-society-plus/openclaw-disconnect

<!-- pattern: /openclaw-with-oauth -->

---

## OpenClaw app building on a VPS — one folder mounted into both containers
**Symptom**: Students confused why OpenClaw wants a second web-server container with a mounted volume.

**Root cause**: OpenClaw runs with root-level access, so it sandboxes builds deliberately: it writes code to a shared folder, a separate container serves it — a crash only kills the web container.

**Fix steps**:
1. Mount one VPS folder (e.g. /root/dashboard — choose your own path) into BOTH the OpenClaw container and the web-server container.
2. Browse the web container's port; after each OpenClaw save, refresh — that's the whole iteration loop.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/looking-for-implementation-details-of-nates-klaus-openclaw-dashboard

<!-- pattern: /looking-for-implementation-details-of-nates-klaus-openclaw-dashboard -->

---

## Don't build multi-account Messenger monitoring on OpenClaw's browser skill
**Symptom**: Plan to poll several Facebook personal inboxes via OpenClaw plus separate Chrome profiles on a fixed schedule, logging unread messages to a dashboard.

**Root cause**: OpenClaw can't drive multiple Chrome profiles concurrently — agents and cron jobs share one browser profile and a file-based lock forces sequential execution, causing tab hijacking and cookie conflicts. Cron scheduling is also among its least stable features. Separately, Facebook's automation detection combines TLS fingerprinting, CDP signal detection and behaviour analysis, a perfectly regular polling interval is itself a detection signal, and disabled personal accounts have no appeal or recovery path.

**Fix steps**:
1. Don't scrape personal-account inboxes. For any Facebook Page, pull it out of the scraping design entirely and use the official Messenger Platform API — supported, stable, and no detection risk.
2. If several personal inboxes just need to be visible in one place, a multi-account inbox app is the low-risk route (one was suggested in-thread but was never confirmed to support personal Facebook accounts — verify before committing).
3. If you proceed anyway: prototype with a single profile for a couple of weeks first, watch for session challenges, CAPTCHAs and account warnings, and never poll a bot-protected platform on perfectly regular intervals.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/is-this-possible-to-build-or-have-better-solution-to-do-this

<!-- pattern: /is-this-possible-to-build-or-have-better-solution-to-do-this -->

---

## PaperClip on a VPS — API-key billing with per-agent budget caps, headless login, external Postgres on LXC hosts
**Symptom**: Students ask whether a Claude Max login can power PaperClip 24/7, and get stuck on Claude Code's browser-based login when deploying to a headless VPS.

**Root cause**: Pro/Max run on Anthropic's Consumer terms, so running agents 24/7 from a subscription is a terms-of-service grey area; API keys run on Commercial terms built for automated use. Separately, Claude Code's default login expects a browser, and Hostinger VPSes are LXC containers whose restricted kernel access breaks PaperClip's embedded PostgreSQL.

**Fix steps**:
1. Billing: prefer an Anthropic API key (Commercial terms) and set budgetMonthlyCents in every agent's config — it is in US cents and auto-pauses the agent at 100% of budget. Set it before letting any agent run unattended.
2. Headless login: run `claude` on the VPS, copy the auth URL it prints, open it in a browser on any machine where you're signed in to Claude, then copy the `code` parameter out of the localhost callback URL and paste it back into the VPS terminal. Or export ANTHROPIC_API_KEY (your key from console.anthropic.com) in ~/.bashrc or ~/.profile so it persists.
3. Database (advance guidance — install PostgreSQL on the VPS or point at a managed instance, then set DATABASE_URL to its connection string, e.g. postgresql://user:password@host:5432/dbname, BEFORE you ever run onboarding; switching from embedded to external after onboarding is reported to break the bootstrap flow). Run PaperClip under a dedicated non-root user, since it refuses to start as root.
4. For remote access, switch the server out of local_trusted into authenticated mode via `paperclipai configure --section server` before setting HOST=0.0.0.0, and don't re-run onboarding afterwards — it resets the server config.
5. If you haven't bought a VPS yet, a KVM provider gives full kernel access and avoids the LXC container restrictions entirely.

**Confidence**: low — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/is-it-possible-to-use-claude-max-plan-subscription-in-paperclip
- https://www.skool.com/ai-automation-society-plus/how-to-set-up-claude-code-on-a-vps-for-paperclip

<!-- pattern: /is-it-possible-to-use-claude-max-plan-subscription-in-paperclip -->

---
## RAG / KNOWLEDGE BASE

## "Should I build RAG?" — for <100-200 doc cases
**Symptom**: User wants a chatbot over their docs; assumes they need vector DB / Pinecone. Variant: the docs belong to a client (Notion / Obsidian / markdown) and the student also has to decide where the chat surface lives and how to migrate the content.

**Root cause**: RAG complexity is overkill for small doc sets.

**Fix steps** (for under ~200 docs):
1. Use Karpathy LLM Wiki approach: plain markdown files in a Git repo, Claude reads directly. No vector DB, no chunking, no embeddings.
2. Client edits markdown in GitHub's web UI (or Decap CMS if they hate Git).
3. Chat surface: Telegram bot (10-min setup, n8n native node), or embedded Chatwoot widget for public-facing.
4. **Every answer should quote the source doc directly + show which page** — this single UX choice eliminates most hallucination complaints.
5. Cross 500-1000 docs later → add pgvector layer for overflow.
6. Use a mid-tier model (Sonnet class) with prompt caching on index/frequently-hit pages — a large cost reduction on the cached portion. ⚠️ Check `/model` or the model docs for the current lineup rather than pinning a specific version.
7. Don't use Relevance AI for this (vendor lock-in, opaque pricing).
- **Choose the surface by audience**: a Telegram bot is the lowest-setup option for internal staff; an embedded web widget is the more polished option for a public docs site. One student instead built a standalone web chat app with Claude Code (user management + model selector) and the client accepted it.
- **If the client already lives in Chatwoot**, two platform constraints to design around up front. These were flagged by the support team as known gotchas rather than verified in this build:
  - Chatwoot enforces a hard **5-second agent-bot webhook timeout** you cannot change. An LLM call will exceed it and the conversation gets force-converted to open. Build async from day one: webhook returns 200 immediately, the LLM runs in the background, and the answer is posted back via the Chatwoot API.
  - Chatwoot only renders images inline when they are uploaded as **real multipart attachments** — markdown image links will not display, so your flow has to fetch and POST the image.
- **Notion → Obsidian migration**: Notion's markdown export is messy (callouts break, databases become CSVs, internal links break). Export as **HTML** instead and import with the official Obsidian Importer plugin; community tooling exists to repair callouts, links and CSV databases.
- **Prompt caching tuning**: if queries trickle in across the day rather than clustering, consider raising `cache_control` ttl to 1h — but note the tradeoff, cache writes cost more (roughly 2x base input rather than 1.25x), so it only pays off when the cache actually gets reused.
- **Before deploying to a VPS**: API keys in environment variables not the repo, a per-user spend cap, HTTPS on any admin panel, and a hard daily or monthly cap that stops the agent responding past the limit.

**Confidence**: medium — team-verified; the Chatwoot constraints are team-flagged rather than verified in this build (Mustafa Tawfiq, detailed pattern from production builds)

**Example thread**: https://www.skool.com/ai-automation-society-plus/best-wiki-ai-agent-rag-karpathys-llm-wiki-obsidian

---

## PROMPTING & MODEL BEHAVIOUR

## AI keeps 'rebuilding' when asked if it's happy — sycophancy; ask for forced analysis instead
**Symptom**: Asking 'are you completely happy with your answer?' triggers endless rebuilds regardless of quality.

**Root cause**: LLM sycophancy: feelings/opinion-framed questions elicit agreeable 'I can improve it' responses — a people-pleasing reflex, not a quality signal.

**Fix steps**:
1. Stop feelings-framed questions ('are you happy?', 'can you do better?').
2. Ask forced-analysis questions: 'What specific information is missing from this response?', 'Spot any logical weaknesses in your answer', 'What context could I have provided for a better result?'.
3. Optionally add standing instructions, e.g. 'Stop agreeing with me by default; give an honest review and feedback' (this variant came from another community member in the same thread, not from the team answer).

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/complete-beginner-but-very-eager-to-learn

<!-- pattern: /complete-beginner-but-very-eager-to-learn -->

---

## Prompt over-engineering — meta-prompting, clarifying questions, and why specific rules beat vague cautions
**Symptom**: Students freeze hand-crafting the 'perfect prompt', or fear that encoding decision rules in CLAUDE.md/skills will corner the model's creativity.

**Root cause**: Treating prompting as one-shot instead of iterative, and the misconception that constraints reduce output quality — vague cautions ('be careful') do nothing while specific structured rules force more computation.

**Fix steps**:
1. Always allow the model to ask clarifying questions — this alone removes most vague outputs.
2. For complex tasks, use a second LLM as the prompt writer: feed it the target platform's prompting best practices plus a brain-dump, and end with 'ask me whatever you need'.
3. Encode decision logic as concrete required steps (e.g. 'before recommending an architecture, outline the problem, propose 2 alternatives, list pros/cons for tokens/speed/complexity') — specific rules improve output; 'be careful' changes nothing.
4. Reserve heavy upfront prompt engineering for prompts that run autonomously at scale (client-facing responses, batch processing with no human in the loop) — this caveat came from a community member in the thread, not from the team answer.

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/analysis-paralysis
- https://www.skool.com/ai-automation-society-plus/claude-code-logic-experts-opinion

<!-- pattern: /analysis-paralysis -->

---

## Quality ceiling? Test the prompt OUTSIDE the pipeline before migrating infrastructure
**Symptom**: Output quality feels capped; builder blames the platform and plans a migration.

**Root cause**: The bottleneck can be prompt, model/platform, or plumbing — migration only fixes the third; most never isolate which.

**Fix steps**:
1. Run the exact system prompt directly against the API/claude.ai (1-hour test).
2. Quality jumps → the pipeline degrades it (truncation, lost context); identical → prompt problem: add few-shot examples of genuinely high-performing outputs.
3. Order of operations: prompt quality first, platform second, infrastructure last.

**Confidence**: high — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/can-i-replace-n8n-with-pure-code-for-an-ai-video-ad-pipeline-and-is-it-worth-it

<!-- pattern: /can-i-replace-n8n-with-pure-code-for-an-ai-video-ad-pipeline-and-is-it-worth-it -->

---


## APP & FRONTEND DELIVERY (Lovable / Supabase / Vercel)

## Escaping Lovable Cloud lock-in — migrate to your own Supabase
**Symptom**: A Lovable project with AI-generated DB/auth is stuck on Lovable Cloud; the agent calls it 'not transferable'.

**Root cause**: No in-place switch to external Supabase exists, but only runtime data lives in Cloud — GitHub sync already carries code, migrations, RLS policies, and edge functions.

**Fix steps**:
1. Push to GitHub via Lovable's sync; export runtime data (Cloud tab > Overview > Advanced > 'Export project data'; per-table Export CSV for few tables).
2. Clone and let Claude Code run the migration using Lovable's external-deployment guide plus YOUR_SUPABASE_URL and YOUR_SUPABASE_SERVICE_ROLE_KEY (Supabase dashboard > Project Settings > API); import one table first.
3. Hard limits: auth passwords never export (users reset on the new backend); click 'Remove Lovable Cloud' only after the new backend fully works.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/anyone-else-frustrated-by-the-lovable-cloud-supabase-lock-in

<!-- pattern: /anyone-else-frustrated-by-the-lovable-cloud-supabase-lock-in -->

---

## Client production apps built in Lovable — hybrid deploy (Lovable → GitHub → Vercel, Supabase direct)
**Symptom**: Builder asks if Lovable Cloud hosting is reliable enough for a paid client app.

**Root cause**: Lovable Cloud has no uptime SLA, a history of outages, and the app stops if the credit balance runs out.

**Fix steps**:
1. Keep building in Lovable but connect two-way GitHub sync; connect the repo to Vercel for auto-deploys (paid Vercel has a 99.99% SLA).
2. Set VITE_SUPABASE_URL / VITE_SUPABASE_PUBLISHABLE_KEY / VITE_SUPABASE_PROJECT_ID in Vercel (values from the Lovable project's .env) and add vercel.json for SPA routing.
3. Use Supabase's own hosted service (Lovable creates a supabase.com project anyway); before go-live enable RLS on every table, MFA for admins, custom SMTP.
4. Client owns the Vercel/Supabase accounts and billing; put hosting ownership in the contract.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/lovable-cloud-hosting-frontend-backend

<!-- pattern: /lovable-cloud-hosting-frontend-backend -->

---

## From throwaway artifact to persistent app — the storage ladder
**Symptom**: Claude artifacts break: no database, no cross-device state; students keep a parallel md file that drifts.

**Root cause**: Artifacts are ephemeral by default and two sources of truth guarantee drift.

**Fix steps**:
1. Simple persistence: the artifact's built-in window.storage key-value API as the ONLY data layer (text/JSON, ~5MB/key).
2. Outgrown it (multi-user, auth, sync, querying)? Prototype in the artifact → Claude Code scaffolds Vite/Next → Supabase (DB/auth/storage) → Vercel.
3. No-code alternative: Lovable or Bolt with bundled Supabase.

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/creating-real-artifacts

<!-- pattern: /creating-real-artifacts -->

---

## Supabase saves look successful but reads come back empty — RLS without policies
**Symptom**: AI-generated app writes 'succeed' but queries return nothing, silently.

**Root cause**: Code generators enable Row Level Security on new tables but forget the policies, so every query filters to zero rows.

**Fix steps**:
1. Ask Claude Code to add INSERT and SELECT (plus UPDATE/DELETE as needed) policies per affected table, targeting the authenticated role; re-test the save-then-read flow.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/creating-real-artifacts

<!-- pattern: /creating-real-artifacts -->

---

## Supabase Google sign-in shows <project>.supabase.co — switch mobile apps to native sign-in
**Symptom**: The Google consent screen shows the raw supabase.co project domain; Supabase's custom domain is a paid per-project add-on (current price at supabase.com/pricing).

**Root cause**: The web OAuth flow's callback domain is always the Supabase address; Google displays it. Neither is a misconfiguration.

**Fix steps**:
1. Use native Google sign-in on mobile: the OS account picker shows your app name; pass the resulting ID token to Supabase with signInWithIdToken — no extra cost.
2. Give Claude Code your sign-in file plus Supabase's auth-google guide to do the migration; register OAuth client IDs (and Android SHA-1) in Google Cloud.
3. Only pay for the custom domain if you must keep a web flow.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/google-sign-in-showing-supabase-domain

<!-- pattern: /google-sign-in-showing-supabase-domain -->

---

## Atomic transactional logic (loyalty points) belongs in a Postgres function via Supabase .rpc(), not n8n
**Symptom**: Builder wants n8n as a mobile app's points backend because it's familiar.

**Root cause**: Read-check-write must be atomic; n8n adds HTTP hops and can't wrap DB transactions, so scans are slow and concurrent scans double-count.

**Fix steps**:
1. Points math in a Postgres function called via supabase .rpc() (transaction + row lock serializes concurrent scans); Edge Functions only for side effects.
2. Model points as an event ledger (one row per scan; balance = sum) with a till-generated UUID for idempotency.
3. Stack: Expo + React Native + supabase-js; hand off via Supabase's Transfer Project flow.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/for-the-shop-mobile-app-would-i-use-n8n

<!-- pattern: /for-the-shop-mobile-app-would-i-use-n8n -->

---

## WordPress vs GitHub+Vercel for content-heavy personal sites
**Symptom**: Student asks whether to migrate a content site to the GitHub+Vercel stack from the course.

**Root cause**: Different jobs: WordPress is a publishing/SEO CMS; GitHub+Vercel excels for interactive apps/dashboards.

**Fix steps**:
1. Keep WordPress for regular content publishing and SEO; automate distribution to socials with n8n.
2. Use GitHub+Vercel for interactive demos/dashboards linked from the site.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/wordpress-vs-github-vercel-for-personal-site-whats-better-for-regular-content-updates

<!-- pattern: /wordpress-vs-github-vercel-for-personal-site-whats-better-for-regular-content-updates -->

---
