# 🤖 LinkedIn Content Creator — AI-Powered Automation

> **Automate your LinkedIn presence with AI.** This n8n workflow researches topics, generates professional posts using local LLMs, and publishes them directly to LinkedIn — all managed from a simple Google Sheet.

---

## ✨ What It Does

| Feature | Description |
|---------|-------------|
| 📊 **Topic Queue** | Manage content ideas in a Google Sheet with status tracking |
| 🔍 **AI Research** | Automatically searches the web for latest articles and insights |
| 📝 **Smart Writing** | Generates inspiring, professional LinkedIn posts with emojis & hashtags |
| 🚀 **Auto-Publish** | Posts directly to your LinkedIn profile via API |
| ✅ **Status Tracking** | Marks completed posts and stores generated content |

---

### Workflow Nodes

| Node | Purpose |
|------|---------|
| **Google Sheets** | Read topic queue & update status |
| **AI Agent** | Orchestrates research & writing |
| **Ollama Chat Model** | Local LLM for content generation |
| **Firecrawl Search** | AI-native web search |
| **Jina AI Reader** | Extracts full article content from URLs |
| **LinkedIn API** | Publish posts to personal profile |

---

## 🚀 Quick Start

### Prerequisites

- [n8n](https://n8n.io/) (self-hosted or cloud)
- [Ollama](https://ollama.com/) with a chat model (e.g., `nemotron-3-super`, `llama3`, `mistral`)
- [Firecrawl](https://firecrawl.dev/) API key
- [Jina AI](https://jina.ai/) API key (free)
- [LinkedIn Developer App](https://developer.linkedin.com/) with `w_member_social` scope
- Google Cloud project with Sheets API enabled

### 1. Import the Workflow

1. Download `LinkedIn-Content-Creator.json`
2. In n8n: **Workflow** → **Import from File**
3. Select the downloaded JSON

### 2. Configure Credentials

| Credential | Where to Get |
|------------|-------------|
| `Google Sheets OAuth2` | [Google Cloud Console](https://console.cloud.google.com/) |
| `Ollama API` | Local Ollama instance (`http://localhost:11434`) |
| `Firecrawl API` | [Firecrawl Dashboard](https://firecrawl.dev/) |
| `Jina AI API` | [Jina AI](https://jina.ai/) (free, no card) |
| `LinkedIn OAuth2` | [LinkedIn Developers](https://developer.linkedin.com/) |

### 3. Set Up Google Sheet

Create a sheet with these columns:

| Topic | Status | Content |
|-------|--------|---------|
| AI Image Generation Trends | To Do | *(empty)* |
| Remote Work Best Practices | To Do | *(empty)* |

### 4. Configure the Workflow

- **Google Sheets node**: Select your sheet and tab
- **AI Agent**: Adjust system prompt for your tone/voice
- **LinkedIn node**: Ensure `w_member_social` product is approved

### 5. Activate & Run

1. Toggle workflow to **Active**
2. Click **"Execute Workflow"** or set a schedule trigger
3. Watch your LinkedIn auto-populate with AI-generated content! 🎉

---

## 🎨 Customization

### Adjust Post Style

Edit the **AI Agent System Prompt** to match your voice:

- Formal vs. casual tone
- More/fewer emojis
- Longer or shorter posts
- Industry-specific hashtags

### Change the LLM Model

In the **Ollama Chat Model** node, switch to any Ollama model:

| Model | Best For |
|-------|---------|
| `llama3` | General purpose |
| `mistral` | Fast & efficient |
| `nemotron-3-super` | High quality (default) |

### Add Image Generation

Extend the workflow with:
- DALL-E / Midjourney / Stable Diffusion API
- Auto-attach generated images to LinkedIn posts

---

## 🔒 Security & Best Practices

- **Never commit credentials** — use n8n's built-in credential store
- **Rate limiting** — LinkedIn allows ~100 posts/day per user
- **Content review** — Consider adding a manual approval step before publishing
- **OAuth scopes** — Only request `openid profile w_member_social` for personal posting

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| `invalid_scope_error` | Ensure LinkedIn app has "Share on LinkedIn" product approved |
| `NONEXISTENT_VERSION` | Use HTTP Request node instead of native LinkedIn node (v2.4.8 bug) |
| `urn:li:person` error | Use `urn:li:member:ID` format with numeric ID from `/v2/me` |
| AI outputs reasoning | Tighten system prompt; add `===POST===` delimiter extraction |
| Empty search results | Check Firecrawl API key and rate limits |

---

## 📄 License

MIT License

Copyright (c) 2026 [Your Name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

## 🙌 Credits

Built with:
- [n8n](https://n8n.io/) — Workflow automation
- [Ollama](https://ollama.com/) — Local LLMs
- [Firecrawl](https://firecrawl.dev/) — AI web search
- [Jina AI](https://jina.ai/) — Article extraction
- [LinkedIn API](https://developer.linkedin.com/) — Social publishing

---

> **Star ⭐ this repo if it helps you automate your content!**
