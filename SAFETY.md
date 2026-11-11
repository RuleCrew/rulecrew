# Safety: what RuleCrew can and cannot do to your systems

Read this before you connect RuleCrew to a Decision Center or a rule workspace.

## What it can change
| Where | What RuleCrew does | When |
|---|---|---|
| Decision Center | Imports a change set into the **working branch you selected** | Only after a person approves the change on its card |
| Decision Center `main` | Nothing - RuleCrew refuses to write to `main` | Never |
| Rule Execution Server | Deploys **private draft RuleApps** (named for RuleCrew) to test a change before you see it, only when you test on a Rule Execution Server instead of the embedded engine (Settings > Governance can make it ask first); a draft left behind by an interrupted run stays on the server until you remove it | While it checks a change |
| A rule workspace folder | Writes the changed files to the folder, uncommitted, after your approval. It makes no commits and no branches (in Rule Designer you apply them through Compare) | After your approval; committing is yours |
| Your computer | Its own settings, knowledge and logs in `%LOCALAPPDATA%\RuleCrew` | Always |

## What it never does
- Write to `main`, delete rules without showing it on the card, or apply anything from the page, the plugin or a connected assistant without approval.
- Write a Java-backed object model (BOM, vocabulary, mappings) in Decision Center: it tells you what to add instead. A data model with no Java is changed on a working branch, on the approval card.
- Write a member derived from Java into an existing project's BOM anywhere: that is Rule Designer's BOM Update.
- Send your rules, data or knowledge to RuleCrew or its author.

## What leaves your computer
- **Your AI model provider**: the request, the rules it needs and the engine's results, under your own agreement with
  that provider (or none, with a model hosted inside your network).
- **GitHub, once a day, if you allow it**: a version check. Nothing about you or your rules is sent.
- See [PRIVACY.md](PRIVACY.md) for the details.

## Writing without a person: two exceptions you turn on yourself

- A script or CI step you run: `rulecrew propose --write`, `rulecrew baseline --write` and `rulecrew engine --write`
  write to the folder with no one approving. Running the command is the decision. `propose --write` writes only a
  proven change: every gate passed and every required gate ran.
- A connected assistant: its tools do not write by default (`write=false`) and return the result for a person to apply. With
  `RULECREW_MCP_WRITE=true` a person lets it write proven changes (`odm_propose`) and first test packs (`odm_baseline`)
  to the folder with no one approving. `odm_init` creates a new workspace in an empty place and refuses an existing one.

## How a change is checked before you see it
Every proposed change is built with ODM's own build command and run on ODM's engine: the examples from your request,
every changed table row, and a replay of known past decisions. A change that fails a check is not proposed as ready. The card
shows what was checked and what was not.

**What "proven" means, and what it does not.** Proven means every required check ran and passed on the examples and
known decisions RuleCrew had. It does not guarantee the change is what your policy means: a misreading made the same
way in the rule and its examples passes every check. Inputs nobody tried are not covered, and a limit the boundary
check cannot read is said on the card and relies on the examples. A person still reads every change.

## How to stop it
- A request stops by itself after its working-time limit (45 minutes by default) or its cost limit
  (Settings > The crew); nothing half-done is written.
- **Quit agent** in the header (shown to approvers) stops everything. Without approver rights, end
  `odm-rule-agent.exe` in Windows Task Manager; the agents stop with it.

## Your responsibility
**No liability.** RuleCrew is free early-access software that is still being improved. It is provided as is, without warranty, and its author accepts no liability for any loss or damage from its use - including rule changes it proposes, decisions your rules make, or anything written to your systems. Using it, and reviewing, testing and approving every change before it reaches production, is entirely your own responsibility. The [license](LICENSE) has the binding terms.

Agent results vary between runs. RuleCrew's checks stop changes that fail them, but you remain responsible for
reviewing and testing every change before it reaches production. Start on a test branch or a copy of a workspace.
