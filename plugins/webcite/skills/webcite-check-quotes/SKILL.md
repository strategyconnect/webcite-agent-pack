---
name: webcite-check-quotes
description: "Use when the user wants to confirm that quotes or cited passages appear in the sources they cite, check every citation in a draft, show the exact passage behind a citation, or look up past verification results. Checks up to 200 quote and source pairs at once with Webcite."
license: MIT
compatibility: Needs the Webcite MCP server (https://api.webcite.co/mcp, OAuth sign-in) or a Webcite API key for the REST API at https://api.webcite.co.
---

# Check quotes with Webcite

## When to use

- A list of quotes, each with its source as inline text, a URL or an uploaded file.
- Showing the passage a citation points to.
- Reading back earlier verification results.

## MCP path

- `verify_batch` with `items` (1 to 200). Each item has a `quote` and one source: `source_text`, `url` or `asset_id` (with an optional `page`), plus your own `id` to map results back. Each result reports whether the quote is grounded, how it matched (exact, normalized, fuzzy or unbound) and the closest passage. A fuzzy match is capped at needs review, never verified. 1 credit per item.
- `get_source_preview` with a `url` or `asset_id` and the `quote` opens the passage: a text-fragment link for a web page, or the cited page and an `#page=N` link for a document. 1 credit.
- `verify_feedback` records a reviewer's verdict on a batch result, using its `feedback_token`.
- `list_citations` and `get_citation` read saved verification results without re-running them.

For quotes in scanned documents where OCR broke the text, see https://webcite.co/api-docs/quotes-in-scans.

## REST path

```bash
curl -s https://api.webcite.co/api/v1/verify/batch -H "x-api-key: YOUR_API_KEY" \
  -H "content-type: application/json" \
  -d '{"items": [{"id": "revenue-2025", "quote": "Revenue was $12 million.", "source_text": "The annual report states: Revenue was $12 million."}]}'
```

Docs: https://webcite.co/api-docs/batch

## Authentication

- Every use needs a Webcite account; sign up at https://webcite.co. The free plan includes 100 credits a month.
- Hosted MCP signs in with OAuth in the client (for example `/mcp` in Claude Code). No API key is needed.
- REST calls and the local server use an API key, sent in the `x-api-key` header. Create one at https://webcite.co/api-keys (sign-in required). The SDKs and the local server read it from `WEBCITE_API_KEY`.
- Never ask the user to paste an API key into the conversation.
