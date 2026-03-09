# Create a Form

### `POST` `/api/v1/forms`

 — Scopes: `write:forms`

**Request Body**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `description` | `string` | No | Optional form description (max 1000 chars) |

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/forms \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "title": "Customer Satisfaction Survey",
    "description": "Help us improve our service."
  }'
```

**JavaScript**
```javascript
const res = await fetch('https://app.formalingo.com/api/v1/forms', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer af_live_YOUR_KEY',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    title: 'Customer Satisfaction Survey',
    description: 'Help us improve our service.',
  }),
})
const { data: form } = await res.json()
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const form = await client.forms.post({
  title: "Customer Satisfaction Survey",
  description: "Help us improve our service.",
})
```

**Python SDK**
```python
from formalingo.models.create_form_body import CreateFormBody

# See SDK setup guide for client initialization
body = CreateFormBody()
body.title = "Customer Satisfaction Survey"
body.description = "Help us improve our service."
form = await client.api.v1.forms.post(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var form = await client.Api.V1.Forms.PostAsync(new() {
    Title = "Customer Satisfaction Survey",
    Description = "Help us improve our service.",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.CreateFormBody{
    Title:       to.Ptr("Customer Satisfaction Survey"),
    Description: to.Ptr("Help us improve our service."),
}
form, err := client.Api().V1().Forms().Post(context.Background(), body, nil)
```

**Response** (201)
```json
{
  "success": true,
  "data": {
    "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "creatorId": "user-uuid",
    "workspaceId": "workspace-uuid",
    "title": "Customer Satisfaction Survey",
    "description": "Help us improve our service.",
    "status": "draft",
    "branding": {},
    "settings": {},
    "publicToken": null,
    "createdAt": "2026-02-25T10:00:00.000Z",
    "updatedAt": "2026-02-25T10:00:00.000Z"
  }
}
```

## Next steps

After creating a form, you'll typically:

1. [Add sections](/docs/forms/sections) (optional)
2. [Add questions](/docs/forms/questions)
3. [Customize branding](/docs/forms/branding) (optional)
4. [Publish the form](/docs/forms/create#publishing) — `PUT /api/v1/forms/{id}` with `{ "status": "published" }`
5. [Add recipients](/docs/forms/recipients)

## Publishing

Update the form status to `published` when ready:

```bash
curl -X PUT https://app.formalingo.com/api/v1/forms/FORM_ID \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{ "status": "published" }'
```

Status transitions: `draft → published → archived`. You cannot go backwards.
