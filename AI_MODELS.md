# Setting up your AI model

In the setup page or **Settings > AI model**, choose **With my API key**, pick your provider from the list, then fill
in what the table asks. Keys are stored only on this computer.

| Provider | API key | Other fields | Example |
|---|---|---|---|
| **Anthropic (Claude)** | Claude Console > API keys (starts with `sk-ant-`) | none | key `sk-ant-...` |
| **OpenAI** | platform.openai.com > API keys (starts with `sk-`) | none | key `sk-...` |
| **Azure OpenAI** | Azure portal > your Azure OpenAI resource > Keys and Endpoint - or no key: **Microsoft Entra ID** (`az login`) | **Endpoint** and **deployment name** | endpoint `https://my-resource.openai.azure.com`, deployment `gpt-4.1` |
| **Google Vertex AI** | no key: your Google sign-in (`gcloud auth application-default login`) or a service-account key file | **project ID**, **location**, **model** | project `my-company-ai`, location `global`, model `google/gemini-2.5-flash` |
| **Amazon Bedrock** | AWS console > Amazon Bedrock > API keys (see below) | **AWS region**; **model** optional | region `us-east-1`, model `us.anthropic.claude-sonnet-4-6` |
| **Google Gemini** | Google AI Studio > Get API key (starts with `AIza`) | none | key `AIza...` |
| **Ollama / LM Studio** (on this computer) | none | **model name** | Ollama: run `ollama pull llama3.1`, model `llama3.1` |
| **Others in the list** (xAI, Together AI, Fireworks AI, Cerebras, SambaNova, DeepInfra, Hugging Face, Cohere, Moonshot AI, Alibaba Model Studio, Hyperbolic, Scaleway, OVHcloud, Mistral, Groq, OpenRouter, NVIDIA NIM, Nebius) | your provider's console > API keys | **model name**, exactly as the provider lists it | Cerebras: model `gpt-oss-120b` |
| **My company's AI endpoint** | the key your company gives you | **endpoint URL** (OpenAI-compatible) and **model name** | `https://ai-gateway.mycompany.com/v1`, model `gpt-4.1` |

### Amazon Bedrock, step by step
1. In the AWS console, open **Amazon Bedrock > API keys** and generate a key:
   - a **long-term key** (starts with `ABSK`) is simplest for a trial;
   - a **short-term key** (starts with `bedrock-api-key-`) is valid up to 12 hours and works only in the region it was
     made in - AWS recommends it for production.
2. In RuleCrew choose **Amazon Bedrock**, paste the key, and enter the **region** you generated it in (e.g. `us-east-1`,
   `eu-west-1`).
3. **Model**: leave empty for GPT OSS, which every region offers, or enter a model or inference profile ID your
   account can use - for Claude, e.g. `us.anthropic.claude-sonnet-4-6` (US) or `eu.anthropic.claude-sonnet-4-6` (EU).
   If a model is refused, ask your AWS administrator to enable access to it.
4. Click **Check my setup**: the AI model line turns green when the key, region and model work together.

**No Bedrock API key?** Many companies switch them off. Under **How do you sign in to AWS?** choose instead:
- **An access key, a secret key and a session token**: paste `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` and - for
  temporary credentials, e.g. from your company's AWS access portal - `AWS_SESSION_TOKEN`. Temporary credentials expire
  (usually within 1 to 12 hours): RuleCrew then says so, and you paste fresh ones.
- **My AWS profile or SSO on this computer**: run `aws sso login --profile <name>` once and enter the profile name;
  the credentials then refresh on their own.

Enter all of these in RuleCrew's Settings (stored on this computer only) - never in a chat or an email.

### Google Vertex AI and Azure with your company login
Many companies allow AI only through their own cloud account and their own login - no API keys:
- **Google Vertex AI**: install the Google Cloud CLI, run `gcloud auth application-default login` once (or ask for a
  service-account key file), then choose **Google Vertex AI** and enter the project ID, the location (`global` works
  for most models) and the model name. Your account needs the *Vertex AI User* role.
- **Azure OpenAI with Microsoft Entra ID**: install the Azure CLI, run `az login` once, then choose **Azure OpenAI**,
  set **How do you sign in to Azure?** to **Microsoft Entra ID**, and enter the endpoint and deployment name. Your
  account needs the *Cognitive Services OpenAI User* role on the resource.

RuleCrew asks Google or Microsoft for a fresh sign-in token before it expires (about every hour), so nothing has to
be pasted again.

### Costs and limits
Settings > The crew has two spending limits (per day, $5 by default, and per request, $2). A model RuleCrew has no
price for is counted at a high ceiling price, so the limits still protect you; enter its real price in
**Settings > AI model > Prices** for exact figures.
