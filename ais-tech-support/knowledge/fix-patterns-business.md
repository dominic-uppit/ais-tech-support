# Fix Patterns — Business, client-facing & design output

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

See also `diagnostics/client-work-pricing.md`, which is the routed entry point for pricing, scoping and client-ownership questions.

---

## BUSINESS / CLIENT-FACING

## Pricing an AI automation build — where to point people, and the 10x framing
**Symptom**: Recurring question: what to charge for a chatbot / RAG assistant / voice agent / automation project, and how to structure a retainer without hourly billing.

**Root cause**: No pricing anchor, and framing the work as 'a chatbot' invites commodity pricing. The community's canonical pricing resources are hard to discover, so the same question resurfaces weekly.

**Fix steps**:
1. First move the question to the Agency Talks category — that is where members who actually close these deals answer, and the team routes pricing posts there.
2. Point to the canonical resources by title: the community post 'AI Agent & Workflow Pricing Framework' (written by a community member, widely used as the go-to reference), and Nate Herk's videos 'Our Pricing Framework + 2 Real Examples' and 'How to Price AI Workflows (Without Losing Clients)' (the latter has an accompanying PDF).
3. The 10x rule, as a team member laid it out for voice agents: (missed calls per day) x (operating days per month) x (average value per job) x (a realistic close rate, ~0.30) = revenue the business is losing per month; quote roughly one tenth of that so the client sees a 10x return. Swap in the equivalent leaked-revenue metric for other build types.
4. Frame the offer around the system and the outcome, not the tool — an agent that is described as 'a chatbot' gets priced like a chatbot.
5. For retainers, one community member's structure that the thread found useful: define 'done' per inclusion instead of buying hours, write an explicit scope ceiling (e.g. two revision rounds, extra scope quoted separately), and include a fixed flex allocation (e.g. a set number of async requests per month). Alternatives raised in threads: a discounted bank of hours, a paid audit-then-prioritize engagement, or milestone-split projects.
6. Set expectations: the team's own answer is that there is no single correct price. Treat the first project as calibration and iterate from the outcome.

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/how-should-i-price-a-rag-based-ai-assistant-for-a-client-2
- https://www.skool.com/ai-automation-society-plus/how-much-are-chatbots-sellable-for
- https://www.skool.com/ai-automation-society-plus/how-do-you-scope-ai-automation-retainers
- https://www.skool.com/ai-automation-society-plus/what-do-i-charge-them
- https://www.skool.com/ai-automation-society-plus/outcome-based-pricing-retainers
- https://www.skool.com/ai-automation-society-plus/please-help-me-determine-the-price
- https://www.skool.com/ai-automation-society-plus/i-walked-into-a-dental-clinic-cold-ran-a-live-demo-and-now-i-need-to-close-it-help-me-not-mess-this-up

<!-- pattern: /how-should-i-price-a-rag-based-ai-assistant-for-a-client-2 -->

---

## Client delivery ownership rules — who owns which accounts, n8n licensing, hybrid hosting
**Symptom**: Builders set up Stripe/Supabase/Resend/n8n/hosting on their own accounts, plan to resell n8n hosting, or ask who pays for what at handoff.

**Root cause**: No ownership mental model, plus a hard constraint: n8n's terms don't permit hosting/reselling access to others on non-enterprise plans, and AI providers require usage attributable to the business using it.

**Fix steps**:
1. The client owns anything touching money, customer data, or outbound sending - Stripe, Supabase, Resend, CRM, and LLM API accounts. Have THEM create those accounts from day one and invite you as developer/admin; transferring after the fact is painful, and AI providers require usage to be attributable to the business actually using it, so client-owned API keys are a terms requirement, not just a preference.
2. n8n: the client must own their instance, whether that is their own cloud account or their own VPS, with you invited as a builder. n8n's terms do not permit hosting or reselling access to others on non-enterprise plans. Bill your build fee and retainer separately from the instance cost rather than rolling the instance into one number.
3. Hybrid model for the pieces an agency legitimately owns: your own VPS n8n for your INTERNAL operations, your Trigger.dev projects, and your code repos as your IP, with client-owned credentials plugged in via OAuth or scoped keys. This is not licence to run client production workloads on your own n8n - client automations still belong in the client's instance.
4. Where you do host multiple clients' non-n8n services yourself, isolate one container per client on separate subdomains. Vercel-hosted client sites may stay on your paid Vercel account as a retainer service (Vercel permits client projects on a paid plan) or move to a client-owned account for one-time-fee deals.
5. Contract and handover: a handover doc listing every account and who holds which role, explicit termination and data-return terms, and suspend-don't-hostage on non-payment (freeze workflows but keep the client's data accessible to them). Supabase has a Transfer Project flow in its dashboard. Practise a full handoff with a friend or unpaid volunteer before your first paying client.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/first-website-for-client-done-but-need-help
- https://www.skool.com/ai-automation-society-plus/handover-hosting-automations
- https://www.skool.com/ai-automation-society-plus/first-client-4
- https://www.skool.com/ai-automation-society-plus/n8n-selfhosted
- https://www.skool.com/ai-automation-society-plus/n8n-self-hosted-vs-cloud-for-clients
- https://www.skool.com/ai-automation-society-plus/self-host-vs-not
- https://www.skool.com/ai-automation-society-plus/thoughts-on-hosting-everything-for-the-client
- https://www.skool.com/ai-automation-society-plus/architecture-advice-from-experts-needed-for-privacy-first-n8n-automation-dockerhetzner-openrouter
- https://www.skool.com/ai-automation-society-plus/need-helpadvice-on-client-creds-for-n8n
- https://www.skool.com/ai-automation-society-plus/airbnbvrbo-guestbook-builder

<!-- pattern: /first-website-for-client-done-but-need-help -->

---

## Selling websites to local businesses — delivery stack and prospecting signals
**Symptom**: Builder can produce Claude Code sites but doesn't know how to structure repos/domains/payments/bookings per client, or which businesses to approach.

**Root cause**: First-time delivery with no standard operating structure or prospecting criteria.

**Fix steps**:
1. One repo per client under YOUR GitHub (small businesses won't manage GitHub), each a separate Vercel project; buy domains at a registrar (Cloudflare/Namecheap) and point DNS at Vercel; roll hosting into the monthly retainer, transfer repo+domain if a client wants ownership.
2. Build one template repo (menu/hours/contact/SEO) and clone per client.
3. Payments: check the client's POS first (Square/Toast often have online ordering to embed); otherwise Stripe Payment Links. Bookings: Cal.com/Calendly embed, or form → n8n webhook → calendar + notification; don't overengineer v1.
4. Prospecting: no site / outdated / broken UX; directories (Maps, Yelp); run candidate sites through similarweb.com for low traffic + high bounce.
5. One-page contract before building; community pricing ballpark: $1.5-2.5K build + $150-300/mo retainer.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/questions-about-website-implementation
- https://www.skool.com/ai-automation-society-plus/questions-regarding-website-buildingselling

<!-- pattern: /questions-about-website-implementation -->

---
## DESIGN & MEDIA OUTPUT QUALITY

## Frontend design gets worse each iteration — principle-based prompting over screenshot copying
**Symptom**: The first build looks decent, but every 'make it look like this screenshot' pass produces worse, more generic output.

**Root cause**: Over-prescriptive visual copying pushes the model toward safe, generic patterns — the more exactly you specify, the blander the result. Long single-session iteration compounds it by accumulating contradictory instructions and overwriting what already worked.

**Fix steps**:
1. Stop sending screenshots. Prompt with design principles and specific CSS deltas instead — 'bold typography, high-contrast pairings, distinctive spacing, avoid generic default fonts', or 'make the header 20px smaller and the background dark gray' — rather than 'make it look like this'.
2. Once a version looks decent, tell Claude Code to save that version before asking for any further changes, so a bad iteration doesn't overwrite it.
3. Start a fresh session rather than continuing to iterate inside a degraded one.
4. Anthropic publishes an official frontend-design skill built for exactly this problem. The team member's install route was /plugin marketplace add anthropics/skills — but be aware the poster in this thread reported that command doing nothing and never got a follow-up, and '/plugin command does nothing' is a known separate issue with its own fix. Resolve that first if you hit it.
5. For a chat UI specifically, consider starting from a pre-built chat component library and having Claude Code only style it, rather than having it build the whole widget from scratch.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-code-frontend-chatbot

<!-- pattern: /claude-code-frontend-chatbot -->

---

## Professional marketing collateral from agents — Recraft + HTML/CSS-to-Image (Canva API won't cut it)
**Symptom**: Agent-generated flyers/posters look amateur; Canva API output stays basic; raw image APIs are hit-and-miss.

**Root cause**: Canva's Autofill API requires Enterprise for both developer and end user (paid non-Enterprise plans get only a limited development trial) and brand kits don't influence API output; raw image models lack design structure/templates.

**Fix steps**:
1. Use the Recraft API (recraft.ai/api) via its MCP for brand-lockable asset generation (vector, 100+ styles, CMYK/DPI).
2. Add HTML/CSS to Image (htmlcsstoimage.com, has MCP + free tier): Claude generates precise HTML/CSS layouts rendered to images.
3. Use them in tandem: Recraft for assets, HTML/CSS render for final composition — confirmed as where the quality jump happens.
4. Put the brand style guide document in the project folder; store both API keys in .env (get them from each provider's dashboard after signup).

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-code-design-output

<!-- pattern: /claude-code-design-output -->

---

## Image pipelines lose quality when the theme changes — separate style-lock from subject
**Symptom**: A generation pipeline tuned for one theme produces noticeably worse images as soon as the topic switches to a different subject.

**Root cause**: The style and mood are baked into the subject text, so swapping the subject makes the model reinterpret the whole aesthetic instead of just changing what is depicted.

**Fix steps**:
1. Split every prompt into two blocks: a fixed style-lock (lighting, colour palette, medium, composition, mood) and a swappable subject. The style-lock block goes FIRST and never changes; only the subject after it varies.
2. The reason ordering matters: image models weight tokens at the start of the prompt more heavily, so leading with the style anchors the aesthetic before the model reaches the subject.
3. Treat the style block as a reusable config and the subject block as an injected variable, rather than writing one combined prompt per image.
4. If Claude Code orchestrates the pipeline, encode the style constants in CLAUDE.md so they persist across sessions instead of being re-specified each run.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-content-prompting

<!-- pattern: /claude-content-prompting -->

---

## Programmatic slides look flat — reverse-engineer one hand-designed slide
**Symptom**: Deterministic slide/diagram output is technically correct but flat; parameter tweaks don't help.

**Root cause**: A design problem: the layout engine treats all elements as equal because rules were hardcoded without knowing why good slides work.

**Fix steps**:
1. Manually design ONE ideal slide (Figma, PowerPoint or Excalidraw), then extract its rules: font-size ratios, white-space proportions, colour contrast, and where the eye lands first.
2. Defaults that fix most flatness: one dominant element per slide; ~2x more white space than feels natural; max 2 font sizes (hierarchy via weight); one accent color.
3. Encode the extracted rules back into the engine/prompt as opinionated defaults, then keep iterating — in the source thread the student reported the output 'came a long way' after this, not that it was finished.

**Lesson reference**: The 'Excalidraw Style Images' classroom module was pointed to as related content — verify it still exists

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/built-a-full-slidediagram-generator-but-the-output-still-looks-terrible

<!-- pattern: /built-a-full-slidediagram-generator-but-the-output-still-looks-terrible -->

---
