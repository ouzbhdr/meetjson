<div align="center">

# ⚡ MeetJSON

**Ultra-Low-Latency (~0.04 ms) Edge MCP Server & Universal Code / JSON Context Optimizer**

[![Website](https://img.shields.io/badge/Website-meetjson.com-00E5FF?style=for-the-badge&logo=cloudflare&logoColor=white)](https://meetjson.com)
[![MCP Endpoint](https://img.shields.io/badge/MCP_HTTP-api.meetjson.com%2Fmcp-10B981?style=for-the-badge)](https://api.meetjson.com/mcp)
[![SSE Endpoint](https://img.shields.io/badge/MCP_SSE-api.meetjson.com%2Fsse-6366F1?style=for-the-badge)](https://api.meetjson.com/sse)
[![License: MIT](https://img.shields.io/badge/License-MIT-F59E0B?style=for-the-badge)](./LICENSE)

Reduce LLM context window bloat and API token costs by **60% – 99%** without losing structural semantics.  
Works out-of-the-box with **Claude Code, Cursor, Windsurf, Cline, Roo Code, and any MCP client** — or as a zero-code OpenAI/Anthropic Reverse Proxy.

[**🌐 Live Playground & Dashboard →**](https://meetjson.com) · [**📖 Documentation →**](https://meetjson.com/docs.html) · [**🔑 Get Free API Key →**](https://meetjson.com)

</div>

---

## 🚀 Why MeetJSON?

Modern AI coding agents and LLM pipelines burn thousands of tokens reading raw source files, bloated API responses, and repetitive JSON arrays into their context window.

**MeetJSON** runs on the global Cloudflare Edge (~0.04 ms processing latency) and provides deterministic, zero-hallucination structural compression before data hits your model:

1. **🧬 AST Code Skeleton Extraction (`mode: "code"` / `"auto"`)**  
   Extracts function signatures, class hierarchies, types, interfaces, and exports while stripping implementation noise. Lets agents understand a 2,000-line file in ~150 tokens before deciding which specific function to edit.
2. **✂️ Intelligent JSON & Data Pruning (`mode: "json"` / `"auto"`)**  
   Deduplicates homogeneous array payloads, collapses deep nested structures, and strips null/empty boilerplate while preserving 100% of schema keys and representative data samples.
3. **🩹 Deterministic Broken JSON & Stream Healer (`mode: "heal"`)**  
   Instantly repairs truncated LLM JSON outputs, unclosed brackets, trailing commas, unescaped quotes, and markdown-wrapped JSON payloads in sub-millisecond time.
4. **🧠 Edge Session Memory (`meetjson_state`)**  
   Lightweight key-value working memory across agent turns so your agent doesn't have to re-read files already analyzed in the session.

---

## 📊 Token & Cost Savings Benchmark (2026 Flagship Models)

Based on standard production input pricing (~$2.00 – $3.00 / 1M tokens for **Claude Sonnet 5.5**, **GPT-6 Sol**, **Gemini 4 Argon**, and $4.00 – $10.00 / 1M tokens for **Claude Opus 5.5** / **GPT-6 Astra**):

| Payload Type | Raw Tokens | MeetJSON Tokens | Reduction | Latency |
| :--- | :---: | :---: | :---: | :---: |
| **TypeScript / Python Module (1,500 LOC)** | ~14,200 | ~1,850 | **-87.0%** | `0.06 ms` |
| **REST / GraphQL JSON Array (500 items)** | ~68,400 | ~1,920 | **-97.2%** | `0.04 ms` |
| **OpenAPI / Swagger Specification** | ~42,000 | ~4,100 | **-90.2%** | `0.05 ms` |
| **Truncated / Broken LLM JSON Stream** | Invalid | Valid JSON | **Healed 100%** | `0.03 ms` |

---

## 🔌 Quick Setup (MCP Server)

MeetJSON is a native remote MCP server supporting both **Streamable HTTP** (`https://api.meetjson.com/mcp`) and **SSE** (`https://api.meetjson.com/sse`). No local Docker container or heavy binary required.

### 1. Cursor / Windsurf / Claude Desktop (`mcp.json`)

Add the following to your `.cursor/mcp.json`, `~/.codeium/windsurf/mcp_config.json`, or `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "meetjson": {
      "url": "https://api.meetjson.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_MEETJSON_API_KEY"
      }
    }
  }
}
```

*(Using an SSE-only client? Use `"url": "https://api.meetjson.com/sse"` instead.)*

### 2. Claude Code CLI

#### macOS / Linux / PowerShell
```bash
claude mcp add --transport http meetjson https://api.meetjson.com/mcp --header "Authorization: Bearer YOUR_MEETJSON_API_KEY"
```

---

## 🛠️ Available MCP Tools

### `meetjson_process`
Compresses code or JSON payloads, extracts AST skeletons, or heals malformed JSON.
- `input` *(string, required)*: Raw source code, JSON string, log output, or malformed JSON stream.
- `mode` *(string, optional)*: `"auto"` (default), `"code"`, `"json"`, or `"heal"`.

### `meetjson_state`
Stores and retrieves compressed structural summaries across agent steps.
- `action` *(string, required)*: `"get"`, `"set"`, `"delete"`, or `"list"`.
- `key` *(string, optional)*: Identifier for the state entry.
- `value` *(string, optional)*: Data to persist when `action` is `"set"`.

---

## ⚡ Direct HTTP API & Zero-Code LLM Proxy

Want to use MeetJSON directly from your backend or CLI?

### PowerShell (Windows)
```powershell
$body = @{
  input = '{"users":[{"id":1,"name":"Alice","role":"admin"},{"id":2,"name":"Bob","role":"user"}]}'
  mode  = "auto"
} | ConvertTo-Json

Invoke-RestMethod -Method Post -Uri "https://api.meetjson.com/v1/process" `
  -Headers @{ "Authorization" = "Bearer YOUR_MEETJSON_API_KEY" } `
  -ContentType "application/json" -Body $body
```

### CMD (Windows)
```cmd
curl.exe -X POST https://api.meetjson.com/v1/process ^
  -H "Authorization: Bearer YOUR_MEETJSON_API_KEY" ^
  -H "Content-Type: application/json" ^
  -d "{"input": "{\"users\":[{\"id\":1,\"name\":\"Alice\"}]}", "mode": "auto"}"
```

### macOS / Linux
```bash
curl -X POST https://api.meetjson.com/v1/process \
  -H "Authorization: Bearer YOUR_MEETJSON_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"input": "{\"users\":[{\"id\":1,\"name\":\"Alice\"}]}", "mode": "auto"}'
```

---

## 🔗 Links

- **Website & Interactive Demo:** [https://meetjson.com](https://meetjson.com)
- **Full Documentation:** [https://meetjson.com/docs.html](https://meetjson.com/docs.html)
- **Author:** OUZ ([@ouzbhdr](https://github.com/ouzbhdr) · X/Twitter: [@ouz_bhdr](https://x.com/ouz_bhdr))
