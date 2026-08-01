# Diagnostic — Third-Party Integrations & API Auth

Use this when the failure is at the boundary between the student's build and somebody else's platform: an OAuth flow that dies, an API key that "works but doesn't", a connector that won't install, a platform that silently drops requests. Trigger phrases: "it worked last week and now it doesn't", "refresh token keeps expiring", "does not support dynamic client registration", "my API key is right but it says unauthorized", "Google keeps asking me to reconnect", "I need API credits?", "the file type isn't supported".

**25 threads (12 newly ingested).** These are high-frustration cases because the error message almost never names the real cause, and the student reasonably assumes the problem is their own configuration.

**Lead with this**: the two most common root causes in this whole category are not configuration errors at all —
1. **A Google OAuth consent screen left in "Testing" mode**, which expires refresh tokens every 7 days. This is the "it just stopped working" bug, and it is the single highest-frequency integration failure in the corpus.
2. **A wrong assumption about what a subscription covers.** A Claude Pro/Max subscription includes **zero** Anthropic API usage, and consumer subscriptions run on different terms from API keys.

Check both before diagnosing anything the student built.

---

## Step 1 — Triage question

Ask ONE question (or read it from the transcript):

> **"Did this ever work — and if it did, roughly how long did it work before it broke?"**

The timing is diagnostic in a way the error message isn't:

| Answer | Almost certainly | Go to |
|---|---|---|
| "It worked for about a week, then died" | Google OAuth consent screen in Testing mode | **Step 2** |
| "It worked for about a day" | A temporary/short-lived access token | **Step 2e** |
| "It never worked — auth fails from the start" | Wrong scheme, wrong header, or wrong credential entirely | **Step 3** |
| "It authenticates but the calls do nothing / hit the wrong thing" | Wrong endpoint, wrong parameter names, or a plan gate | **Step 4** |
| "The connector/plugin won't install at all" | Platform-side incompatibility, not user error | **Step 5** |
| "It says I'm out of credits but I pay for Claude" | Subscription vs API billing confusion | **Step 6** |

---

## Step 2 — Google OAuth (the "it stopped working after a week" family)

This is the highest-value branch in the file. If the student's automation touches Gmail, Drive, Sheets or Calendar and it died about seven days after it started working, you can lead with the diagnosis.

### 2a — The cause

Google Cloud OAuth apps left in **"Testing"** publishing status issue refresh tokens that **expire after 7 days**. Any Gmail/Drive/Sheets credential built on that app silently dies about weekly. Nothing in the error text says this.

### 2b — The fix, by situation

**If every mailbox/account involved is inside ONE Google Workspace:**
Set the OAuth consent screen **User type to Internal**. No 7-day expiry and no Google review. This is the cleanest outcome. ⚠️ Internal only appears if the Cloud project belongs to a Google Workspace / Cloud Identity organization — on a personal gmail.com account the option isn't there at all. If they can't see it, that's the tell: use the Publish route below rather than hunting for the setting.

**Otherwise:**
OAuth consent screen → **Publish App** ("In production"). **Publishing is instant and is a completely different process from verification** — students conflate the two and think they're blocked for weeks. Contributors in these threads report a private, low-user client tool continuing to work unverified. After publishing, reconnect the credential once, and anyone signing in clicks **Advanced → Continue** past the "app isn't verified" warning one time.

**Publishing does not lift the user cap on its own.** Per Google's docs, an app that is published but still requests *unapproved* sensitive or restricted scopes stays capped at **100 new users total** — a permanent per-project cap that creating a new client ID does not reset. For one client or a handful of mailboxes this is a non-issue; for anything wider, verification is unavoidable. (The cap does not apply once the sensitive/restricted scopes you request are approved.)

**The caveat that was raised and NOT settled in-thread** — say it plainly rather than overselling the fix: Gmail's sensitive/restricted scopes can require a Google security review (a CASA assessment, reported as taking weeks) as part of getting *verified*. If the app uses restricted Gmail scopes against external accounts, treat the **Internal** route — the client owns the Google project inside their own Workspace — as the reliable fix, rather than assuming publishing alone is enough. Current requirements: https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification

### 2c — Skip all of it on n8n Cloud

n8n Cloud's built-in "Sign in with Google" runs on n8n's **already-verified** app. If the student is on Cloud and fighting their own Google Cloud project, the fastest fix is to stop: delete their custom credential and use the built-in sign-in.

### 2d — Scope hygiene

Request the narrowest scopes the workflow actually needs — read plus compose, rather than full mailbox access with send and delete. This matters for the review threshold above and for what you're asking a client to trust you with.

### 2e — It died after a DAY, not a week

That's a different token. **Temporary Meta Cloud API access tokens are short-lived** — Meta documents them as expiring in under 24 hours, and its access-token docs say "every few hours", so treat anything from a few hours to a day as consistent with this. It's the classic "worked yesterday, dead today" on WhatsApp builds. Replace with a permanent system user token. The same shape applies to any provider's "temporary" or "test" token.

### 2f — Still revoking after publishing

If tokens keep revoking after the app is published, and the account owner reports being forced to re-login to Google around the same time (a Workspace session reset), escalate to a **Google service account**. Gmail then needs **domain-wide delegation** — which is a genuine last resort, see Step 2g.

### 2g — Per-user OAuth vs Domain-Wide Delegation

Students sometimes arrive having set up DWD on an AI's advice and ask if it's safe. The honest answer:

- **DWD lets the service account impersonate ANY user in the domain**, and anyone with key-creation rights can mint a master key. Google now steers admins toward per-user OAuth except for critical bulk cases.
- **Default to per-user OAuth** (one consent click per person) for on-behalf-of automations — this is what n8n and the major connectors use.
- **Reserve DWD for unattended bulk work** (migrations, mass mailbox operations, provisioning). If you use it: never share the service-account JSON key, lock down key-creation IAM rights, scope narrowly, consider Workload Identity Federation.
- Both models can coexist in the same org.
- Regardless of model: include **`access_type=offline` plus `prompt=consent`** in the auth URL, or no refresh token is issued at all.

### 2h — Setting up the OAuth client in the first place (Phase 3 blocker)

"I followed the steps but it doesn't work" on the Phase 3 secrets-management build. OAuth bundles four distinct pieces and students conflate them: the client ID/secret, the authorized redirect URIs, the scopes, and the consent-screen test users.

1. **Use Claude Code as a live guide** through Google Cloud Console: tell it you're setting up Google OAuth 2.0 credentials for the Phase 3 project and ask it to name each menu to click, what to fill in on the consent screen, and what to copy where. Paste back what you actually see whenever a screen differs.
2. **Add your own email as a consent-screen test user** — nothing works pre-verification until you do, not even your own testing.
3. **Put the deployment platform's callback URL and the Authorized redirect URI side by side.** They must match **byte-for-byte**, including `http` vs `https`, trailing slash, and port. This is the single most common cause of "I followed the steps but it doesn't work."
4. **Secrets pattern to carry forward**: environment variables locally, secrets in the deployment platform's own dashboard, nothing committed to GitHub — the pattern taught in the Phase 3 Secrets Management lesson.
5. **For client work**: one Google Cloud project with its own OAuth credentials **per client**, never shared across clients.

<!-- pattern: /urgent-google-oauth-refresh-token-keeps-revoking-sheetsdrivegmail-need-a-permanent-fix, /building-an-ai-email-agent-for-a-friend-google-cloud-functions-vs-n8n-what-would-you-do, /google-workspaces-apis, /trouble-with-phase-3, /whatsapp-bot-by-claude-code-and-currently-living-on-render -->

---

## Step 3 — It never authenticated: wrong scheme, wrong header, wrong credential

### 3a — n8n Generic Header Auth

If the error is `Header name must be a non-empty string`, or a key the provider confirms is valid gets rejected:

1. **Open the CREDENTIAL, not the node.** The **Name** field must be exactly `Authorization`. The **Value** field is the provider's scheme, one space, then the key.
2. **Schemes differ by provider and a mismatch looks identical to a wrong key**: OpenAI and Kie.ai use `Bearer <key>`; **fal.ai uses `Key <key>` and rejects a Bearer prefix**.
3. **Check the header/auth option is actually enabled on the node** — one failure traced to Header Authorization being selected with the header toggle switched off.
4. **For well-known APIs, use Authentication → Predefined Credential Type** instead of Generic Header Auth. n8n builds the header for you and this entire error class disappears.

### 3b — You're sending the wrong credential entirely

The corpus example is worth generalising. A student calling a local bridge server kept getting "Authorization failed", trying first the ngrok **Authtoken** and then the ngrok **API key**. Neither is what the bridge validates: the Authtoken authenticates the tunnel agent in the terminal, the API key is for ngrok's own management API, and the bridge checks its **own** `BRIDGE_API_KEY` sent in an `x-api-key` header.

**Generalised check**: when a request through a tunnel or proxy fails auth, ask *"which service is actually validating this credential — the tunnel, the tunnel vendor's management API, or the application behind it?"* They are three different credentials and the error text won't distinguish them.

*(Practical note for that specific build: set Header Name to `x-api-key`, and the value to whatever string is assigned to the `BRIDGE_API_KEY` constant in the bridge's `server.js`. If it's still the setup guide's placeholder, change it in `server.js` to a long random string of your own and use that — the bridge is reachable through a public tunnel. Also: an ngrok domain ending `.dev` rather than `.app` is only a plan/region difference, not the cause of an auth error.)*

### 3c — Stop regenerating the key

Two antipatterns worth interrupting:
- **Re-creating and re-pasting an API key repeatedly** when the failure is an account-state problem (e.g. no prepaid credits). Test the key with a raw HTTP request to see the real error instead of guessing through the node.
- **Regenerating a token** when the request is actually missing a required header. Some APIs require a version header alongside auth — check the provider's API docs, or feed them to an LLM, before touching the credential again.

<!-- pattern: /the-ai-marketing-team-42625-template-but-im-stuck-on-an-error-i-cant-resolve, /kie-connection-instruction, /n8n-self-healing-need-help -->

---

## Step 4 — It authenticates but the calls do nothing

### 4a — The parameter name changed with the model

If a generation API appears to **silently ignore an input** you're clearly sending, diff your request body against the reference build's **model name AND its parameter names**, not just the values.

Corpus example: a UGC image step invented a fake product despite the real product photo URL being present in the JSON body. The request used a different Kie.ai model than the course build, and that model expects a differently-named image parameter — so the image field was ignored and the model generated from the text prompt alone.
- Match the course build: model `google/nano-banana-edit`, parameter `image_urls`, **all lowercase** (a capitalised `Image_urls` is not recognised). Map its value by expression from the sheet column holding the product image URL rather than typing a literal.
- For `nano-banana-pro` the parameter is `image_input` — verify current names on that model's API docs page before relying on it, since they differ per model.
- Rule of thumb: the `-edit` model places an existing image into a scene; the `-pro` model generates from scratch with optional reference images.

### 4b — Two-call flows misread as auth failures

Some APIs are submit-then-poll. Kie.ai is the corpus example: submit with the create/request endpoint, then poll a **separate query endpoint** using the `taskId` from the previous node. A student chasing 401 → 500 → "model cannot be null" traced it to using the wrong curl and to expression syntax, not to the credential at all.

### 4c — The dashboard lies

Two independent cases in the corpus of an API reporting success while the vendor UI shows nothing:
- **Assuming an API "success" response means the record is visible in the vendor's dashboard.** Square orders created via API stay hidden until a payment is attached. Verify via the API, not the dashboard, before debugging the workflow.
- **Reusing the same idempotency key** for an order-create call and its follow-up payment call — the payment silently doesn't go through.

### 4d — n8n MCP: Documentation tools work, Management tools fail

Not strictly third-party, but it presents identically and belongs in the same reflex: **Documentation tools serve static reference data** and work regardless of your instance; **Management tools make live REST calls**. If Documentation works and Management doesn't, **the problem is the URL, not the API key** — usually a trailing slash, or the dashboard URL used where the API endpoint URL belongs. Full detail in `knowledge/fix-patterns-claude-code.md` ("n8n MCP on Hostinger — get the URL right") and `diagnostics/lesson-1-4-n8n-mcp.md`.

### 4e — A platform-side plan gate

Some integrations simply aren't available on the tier the student is on, and the failure looks like a bug:
- **n8n API key generation** is not available on the Cloud free trial — needs a paid Cloud plan or a self-hosted instance.
- **Canva's Autofill API requires Enterprise** for both the developer and each end user — though paid non-Enterprise plans get a limited development trial before an upgrade is forced, so a student on a paid plan can still prototype. Brand kits don't influence API output at all, which is why agent-generated collateral through it stays basic no matter how the prompt is tuned. Confirm current tiering at canva.dev/docs/connect/api-reference/autofills/.
- **Claude Code Remote Control requires a claude.ai subscription login** — API-key auth is not supported, and it's unavailable on Bedrock/Google Cloud/Foundry or behind a custom `ANTHROPIC_BASE_URL`. On Team and Enterprise an Owner must enable it first. ⚠️ Don't assert a specific plan tier from memory — check the [Remote Control requirements](https://code.claude.com/docs/en/remote-control) for the current list.

Before debugging further, confirm the feature exists on their tier.

<!-- pattern: /n8n-masterclass-module-3-ucg-content-system-project-image-output-problem, /kie-connection-instruction, /phs-11-to-16, /claude-code-design-output -->

---

## Step 5 — The connector or plugin won't install

### 5a — "Incompatible auth server: does not support dynamic client registration"

Seen on the GitHub plugin, and reported on Slack. **This is not user error.** GitHub's OAuth server doesn't support Dynamic Client Registration (RFC 7591), which Claude Code's plugin auth flow expects; GitHub requires manual app registration before it issues a fixed client ID.

1. **Confirmed working**: use the **GitHub CLI** instead — run `gh auth login` and Claude Code manages repos through it. In the source thread this fully replaced the plugin.
2. Untested fallback reported in-thread (the poster had already tried a token env var without success, so treat as unverified): create a **fine-grained PAT** at GitHub Settings → Developer Settings → Personal Access Tokens → Fine-grained tokens with the repo permissions you need, set `GITHUB_PERSONAL_ACCESS_TOKEN` to that value as a system environment variable, and restart Claude Code. GitHub shows the token once at creation — copy it then.
3. **Don't keep debugging the plugin OAuth flow.** The fix has to come from Anthropic's side.

### 5b — No connector exists for the platform at all

Two paths, in order of effort:

**Use the CLI instead of an MCP.** For Google Drive inside Antigravity, the confirmed-working route was installing the **Google Workspace CLI** and letting the agent drive it. This generalises: if a well-known CLI exists, the agent can usually already drive it, and it costs a fraction of the context an MCP server does.

**Or build the MCP server.** If the vendor publishes an OpenAPI/Swagger spec, generate from it — Python: FastMCP's `from_openapi` constructor with the spec downloaded from the vendor's docs; TypeScript: Speakeasy or Stainless. **No spec published?** Have Claude Code read the vendor's API documentation and generate an OpenAPI spec first, then feed that into the generator. Curate afterwards — auto-generated servers work, but models do meaningfully better once you prune endpoints you don't need and rewrite the tool descriptions. Test with MCP Inspector.

**Before reaching for either, ask whether you need it at all.** MCP servers inject their full tool schemas into context at session start (the GitHub MCP alone exposes 90+ tools; benchmarks cited in-thread put MCP at roughly 30-35x the tokens of the CLI equivalent for the same task). Default to CLI for well-known tools; reach for MCP for organisational standardisation, enterprise auth, custom internal tools the model has never seen, or environments where the CLI isn't installed.

**And note MCP is a dev-time layer.** Deployed workflows do not need MCP servers attached — in production your code makes direct API calls and the agentic behaviour comes from the LLM SDK's native function calling. Nothing becomes hardcoded just because MCP is gone.

### 5c — Microsoft 365 / Office files

"File incompatible" when uploading a `.docx` to Claude chat: the chat interface only accepts PDFs, plain text and images (`.docx` is a ZIP of XML and is rejected outright). Pick the tier that matches the job:
- **Editing documents interactively**: install the official "Claude by Anthropic" **add-ins** for Word/Excel/PowerPoint from the Microsoft Marketplace. They need a paid Claude plan (Pro, Max, Team or Enterprise) at no extra cost, and read/write the open document natively.
- **Read-only research across a tenant**: enable the free **Microsoft 365 Connector** in Claude settings under Customize → Connectors (SharePoint, OneDrive, Outlook, Teams).
- **Automation from Claude Code**: the Softeria ms-365-mcp-server. *Documented in-thread but not confirmed working by the poster.* If you go this route, **set persistent token-cache paths in your shell profile first**, otherwise tokens are wiped on every npm update. On Windows, wrap the npx command as `cmd /c "..."` or the flags won't pass through.
- **Related antipattern**: writing values into Excel cells with a basic file tool and shipping the output — formulas don't recalculate, so stakeholder-facing numbers go out silently stale.

<!-- pattern: /github-plugin-error-for-claude-code, /how-to-access-google-drive-data-in-claude-code-via-antigravity, /creating-an-mcp, /difference-between-cli-and-mcp, /mcp-dilemma-needed-for-triggerdev, /rw-office-365-files-with-claude -->

---

## Step 6 — "It says I'm out of credits but I pay for Claude"

**A Claude Pro or Max subscription includes ZERO Anthropic API usage.** They are separately billed products. This catches students on any course module whose build calls the Anthropic API directly — the corpus case is the Scheduled Research Agent module, which fails with low-credit errors on a perfectly active Max subscription.

1. **Top up a small amount at console.anthropic.com under Billing** — the team's guidance was that $5 is plenty to complete the module (verify current minimums on the billing page).
2. **No-payment alternative**: follow the same lesson but substitute the **Gemini API**, which still has a free tier. Create the key at aistudio.google.com. ⚠️ Per-model daily limits change often and are shown per-project in AI Studio — have them check there rather than quoting a fixed number and tell Claude to use Gemini instead of the Anthropic API when prompting.

The same distinction has two other consequences worth naming when they come up:
- **Don't run a client's production helpdesk on a personal Claude subscription.** Production goes on API keys — a personal plan is for that person's own use. Point the student at Anthropic's current terms rather than paraphrasing them.
- **Consumer subscriptions run on Consumer terms; API keys run on Commercial terms built for automated use.** Running agents 24/7 from a subscription is a grey area at best — and connecting Claude or Gemini **consumer-subscription OAuth to OpenClaw has resulted in account bans**. Use API keys there.

<!-- pattern: /issue-with-module-16-scheduled-research-agent-api-credits, /openclaw-with-oauth -->

---

## Step 7 — Platform-policy failures (not fixable by configuration)

Some integration problems are the platform enforcing its rules. No amount of debugging helps; the answer is to change the approach.

- **Meta / WhatsApp**: unofficial DM automation that "mimics manual actions" gets accounts restricted. Direct Graph API hits are fine. Recovery path and the switch to an official Business Partner are in `knowledge/fix-patterns-hosting.md` ("Meta WhatsApp Business account got restricted by automated systems").
- **Facebook / LinkedIn via a logged-in personal-account session** (cookies, browser extensions): platform detection can permanently ban the account with no appeal. Use official platform APIs or cookie-free scrapers fed public URLs. For a Facebook **Page**, use the official Messenger Platform API and take the scraping out of the design entirely.
- **A perfectly regular polling interval is itself a detection signal** on bot-protected platforms. Every 15 minutes on the dot is a fingerprint.
- **US SMS**: A2P 10DLC registration is a carrier-registry requirement that follows every US provider. Switching Twilio → Telnyx → Plivo to escape it just buys a fresh registration fee and a new place in the queue. Registering with placeholder company names, or "LLC" in the brand name when no LLC exists, gets rejected by human reviewers. The consent checkbox on an opt-in form must be **unchecked by default** — the user has to actively tick it (this is what Twilio error 30925 is about). Separately, consent must not be a condition of purchase. Don't confuse the two: making the field "optional" while leaving it pre-checked still gets rejected.
- **Cloudflare Enterprise** checks TLS fingerprints at handshake — adding headers does nothing. See the web-scraping patterns in `knowledge/taxonomy.md` **§ 13 (Web Scraping)** — read that section only, not the whole file.

For all of these: name the constraint plainly, give the compliant alternative, and don't help engineer around a platform's ban policy.

---

## If none of the branches matched

Switch to drafting a Support Needed‼️ post (see SKILL.md Escape Hatch B). For integration issues, pre-fill:

- **Which third-party service**, and which specific endpoint or node
- **Did it ever work, and for how long** — the single most diagnostic fact in this category
- **The exact error text**, verbatim, including any error code
- **Where the credential came from** — which dashboard, which key type, and whether it was created by the student or the client
- **Auth method** — OAuth, API key, service account, header auth — and the exact header name if header auth
- **Plan tier** on both sides (their Claude plan, and their tier on the third-party platform)
- **Whether it fails identically outside the build** — a raw HTTP request or curl against the same endpoint separates "my workflow is wrong" from "this credential doesn't work"

That last one is worth insisting on: it halves the diagnostic space and the corpus shows students rarely try it unprompted.

No need to tag anyone — the support team watches Support Needed‼️. Remind them to retitle the post **[SOLVED]** once it's resolved, and to redact keys and client data from any screenshot.
