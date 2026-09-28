# Source: https://dev.terma.ai/cli

Onboarding

## Two commands with different owners, then proof

\`terma setup\` runs once per developer and signs you in. \`terma install\` lands once per repository as a pull request. \`terma doctor\` verifies the whole chain end to end with a real scratch commit.

1. 01once per machineInstall the binaryOne static binary, no runtime. Homebrew, the install script, or npm — all verify the release checksum first.
2. 02once per developerSet up your machineSigns you in through the browser (PKCE, single-use code), asks which project to report to, binds your identity, and points Claude Code, Codex and OpenCode at Terma. A checklist decides what is sent before anything is written.
3. 03once per repositoryWire the repositoryBinds the repo to the project you picked, then writes the commit hooks, the agent adapters and .terma.toml into files you commit. terma goes through the hook manager the repo already uses and shows the plan before asking to write it. No secrets — one merged PR onboards everyone.
4. 04any timeProve it worksEvery check names what it verified and, on failure, the one command that fixes it. The scratch commit runs in a temporary worktree, and the coverage figure is what the dashboard will attribute.

terma · install the binary

```
$ 
```

Playing through the steps. Pick one to pause.Replay

How commits get stamped

## Trailers, not guesses

Every hook is a one-liner that shells out to \`terma hook <event>\`. Agent hooks record which files each session touched; prepare-commit-msg intersects that with the staged files and appends one trailer pair per session that actually contributed. Pure human work gets nothing.

**Staged for commit**Toggle files to see the trailers follow the staged set.

- `src/billing/prorate.ts`Claude Code
- `src/billing/prorate.test.ts`Claude Code
- `src/api/plans.ts`Codex
- `docs/CHANGELOG.md`you

**Commit message**git commit

```
feat(billing): prorate invoices on plan change

Credit the unused portion of the old plan when a customer switches mid-cycle.

Agent-Session-Id: 018f3a2c-7d4e-7a1b-9c3d-2e5f6a7b8c9d
Agent-Tool: claude-code/2.1
```

### One trailer pair per session

Several sessions in one commit produce several pairs. Merge and squash messages are never touched.

### Under 50 ms, zero network

prepare-commit-msg reads local manifests only. CI enforces the budget, and no hook ever blocks on Terma being reachable.

### Spooled, then delivered

post-commit retires the committed files and appends an event to a local spool; a background flush delivers it with backoff.

Ask what your agents did

## The ledger, readable from the terminal

Everything a connected agent exports is readable back — by you, or by an agent shelling out to terma, because output switches to JSON whenever the caller is not a terminal. Build a question and watch the command and its answer change.

Who

EveryoneAdaPriyaMarco

Since

todayyesterday7d30d

Group by

usermodelsourceprovider

Output

tablejson

terma · usageCopy

```
$ terma usage --since todaySOURCE       NAME          USER ID           COST USD  INPUT    OUTPUT  CACHE READ  CACHE WRITE  TOTAL TOKENS  MODEL CALLSclaude-code  Ada           7c5c8f2e1a9b4d03  18.9180   1344655  150364  12977317    439792       14912128      77claude-code  Priya         b31e0d9c77a24f18  7.4400    966138   119922  8413746     246140       9745946       86codex        Ada           7c5c8f2e1a9b4d03  5.9220    1142646  52872   3672026     0            4867544       37opencode     Marco         e9a4c1207d5b3f66  3.6120    492068   57846   3006601     72264        3628779       44openrouter   (no user id)                    1.2840    1872240  144066  0           0            2016306       127TOTAL                                        37.1760   5817747  525070  28069690    758196       35170703      371
```

\`--user\` takes a name, email, alias or id. One person is often several principals (the same email seen by Claude Code and by Codex), so a name match covers every agent they use.

\`--since\` and \`--until\` take RFC 3339, a date, a relative age (\`24h\`, \`7d\`), \`today\` or \`yesterday\`. \`usage\` sums spend that settled inside the window; \`session list\` selects by when a session started.

Traffic attributed to an API key rather than a person shows as \`(no user id)\`. Group by api-key to see which one.

The sessions behind the numbers

```
$ terma session list --user dawson --since yesterday$ terma session events <routing-key> --source claude-code --tools-only$ terma session git <routing-key> --source claude-code$ terma principal find dawson
```

Coding agents

## Hooks where the agent runs, telemetry where it bills

Each adapter is a one-liner that shells out to \`terma hook <event>\`; all logic lives in the binary, so updating terma never means touching the committed files.

### ![](https://cdn.simpleicons.org/claude)Claude Code

Project hooks in .claude/settings.json: SessionStart, PostToolUse (edits), SessionEnd. OpenTelemetry export via terma setup.

### ![](https://cdn.jsdelivr.net/npm/simple-icons@13/icons/openai.svg)Codex

A notify entry announces each turn; .codex/hooks.json turns apply\_patch into a file manifest. Trust the hooks once with /hooks.

### ▮Cursor

.cursor/hooks.json for the IDE and cursor-agent: sessionStart, afterFileEdit, sessionEnd. Spend arrives via the Enterprise export.

### \>\_OpenCode

One dependency-free plugin turns OpenCode events into OpenTelemetry and hands session and file events to terma hook.

Commands

## Everything terma does

\`terma <command> --help\` documents each. Output formats: \`-o table|json|yaml|csv\`; a non-terminal caller gets JSON without asking.

Filter commands

### Onboarding

- `terma setup`Sign in and connect your coding agents (once per developer)
- `terma install`Wire a repository for commit stamping (once per repo, lands as a PR)
- `terma uninstall`Remove everything terma install wrote to this repository
- `terma doctor`Verify the whole chain end to end and predict coverage
- `terma status`Show what is connected, what is queued, and the predicted coverage

### Insights

- `terma usage`Sum token usage and cost over a time window, by person, agent or model
- `terma session list`List sessions, most recently active first
- `terma session get`Show one session's roll-up
- `terma session events`Replay one session's events in conversational order
- `terma session git`Show the git and GitHub actions a session performed
- `terma principal find`Find the principals matching a name, email, alias or id

### Harnesses

- `terma connect`Point one or more agent harnesses at Terma
- `terma disconnect`Stop a harness exporting to Terma
- `terma harness status`Show which harnesses are connected
- `terma spool flush`Deliver queued events to Terma now
- `terma spool status`Show what is queued

### Account

- `terma login`Authorize this machine in your browser
- `terma logout`Revoke this machine's credential
- `terma whoami`Show the identity and scope of the current credential
- `terma project use`Select the project subsequent commands read from
- `terma org list`List organizations you belong to
- `terma update`Update terma to the latest release

Get started

## Onboard a repository this afternoon

Install terma, run \`terma setup\` on your machine, then open the pull request \`terma install\` writes. \`terma doctor\` tells you the coverage before the first real commit lands.

[Create your account](https://dev.terma.ai/sign-up) [Browse the source](https://github.com/miradorlabs/terma-cli)