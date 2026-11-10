# Third-party notices

RuleCrew includes or uses the components below. Each is licensed to you under its own license, shown here; the
RuleCrew Freeware License Agreement does not limit your rights under them. Full license texts ship with the
Windows application (`licenses\` for the IBM components, `_internal\` for the Python libraries and
`_internal\tools\jdk21\legal\` for Java) or are available at the links.

| Component | Used for | License |
|---|---|---|
| IBM ODM Management (Decision Center) MCP server - github.com/DecisionsDev/ibm-odm-management-mcp-server | Decision Center operations | Apache License 2.0 |
| IBM ODM Decision MCP server - github.com/DecisionsDev/ibm-odm-decision-mcp-server | Rule Execution Server calls | Apache License 2.0 |
| IBM ODM AI toolkit for Bob - github.com/DecisionsDev/ODM-AI-toolkit-for-Bob | BAL error hints and rewrites, rule-project design practices, dependency and ruleflow-order checks (a rule that reads what a later task sets), BOM-to-XOM getter and reserved BOM name checks, deployment-file consistency checks (output variables, RuleApp name, ruleset version, XOM management) and decision-table and ruleflow guidance adapted from its odm-builder skill and `Odm.java`; all checks are RuleCrew's own code. (c) Copyright IBM Corporation 2026 | Apache License 2.0 |
| IBM Semeru Runtime Open Edition 21 (OpenJDK with Eclipse OpenJ9) - bundled Java | running ODM's engine and build command | GPL v2 with the Classpath Exception; OpenJ9: EPL 2.0 / Apache 2.0 |
| Python and its standard library | the application runtime | Python Software Foundation License |
| PyInstaller bootloader | the Windows executable | GPL v2 with the PyInstaller bootloader exception |
| Pillow | images | MIT-CMU (HPND) |
| Microsoft Visual C++ and Universal C runtime DLLs | the Python runtime | Microsoft redistributable runtime terms |
| IBM Plex Sans and IBM Plex Mono - bundled fonts (static/fonts) | the web page's typefaces | SIL Open Font License 1.1 (static/fonts/OFL.txt); Copyright 2017 IBM Corp. with Reserved Font Name "Plex" |
| sqlite-vec | memory search | Apache License 2.0 / MIT |
| uv (Astral) | packaging | Apache License 2.0 / MIT |
| MCP Python SDK, Starlette, Uvicorn, websockets, httpx, pydantic, openpyxl, python-dotenv, jsonschema and their dependencies | service, protocols, workbooks | MIT, BSD or Apache 2.0 (each package's own license under `_internal\`) |
| Anthropic and OpenAI Python SDKs | calls to the model provider you configure | MIT |
| boto3 and botocore (AWS SDK for Python) and their dependencies | calls to Amazon Bedrock | Apache License 2.0 |
| google-auth and its dependencies | Google Vertex AI sign-in | Apache License 2.0 |
| azure-identity, msal and their dependencies | Azure OpenAI with Microsoft Entra ID sign-in | MIT |

**Not included.** IBM ODM, IBM's rules compiler (build command) and any IBM ODM jars. RuleCrew uses the ones from
your own licensed ODM installation.

The exact component versions of a release are those in its `_internal\` folder.
