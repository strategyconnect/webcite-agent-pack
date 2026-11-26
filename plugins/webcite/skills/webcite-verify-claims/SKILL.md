---
name: webcite-verify-claims
description: "Use when the user asks to fact-check, verify or source a single claim, statistic or statement, asks whether something is true, or wants citations added to generated text. Verifies the claim against web sources with Webcite and returns citations, a stance for each source and a verdict."
license: MIT
compatibility: Needs the Webcite MCP server (https://api.webcite.co/mcp, OAuth sign-in) or a Webcite API key for the REST API at https://api.webcite.co.
---

# Verify claims with Webcite

## When to use

- One factual claim or statistic to check against web sources.
- Adding citations to a statement.

For many quotes with known sources use `webcite-check-quotes`; for every claim in a document use `webcite-review-document`.

## MCP path

1. `verify_claim` with `{ "claim": "..." }`. Useful options:
   - `source_urls`: up to five public HTTPS URLs (PDFs included) to read directly, such as an official report the user named.
   - `filters`: `official_country`, `is_primary_source`, `source_type`, `domain`, `exclude_domain`, `publication_year`. Use these for an official-source requirement; wording in the claim does not replace them.
   - `decompose_claim: true` checks each part of a compound sentence separately.
   - `thread_id` groups related checks in one session.
2. `get_source_preview` with the citation URL and quote opens the passage, to show the evidence.

`verify_claim_stream` runs the same check and returns the intermediate events with the final result.

A check costs 2 credits for sources only, 3 with stances and 4 with a verdict (the default).

## Reading the result

A source match, a stance for each source (supports, contradicts or neutral) and the overall verdict are separate results. Report only a verdict the response contains, and treat a claim with no verdict as unresolved rather than supported. Report contradicting sources as well as supporting ones, match the period, metric and geography of the claim, and never invent citation URLs.

## REST path

```bash
curl -s https://api.webcite.co/api/v1/verify -H "x-api-key: YOUR_API_KEY" \
  -H "content-type: application/json" -d '{"claim": "Global EV sales passed 17 million in 2024."}'
```

Docs: https://webcite.co/api-docs/verify

## Authentication

- Every use needs a Webcite account; sign up at https://webcite.co. The free plan includes 100 credits a month.
- Hosted MCP signs in with OAuth in the client (for example `/mcp` in Claude Code). No API key is needed.
- REST calls and the local server use an API key, sent in the `x-api-key` header. Create one at https://webcite.co/api-keys (sign-in required). The SDKs and the local server read it from `WEBCITE_API_KEY`.
- Never ask the user to paste an API key into the conversation.
