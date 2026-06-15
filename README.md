 
# 🤖 LinkedIn Content Creator V2 — AI-Powered B2B Lead Generation

> Automate your LinkedIn presence with Agentic AI. This n8n workflow generates hyper-targeted, B2B lead-generating content using local LLMs, and publishes directly to LinkedIn — all managed from a Google Sheet.

---

## ✨ What It Does

| Feature | Description |
|---------|-------------|
| 📊 **Topic Queue** | Manage content ideas in Google Sheets with status tracking |
| 🧠 **AI Topic Research** | Generates B2B topics targeting CTOs, VPs of Engineering, and Tech Founders |
| 📝 **Smart Writing Engine** | Creates high-converting LinkedIn posts with structured output parsing |
| 🏷️ **Auto-SEO Hashtags** | Generates category-mapped hashtags (Broad / Niche / Audience) |
| 🚀 **Auto-Publish** | Posts directly to LinkedIn via the latest REST API |
| ✅ **Status Tracking** | Auto-marks completed posts and stores generated content |

---

## 🆕 V2 vs V1

| Capability | V1 | V2 |
|-----------|----|----|
| **Content Strategy** | Generic topics | Pain-point-driven B2B topics with lead-gen hooks |
| **AI Pipeline** | Single AI Agent | 3-stage: Topic Generator → Content Creator → Hashtag/SEO |
| **Output Control** | Free-form text | Structured JSON parsers for consistent results |
| **LLM Models** | Single model | Multi-model: `qwen3-coder:480b` + `minimax-m3` |
| **LinkedIn API** | Native node (prone to deprecation) | Raw HTTP with `LinkedIn-Version: 202605` |
| **Person ID** | Manual / broken | Auto-extracts `sub` from `/v2/userinfo` |
| **Post Body** | Simple text | Title + Content + Categorized Hashtags |
| **Data Sources** | Firecrawl + Jina AI web search | Fully prompt-driven, zero API costs |
| **Target Audience** | General users | CTOs, VPs of Engineering, Tech Founders, CXOs |
| **Commercial Intent** | None | Every post ends with an inbound conversion hook |

### V2 Special Features

1. **Three-Stage AI Pipeline**
   - **Topic Generator** (Agent + `qwen3-coder:480b`): Generates Title, Rationale, Hook
   - **Content Creator** (LLM Chain + `qwen3-coder:480b`): Writes the post with strict copywriting rules
   - **Hashtag Generator** (Agent + `minimax-m3`): Categorizes hashtags into Broad, Niche, Audience

2. **Structured Output Parsers** — Every AI node enforces JSON schemas, eliminating malformed outputs

3. **Future-Proof LinkedIn Integration** — Uses `/rest/posts` endpoint with protocol version headers; auto-resolves Person URN

4. **Zero External API Costs** — All AI runs locally via Ollama; no Firecrawl or Jina AI needed

---

## 🏗️ Architecture

```
[Manual Trigger]
    │
    ▼
[Content Topic Generator] ──► [Ollama qwen3-coder:480b] ──► [Structured Output Parser]
    │
    ▼
[Append Row in Sheet] ──► writes Title, Rationale, Hook, Status="To Do"
    │
    ▼
[Get Row(s) in Sheet] ──► filters Status="To Do", returnFirstMatch=true
    │
    ▼
[Content Creator] ──► [Ollama qwen3-coder:480b] ──► [Structured Output Parser]
    │
    ▼
[Hashtag Generator / SEO] ──► [Ollama minimax-m3] ──► [Structured Output Parser]
    │
    ▼
[Edit Fields] ──► composes: post title + content + hashtags
    │
    ▼
[LinkedIn Person-ID Extraction] ──► GET /v2/userinfo → extracts `sub`
    │
    ▼
[Post Content on LinkedIn] ──► POST /rest/posts with URN + content
    │
    ▼
[Update Row in Sheet] ──► Status="Completed", writes Content
```

---

## 🚀 Quick Start

### Prerequisites

- [n8n](https://n8n.io/) (self-hosted recommended)
- [Ollama](https://ollama.com/) with models pulled:
  ```bash
  ollama pull qwen3-coder:480b
  ollama pull minimax-m3
  ```
- [LinkedIn Developer App](https://developer.linkedin.com/) with **Share on LinkedIn** + **Sign In with LinkedIn using OpenID Connect** approved
- Google Cloud project with Sheets API enabled

### 1. Import

1. Download `LinkedIn-Content-Creator-V2.json`
2. In n8n: **Workflow** → **Import from File**

### 2. Configure Credentials

| Credential | Source |
|------------|--------|
| `Google Sheets OAuth2` | [Google Cloud Console](https://console.cloud.google.com/) |
| `Ollama API` | Local instance (`http://localhost:11434`) |
| `LinkedIn OAuth2` | [LinkedIn Developers](https://developer.linkedin.com/) |

### 3. Set Up Google Sheet

Create a sheet named **"LinkedIn Posts"** with tab **"V2"** and columns:

| Title | Rationale | Hook | Status | Content |
|-------|-----------|------|--------|---------|

> **Note:** The workflow auto-sets `Status="To Do"` when appending new topics. No manual entry needed.

### 4. Run

1. Toggle workflow to **Active**
2. Click **"Execute Workflow"**
3. The pipeline auto-generates, publishes, and marks rows as **"Completed"**

---

## 🎨 Customization

### Adjust Voice & Niche

Edit the **Content Topic Generator** prompt to change target audience and strategic pillars.

Edit the **Content Creator** prompt to adjust tone, word count, and forbidden phrases.

### Swap LLM Models

| Node | Default | Alternatives |
|------|---------|-------------|
| Topic Generator | `qwen3-coder:480b` | `llama3`, `mistral`, `deepseek-coder` |
| Content Creator | `qwen3-coder:480b` | `llama3.1`, `nemotron-3-super` |
| Hashtag Generator | `minimax-m3` | `phi3`, `gemma2` |

---

## 🔒 Security & Best Practices

- Store credentials in n8n's built-in vault — never commit them
- LinkedIn rate limit: ~100 posts/day
- All AI processing is local via Ollama — zero data leaves your network
- Consider adding a **Wait** node for manual approval before publishing

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| `invalid_scope_error` | Approve "Share on LinkedIn" + "Sign In with LinkedIn" products |
| `Requested version is not active` | Update `LinkedIn-Version` header to latest YYYYMMDD |
| AI outputs reasoning/thinking | Structured Output Parsers enforce JSON; tighten prompts if needed |
| Ollama connection refused | Run `ollama serve` and verify models are pulled |
| Hashtags not categorized | Check `minimax-m3` is installed and Parser 3 schema is valid |

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🔄 V1 → V2 Migration

1. Backup your V1 workflow
2. Update Google Sheet: add `Rationale` and `Hook` columns; rename `Topic` to `Title`
3. Pull new Ollama models (see Prerequisites)
4. Reconfigure LinkedIn credentials for HTTP Request node
5. Remove Firecrawl/Jina AI credentials — no longer needed
6. Test with "Execute Workflow" before activating

---

Built with [n8n](https://n8n.io/), [Ollama](https://ollama.com/), and [LinkedIn REST API](https://developer.linkedin.com/).

> **Star ⭐ this repo if it powers your B2B content engine!**
