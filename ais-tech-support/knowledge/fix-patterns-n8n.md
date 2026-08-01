# Fix Patterns — n8n workflow building & platform limits

Part of the `/ais-tech-support` fix-pattern corpus. Index and provenance conventions: `knowledge/fix-patterns.md`.

Each entry is a verified fix pulled from the AIS+ support corpus — real solved threads answered by the AIS+ team and community members. Prioritize Claude Code + n8n/MCP entries; those are the v1 targets.

Provenance labels used below: **lesson-backed** (matches classroom content), **team-verified** (confirmed by an AIS+ team member in a solved thread), **community-verified** (confirmed by community members in solved threads; no specific lesson covers it). Never name non-team community members in answers.

---

See also `diagnostics/n8n-workflow-quality.md` (routed first for workflows that run but misbehave) and `diagnostics/lesson-1-4-n8n-mcp.md` (connecting Claude Code to n8n).

---

## N8N WORKFLOW BUILDING — Agents & AI Nodes

## n8n AI Agent never calls its tool — 'Set Automatically' description + pre-resolved expressions
**Symptom**: Agent replies conversationally but never calls the attached tool; output panel hints 'None of your tools were used in this run.'

**Root cause**: The tool's description is on 'Set Automatically' (generic text the model deems irrelevant), and n8n expressions pre-resolved inside the system prompt's Tools section make the model think it already has the data.

**Fix steps**:
1. Set a custom Tool Description stating what the tool does and when it MUST be called.
2. Keep the prompt's Tools section as plain descriptions (no {{ }} expressions); move dynamic values into Instructions (e.g. 'ALWAYS call Search_Client first; the phone number is {{ $json.phone_number }} — pass it to the tool').
3. Re-run and confirm the 'none of your tools were used' banner is gone.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/ai-agent-not-using-the-tools

<!-- pattern: /ai-agent-not-using-the-tools -->

---

## RAG agent skips the vector store — force tool use and check the retrieval embedding model
**Symptom**: Answers look plausible but never come from your documents; the execution log shows the vector-store tool was never called.

**Root cause**: The agent's system prompt never tells the model to consult the knowledge base, so for any topic the LLM already knows it just answers from training and skips the tool entirely. A retrieval-side vector store node with no embedding model connected will also fail even once the tool is called, since it can't turn the question into a vector to search with.

**Fix steps**:
1. Read the execution chain first. If the vector store node never appears between the agent and the model, the tool is being skipped — the answer you're reading came from the LLM's own knowledge, not your documents.
2. Add an explicit instruction to the agent's system prompt, e.g. 'Always check the knowledge base using the available tools before answering'.
3. Check the retrieval-side vector store node has an embedding model connected below it, matching the one used at ingestion. The ingestion workflow having its own embedding setup does not cover the retrieval side.
4. Re-test with a question that can only be answered from your documents, and confirm the vector store node now appears in the execution log.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/supabase-vector-store-doesnt-seem-to-work

<!-- pattern: /supabase-vector-store-doesnt-seem-to-work -->

---

## Agent hallucinates after the user replies 'yes' — bare confirmation passed as the RAG query
**Symptom**: Multi-agent chatbot works until a short 'yes' reply, then hallucinates or skips the knowledge base.

**Root cause**: The orchestrator passes the literal 'yes' as the retrieval query, which matches nothing.

**Fix steps**:
1. Option A: restructure the flow to deliver data with the answer instead of asking a follow-up.
2. Option B: add a query-synthesis rule — 'when using the knowledge tool, ALWAYS formulate a complete detailed query from conversation context; never pass a bare confirmation.'

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/prompt-engineering

<!-- pattern: /prompt-engineering -->

---

## RAG chatbot slow and token-hungry — retrieval limit, chunking, Q&A chain instead of a sub-agent
**Symptom**: ~30s and ~8K tokens per answer; more knowledge makes it slower, trimming prompts makes it wrong.

**Root cause**: A full AI Agent in the retrieval sub-workflow (redundant), too many chunks returned, and chunking that splits questions from answers.

**Fix steps**:
1. Lower the vector store 'limit' (try 2) and compare time/tokens.
2. Re-chunk so Q&A pairs stay together (working values: chunk 1500, overlap 200); delete and re-upload broken chunks.
3. Replace the retrieval sub-workflow's AI Agent with a Question & Answer Chain (small model like GPT-4o mini, minimal prompt, Vector Store Retriever, store in 'Retrieve Documents (As Vector Store for Chain/Tool)' mode).
4. Iterate remaining logic quirks via prompt-optimization cycles.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/rag-more-info-better-answers-less-money-and-takes-double-time

<!-- pattern: /rag-more-info-better-answers-less-money-and-takes-double-time -->

---

## n8n agent observability on a budget — log every branch to Sheets; token usage needs a Code node
**Symptom**: Chatbot going into testing with no way to catch wrong answers or token misuse short of reading executions.

**Root cause**: n8n has no conversation/token dashboard, and agent token usage isn't drag-and-drop mappable.

**Fix steps**:
1. End every branch with a Sheets append (datetime {{$now}}, user ID, in/out messages, token cost) placed AFTER the response webhook so logging can't block replies.
2. Extract token usage with a Code node (per Nate Herk's auto-track video post).
3. Set hard spend limits in the LLM provider's billing dashboard; graduate to Helicone/LangSmith when justified.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/prompt-engineering

<!-- pattern: /prompt-engineering -->

---

## AI-pipeline cost optimization — dedup before the LLM, batch lookups, no Agent for classification
**Symptom**: A high-volume workflow burns API credits and hits timeouts: every item passes through an AI classifier and per-item database lookups before anything is filtered out.

**Root cause**: Ordering and node-choice inefficiencies — classification running before deduplication, per-item lookups inside the loop, a failing Structured Output Parser causing paid retries, and an AI Agent node used for a tool-less yes/no classification.

**Fix steps**:
1. Move the duplicate check BEFORE any AI classification — items already processed on a previous run then get skipped before you pay for a model call, and non-matching items simply fall through to the classifier as normal.
2. Instead of hitting the database once per item inside the loop, fetch all known keys (e.g. article URLs) once before the loop and use a Compare Datasets node so only new items continue.
3. Fix any erroring Structured Output Parser — every failed parse is a paid retry. Adding an example of the expected JSON to the prompt, or simplifying an over-strict field type, usually clears it.
4. For tool-less classification use a Chat Model plus a parser, or the Text Classifier node; reserve AI Agent nodes for steps that actually have tools attached. If timeouts are the main pain, move the stages after dedup into a child workflow called via Execute Workflow so one slow item cannot take down the batch.
5. On batch size, try 2-3 items at a time rather than jumping between 1 (stable but slow) and the whole set.
6. Note: these are optimization recommendations from the support team; the student had not reported back with measured results at the time of the thread.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/optimise-n8n-system

<!-- pattern: /optimise-n8n-system -->

---

## Text Classifier 'Expected object, received array' — toggle off the Responses API
**Symptom**: A Text Classifier fed by a chat model throws OUTPUT_PARSING_FAILURE — 'Expected object, received array' — even though the classification values in the error text look correct.

**Root cause**: The chat model returns its response wrapped in an array, while the Text Classifier expects a JSON object. Community reports link this to the 'Responses API' option on the OpenAI Chat Model node.

**Fix steps**:
1. First try: open the OpenAI Chat Model node feeding the classifier and toggle OFF the 'Responses API' option. This is the workaround several community threads report as fixing it (reported, not confirmed in this thread).
2. Confirmed workaround from this thread: feed the classifier a field that arrives as an object rather than the array-wrapped one — in an email workflow the poster switched from the text field to the HTML field and the classifier proceeded normally.
3. Alternative node: use an Information Extractor node instead of the Text Classifier if the parsing failure persists.

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/quick-question-ef67236e

<!-- pattern: /quick-question-ef67236e -->

---

## LLM transcript cleaning — the output-token cap is the bottleneck; don't chunk rewrites
**Symptom**: Transcript-cleaning workflows lose content or students assume they must chunk with overlap.

**Root cause**: Chunk overlap gets rewritten differently per chunk (seams/duplicates); content loss is the model hitting max OUTPUT tokens — a per-platform cap separate from input context.

**Fix steps**:
1. Fit the whole transcript in one call where possible (1h ≈ 10-15K tokens vs 128K input contexts); check the platform's max output tokens — a mid-sentence stop means you hit it.
2. Use 70B-class models for long rule-following rewrites (8B models silently summarize/skip).
3. Fallbacks: two passes (strip filler → structure/metadata), or chunk at natural breaks as large as the output cap allows with a context snapshot per chunk.
4. Architecture: source-specific parents pass transcript + context into one universal cleaning sub-workflow.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/transcript-cleaner

<!-- pattern: /transcript-cleaner -->

---

## Chatbot file uploads — webhook binary data, vision routing, and text-only agent memory
**Symptom**: Chatbot must accept photos/PDFs and forward them to support, but the agent later 'forgets' the uploaded file's Drive link.

**Root cause**: Missing the webhook binary-data option and content-type routing; AI Agent memory (Simple Memory, default window of 5 interactions — a user message + AI reply counts as one, so ~10 messages) only remembers conversation TEXT — binary and side-channel JSON never enter history.

**Fix steps**:
1. Webhook node > Add option > 'Field name for binary data'; IF node checks whether binary exists; PDFs → Extract From File; images → vision-capable LLM describes them.
2. Email attachments: Gmail node > Add option > Attachments > the binary property name.
3. Persist file links by appending them to the user message as text: '[System Note: user uploaded a file: {{ $json.webContentLink }}]' with Drive sharing set to anyone-with-link; system prompt says System Notes are internal-only.
4. Add an explicit rule in BOTH the prompt and the email tool description: search conversation history for Drive links in System Notes and copy them into support emails.
5. Raise the Simple Memory context window beyond 5 if messages can separate upload from use.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/sending-photos-in-chatbots

<!-- pattern: /sending-photos-in-chatbots -->

---
## N8N WORKFLOW BUILDING — Data Flow & Expressions

## Zero items = dead branch — Always Output Data, IF routing, multi-input stalls
**Symptom**: A workflow silently stops after a lookup or delete that matched nothing; or a form submitted without an optional file upload never reaches the downstream agent even though the text fields were captured fine.

**Root cause**: A node that emits zero items halts its branch — there is nothing for the next node to run on. Separately, a node with multiple incoming connections waits for ALL of them to deliver before it runs, so an optional branch that produced nothing leaves it waiting forever.

**Fix steps**:
1. On lookup and delete nodes, open Settings and enable 'Always Output Data' so an empty result emits one empty item instead of zero. Do not set this on an IF node — it can cause a loop.
2. Route explicitly with an IF right after the lookup: key field populated → existing-record path; empty → create-new path.
3. For an optional branch feeding a multi-input node, add an IF right after the trigger that checks whether the data exists (e.g. {{ $binary.Photos }} — substitute your own field name), send the populated branch down the normal path, and route the empty branch through a No Operation node that also connects to the downstream node, so it always receives input on both connections.
4. When a node pushes through a blank item, make sure downstream nodes reference their values from the original trigger rather than from that node's now-empty output.
5. Confirm the diagnosis from the execution log — the stalled node shows as waiting, which tells you it's a multi-input stall rather than a config error in the node that appears to stop.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/submission-form-is-not-passing-through-information-if-binary-file-is-not-attached
- https://www.skool.com/ai-automation-society-plus/help-with-dynamic-rag

<!-- pattern: /submission-form-is-not-passing-through-information-if-binary-file-is-not-attached -->

---

## $json reads the previous node; single-item nodes collapse the loop — pin sources and Merge
**Symptom**: Move File moves the wrong item, or only one of several files is processed downstream.

**Root cause**: {{ $json.id }} reads the immediately-previous node, not the intended one; an intermediate single-item node (create-folder, append-row) collapses item count to 1 so downstream runs once.

**Fix steps**:
1. Pin the source: {{ $('UPLOAD_NODE_NAME').item.json.id }} — replace UPLOAD_NODE_NAME with the exact canvas name of the node whose data you need.
2. If only one of many items proceeds, check item counts between nodes; insert a Merge node: multi-item source into Input 1, the single-item node into Input 2.
3. Reference the single-item value with .first() (e.g. {{ $('CREATE_FOLDER_NODE').first().json.id }}).

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/drive-node-move-file-issue

<!-- pattern: /drive-node-move-file-issue -->

---

## n8n node fails with valid-looking config — stray whitespace in URL/expression fields
**Symptom**: API-backed node fails though configuration looks correct.

**Root cause**: A stray space between a slash and an inserted variable (often carried in when dragging variables) makes the resource path invalid and is nearly invisible.

**Fix steps**:
1. Inspect the failing field character by character around slashes and {{ }} insertions; delete whitespace so the variable sits flush.
2. Make this a standard first check when a node fails while 'looking right'.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/google-drive-issue-move-files

<!-- pattern: /google-drive-issue-move-files -->

---

## 'JSON parameter needs to be valid JSON' — remove duplicated quotes, wrap LLM output in JSON.stringify
**Symptom**: An HTTP Request node whose JSON body embeds AI-generated text fails validation with 'JSON parameter needs to be valid JSON'. Commonly hit during the UGC content-generation build, but the cause is general n8n HTTP Request behaviour.

**Root cause**: Raw LLM text carries unescaped quotes and newlines that break the surrounding JSON; and students frequently wrap the expression in their own quotes on top of quotes already present, producing a doubled set.

**Fix steps**:
1. Check the expression for duplicated quotes first — if there are two sets of quotes around the prompt variable, delete one. In the source thread this alone cleared the error.
2. Then wrap the value so the text can't break the JSON: use {{ JSON.stringify($json.output) }} in place of the bare expression, adjusting $json.output to whatever field actually holds the generated text.
3. Remove any quotes you added around it — JSON.stringify adds its own, and leaving yours in recreates the doubled-quote problem.

**Lesson reference**: Arises in the UGC content generation build; the fix is general n8n HTTP Request knowledge

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/error-message

<!-- pattern: /error-message -->

---

## Filter/IF date comparisons fail — .format() strings, type conversion, blank cells
**Symptom**: Date conditions in a Filter or IF node error out, including "the string '' can't be converted to a dateTime". Commonly hit when building the second agent in the Fast Track Challenge, but the cause is general.

**Root cause**: Three stacked issues: .format(...) converts a DateTime into a plain string while the comparison operator expects DateTime objects on both sides; the node's 'Convert types where required' toggle is off; and rows with an empty date cell have nothing that can be converted to a date at all.

**Fix steps**:
1. Remove the .format(...) call so both sides stay DateTime objects — e.g. just {{ $now.minus({days: 2}) }} on the right-hand side.
2. Turn on 'Convert types where required' at the bottom of the Filter/IF node.
3. If you're only working through test data, the fastest fix for blanks is to fill a date into the empty cells in your sheet and click 'Test step' on the Google Sheets node to pull the fresh data before re-running the filter.
4. Production-ready handling of blanks: {{ $json['YOUR_DATE_COLUMN'] || $now.minus({days: 365}) }} — replace YOUR_DATE_COLUMN with the exact column header from your sheet. Rows with no date then read as a year ago and still route through, which is usually the behaviour you actually want for new leads.

**Lesson reference**: Fast Track Challenge — Agent 2 build

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/fast-track-challenge-agent-2

<!-- pattern: /fast-track-challenge-agent-2 -->

---

## Gmail trigger returns no data — self-starvation, unpublished workflow, and form-mailer sender addresses
**Symptom**: A Gmail-to-Sheets workflow reports 'success' with nothing fetched and no rows written; the Gmail trigger node shows no output at all.

**Root cause**: Several issues stack. An unread-only trigger combined with a mark-as-read step at the end of the flow consumes its own trigger condition, and team members opening the mail manually finishes the job. The workflow may also simply never have been published. And website form notifications arrive From the site's own mailer address, not from the lead, so any filter set to the leads' addresses matches nothing.

**Fix steps**:
1. Publish/activate the workflow — in the source thread this was the one step the poster confirmed he had skipped. Note the trigger only fires for mail arriving after activation; older mail sitting in the inbox won't trigger it.
2. Stop the self-starvation: set Read Status to 'unread and read', or stop marking test mail as read at the end of the flow.
3. If the mail comes from a website form, filter on the notification mailer's single address rather than the leads' addresses, then pull the lead's real email from the Reply-To header and parse name/phone by the form's fixed field labels rather than generic regex.
4. Reported quirk worth checking if a sender filter matches nothing: the trigger's Sender field may only accept a single address, with comma-separated lists silently matching zero. Test with one address before assuming the filter is broken.
5. General rule for IF nodes: multiple 'From equals X' conditions combined with AND can never all be true for one email — use ANY, or move the filter to the trigger.

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/google-sheet-data-isnt-parsing

<!-- pattern: /google-sheet-data-isnt-parsing -->

---

## Google Sheets values arrive with trailing \n and lookups return empty — trim at the source, check headers, replace an old node
**Symptom**: Values written by a Google Sheets node come out with a \n appended (email, timestamp, score), so the downstream lookup filters on a value that matches nothing and returns empty, killing the branch.

**Root cause**: Reported causes stack. Inbound webhook text can carry a trailing newline that propagates through every node; hidden line breaks in the sheet's row-1 headers can attach to the column name and therefore to every mapped value; and Google Sheets node v4.5 has a reported bug returning empty fields on row lookups.

**Fix steps**:
1. Trim at the source first — this is what actually resolved it in the source thread. Apply .trim() to the inbound text field in the node that prepares the data (text fields only; it errors on numeric fields like scores).
2. Also check row 1 for hidden line breaks: put =LEN(A1) next to a header cell and compare to =LEN(TRIM(CLEAN(A1))). If they differ, clean the row with =ARRAYFORMULA(TRIM(CLEAN(A1:Z1))) in a spare cell, paste it back over row 1 as Values Only, then reopen the Sheets node and re-select the mapping columns so it picks up the clean header names.
3. If lookups still return empty after the values are clean, the student who solved this traced it to the Google Sheets node running at version 4.5. Delete that lookup node and add a brand-new Google Sheets node in its place so it installs at the current version.
4. Finally, confirm the value you're searching for actually exists in the target sheet — an empty lookup result is correct for a genuinely new record, and that path needs to be handled rather than fixed.

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/node-adding-n-to-output-values-breaking-downstream-lookup-node

<!-- pattern: /node-adding-n-to-output-values-breaking-downstream-lookup-node -->

---

## Loop Over Items writes phantom rows / never finishes — 'loop' vs 'done' wired backwards
**Symptom**: A Loop Over Items build writes wrong or invented values into the CRM sheet, IDs do not line up with the source rows, and the execution never reports success (it has to be cancelled manually).

**Root cause**: The per-item work was connected to the loop node's 'done' branch — which fires once after all items have been processed — instead of the 'loop' branch, which fires once per item.

**Fix steps**:
1. On the Loop Over Items node, hover each of the two outgoing connections ('done' and 'loop') and delete both, so neither branch is wired to anything.
2. Connect only the 'loop' branch to the per-item node (the CRM/Sheets update). Leave 'done' for steps that should run once after the whole loop finishes, or unconnected.
3. Re-run the workflow; it should now complete on its own instead of running indefinitely.

**Lesson reference**: Live sales bot tutorial (live class), CRM bot section

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/issue-faced-whilst-building-sales-bot-in-live-sales-bot-tutorial

<!-- pattern: /issue-faced-whilst-building-sales-bot-in-live-sales-bot-tutorial -->

---

## n8n data pinning requires a single main output — pin the trigger instead
**Symptom**: Pin greyed out on Text Classifier (also IF/Switch) though videos appear to pin nodes.

**Root cause**: Pinning only works on single-main-output nodes; multi-branch nodes can't be pinned — what looks pinned in videos is execution output.

**Fix steps**:
1. Pin the trigger node's output (execute once with a real item) and re-run the workflow to iterate against it.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/cant-pin-text-classifier-node

<!-- pattern: /cant-pin-text-classifier-node -->

---

## IDs lost between nested AI agents ($fromAI text handoffs)
**Symptom**: Sub-workflow tools fail with 'could not access the source file' or a bad-request error because a Google Drive file ID didn't survive being relayed through two nested agents. Reported on the Ultimate Media Agent template, where a main agent delegates to a creative agent which then calls an image-edit sub-workflow.

**Root cause**: $fromAI('...') parameters depend on one LLM writing the value into its text message and the next LLM extracting it back out. There is no structural data pipe between the hops, so an exact identifier can be dropped or mangled purely on LLM behaviour.

**Fix steps**:
1. Trace the value's full path first and find the exact hop where it stops being structured data and becomes text — that hop is the break.
2. Mitigation: add explicit prompt rules to BOTH the orchestrator and the sub-agent instructing them to pass file IDs through verbatim when delegating.
3. Structural fix that doesn't depend on LLM cooperation: replace the $fromAI('...') parameter in the toolWorkflow node with a direct expression that reads the ID from the workflow data instead.

**Lesson reference**: Ultimate Media Agent template build (classroom resource)

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/error-with-ultimate-agent-workflow

<!-- pattern: /error-with-ultimate-agent-workflow -->

---

## Parallel media generation with retries — Merge waits forever on unfired branches
**Symptom**: Generating several media clips in parallel: a Merge node hangs forever when success and retry branches are wired into it conditionally, and clips that go down the retry path never rejoin the successful ones for the downstream step.

**Root cause**: A Merge node waits for input on every branch connected to it, so a conditional retry path that never fires leaves an input unsatisfied and the execution stalls. Processing the clips sequentially in a Loop Over Items fixes correctness but multiplies total runtime.

**Fix steps**:
1. Suggested in the thread (not confirmed by the poster): give each clip its own dedicated parallel branch, all feeding a single final Merge node configured with one input per clip. The Merge then waits for every branch because every branch always fires, and all clips move to the downstream step together.
2. The poster's own untested idea: fire all clips in parallel, collect only the failures, and run a Loop Over Items over just that failed subset before merging — parallel speed for the successes, sequential retries only where needed.
3. Note: this thread ended without the poster reporting which approach worked, so treat both as starting points to test rather than proven fixes.

**Confidence**: low — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/handling-sora-2-api-failures-in-n8n-need-help-with-logic

<!-- pattern: /handling-sora-2-api-failures-in-n8n-need-help-with-logic -->

---
## N8N WORKFLOW BUILDING — Credentials, Triggers & Reliability

## n8n HTTP Request header-auth failures — blank Name, wrong scheme, provider prefixes
**Symptom**: 'Header name must be a non-empty string' from an HTTP Request node; fal.ai rejecting a valid key; template calls to OpenAI image endpoints failing on auth.

**Root cause**: The Generic Header Auth CREDENTIAL is misconfigured rather than the node — most often the credential's Name field is left blank (so n8n has no header name to inject), the header/auth option is switched off on the node, or the wrong auth scheme is used, since providers differ.

**Fix steps**:
1. Open the Header Auth CREDENTIAL itself, not the node. The Name field must be exactly: Authorization. The Value field is the provider's scheme, one space, then your key.
2. Schemes differ by provider: OpenAI and Kie.ai use 'Bearer <key>'; fal.ai uses 'Key <key>' and will reject a Bearer prefix. Get the key from that provider's own dashboard.
3. Check that the header/auth option is actually enabled on the node — a community member traced one fal.ai failure to Header Authorization being selected with the header toggle switched off.
4. For well-known APIs, prefer Authentication > Predefined Credential Type (e.g. 'OpenAI API') over Generic Header Auth — n8n then builds the auth header for you and this class of error disappears.
5. Kie.ai specifically is a two-call flow: submit with the create/request endpoint, then poll the separate query endpoint using the taskId from the previous node. A student who kept getting 401 then 500 then 'model cannot be null' traced it to using the wrong curl and to expression syntax, not to the credential.

**Confidence**: high — team-verified

**Example threads**:
- https://www.skool.com/ai-automation-society-plus/the-ai-marketing-team-42625-template-but-im-stuck-on-an-error-i-cant-resolve
- https://www.skool.com/ai-automation-society-plus/kie-connection-instruction
- https://www.skool.com/ai-automation-society-plus/looking-for-video-tutorial-and-template-of-photoshop-ai-agent

<!-- pattern: /the-ai-marketing-team-42625-template-but-im-stuck-on-an-error-i-cant-resolve -->

---

## Claude-created placeholder credentials lock n8n nodes
**Symptom**: Nodes in a workflow that Claude Code built are locked and uneditable in n8n; the UI offers only to request credential sharing or to duplicate the node.

**Root cause**: Claude Code either created the credentials itself while building or used a placeholder. Because the account did not create them, n8n does not recognise them as yours and locks the node.

**Fix steps**:
1. Immediate unblock: duplicate the locked node and attach your own credential to the copy.
2. Permanent fix: create each credential yourself in the n8n UI first, then tell Claude Code to reference those existing credentials by name when it builds, instead of letting it create or invent them.
3. Untested community suggestion: keep a dummy template workflow in n8n with your common credentials (Gmail, Sheets, etc.) already attached, and point Claude Code at that template so new builds inherit the auth.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/claude-code-duplicating-nodes

<!-- pattern: /claude-code-duplicating-nodes -->

---

## Imported course template checklist — activate, re-credential every generation node, unpin
**Symptom**: Template only runs when manually triggered; generation nodes error despite credits; bot repeats stale output.

**Root cause**: Imported workflows arrive unactivated, with the instructor's placeholder credentials, and sometimes with pinned test data forcing stale prompts.

**Fix steps**:
1. Activate/publish so triggers fire on their own.
2. Open the Executions tab to find failing nodes; add YOUR OWN credential/API key on each generation service node (from that provider's dashboard).
3. Unpin any leftover pinned data/prompts.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/n8n-marketing-automation

<!-- pattern: /n8n-marketing-automation -->

---

## Kie.ai nano-banana model/parameter mismatch — image input silently ignored
**Symptom**: A UGC image-generation step invents a fake product instead of using the real one, even though the product photo URL is present in the HTTP Request node's JSON body.

**Root cause**: The request used a different Kie.ai model than the course build, and that model expects a differently-named image parameter — so the image field was silently ignored and the model generated from the text prompt alone.

**Fix steps**:
1. Match the course build: set the model to google/nano-banana-edit and the image parameter to image_urls, all lowercase (a capitalised Image_urls will not be recognised). Map its value by expression from the sheet column holding the product image URL rather than typing a literal.
2. If you deliberately want nano-banana-pro instead, rename the parameter to image_input — check the current parameter names on the nano-banana-pro API docs page at kie.ai before relying on this, as they differ per model.
3. Rule of thumb: the -edit model places an existing image into a scene, the -pro model generates from scratch with optional reference images. Whenever a generation API appears to ignore an input, diff your request body against the course build's model name AND its parameter names, not just the values.

**Lesson reference**: n8n Masterclass Module 3 — UGC Content System project

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/n8n-masterclass-module-3-ucg-content-system-project-image-output-problem

<!-- pattern: /n8n-masterclass-module-3-ucg-content-system-project-image-output-problem -->

---

## n8n error workflows only fire on production executions
**Symptom**: A correctly-assigned error workflow never triggers during intentionally-failing manual tests.

**Root cause**: By design, manual canvas runs never fire error workflows.

**Fix steps**:
1. Swap the Manual Trigger for a Webhook node, activate the workflow, and hit the Production URL from a browser to force a real production execution.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/n8n-self-healing-need-help

<!-- pattern: /n8n-self-healing-need-help -->

---

## Self-healing bridge server 'Authorization failed' — x-api-key = BRIDGE_API_KEY (not ngrok tokens)
**Symptom**: The HTTP Request node calling the self-healing bridge server returns 'Authorization failed - please check your credentials', whether the header carries the ngrok Authtoken or the ngrok API key.

**Root cause**: Neither ngrok credential is what the bridge validates. The ngrok Authtoken authenticates the tunnel agent in your terminal and the ngrok API key is for ngrok's own management API; the bridge server checks its own BRIDGE_API_KEY, sent in an x-api-key header.

**Fix steps**:
1. In the HTTP Request node set Header Name to x-api-key. For the value, open your bridge server.js and find the BRIDGE_API_KEY constant — whatever string is assigned there is what goes in the header. If it is still the setup guide's placeholder value, change it in server.js to a long random string of your own and use that, since the bridge is reachable through a public tunnel.
2. Self-hosted n8n: make sure N8N_HOST in claude_desktop_config.json points at YOUR n8n base URL, not the cloud URL printed in the guide.
3. A ngrok domain ending in .dev rather than .app is only a plan/region difference — it is not the cause of the auth error.

**Lesson reference**: 'n8n 2.0: Self-Healing Workflows with Claude Code' video + PDF setup guide, Part 5 Step 3

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/n8n-self-healing-need-help

<!-- pattern: /n8n-self-healing-need-help -->

---

## Retry On Fail on the model sub-node does nothing; error branch never fires
**Symptom**: LLM step times out, retries never happen, wired error branch never executes.

**Root cause**: Retry was set on the circular Chat Model sub-node (config only); the rectangular root node performs the call; On Error left at 'Stop Workflow' kills the run before the branch fires; n8n Cloud kills executions ~5 minutes.

**Fix steps**:
1. Move Retry On Fail to the executing root node(s); set On Error to 'Continue (Using Error Output)'.
2. For long generations mind the ~5-minute Cloud timeout; self-hosted can raise EXECUTIONS_TIMEOUT (seconds, in the instance env).
3. Test AI nodes with varied input sizes to find the timeout threshold before scheduling unattended runs.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/open-ai-node-request-time-out

<!-- pattern: /open-ai-node-request-time-out -->

---

## Scheduled workflow fires exactly twice — duplicate trigger, stale registration, or a second n8n container
**Symptom**: A once-daily schedule fires twice at the same moment every time.

**Root cause**: A second (even disconnected) trigger on the active canvas, a duplicate active copy, a stale trigger registration, or — confirmed here — two n8n containers sharing one database, each firing the schedule.

**Fix steps**:
1. Scan for extra Schedule Trigger nodes (disconnected ones still fire) and duplicate active workflow copies.
2. Deactivate, wait ~10s, reactivate to clear stale registrations; if needed, copy into a brand-new workflow and update n8n.
3. Self-hosted and still doubling: run docker ps | grep n8n — more than one container on the same DB means every schedule fires per container; remove the replica.
4. The '0/2' badge is the Production Checklist, unrelated.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/help-with-dual-executions-in-n8n

<!-- pattern: /help-with-dual-executions-in-n8n -->

---

## Production webhook 200-with-empty-body — stale Active registration and test-vs-prod payload paths
**Symptom**: Production webhook returns HTTP 200 with an empty body; the execution stops partway (e.g. a lookup node runs but the next node never fires); the same request works when the node is executed manually.

**Root cause**: After a workflow is edited, an active workflow can keep serving a stale production webhook registration, so requests hit the old version of the flow. (Two secondary possibilities were raised in the thread but never confirmed: the payload path differing between test and production runs, and a not-found lookup silently ending the branch.)

**Fix steps**:
1. Confirmed fix: toggle the workflow Active/Published off and back on to re-register the production webhook, then re-send the request.
2. If that does not resolve it, open a real production execution in the Executions tab, click the Webhook node, and read the actual output JSON before hard-coding an expression path — the field may sit at a different level than you assumed (unconfirmed suggestion from the thread).
3. If a lookup returning zero rows is ending the branch, turning on 'Always Output Data' in that node's settings lets an empty item through so an IF node can still route to an error response (unconfirmed suggestion from the thread).

**Confidence**: medium — community-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/interactive-live-avatar-support-needed

<!-- pattern: /interactive-live-avatar-support-needed -->

---

## Simple Memory errors with two triggers in one workflow
**Symptom**: With two triggers active in one workflow, the Simple Memory node errors out and the session ID cannot be resolved.

**Root cause**: Reported by the student as the memory node not resolving which trigger owns the session. The support team could not reproduce it, and the case was ultimately fixed by an n8n version update, pointing to a platform bug rather than a configuration mistake.

**Fix steps**:
1. Confirmed fix in this case: update n8n — the student's issue disappeared on 2.11.3.
2. Also helped: split the automations so each trigger lives in its own workflow. This is the preferred architecture anyway.
3. Worth trying, though it did not help this student: open the Simple Memory sub-node, switch the key from 'Connected chat trigger node' to 'Define below', execute the chat trigger, and drag the session ID variable from its output into the key field.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/two-triggers-active

<!-- pattern: /two-triggers-active -->

---

## Protecting an n8n webhook embedded in a public website form
**Symptom**: A public website form calls an n8n webhook, so the webhook URL (and any token) sits in client-side HTML where anyone can inspect it and replay requests from Postman.

**Root cause**: Client-side code cannot hold a secret. Anything shipped in page source is readable, so protection has to come from server-side restrictions layered on top.

**Fix steps**:
1. Add header authentication on the Webhook node as one layer. Treat it as a speed bump, not a wall — a determined visitor can still find the token by inspecting the page.
2. Combine it with an origin restriction: open the Webhook node, click 'Add option' at the bottom, choose 'Allowed origin (CORS)', and enter the domain of the site the form lives on (e.g. the client's own www domain — the exact host you are handing the HTML to). This makes the webhook reject requests from Postman and other origins.
3. Strongest option, and what the poster settled on: put a gatekeeper endpoint on your own server between the client's form and n8n, so the real n8n URL never appears in the page source. Add reCAPTCHA and spam filtering there too.
4. Never send values you care about (such as pricing) in the client-side payload — compute them server-side after the request arrives.

**Lesson reference**: 'Protect Your Webhooks' lesson at the end of the n8n Masterclass — verify exact title

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/webhook-protection

<!-- pattern: /webhook-protection -->

---

## Supabase vector-store refresh — file ID lives in the metadata JSONB; delete via PostgREST
**Symptom**: Delete-then-re-embed RAG refresh stops: Delete Rows outputs zero items and column filters on file_id match nothing.

**Root cause**: The Supabase Vector Store writes metadata (incl. file ID) into a metadata JSONB column, which the standard Delete node can't filter on.

**Fix steps**:
1. Keep delete-then-re-embed (upsert leaves orphaned chunks when chunk counts change).
2. Enable Always Output Data on the delete step; reference the file ID from the original Drive trigger, not the delete output.
3. Replace the Delete node with an HTTP DELETE to https://YOUR_PROJECT.supabase.co/rest/v1/documents?metadata->>file_id=eq.YOUR_FILE_ID (YOUR_PROJECT = your project ref from the Supabase dashboard; YOUR_FILE_ID = the Drive trigger's file ID expression; include your Supabase API key headers).
4. Ensure ingestion stores the Drive file ID into metadata so future deletes match.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/help-with-dynamic-rag

<!-- pattern: /help-with-dynamic-rag -->

---

## MCP-created n8n workflow nodes snap back / edits don't stick (settings.nodes bug)
**Symptom**: Dragging a node on a workflow Claude Code built through the n8n MCP snaps it back to its original position; manual edits revert; Claude claims it fixed things that never change on the canvas.

**Root cause**: n8n issue #27638 (now fixed): when a workflow was created through the MCP's create_workflow_from_code tool, the nodes could be saved under a settings property instead of the root-level nodes array, so the canvas editor didn't track them as editable. Confirmed on 2.13.3 stable / 2.14.2 beta, closed by PR #32341 — check the installed version before applying the manual workaround below.

**Fix steps**:
1. Check first whether the upstream issue has since been fixed before doing anything destructive — this was unpatched at the time of the thread.
2. Quickest route: rebuild the workflow manually in the n8n editor so the nodes are stored correctly from the start.
3. JSON route if the workflow is too complex to rebuild: export the workflow JSON, move the node definitions from settings.nodes into the root-level nodes array, and reimport.
4. Prevention until it's patched: let Claude Code plan and generate the workflow logic, but build and deploy it manually rather than having it deploy through the MCP.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/dragging-an-n8n-node-and-then-jumps-back-to-its-original-position

<!-- pattern: /dragging-an-n8n-node-and-then-jumps-back-to-its-original-position -->

---
## N8N — Platform Limits & Scaling

## n8n Cloud memory/execution ceilings — mitigations and the self-host decision
**Symptom**: Growing usage throws memory errors and approaches execution caps on an n8n Cloud plan; workflow refactoring only buys limited headroom.

**Root cause**: Cloud plans cap RAM and executions per tier. Enterprise removes the immediate pain with zero migration but is a custom quote typically starting around $2,000+/mo, while self-hosting removes the caps for a fraction of that at the cost of owning maintenance, updates, backups and security.

**Fix steps**:
1. Immediate headroom on your current plan: process data in smaller chunks with Split In Batches instead of loading everything at once; break complex workflows into sub-workflows, since each releases its memory when it finishes rather than holding it for one long execution; and stop testing via manual executions — they use more memory because n8n copies the data for the UI. Fire production runs instead via the webhook node's Production URL (not the Test URL) or the n8n API.
2. The decision: Enterprise = zero migration at a steep premium; self-hosting ≈ $150-300/mo for a starter setup (roughly 4 vCPU / 8GB plus managed PostgreSQL) up to $400-800/mo production-grade with monitoring and redundancy — early-2026 ballparks — with unlimited executions plus ongoing DevOps time.
3. Don't switch platforms. A year of business-critical workflows is a massive rebuild, and per-operation pricing elsewhere adds up fast at high execution volume.
4. Migrate in stages: stand up the self-hosted instance, move a few non-critical workflows first to validate performance, then move the business-critical ones. Don't flip everything at once.
5. The community has a longer write-up on the self-host vs Cloud decision covering hidden costs and backup strategy — worth searching the community for it before committing.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/advice-on-n8n-scalingalternatives

<!-- pattern: /advice-on-n8n-scalingalternatives -->

---

## n8n Cloud degraded — 502/524 on publish, schedules silently not firing (execution-history bloat)
**Symptom**: Publishing a workflow on n8n Cloud returns 502 or 524; Schedule Triggers stop firing with nothing at all in the execution log (not even failures) while manual runs still work.

**Root cause**: The instance is out of memory, backed up by a very large stored execution history on heavy workflows.

**Fix steps**:
1. Confirmed fix: copy the whole workflow into a brand-new workflow, activate that one, and delete the bloated original. Do the same for any other workflow carrying an excessive execution history.
2. Tried first in this thread and it did NOT help: restarting the workspace (cloud icon bottom-left > Manage > confirm latest stable version > Restart workspace). Still worth a minute since it is quick, but do not stop there.
3. Prevention if it recurs: workflow menu (three dots, top right) > Settings > set 'Save production executions' to 'Do not save' on your heaviest workflows, or clear the Executions tab periodically. Note that n8n Cloud prunes execution logs on a per-plan schedule (Starter tightest, Enterprise unlimited) — check their plan's current limits at docs.n8n.io/deploy/use-n8n-cloud/configure-cloud/manage-your-data before concluding a single occurrence was a one-off.
4. If publishing still fails or schedule triggers still do not fire, open a support ticket with n8n from the same admin panel.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/struggling-with-n8n-cloud-triggers-not-firing-502524-errors-anyone-else

<!-- pattern: /struggling-with-n8n-cloud-triggers-not-firing-502524-errors-anyone-else -->

---

## Self-hosted n8n at scale — workflow_statistics lock contention (SKIP_STATISTICS_EVENTS) and IO tuning
**Symptom**: Queue-mode instance with many workers overloads Postgres; top lock waits on INSERT ... ON CONFLICT into workflow_statistics; N8N_DISABLED_MODULES=insights doesn't help.

**Root cause**: Statistics writes fire on every execution via core lifecycle handlers (Insights is a separate code path); the unique (workflowId,name) constraint makes concurrent executions of one workflow fight over a row lock.

**Fix steps**:
1. Set SKIP_STATISTICS_EVENTS=true on ALL instances (main, webhook, workers).
2. Tune execution data: EXECUTIONS_DATA_SAVE_ON_SUCCESS=none, keep ON_ERROR=all, ON_PROGRESS=false; confirm pruning on and tighten EXECUTIONS_DATA_MAX_AGE.
3. Raise DB_POSTGRESDB_POOL_SIZE (default 2 → ~5/worker, 10 main) with Postgres max_connections headroom; consider PgBouncer.
4. If pressure persists, consolidate always-together lightweight subworkflows back into parents.

**Confidence**: high — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/workflowstatistics-locking-db

<!-- pattern: /workflowstatistics-locking-db -->

---

## Telegram bot burning n8n Cloud executions during testing
**Symptom**: Webhook messages eat the 2,500/month Starter quota while testing.

**Root cause**: Testing against the production webhook counts executions; test executions are free and unlimited.

**Fix steps**:
1. Test via the webhook Test URL + Execute workflow instead of the production URL.
2. For heavy testing, self-host (local Community Edition or a one-click Hostinger template).

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/telegram-bot-burning-n8n-cloud-limits

<!-- pattern: /telegram-bot-burning-n8n-cloud-limits -->

---

## DeepSeek (OpenAI-compatible APIs) via the native OpenAI node — plus monitoring-feed hygiene
**Symptom**: A monitoring workflow drives DeepSeek through hand-built HTTP Request nodes with manually written JSON payloads, which is fragile and awkward to maintain.

**Root cause**: DeepSeek's API is OpenAI-compatible, so n8n's native OpenAI node can drive it directly once the base URL is pointed at DeepSeek — the manual HTTP plumbing is unnecessary.

**Fix steps**:
1. Create an OpenAI credential in n8n and override its Base URL with DeepSeek's API endpoint (the base URL published in DeepSeek's own API documentation), using an API key generated in your DeepSeek platform console. Then swap the HTTP Request nodes for the native OpenAI node — n8n handles the payload and output parsing for you.
2. Feed hygiene for monitoring workflows: deduplicate on the post URL or ID before anything reaches the summarizing step, since the same post can match several keywords and double-count.
3. Classify each item first (have the model tag it, then route with a Switch node) and summarize only what survives the filter, so summaries and charts are built from signal rather than noise.
4. If you outgrow f5bot, tools raised in the thread as webhook-capable alternatives were Syften, CatchIntent, and Alertly; polling the Reddit API directly from n8n avoids a subscription entirely. Check current pricing yourself — none of these were evaluated in the thread.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/reddit-automation-for-freelancer-community-i-build

<!-- pattern: /reddit-automation-for-freelancer-community-i-build -->

---

## Multi-site social posting — one parameterized master workflow + context table + Blotato
**Symptom**: Deciding how to automate social posting across several websites' accounts — custom Python scripts versus n8n — and how to structure it so it does not become one cloned workflow per site.

**Root cause**: Pure scripts mean building your own database, cron scheduling, and error dashboard from scratch. Cloning a workflow per site is the obvious n8n approach but breaks down as sites are added.

**Fix steps**:
1. Support team recommendation: use n8n plus Blotato rather than scripts — n8n gives you scheduling, credential management, and execution logging out of the box, and Blotato handles the social platforms. The shape is Schedule Trigger, pull the source articles, an LLM step that writes the post, then Blotato to publish or schedule it.
2. If you are comfortable in Claude Code, connect the n8n MCP and let Claude Code build the workflow for you.
3. Community architecture suggestion for the multi-site case: instead of duplicating the workflow per site, keep one master workflow that reads each site's context (URL, brand voice, feed, target account IDs, post schedule) as a row in a Supabase or Airtable table, so adding a site means adding a row.
4. Same suggestion, continued: build research, content generation, image generation, and posting as generic sub-workflows that each accept a site context object, and let the Blotato account ID in the row route the post to the right account instead of building IF chains. Check Blotato's current plan account limits early, since multiple sites consume them quickly.
5. Note: this is planning guidance from the thread; the poster had not yet built the system.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/social-media-automation-using-one-of-nates-video

<!-- pattern: /social-media-automation-using-one-of-nates-video -->

---

## Dashboards over multi-source n8n data — Postgres 'data hub', not direct BI pushes
**Symptom**: Multi-SaaS dashboard wanted; Power BI has no native n8n node and the community node is risky.

**Root cause**: Direct pushes into BI tools are brittle (auth pain, lagging community nodes); the real work is normalizing sources, which belongs in a database layer.

**Fix steps**:
1. Have n8n normalise each source into a common schema and write to PostgreSQL as the central data hub. Budget most of the effort here: reconciling how each SaaS structures its data (support tickets vs CRM engagements vs database properties) is where builds like this actually stall, not in picking the chart tool.
2. Point the BI layer at Postgres rather than pushing data into it. Metabase is the usual pick when non-technical staff need to build their own charts (open source, self-hostable), Grafana suits real-time and more technical teams, and Power BI can read from Postgres directly.
3. Fastest way to validate the pipeline before committing to a stack: n8n -> Google Sheets -> Looker Studio, using n8n's native Google Sheets nodes.
4. If Power BI must be pushed to rather than reading from Postgres, use its REST API via an HTTP Request node rather than the community node - community nodes can lag behind on updates, and the Azure AD OAuth setup is the reported time sink.

**Confidence**: medium — team-verified

**Example thread**: https://www.skool.com/ai-automation-society-plus/which-data-visualization-tools-work-best-with-n8n

<!-- pattern: /which-data-visualization-tools-work-best-with-n8n -->

---
