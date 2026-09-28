# Source: https://dev.terma.ai/

app.terma.ai/sessions/a7f3c9e2-41b0…

Back to SessionsLive usage

### Token timeline

112 points · click to peek, drag to select

6:11 PMpeak 118K · claude-sonnet-58:25 PM

AgentToolUserErrordrag to select a range

### Session replay

48 messages · 132 tool calls

Prompt 1 of 6

All312Conversation48Tools264Errors1

Search messages, tools, files…

Expand tools

1. Mara S.Prompt 106:11:28 PM

 Add retry with exponential backoff to the API client for idempotent requests, and cover the new behaviour with tests.

2. Model usage · claude-haiku-4-5933 tokens$0.000997006:11:28 PM
3. Assistantclaude-sonnet-506:11:41 PM

 I’ll read the client and its tests first, then add a small retry helper, wire it into the idempotent methods, and extend the test suite for the backoff schedule.

 85.7K tokens$0.21

4. **4 tool calls**Read, Grep
5. EditCallsrc/lib/api-client.tssuccess38 ms

 Input412 tokens06:13:52 PM

 src/lib/api-client.ts

 @@ -41,7 +41,9 @@ export class ApiClient { async get<T>(path: string): Promise<T> {\- const res = await this.fetch(path);\- return this.parse<T>(res);\+ return withRetry(() => this.fetch(path), {\+ attempts: 3,\+ backoff: exponential({ base: 200, jitter: true }),\+ }).then((res) => this.parse<T>(res)); }

6. BashCallpnpm vitest run src/libsuccess4.2 s

 Input286 tokens06:17:40 PM

 $ pnpm vitest run src/lib ✓ src/lib/api-client.test.ts (14 tests) 312ms ✓ src/lib/retry.test.ts (6 tests) 41ms Test Files 2 passed (2) Tests 20 passed (20)

7. Assistantclaude-sonnet-506:18:05 PM

 GET and PUT now retry up to three times with jittered backoff; POST is left alone. Tests cover the schedule and the give-up path, and the suite passes.

 109.2K tokens$0.27

8. Mara S.Prompt 206:24:11 PM

 Nice. Make the attempt count and base delay configurable per client, and document the defaults in docs/api.md.

9. **3 tool calls**Edit, Read
10. Context compacted06:29:48 PM
11. Assistantclaude-sonnet-506:31:02 PM

 Done. \`new ApiClient({ retry: { attempts, baseMs } })\` overrides the defaults of 3 attempts and 200 ms, and docs/api.md now has a Retries section with the schedule table.

 97.8K tokens$0.12

Oldest first312 of 312 events

Show the full session

Codebase attribution

## See where AI-assisted work lands in your codebase

Every session connects to the files it read, edited, or used as prompts. The burn map turns a repository into a living view of AI activity, revealing repeated reads, retry loops, and concentrated activity with the sessions and changes behind each pattern.

app.terma.ai/codebase · August 2026

**AI activity, by file**Aug 1 – Aug 29 · 4 providers · 5 harnessesInvoicedAmortizedAPI-equiv.3 findings this week

AI spend · blended

€0

▲ 6% vs July · 61% seats · 39% metered

Cost / merged change

€0

▼ 12% · 204 changes merged

Needs attention

0

re-read hotspots · CI retries · long loops

Sessions

0

8 engineers · 14 entitlements · 5 harnesses

re-read hotspotsrc/lib/api-client.ts€1,240 · read 4.2×/session

regression originprompts/support-agent/system.md€980 · CH-812

workloads/support/tools/€760

tests/e2e/€610 · CI retries

workloads/kyc/€430

src/services/billing/€290

prompts/kyc-extractor/€240

infra/€150

everything else€1,180

coldhotclick any file for reads · edits · sessions · trend

Findingsthis week

0.0×

api-client.ts is re-read per session on average

€310/wk in repeat reads · `src/lib/api-client.ts` · nothing summarizes the module, so every session re-reads it

**0%**of read tokens

[Draft summary doc](https://dev.terma.ai/dashboard)

€0.0k

Retry-loop sessions cluster on one flaky CI job

11 sessions · CI was re-run to green instead of fixing the test · `tests/e2e/checkout.spec.ts`

**0**retry runs

[Open fix PR](https://dev.terma.ai/dashboard)

−0%

Sessions that start with a plan use 38% fewer tokens

n=204 merged changes · same task class, same repo area

**0**changes compared

[Set plan default](https://dev.terma.ai/dashboard)

Multi-provider by default

## Every provider your team actually uses, on one ledger

A Claude Max seat, a Codex seat, an OpenRouter balance and a raw Anthropic key are four different ways to buy the same thing: tokens. Terma models each engineer's entitlement portfolio as it is — subscription, committed capacity, metered, prepaid — meters effort in tokens as the primitive, and derives money on whichever basis you ask for. Routers are first-class: a call that went Claude Code → OpenRouter → Grok is one session, attributed once.

Entitlement portfoliosregisteredobservedinferred

**Dawson R.**4 rails · 3 harnesses

![](https://cdn.simpleicons.org/claude)Max 20×92% of window![](https://cdn.jsdelivr.net/npm/simple-icons@13/icons/openai.svg)Codex seat4% used![](https://cdn.simpleicons.org/openrouter/181316)OpenRouter$41 left![](https://cdn.simpleicons.org/x/181316)xAI keymetered

€0blended · 38.2M tok

**Daniel O.**3 rails · 2 harnesses

![](https://cdn.simpleicons.org/claude)Max 5×64%![](https://cdn.simpleicons.org/openrouter/181316)OpenRouter$118 left![](https://cdn.simpleicons.org/anthropic/181316)Anthropic keymetered

€0blended · 29.7M tok

**Jesper L.**1 rail · 1 harness

![](https://cdn.jsdelivr.net/npm/simple-icons@13/icons/openai.svg)Codex seat71%

€0invoiced · 22.1M tok

**Mara S.**2 rails · 2 harnesses

![](https://cdn.simpleicons.org/claude)Max 5×48%![](https://cdn.simpleicons.org/openrouter/181316)personal OpenRouter · inferred

€0blended · 17.4M tok

\+ 4 more · entitlements register from billing, observe from harness hooks, or get inferred from traffic · API-equivalent figures always tagged notional

Cross-rail findings3 open

Dawson's AI activity is concentrated on Claude Max

Only 4% lands on Codex, three months running · compare task shape and routing across both tools

4%Codex activity

Max window throttles stall 6 sessions/wk

Dawson hits 92% of the 5-hour window by 14:00 · the affected sessions and fallback routes are attached

6sessions stalled

Metered and seat activity overlap

Daniel's Anthropic key handled €190 of interactive work while his Max seat had headroom

€190overlapping usage

Personal OpenRouter key on the work repo

Mara · inferred from traffic · the sessions, repositories, and provider route are visible for review

governanceunmanaged rail

Breakdowns

## See who did what, where, and with which tools

Pivot the same attributed activity by person, tool, model, provider, harness, or repository area. Follow how sessions become changes, compare working patterns across the team, and keep token and cost context attached.

By person & tool

Priya N.412 sessions€0 · €41/chg

Jonas K.388 sessions€0 · €52/chg

Mara S.301 sessions€0 · €89/chg

deps-botautonomous€0 · €9/chg

\+ 4 more · €4,460 · €/chg = cost per merged change

By model · all providers

sonnet-4.5€0

gpt-5-codex€0

opus-4.1€0

grok-4-fast€0

haiku-4€0

same model via 3 routes priced separately · opus spend concentrated in 3 sessions — flagged

By provider & rail

Anthropic6 Max seats + 2 keys€0

OpenAI4 Codex seats€0

OpenRouterprepaid · 11 models€0

xAImetered key€0

4 providers · 5 harnesses · one ledger — pivot to harness, rail, or basis in one click

Change contextCH-812 · PR #1341

**draft****open****merged Aug 18****deployed**

AI activity

3 sessions · 18 tool calls

Attribution

S-1053 _96%_

Review rounds

1 · CI retries 0

Every line links back to the session timeline, tool calls, file activity, and the turn that produced the change.

Change attribution

## Every merged change keeps its context

Terma connects AI-assisted sessions to commits and pull requests, so every change carries the story of how it was produced: the prompts and tools involved, the files touched, the people who reviewed it, and the token and cost footprint along the way.

Budgets

## Budgets that stop the burn, not the work

Set budgets the way spend actually happens: per team, per repo area, per experiment. Soft thresholds notify in Slack while there's still budget to steer; hard caps stop the harness mid-session. It's the same envelope machinery that guards production tenants — pointed at dev-time.

Dev-time budgets · August1 capped

**Platform team**€6,000/mo · notify 90%

0% used · on pace

**AI tools & prototyping**€3,000/mo · notify 80%

0% used · Slack notified Aug 22

**sandbox/experiments**€500/wk · hard cap

capped Thu 14:10 · 2 sessions stopped, resumable next week

x402 · Agent treasury

## When agents hold wallets, spend needs receipts too

Agents increasingly pay for what they use per-call, on-chain, over x402 — search APIs, media generation, data, gas. Terma treats that as the same ledger: each agent gets a wallet funded from a treasury, with a refill policy and a hard ceiling, and every on-chain payment is reconciled against the tool call that claimed it. Unattributed outflows and claimed-vs-settled gaps get flagged, not buried. Metadata and settlement only — Terma never holds keys.

Agent walletsno keys held

nightly-refactor0x93aA…04C1registered$0.00/mo · cap $60

pr-reviewer0xB2e0…5D19registered$0.00/mo · cap $30

ci-triage0x40C7…E83arefill pending$0.00/mo · cap $20

0x8d21… unclaimedinferred owner$0.00/mo

funded from org-treasury · auto-refill to target · approvals above cap

Reconciliation: Exa call in session S-1017 claimed **$0.42** — settled on-chain **$0.57** · discrepancy flagged to the wallet owner

---

Production correlation

## Follow AI-assisted changes into production

When an AI-assisted change reaches a production workload, Terma marks the deploy and aligns the signals before and after it — outcome, loop depth, latency, and cost per unit. It is correlation at a clear boundary, never a causal claim, with the originating session and files still attached.

CH-812support-agent: tighten tool-use instructionsregression

**merged****deployed Aug 18****canary · 7d vs 7d**

cost / resolved ticket

€1.53 → _€1.87_

loop depth p95

4 → _11_

resolution rate

64.1% → _64.0%_

**Terma**APPAug 25, 09:00

Canary verdict for **CH-812** on support-agent — cost regression, outcome flat.

Cost/resolved ticket **€1.53 → €1.87** · loop depth p95 **4 → 11** · quality flat. Origin: **prompts/support-agent/system.md**, session **S-1053**. Suggested action: scope the compliance re-check to policy-relevant tool results.

Run-time · the same context in production

## See how AI workloads behave in their own units

The same attribution extends to run-time: every production run belongs to a workload and a beneficiary — tenant, feature, shared asset, or platform. Follow outcomes, loop depth, and cost per resolved ticket together, with per-tenant envelopes for the nights a webhook re-fires 240 times.

Beneficiary mix · €51,070 run-time spend, Aug

tenant 62%feature 18%shared 9%platform 11%

support-agentregressing

€0.00

per resolved ticket · 14,120 this month

resolution rate 64.0%▲ 22% since Aug 18

kyc-extractorimproving

€0.00

per auto-cleared case · was €0.48

auto-clear 86.3 → 87.4%▼ 15% after Aug 12 change

article-summarizershared asset

€0.0000

per summary · served to every tenant

read rate 88% · flat▼ 11% after model swap

€0 ~€610~envelope hard cap vs modeled burn during the Aug 24 retry storm

MRR vs AI cost margin per customer, sorted worst-first

Ten-minute setup

## Bring your team’s AI tools into one view

Coding tools, AI workspaces, providers, gateways, and repositories all produce different fragments of the same work. Terma connects that activity to the people and projects behind it.

01 · METER

### Connect your AI tools

Integrations capture conversations and sessions with their context — prompts, tool calls, models, and token usage.

02 · LINK

### Connect the work

Link activity to projects, repositories, files, and changes. Code teams also get a burn map and session-to-change attribution.

03 · WATCH

### See patterns in context

Findings surface hotspots, attribution gaps, repeated work, and production shifts across people and tools.

![](https://cdn.simpleicons.org/claude)Claude Code

![](https://cdn.jsdelivr.net/npm/simple-icons@13/icons/openai.svg)Codex

![](https://cdn.simpleicons.org/x/181316)Grok Build

⚡Hermes

πPi

\>\_OpenCode

![](https://www.google.com/s2/favicons?domain=marlin.wtf&sz=64)Marlin

![](https://cdn.simpleicons.org/openrouter/181316)OpenRouter

![](https://cdn.simpleicons.org/vercel/181316)Vercel AI Gateway

![](https://cdn.simpleicons.org/cloudflare)Cloudflare AI

![](https://cdn.simpleicons.org/anthropic/181316)![](https://cdn.jsdelivr.net/npm/simple-icons@13/icons/openai.svg)Anthropic + OpenAI SDKs

\+ anything that routes model calls

terma

one attributed view

Team activity

Conversation attribution

Code burn map

Attributed findings

Session-to-change links

and contextual alerts

Get started

## See how your team works with AI by Friday

Connect the tools, workspaces, providers, and repositories your team already uses. Conversations, sessions, changes, and team attribution come into view as real activity arrives.

[Create your account](https://dev.terma.ai/sign-up) [Talk to us](mailto:hello@terma.ai)