# ningsiri_agent

# NingSiri MCP Hub - Agent Technical Manual (v2.0 Native)

This manual defines the protocol for both the **Legacy REST Hub (v1)** and the **Native MCP Server (v2)**. Use v2 for modern agents (Manus AI, Claude, etc.) that support the Anthropic Model Context Protocol.

---

## 🚀 Native MCP Server (v2.0)

The native server uses JSON-RPC over stdio. It is located at `web/mcp/v2/server.py`.

### Capabilities:
- **Resources**: Read-only access to Ning's Soul DNA.
- **Tools**: Dynamic generation of prompts and context.

### Tools Reference:

### Tools Reference (v2.0 Native):

| Tool | Scopes Required | Parameters | Output |
| :--- | :--- | :--- | :--- |
| `get_persona_profile` | `voice` | `token` | The full `Soul.md` content. |
| `get_voice_examples` | `voice` | `token`, `limit` | List of approved captions. |
| `generate_caption_prompt` | `voice` | `token`, `title`, `description` | Structured prompt for LLMs. |
| `search_products` | `products` | `token`, `q`, `limit` | JSON list of matching products. |
| `search_videos` | `videos` | `token`, `q`, `limit` | JSON list of matching videos. |

---

## 🛡️ Permission Scopes

Access is now granted via granular **Scopes**. An agent can have one or more of the following:

- **`products`**: Access to the product catalog and shop metadata.
- **`videos`**: Access to the tutorial library and YouTube metadata.
- **`voice`**: Access to the NingSiri Persona DNA and approved voice examples.
- **`all`**: Universal access (Default for `admin` level tokens).

> [!IMPORTANT]
> If a tool is called without the required scope, the server will return an `Access Denied` error. Check your token's scope assignment in the Hub.

---

## 🔑 Authentication (Legacy v1)

For the REST API, use your assigned **X-Agent-Key**.

### 🛠 REST Command Reference (v1.2)

Base URL: `https://ningsiri.com/mcp/v1/?action=[COMMAND]`

| Command | Param | Description |
| :--- | :--- | :--- |
| `context` | `vdo_id` | Returns video data + linked products. **Use this first.** |
| `search` | `table`, `q` | Parametric search across whitelisted tables. |
| `sync_health` | - | Metadata completeness report. |

---

## 🧶 Persona: หนิง (Voice DNA)

When generating content, you **MUST** align with the following DNA:

### 🗣️ Core Patterns:
1.  **Warmth**: Always address as "เพื่อนๆ" (friends) or "ทุกคน" (everyone).
2.  **Reassurance**: Use "ไม่ต้องกังวล" (don't worry) or "ไม่เป็นไร" (it's okay).
3.  **Permission**: Use "ตามใจเรานั่นเองค่ะ" (it's up to you).
4.  **Politeness**: End sentences with "นะคะ" or "เนาะ".

### 📚 Domain Terms:
- **คท.**: Single Crochet
- **พ1ค**: Double Crochet
- **รห**: Slip Stitch
- **เฉลี่ยไป**: Average it out (used to defuse anxiety over stitch counts).

---

## 📋 Implementation Strategy for Agents

1.  **Initialize**: Call `get_persona_profile` to load the brand DNA.
2.  **Learn**: Call `get_voice_examples` to see how "หนิง" speaks in reality.
3.  **Draft**: Use `generate_caption_prompt` to get a prompt template.
4.  **Finalize**: Execute the prompt with an LLM to produce the final Thai caption.
5.  **Refine**: If possible, check the `Supervision` table (v1) to see if previous drafts were corrected.
