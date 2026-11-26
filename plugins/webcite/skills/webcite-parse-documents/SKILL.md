---
name: webcite-parse-documents
description: "Use when the user wants a PDF, scanned page, image, spreadsheet, Word or PowerPoint file converted to Markdown or text, OCR on a scan, tables pulled out, or a document split into citation-ready chunks for RAG. Parses documents with Webcite page by page, keeping the page or sheet each part came from."
license: MIT
compatibility: Needs the Webcite MCP server (https://api.webcite.co/mcp, OAuth sign-in) or a Webcite API key for the REST API at https://api.webcite.co.
---

# Parse documents with Webcite

## When to use

- Converting a PDF, scan, JPEG/PNG/WebP image, spreadsheet (XLSX, XLS, ODS, CSV), DOCX, PPTX, HTML or text file to Markdown.
- Splitting a document into chunks with page, sheet or section anchors for indexing.

To check whether the content is true, use `webcite-review-document` or `webcite-verify-claims`.

## MCP path

1. Get an `asset_id`. Use `upload_url` for a public HTTPS link (up to 100 MB). On the hosted server, `upload_file` takes a filename and base64 content (up to 20 MB); the local server takes a file path. A chat attachment is not passed to MCP tools: upload the original file in the Webcite Playground (https://webcite.co/playground) or through the REST API, then use its `asset_id`. Uploading is free.
2. `extract_document` with the `asset_id` returns whole-document Markdown and per-page or per-sheet units, each carrying its page or sheet. Scanned pages and images go through OCR.
3. `extract_pages` returns citation-ready chunks with anchors.

Extraction is metered per page (see https://webcite.co/api-docs/credits); spreadsheets, DOCX, PPTX, HTML and text files cost 0 credits because they are read from the file itself. Keep the returned structured content and reuse it, since repeating an extraction is charged again. Check each unit's read state, and tell the user which pages could not be read.

## REST path

```bash
curl -s https://api.webcite.co/api/v1/upload -H "x-api-key: YOUR_API_KEY" -F file=@report.pdf
curl -s https://api.webcite.co/api/v1/extract -H "x-api-key: YOUR_API_KEY" \
  -H "content-type: application/json" -d '{"asset_id": "<asset_id from upload>"}'
```

Docs: https://webcite.co/api-docs/extraction

## Authentication

- Every use needs a Webcite account; sign up at https://webcite.co. The free plan includes 100 credits a month.
- Hosted MCP signs in with OAuth in the client (for example `/mcp` in Claude Code). No API key is needed.
- REST calls and the local server use an API key, sent in the `x-api-key` header. Create one at https://webcite.co/api-keys (sign-in required). The SDKs and the local server read it from `WEBCITE_API_KEY`.
- Never ask the user to paste an API key into the conversation.
