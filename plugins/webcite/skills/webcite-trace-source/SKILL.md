---
name: webcite-trace-source
description: "Use when the user wants to find the original publication behind a figure, table, chart, screenshot or slide: who first published the numbers, on which page, and whether the document quotes them correctly. Traces the source with Webcite by extracting the exact values and searching for them."
license: MIT
compatibility: Needs the Webcite MCP server (https://api.webcite.co/mcp, OAuth sign-in) or a Webcite API key for the REST API at https://api.webcite.co.
---

# Find the original source with Webcite

## When to use

A slide, screenshot, chart or table shows figures without a reliable citation, and the user wants the original publisher and page.

## MCP path

1. Get an `asset_id` for the image or file (`upload_url` or `upload_file`) and call `extract_document`. Note the exact table labels, units, years, geography and distinctive values. If part of the image is unreadable, say which part.
2. `search_sources` with those exact details in `query`. Use `filters` (`official_country`, `is_primary_source`, `source_type`, `domain`, `publication_year`) when a regulator or official body is the likely publisher. If results are secondary or empty, refine the query with a likely report title or publisher. A filtered search that returns nothing does not prove the source does not exist.
3. Open each candidate with `get_source_preview`, passing the `url` and a distinctive value as `quote`. Accept a page as the original only when the same metric, period, unit, geography and values appear there.
4. To check a claim once the source is confirmed, call `verify_claim` with that URL in `source_urls`.

If the documents and checked pages do not establish the publisher, report the search as unresolved, list the candidates you checked, and suggest a next search. Do not infer a country from a currency symbol alone, and do not cite a news summary as the original publication.

`ask_document` answers numeric questions from text you supply; it does not search for outside sources.

## REST path

```bash
curl -s https://api.webcite.co/api/v1/sources/search -H "x-api-key: YOUR_API_KEY" \
  -H "content-type: application/json" -d '{"query": "electricity generation 2023 TWh renewables share 30.3%"}'
```

Docs: https://webcite.co/api-docs/search

## Authentication

- Every use needs a Webcite account; sign up at https://webcite.co. The free plan includes 100 credits a month.
- Hosted MCP signs in with OAuth in the client (for example `/mcp` in Claude Code). No API key is needed.
- REST calls and the local server use an API key, sent in the `x-api-key` header. Create one at https://webcite.co/api-keys (sign-in required). The SDKs and the local server read it from `WEBCITE_API_KEY`.
- Never ask the user to paste an API key into the conversation.
