# Webcite

Webcite is an evidence suite for AI agents: it reads documents and checks claims and quotes against sources. This extension connects the hosted Webcite MCP server at https://api.webcite.co/mcp. Run `/mcp auth webcite` to sign in with your Webcite account (sign up at https://webcite.co; the free plan includes 100 credits a month).

Which tool for which request:

- Convert a PDF, scan, image, spreadsheet or office file to Markdown: `upload_url` (public HTTPS link) or `upload_file` (filename and base64 content, up to 20 MB), then `extract_document`. `extract_pages` returns chunks with page anchors for RAG. The hosted server cannot read a local path.
- Check one claim against web sources, with citations, a stance for each source and a verdict: `verify_claim`.
- Check that quotes appear in their sources: `verify_batch`; show the passage behind a citation with `get_source_preview`.
- Fact-check every claim in a document: `review_document` with `asset_ids` and a stable `thread_id`, then `get_document_review_job` (obey `poll_after_ms`, wait at most 30 seconds between polls) until a terminal status, then `get_document_review`. Claim full coverage only when `coverage_complete` is true and no claims are pending.
- Extract and cross-check figures: `analyze_document`, or `extract_figures` then `analyze_conflicts`. Answer a numeric question from document text: `ask_document`, then `get_ask_result`.
- Find the original source of a figure or slide: `extract_document`, `search_sources` with the exact values, then `get_source_preview` on candidates.
- Find sources without a verdict: `search_sources`. Read past results: `list_citations`, `get_citation`. Credit balance: `get_credit_balance`.

`webcite_guide` lists each workflow with its inputs, order and cost. Never ask the user to paste an API key into the conversation. The skills in `plugins/webcite/skills` give each workflow in more detail.
