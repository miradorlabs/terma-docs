# SOC 2 requirements

Mirador Labs is preparing a SOC 2 Type 1 audit. This file lists the engineering rules that contributors and coding agents follow in this repository. It is maintained in every Mirador repository and given to coding agents through `AGENTS.md`. This repository is public, so internal infrastructure details are kept out of it.

**How to use it**
- Read it before changing anything it covers: secrets, dependencies, CI, releases or data handling.
- Never weaken a control here to get a change through. If a change needs one to change, stop and ask a maintainer.
- A change that alters a control updates this file in the same pull request.

## Change management

- Every change reaches the default branch through a pull request; branch rules block force-pushes and deletion. Required CI checks must pass and are never skipped or loosened.
- Where this repository publishes releases, only Mirador release managers create release tags, and the release pipeline publishes them, never a person by hand.

## Secure development

- Never commit a secret: no keys, tokens, passwords or credentials in code, tests, fixtures, examples or CI logs. Secrets live in the CI secret store. Secret scanning with push protection stays on.
- Dependency alerts and security updates stay on. Fix confirmed vulnerabilities within **Critical 7 days, High 30 days, Medium 90 days**.
- Add dependencies only from their official registries, pinned by lockfile. Do not add a third-party SDK that sends data off the user's machine (analytics, crash reporting, AI) without Mirador's vendor review.
- Never log secrets, tokens, passwords or user content.

## Data

- No customer or personal data in this repository: tests and examples use made-up data.
- Software that runs on a user's machine sends data only to Mirador's own documented endpoints over TLS 1.2 or higher, and stores credentials in the operating system's secure store where one exists.

## Reporting security issues

- Report a suspected vulnerability privately by email to support@mirador.org (support@terma.ai for Terma) with "Security" in the subject line, never in a public issue.

## This repository

terma-docs is Terma's public documentation site: MDX pages and the site configuration in `docs.json`.

- **Use only made-up data** in examples and screenshots (for example `ada@example.com`): never a real customer, person, organization, session, key or token.
- **Document only public hosts and production behavior.** Never document internal hosts, non-public environments or internal tooling.
- **Keep the privacy and collection pages true.** `concepts/privacy.mdx` and the collection-policy pages describe what Terma collects; update them when the product's behavior changes.
