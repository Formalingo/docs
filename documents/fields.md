# Document Fields

Fields are positioned elements on document pages — signature boxes, text inputs, date pickers, etc. Coordinates are normalized (0–1) relative to the page dimensions.

### `GET` `/api/v1/documents/{id}/fields`

 — Scopes: `read:documents`

### `POST` `/api/v1/documents/{id}/fields`

Place a field — Scopes: `write:documents`

### `PUT` `/api/v1/documents/{id}/fields/{fid}`

Update a field — Scopes: `write:documents`

### `DELETE` `/api/v1/documents/{id}/fields/{fid}`

Remove a field — Scopes: `write:documents`

## Field types

Same types as form questions: `digital_signature`, `initials`, `short_text`, `date`, `email`, `phone`, `number`, `yes_no`, `mcq`, `dropdown`.

## Place a field

**Required Fields**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `pageNumber` | `integer` | Yes | 1-indexed page number |
| `x` | `float` | Yes | Normalized x position (0–1 from left) |
| `y` | `float` | Yes | Normalized y position (0–1 from top) |
| `width` | `float` | Yes | Normalized width (0–1) |
| `height` | `float` | Yes | Normalized height (0–1) |

**Optional Fields**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `defaultValue` | `string` | No | Pre-populated value. Use __today__ for current date. |
| `direction` | `auto | rtl | ltr` | No | Text direction for RTL languages |

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents/DOC_ID/fields \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "label": "Client Signature",
    "type": "digital_signature",
    "pageNumber": 3,
    "x": 0.1,
    "y": 0.85,
    "width": 0.35,
    "height": 0.06,
    "isRequired": true,
    "assigneeRole": "signer_1"
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const field = await client.documents.byDocumentId("DOC_ID").fields.post({
  label: "Client Signature",
  type: "digital_signature",
  pageNumber: 3,
  x: 0.1,
  y: 0.85,
  width: 0.35,
  height: 0.06,
  isRequired: true,
  assigneeRole: "signer_1",
})
```

**Python SDK**
```python
body = CreateFieldBody()
body.label = "Client Signature"
body.type = "digital_signature"
body.page_number = 3
body.x = 0.1
body.y = 0.85
body.width = 0.35
body.height = 0.06
body.is_required = True
body.assignee_role = "signer_1"

field = await client.api.v1.documents.by_document_id("DOC_ID").fields.post(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var field = await client.Api.V1.Documents["DOC_ID"].Fields.PostAsync(new() {
    Label = "Client Signature",
    Type = "digital_signature",
    PageNumber = 3,
    X = 0.1f,
    Y = 0.85f,
    Width = 0.35f,
    Height = 0.06f,
    IsRequired = true,
    AssigneeRole = "signer_1",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.CreateFieldBody{
    Label:        to.Ptr("Client Signature"),
    Type:         to.Ptr("digital_signature"),
    PageNumber:   to.Ptr(int32(3)),
    X:            to.Ptr(float64(0.1)),
    Y:            to.Ptr(float64(0.85)),
    Width:        to.Ptr(float64(0.35)),
    Height:       to.Ptr(float64(0.06)),
    IsRequired:   to.Ptr(true),
    AssigneeRole: to.Ptr("signer_1"),
}
field, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Fields().Post(context.Background(), body, nil)
```
