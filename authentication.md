# Authentication

## API Keys

The Formalingo API uses API keys for authentication. Every request must include your key in the `Authorization` header using the `Bearer` scheme.

```
Authorization: Bearer af_live_YOUR_KEY
```

API keys are **workspace-scoped** — they can only access resources belonging to the workspace where the key was created.

## MCP Authentication

Hosted MCP supports automatic OAuth install/login for clients that implement remote MCP OAuth:

```bash
npx @formalingo/mcp install codex --login
npx @formalingo/mcp install claude --scope user --login
```

The MCP client discovers Formalingo OAuth from the hosted MCP endpoint, opens a consent screen, and stores a scoped OAuth access token.

MCP can also use the same bearer credentials as the REST API when a client needs manual configuration:

```http
Authorization: Bearer af_live_YOUR_KEY
```

The MCP server forwards API keys and OAuth tokens to `/api/v1`, so scopes, rate limits, and workspace access are enforced exactly like direct API calls. See [Hosted MCP](/docs/mcp) for OAuth setup and client-specific install commands.

## Creating an API Key

1. Go to **Settings → API Keys** in your dashboard
2. Click **Create API Key**
3. Give it a name and select the required permissions
4. Copy the key — **it is only shown once**

## Permissions

Each API key has a set of granted permissions. Attempting an action without the required permission returns a `403 Forbidden` response.

| Permission | Description |
|---|---|
| `forms:read` | Access forms, sections, questions, rules, settings, and branding |
| `forms:write` | Create and modify form drafts |
| `forms:publish` | Publish form drafts |
| `forms:delete` | Delete forms in your workspace |
| `recipients:read` | Access recipient status and responses |
| `recipients:write` | Create and update recipient links and prefill values |
| `recipients:bulk` | Create recipient links in batches |
| `recipients:send_notifications` | Trigger Formalingo recipient notifications |
| `documents:read` | Access document drafts, fields, signer roles, and summaries |
| `documents:write` | Upload and modify documents, fields, and signer roles |
| `documents:parse_ai` | Use Formalingo AI to detect document fields and signer roles |
| `documents:publish` | Publish document drafts |
| `documents:delete` | Delete documents in your workspace |
| `submissions:read` | Access submission, signer, and signing status data |
| `submissions:write` | Create and update document submissions and signers |
| `submissions:send_notifications` | Trigger Formalingo signer notifications |
| `submissions:download_pdf` | Get signed PDF file descriptors and temporary download links |
| `submissions:download_files` | Get temporary download links for uploaded response attachments |
| `analytics:read` | Read completion counts and recent activity |
| `integrations:read` | Read integration profile and status summaries |
| `deliveries:read` | Read integration and notification delivery logs |
| `revisions:read` | Read revision history and snapshots |

Legacy scope names such as `read:submissions` are accepted for backward compatibility and are normalized to the matching catalog permission.

## Security Best Practices

- **Never expose API keys client-side** (browser, mobile app). Use them server-side only.
- **Use minimal scopes** — only grant the scopes your integration needs.
- **Set expiry dates** on keys used for automations.
- **Rotate keys regularly** and revoke any that may have been compromised.

## Feature Restrictions

Some dashboard features are not available via API key:

- AI form/document generation
- Workspace and team management
- Integration (webhook profile) management

These require a logged-in session.
