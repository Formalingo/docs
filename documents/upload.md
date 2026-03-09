# Upload a Document

### `POST` `/api/v1/documents`

Upload via multipart/form-data — Scopes: `write:documents`

Documents must be uploaded as `multipart/form-data`. Supported formats: **PDF** and **DOCX**. Maximum size: **50 MB**.

**Form Data Fields**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `file` | `File` | Yes | The PDF or DOCX file to upload |
| `title` | `string` | No | Document title. Defaults to the filename. |

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -F "file=@/path/to/contract.pdf" \\
  -F "title=Service Agreement"
```

**JavaScript**
```javascript
const formData = new FormData()
formData.append('file', fileBlob, 'contract.pdf')
formData.append('title', 'Service Agreement')

const res = await fetch('https://app.formalingo.com/api/v1/documents', {
  method: 'POST',
  headers: { 'Authorization': 'Bearer af_live_YOUR_KEY' },
  body: formData,
})
const { data: document } = await res.json()
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

// Note: File uploads use multipart/form-data.
// Use the raw fetch approach above or the generated
// multipart request builder from the SDK.
const document = await client.documents.post({
  file: fileBlob,
  title: "Service Agreement",
})
```

**Python SDK**
```python
# Note: File uploads use multipart/form-data.
# Use the generated multipart request builder.

document = await client.api.v1.documents.post(body)
# body.file = open("/path/to/contract.pdf", "rb")
# body.title = "Service Agreement"
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

// File uploads use multipart/form-data
using var stream = File.OpenRead("/path/to/contract.pdf");
var document = await client.Api.V1.Documents.PostAsync(new() {
    File = stream,
    Title = "Service Agreement",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

// File uploads use multipart/form-data
f, _ := os.Open("/path/to/contract.pdf")
defer f.Close()

body := &models.CreateDocumentBody{
    File:  f,
    Title: to.Ptr("Service Agreement"),
}
doc, err := client.Api().V1().Documents().Post(context.Background(), body, nil)
```

**Response** (201)
```json
{
  "success": true,
  "data": {
    "id": "doc-uuid",
    "title": "Service Agreement",
    "status": "draft",
    "originalFilename": "contract.pdf",
    "fileType": "pdf",
    "originalUrl": "https://storage.url/...",
    "pageCount": null,
    "createdAt": "2026-02-25T10:00:00.000Z",
    "updatedAt": "2026-02-25T10:00:00.000Z"
  }
}
```

## Next steps

After uploading:

1. [Place fields](/docs/documents/fields) on the document pages
2. [Create signer roles](/docs/documents/signer-roles)
3. [Create a submission](/docs/documents/submissions) with your signers
