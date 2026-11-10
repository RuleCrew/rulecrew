# RuleCrew™

**AI agents for IBM® Operational Decision Manager (ODM) rule development and maintenance.**
Describe a business change in plain English; RuleCrew's agents find the affected rules, decision tables and
ruleflows, write the change, and prove it on ODM's own engine before you approve it.

**Why RuleCrew exists:** to help IBM ODM teams turn a policy change into a tested change ready for approval. How much
time that saves on your rules has not been measured; on the [benchmark](BENCHMARK.md)'s sample projects a case takes
the crew about 1 to 7 minutes. We keep improving it with every release, driven by what ODM users need.

**RuleCrew 0.3.4 is early access.** Expect rough edges, and results that vary between runs.

> **Free for now, no liability.** RuleCrew is free to use during early access. It is provided as is, without
> warranty, and its author accepts no liability for any loss or damage from its use - you review, test and approve
> every change before it reaches production. See [No liability](#no-liability) and the [license](LICENSE).

**Watch the demo:** a narrated walkthrough recorded with an earlier version (0.3.3) - business questions answered
from ODM's engine, generated documentation, and agents planning, building and testing a rule change. A new demo of
0.3.4 is being recorded.

https://github.com/user-attachments/assets/98f04965-fc1a-4e1f-8f25-1e3a3c46443e

> **Runs entirely on your machine.** RuleCrew has no cloud service and sends nothing to RuleCrew or its author -
> no telemetry, analytics, usage data or crash reports. Your rules, data and settings stay on your
> computers and your own ODM servers. The only calls that leave your network are to the language model **you**
> choose and configure (for example your Anthropic, OpenAI, Azure OpenAI, Amazon Bedrock or Google Vertex AI account,
> your company's AI gateway, or a model hosted inside your network), under your own agreement with that provider. If you allow it, RuleCrew also asks GitHub once a day for the
> latest version number - nothing about you or your rules is sent. See [PRIVACY.md](PRIVACY.md).

## Why RuleCrew
- **Built for existing rule projects.** RuleCrew is made for maintaining rule projects you already have (brownfield):
  it reads your rules, decision tables, ruleflows and data model before it changes anything.
- **Checked before you see it.** Every change is built with IBM's build command, run on ODM's engine and
  regression-checked against known decisions. A draft that fails a check is rejected internally and never offered
  for approval.
- **Bring your own AI model.** Anthropic, OpenAI, Azure OpenAI, Amazon Bedrock, Google Vertex AI, or a model inside
  your network through your company's gateway. Your rules go only to the provider you choose.
- **Explains decisions in plain words.** It runs a decision, reads the trace of which rules fired, and tells you why
  the outcome is what it is.
- **Writes tests for you.** Test scenarios with expected results, boundary values included - see below.

## Tests it writes for you
Ask for tests of a rule or a behaviour ("write tests for the maximum amount rules"). RuleCrew:
- Writes 4 to 12 scenarios, each with a short id, a one-line description, input values and expected results.
- For every threshold in a condition, tests just below, at and just above the boundary; for each rule, a case where
  it fires and one where it does not.
- Runs each scenario on the current build and compares the actual results and the rules that fired with the
  expectations. A scenario that still differs after review is left out of the suite and shown to you with the
  evidence - it may point at a real rule problem.
- Proposes the rest as a Decision Center test suite (Excel scenarios). Nothing is added until you approve it.

## What it does

**For business users - understand your decisions, in plain language**
- **"Why was this application refused?"** Paste the transaction (request data) or describe it: RuleCrew runs it on
  your test Rule Execution Server, traces which rules fired and which did not, and explains in everyday words why
  the outcome is what it is - and what would have to change to get the expected result.
- **"Which rules decide X?"** Ask about any part of the logic - eligibility, pricing, limits, dates - and get the
  rules involved, their conditions and their order, without reading rule code.
- **"Write the documentation"** - a readable description of a decision service: its flow, rules and data model,
  produced from the rules themselves.

**For rule developers and maintainers - change rules safely**
- **Changes rules from plain English** - action rules, decision tables and ruleflows; a data model follows your XOM through BOM Update.
- **Works where your rules are** - in Decision Center (on a working branch; `main` is never changed), or in a Rule
  Designer workspace folder (approved changes are written there uncommitted; you commit). No plug-in is needed.
- **Checks every change on ODM's engine** - the build must pass, examples run before and after, changed decision-table
  rows are run, known decisions are replayed, and critics try to break the change. The card says what ran and what
  did not. A change that fails a check is not offered as ready.
- **Tests it writes for you** - requirement and boundary examples, regression replay of known (or production)
  decisions, and Decision Center test suites (Excel scenarios); every approved decision becomes a re-runnable suite.
- **Prove my change** - checks any change in your workspace, whoever wrote it, with no language model.
- **You stay in control** - nothing is applied without your approval; every action is in a local audit log.

**What "proven" means, and what it does not.** Proven means every required check ran and passed on the examples and
known decisions RuleCrew had. It does not guarantee the change is what your policy means: a misreading made the same
way in the rule and its examples passes every check. Inputs nobody tried are not covered, and a limit the boundary
check cannot read is said on the card and relies on the examples. A person still reads every change.

See the step-by-step [Getting started guide](GETTING_STARTED.md).

## Download
From [Releases](../../releases) - the latest is [0.3.4](../../releases/tag/v0.3.4):
- [`RuleCrew-0.3.4-windows.zip`](../../releases/download/v0.3.4/RuleCrew-0.3.4-windows.zip) - the Windows desktop application (Java included). Unzip, run
  `odm-rule-agent.exe`; the browser opens at `http://127.0.0.1:8765`; fill in Settings.
- A Rule Designer plug-in is included as an optional early-access add-on. It works on the rule projects open in Rule Designer; Decision Center work happens on the web page.

**Updates.** RuleCrew can tell you when a new version is out (you are asked at first start). Install it by
unzipping or installing it anywhere - your setup is saved once (in `%LOCALAPPDATA%\RuleCrew`) and every version uses it. This is an early-access release: each version stops
working about six months after its release date (shown in the app), and newer early-access versions are free.

**Free during early access.** RuleCrew is free while it is in early access. Later versions may add paid editions or
paid features (for example team features and support); any such change will be announced in the release notes
before that version is published, and a version you already have keeps the terms it came with.

**Windows SmartScreen.** `odm-rule-agent.exe` is not code-signed yet, so Windows SmartScreen may block the first
start. To run it, click **More info**, then **Run anyway**. If your company requires signed software, your IT team
can sign `odm-rule-agent.exe` with its internal code-signing certificate before rolling it out.

## Requirements
- Windows 10 or 11. (macOS support is being tested.)
- **Your own licensed IBM ODM** (9.0 to 9.6 supported; 8.11 and 8.12 experimental). IBM ODM is **not** included. RuleCrew was developed and tested
  against IBM ODM 9.6 using IBM's publicly available *IBM ODM for Developers* container image, under IBM's terms
  for that image; this does not imply any review, certification or endorsement by IBM.
- A language model you provide: Anthropic, OpenAI, Azure OpenAI (key or Microsoft Entra ID), Amazon Bedrock (API key,
  access keys or AWS profile), Google Vertex AI, many other providers listed in Settings, or any OpenAI-compatible
  endpoint (your company gateway, or a model you host). Tested end to end with Anthropic Claude; other providers
  use the same standard interface - please report what you find.

## Tailored to your organisation
RuleCrew can be customised to your needs - for example your AI provider or company gateway, your branch and review
conventions, approval rules (four-eyes, who may approve), your naming and documentation standards, additional checks,
and integration with your CI/CD or ticketing. Tell us what you need: [open an issue](../../issues/new).

## Status
<a id="no-liability"></a>**No liability.** RuleCrew is free early-access software that is still being improved. It is provided as is, without warranty, and its author accepts no liability for any loss or damage from its use - including rule changes it proposes, decisions your rules make, or anything written to your systems. Using it, and reviewing, testing and approving every change before it reaches production, is entirely your own responsibility. The [license](LICENSE) has the binding terms.

Early release, free to use. Results of the agents vary between runs; RuleCrew's checks stop changes that fail
them, but you remain responsible for reviewing and testing every change before it reaches production.
Problems and questions: [Issues](../../issues). Security problems: see [SECURITY.md](SECURITY.md).

## License and trademarks
RuleCrew is proprietary software, free to use under the [RuleCrew Freeware License Agreement](LICENSE): free for
individuals and organizations, no redistribution, modification or reverse engineering, provided as is without
warranty. Your rules, data and the changes RuleCrew drafts are yours; you remain responsible for reviewing them and
for the decisions your rules make. Third-party components: [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

RuleCrew is an independent product, not affiliated with, sponsored by, endorsed by or supported by IBM. IBM, IBM
Operational Decision Manager, ODM, Decision Center, Rule Designer and Rule Execution Server are trademarks or
registered trademarks of IBM Corp., in the US or other countries or both. See [TRADEMARKS.md](TRADEMARKS.md).
