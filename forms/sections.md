# Sections

Sections are optional groupings of questions within a form. Use them to divide long forms into logical pages or chapters.

### `GET` `/api/v1/forms/{id}/sections`

 — Scopes: `read:forms`

### `POST` `/api/v1/forms/{id}/sections`

Create a section — Scopes: `write:forms`

### `PUT` `/api/v1/forms/{id}/sections/{sid}`

Rename or reorder a section — Scopes: `write:forms`

### `DELETE` `/api/v1/forms/{id}/sections/{sid}`

Delete a section (questions remain, unassigned) — Scopes: `write:forms`

## Create a section

**Request Body**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `order` | `integer` | No | Position (0-indexed). Auto-appended if omitted. |

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/sections \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{ "title": "Personal Information" }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const section = await client.forms.byFormId("FORM_ID").sections.post({
  title: "Personal Information",
})
```

**Python SDK**
```python
body = CreateSectionBody()
body.title = "Personal Information"

section = await client.api.v1.forms.by_form_id("FORM_ID").sections.post(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var section = await client.Api.V1.Forms["FORM_ID"].Sections.PostAsync(new() {
    Title = "Personal Information",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.CreateSectionBody{
    Title: to.Ptr("Personal Information"),
}
section, err := client.Api().V1().Forms().ByFormId("FORM_ID").Sections().Post(context.Background(), body, nil)
```

## Assign questions to a section

When creating a question, pass the `section_id` field:

```json
{
  "question_text": "What is your name?",
  "type": "short_text",
  "section_id": "SECTION_UUID"
}
```
