# Authentication

## API Keys

The Formalingo API uses API keys for authentication. Every request must include your key in the `Authorization` header using the `Bearer` scheme.

```
Authorization: Bearer af_live_YOUR_KEY
```

API keys are **workspace-scoped** — they can only access resources belonging to the workspace where the key was created.

## Creating an API Key

1. Go to **Settings → API Keys** in your dashboard
2. Click **Create API Key**
3. Give it a name and select the required scopes
4. Copy the key — **it is only shown once**

## Scopes

Each API key has a set of granted scopes. Attempting an action without the required scope returns a `403 Forbidden` response.

| Scope | Description |
|---|---|
| `read:forms` | List and get forms, sections, questions |
| `write:forms` | Create and update forms, sections, questions, branding, settings |
| `delete:forms` | Soft-delete or permanently delete forms |
| `read:recipients` | List recipients |
| `write:recipients` | Create and update recipients (including prefill) |
| `delete:recipients` | Delete recipients |
| `read:documents` | List and get documents, fields, signer roles |
| `write:documents` | Create and update documents, fields, signer roles, submissions |
| `delete:documents` | Soft-delete or permanently delete documents |
| `read:submissions` | List submissions and signers |
| `write:submissions` | Create submissions with signers and prefill; update/delete signers |
| `webhooks` | Manage webhook subscriptions |

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
