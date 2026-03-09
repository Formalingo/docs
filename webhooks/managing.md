# Managing Webhooks

## Dashboard setup

Webhooks are configured in the **Integrations** section of the Formalingo dashboard:

1. Go to **Settings → Integrations**
2. Click **Add Integration**
3. Choose **Webhook**
4. Enter your endpoint URL
5. Select the events you want to receive
6. Bind the integration to specific forms or documents (or globally)

## Testing your endpoint

Your endpoint must return a `2xx` HTTP status code to acknowledge receipt. Formalingo logs delivery results for debugging.

## Verifying webhook authenticity

Currently, webhook payloads don't include a signature header. We recommend:

- Using a secret URL path or query parameter token (e.g. `/webhooks/formalingo?token=YOUR_SECRET`)
- Validating the `workspaceId` in the payload matches your expected workspace

A signature scheme (`X-Formalingo-Signature`) is planned for a future release.

## Retry policy

If your endpoint returns a non-2xx response or times out, Formalingo logs the delivery failure. Manual retries can be triggered from the dashboard.

## Delivery logs

View delivery history at **Settings → Integrations → [Integration] → Deliveries** in the dashboard.
