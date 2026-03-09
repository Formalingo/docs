# Getting Started

The Formalingo REST API lets you create and manage forms, digital signature documents, recipients, and submissions — all programmatically from your own application.

## Base URL

All API requests are made to:

```
https://app.formalingo.com/api/v1
```

## Quick Start

**1. Generate an API key** from [Settings → API Keys](/settings/api-keys) in your dashboard.

**2. Make your first request:**

**curl**
```bash
curl https://app.formalingo.com/api/v1/forms \\
  -H "Authorization: Bearer af_live_YOUR_KEY"
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")
const forms = await client.forms.get()
```

**Python SDK**
```python
# See SDK setup guide for full client initialization
forms = await client.api.v1.forms.get()
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");
var forms = await client.Api.V1.Forms.GetAsync();
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")
forms, err := client.Api().V1().Forms().Get(context.Background(), nil)
```

**3. Create a form:**

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/forms \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{ "title": "My First Form" }'
```

**TypeScript SDK**
```typescript
const form = await client.forms.post({
  title: "My First Form",
})
```

**Python SDK**
```python
body = CreateFormBody()
body.title = "My First Form"
form = await client.api.v1.forms.post(body)
```

**.NET SDK**
```csharp
var form = await client.Api.V1.Forms.PostAsync(new() {
    Title = "My First Form",
});
```

**Go SDK**
```go
body := &models.CreateFormBody{
    Title: to.Ptr("My First Form"),
}
form, err := client.Api().V1().Forms().Post(context.Background(), body, nil)
```

## What you can do

| Resource | Description |
|---|---|
| [Forms](/docs/forms) | Create forms with sections, questions, branding, and logic |
| [Recipients](/docs/forms/recipients) | Add recipients with optional pre-filled answers |
| [Documents](/docs/documents) | Upload PDFs/DOCX and place signature fields |
| [Submissions](/docs/documents/submissions) | Create signing submissions with pre-filled data |
| [Webhooks](/docs/webhooks) | Receive real-time events when forms are submitted or documents are signed |

## Response format

All endpoints return a consistent JSON envelope:

```json
{
  "success": true,
  "data": { ... },
  "meta": { "total": 42, "page": 1, "limit": 20 }
}
```

Errors follow the same envelope with `success: false`:

```json
{
  "success": false,
  "error": "Form not found",
  "hint": "Check that the ID is correct and belongs to your workspace."
}
```

See [Error Reference](/docs/errors) for all error codes.
