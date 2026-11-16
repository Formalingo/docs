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

Supported types: `digital_signature`, `initials`, `short_text`, `date`, `email`, `phone`, `number`, `yes_no`, `mcq`, `dropdown`, `calculation`.

`calculation` fields are computed outputs, not signer inputs. Set `options.formula` with field references such as `{{First Name}} {{Last Name}}`; the signer never fills the computed field directly, and the computed value is included in the completed PDF. Use `options.result_type: "text"` for text interpolation or `"number"` for arithmetic formulas.

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
| `options.result_type` | `text | number` | No | Computed output type. Defaults to text for document calculation fields. |
| `direction` | `auto | rtl | ltr` | No | Text direction for RTL languages |

## Computed fields

Use `type: "calculation"` when a document field should display a value derived from other fields on the same document. Computed fields are always read-only and non-required; they require an `assigneeRole` so the derived value is materialized with the correct signer when they submit.

```json
{
  "label": "Full Name",
  "type": "calculation",
  "pageNumber": 1,
  "x": 0.1,
  "y": 0.2,
  "width": 0.35,
  "height": 0.03,
  "assigneeRole": "signer_1",
  "options": {
    "formula": "{{First Name}} {{Last Name}}",
    "result_type": "text"
  }
}
```

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

const field = await client.documents.byId("DOC_ID").fields.post({
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

field = await client.api.v1.documents.by_id(UUID("00000000-0000-0000-0000-000000000001")).fields.post(body)
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
field, err := client.Api().V1().Documents().ById(uuid.MustParse("00000000-0000-0000-0000-000000000001")).Fields().Post(context.Background(), body, nil)
```

## Images uploaded as company stamps

An ordinary placed `file_upload` field containing a PNG, JPEG, WebP or single-frame GIF renders the image inside its field box in newly generated signed PDFs. The image keeps its aspect ratio and is centered in the box. Other attachment types render their filename. Set the field’s `assigneeRole` to the signer who uploads the stamp; no separate stamp field type is required.

Uploads are bounded and verified against their recorded byte hash before rendering. Historical uploads without a recorded hash require an audited recovery or a new upload. Existing sealed PDF files are retained unchanged.

## Supporting attachments

To collect files without adding them to the signed document, open **Supporting
attachments** in the document editor and choose **Add attachment**. The section
is in the left sidebar for uploaded PDFs and the right sidebar for generated
documents. Enter a label, choose the signer who should upload it, and save.
**Required** is enabled by default; turn it off for an optional request.

Supporting attachments need no placement on a page. The signer uploads them
before finishing, and required uploads must succeed before completion. These
files, their filenames, and their links stay out of the document and signed PDF.
They are available to the document creator and the configured completion webhook,
not to other signers or recipient copies. Existing placed File fields continue
to render on the document.

To retrieve uploaded files, open the submission's menu and select **Supporting
files**, then download the file you need. See [completion webhook file
payloads](/docs/webhooks/events#supporting-files-in-completion-webhooks) for
integration downloads.

### Create and manage requests through the API

Use the existing document field endpoints. For example, create a supporting
request with `POST /api/v1/documents/DOC_ID/fields`:

```json
{
  "id": "00000000-0000-4000-8000-000000000001",
  "label": "Proof of address",
  "type": "file_upload",
  "assigneeRole": "signer_1",
  "options": { "supporting_attachment": true }
}
```

`id` is optional; keep the same UUID when retrying the same creation request.
For supporting requests, omit page coordinates or provide all five geometry
values as zero. `isRequired` defaults to true; set it to false for an optional
upload. Ordinary placed fields still require page positioning.

- Read requirement definitions with `GET /api/v1/documents/DOC_ID/fields`.
- Edit a request with `PUT /api/v1/documents/DOC_ID/fields/FIELD_ID`, for example
  `{ "label": "Updated proof of address", "isRequired": false }`.
- Remove it with `DELETE /api/v1/documents/DOC_ID/fields/FIELD_ID`.

The field's supporting/ordinary kind is fixed after creation. If an update
includes `options`, keep `supporting_attachment: true`. Draft edits affect
future published submissions; existing submissions retain their pinned request
requirements. Generated SDKs expose typed field options for the marker;
see the [SDK guides](/docs/sdks).

API keys need `documents:write` for authoring and `documents:read` for definitions.
Downloading actual uploads separately requires `submissions:download_files`.
Session/OAuth management of supporting requests is limited to the document
creator; OAuth also requires the applicable document permission.
