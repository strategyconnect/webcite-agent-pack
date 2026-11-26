# Webcite agent pack

Instructions for coding agents working with this repository or using Webcite.

- Webcite is an evidence suite for AI apps and agents: it parses documents to Markdown and checks claims and quotes against sources. Use it through the hosted MCP server (`https://api.webcite.co/mcp`, OAuth sign-in), the local server (`npx -y webcite-mcp-server`), the SDKs (`webcite` on npm and PyPI) or the REST API (`https://api.webcite.co/api/v1/*`, `x-api-key` header). Every use needs an account; the free plan includes 100 credits a month.
- The skills in `plugins/webcite/skills/` describe each job: `webcite` (choose a workflow), `webcite-parse-documents`, `webcite-verify-claims`, `webcite-check-quotes`, `webcite-review-document`, `webcite-check-figures`, `webcite-trace-source`.
- Skills must name only tools in the Webcite MCP server's default (public) profile. The list is in `tools/list` of `webcite-mcp-server` and in https://webcite.co/api-docs/mcp.
- Never ask the user to paste an API key into the conversation. Keys are created at https://webcite.co/api-keys; the SDKs and the local server read them from `WEBCITE_API_KEY`. Examples in this repository use the `YOUR_API_KEY` placeholder.
- Every statement in a skill must be true of the current server; when the server changes, update the skill in the same release.
