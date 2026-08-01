# Diagnostic — n8n Workflow Not Working (Building & Quality Issues)

Use this when a student has an n8n workflow that runs but does the wrong thing, stops silently, never fires, or costs too much. Trigger phrases: "my workflow stops halfway", "the agent never uses its tools", "it fires twice", "the trigger returns nothing", "nodes are locked", "expression not working", "it says success but nothing happened", "the loop never finishes", "my RAG agent ignores the knowledge base".

**53 threads (37 newly ingested)** — the largest technical category after course access. Distinct from `diagnostics/lesson-1-4-n8n-mcp.md`, which covers *connecting* Claude Code to n8n. This file is about the workflows themselves.

**Lead with this**: the single most useful move in almost every one of these threads is **reading the execution log before theorising.** Which node ran, which node shows zero items, which node is still waiting. Students consistently describe symptoms in terms of the final output and skip the one artifact that localises the fault. Ask for it early.

**Second thing to know**: a startling share of these are not logic bugs at all. Unpublished workflows, unactivated triggers, trailing whitespace, a branch wired to the wrong output, an unchecked settings toggle. Check the cheap things first — Step 2 exists for exactly this reason.

---

## Step 1 — Triage question

Ask ONE question (or read it from the transcript):

> **"Does the workflow run at all — and if it runs, where in the execution log does it stop or go wrong?"**

Then branch:

| What they say | Go to |
|---|---|
| "It never runs / the trigger does nothing" | **Step 3** — triggers |
| "It runs but stops partway / a branch never fires" | **Step 4** — dead branches and item counts |
| "It runs to the end but the DATA is wrong" | **Step 5** — expressions and data references |
| "The AI agent doesn't use its tools / ignores the knowledge base" | **Step 6** — agents and RAG |
| "A node errors with a credential or auth message" | **Step 7** — credentials and auth |
| "It works but is slow / expensive / times out" | **Step 8** — cost, timeouts and scale |
| "Claude Code built it and something's off about the workflow itself" | **Step 9** — Claude-built workflow quirks |

**Always run Step 2 first regardless of branch** — it's four checks and it resolves a meaningful fraction of these outright.

---

## Step 2 — The four cheap checks (do these before anything else)

1. **Is the workflow published/activated?** Older n8n says Active/Deactivate, newer says Publish/Unpublish — same toggle, and students think they're different things. An unpublished workflow with a live trigger does nothing. This was the entire problem in at least one thread. Note also that a trigger only fires for events arriving **after** activation — older items sitting in the inbox won't retroactively trigger it.
2. **Stray whitespace in URL and expression fields.** A single space between a slash and an inserted `{{ }}` variable — usually carried in by dragging a variable — makes the resource path invalid and is nearly invisible. Inspect the failing field character by character around slashes and insertions. Make this a standard first check whenever a node fails while "looking right".
3. **Are you testing the right way for what you're testing?** Manual canvas runs and production runs behave differently in three ways that each produce a "it's broken" report:
   - **Error workflows only fire on production executions.** A correctly-assigned error workflow will never trigger during intentionally-failing manual tests. To test one: swap the Manual Trigger for a Webhook node, activate, and hit the Production URL.
   - **Manual executions use more memory** than production runs, because n8n copies the data for the UI display. High-volume workflows can OOM in manual runs and be fine in production.
   - **On n8n Cloud, production-URL testing burns your monthly execution quota.** The Test URL plus Execute workflow gives unlimited free test executions.
4. **Read the execution log, not the output.** Open the Executions tab, click into the failing run, and look at which node shows zero items and which is still waiting. A stalled node showing as *waiting* means a multi-input stall (Step 4), not a config error in the node that appears to stop.

<!-- pattern: /google-sheet-data-isnt-parsing, /google-drive-issue-move-files, /n8n-self-healing-need-help, /telegram-bot-burning-n8n-cloud-limits -->

---

## Step 3 — The trigger doesn't fire (or fires twice)

### 3a — Fires exactly twice, every time

Check in this order:
1. **Extra Schedule Trigger nodes on the canvas** — even disconnected ones still fire.
2. **A duplicate active copy** of the workflow.
3. **Stale trigger registration** — deactivate, wait ~10 seconds, reactivate. If needed, copy into a brand-new workflow.
4. **Self-hosted and still doubling**: run `docker ps | grep n8n`. More than one container sharing the same database means every schedule fires once per container. Remove the replica. *This was the confirmed cause in the source thread.*

(The `0/2` badge next to a workflow is the Production Checklist. Cosmetic. Unrelated.)

### 3b — Gmail trigger returns no data

Several causes stack — check all of them:
- **Self-starvation**: an unread-only trigger combined with a mark-as-read step at the end consumes its own trigger condition. Team members opening the mail manually finishes the job. Set Read Status to "unread and read", or stop marking test mail as read.
- **Website form notifications arrive From the site's own mailer address**, not from the lead. Any filter set to the leads' addresses matches nothing. Filter on the notification mailer's single address instead, then pull the lead's real email from the **Reply-To** header and parse name/phone by the form's fixed field labels rather than generic regex.
- **Reported quirk**: the trigger's Sender field may only accept a single address — comma-separated lists silently match zero. Test with one address before assuming the filter is broken.
- **General IF-node rule**: multiple "From equals X" conditions combined with AND can never all be true for one email. Use ANY, or move the filter to the trigger.

### 3c — Production webhook returns 200 with an empty body

The confirmed fix: **toggle the workflow Active/Published off and back on** to re-register the production webhook, then re-send. After a workflow is edited, an active workflow can keep serving a stale registration, so requests hit the old version of the flow.

If that doesn't resolve it — two unconfirmed suggestions from the thread, offer as leads: open a real production execution in the Executions tab, click the Webhook node and read the actual output JSON before hard-coding an expression path (the field may sit at a different level in production than in test); and a not-found lookup may be silently ending the branch (see Step 4).

### 3d — Schedule triggers stop firing with nothing in the log at all

Not even failures — this is n8n Cloud degradation, covered in Step 8c.

<!-- pattern: /help-with-dual-executions-in-n8n, /google-sheet-data-isnt-parsing, /interactive-live-avatar-support-needed -->

---

## Step 4 — It stops partway / a branch never fires

**The governing rule: a node that emits ZERO items halts its branch.** There is nothing for the next node to run on. This is the most common silent-stop cause in the corpus.

### 4a — After a lookup or delete that matched nothing

1. Open the node's **Settings** and enable **"Always Output Data"** so an empty result emits one empty item instead of zero.
   - ⚠️ **Do NOT set this on an IF node** — it can cause a loop.
2. Route explicitly with an **IF right after the lookup**: key field populated → existing-record path; empty → create-new path.
3. When a node pushes through a blank item, make sure downstream nodes reference their values from the **original trigger**, not from that node's now-empty output.
4. Sanity check: confirm the value you're searching for actually exists in the target sheet. An empty lookup result is *correct* for a genuinely new record — that path needs handling, not fixing.

### 4b — A node with multiple incoming connections never runs

**A node with multiple inputs waits for ALL of them to deliver before it runs.** An optional branch that produced nothing leaves it waiting forever. The execution log shows the node as *waiting* — that's your confirmation.

Fix: add an IF right after the trigger checking whether the optional data exists (e.g. `{{ $binary.Photos }}` — substitute the actual field name), send the populated branch down the normal path, and route the empty branch through a **No Operation** node that also connects to the downstream node. It then always receives input on both connections.

Same shape applies to **Merge nodes**: a Merge waits for input on every branch connected to it, so a conditional retry path that never fires stalls the execution forever. For parallel media generation with retries, the suggestion in-thread (untested) was to give each item its own dedicated always-firing branch into a single Merge configured with one input per item.

### 4c — Loop Over Items writes phantom rows or never finishes

Symptom: wrong/invented values in the destination sheet, IDs that don't line up with source rows, and an execution that has to be cancelled manually.

Cause: **the per-item work was wired to the loop node's "done" branch** — which fires once, after all items — **instead of the "loop" branch**, which fires once per item.

1. Hover each of the two outgoing connections ("done" and "loop") and **delete both**.
2. Connect **only "loop"** to the per-item node.
3. Leave "done" for steps that should run once after the whole loop finishes, or unconnected.
4. Re-run. It should now complete on its own.

<!-- pattern: /submission-form-is-not-passing-through-information-if-binary-file-is-not-attached, /help-with-dynamic-rag, /issue-faced-whilst-building-sales-bot-in-live-sales-bot-tutorial, /handling-sora-2-api-failures-in-n8n-need-help-with-logic -->

---

## Step 5 — It runs but the data is wrong

### 5a — Wrong item, or only one of many items processed

Two stacked causes:
- **`{{ $json.x }}` reads the immediately-previous node**, not the node you meant. Pin the source explicitly: `{{ $('UPLOAD_NODE_NAME').item.json.id }}` — replace `UPLOAD_NODE_NAME` with the exact canvas name of the node whose data you need.
- **An intermediate single-item node** (create-folder, append-row) collapses the item count to 1, so everything downstream runs once. Check item counts between nodes. Insert a **Merge** node: multi-item source into Input 1, single-item node into Input 2. Reference the single-item value with `.first()` — e.g. `{{ $('CREATE_FOLDER_NODE').first().json.id }}`.

### 5b — Date comparisons in Filter / IF nodes fail

Including `the string '' can't be converted to a dateTime`. Three stacked issues:
1. **Remove the `.format(...)` call** — it converts a DateTime into a plain string while the comparison operator expects DateTime objects on both sides. Use e.g. `{{ $now.minus({days: 2}) }}` bare.
2. **Turn on "Convert types where required"** at the bottom of the Filter/IF node.
3. **Blank date cells** have nothing to convert. For test data, fill the empty cells and click "Test step" on the Sheets node to pull fresh data. For production: `{{ $json['YOUR_DATE_COLUMN'] || $now.minus({days: 365}) }}` — replace `YOUR_DATE_COLUMN` with the exact column header from their sheet. Rows with no date then read as a year ago and still route through, which is usually the desired behaviour for new leads.

### 5c — "JSON parameter needs to be valid JSON" on an HTTP Request body

1. **Check for duplicated quotes first** — students wrap the expression in their own quotes on top of quotes already there. If there are two sets around the prompt variable, delete one. In the source thread this alone cleared the error.
2. Then wrap the value so LLM text can't break the JSON: `{{ JSON.stringify($json.output) }}` — adjust `$json.output` to whatever field holds the generated text.
3. **Remove any quotes you added around it** — `JSON.stringify` adds its own, and leaving yours recreates the doubled-quote problem.

### 5d — Values arrive with a trailing `\n`, and lookups return empty

Reported causes stack:
1. **Trim at the source** — this is what actually resolved it. Apply `.trim()` to the inbound text field in the node that prepares the data. Text fields only; it errors on numeric fields like scores.
2. **Check row 1 for hidden line breaks** in the sheet headers. Put `=LEN(A1)` next to a header cell and compare to `=LEN(TRIM(CLEAN(A1)))`. If they differ, clean the row with `=ARRAYFORMULA(TRIM(CLEAN(A1:Z1)))` in a spare cell, paste back over row 1 as Values Only, then reopen the Sheets node and **re-select the mapping columns** so it picks up the clean header names.
3. If lookups still return empty after the values are clean: the student who solved this traced it to the Google Sheets node running at **version 4.5**. Delete that lookup node and add a brand-new Google Sheets node in its place so it installs at the current version.

### 5e — IDs lost between nested AI agents

Symptom: a sub-workflow tool fails with "could not access the source file" because a Drive file ID didn't survive being relayed through two nested agents.

Cause: `$fromAI('...')` parameters depend on one LLM writing the value into its text message and the next LLM extracting it back out. **There is no structural data pipe between the hops** — an exact identifier can be dropped or mangled purely on LLM behaviour.

1. Trace the value's full path and find the exact hop where it stops being structured data and becomes text. That hop is the break.
2. Mitigation: add explicit prompt rules to **BOTH** the orchestrator and the sub-agent to pass file IDs through verbatim.
3. Structural fix that doesn't depend on LLM cooperation: replace the `$fromAI('...')` parameter in the toolWorkflow node with a **direct expression** reading the ID from the workflow data.

### 5f — Can't pin data to iterate

Pin is greyed out on Text Classifier, IF and Switch. **Pinning only works on single-main-output nodes** — multi-branch nodes can't be pinned, and what looks pinned in videos is execution output. Pin the **trigger** node's output instead (execute once with a real item) and re-run the workflow against it.

<!-- pattern: /drive-node-move-file-issue, /fast-track-challenge-agent-2, /error-message, /node-adding-n-to-output-values-breaking-downstream-lookup-node, /error-with-ultimate-agent-workflow, /cant-pin-text-classifier-node -->

---

## Step 6 — The AI agent misbehaves

### 6a — The agent never calls its tool

Output panel often hints "None of your tools were used in this run."

Two causes, usually together:
1. **The tool's description is on "Set Automatically"** — n8n auto-generates generic text the model deems irrelevant. Write a **custom Tool Description** stating what the tool does and **when it MUST be called**.
2. **n8n expressions inside the system prompt's Tools section pre-resolve**, so the model sees the data already sitting there and concludes it doesn't need the tool. Keep the Tools section as plain descriptions with no `{{ }}`; move dynamic values into **Instructions** instead — e.g. "ALWAYS call Search_Client first; the phone number is `{{ $json.phone_number }}` — pass it to the tool."

Re-run and confirm the "none of your tools were used" banner is gone.

### 6b — The RAG agent skips the vector store

Answers look plausible but never come from the documents.

1. **Read the execution chain first.** If the vector store node never appears between the agent and the model, the tool is being skipped — the answer came from the LLM's own knowledge, not the documents.
2. Add an explicit system-prompt instruction: *"Always check the knowledge base using the available tools before answering."* Without it, the model answers from training for any topic it already knows.
3. **Check the RETRIEVAL-side vector store node has an embedding model connected below it**, matching the one used at ingestion. The ingestion workflow having its own embedding setup does not cover the retrieval side — without one, the node can't turn the question into a vector to search with.
4. Re-test with a question only your documents can answer, and confirm the vector store node now appears in the log.

### 6c — The agent hallucinates right after the user says "yes"

The orchestrator passes the literal "yes" as the retrieval query, which matches nothing. Either restructure the flow to deliver data **with** the answer instead of asking a follow-up, or add a query-synthesis rule: *"when using the knowledge tool, ALWAYS formulate a complete detailed query from conversation context; never pass a bare confirmation."*

### 6d — The RAG chatbot is slow and token-hungry

~30s and ~8K tokens per answer, and more knowledge makes it worse:
1. **Lower the vector store `limit`** (try 2) and compare time/tokens.
2. **Re-chunk so Q&A pairs stay together** — chunk 1500 / overlap 200 were the working values. Delete and re-upload broken chunks.
3. **Replace the retrieval sub-workflow's AI Agent with a Question & Answer Chain** (small model, minimal prompt, Vector Store Retriever in "Retrieve Documents (As Vector Store for Chain/Tool)" mode). A full Agent node in a retrieval-only sub-workflow is redundant when the main agent already rewrites the answer.

### 6e — Text Classifier: "Expected object, received array"

`OUTPUT_PARSING_FAILURE` even though the classification values in the error text look right.
1. **First try**: open the OpenAI Chat Model node feeding the classifier and toggle **OFF** the "Responses API" option. Several community threads report this as the fix (reported, not confirmed in this thread).
2. **Confirmed workaround from the source thread**: feed the classifier a field that arrives as an object rather than the array-wrapped one — in an email workflow the poster switched from the text field to the HTML field and it proceeded normally.
3. Alternative: use an **Information Extractor** node instead.

### 6f — Simple Memory errors with two triggers in one workflow

1. **Confirmed fix in that case: update n8n** — the issue disappeared on 2.11.3, pointing at a platform bug rather than a config mistake (the team could not reproduce it).
2. **Also helped, and the better architecture anyway**: split so each trigger lives in its own workflow.
3. Worth trying though it didn't help that student: open the Simple Memory sub-node, switch the key from "Connected chat trigger node" to "Define below", execute the chat trigger, and drag the session ID variable from its output into the key field.

Related: **Simple Memory only remembers conversation TEXT** with a default window of 5 **interactions** (n8n counts a user message + AI reply as one interaction, so 5 ≈ 10 messages). Binary data and side-channel JSON never enter history, which is why an agent "forgets" an uploaded file's link. Persist such links by appending them to the user message as text (e.g. a `[System Note: ...]` line the system prompt marks internal-only) and raise the context window past 5.

<!-- pattern: /ai-agent-not-using-the-tools, /supabase-vector-store-doesnt-seem-to-work, /prompt-engineering, /rag-more-info-better-answers-less-money-and-takes-double-time, /quick-question-ef67236e, /two-triggers-active, /sending-photos-in-chatbots -->

---

## Step 7 — Credential and auth errors

### 7a — "Header name must be a non-empty string" / a valid key rejected

The **credential** is misconfigured, not the node.
1. Open the Header Auth **CREDENTIAL** itself. The **Name** field must be exactly `Authorization`. The **Value** field is the provider's scheme, one space, then the key.
2. **Schemes differ by provider**: OpenAI and Kie.ai use `Bearer <key>`; **fal.ai uses `Key <key>` and rejects a Bearer prefix.** A mismatched prefix looks identical to a wrong key. Get the key from that provider's own dashboard.
3. Confirm the header/auth option is actually **enabled on the node** — one fal.ai failure traced to Header Authorization being selected with the header toggle switched off.
4. For well-known APIs, prefer **Authentication → Predefined Credential Type** over Generic Header Auth. n8n then builds the header for you and this entire error class disappears.

### 7b — Nodes are locked and uneditable in a Claude-built workflow

The UI offers only to request credential sharing or duplicate the node. Claude Code either created the credentials itself or used a placeholder, and because the account didn't create them, n8n doesn't recognise them as yours.
1. **Immediate unblock**: duplicate the locked node and attach your own credential to the copy.
2. **Permanent fix**: create each credential yourself in the n8n UI first, then tell Claude Code to reference those existing credentials **by name** when it builds.
3. Untested community suggestion: keep a dummy template workflow with your common credentials attached and point Claude Code at it so new builds inherit the auth.

### 7c — An imported course template doesn't work

Three things in order:
1. **Activate/publish** it — imported workflows arrive unactivated, so triggers never fire.
2. **Open the Executions tab** to find the failing nodes, and **add YOUR OWN credential/API key on every generation node** — they ship with the instructor's placeholders.
3. **Unpin any leftover pinned test data or prompts**, which is why the bot repeats stale output.

### 7d — Google OAuth dies about weekly

If a Gmail/Drive/Sheets automation "just stops working" roughly every 7 days, the Google Cloud OAuth consent screen is in **Testing** mode. See `knowledge/fix-patterns-integrations.md` → "Google OAuth app left in Testing mode". This is the top cause of handed-off client builds dying a week after go-live.

### 7e — Model/parameter mismatch on image APIs

If a generation API appears to **ignore an input** you're clearly sending, diff your request body against the reference build's **model name AND its parameter names**, not just the values. Kie.ai's nano-banana models are the corpus example: `nano-banana-edit` expects `image_urls` (all lowercase — a capitalised `Image_urls` is not recognised) while `nano-banana-pro` expects `image_input`. Swapping the model without renaming the parameter means the image field is silently dropped and the model generates from the text prompt alone.

<!-- pattern: /the-ai-marketing-team-42625-template-but-im-stuck-on-an-error-i-cant-resolve, /kie-connection-instruction, /claude-code-duplicating-nodes, /n8n-marketing-automation, /n8n-masterclass-module-3-ucg-content-system-project-image-output-problem -->

---

## Step 8 — Slow, expensive, or timing out

### 8a — Retries never happen and the error branch never fires

1. **Retry On Fail was set on the circular Chat Model sub-node** — that node only holds configuration. Move Retry On Fail to the **rectangular executing root node**.
2. Set **On Error to "Continue (Using Error Output)"**. Left at "Stop Workflow", the execution is killed before the error branch can receive anything.
3. Mind the **~5-minute n8n Cloud execution timeout** for long generations; self-hosted can raise `EXECUTIONS_TIMEOUT`. Test AI nodes with varied input sizes to find the threshold before scheduling unattended runs.

### 8b — The pipeline burns API credits

Ordering and node-choice fixes, most impactful first:
1. **Move the duplicate check BEFORE any AI classification.** Items processed on a previous run get skipped before you pay for a model call.
2. **Fetch all known keys once before the loop** and use a **Compare Datasets** node, instead of hitting the database per item inside the loop.
3. **Fix any erroring Structured Output Parser** — every failed parse is a paid retry. Adding an example of the expected JSON to the prompt, or simplifying an over-strict field type, usually clears it.
4. **Don't use an AI Agent node for tool-less classification.** Use a Chat Model plus a parser, or the Text Classifier node. Reserve Agent nodes for steps that actually have tools attached.
5. If timeouts are the pain, move the stages after dedup into a **child workflow via Execute Workflow** so one slow item can't take down the batch.
6. On batch size, try **2-3 items at a time** rather than jumping between 1 (stable but slow) and the whole set.

*(These are team optimization recommendations; the student hadn't reported measured results when the thread closed.)*

### 8c — n8n Cloud degraded: 502/524 on publish, schedules silently dead

The instance is out of memory, backed up by a very large stored execution history on heavy workflows.
1. **Confirmed fix**: copy the whole workflow into a **brand-new workflow**, activate that one, and delete the bloated original. Repeat for any other workflow carrying excessive execution history.
2. **Tried first and it did NOT help**: restarting the workspace. Quick enough to try, but don't stop there.
3. **Prevention**: workflow menu → Settings → set "Save production executions" to "Do not save" on the heaviest workflows, or clear the Executions tab periodically. n8n Cloud prunes execution logs on a per-plan schedule (Starter is the tightest, Enterprise unlimited) — check the current limits for their plan at docs.n8n.io/deploy/use-n8n-cloud/configure-cloud/manage-your-data before concluding this was a one-off.
4. Still failing → open a support ticket with n8n from the same admin panel.

### 8d — Approaching Cloud memory/execution ceilings

Immediate headroom on the current plan: process data in smaller chunks with **Split In Batches**; break complex workflows into **sub-workflows** (each releases its memory on completion rather than holding it for one long execution); stop testing via manual executions.

The decision when that runs out: **Enterprise** = zero migration at a steep premium (custom quote, typically starting around $2,000+/mo); **self-hosting** ≈ $150-300/mo for a starter setup up to $400-800/mo production-grade with monitoring and redundancy (early-2026 ballparks), unlimited executions, plus ongoing DevOps time.

**Don't switch platforms.** A year of business-critical workflows is a massive rebuild and per-operation pricing elsewhere adds up fast at volume. Migrate in stages: stand up the self-hosted instance, move a few non-critical workflows to validate performance, then the business-critical ones.

### 8e — Self-hosted at scale: Postgres lock contention

Queue-mode instance with many workers overloading Postgres, with top lock waits on `INSERT ... ON CONFLICT` into `workflow_statistics`:
1. Set **`SKIP_STATISTICS_EVENTS=true` on ALL instances** (main, webhook, workers). Note `N8N_DISABLED_MODULES=insights` does **not** help — Insights is a separate code path.
2. Tune execution data: `EXECUTIONS_DATA_SAVE_ON_SUCCESS=none`, keep `ON_ERROR=all`, `ON_PROGRESS=false`; confirm pruning is on and tighten `EXECUTIONS_DATA_MAX_AGE`.
3. Raise `DB_POSTGRESDB_POOL_SIZE` (default 2 → ~5/worker, 10 main) with Postgres `max_connections` headroom; consider PgBouncer.
4. If pressure persists, consolidate always-together lightweight subworkflows back into their parents.

<!-- pattern: /open-ai-node-request-time-out, /optimise-n8n-system, /struggling-with-n8n-cloud-triggers-not-firing-502524-errors-anyone-else, /advice-on-n8n-scalingalternatives, /workflowstatistics-locking-db -->

---

## Step 9 — Claude Code built it and the workflow itself is off

### 9a — Nodes snap back / canvas edits don't stick

Dragging a node moves it back; manual edits revert; Claude claims it fixed things that never change on the canvas.

**n8n issue #27638 (fixed — check their version first)**: when a workflow was created through the MCP's `create_workflow_from_code` tool, the nodes could be saved under a `settings` property instead of the root-level `nodes` array, so the canvas editor didn't track them as editable. This was confirmed on n8n 2.13.3 stable / 2.14.2 beta and closed by PR #32341, so have the student upgrade rather than hand-editing JSON. If it reproduces on current n8n, it's a new issue worth filing.

1. **Check whether the upstream issue has been fixed** before doing anything destructive — it was unpatched at the time of the thread.
2. **Quickest route**: rebuild the workflow manually in the n8n editor so nodes are stored correctly from the start.
3. **JSON route** if it's too complex to rebuild: export the workflow JSON, move the node definitions from `settings.nodes` into the root-level `nodes` array, and reimport.
4. **Prevention until patched**: let Claude Code plan and generate the workflow logic, but build and deploy it manually rather than deploying through the MCP.

### 9b — Claude Code suddenly inventing n8n nodes

Workflows that built fine last week now contain made-up nodes, often right after a new model release.
1. **Confirm the n8n skills repo is not just cloned but actually REFERENCED in your CLAUDE.md.** Reviewing the CLAUDE.md was the step the student had missed, and fixing it resolved her case. (The skills repo is taught in lesson `1.5 n8n Skills`.)
2. **Connect the n8n MCP** so Claude reads real node schemas instead of guessing. Without it, invented nodes are expected behaviour.
3. If it started right after a new Opus release, try **Sonnet** for n8n workflow building and make sure the CLI itself is up to date — the CLI sometimes lags a model release.
4. Use `/clear` between unrelated tasks, and at most one deliberate `/compact` mid-task; the student credited this session hygiene as part of what fixed her sessions.

### 9c — Claude Code keeps asking YOU to check n8n

The student ends up relaying execution logs and screenshots by hand. Claude Code has no access to the system it's debugging, so it uses the human as its eyes.
1. **Give it eyes**: with the n8n MCP connected (or n8n API access), tell it explicitly — *"use the n8n MCP tools to inspect the most recent execution and find where it failed; if MCP is unavailable, use the API."* If n8n is on a VPS, tell Claude how to reach it (e.g. over SSH) so it can fetch logs itself.
2. **Cheaper alternative** when you don't want it crawling the whole workflow JSON: open the failed node, copy its execution JSON, paste it into the chat. Often faster and fewer tokens.
3. **Stop repeating yourself**: have Claude Code add a line to CLAUDE.md making "check the n8n execution log via MCP yourself before asking me" the default.
4. **General habit worth teaching**: whenever Claude asks you to do something manually, ask *"can you do this yourself?"* then *"is there a CLI or MCP you could be given so you can handle this on your own?"*

<!-- pattern: /dragging-an-n8n-node-and-then-jumps-back-to-its-original-position, /is-it-me-or-claude, /im-claude-codes-assistant -->

---

## If none of the branches matched

Switch to drafting a Support Needed‼️ post (see SKILL.md Escape Hatch B). For n8n workflow issues, pre-fill:

- **n8n flavor and version** — Cloud (which plan) / self-hosted (which host, which version)
- **What the workflow is supposed to do** in one sentence
- **Where in the execution log it stops or goes wrong** — the node name, and whether it shows zero items, an error, or *waiting*
- **The exact error text**, verbatim
- **A screenshot of the canvas** plus the failing node's parameters and its execution JSON (redact keys and client data)
- **Manual run or production run?** — and whether the other one behaves differently
- **What's already been tried** — including whatever this diagnostic walked them through

No need to tag anyone — the support team watches Support Needed‼️. Remind them to retitle the post **[SOLVED]** once it's resolved.
