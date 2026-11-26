---
name: webcite
description: "Use when a task involves reading documents or checking facts and it is not yet clear which Webcite workflow fits: parsing a file to Markdown, verifying a claim, checking quotes, fact-checking a whole document, checking figures, finding the original source of a figure or slide, reading saved results or the credit balance. Routes to the matching Webcite skill."
license: MIT
compatibility: Needs the Webcite MCP server (https://api.webcite.co/mcp, OAuth sign-in) or a Webcite API key for the REST API at https://api.webcite.co.
---

# Webcite: choose the workflow

Webcite is an evidence suite for AI apps and agents: it parses documents and checks claims and quotes against sources, with each citation tied to the passage it quotes. Pick the skill that matches the request:

| Request | Skill | First MCP tool |
| --- | --- | --- |
| Convert a PDF, scan, image, spreadsheet or office file to Markdown, or chunk it for RAG | `webcite-parse-documents` | `upload_url` or `upload_file`, then `extract_document` |
| Is this claim true? Add citations to a statement | `webcite-verify-claims` | `verify_claim` |
| Do these quotes appear in their sources? Show the passage behind a citation | `webcite-check-quotes` | `verify_batch`, `get_source_preview` |
| Fact-check every claim in a document, with a coverage report | `webcite-review-document` | `review_document` |
| Extract figures, find numbers that conflict, or ask a numeric question about a document | `webcite-check-figures` | `extract_figures`, `analyze_document`, `ask_document` |
| Find the original publication behind a figure, chart, screenshot or slide | `webcite-trace-source` | `extract_document`, then `search_sources` |
| Find sources on a topic without a verdict | (this skill) | `search_sources` |
| Read past verifications | (this skill) | `list_citations`, `get_citation` |
| How many credits are left? | (this skill) | `get_credit_balance` |

`webcite_guide` costs no credits and lists each workflow with its inputs, order and cost. When unsure, call it with `workflow: "choose"`.

## Files

The hosted server cannot read a path on the user's computer, and a file attached to a chat is not passed to MCP tools. Use `upload_url` for a public HTTPS link (up to 100 MB), `upload_file` with a filename and base64 content for a small file (up to 20 MB), or upload the original file in the Webcite Playground (https://webcite.co/playground) or through the REST API and reuse its `asset_id`. The local server reads files by path.

## Setup

Hosted MCP, no key in the command:

```bash
claude mcp add --transport http webcite https://api.webcite.co/mcp
codex mcp add webcite --url https://api.webcite.co/mcp && codex mcp login webcite
```

Local stdio server: `npx -y webcite-mcp-server` with `WEBCITE_API_KEY` set in its environment. SDKs: `npm install webcite` (Vercel AI SDK tools: `webcite-ai-sdk`), `pip install webcite` (LangChain: `langchain-webcite`, Haystack: `webcite-haystack`).

For current API details, read https://webcite.co/llms.txt or https://webcite.co/api-docs. Credit prices for each operation are at https://webcite.co/api-docs/credits.

## Authentication

- Every use needs a Webcite account; sign up at https://webcite.co. The free plan includes 100 credits a month.
- Hosted MCP signs in with OAuth in the client (for example `/mcp` in Claude Code). No API key is needed.
- REST calls and the local server use an API key, sent in the `x-api-key` header. Create one at https://webcite.co/api-keys (sign-in required). The SDKs and the local server read it from `WEBCITE_API_KEY`.
- Never ask the user to paste an API key into the conversation.
