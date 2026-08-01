# Diagnostic — Client Work: Pricing, Scoping, Ownership & Delivery

Use this when the question is about the *business* of building automations rather than a bug: what to charge, how to scope a retainer, who owns which accounts, what to hand over at the end, how to run a first client engagement. Trigger phrases: "what should I charge", "how do I price this", "my first client", "retainer", "scope creep", "who owns the n8n instance", "can I host it for them", "they want me to use my API key", "handover".

**56 threads (48 newly ingested) — the second-largest category in the corpus.** This is a decision-tree diagnostic, not a fix procedure. There are no verified commands here, only what the AIS+ team and community actually said in solved threads.

**Two framing rules before you answer anything:**

1. **This is not a substitute for Agency Talk 💼.** The community has a dedicated channel where people who have actually closed these deals discuss it, and the team routes pricing posts there. But do NOT just redirect — the threads contain real, specific answers, and a bare "go ask in Agency Talk" wastes the student's time. **Give the substantive answer, then point them at Agency Talk for the follow-up conversation.**
2. **One hard constraint runs through this entire category and it is not negotiable.** n8n's terms do not permit hosting or reselling access to others on non-enterprise plans, and AI providers require usage to be attributable to the business actually using it. That makes several intuitive-seeming setups a licensing or terms problem, not a preference. Step 3 covers it — surface it early if their plan collides with it.

---

## Step 1 — Triage: which of four questions is this?

| What they're asking | Go to |
|---|---|
| "What do I charge?" / "how do I structure a retainer?" | **Step 2** — pricing |
| "Who owns the n8n / Stripe / Supabase account?" / "can I host it for them?" | **Step 3** — ownership (read this even if they didn't ask) |
| "I have a first client meeting / I'm about to close one" | **Step 4** — first-engagement play |
| "How do I actually deliver and hand this over?" | **Step 5** — delivery |

If the question is really "how do I *build* the thing they asked for", that's not this file — route to the relevant technical diagnostic or the matching `knowledge/fix-patterns-*.md` category file (index: `knowledge/fix-patterns.md`).

---

## Step 2 — Pricing

**Start by saying the true thing**: there is no single correct price, and the team's own answer says so. The first project is calibration. That framing prevents the student from freezing.

### 2a — Point them at the canonical resources by title

These are the references the community actually uses, and they're hard to discover, which is why the question resurfaces weekly:

- The community post **"AI Agent & Workflow Pricing Framework"** — written by a community member and widely used as the go-to reference.
- Nate Herk's **"Our Pricing Framework + 2 Real Examples"**.
- Nate Herk's **"How to Price AI Workflows (Without Losing Clients)"** — has an accompanying PDF.

### 2b — The 10x framing (the one concrete formula in the corpus)

A team member laid this out for voice agents. Adapt the leaked-revenue metric for other build types:

```
(missed calls per day) × (operating days per month) × (average value per job) × (realistic close rate, ~0.30)
    = revenue the business is currently losing per month

Quote roughly ONE TENTH of that number.
```

The point is that the client sees a 10x return, and the conversation stops being about your hourly rate. For a lead-response build the metric is unworked leads; for a support build it's ticket-handling hours; the structure is identical.

⚠️ **Never produce a dollar figure the student did not give you.** Hand them the formula and name the inputs to fill in; do not substitute your own numbers for a loaded hourly rate, an average job value, or a close rate, and do not carry the arithmetic through to a quote. A number that came from you reads to the student as the community's answer, and the corpus has exactly one published ballpark (2e, local-business websites only), which does not transfer to other build types. If the student hasn't given you the inputs, ask for them, or state the formula and let them run it.

The hours-saved case is the most common non-voice variant and the easiest to get wrong: "20 hours a month" is a labour-cost input, not a revenue-leak input, so the 10x framing above does not apply to it directly. Walk them to their own number instead: loaded cost per hour (their figure, not a guess), times hours saved per month, plus whatever else the build recovers, then annualise. That total is what the quote gets anchored against. Do not pick the multiplier for them.

### 2c — Frame the offer around the system, not the tool

Directly from the threads: **"an agent that is described as 'a chatbot' gets priced like a chatbot."** Sell the outcome and the system. This is the single highest-leverage change to how a student talks about their work.

### 2d — Retainers without hourly billing

One community member's structure that the thread found useful:

1. **Define "done" per inclusion** instead of selling hours.
2. **Write an explicit scope ceiling** — e.g. two revision rounds, extra scope quoted separately.
3. **Include a fixed flex allocation** — e.g. a set number of async requests per month.

Alternatives raised across threads: a discounted bank of hours; a paid audit-then-prioritise engagement; milestone-split projects.

Related, from the ownership threads: **bill your build fee and retainer separately from the client's infrastructure cost.** Rolling their n8n subscription or VPS into one number is both a licensing problem (Step 3) and a pricing problem — you end up defending a bill that's mostly pass-through.

### 2e — One published ballpark

For local-business websites specifically, the community ballpark in-thread was **$1.5–2.5K build + $150–300/mo retainer**. That is one data point from one niche — offer it as an anchor, not a rate card.

### 2f — Then route

> "That's the shape of it. For the back-and-forth on your specific deal, **Agency Talk 💼** is where the people who've actually closed these hang out — want me to draft the post?"

<!-- pattern: /how-should-i-price-a-rag-based-ai-assistant-for-a-client-2, /how-do-you-scope-ai-automation-retainers, /how-much-are-chatbots-sellable-for, /outcome-based-pricing-retainers, /what-do-i-charge-them, /please-help-me-determine-the-price -->

---

## Step 3 — Ownership: who owns which account

**Surface this branch even when the student didn't ask.** Several threads are students who already built everything on their own accounts and now have a painful migration. Getting here first is worth more than any pricing advice.

### 3a — The rule

**The client owns anything touching money, customer data, or outbound sending.** That means Stripe, Supabase, Resend, their CRM, and their LLM/API accounts. Have THEM create those accounts from day one and invite you as developer or admin.

Two reasons, and the second one is the one students don't know:
- Transferring after the fact is painful.
- **AI providers require usage to be attributable to the business actually using it.** Client-owned API keys are a terms requirement, not a preference.

### 3b — n8n specifically — the hard constraint

**The client must own their n8n instance** — their own cloud account or their own VPS — with you invited as a builder.

**n8n's terms do not permit hosting or reselling access to others on non-enterprise plans.** This kills three plans students arrive with:
- "I'll run all my clients on my own self-hosted n8n"
- "I'll bundle their n8n cost into my invoice"
- "I'll resell n8n hosting as part of the retainer"

Bill your build fee and retainer separately from the instance cost.

### 3c — What an agency legitimately CAN own (the hybrid model)

- Your own VPS n8n for your **internal** operations
- Your Trigger.dev projects
- Your code repos as your IP

with client-owned credentials plugged in via OAuth or scoped keys. **This is not licence to run client production workloads on your own n8n** — client automations still belong in the client's instance.

Where you do host multiple clients' **non-n8n** services yourself, isolate one container per client on separate subdomains. Vercel-hosted client sites may stay on your paid Vercel account as a retainer service (Vercel permits client projects on a paid plan — but the **Hobby** tier is personal/non-commercial only, so client work on Hobby is a terms violation), or move to a client-owned account for one-time-fee deals.

### 3d — Credentials: how to actually take access without holding secrets

Full playbook is in `knowledge/fix-patterns-config-skills.md` ("Client credential handling playbook"). The headlines:

- **Email/Google: OAuth2 only.** Never the client's password, never app passwords (Google has deprecated basic password auth for Gmail). On n8n Cloud the client just clicks "Sign in with Google" and can revoke at any time.
- **The self-hosted trap**: with your own Google Cloud app, Gmail scopes are sensitive/restricted, so a consent screen left in **Testing** mode expires the refresh token in about **7 days** and the client's connection silently dies. Either have the client own the Google project inside their own Workspace with the consent screen set to Internal, or use n8n Cloud's already-verified app.
- **Scoped keys, not master keys**: an Airtable PAT limited to one base; Stripe/HubSpot restricted keys.
- **Secrets live where the client owns them.** On Trigger.dev, invite the client to the team and have them enter keys as **Secret** environment variables — once marked Secret the value is hidden in the dashboard and can't be viewed there after creation. Treat that as UI-level protection rather than a cryptographic guarantee: don't promise a client the key is beyond your reach, since anyone with management-API access to the project may still be able to read it (current behaviour: trigger.dev/docs/deploy-environment-variables). For one-off handoffs, a 1Password or Bitwarden single-view expiring link.
- **Build against your own dummy credentials**, then do a short **live handoff call** where the client shares their screen and pastes their own keys or completes the OAuth flow themselves. You never touch a raw secret.
- At roughly **5-10+ clients**, graduate from encrypted env vars to a managed vault (Doppler, 1Password, HashiCorp Vault).
- **Already holding a client password or key?** Migrate in this order: swap email to OAuth2 first (highest risk), then move the API keys, then rotate everything you previously held.

<!-- pattern: /handover-hosting-automations, /n8n-self-hosted-vs-cloud-for-clients, /thoughts-on-hosting-everything-for-the-client, /self-host-vs-not, /clients-api-keys, /clients-api-keys-and-password-management, /need-helpadvice-on-client-creds-for-n8n, /first-client-4 -->

---

## Step 4 — First client engagement

Students posting "I'm about to land a huge client, help me not mess it up" need a sequence, not a pep talk.

### 4a — Lead with discovery, not technology

A recurring failure in the threads: the student led the meeting with the tech stack and froze when the prospect asked **"show me what you'd actually do with my data."**

Do the discovery call and the homework (their competitors, where their data actually lives) BEFORE pitching. Pitching a generic "AI solutions for your industry" menu without diagnosing the actual bottleneck doesn't close — interest without need plus urgency isn't a deal.

### 4b — Scope ONE thing

Ship one scoped foot-in-the-door build with defined milestones. Do not pitch or build every workflow in a multi-part plan at once. This appears as an antipattern across several threads.

### 4c — Don't block on their systems

If the demo depends on access to the client's proprietary system, **mock the integration with a free stand-in** (Calendly, Google Calendar, Booksy) rather than waiting weeks for access.

### 4d — Stop building demos

Also from the threads, bluntly: 3-5 demos is enough. "One more build" past that is procrastination wearing a portfolio costume. If they have demos and no outreach, the bottleneck is not the portfolio.

### 4e — Paper before code

A one-page contract before building. For the retainer structure see Step 2d; for the handover terms see Step 5.

### 4f — Decline TOS-violating asks

If a client asks for something that violates a platform's terms (Facebook Group auto-posting, unofficial DM automation, scraping phone numbers for cold calls), **decline with a protect-their-assets framing** and pivot to a compliant alternative. Their account is the thing at risk. This is a client-relationship win, not a lost sale.

<!-- pattern: /first-client-4, /first-website-for-client-done-but-need-help, /i-walked-into-a-dental-clinic-cold-ran-a-live-demo-and-now-i-need-to-close-it-help-me-not-mess-this-up -->

---

## Step 5 — Delivery and handover

### 5a — The handover document

List every account and who holds which role. Add explicit termination and data-return terms. Supabase has a **Transfer Project** flow in its dashboard for the database side.

### 5b — Suspend, don't hostage

On non-payment: freeze workflows but keep the client's data accessible to them. This is the community's stated norm.

### 5c — Practise it

Do a full handoff with a friend or an unpaid volunteer before your first paying client. Named explicitly in the threads.

### 5d — Kill the "set once, runs forever" expectation

Real maintenance exists: Gmail OAuth expiries, model deprecations, edge cases. Quarterly check-ins minimum. Two specific things worth naming in the contract:
- **Tool API changes that break flows are billable.**
- **Scrapers rot as target sites change.** Handing over a workflow JSON as a one-off deliverable with no maintenance story sets up a dispute. Sell the data output, or scope an explicit warranty period.

### 5e — Client visibility

Clients want to see what the system is doing. Give them a dashboard, not terminal access — **the client of a sold AI system does not need their own Claude Code**. For n8n builds, an Airtable or Sheets control plane plus per-run logs is the pattern the corpus uses. For agent builds, see the observability pattern in `knowledge/fix-patterns-hosting.md` ("No visibility into vibe-coded background workflows").

### 5f — Local-business website delivery (specific, recurring sub-case)

- One repo per client under **YOUR** GitHub — small businesses won't manage GitHub — each a separate Vercel project. Transfer repo + domain later if they want ownership.
- Buy domains at a registrar (Cloudflare, Namecheap) and point DNS at Vercel.
- Build ONE template repo (menu, hours, contact, SEO) and clone it per client.
- **Payments**: check the client's POS first — Square and Toast often have online ordering you can embed. Otherwise Stripe Payment Links.
- **Bookings**: a Cal.com/Calendly embed, or form → n8n webhook → calendar + notification. Don't overengineer v1.
- **Prospecting signals**: no site, outdated site, broken UX. Source from directories (Maps, Yelp); run candidates through a traffic-estimate tool looking for low traffic and high bounce.

<!-- pattern: /questions-about-website-implementation, /questions-regarding-website-buildingselling, /handover-hosting-automations, /airbnbvrbo-guestbook-builder -->

---

## Step 6 — Antipatterns to interrupt on sight

If the student states any of these as their plan, say so before answering the question they asked:

- Building client systems on **your own** Stripe / Supabase / Resend / OpenAI accounts
- **Reselling n8n hosting** or bundling the client's n8n cost into your invoice (terms violation on non-enterprise plans)
- Running a client's production workload on **your** self-hosted n8n Community Edition
- Hosting a client's production automations on **Claude Desktop `/schedule`** or a spare laptop with a free ngrok tunnel — they stop when the laptop sleeps and the tunnel URL rotates
- Running a client helpdesk on a **personal Claude subscription** — production needs API keys (a personal plan covers that person's own use, not a client's end users)
- **Quoting third-party running costs as your own charges** — give an "estimated monthly costs" section instead
- Collecting **raw client passwords or API keys** over chat, email or a spreadsheet
- Leaving a **Google OAuth consent screen in Testing mode** on a handed-off build (dies in 7 days)
- Storing patient or healthcare data in **Google Sheets** for a client-facing demo without flagging HIPAA/GDPR — it destroys credibility with medical clients instantly

---

## If none of the branches matched

This category has a different escape hatch from the technical ones. Prefer **Escape Hatch A** — draft an **Agency Talk 💼** post, not a Support Needed‼️ post. Pre-fill:

- **The deal shape** — what the client does, what they asked for, roughly what size
- **What they've already considered** — a number they have in mind, a structure they're weighing
- **The specific decision they're stuck on** — "is $X too low for this?" beats "how do I price?"
- **Any constraint** — the client insists on owning nothing, or wants you to use their existing tool

Use Support Needed‼️ only if the underlying blocker turns out to be technical after all.
