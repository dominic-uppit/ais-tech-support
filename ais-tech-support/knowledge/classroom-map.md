# Classroom Map — AIS+ Lesson Reference Lookup

Generated from the AIS+ classroom (10 courses, 634 modules). Use this when a student references a lesson by number, title, or partial name, to translate it to the correct course path.

Lesson URL pattern: `https://www.skool.com/ai-automation-society-plus/classroom/{course-slug}?md={lesson-id}` — course-slug is the short 8-char code, lesson-id is the full 32-char id. Both are below; never shorten the lesson id.

## How to look up a lesson — grep, don't read

**This file is a lookup table, not a document. Do not read it end to end** (634 entries, ~24k tokens). To build one lesson link:

1. **Grep for the lesson title** to get its id:
   `grep -n "WAT Framework" knowledge/classroom-map.md`
   → `168: - **1.3 The WAT Framework** — lesson-id: \`51acd25ee1d544d0af24b8eee4d7101e\``
2. **Get the course slug from the table below** — *not* from the grep hit. The `Course slug:` line can sit up to ~210 lines above an entry, so a single grep gives you the id only. Never infer or guess a slug.
3. Assemble: `https://www.skool.com/ai-automation-society-plus/classroom/{slug}?md={id}` — full 32-char id, unshortened.

If the grep misses, try a distinctive word from the title rather than the whole phrase (titles carry numbering and emoji, e.g. `**1.4 n8n MCP Server**`, `**AI Voice Agents 🗣️**`). If a title genuinely isn't here, the lesson isn't in the map — say so rather than constructing a link.

**If the grep returns more than one hit, stop and disambiguate — the ids differ.** Four titles are reused across courses, and picking the wrong row 404s the student or sends them to the wrong course:

| Reused title | Appears in |
|---|---|
| `Track Your Progress` | START HERE, Build Your Portfolio (×2), Scale, Archived |
| `Delivering n8n Projects` | Scale, Community Resources |
| `Human in the Loop Calendar Agent` | Archived, Community Resources |
| `🧩 Structured Output Parser Puzzle` | Archived (×2) |

To resolve one, grep with context so you can see which `## ` course section the hit sits under — `grep -n -B 40 "Track Your Progress" knowledge/classroom-map.md | grep -E "^[0-9]+.(## |- \*\*Track)"` — or ask the student which course they're in. Do not pick the first hit by default.

| Course (`## ` section) | Course slug |
|---|---|
| START HERE | `23a170fe` |
| The AI Partner Model | `8de65cec` |
| Build Your Portfolio | `d99017d2` |
| Claude Code | `bd6b51dc` |
| Get Your First Clients | `c89ccfd3` |
| Scale | `114ef87f` |
| Member Perks | `308fec3a` |
| Archived | `ec6512da` |
| Community Resources | `a7f6a1e7` |
| Live Call Recordings | `f315a988` |

## START HERE

Course slug: `23a170fe`

### START HERE

- **Track Your Progress** — lesson-id: `5e3fc24df27149e1b7cdc783543a4029`
- **Your Journey from 0 to First Client** — lesson-id: `fcd6622018ee4d7aa67778ec8599463f`
- **Milestones Along the Way** — lesson-id: `3e801ec70ce74ee9b8cc29fce67cbf54`
- **Success Stories from Members** — lesson-id: `4c873d74c5a44d71a393f932d775703b`
- **Help With Billing & Your Account** — lesson-id: `60878a56ab794a23b53c31c45d8d6332`
- **Getting Help with Automations** — lesson-id: `9e6dcdd305ee4e2d869c0a33027b2b7f`
- **AIS+ Community Rules** — lesson-id: `6d33ccc0efdd4c9eaa0ac6da9feb87a1`
- **AIS+ Team** — lesson-id: `b8fd0c465e8c4ce1830afb026cdc82f0`

## The AI Partner Model

Course slug: `8de65cec`

### The AI Partner Model

- **The Biggest Wealth Event of Your Life** — lesson-id: `f71476ab1274462a94258fd8dfaefe58`
- **Get Paid Big as an AI Transformation Partner** — lesson-id: `1493ab6e5e1f424fb7d75e7bca03eaa6`
- **The Three Ways AI Creates Profit** — lesson-id: `e3e895fb865946b5bd8c4d30315a6bbe`
- **You Don’t Need Tech Skills.You Need Clear Thinking** — lesson-id: `b5bc975f961b4a26866ce8116deefae9`
- **Earn Consistently Without Chasing Clients** — lesson-id: `cd6cd021b54548848c024a902edb5e07`
- **How to Go from Zero to $10,000 a Month** — lesson-id: `efa4128600b44464bbc00de9e14d29ac`

## Build Your Portfolio

Course slug: `d99017d2`

### Build Your Portfolio → 📍Why You Need a Portfolio

- **It All Starts with Your Portfolio** — lesson-id: `c280cefc6a194f7d8f1a1dbcd502e159`
- **Why You Don't Need a Niche Yet** — lesson-id: `391391865fb44338b381a99b2d6cb4c7`
- **What Businesses Are Happy to Pay For** — lesson-id: `c114f887e59e4136aeff2eb88a9e9258`
- **Your Path to a Client-Ready Portfolio** — lesson-id: `22026173df714931bdf57902be570ebb`
- **What a Client-Ready Portfolio Looks Like** — lesson-id: `a9110d31264141aeac94021ec12d9adb`
- **🏆END OF INTRO 🏆** — lesson-id: `7830cfdd1eb34e49bf65f5aa67cd02fe`

### Build Your Portfolio → 📍Agent Zero: AI Foundations

- **Welcome to Agent Zero** — lesson-id: `19eaf23676da4aa6bb3ec7489c55478f`
- **Track Your Progress** — lesson-id: `a3943eea8d7a4012a994aed11a6e323e`

### Build Your Portfolio → Large Language Models

- **Introduction to Artificial Intelligence 🧠** — lesson-id: `1eee5d5b83c14bdeb3ef36a7abc318b3`
- **Understanding LLMs📘** — lesson-id: `dc2e349aa4bc48829b74752d1d48fb52`
- **Popular LLMs ​ 📊** — lesson-id: `f8ce9de33ce345f08e19f8a909678424`
- **Open Source vs. Closed Source Models 🔓** — lesson-id: `544561202bad4338ba6777212f10e015`
- **Prompt Engineering for LLMs 🛠️** — lesson-id: `56078331279f429782903c82d9c861f7`
- **Module Summary: LLMs** — lesson-id: `459801b07b714fe4ac8fae361582ea0a`

### Build Your Portfolio → Data Foundations

- **What is Data? 📊** — lesson-id: `65868a3130fd4b9782cb20eb5ba0ab66`
- **Data Types 1️⃣** — lesson-id: `9f5df97f2633437bae72dbbc91110b41`
- **What is JSON? 🌐** — lesson-id: `ab96926ddea24c68afeff7ceb4e68c85`
- **Data Processing 🔁** — lesson-id: `67328e4099a54a359e1739bec91b0899`
- **Module Summary: Data Foundations** — lesson-id: `3a71e26ef7944e8eba37355f247d753b`

### Build Your Portfolio → APIs & HTTP Requests

- **APIs & HTTP Requests Overview** — lesson-id: `9ce67c7c215f4ffdb3066ff309f79209`

### Build Your Portfolio → RAG & Vector DBs

- **What is RAG?** — lesson-id: `b68aa9e5f46a475894f0f99590bb66a2`
- **What are Vector Databases?** — lesson-id: `6a065ae9366549f698b1445cd9a596f4`
- **Vector Embeddings** — lesson-id: `bf61acad011c41d8af1f6041c4c63177`
- **Vector Dimensions** — lesson-id: `d93fc3abcf344b299b531d0a4c864af0`
- **Real World Applications** — lesson-id: `e6ca06b1d24b48019623674e4c820c41`
- **Tokenization & Text Processing** — lesson-id: `0b431b2a80d94d43993e6d372da6d12b`
- **OpenAI Tokenizer** — lesson-id: `bc8f2de89005405ead4d8e9e1f2d6530`
- **Context Windows** — lesson-id: `aeea5ef374c24284a2a1e753e19232d2`
- **Module Summary: RAG & Vector Databases** — lesson-id: `dc6d7b489e92446f9b36b649514220eb`

### Build Your Portfolio → ChatGPT to Autonomous Agents

- **What is Modern AI? 📜** — lesson-id: `0041ed5d42864c80a05ca8cd0bfa6280`
- **Understanding AI Workflows 🔧** — lesson-id: `4b891159c3244fb7901754efb9140827`
- **Understanding AI Agents 🤖** — lesson-id: `2deb436434a14da79907f737988d22b2`
- **AI Voice Agents 🗣️** — lesson-id: `964fbaa3c01e4ef899e60cb0be3f5655`
- **Ethical AI 🌍** — lesson-id: `863a9a10b5614cd08d9d8d34ba31614d`
- **Please Share Your Feedback!!** — lesson-id: `179fe40e9c794f688c6b1169ef480e6b`

### Build Your Portfolio

- **🏆END OF AGENT ZERO🏆** — lesson-id: `38dd328fd8d94b1fbbad145adbf2fd1a`
- **📍10 Hours to 10 Seconds V2** — lesson-id: `8bb9f704cba9403bb00471b499a7bdcb`
- **Course Overview** — lesson-id: `dca6d2e7d4744d2490a6bb4817c31b84`
- **The AI Bubble?** — lesson-id: `74f03cc43dc34ec9a8144ba7bb1c790a`
- **Track Your Progress** — lesson-id: `e4cae18dbea54bb4837bc652ecb646f4`
- **🏆END OF 10 HRS 10 SECS V2🏆** — lesson-id: `238fbaa670444269a633e23e9bc9737f`

### Build Your Portfolio → Phase 1: Find Your Personal Leverage

- **The Time Lens: Reclaiming Your Calendar** — lesson-id: `54f0b94d39fe4bdf899274f954500c05`
- **The Personal Audit** — lesson-id: `3520e78eb93e403286050b1f80666786`
- **The Income Lens: Your Growth Flywheel** — lesson-id: `10922c6629ba43868ae4dad4eac42bb9`
- **The Step-by-Step Test (Delegation-Ready Filter)** — lesson-id: `0e1cd3c4bcf34217a8ee654f318c1d52`
- **Scoring with ICE (Personal)** — lesson-id: `95821c1d14594fa8a2a277e3930e5930`
- **End of Phase 1** — lesson-id: `29380777d08540c0958a391af2fa35db`

### Build Your Portfolio → Phase 2: Mapping the Steps

- **Intro to Process Mapping📝** — lesson-id: `f52fc2e1fac24e368867e11c398181eb`
- **Identifying Workflow Triggers⚡** — lesson-id: `74c1a862ce944d54b27e84c37fc98e40`
- **Data Sources and Transformation 🔄** — lesson-id: `07e0118f2f2c4984ab2424828e081d7f`
- **Integrating AI 🤖** — lesson-id: `9c13171b33864788897d53b2ab5c1968`
- **Wireframing 📐** — lesson-id: `5a458f3886f74ae197f17633fb7611f1`
- **Intro to Context Engineering** — lesson-id: `3768f0dc06f64a92bff11fa02f73911d`
- **🎁Bonus: Live Wireframe** — lesson-id: `db19ed63dd1c4948a92b70366836f8ed`

### Build Your Portfolio → Phase 3: Finding Opportunities for Clients

- **Intro to Phase 3** — lesson-id: `0ec385611aaa4bbcbecea6773858e533`
- **The Time Lens for Businesses (Productivity Lesson)** — lesson-id: `88679cbde8294ffba245d1c7d26f99df`
- **The Income Lens for Businesses (Growth Lesson)** — lesson-id: `2c557b4d5f784705897fd6c2d7cb8284`
- **The SOP Audit: Inventory vs. Creation** — lesson-id: `a75d1f329d5a47c4beb1ebf60d3e2719`
- **Scoring Before Mapping (The Business ICE)** — lesson-id: `e97714fa9aa14b34afd97a9f43d34f89`
- **The Live Blueprint: Mapping in the Room** — lesson-id: `b7b3164b9d2b4b79b3991a781c60c7e9`
- **Should you charge for the Opportunity Map ?** — lesson-id: `701e755634e94616a0fcb2ff8b23fd5b`
- **Congratulations! You made it 🎉** — lesson-id: `d65e516b801442c4b6049bca5351fc8a`

## Claude Code

Course slug: `bd6b51dc`

### Claude Code

- **📍Claude Code** — lesson-id: `442250725ac849c69dc24feecdef5eb8`
- **🏆END OF CLAUDE CODE🏆** — lesson-id: `86546f13618a4baa8fa317c318fad32d`

### Claude Code → Phase 1: Setup & First n8n Workflow

- **1.1 INTRODUCTION** — lesson-id: `d28605209691441eb9b1ba9e15fa99fa`
- **1.2 Claude Code Setup** — lesson-id: `55ccd056d2ef4649b6fdaaefdb5bfba1`
- **1.3 Project + CLAUDE.md** — lesson-id: `1494349dc243478eb68ecb9ff5602845`
- **1.4 n8n MCP Server** — lesson-id: `ca61d3a0e7294ef5922625da7c12151e`
- **1.5 n8n Skills** — lesson-id: `68ea1aaaac5541109ed7a9b3db477532`
- **1.6 Verify All Tools Connected** — lesson-id: `c04b63404036465b936b3527d7613895`
- **1.7 Demo - The Full Cycle** — lesson-id: `85f249b9eac5492683eb1386a76b0fa0`
- **1.8 Wrap-Up & Next Steps** — lesson-id: `f731ff71f4e84721a1a6ec437e86f00b`
- **2.1 Intro & The Vibe Coding Framework** — lesson-id: `1b7a727e3aa24b838ff8a0d6704434c2`
- **2.2 Masterclass - Starting the Convo** — lesson-id: `960d5fe61ca64178ad45bed11fc92097`
- **2.3 Masterclass - Building the Workflow** — lesson-id: `a31bef5aeb984061a464bb286565be36`
- **2.3B OPEN AI Credentials** — lesson-id: `a9cb9656a04746ea9841ed1781c3b3a9`
- **2.4 Masterclass - Verification & Testing** — lesson-id: `39985ab4be514bde8f94a6b32d6f2b9c`
- **2.5 Challenge Introduction** — lesson-id: `7ebd15fb51ee474892004c152dc843c8`
- **3.1 Intro & Enhancement Philosophy** — lesson-id: `ee3199a4d4e5428a82c4ae32226252c9`
- **3.2 Masterclass - Enhancement Techniques Part 1** — lesson-id: `1b86a2565cda4c68abefa751b91e9d65`
- **3.3 Masterclass - Enhancement Techniques Part 2** — lesson-id: `f9019fee5d464029b3d1bdadd47f0de5`
- **3.4 Challenge Introduction** — lesson-id: `4e94373a212540f2b8030e56afdfb988`

### Claude Code → Phase 2: Mastering Claude Code

- **1.1 Phase Two Claude Code Intro** — lesson-id: `d36f34ef94064121a0dccf8ed7d39eba`
- **1.2 Understanding Agentic Workflows** — lesson-id: `23c002fdf70f4e5ea1699734db141234`
- **1.3 The WAT Framework** — lesson-id: `51acd25ee1d544d0af24b8eee4d7101e`
- **1.4 First Agentic Workflow** — lesson-id: `460234560d204863ad4c2990248a85ba`
- **1.5 Introduction to MCP Servers in Cloud Code** — lesson-id: `f1969481d1ed4d94b7d2c655d194ae78`
- **1.6 Research Workflow: Firecrawl MCP** — lesson-id: `e49e1a8aa9fd4133a876007010d77e59`
- **1.7 Skills** — lesson-id: `f8bc21126a74442eb5c46ef01cfc89bb`
- **1.8 Skills in Action** — lesson-id: `811c53a6ffd349399eb0df073f9986a0`
- **1.9 Slide Deck Generator** — lesson-id: `5065eba8c90b43029867e776bd608dd0`
- **1.10 Token Management** — lesson-id: `66d9e31641f2455d9bc54c68c2095394`
- **1.11 Personal Challenge + Final Tips** — lesson-id: `24290b8f1cfb42f1b8653af9820bd091`
- **1.12 Phase Two Recap and Phase Three Preview** — lesson-id: `8b65853e7cb846bd8d44e12a1b9e593a`

### Claude Code → Phase 3: Hosting & Deployment

- **1.1 Welcome & Recap** — lesson-id: `5e45022377704b5bbbce6e7bcad6a453`
- **1.2 The Agentic Gap** — lesson-id: `46ad819bc6344cc6a0f3499109ba1288`
- **1.3 GitHub** — lesson-id: `3b1f2bbefc32451fbc13617e311b39ab`
- **1.4 Trigger Dev** — lesson-id: `46362a522b6e4cb48aacb9d57beb0e06`
- **1.5 Secrets Management** — lesson-id: `6e1a611b355b4c01964cc1d47634b9bc`
- **1.6 Masterclass - Scheduled Research Agent** — lesson-id: `0aaf978b5d00478ebf1826e2166e0259`
- **1.7 Masterclass - Webhook Report Generator** — lesson-id: `b2ba5e64df4e419e95e171261e30f521`
- **1.8 Error Handling** — lesson-id: `f00fe0169b91462dada6e763cb49e0b8`
- **1.9 Your Challenge** — lesson-id: `64ea224a233745f78b3f17d1dc961b7f`
- **1.10 Wrap Up** — lesson-id: `fa6ff74ee3b94b75988dc4b47564013b`

### Claude Code → Phase 4: Building Frontends

- **1.1 Intro: Building Frontends** — lesson-id: `281d532c726948c899d92f9c31d3bc6a`
- **1.2 The Mental Model to Building Apps** — lesson-id: `8a010853e30a4e299af56e99dfc6f98f`
- **1.3 Frontend Tips** — lesson-id: `7d2e7d35dff54bceb7c9b7c74cf3af20`
- **1.4 Build My First AI Lead Qualifier App** — lesson-id: `9a0282aa789b4644aa9e203184549ad5`
- **1.5 Authentication & Security Audit** — lesson-id: `22a39036e22447a68dadb43b6a689bb6`
- **1.6 Integrate Stripe Payments** — lesson-id: `0bac6a9668cd43b199f6e4a0f6b26733`
- **1.7 Build a Frontend For Your n8n Workflow** — lesson-id: `cafb13baae024ab3851c3576a1d963e4`
- **1.8 Wrap Up** — lesson-id: `7d53716d8a154fbc93f7576736977463`

## Get Your First Clients

Course slug: `c89ccfd3`

### Get Your First Clients → How to Get Your First Client

- **Why your First Client is the Hardest** — lesson-id: `f3155df6a5cb464c91b19aee5b01e823`
- **The Only 4 Ways to Get Clients** — lesson-id: `b055acee948945e7b391075f74a4009f`
- **Cold vs Warm Outreach** — lesson-id: `1d64d6223f5a4e0c81fcf510bd7180f1`
- **The Retainer Trajectory** — lesson-id: `ec2fce3c12414648ba8d379dffe84ad2`
- **Your Action Plan** — lesson-id: `943fcbd362214d67b74c77f77c674a41`

### Get Your First Clients → Warm Outreach

- **The Fastest Path to Your First Client** — lesson-id: `afe0906b7bf04088803e94bf14a6000d`
- **Creating Your Outreach List** — lesson-id: `e5b4c5a2e1ae4ba7894fd6af8004ee93`
- **What to Say in Your DMs** — lesson-id: `99f05d2bb3d243629059fe82871a6f96`
- **How to Get People on a Discovery Call** — lesson-id: `9d7b393daaaf4f7f964fa70db100beba`
- **Use Mini Audits to Build Trust** — lesson-id: `283178de772a4ebe8e6fded9322bfad0`
- **Use the Rule of 100 to Book Your First Call** — lesson-id: `7c9b73ee1867400a9551beb60eec0eaa`

### Get Your First Clients → Upwork

- **Why Upwork Makes It Easier to Get Clients** — lesson-id: `3712ad1e7e024a509c4303a82b9a8d7c`
- **Optimizing Your Upwork Profile** — lesson-id: `360679a601474181b7ac2b4b8ee3fd49`
- **How to Find High-ROI Jobs to Bid On** — lesson-id: `ec5c0b77b3ed4a1d87688e2fc9903078`
- **Job Proposals: A Proven Framework** — lesson-id: `536426daf0174939981f34745785a3cb`
- **Build Trust with a Loom Video** — lesson-id: `e5bd92ff465749eca47de7a34f51b2c0`
- **Submit Your First 10 Proposals** — lesson-id: `c51210204e754ddab919b66d64cf6e54`

## Scale

Course slug: `114ef87f`

### Scale → 📍 One Person AI Automation Agency

- **What is a One Person AI Agency?** — lesson-id: `6153d1a04d0b47aeb3bb5197a3d20c3c`
- **Phase 2 of AI Agencies** — lesson-id: `bbdeb95dcb30475f979ab876e307fde3`
- **Problem Solving Mindset** — lesson-id: `4df0b193a82149fab2f3345d049c5ac5`
- **Track Your Progress** — lesson-id: `14cc67d06fcc4aa4b947cd26d7aa8a84`
- **🎁 Agency Mastermind Q&A** — lesson-id: `c6a52cfa973342a0a31f0eb93f976bf6`

### Scale → Module 0: Mindset

- **Your Dream Outcome** — lesson-id: `c94e9221e17d4662b4cca8a9bee9d06d`
- **The Transition Curve** — lesson-id: `97c4bb2721024b049ddf8f2ea28f2f82`
- **Accountability & Discipline** — lesson-id: `2ac208a3a9a147089006b23ce1f4e84f`
- **Designing Habits & Weekly Planning** — lesson-id: `8e34a881397147eb90a6db570d828373`
- **Reps > Outcomes** — lesson-id: `0bbde0c319dc4f5698032c115c1e6199`
- **🎁 My Systems** — lesson-id: `f721341e2d3a4e5a935a139a8a9ce5a1`
- **📝 Knowledge Check 0** — lesson-id: `6b6d792291b7487188c705506de3c1ac`

### Scale → Module 1: The AI Opportunity

- **The Automation Landscape** — lesson-id: `8ec72e819e9a40d283a58b27e802aa69`
- **The Blue Ocean** — lesson-id: `271248cf5606492ebd3aee68c53eff0b`
- **Different Paths** — lesson-id: `089be321cfbe4b7e9f7b1e40623d7f02`
- **📝Knowledge Check 1** — lesson-id: `01ecc91ecdea44c8b06336aca1404bd1`

### Scale → Module 2: Business Formation

- **What is an LLC?** — lesson-id: `c73723f34d624490a694f8bdc8343803`
- **Banking & Taxes** — lesson-id: `05539bcf602c4f7e8c7e5d6423aaa2fb`
- **Contracts & Legal Protection** — lesson-id: `1355ee21c22f47d38ba2b577380748ef`
- **Invoicing & Getting Paid** — lesson-id: `57a3e7000b474d19af1402bb0cf0f538`
- **My Tech Stack** — lesson-id: `22ef43f3e9a44936b6828933e647270f`
- **🎁My Secret for 10x Productivity** — lesson-id: `537cd19b0bbd4a2183455345f6fa0b6a`
- **💲2 Months FREE ClickUp** — lesson-id: `8252036870ce4c74b269a9a6b9586cb3`
- **💬 Discussion Post: AI Tools!** — lesson-id: `774ca1843e074c5fb8f1d0ac1eda2148`
- **📝Knowledge Check 2** — lesson-id: `0a26978639ea4480b7c54c1ee19ab4bb`

### Scale → Module 3: Niche Selection

- **Why Niching Down Matters** — lesson-id: `7e09417bc69b498c835c6f9cf0654087`
- **Market Research Methods** — lesson-id: `dca635bae63e4bfe9e81fb12b3ecc8a4`
- **A Good Niche** — lesson-id: `5c792974f0854244aa7577605def3409`
- **Choosing Your Niche** — lesson-id: `710349f090cd491ab1823d950a2a78e5`
- **💬 Discussion Post: Let’s Talk Niches** — lesson-id: `7cea42fdc90f464dae2200b71d646a14`
- **📝Knowledge Check 3** — lesson-id: `366c492414ce4b66a04668df81bcf6c7`

### Scale → Module 4: Online Presence

- **Your Presence Matters** — lesson-id: `d72eb8635ede40a58a92a18d41500426`
- **🎁 7-11-4** — lesson-id: `4047092c40014060957368ee23364208`
- **Business Branding** — lesson-id: `5b6573a6428f455d8047a2dc04ffc4ec`
- **Your Website & Portfolio** — lesson-id: `65c4965908094a11b106b2fff10100f6`
- **Optimizing LinkedIn** — lesson-id: `bceca8867fc2494e83a223db2c343df2`
- **📝Knowledge Check 4** — lesson-id: `c6ac0ecac93c48f8b77f60524e31afbf`

### Scale → Module 5: Packaging & Pricing

- **Why Pricing is Everything** — lesson-id: `69c0e799cd964674848a066f1817c3f1`
- **Pricing Models Explained** — lesson-id: `7200b3a9051a4aaaa80f462e6b99bbf7`
- **The ROI Formula** — lesson-id: `40f194da5cb64009b78c1f895a6ae678`
- **🎁ROI Calculation Template** — lesson-id: `84e48be3df444d5d86ab89c56672325b`
- **Packaging & Anchoring** — lesson-id: `3df59511bc614f2e8b9d0a2cb072af07`
- **Retainers & Maintenance** — lesson-id: `b3587b00bf7440128a4cd18568a5e2c4`
- **Avoiding Pricing Pitfalls** — lesson-id: `a133bff477674252b2c39d6e66564719`
- **Advanced Pricing & Retainers** — lesson-id: `637ff0a4bc8d466bb16f3c7818baa8d4`
- **📝Knowledge Check 5** — lesson-id: `fbfa5a4cf4ee48c181bfb8f982a52d6e`

### Scale → Module 6: Outbound Systems

- **Outreach Foundations** — lesson-id: `71957ba9c8024d5aa75ed16a03db2475`
- **🎁 Sales Mastermind Call** — lesson-id: `ba2d5dc073314bc6ada0ee7380a9abee`
- **Setting Objectives** — lesson-id: `3aa2a3f4bc6a4803bcada3f51155c91e`
- **Cold Outreach Systems** — lesson-id: `510714edf0454b7592f0bb981d2a25f2`
- **Crafting & Sending Messages** — lesson-id: `1721721a72b845688b4ac32e7043e065`
- **Case Study - $500k w/ Cold Email** — lesson-id: `205e489aaac74079b2be16bd858da904`
- **Conversations & Converting** — lesson-id: `22ed697e66184743a2240cca67c0a85c`
- **Fiverr & Upwork** — lesson-id: `65e14762963a4a35b8e667893c3a19df`
- **📝Knowledge Check 6** — lesson-id: `61ddf69b8f9045a3bea015d3c204041d`

### Scale → Module 7: Discovery & Sales

- **The Role of Discovery** — lesson-id: `781177ea44644a93bc00a2296f2aa309`
- **L.R.P Discovery Framework** — lesson-id: `6aab22c9825f465c83d994c580c67836`
- **Discovery Breakdown Example** — lesson-id: `8794b785e60a4f58b5f0fe481ef47e14`
- **Scoping Out Projects** — lesson-id: `c9aa97ac9f834911aa5e57751282256f`
- **From Call to Proposal** — lesson-id: `57b938c45bd54ad181749c99d5e1d100`
- **Building Your Sales Rhythm** — lesson-id: `bfeca7434055442e80136a0d202a6c62`
- **🎁Bonus: TrueHorizon Disco Framework** — lesson-id: `ba5e6ee4f75f4dc1aaf82db271c3cf2e`
- **📝Knowledge Check 7** — lesson-id: `211a9eeca5494308a3d50a6b1864d9ee`

### Scale → Module 8: Client Onboarding

- **Partnership Kickoff** — lesson-id: `05fdda8e614d4397830bf8e839b3de43`
- **Client Portal Setup** — lesson-id: `941beec96d6349d4860f17eecb9e47fe`
- **Collecting Data** — lesson-id: `204ba40d17364204808e32e7fc1b7ac0`
- **n8n Licenses for Hosting** — lesson-id: `1a3adff9f8b54deab9a016d57a0a110e`
- **Testing & QA** — lesson-id: `25df9a2d9ef14bbda5e29ac0139b0ea7`
- **📝Knowledge Check 8** — lesson-id: `7226f3502ad148e38c8d821c1378720b`
- **Delivering n8n Projects** — lesson-id: `b395adf8dcc24a7eb3f6e12406fb3685`

### Scale → Module 9: Sustainable Lead Flow

- **7 Lead Engines** — lesson-id: `cf427cc423b444019fc0a783df6759a4`
- **The Relationship Engine** — lesson-id: `fc28697ee0724c249dbf505c2963ba24`
- **The Event Engine** — lesson-id: `097e815705c04d4092e7144cbb42a345`

### Scale

- **🎁 Finding Constraints and Hiring** — lesson-id: `2124e249835e4cceb9dfb46aab46472e`
- **Nate's YouTube Business Talks** — lesson-id: `b9c73e23566844dab0cd800e2667a2b1`

### Scale → 📍Subs to Sales

- **1) Why Content Matters** — lesson-id: `84ad7ff7cf3240c385b9d30e599af041`
- **2) Identifying Your ICP** — lesson-id: `48f4615d064f45bfbc32b113a226ff31`
- **3) YouTube Strategy and Positioning** — lesson-id: `7e35361d83d84208a973cc999898a8f7`
- **4) Creating Thumbnails** — lesson-id: `e99b0120f2864c218d85e7c25c8554eb`
- **5) YouTube Conversion Process** — lesson-id: `9848dfa108624163a2ac32a3078d98e1`
- **6) YouTube Tech & Tool Stack** — lesson-id: `1e266df0816d479182754f2fd37d2202`
- **7) Content Monetization** — lesson-id: `7c014fc954a5490ea7a88b97b7967bf9`
- **8) Ideation** — lesson-id: `dc06eac36703466db7c28e8461d079b4`
- **9) Systems to Achieve Your Goals** — lesson-id: `121ba15f48ed4c3c807186ab19651762`

## Member Perks

Course slug: `308fec3a`

### Member Perks

- **Weekly Contributor Rewards** — lesson-id: `bfcac5953d684435bcf868a2a796aa56`
- **Become an AIS+ Affiliate 💸** — lesson-id: `359bd69742be4ec8a247e6c865903c2e`
- **Represent the Group!** — lesson-id: `c9ba1a4010894325839d155e2fbfe8e5`
- **The Map Feature** — lesson-id: `4a993a87372b48b8ba354a87978b88bc`

### Member Perks → $3M Savings Vault

- **Welcome to Annual! 🎉** — lesson-id: `25fd7e3a1d8840b5868f6c601bc5c924`
- **Unlock $3M in AI Tools** — lesson-id: `033a1691344640cebe61f78dc3224c2a`
- **How to Unlock Your AI Discount Vault 💸** — lesson-id: `831ca4dd570e40498028b85b1cd8edae`

### Member Perks → Lifetime Membership - Level 9

<!-- ⚠️ NAMING POLICY: the numbered lesson titles below are member-spotlight names (non-team community members).
     They are recorded here only so lesson ids resolve. NEVER repeat these names to a student, and never cite them
     as a source or a person to contact — refer to the section as "the Level 9 lifetime-membership spotlights".
     See SKILL.md → naming policy. -->

- **Unlock FREE Access to the Community For Life** — lesson-id: `54ebd6290e5f4c5ca272a4d1d0f07014`
- **1) Usman Mohammed** — lesson-id: `0da9dd9ab03e41afa7b0c2bd294fe420`
- **2) Michael Wacht** — lesson-id: `74d5800c117f4b5da47a10fe53013e37`
- **3) Jason Hagen** — lesson-id: `2b1d34e2583a45c6bf5dce621288a46c`
- **4) Holger Peschke** — lesson-id: `d8039ffa32df4914a7ff38b7d9fdf386`
- **5) AI Stromae** — lesson-id: `56420faee6e8498e9f631a1822b8c52f`

## Archived

Course slug: `ec6512da`

### Archived → n8n Masterclass

- **Track Your Progress** — lesson-id: `96655f54383046e881b32dc8da96583c`

### Archived → Module 1: Foundations

- **Module 1 Prerequisite** — lesson-id: `fee737721e944ea8bc092f4f6aa68b2d`
- **Familiarizing with n8n** — lesson-id: `e8f4c622d7fb4bd2abb3ba97732498dd`
- **Credentials** — lesson-id: `03d011b540054401b5e3a6d5b01d6a8a`
- **Connecting Nodes for Workflows** — lesson-id: `1bbdbfe96534444697487be21768909a`
- **Front End vs Back End** — lesson-id: `55063456decb4462b0dd6021767b0a58`
- **Handling JSON Data** — lesson-id: `6b0ba87e32e748248def1ae4ae942c90`
- **Binary Data** — lesson-id: `f7a9639a7e314f378d33a5ec5decd480`
- **📝Interactive JSON Practice** — lesson-id: `fb4540238d8240beb032ad8d2c3657b8`
- **🧩Module 1 Puzzle** — lesson-id: `0973d33baaac403694f786396e582d8b`
- **🎁Bonus: Google Connection Guide** — lesson-id: `e647340608b2435891b2fdf29151a9a2`

### Archived → Module 2: Workflows

- **Module 2 Prerequisite** — lesson-id: `79989e40cfae49ac9f61c6ec230bd3be`
- **Pinning & Creating Test Data** — lesson-id: `0ca5948069b049ee811aa414d8a7ff82`
- **JavaScript Variables** — lesson-id: `2710d94b1d7a449a8e1db8ae71eaaac0`
- **Conditional Logic Routing** — lesson-id: `ebd6d6926fe1463293e21a3727c3918c`
- **Handling Items in n8n** — lesson-id: `0930bcae0e7a409c9be13e0868782f2f`
- **APIs & HTTP Requests** — lesson-id: `43c6772955ed43f4b044e9df964e3a4f`
- **cURL Commands** — lesson-id: `772f02e9d6904ca4a84899632dd23a40`
- **Mastering Google Sheets** — lesson-id: `b6325fdf0fb642b985c6db344a53d2d2`
- **🧩 Structured Output Parser Puzzle** — lesson-id: `02769c4da7a1474fa5bb398fc55d44f5`
- **🧩 Sales Data Filter Puzzle** — lesson-id: `0c790d08be0345da9a26b78252c76fcd`
- **🧩 Generate AI Image Puzzle** — lesson-id: `77856084924a4759ae32b457d09a0f73`

### Archived → Module 3: AI Workflows

- **Module 3 Prerequisite** — lesson-id: `1972196c6393440e8d4edb7a9a8b7c59`
- **Chat Models** — lesson-id: `ea49260dfcb34c0b9392775311b52463`
- **Information Extractor** — lesson-id: `e356698889544d928625957f4d674936`
- **Text Classifier** — lesson-id: `b87f5b1fb15a48fcb80e207917579843`
- **AI Agent Node** — lesson-id: `cef8e5e2864748669bc73a0b62dbe93f`
- **🧩 Structured Data Puzzle** — lesson-id: `a451122bb7224a4596074dc555fc8b07`
- **🧩 Merging HTML Puzzle** — lesson-id: `84ebafcd578d484887a6edf747102258`
- **🧩 Review Analysis Puzzle** — lesson-id: `eb43eea53ebc4e418a82a7d55d5fda79`
- **Webhook and Responses** — lesson-id: `42088a477c8f44b9855dc6e611f4c10c`
- **Continue on Error Setting** — lesson-id: `870bb7e6e331403399bc9d91d6b6eb21`
- **🎁Bonus: 25 n8n Hacks** — lesson-id: `97f4aa939b6e4f4ead606e01d68df6f2`
- **📂Project: Personal News Summary** — lesson-id: `1dfe418e6996447d8d13acb22ba10062`
- **📂Project: Daily Events/Tasks Summary** — lesson-id: `1ac57ce9d2994f18b0f629cc9120f246`
- **📂Project: Invoice Processing Workflow** — lesson-id: `c3d8b65108fc4cdc8f3651a9e0b388a2`
- **📂Project: Personal Inbox Manager** — lesson-id: `73afd3dbbd8845ea893d14d7f22229b7`
- **📂Project: UGC Content System** — lesson-id: `c5a52dbb4f6640dabbf89a429dcf3eea`
- **Build 3 Workflows With Me** — lesson-id: `b4d1015206c74c1895d86532d58cbd72`

### Archived → Module 4: AI Agents

- **Module 4 Prerequisite** — lesson-id: `f615bd7caf8c4eddb160d0040bbf5484`
- **Choosing an AI Model** — lesson-id: `d77a5f026ecf411c92a0d482e54a2776`
- **User vs System Prompt** — lesson-id: `9d26754d0cc041459f884e7c103cac7c`
- **Agent Memory** — lesson-id: `8c7c99e3b1334d3b942cd2cc597642d8`
- **Dynamic Memory** — lesson-id: `bfe8d4dafc964113aad2733ad7444cd7`
- **$now and $fromAI** — lesson-id: `3c4649bd7524455d97e2d7de2e3eff35`
- **Tools vs. Nodes** — lesson-id: `f676d9f8e5804f96863ccfd972c6c6f3`
- **Sub-Workflows** — lesson-id: `82d88c145c9048efb30435711ea39e97`
- **Making Agents Correct Themselves** — lesson-id: `25c2635ef37e43f798f694e6a4ff50ff`
- **Agent Logs** — lesson-id: `32275e54533f481bacdea5353e4d3b55`
- **Output Parsing** — lesson-id: `1215639b70894f119ae7160226175bfb`
- **🧩 Structured Output Parser Puzzle** — lesson-id: `f09d3522d2014235a8d993d7ca8917af`
- **Structuring Prompts** — lesson-id: `6dcb3ab16c73451f8d35d66815b8b25c`
- **Reactive Prompting** — lesson-id: `0dc35292d0dc4428a89e930956cf4724`
- **Dynamic Prompting** — lesson-id: `910dae157a574ff5b66c2add798f8969`
- **Live Prompting Example** — lesson-id: `21ddb497236a4626b4d938d3c65e0a64`
- **Creative Problem Solving Mindset** — lesson-id: `ea9fad2ac31e4b9a887890e591eee986`
- **Think Tool** — lesson-id: `4d28ee7a416141b1abfab74dd9fafaef`
- **🧩Email Agent Puzzle** — lesson-id: `e2860c2d0ce540fca47a350156979286`
- **🧩Photoshop Agent Puzzle** — lesson-id: `21fb89940617456baf1f8bd8a8395dcf`

### Archived → Module 5: Advanced RAG

- **Supabase RAG Agent Course** — lesson-id: `ce2e17d43d9f4eb9bef2cd924352b826`
- **📂Project: Dynamic RAG Pipeline** — lesson-id: `640f4a0ccde34058a39928bbc0ac77cd`
- **Metadata Course** — lesson-id: `c48f43c71c2e476f9e82e5d0d6356d62`
- **📂Project: Gemini File Search** — lesson-id: `36681457b2c84b67957003cf6af3eae9`
- **Relational vs. Vector DBs** — lesson-id: `11d9a8885d3f4b86a153fe7b6b52e97d`
- **Why Vector RAG Agents Hallucinate** — lesson-id: `87bcdbcf8d32480fa50038edaad90978`
- **Agentic RAG** — lesson-id: `394a273661314e14800d2f78b3705275`

### Archived → Module 6: Smarter Workflows

- **Production Error Handling** — lesson-id: `2961daad67184d838153bf556c0a683e`
- **Rate Limits & Error Handling** — lesson-id: `db5dbc8bc1e448e3997980fa4c531065`
- **🎁Bonus: Mistral OCR Step by Step** — lesson-id: `68768b4bf935421fbbb136cd733c992c`
- **AI Workflow Evaluations** — lesson-id: `06299ebae78e4816ba6f44cff3026c44`
- **Category Based Evaluations** — lesson-id: `ae4a27ef6276445f87ca6698d2c6b09a`
- **Accuracy Evaluations** — lesson-id: `45fb930686cf44968f05e07cacd341d3`
- **Referencing Binary Files** — lesson-id: `8184984489fb49efa223125b1908145d`
- **Essential JavaScript Functions** — lesson-id: `4ded591412cc41169c3941b584543236`
- **Handling a Sequence of Messages** — lesson-id: `f5780eb7757343178a6d98a2bceaedb8`
- **What is Caching Data?** — lesson-id: `b2ff876bc2944c32b2430fa19b09ff25`
- **Human in the Loop** — lesson-id: `3b075e0292de4177bbff158a2bbef674`
- **LLM Observability** — lesson-id: `4d3fc720f9a04c41bb174ab806dd696b`
- **Logging Agent Actions** — lesson-id: `b470281bcab54b608f87064812e11c82`
- **📝Interactive Keyboard Shortcuts** — lesson-id: `998cc1c5b3a24f958c12082aea971171`
- **Protect Your Webhooks** — lesson-id: `a922742677ba4aa2b30228763003a2e4`

### Archived → n8n Projects

- **Watch This First‼️** — lesson-id: `2413ff29e0394a2c9354d67b0f29efc4`
- **1) Personal AI News Summary** — lesson-id: `4fe6d41c880041b4a711d2b0c4a01941`
- **2) Daily Events/Tasks Summary** — lesson-id: `246182445bc24e019c9568f9d357a7d6`
- **3) Invoice Processing Workflow** — lesson-id: `711f34e09fcc406fada3d2935eeb0e7d`
- **4) Personal Inbox Manager** — lesson-id: `7688631f8855455cb05a1db7df208998`
- **5) Gemini File Search Tutorial** — lesson-id: `a74beb500f3c42dabad5b978252549e6`
- **6) UGC Content System** — lesson-id: `a19388dfbfad41798b9b4335023a8c41`
- **7) Dynamic RAG Pipeline** — lesson-id: `4d5fcf481ccf45338182a27361ab314c`
- **8) Migrate n8n Workflows** — lesson-id: `761458acb4c4407d8ec630c373706454`
- **9) AI Receptionist: n8n MCP / Vapi** — lesson-id: `f6a9fb90a9a44e00b13d0b8384c01ee2`
- **10) Lead Qualifier: n8n / Vapi Outbound** — lesson-id: `9bd1d84482694052b001f2a0edd6a0ab`

### Archived → Fast Track Challenge

- **1. The Sales Agent Live Class** — lesson-id: `e76e590e9eeb450ebf56b384cc07fe50`
- **1.1 The Sales Agent Q&A Call** — lesson-id: `ac9119a01b7f4e10a5b50343c98ee148`
- **2. The Proposal Agent Live Class** — lesson-id: `350c0a8741354d238b822a7827f30fef`
- **2.1 The Proposal Agent Q&A Call** — lesson-id: `0fa8888866b44504b112f3b613e60877`
- **2.2 The Proposal Agent - Fathom Solution** — lesson-id: `0716968c7d6442cb94ffb19573a3d970`
- **2.3.1 The Proposal Agent Fathom Route (NEW)** — lesson-id: `76e5cd918e0044ad886ac504e3c90e34`
- **2.3.2 The Proposal Agent Google Meets Route** — lesson-id: `0562be82f5e04d61b8c7e89857756149`
- **3. The Onboarding Agent Live Class** — lesson-id: `dab279a79c2e42fc9f9c36409ea40cd5`
- **3.1 The Onboarding Agent Q&A Call** — lesson-id: `c3661480d006408899afc33a9f32555b`
- **4. The Final Q&A** — lesson-id: `d64e6ccf8f554b55b5834cf4342de8db`

### Archived → Extra Step-by-Step Builds

- **The Ultimate Assistant** — lesson-id: `7a14a9e4db2b4b4393ec2059effbc6be`
- **Faceless Shorts System V1** — lesson-id: `d5949ac8c73a49b49057f52eeef1218f`
- **Faceless Shorts System V2** — lesson-id: `1e4b585b31424a05a8c5375e99cece74`
- **Human in the Loop Calendar Agent** — lesson-id: `74e7e6b27644497181075e37f78426be`
- **Voice Travel Agent** — lesson-id: `8c14b053d41042d98082d81b37631c53`
- **Human in the Loop Sales Team** — lesson-id: `5d0e39edec3a4193a7eafbbf35bb373b`
- **ElevenLabs Voice RAG Agent** — lesson-id: `592a084c42f444ec80e955fc3e7c5f25`
- **Technical Analyst** — lesson-id: `5bc43fa0f6d84ae5a94af6792f47ee82`
- **News Aggregator & Summarization - Abdul** — lesson-id: `27bd3e3e5a984addb1f28a894e31cc27`
- **Mistral OCR Document Extraction** — lesson-id: `cbf22c2828ab48e9b7c76ed8f848c46b`
- **OpenAI Web Search API** — lesson-id: `59f44dc8868241f280cf50a475935a70`

### Archived → Credentials Setups

- **Telegram** — lesson-id: `ef6606b100dd4ca48413bd3252c945ea`
- **Airtable** — lesson-id: `a40953b912ce45d78086543a072bbec8`

### Archived → Working With Clients

- **Scoping Out AI Agent Projects🔭** — lesson-id: `ebeadf4dde9247c3a750c5c12846ab6a`
- **Discovery Calls & Wireframing** — lesson-id: `45d694ad5d4049e1ab04b121da54f94f`
- **Initial Proposal & Live Wireframe** — lesson-id: `a6bb3ff08dd54132a0314b173a4fd7d1`
- **Our Pricing Framework** — lesson-id: `45d7e9e66e1e4345b40c3e520ca0642b`
- **n8n Licenses** — lesson-id: `49b1e7fcc00842d0a23d111d32230ea1`
- **Lessons Learned in my First 6 Months** — lesson-id: `ddb10062c38f42a6bf3fa9d89f71c867`

### Archived → AirBnB Booking Agent

- **AirBnB Agent Intro** — lesson-id: `bd5aa7fcdf4f4d9d8695349e0cfe823f`
- **SMS Agent** — lesson-id: `eb16a606f377498b97d58ffdc947e722`
- **Restructuring the SMS Agent** — lesson-id: `d24fe4d87aec4f4580b89c23e1a96dd4`
- **Requesting Human Help** — lesson-id: `32dd9e3de934401db9b63845dc47db35`

### Archived → Content Creation Agent

- **Content Creation Agent Intro** — lesson-id: `cc7044dacab74b52925be35899109f31`

### Archived → Newsletter & Research Agent

- **Researching Competitors Newsletters/Websites** — lesson-id: `a454af040f28459ab46693cf0e496ced`
- **More Article Scraping - Titles, Dates, & Content** — lesson-id: `40f59e1c87664c08868e8ac1fdec6549`
- **First POC Newsletter Agent** — lesson-id: `677adf80fa5f4100a79e16d17b2a9de9`

### Archived → The Hackathon Handbook

- **Your Chance to Win $5,000 (and More)** — lesson-id: `b5004797db4c402ba7005a5ed8beeca8`
- **Theme for September: Lead Generation Automation** — lesson-id: `cc095526dc9d412898569f275c9197e3`
- **Quickstart Guide** — lesson-id: `905dca3be56f49eb86c16c43dc67b479`
- **Hackathon Milestones and Dates** — lesson-id: `008fd99ebe8e4e1da360bd45a346e0f0`
- **How We Pick the Winners** — lesson-id: `17c40400986945c1b37ce55a1227aecf`
- **Can You Compete As a Beginner?** — lesson-id: `5be0ff8f22294d52baba906664ab1c1b`
- **🚨Eligibility** — lesson-id: `c53e3e6755e94fe2a28a7cc4dcbf6017`

### Archived → AIS+ Hackathon Winners (PRO)

- **August - Azim K** — lesson-id: `1ee125de0492498a996ba9cd56c887a6`
- **September - Brett F** — lesson-id: `2cf287337fff459d810f81b234c0622a`

### Archived → AIS+ Hackathon Winners (Beginner)

- **August - Kyle Stefan** — lesson-id: `c7d876089faa45beaee8cc409205c5c9`
- **September - Kacper Rutkiewicz** — lesson-id: `053ea47f8d4549eca7f8105d9d72e429`

### Archived → Hackathon Member's Reflections

- **August** — lesson-id: `bce1dd148bd04ab0813a36203b934e8f`
- **September** — lesson-id: `cfa49f19475d4ed5bb7b6316917b23da`

## Community Resources

Course slug: `a7f6a1e7`

### Community Resources

- **Nate Herk - Video Database** — lesson-id: `275b65da2ad945948a74f7a9ae951adf`
- **Community Shared Skills** — lesson-id: `d6875ebd69184ad482e58e1e1cdd4181`
- **AIS+ REFUND POLICY** — lesson-id: `cb2a16d8bed643c5816a1bf0443b4240`
- **How to Upgrade or Cancel Membership** — lesson-id: `c8981e84e00a4eb087e4f6cd702f71dd`

### Community Resources → Agent Skills

- **Skill Builder** — lesson-id: `8b02e74d3a9145bdb4572502ba2c9a3a`
- **Excalidraw Diagrams** — lesson-id: `84f469c668324cb2ac8505f4c6035430`
- **Excalidraw Style Images** — lesson-id: `beb1ed69076c4d7d981657e5aa310bf8`
- **Nano Banana 2** — lesson-id: `eb69d019337543ad8fc32bcc023838b3`
- **Nate's Frontend Design** — lesson-id: `6a491fc4c91d499082c698154d44c5e7`
- **Video to Website** — lesson-id: `55f24abb25cd4cc2bf6805fab019c12c`

### Community Resources → Earn $ With GEMS💎

- **Earn $ With GEMS💎** — lesson-id: `8b7ed205b38b4a12895b72272ee6d5bf`
- **Business Gems** — lesson-id: `829495d708024ab99f9b1c48dc47e49d`
- **Thoughtful Gems** — lesson-id: `290eacc6c0f440c4b44ea3a4f13a752c`
- **Techy Gems** — lesson-id: `457964518cc04fb693b1bf74ddc9d9be`

### Community Resources → Wins 🌟

- **$60k Deal** — lesson-id: `13a70411cf8f440a8111a19a01ceb531`
- **$35k in 2 Weeks** — lesson-id: `281f5e6989e242e5b1b72a794732f195`
- **$25k Deal** — lesson-id: `10bf09a184294ecbb88abc36870d64ac`
- **First AI Automation Client** — lesson-id: `59e4448968254be895ba0ded7b1eab7f`
- **First Project in 10 Days** — lesson-id: `2a43007dc469472eb2da86ce4869178f`
- **First Automation** — lesson-id: `5c6658c5a57f430e937bb5e2d3ce66aa`

### Community Resources → Nate's AI Business Talks

- **Building Agents... Now What?** — lesson-id: `f970318a47b942d695fdf992f1f26bd2`
- **$1,200 in 2 Hours** — lesson-id: `db9a4f12527845e28c2271abf9096465`
- **Sell AI Workflows (w/o Agency)** — lesson-id: `f3a61a0f0eb940e096e3c0e618873fbc`
- **4 Agents for $23k Total** — lesson-id: `624b867d83b24fb7990b7912d5b7e60d`
- **Live $6k Agent Sale** — lesson-id: `c9a500a5ebe5499499fc186b38b4404b`
- **Why No One is Buying** — lesson-id: `e0832f07ba4c4c5e89b662f6209c97e7`
- **Stop Selling Agents, Sell Solutions** — lesson-id: `fb06b3b338714e32a86c735c3da2f0c1`
- **How I'd Make Money with AI in 2026** — lesson-id: `2df49c68b2e4424db96346ae45761712`
- **AI Isn't Hard. It's Misunderstood.** — lesson-id: `d0cadc612867485cb3023e0c59a23627`
- **Becoming an AI Consultant** — lesson-id: `766e62cd3acd49819fb6111859966240`
- **Your First Client** — lesson-id: `f04323994f664c7099c0fea5de36f657`
- **Pricing Workflows** — lesson-id: `352b0f8bc801441cb3e6b66dd0c0a86a`
- **Learning n8n in 2026** — lesson-id: `975292c9ee1e4fb695d4c3302cca2ee9`
- **$2,600 in 2 Hours** — lesson-id: `7c086187ea304e80a4b60de554f3718a`
- **Delivering n8n Projects** — lesson-id: `63f2a18f4714465098068c6dfa4a4f51`
- **$1,650 in 3 Hours** — lesson-id: `3bc08be499fc48c18ce149a85f932ada`

### Community Resources → Client Case Study Vault

- **Jose M Lopes** — lesson-id: `5dbaa6bbf5174bceabd90839c34a574e`

### Community Resources → Monthly Resources (Tech Team)

- **April 2026** — lesson-id: `f21597a7ec9c4d03b7d0e867b2896d67`

### Community Resources → Community Builds

- **Procurement Automation** — lesson-id: `88a3a20b3ae04ae994d190dfed8a6f47`
- **Outbound Vapi Agent** — lesson-id: `5ed637130f0145dbbb699f2516e813cf`
- **Meal Plan Generator** — lesson-id: `277d3dbcc6b049e895492fedef78e8fe`
- **Personalized Paint Protection Guide** — lesson-id: `ea4fef2b78f04bb4acc56ab9fb913de6`
- **Film Crew Booking** — lesson-id: `d4d4f0e783e444bdadb4ccbc7e777f9b`
- **Lead Qualifier** — lesson-id: `866bd80dbac849eda19a80468ef74560`
- **Lead Generator w/ Google Maps Scraper** — lesson-id: `c44fa3328aa243cbb92a2cf6f74efb27`

### Community Resources → Discount Codes 💸

- **Welcome!** — lesson-id: `74cdff2b76f949188b12ba61f11ba72f`
- **Airtop** — lesson-id: `8d5103e1808d480b981346ee30102a7a`
- **Apify** — lesson-id: `1095dbc2972a49a78c8efb4e7ee817f5`
- **Blotato** — lesson-id: `77223072421a4800896dd29a30e464a2`
- **Firecrawl** — lesson-id: `39d1c2373aa5472dabb38e62818fac46`
- **Hostinger VPS** — lesson-id: `99571a6a851d4f6ab51bb970c8a506e9`
- **Lindy** — lesson-id: `0cae933a752d423faf57df7acecbc821`
- **Twitter Scraper** — lesson-id: `c52253ada0a84f9892c19f1fe304e92a`
- **Poppy AI** — lesson-id: `44d251df6908459b82ff540de6b0bdba`

### Community Resources → Identifying n8n Sales

- **What are n8n Sales?** — lesson-id: `65ef23b7b76646afa8f7e0ebffc5e1ea`
- **Part 1** — lesson-id: `2db1a308c75e4a1aa7e774fd5f8383f9`
- **Part 2** — lesson-id: `e9d17e40fe764631b2b870af6f081ced`
- **Part 3** — lesson-id: `5e3bc06ded8a4ae8958effc61d0327fb`
- **Part 4** — lesson-id: `ffd415d2268646f89c1e53601d0e6fab`
- **Part 5** — lesson-id: `d29c3e9c70644fa4879118661db1e2b0`

### Community Resources → Useful Resources

- **Agentic Design Patterns - Guide by Google CTO** — lesson-id: `6f90186c7a8441bb8c63ced1355630a6`
- **RAG Explained: Reranking for Better Answers** — lesson-id: `a19cb1128e7041f68a70a8a998c86aec`
- **Claude Code Quick Reference Guide** — lesson-id: `2cf5778e8c1742d69e81546357296a18`
- **Beginner's Guide to MCP** — lesson-id: `c75d5e1a414e449fb8c2a3bc44be845b`
- **Switch to a Mac from PC - The Easy Way !** — lesson-id: `c5f2191669b44ac8b02f6e060ff19200`
- **Claude Code Project Starter kit** — lesson-id: `c5ca4c7fadfc4ba3b27430b06d178be7`
- **Claude Cowork Explained** — lesson-id: `e616f6d92cf243c89801fd1a8c691978`

### Community Resources → n8n Templates

- **Gamma Proposals (1/19/26)** — lesson-id: `ef4057d61e3d44bb841c3671845e9e80`
- **Outbound Lead Qualifier (1/12/26)** — lesson-id: `325bdd0ebf1d45e1b399da951fc76eee`
- **17 Nodes to Master (11/25/25)** — lesson-id: `ad457e506fa64dad9474d63b4fada61c`
- **Gemini File Search (11/23/25)** — lesson-id: `fbe2ea6347c94935bafacfbf2e9db207`
- **Nano Banana Pro (11/20/25)** — lesson-id: `6bded9b445414736ae9ef71d7b717d39`
- **Gemini 3 Pro (11/18/25)** — lesson-id: `fbce577dd76c4eae91835fcbf9af7f66`
- **Guardrails (11/11/25)** — lesson-id: `5cb3b82669ac4a41be1b31bb99a39271`
- **UGC Content Veo 3.1 and Sora 2 (11/4/25)** — lesson-id: `5ab67ecf2b3347d8a775995d61d68d91`
- **Inbox Manager (10/29/2025)** — lesson-id: `9e2a144b588f493f8b8d5b851a057562`
- **Sora 2 (10/22/25)** — lesson-id: `1e1aa22a5f3a49cbba0d2a31748e6b2b`
- **RAG Pipelines (10/18/2025)** — lesson-id: `0346635f2f7943168b4f44ce50f95e3c`
- **Base44 Web Apps (10/10/2025)** — lesson-id: `a89bba8ac1824066a97a847e062d40aa`
- **n8n Data Tables (9/22/2025)** — lesson-id: `b2cdbe84796c487b99809684b44f46f2`
- **Pinecone Assistant (9/19/2025)** — lesson-id: `a1b088c63d534242b4060eb572fa6e48`
- **Photoshop Agent (9/5/2025)** — lesson-id: `a8d2578d9d2847dca49f1e9d2a84228a`
- **Workflow Evaluations (9/3/2025)** — lesson-id: `f2bb1866a18d4726bb27cce585dfb77a`
- **Newsletter System (8/21/2025)** — lesson-id: `6bc1f35e99f642849206c0695af1d053`
- **Ultimate Media Agent (8/15/2025)** — lesson-id: `bcda7339b08f49f9884b8ca4a264d444`
- **9 Socials (8/13/2025)** — lesson-id: `9b86aba6bcf4455a8de46d1bcc48db79`
- **Error Handling (8/7/2025)** — lesson-id: `37cc3be98b9245b3b08072b8aeda3a24`
- **ElevenLabs Voice (7/31/2025)** — lesson-id: `a7a57899b70e4e4d9acf379ac55c945d`
- **Agent Swarm (7/25/2025)** — lesson-id: `1b03f3d2d93348b1be80356bf1e4a68a`
- **Metadata (7/23/25)** — lesson-id: `f463f023e7b9401b8c0a6947069511fb`
- **First RAG Agent (7/21/25)** — lesson-id: `9573d0250eef492ea67803f2e0ef1f4b`
- **Webhook Security (7/18/2025)** — lesson-id: `5818fc97b4f141549522ca32f6c93d64`
- **Zep Memory (7/14/25)** — lesson-id: `f2b827149f5544d9b692fb464e003a03`
- **Parallelization (7/9/25)** — lesson-id: `3d24d7d9098b45d4a7512d832b299d9d`
- **Resume Screening System (6/30/25)** — lesson-id: `924dffb720a94874a7b7a1331971ccc0`
- **Reranking RAG Agent (6/28/25)** — lesson-id: `81ce58c431644e2cb3680c1e9c64415a`
- **YouTube Strategist (6/24/25)** — lesson-id: `0240c29738594f418c01dc4683fa3bf7`
- **Glass Fruit ASMR (6/22/25)** — lesson-id: `fbefe779b33d4fd89338df1137f3a977`
- **n8n Developer Agent (6/18/25)** — lesson-id: `068faa5382ff4cb18f5183eea5ad8ff2`
- **Video Analysis (6/12/25)** — lesson-id: `897377490550473e9df75df0ba7766fe`
- **Multi Agent System (6/10/25)** — lesson-id: `01c0bcd41df843cbb7953de857d8682c`
- **Browser Agent (6/8/25)** — lesson-id: `48634e51f516477db933e023e8f0bb12`
- **Firecrawl Search and Scrape (6/3/25)** — lesson-id: `89173d10a7a54b31acb1d84e4860fee3`
- **HeyGen Avatar Automation (5/20/25)** — lesson-id: `52ef6cac89be46c69a05333c7fbc740a`
- **Brand Reimagination Shorts (5/18/25)** — lesson-id: `773ae257b97e4e339ce0ecac6e0bc442`
- **Apify Scraping (5/16/25)** — lesson-id: `0150f32389554f8a82657063f2821560`
- **25 n8n Hacks (5/13/25)** — lesson-id: `a6c290d2fd0e488ab4ab3f8be7984397`
- **Faceless Shorts Machine (5/11/25)** — lesson-id: `bfcde09caca641b68c47188966ad788a`
- **APIs for Agents (5/10/25)** — lesson-id: `1e68f9148f7f41d5928e983a688b1a6b`
- **HITL (5/9/25)** — lesson-id: `8f282109e8344c8eabcf9806c1004edd`
- **Track Agent Actions (5/3/25)** — lesson-id: `6702ec942fb34647ad1c96968298ef1a`
- **Dynamic Brain (4/30/25)** — lesson-id: `a52da9c066bc4c9dbb053fda0f337338`
- **Product Videography (4/28/25)** — lesson-id: `6922e50b498a4730a7e4119a0e969e50`
- **AI Marketing Team (4/26/25)** — lesson-id: `9130d5bf6c264e1cb84b8ad8003086de`
- **LinkedIn Post w/ Graphic (4/23/25)** — lesson-id: `01e07023a89d47258309035dfe7f1ec6`
- **Error Workflow (4/20/25)** — lesson-id: `0a3a22951011451c89a1ddd1fbb13a40`
- **Think Tool (4/16/25)** — lesson-id: `febbc9bc7ba84368bc348cc86c3f4afd`
- **Firecrawl Extract Template (4/13/25)** — lesson-id: `a9462168150342559ebbaad99ce08762`
- **3 AI Workflows (beginner) (4/7/25)** — lesson-id: `1f223f21b3e74cbda0e32a93530ee975`
- **Long Form Content Automation (4/3/25)** — lesson-id: `ed5ad73114c349128b76083f2e6730dd`
- **Deep Research PDF Report (3/30/25)** — lesson-id: `078742a906c140ca8015acd8224529c1`
- **Workflows vs Agents (3/27/25)** — lesson-id: `a9b8a470eab04ea4af257f7ec7318bd7`
- **Mistral OCR (3/25/25)** — lesson-id: `c65d10693a174ebca0cafa2a604a20e1`
- **JARVIS (3/23/25)** — lesson-id: `704840e5fa3d4d41bf0fe5e1094665ef`
- **MCP Setup Guide (3/17/25)** — lesson-id: `bed5585263ed48aa9f6e2d180b11895a`
- **Twitter/X Scraper (3/14/25)** — lesson-id: `16bd0c7457c843cc97f675edcbb25536`
- **Faceless Shorts (3/11/25)** — lesson-id: `69313650d7174e3bad1c2b9abece3737`
- **Agentic RAG (3/8/25)** — lesson-id: `9400edbd46f54666b238301700d0b872`
- **Outlook Inbox (3/1/25)** — lesson-id: `94d68f8fdc8746b5b150bb2c37365ac9`
- **Voice Travel Agent (2/15/25)** — lesson-id: `1e09a7ca57104cfdba0b0faaa565aeaa`
- **Human in the Loop Sales Team (2/5/25)** — lesson-id: `d07e54351951460089118e0fc823a328`
- **Ultimate Personal Assistant (2/2/25)** — lesson-id: `c8aa2f1a1897405b82203b3a42fc4e7c`
- **Human in the Loop Calendar Agent** — lesson-id: `3867d97cd8c543f5b76b24eb2e3504e7`
- **Technical Analyst AI Agent** — lesson-id: `c83e4966dee84ed3b3f8236968814855`
- **Newsletter Creation Agent Team** — lesson-id: `fea08355b03241edbeb5717c4a7805cd`
- **4 Agentic Frameworks** — lesson-id: `07459a25321c4634989d1b64825e06c1`
- **Personal Assistant (with Voice Input/Output)** — lesson-id: `a17049cb36b24d7e9bf6c3c4f42dad54`
- **RAG AI Agent - Supabase & Postgres** — lesson-id: `22d8821dafce4d38a2a806e09caa2657`
- **RAG Pipeline 2.0** — lesson-id: `eb67a6a404bd47e798094c5ef9e06ae2`
- **RAG AI Agent - Pinecone & Multiple Files** — lesson-id: `9f8e403909db4cc8af3efbc8390e9481`
- **Blog Writer AI Agent** — lesson-id: `127d9fe55fd249e794c54374c103d381`
- **Scraping Emails From Maps** — lesson-id: `c2e3c3a2712748afadeb1a0c359efa4c`
- **Personal Assistant 2.0** — lesson-id: `740991f6401f4ebc9b9a4cf261001b72`
- **Google Scraping Agent (LinkedIn Profiles)** — lesson-id: `76fca52ce6b94fd6a59600ec8031cdd6`
- **Supabase & Postgres RAG Agent** — lesson-id: `992c81fee6a241c9b353762c877b4380`
- **Scraping Using Custom Engine** — lesson-id: `8dd52a9845c247d48cf969dc1c98b188`
- **Inbox Management AI Agent** — lesson-id: `a2e8a78295714c6191cd875593d9d878`
- **Research and Content Creation Agents** — lesson-id: `d665462b6f624c63b5f023d57f5c4dfa`
- **Invoice AI Agent** — lesson-id: `dc3bb62feea6417e8cc70e67a3dac866`
- **Customer Support Email AI Agent** — lesson-id: `0828e74a79d343519f84bed1dd6f9ebb`
- **Simple AI Lead Nurturing** — lesson-id: `dabaa922a3874d33b95c53fa22b5c3f7`

### Community Resources → Guides📝

- **n8n Licensing** — lesson-id: `ddb6520ff18a4429a64f9a4bd6701f99`
- **n8n <> Google Setup** — lesson-id: `d6111098c5c542249cac0fcccb118fcf`
- **You're Building Agents... Now What** — lesson-id: `a96bb38af4c64e9fb9ae390744f42745`
- **$1,200 for a 2-Hour Build** — lesson-id: `950c075b3c4148be9ac0706d1d6256b0`

### Community Resources → AI Terms

- **What are AI Terms?** — lesson-id: `5681da9b75104710b9f3bbc3fa07b8ae`
- **Level 1** — lesson-id: `3d2c62edaa834508a05818c9439032e3`
- **Level 2** — lesson-id: `d263567cff6b46c985a1732db670bb77`
- **Level 3** — lesson-id: `c7ccae1bb0534f91ae9cc327303a8aa7`
- **Level 4** — lesson-id: `9f047dd60c2e45fdaa86757353c492a8`

### Community Resources → The Daily Node

- **What is the Daily Node?** — lesson-id: `002341c626fe4e24996361a8d01be509`
- **Webhook** — lesson-id: `b851ac092a7b4f489d95fdf259944338`
- **Form Trigger** — lesson-id: `610370c0d60d4297b3cfcf17f4e602ff`
- **Schedule Trigger** — lesson-id: `44ffa0ac7cc94734b62024b7cbea1c63`
- **Manual Trigger** — lesson-id: `e3aa7e29b12243c4b98d274c13f74bbe`
- **Chat Trigger** — lesson-id: `8d5288b9651740f1a795d806e1019780`
- **Error Trigger** — lesson-id: `32f6306898b646f2aef00b0c159183e4`

## Live Call Recordings

Course slug: `f315a988`

### Live Call Recordings

- **FIRST AIS+ CALL | Nov 14th, 2024** — lesson-id: `aa5dc0e9f8074b57a8ac631c6004468e`

### Live Call Recordings → 🎤 Guest Speakers

- **Dan Martell** — lesson-id: `edf00b8caea54373bb13d7abd3be92d7`
- **Cole Medin** — lesson-id: `3e37674f3a544e3fafaef65d880320fe`
- **Max Tkacz (n8n)** — lesson-id: `1f06ab8f579e4768958286eacfa7c8e6`
- **Devin Kearns** — lesson-id: `cc32a1520c0d4062876da79be4dfde8e`
- **Mark Kashef** — lesson-id: `6d82b6d6448d4c4fa2e977dbad52e66f`
- **Zubair Trabzada** — lesson-id: `9394daf583614367be8868e90f6044f6`
- **Nick Sonnenberg** — lesson-id: `305d9010e95743218b5b20940299157a`
- **Bart Veldhuizen (n8n)** — lesson-id: `b2882666156a438e91444d4850b4eac1`
- **Ahmed Mukhtar** — lesson-id: `ec8a5fcb80004469ba33d875dfb7b6ec`
- **The AI Automators** — lesson-id: `2b39b08b11054e33a97ef9dea3c9938d`
- **Zeb Evans** — lesson-id: `dc92bd264ef544079ab0db5934f21eb4`
- **Rex Manchester** — lesson-id: `d84579aa2f364c8abc27f71f584b8bb2`
- **Nate Herk (in a different community)** — lesson-id: `eaa58038891b4d67b011af9f98da7383`

### Live Call Recordings → ⭐ TrueHorizon AI - $211k Deal

- **Part 1 (Scoping & Discovery)** — lesson-id: `ee9c4edc93314dc99c9086e96b3250e4`
- **Part 1 (Bonus Extra Session)** — lesson-id: `7910ae9ffab14dbebfec5433539ba37e`
- **Part 2 (Project Management)** — lesson-id: `80bd2b6a43604a1596c0f3127409e55e`
- **Part 3 (Modern Sales Pipeline)** — lesson-id: `aef6512c213441fa9cd21b164c60c49e`
- **🎁THAI Discovery Call SOP** — lesson-id: `dbf30cfcb0bb40b081a43f4c25a256df`

### Live Call Recordings → 💬 Q&As

- **💬AI OS Architecture?** — lesson-id: `9ff7a9acfb3844f487d4991bdeb17b25`
- **💬Shared Company Data** — lesson-id: `eaa56804b42247c393fcdf81caf42d7c`
- **💬Contracts and SOW** — lesson-id: `fe941a3f5d4243459f2447306c658d37`
- **💬Codex vs Claude Code?** — lesson-id: `24455c6fa877408c883c3293a7679a7a`
- **💬What service/product to offer?** — lesson-id: `0360833ca39148e79ef9b79236197624`
- **💬Building Trust in Outreach** — lesson-id: `5cccb78557a14582b6d0413aaff09259`
- **💬What's the problem you're solving?** — lesson-id: `3736a95baeb64e5faaf2ffa699466279`
- **💬Matthew Fired Claude?** — lesson-id: `41589ce297424a0f85baff6d0d863721`
- **💬From MVP to Production** — lesson-id: `236f094592a042559f2e28cb2bc8253e`
- **💬AI Second Brain** — lesson-id: `a0741b87ce5f44108dd016f91a54adeb`
- **💬Member Wins** — lesson-id: `2376be9ce66d43e2b43a480e6ceb37e7`
- **💬Discovery Calls and Niching Down** — lesson-id: `d22242365ce24524bcd2804c0a6b8cb0`
- **💬n8n, Cowork, Code** — lesson-id: `09b68a4128274401b02cba17a3eb017f`
- **💬Claude Context Management** — lesson-id: `587d977a3dea4688974732a54f5df66f`
- **💬 First Consulting Call** — lesson-id: `8bfc9ad8a8934520bb4a40ce2b3b4a53`
- **💬Claude Code** — lesson-id: `4c005da34bda42e19ec2f6a535644dd5`
- **💬How to Be Productive** — lesson-id: `9337cb9ee44348ed8d4d2d3b0342a65c`
- **💬Claude Code or n8n?** — lesson-id: `1baf60dc3b52476dbb87951f9de7811e`
- **💬Future Trends of AI** — lesson-id: `2f7c680c84c0472e9c4704c9e8d472dd`
- **💬Quality Assurance and Client Communication** — lesson-id: `ca0a50fa602d49af86597b6332eff918`
- **💬Happy New Year** — lesson-id: `32ec1b2e2d7a43e6b7a3a08509823ea4`
- **💬Learning and Scoping AI Automation** — lesson-id: `15a867dfa3cc4d5aa2526f07f3ce9ae9`
- **💬n8n and Other AI Platforms** — lesson-id: `07ccc9778e6c43b5b9ef294956c5b5c3`
- **💬 Overwhelm When Learning** — lesson-id: `ef2e8570b3be45b1b835a82ff5fb1ace`
- **💬High Impact, Low Effort** — lesson-id: `e6d84eea46b445a8b56d6d47cdadbd15`
- **💬Business Consulting and AI** — lesson-id: `6a0fc10e3e574cca99e65ffcbb635915`
- **💬 Conducting an AI Audit** — lesson-id: `37db75ca280c49529d7c022d3c0c61fa`
- **💬 Framing AI Automation Offers** — lesson-id: `21faf40004b74a7a89fa2785980509aa`
- **💬AI Automator Mindset** — lesson-id: `48bc3690353449d2b744f94c0a097a52`
- **💬Product Development and Scaling Approaches** — lesson-id: `80cfbf6023a140259834359d66d5eac0`
- **💬 Getting Started with Client Projects** — lesson-id: `27634d0ec14748ae8c9b32099520d1a8`
- **💬 Agency Pricing & Discovery** — lesson-id: `d1f0196f09ab4a8a904c8d7cbab7bc43`
- **💬Audits, Expectations, & More** — lesson-id: `3b01538ac0a6400c8762cd7be77ed9b2`
- **💬Becoming AI Problem Solvers** — lesson-id: `4625cf3da83641c3ba3b88957ebe7668`
- **💬Mindset for AI Agency Success** — lesson-id: `8fa3d0f273cb4e4ca4b2e2d0d044a0dd`
- **💬Self-Hosting Infrastructure** — lesson-id: `58d3d0458f17498799510723f953b03d`
- **💬 Delivering AI Solutions** — lesson-id: `7f64f9e159f14707bd2840e1f127787c`
- **💬 Voice AI & Lindy** — lesson-id: `d2f66468c7df4587811675258cf0a9f1`
- **💬 AI Products vs. Services** — lesson-id: `daf3798e95994794975d58363990da13`
- **💬Niche Selection for Client Work** — lesson-id: `f5735df3faa74159962cafe816ba1ab4`
- **💬The AAA Model** — lesson-id: `128b476e32414c6498866b4cad72d22a`
- **💬 Starting Your AI Business Journey** — lesson-id: `6c6f64f79ceb41399bf7c39313677611`
- **💬Managing Client Expectations** — lesson-id: `57ead1a01b874c168ba8d5a12fcca9a0`
- **💬Agent/Workflow Architecture** — lesson-id: `260f716496ba4ec8ab8d6e515e2776d0`
- **💬Maintenance for Clients** — lesson-id: `133c0c952d4c4d0ab6e7dd71e24476c3`
- **💬Multi Agent System Design** — lesson-id: `c23bdd5e773f4cd3b0d8396ef51aadbf`
- **💬Google Gemini Vision** — lesson-id: `fa778fa0613240369c2a2061663028d5`
- **💬MCP Implementation Challenges** — lesson-id: `074ce60923ec44e48271bb0cd9916746`
- **💬 AI Learning Path** — lesson-id: `39e3ab494f8e4226baacd59392cc0502`
- **💬 AI Agency Talk** — lesson-id: `41e5055bd80b4cebbb9b5c690989bda8`
- **💬 Custom vs. Productized Solutions** — lesson-id: `07b28dc818bd457a844dcb9a90337b33`
- **💬 Natural Language Workflow Builder?** — lesson-id: `d009d12ef0014233a2bd5b6d6ba42a3c`
- **💬 n8n's Future and Differentiation** — lesson-id: `c7f73e38dedc4dd9b8337b6a03513d5a`
- **💬 The 2025 AI Landscape** — lesson-id: `a6dad695938e40af9e0c058d3c8c4cd8`
- **💬 Vector DB RAG Optimization** — lesson-id: `257f9048b5dc4e32bea92cc76dfe7272`
- **💬 RAG Ingestion and Iteration** — lesson-id: `bb81f933b5b54b38aeaba47a749ea83e`
- **💬 AIS+ Community Vision & Growth** — lesson-id: `394340ec750146a99d08f42e2baa0e22`
- **💬 Workflow Development & Error Handling** — lesson-id: `b996ac9054fe4611887bc59132aae31f`
- **💬 Crawl4AI & AI's Impact on Employment** — lesson-id: `178a235f46b04c99accddb1c4ca5ee7b`
- **💬 Current State of AI Automation** — lesson-id: `c90dd644cc9543ca91655f3ddfa2f7c8`
- **💬 Project Scoping & Security and Data Privacy** — lesson-id: `18668ba0fe384930a6e09a1355b4605a`
- **💬 Financial Analysis Workflow** — lesson-id: `e7d57130a64540dbad404965f4e0689e`
- **💬 AI Content Creation** — lesson-id: `bf984f7407674787a50fb6bd66bf2dc8`

### Live Call Recordings → Chill, Chat & Chaos - Call Recaps

- **Recap (Nov 30, 2025)** — lesson-id: `8e399a810b604328a4799b3e3cc1c132`
- **Recap (Sep 20, 2025)** — lesson-id: `68530aee75114e2aa9595a77b7d51b83`
- **Recap (Nov 16, 2025)** — lesson-id: `a7c1ff0300b84f6bbbbaddef62fd8316`
