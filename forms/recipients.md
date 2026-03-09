# Recipients

A recipient is a person invited to fill out a form. Each recipient gets a unique link and optionally pre-filled answers.

### `GET` `/api/v1/forms/{id}/recipients`

 — Scopes: `read:recipients`

### `POST` `/api/v1/forms/{id}/recipients`

Create a recipient with optional pre-fill — Scopes: `write:recipients`

### `PUT` `/api/v1/forms/{id}/recipients/{rid}`

Update a recipient — Scopes: `write:recipients`

### `DELETE` `/api/v1/forms/{id}/recipients/{rid}`

Delete a recipient — Scopes: `delete:recipients`

## Create a recipient

**Request Body**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `email` | `email` | No | Triggers invitation email if a mail integration is configured |
| `phone` | `string` | No | Phone number (for SMS notifications if configured) |
| `password` | `string` | No | Password-protect the form link (min 4 chars) |
| `expires_at` | `datetime` | No | ISO 8601 expiry — link becomes inactive after this time |
| `prefill` | `object` | No | Map of questionId → value. Saves responses immediately. |

## Pre-filling answers

The `prefill` field accepts a map of question IDs to values. The API saves these as `Response` records immediately — the recipient will see the answers pre-populated when they open the link.

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/recipients \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "label": "John Doe",
    "email": "john@acme.com",
    "expires_at": "2026-03-31T23:59:59Z",
    "prefill": {
      "question-uuid-1": "John Doe",
      "question-uuid-2": "Acme Corp",
      "question-uuid-3": "john@acme.com"
    }
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const recipient = await client.forms.byFormId("FORM_ID").recipients.post({
  label: "John Doe",
  email: "john@acme.com",
  expiresAt: "2026-03-31T23:59:59Z",
  prefill: {
    "question-uuid-1": "John Doe",
    "question-uuid-2": "Acme Corp",
  },
})
console.log(recipient.link)
```

**Python SDK**
```python
# See SDK setup guide for client initialization

body = CreateRecipientBody()
body.label = "John Doe"
body.email = "john@acme.com"
body.prefill = {
    "question-uuid-1": "John Doe",
    "question-uuid-2": "Acme Corp",
}

recipient = await client.api.v1.forms.by_form_id("FORM_ID").recipients.post(body)
print(recipient.link)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var recipient = await client.Api.V1.Forms["FORM_ID"].Recipients.PostAsync(new() {
    Label = "John Doe",
    Email = "john@acme.com",
    ExpiresAt = "2026-03-31T23:59:59Z",
    Prefill = new() {
        ["question-uuid-1"] = "John Doe",
        ["question-uuid-2"] = "Acme Corp",
    },
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.CreateRecipientBody{
    Label: to.Ptr("John Doe"),
    Email: to.Ptr("john@acme.com"),
    ExpiresAt: to.Ptr("2026-03-31T23:59:59Z"),
    Prefill: map[string]string{
        "question-uuid-1": "John Doe",
        "question-uuid-2": "Acme Corp",
    },
}
recipient, err := client.Api().V1().Forms().ByFormId("FORM_ID").Recipients().Post(context.Background(), body, nil)
```

**Response** (201)
```json
{
  "success": true,
  "data": {
    "id": "recipient-uuid",
    "form_id": "form-uuid",
    "label": "John Doe",
    "email": "john@acme.com",
    "token": "abc123",
    "status": "not_started",
    "is_active": true,
    "is_anonymous": false,
    "has_password": false,
    "link": "https://www.formalingo.com/f/abc123",
    "response_count": 3,
    "visit_count": 0,
    "total_questions": 5,
    "created_at": "2026-02-25T10:00:00.000Z"
  }
}
```

## The recipient link

The `link` field in the response is the URL the recipient uses to fill out the form. Share this via email, embed it in your application, or use it for redirects.
