# Getting started with RuleCrew

You need your own licensed IBM ODM - its installation folder on this computer (RuleCrew uses its build command and
engine) - and your rule projects. An AI model is needed only for chat and changes.

There is no download yet: release 0.3.4 was withdrawn to be rebuilt. A demo video comes first, then the download.
The steps below describe that download.

## Start

1. Download `RuleCrew-<version>-windows.zip` from [Releases](../../releases) once it is published.
2. Unzip it into any folder, for example `C:\Tools\RuleCrew`.
3. Double-click **`odm-rule-agent.exe`**.
   - Windows may say "Windows protected your PC" (the program is not code-signed yet): click **More info** >
     **Run anyway**.
4. Your browser opens **http://localhost:8765**. If it does not, open that address yourself.

Java is included; you do not need to install anything else.

The first screen is **Open your project**. Choose one of the two below. You get the same crew, approval card and proof.
**Where to run** stays on ODM's embedded engine unless you name a non-production Rule Execution Server and your
production one. The approval card and the evidence say which engine ran.

If you choose **Real client rules and data**, four-eyes is on: nobody approves their own change. Setup asks who may
approve. You can switch it off on purpose in **Settings > Governance**.

## Rule Designer workspace

1. Enter your workspace folder and your ODM installation folder.
2. Create a RuleCrew user name and password. This is a new sign-in, not your ODM login.
3. Click **Open project**, then sign in.
4. Click **Explain a decision** or **Health check**. Neither needs an AI key, and neither changes anything.
5. With an AI model set up, describe a change in the box. Read the card, then **Approve & apply**.
6. The change is now in your folder, uncommitted. Refresh in Rule Designer, review it and commit with your own tools.

## Decision Center

1. Choose **Decision Center**. Enter its address, user and password, and your ODM installation folder.
2. Click **Open project** and sign in with your own Decision Center account.
3. Click the project name in the header and choose the service and branch.
4. Use Explain, Health and chat as above. RuleCrew keeps its own local copy for you; you never manage it.
5. An approved change goes to a working branch, never main. Merging and deploying stay with your team.

Decision Center's history shows the account RuleCrew writes with. Its import offers no place for a comment, so the
requester and approver are recorded in RuleCrew's ledger and evidence pack, not in Decision Center's history.

## If the change needs a new field in Java

1. RuleCrew says what is needed before it spends on building the change.
2. Approve the Java change it proposes, or make it yourself.
3. Click **BOM Update** in Rule Designer.
4. Tell RuleCrew to continue. It checks again and proves the change.

What is verified so far: RuleCrew stopping early, and re-checking on the workspace. BOM Update itself has not yet been
run in a real Rule Designer as part of this flow.

## When something waits

A **1 waiting** badge appears in the header only when a change waits. Click it to reach the card, even if a colleague
started the request. **Recent decisions**, next to it when nothing waits, lists what was decided.

## AI model

Choose it in setup or in **Settings > AI model**. Keys stay on this computer. Eight providers are listed, with the rest
under **More providers**. See [what to enter for each one](AI_MODELS.md).

## Setting up your AI model

See [AI_MODELS.md](AI_MODELS.md).

## Troubleshooting

| Symptom | What to do |
|---|---|
| The browser does not open | Open http://localhost:8765 yourself |
| "ODM build command not found" | Check the ODM installation folder: it must contain `buildcommand\rules-compiler\rules-compiler.jar` |
| Decision Center or RES red in the checks | Check the address and that you can open it in the browser; check user and password |
| "No AI model yet" | Explain and Health work without one. For chat and changes, add one in **Settings > AI model** |
| "This RuleCrew uses its own account" | Use the RuleCrew account made in setup, not your ODM login. Forgot it? On that computer, in a terminal in the program's folder, run `rulecrew reset-password <user name>` (`rulecrew.cmd` beside `odm-rule-agent.exe`); it is recorded in the audit log |
| "Spending limit reached" | Raise the limits in **Settings > The crew** |
| Port 8765 already in use | Another RuleCrew is running: use it, or quit it from its page |
| Anything else | `%LOCALAPPDATA%\RuleCrew\data\agent.log` has the details |

## Feedback

[Open an issue](../../issues/new) with the RuleCrew version, your ODM version and what you did. Remove anything
confidential first. Security problems: see [SECURITY.md](SECURITY.md).

*RuleCrew is an independent product, not affiliated with, endorsed by or supported by IBM. IBM, IBM Operational
Decision Manager, ODM, Decision Center, Rule Designer and Rule Execution Server are trademarks of IBM Corp.*
