# What stays on your machine, and what leaves it

RuleCrew runs on your computer. **Nothing is sent to RuleCrew or its author**: there is no RuleCrew cloud service, telemetry, analytics, usage reporting or crash reporting. Its own data never goes anywhere else. The only things that leave are the
calls to the language model you chose (and, if enabled, to an embedding service), and the calls to your own
ODM servers. This page lists each one.

## Stays local

| What | Where | Notes |
|---|---|---|
| Settings | `AGENT_HOME/settings.json` | Passwords and API keys are protected for your user: encrypted with Windows DPAPI, or kept in the macOS Keychain on a Mac (the settings file is readable only by you). Only you, on this machine, can read them. A backup restored for another user or machine needs them entered again. |
| Project boards (requirements, design, tests, reviews) | `AGENT_HOME/projects/` | One file per change project |
| Audit log (who asked, approved, changed what) | `AGENT_HOME/audit.jsonl` | Personal values masked as written (limits below). Never sent anywhere, except in a support bundle you send yourself, which masks every line again. Older lines keep what they were written with. Past 50 MB the file moves to a dated `audit-<when>.jsonl`, and those go after `AUDIT_KEEP_DAYS` (default 400; 0 keeps them). |
| Change records: who asked, who approved, every file before and after, the proof | `AGENT_HOME/ledger.db`, table `proposals` (or the team database) | The request and summary text are masked. **Kept exact, because approval and Apply need them: the change's files, and the example requests with their outcomes in the proof (Revert replays them).** So a value a rule or a test example holds is in this record unmasked, it is shown only through the proposal views, it is not in a support bundle, and Forget cannot erase it (it reports it as still found): the record's hash chain depends on it. |
| Interrupted requests: the text, the questions and answers, the model replies needed to resume | `AGENT_HOME/requests.db` | **The request text is kept exact, because a resumed request must mean the same thing.** The model replies kept with it may quote those values too. Old entries are removed at start-up. Forget erases a fact here. The support bundle takes only an outline, masked. |
| Accepted examples the crew learns from (exemplars) | Under the knowledge folder | Masked when recorded |
| Conversations: each person's messages, the crew's replies and the results shown, so a reload or a restart keeps them | `AGENT_HOME/ledger.db`, table `chat_events` (or the team database) | Personal values masked with the same settings as model calls, so a reload shows a token. The files of a proposed change stay exact, for Apply. Kept 90 days (`CHAT_KEEP_DAYS`) and at most 3,000 events per person. Never in a support bundle. Forget (Knowledge) erases a fact here too. |
| Knowledge (lessons, pitfalls, glossary, service facts) | Installed app: `%LOCALAPPDATA%\RuleCrew\knowledge\`; source checkout: `.knowledge\`; or the folder set in Settings > Knowledge folder (`AGENT_KNOWLEDGE_DIR`) | Readable JSON; shown under Knowledge in the Project tab |
| Decision Center exports, compiled drafts | `workspaces\<server>\` under the cache folder: `%LOCALAPPDATA%\RuleCrew\cache\` in the installed app, `.cache\` in a source checkout (`AGENT_CACHE` when set) | Kept per server; drafts older than a day are removed at start-up |
| Traces of model calls | `trace\` under the same cache folder | Masked values stay masked (see below) |

`AGENT_HOME` is `%LOCALAPPDATA%\RuleCrew\data\` in the installed app (or `data\` next to the program when an earlier portable copy kept it there) and `./data` in a source checkout.

No telemetry, analytics or crash reports are sent.

## Leaves the machine

| What | Goes to | What it carries |
|---|---|---|
| Language-model calls | The provider you chose in Settings: your own key (Anthropic, OpenAI, Azure, Bedrock, ...), your company's endpoint, or free providers (demo data only) | Your request; the rule text, decision tables, vocabulary and ruleflow facts the agents read; test inputs and decision outputs; knowledge items relevant to the request |
| Embeddings for memory search (optional) | The embedding endpoint set in Settings > Semantic memory (`EMBEDDING_BASE_URL`), or none | Knowledge item texts and rule names/texts, masked like model calls. Off when no embedding endpoint is set (keyword search is used instead; `agent.log` says so). Your model provider's key is never used to send knowledge to another service: without an endpoint of its own, RuleCrew uses its hosted default embedding endpoint only with a key made for that endpoint (`NEBIUS_API_KEY`, or an embeddings key entered under Semantic memory). `EMBEDDINGS=gemini` sends them to Google's embedding service with your Gemini key, never with real client data |
| Update check (only if you allow it: Settings > Updates, or `RULECREW_UPDATE_CHECK=false` for a whole company) | GitHub (api.github.com), once a day | Nothing about you, your machine or your rules: a request for the public latest-release record of RuleCrew/rulecrew, like opening that page in a browser |
| ODM calls | Your Decision Center and Rule Execution Server only | Exports, imports, compiles, draft deployments and test runs - everything stays inside your ODM |

## Protections

- **Client data policy.** With "Real client rules and data" in Settings (`DATA_POLICY=client`), free cloud
  providers are never called - neither for the agents nor for embeddings - because a free tier may train on
  what it receives. Use your own key or your company's endpoint.
- **Field masking.** Before anything reaches a model, and in the audit log, the request text of change records, kept
  conversations and support bundles, RuleCrew replaces personal values with tokens; a model's answer is unmasked
  locally. It finds two things:
  - Values under a named field (`LLM_MASK_FIELDS`), wherever they appear as `"name": value` or `name=value`: by default
    SSN, tax id, national id, first, last and full name, email, phone, date of birth, IBAN, account, card, passport,
    driver licence and address fields.
  - Patterns in free text (`LLM_MASK_PATTERNS`): US SSN, email, international phone (`+44 20 ...`), US phone, card
    number, IBAN, UK National Insurance number.
  - Your own: `LLM_MASK_CUSTOM=CLIENT=CL-\d{6};;POLICY=P\d{8}`.

  **It does not find** names in free text ("Refuse Jane Roe"), values inside rule text unless they match a pattern,
  identifiers of other countries in forms not listed, passport numbers or dates of birth outside a named field. There
  is no language analysis. If your requests can carry these, add a pattern, or do not put them in requests. Setting the
  variables to `none`, for a client that has approved sending data to a model, turns masking off in these stores too.
- **Your approval.** Nothing is written to Decision Center without your yes; paid model runs above the
  configured limits stop.
- **Local model.** With your company's own model endpoint on your network, no request leaves your network.

## What RuleCrew does not do

- It does not retrain any model. "Learning" means saving notes in the knowledge folder (see "Stays local") that
  are given back to the agents later; model-written notes are marked unverified until a person confirms them.
- It does not send lessons to other installations. General lessons are shared only through a release, after a
  person reviewed them and after anything that may name a client (services, rules, values) was held back.
