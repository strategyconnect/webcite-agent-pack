# Webcite agent pack

Skills and plugin manifests that connect AI agents to [Webcite](https://webcite.co), an evidence suite for AI apps and agents. Webcite parses documents to Markdown with page and sheet locations, and checks claims and quotes against sources, with each citation tied to the passage it quotes.

The pack connects the hosted Webcite MCP server at `https://api.webcite.co/mcp`, which signs in with OAuth, so no API key is needed. Every use needs a Webcite account; sign up at [webcite.co](https://webcite.co). The free plan includes 100 credits a month.

## Skills

| Skill | Use it to |
| --- | --- |
| `webcite` | Choose the right Webcite workflow for a request |
| `webcite-parse-documents` | Convert PDFs, scans, images, spreadsheets and office files to Markdown, or chunk them for RAG |
| `webcite-verify-claims` | Check a claim against web sources, with citations, a stance for each source and a verdict |
| `webcite-check-quotes` | Check that quotes appear in the sources they cite, show the passage, and read saved results |
| `webcite-review-document` | Fact-check every claim in a document, with a coverage report |
| `webcite-check-figures` | Extract financial figures, find numbers that conflict, and answer numeric questions from a document |
| `webcite-trace-source` | Find the original publication behind a figure, chart, screenshot or slide |

## Install

| Agent | Command |
| --- | --- |
| Any agent supported by the `skills` CLI | `npx skills add strategyconnect/webcite-agent-pack` |
| Claude Code | `claude plugin marketplace add strategyconnect/webcite-agent-pack`, then `claude plugin install webcite@webcite` |
| Codex | `codex plugin marketplace add strategyconnect/webcite-agent-pack`, then install `webcite` from the Plugins Directory |
| Gemini CLI | `gemini extensions install https://github.com/strategyconnect/webcite-agent-pack` |

Without the pack, connect the MCP server directly: `claude mcp add --transport http webcite https://api.webcite.co/mcp`. Other clients are listed at [webcite.co/api-docs/mcp](https://webcite.co/api-docs/mcp). To read files by path on your machine, run the local server instead: `npx -y webcite-mcp-server` with `WEBCITE_API_KEY` set to a key from [webcite.co/api-keys](https://webcite.co/api-keys).

SDKs: `npm install webcite` (Vercel AI SDK tools: `webcite-ai-sdk`) and `pip install webcite` (LangChain: `langchain-webcite`, Haystack: `webcite-haystack`).

## Layout

- `plugins/webcite/`: the plugin (Claude Code `.claude-plugin/plugin.json` and `.mcp.json`; portable `plugin.json` and `mcp.json` for Codex and Cursor; Cursor `.cursor-plugin/plugin.json`; `skills/`).
- `.claude-plugin/marketplace.json` and `.agents/plugins/marketplace.json`: marketplaces for Claude Code and Codex.
- `gemini-extension.json` and `GEMINI.md`: the Gemini CLI extension.
- `context7.json`: Context7 indexing settings.

## License

MIT. See [LICENSE](LICENSE).
