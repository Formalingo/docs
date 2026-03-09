# Webhooks

Webhooks let your server receive real-time notifications when events occur — form submissions, document signings, invitations, and more.

## How it works

1. Create an **Integration Profile** in the dashboard (Settings → Integrations) pointing to your webhook URL
2. Bind it to a specific form or document
3. Select which events to receive

When an event fires, Formalingo sends an HTTP POST to your endpoint with a JSON payload.

## Event types

| Event | Description |
|---|---|
| `form_submitted` | A recipient completed a form |
| `form_opened` | A recipient opened their form link |
| `form_expired` | A recipient's link expired |
| `recipient_invite` | A new recipient was added |
| `recipient_reminder` | A reminder was sent to a recipient |
| `document_sent` | A document submission was created |
| `document_signer_viewed` | A signer opened their signing link |
| `document_signer_completed` | A signer completed all fields |
| `document_all_complete` | All signers on a submission completed |
| `signer_invite` | A new signer was added to a submission |
| `signer_reminder` | A reminder was sent to a signer |

## Webhook payload structure

All events share a common envelope:

```json
{
  "event": "form_submitted",
  "timestamp": "2026-02-25T10:00:00.000Z",
  "workspaceId": "workspace-uuid",
  "data": { ... }
}
```

See [Event Payloads](/docs/webhooks/events) for the `data` shape of each event.

## Managing webhooks

Webhooks are currently managed through the **Integrations** section of the dashboard. Programmatic management of integration profiles is not available via API key — use the dashboard to configure your webhook endpoints.
