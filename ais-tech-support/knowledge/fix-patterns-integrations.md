# Fix Patterns — Third-party auth & integrations

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

See also `diagnostics/third-party-integrations.md`, which is the routed entry point for third-party API and OAuth problems.

---

## THIRD-PARTY AUTH — Google OAuth

## Google OAuth app left in Testing mode — refresh tokens die every 7 days
**Symptom**: Gmail/Drive/Sheets automations 'just stop working' about weekly; credentials need re-auth; a handed-off client build dies a week after go-live.

**Root cause**: Google Cloud OAuth apps left in 'Testing' publishing status issue refresh tokens that expire after 7 days, so any Gmail/Drive/Sheets credential silently dies about weekly. Publishing the app to 'In production' is an instant action and is separate from Google's app-verification review.

**Fix steps**:
1. If every mailbox/account involved is inside one Google Workspace: set the OAuth consent screen User type to Internal. No 7-day expiry and no Google review. Note the Internal option only exists when the Cloud project sits inside a Google Workspace / Cloud Identity org — on a personal gmail.com account it won't be shown, so if the student can't find it, go straight to step 2.
2. Otherwise: OAuth consent screen > Publish App ('In production'). Publishing is instant and is a different process from verification; contributors in these threads report a private, low-user client tool continuing to work unverified. Reconnect the credential once afterwards, and anyone signing in clicks Advanced > Continue past the 'app isn't verified' warning one time.
3. Caveat raised in the same threads and NOT settled there: Gmail's sensitive/restricted scopes can require a Google security review (a CASA assessment, reported as taking weeks) before those scopes are served to accounts outside your own org. If your app uses restricted Gmail scopes against external accounts, treat the Internal route (the client owns the Google project inside their own Workspace) as the reliable fix rather than assuming publishing alone is enough.
4. Request the narrowest scopes the workflow actually needs, e.g. read plus compose rather than full mailbox access with send and delete.
5. On n8n Cloud you can skip all of this: the built-in 'Sign in with Google' runs on n8n's already-verified app.
6. If tokens still revoke after publishing, and the account owner reports being forced to re-login to Google around the same time (a Workspace session reset), escalate to a Google service account. Gmail then needs domain-wide delegation, which is a last resort - see the per-user-OAuth-vs-DWD pattern for the security tradeoffs.

**Confidence**: medium — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/building-an-ai-email-agent-for-a-friend-google-cloud-functions-vs-n8n-what-would-you-do
- https://www.skool.com/ai-automation-society-plus/urgent-google-oauth-refresh-token-keeps-revoking-sheetsdrivegmail-need-a-permanent-fix
- https://www.skool.com/ai-automation-society-plus/need-helpadvice-on-client-creds-for-n8n

<!-- pattern: /building-an-ai-email-agent-for-a-friend-google-cloud-functions-vs-n8n-what-would-you-do -->

---

## Per-user OAuth vs Domain-Wide Delegation for Google Workspace automations
**Symptom**: Student sets up a DWD service account on an AI's advice and asks if it's safe.

**Root cause**: DWD lets the service account impersonate ANY domain user and anyone with key-creation rights can mint a master key; Google now steers admins to per-user OAuth except for critical bulk cases.

**Fix steps**:
1. Default to per-user OAuth (one consent click per person) for on-behalf-of automations — what n8n and major connectors use.
2. Reserve DWD for unattended bulk work (migrations, mass mailbox ops, provisioning); if used: never share the SA JSON key, lock key-creation IAM rights, scope narrowly, consider Workload Identity Federation.
3. Include access_type=offline plus prompt=consent in the auth URL or no refresh token is issued.
4. Both models can coexist.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/google-workspaces-apis

<!-- pattern: /google-workspaces-apis -->

---

## Google OAuth client setup overwhelm — the four pieces and the redirect-URI trap
**Symptom**: Stuck creating OAuth client ID/secret for Phase 3 secrets management; 'followed the steps but it doesn't work'.

**Root cause**: OAuth bundles four distinct pieces (client ID/secret, authorized redirect URIs, scopes, consent-screen test users); the top failure is a redirect URI that doesn't byte-match the platform's callback.

**Fix steps**:
1. Use Claude Code as a live guide through Google Cloud Console: tell it you are setting up Google OAuth 2.0 credentials for the Phase 3 project and ask it to name each menu to click, what to fill in on the consent screen, and what to copy where. Paste back what you actually see whenever a screen differs from its description.
2. Add your own email as a consent-screen test user - nothing works pre-verification until you do, not even your own testing.
3. Put the deployment platform's callback URL (Trigger.dev's, in the Phase 3 build) and the Authorized redirect URI registered in Google Cloud side by side. They must match byte-for-byte including http vs https, trailing slash and port. This is the single most common cause of 'I followed the steps but it doesn't work'.
4. Secrets pattern to carry forward: environment variables locally, secrets stored in the deployment platform's own dashboard, nothing committed to GitHub - the pattern taught in Phase 3 lesson 1.5 Secrets Management.
5. For client work: one Google Cloud project with its own OAuth credentials per client, never shared across clients.

**Lesson reference**: Lesson 1.5 Secrets Management covers the pattern — verify lesson number

**Confidence**: medium — team-verified, lesson-backed

**Example thread**: https://www.skool.com/ai-automation-society-plus/trouble-with-phase-3

<!-- pattern: /trouble-with-phase-3 -->

---

## VOICE AGENT TELEPHONY — carriers & numbers

## "My country isn't supported by Vapi / Twilio" — it's a provider-shopping problem, not a blocker
**Symptom**: Student wants to build a voice agent but reports their country is not supported by Vapi, Retell or Twilio, and treats it as a hard stop. Variants: 'which telcos in my country work with Vapi', 'what should I ask my local carrier'.

**Root cause**: The premise is wrong, and correcting it is most of the fix. Vapi and Retell do not connect to national telcos and are not tied to a country's carriers. They ride on top of a programmable telephony provider over a SIP trunk. A regular local number cannot sit behind a SIP endpoint, so the student is not asking a telco anything. They are choosing which programmable provider has numbers and pricing in their country, then pointing it at Vapi or Retell. Both platforms accept bring-your-own SIP, so a gap in Twilio's coverage is not a gap in Vapi's.

**Fix steps**:
1. Check Telnyx coverage for their country first: https://telnyx.com/global-coverage. Filter on voice inbound and local outbound, not SMS. In many countries where Twilio is thin or toll-free-only, Telnyx has local numbers.
2. If Telnyx is thin, check Plivo: https://www.plivo.com/phone-numbers/. Vapi has first-party setup guides for both, so neither is an unsupported hack: Telnyx https://docs.vapi.ai/telnyx, Plivo https://docs.vapi.ai/advanced/sip/plivo.
3. If both come up empty, a regional SIP carrier plus the bring-your-own SIP trunk route still works: https://docs.vapi.ai/advanced/sip/sip-trunk.
4. Warn about the regulatory step, which is the thing that actually delays people. Most countries require proof of local address or business registration before releasing a local number. Budget days, not an afternoon.
5. Sequence matters: get the number sorted before building the agent. Building first and discovering the only obtainable number is a foreign toll-free changes both call economics and how calls land.
6. Non-English target market: pair with ElevenLabs and clone a voice from samples reading phrases the agent will actually use. A stock voice in a non-English language is a common quality complaint.

**Known exception**: India. TRAI rules require SIP termination on Indian servers, which Vapi does not currently support, so Indian numbers are not compatible. Verify per country rather than generalising this to other markets.

**Volatile**: per-country carrier coverage and pricing change often. Always route the student to the coverage pages above rather than asserting whether a specific country is covered.

**Confidence**: medium — team-verified reframe (a team member gave the SIP/provider reframe and the Telnyx-then-Plivo recommendation in a solved thread); provider support and the India exception confirmed against current Vapi documentation.

**Example thread**: https://www.skool.com/ai-automation-society-plus/telcos-in-my-country-are-supported-by-vapi-or-resell

<!-- pattern: /telcos-in-my-country-are-supported-by-vapi-or-resell -->

---

## Where the voice agent lessons live (most are Archived)
**Symptom**: Beginner asks for voice agent tutorials or which stack to use. Follow-up risk: they open a build lesson, the UI does not match the recording, and they think they broke something.

**Root cause**: The hands-on Vapi and ElevenLabs builds sit in the **Archived** course, so their UIs have drifted. The only voice lesson in a current course is conceptual, not a build.

**Fix steps**:
1. Start conceptual: **AI Voice Agents** in Build Your Portfolio, roughly 5 minutes. https://www.skool.com/ai-automation-society-plus/classroom/d99017d2?md=964fbaa3c01e4ef899e60cb0be3f5655
2. Then the builds, flagging Archived status out loud so the drift is expected rather than alarming:
   - AI Receptionist: n8n MCP / Vapi https://www.skool.com/ai-automation-society-plus/classroom/ec6512da?md=f6a9fb90a9a44e00b13d0b8384c01ee2
   - Lead Qualifier: n8n / Vapi Outbound https://www.skool.com/ai-automation-society-plus/classroom/ec6512da?md=9bd1d84482694052b001f2a0edd6a0ab
   - ElevenLabs Voice RAG Agent https://www.skool.com/ai-automation-society-plus/classroom/ec6512da?md=592a084c42f444ec80e955fc3e7c5f25
3. Tell them to match the end state rather than the clicks, per the standard 'it doesn't look like the video' guidance.
4. Vapi plus n8n, with ElevenLabs for the voice, is the stack the course builds with. Confirming a beginner's stack choice is a useful answer on its own.

**Confidence**: high — lesson-backed, ids verified against `knowledge/classroom-map.md`

---
