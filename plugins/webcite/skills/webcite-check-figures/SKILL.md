---
name: webcite-check-figures
description: "Use when the user wants financial figures extracted from a spreadsheet, financial model, PDF, deck or image with their units and periods, totals recomputed, numbers that conflict found, or a numeric question answered from a document's text. Reads and cross-checks figures with Webcite."
license: MIT
compatibility: Needs the Webcite MCP server (https://api.webcite.co/mcp, OAuth sign-in) or a Webcite API key for the REST API at https://api.webcite.co.
---

# Check figures with Webcite

## When to use

- Reading financial figures with units, periods and source locations (sheet, cell, page).
- Recomputing totals and finding figures that conflict within or across documents.
- Answering a numeric question from document text, with each number checked against that text.

To check the figures in a document against outside sources, use `webcite-review-document`. To find where a figure was first published, use `webcite-trace-source`.

## MCP path

1. Get an `asset_id` (`upload_url` or `upload_file`) and read the file with `extract_document`.
2. Pick the tool:
   - `analyze_document` (3 credits) extracts, recomputes and cross-checks the figures in a PDF, spreadsheet or JPEG/PNG/WebP image in one call.
   - `extract_figures` (2 credits) returns recognized financial metrics with unit, period, entity and provenance (asset, sheet, cell, page). It lists recognized metrics only, so it is not an inventory of every number in the file.
   - `analyze_conflicts` (1 credit) recomputes totals and flags conflicts across figures you supply, in the `extract_figures` format or your own.
   - `ask_document` (5 credits) answers one numeric question from document text you pass in `documentText`. It returns a job ID; read the answer with `get_ask_result` (0 credits). Numbers it could not check stay null.
3. Figures read from images are OCR or model reads; ask the user to review them against the source.

These tools check the numbers inside the documents. They do not decide whether a figure is true in the outside world.

## REST path

```bash
curl -s https://api.webcite.co/api/v1/analyze/document -H "x-api-key: YOUR_API_KEY" \
  -H "content-type: application/json" -d '{"asset_id": "<asset_id from upload>"}'
```

`POST /api/v1/extract/figures` with `{"asset_id": "..."}` and `POST /api/v1/analyze/conflicts` with `{"figures": [...]}` take the same header.

Docs: https://webcite.co/api-docs/figures and https://webcite.co/api-docs/analyze-document

## Authentication

- Every use needs a Webcite account; sign up at https://webcite.co. The free plan includes 100 credits a month.
- Hosted MCP signs in with OAuth in the client (for example `/mcp` in Claude Code). No API key is needed.
- REST calls and the local server use an API key, sent in the `x-api-key` header. Create one at https://webcite.co/api-keys (sign-in required). The SDKs and the local server read it from `WEBCITE_API_KEY`.
- Never ask the user to paste an API key into the conversation.
