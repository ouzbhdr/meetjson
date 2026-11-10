<div align="center">

# ⚡ MeetJSON

**Drop-In OpenAI / Anthropic / Gemini BaseURL Proxy & Edge MCP Server (~0.04 ms)**  
*Cut LLM input token costs by **60% – 99%** with a single environment variable or `baseURL` change.*

[![Website](https://img.shields.io/badge/Website-meetjson.com-00E5FF?style=for-the-badge&logo=cloudflare&logoColor=white)](https://meetjson.com)
[![BaseURL Gateway](https://img.shields.io/badge/BaseURL-api.meetjson.com%2Fv1-10B981?style=for-the-badge)](https://meetjson.com/docs.html)
[![MCP Endpoint](https://img.shields.io/badge/MCP-api.meetjson.com%2Fmcp-6366F1?style=for-the-badge)](https://api.meetjson.com/mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-F59E0B?style=for-the-badge)](./LICENSE)

Works transparently with **Claude Code, Cursor, Cline, Roo Code, Windsurf, OpenAI SDK, Anthropic SDK, LangChain, and 15+ LLM providers** (OpenAI, Anthropic, Gemini, DeepSeek, Groq, OpenRouter, xAI, Mistral).

[**🌐 Live Playground & Token Calculator →**](https://meetjson.com) · [**📖 Documentation →**](https://meetjson.com/docs.html) · [**🔑 Get Free API Key →**](https://meetjson.com)

</div>

---

## 🔥 10-Second Drop-In Setup (Zero Code Changes)

You don't need to rewrite your prompts or install local daemons. Point your existing `BASE_URL` to MeetJSON — your provider API keys (`sk-ant-...`, `sk-proj-...`, `sk-or-v1-...`, `AIza...`) pass through directly while MeetJSON compresses bloated tool outputs, file reads, and JSON payloads in **~0.04 ms** at the Cloudflare Edge.

### 1. Claude Code (Terminal)

#### PowerShell (Windows)
```powershell
$env:ANTHROPIC_BASE_URL = "https://api.meetjson.com/mj_live_YOUR_MEETJSON_KEY"
claude
```

#### CMD (Windows)
```cmd
set ANTHROPIC_BASE_URL=https://api.meetjson.com/mj_live_YOUR_MEETJSON_KEY
claude
```

#### macOS / Linux
```bash
export ANTHROPIC_BASE_URL="https://api.meetjson.com/mj_live_YOUR_MEETJSON_KEY"
claude
```

*(Or use without a MeetJSON key on the instant free tier: `https://api.meetjson.com`)*

---

### 2. OpenAI / Anthropic / Universal SDKs (Node.js & Python)

#### TypeScript / JavaScript (`openai` or `@anthropic-ai/sdk`)
```typescript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY, // Your real OpenAI / OpenRouter / DeepSeek / Groq key
  baseURL: "https://api.meetjson.com/mj_live_YOUR_MEETJSON_KEY/v1"
});

const res = await client.chat.completions.create({
  model: "gpt-6-sol", // or claude-sonnet-5-5, gemini-4-argon, deepseek-chat
  messages: [{ role: "user", content: "Analyze this payload..." }]
});
```

#### Python (`openai` / `anthropic`)
```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-...",
    base_url="https://api.meetjson.com/mj_live_YOUR_MEETJSON_KEY/v1",
)
```

---

### 3. Cursor / Cline / Roo Code / Continue.dev

In your IDE settings under **Override OpenAI Base URL** or **Anthropic Base URL**, paste:
```text
https://api.meetjson.com/mj_live_YOUR_MEETJSON_KEY/v1
```

Supported provider routes out-of-the-box:
- **Anthropic (`/v1/messages`):** `https://api.meetjson.com/v1`
- **OpenAI (`/v1/chat/completions`, `/v1/responses`):** `https://api.meetjson.com/v1`
- **Google Gemini:** `https://api.meetjson.com/v1/gemini`
- **OpenRouter:** `https://api.meetjson.com/v1/openrouter`
- **DeepSeek / Groq / Mistral / xAI:** `https://api.meetjson.com/v1/{deepseek|groq|mistral|xai}`

---

## 🧠 What Happens Under the Hood in 0.04 ms?

1. **🧬 AST Code Skeleton Extraction:** Strips implementation noise from large source files in conversation history while keeping 100% of function signatures, types, interfaces, and exports intact.
2. **✂️ Intelligent JSON & Log Pruning:** Deduplicates homogeneous JSON arrays (e.g., 500 database rows or API items → schema + representative sample) and strips null/empty boilerplate before sending to the upstream LLM.
3. **🩹 Real-Time Broken JSON & Stream Healer:** Automatically repairs truncated JSON tool calls, unclosed brackets, and unescaped quotes on the fly.
4. **📊 Live Savings Telemetry:** Every response includes `X-MeetJSON-Saved-Tokens` headers and real-time cost analytics in the [MeetJSON Cockpit](https://meetjson.com).

---

## 📊 Token & Cost Savings Benchmark (2026 Flagship Models)

Evaluated against production input pricing (~$2.00 – $3.00 / 1M tokens for **Claude Sonnet 5.5**, **GPT-6 Sol**, **Gemini 4 Argon**, and $4.00 – $10.00 / 1M tokens for **Claude Opus 5.5** / **GPT-6 Astra**):

| Workload / Payload Type | Raw Input Tokens | Via MeetJSON BaseURL | Token Savings | Edge Overhead |
| :--- | :---: | :---: | :---: | :---: |
| **Claude Code 30-Turn Repo Refactor** | ~410,000 | ~89,000 | **-78.3%** | `0.06 ms` |
| **REST / GraphQL JSON Array (500 items)** | ~68,400 | ~1,920 | **-97.2%** | `0.04 ms` |
| **TypeScript / Python Module (1,500 LOC)** | ~14,200 | ~1,850 | **-87.0%** | `0.05 ms` |
| **Truncated / Malformed JSON Tool Output** | `SyntaxError` | Valid JSON | **100% Healed** | `0.03 ms` |

---

## 🔌 Native Remote MCP Server (Optional)

In addition to the BaseURL proxy, MeetJSON exposes a native remote MCP server (`meetjson_process` & `meetjson_state`):

```json
{
  "mcpServers": {
    "meetjson": {
      "url": "https://api.meetjson.com/mcp",
      "headers": {
        "Authorization": "Bearer mj_live_YOUR_MEETJSON_KEY"
      }
    }
  }
}
```

---

## 🔗 Links

- **Website & Live Cockpit:** [https://meetjson.com](https://meetjson.com)
- **Full Documentation:** [https://meetjson.com/docs.html](https://meetjson.com/docs.html)
- **Author:** OUZ ([@ouzbhdr](https://github.com/ouzbhdr) · X/Twitter: [@ouz_bhdr](https://x.com/ouz_bhdr))
