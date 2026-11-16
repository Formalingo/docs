# Hosted MCP

Formalingo provides an MCP server for AI clients that support remote tools.
It exposes curated tools, resources, and prompts for forms, recipients, document signing, revisions, analytics, and quota checks.

Use MCP when you want an AI client such as Codex, Claude, Cursor, or another MCP-compatible assistant to work directly with Formalingo instead of writing raw API calls.

## Authentication

Hosted MCP supports automatic OAuth for clients that implement remote MCP OAuth.
When the client connects without credentials, Formalingo returns an OAuth discovery challenge.
The client opens a Formalingo consent screen, stores the access token, and then calls tools with:

```http
Authorization: Bearer fl_at_YOUR_OAUTH_TOKEN
```

For clients that do not support OAuth yet, the MCP server also accepts the same API-key bearer credentials as the public API:

```http
Authorization: Bearer af_live_YOUR_KEY
```

You can authenticate with either:

- Automatic OAuth from the MCP client install/login flow
- A Formalingo API key (`af_live_...`) from **Settings -> API Keys**
- A Formalingo OAuth access token (`fl_at_...`) from an OAuth connection

API keys and OAuth tokens are workspace-scoped and permission-scoped. The MCP server forwards the bearer token to `/api/v1`, so the same scopes, rate limits, and audit behavior apply.

## Endpoint

The hosted endpoint is:

```text
https://mcp.formalingo.com/mcp
```

For local development, run the MCP server on:

```text
http://localhost:3333/mcp
```

For hosted deployments, use the deployed MCP service URL configured by `MCP_SERVER_PUBLIC_URL`.

## Quick Install

Use the Formalingo installer package when your MCP client has a CLI:

```bash
npx @formalingo/mcp install codex --login
```

```bash
npx @formalingo/mcp install claude --scope user --login
```

`--login` runs the client's OAuth login command immediately after adding the MCP server. Your browser opens to Formalingo, you approve the requested scopes, and the client stores the OAuth token.

For clients without a stable install command, print manual config:

```bash
npx @formalingo/mcp print cursor
npx @formalingo/mcp print gemini
npx @formalingo/mcp print windsurf
```

## Codex OAuth Setup

The installer runs these commands for Codex:

```bash
codex mcp add formalingo \
  --url https://mcp.formalingo.com/mcp \
  --oauth-resource https://mcp.formalingo.com/mcp

codex mcp login formalingo
```

To request a smaller initial scope bundle:

```bash
codex mcp login formalingo \
  --scopes forms:read,documents:read,submissions:read
```

## Claude Code OAuth Setup

The installer runs these commands for Claude Code:

```bash
claude mcp add \
  --transport http \
  --scope user \
  formalingo \
  https://mcp.formalingo.com/mcp

claude mcp login formalingo
```

Use `--scope local` for only the current project, or `--scope project` when committing shared project MCP config.

## API-Key Fallback

Store your API key in an environment variable, not in the Codex config:

```bash
export FORMALINGO_MCP_BEARER_TOKEN=af_live_YOUR_KEY
```

Then add the MCP server:

```bash
npx @formalingo/mcp install codex \
  --bearer-token-env-var FORMALINGO_MCP_BEARER_TOKEN
```

This fallback is useful for clients that cannot complete hosted MCP OAuth yet.

For local testing:

```bash
codex mcp add formalingo-local \
  --url http://localhost:3333/mcp \
  --bearer-token-env-var FORMALINGO_MCP_BEARER_TOKEN
```

## Local Development

Start the app API:

```bash
yarn dev
```

Start the MCP server in another terminal:

```bash
cd apps/mcp-server
yarn install
FORMALINGO_API_BASE_URL=http://localhost:3000 \
MCP_SERVER_PUBLIC_URL=http://localhost:3333 \
FORMALINGO_OAUTH_ISSUER_URL=http://localhost:3000 \
PORT=3333 \
yarn dev
```

For local OAuth testing, also set the app environment variable `MCP_SERVER_PUBLIC_URL=http://localhost:3333` so `/api/oauth/authorize` accepts `http://localhost:3333/mcp` as a valid OAuth resource.

## OAuth Discovery

Automatic OAuth relies on these endpoints:

| Endpoint | Purpose |
|---|---|
| `GET https://mcp.formalingo.com/.well-known/oauth-protected-resource` | Describes the MCP resource and Formalingo authorization server |
| `GET https://formalingo.com/.well-known/oauth-authorization-server` | Describes OAuth authorize, token, register, revoke, PKCE, scopes, and resource-indicator support |
| `GET /api/oauth/authorize` | Starts login and consent |
| `POST /api/oauth/token` | Exchanges authorization codes and refresh tokens |
| `POST /api/oauth/register` | Dynamic client registration for public MCP clients |
| `POST /api/oauth/revoke` | Revokes stored OAuth tokens |

When the MCP client connects without credentials, Formalingo responds:

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer resource_metadata="https://mcp.formalingo.com/.well-known/oauth-protected-resource"
```

## Available Tools

The MCP server focuses on deterministic Formalingo operations:

| Surface | Examples |
|---|---|
| Forms | List, create, update, publish, configure branding and settings |
| Questions and sections | Create, update, delete, reorder |
| Recipients | Create links, bulk-create recipients, read status and responses |
| Documents | Upload PDFs, update documents, manage fields and signer roles |
| AI document parsing | Start async parse jobs, check status, apply parsed fields |
| Submissions | Create signing packets, update signers, read signed PDF links |
| Readback | Analytics summary, delivery logs, revisions, quota status |

## Security Notes

- Keep API keys out of prompts, browser code, and checked-in config.
- Grant only the scopes the AI client needs.
- Revoke API keys immediately when rotating clients or removing access.
- Use OAuth where the MCP client supports a user-consent flow.

See [Authentication](/docs/authentication) for API key scopes and [AI Skills](/docs/ai-skills) for the downloadable non-MCP skill file.
