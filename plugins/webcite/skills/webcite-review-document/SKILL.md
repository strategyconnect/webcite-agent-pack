---
name: webcite-review-document
description: "Use when the user asks to audit, fact-check or check every claim or figure in a whole document, report, memo, deck, slide or AI-generated draft, or wants a coverage report of what was checked. Runs a Webcite document review job over an uploaded file and reads the saved results."
license: MIT
compatibility: Needs the Webcite MCP server (https://api.webcite.co/mcp, OAuth sign-in) or a Webcite API key for the REST API at https://api.webcite.co.
---

# Fact-check a whole document with Webcite

## When to use

Every material claim in an uploaded document needs checking, with a record of coverage. A few single-claim checks are not a substitute for a full review.

## MCP path

1. Get an `asset_id` for the original file. Reuse one the user already has. Otherwise use `upload_url` for a public HTTPS link or `upload_file` for a small file; a chat attachment must first be uploaded in the Webcite Playground (https://webcite.co/playground) or through the REST API. No separate extraction or balance check is needed before the review.
2. `review_document` with `prompt`, `asset_ids` (one or more) and a stable `thread_id`. Pass official reports the claims must be checked against in `source_urls`, and `filters` for source requirements. Optional `max_account_credits` caps new charges for this run. It returns a `job_id` at once; repeating the same input reuses the job without a new charge.
3. `get_document_review_job` with the `job_id` (0 credits). Obey `poll_after_ms` and wait at most 30 seconds between polls. `queued`, `running`, `waiting_parse` and `waiting_review` are still in progress. Stop as soon as the status is terminal: `complete`, `partial_coverage`, `credits_exhausted` or `failed`.
4. `get_document_review` with the `review_id` (0 credits) pages through the saved claims, verdicts, citations, uncovered source passages and rejected candidates. `get_review_source_span` reads the exact text of one uncovered passage.

Each checked claim is charged like a single verification: up to 4 credits with stance and verdict on.

## Reporting

- Claim full coverage only when the saved review reports `coverage_complete: true` and no pending claims. A `complete` job status means processing finished; on its own it does not prove full coverage.
- Report pending, failed, rejected and uncovered counts separately. Failed claims are part of pending claims, so do not add them twice. Treat a null or missing count as unknown.
- After `credits_exhausted` or `failed`, give the user the checked claims, the unchecked claims and the uncovered passages straight away. To resume, call `review_document` again with the same prompt, thread, assets, sources and filters plus `retry_failed: true` and the returned `retry_idempotency_key`. Completed claims are kept.

Docs: https://webcite.co/api-docs/mcp

## Authentication

- Every use needs a Webcite account; sign up at https://webcite.co. The free plan includes 100 credits a month.
- Hosted MCP signs in with OAuth in the client (for example `/mcp` in Claude Code). No API key is needed.
- REST calls and the local server use an API key, sent in the `x-api-key` header. Create one at https://webcite.co/api-keys (sign-in required). The SDKs and the local server read it from `WEBCITE_API_KEY`.
- Never ask the user to paste an API key into the conversation.
