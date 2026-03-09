# Submissions

A submission instantiates a signing workflow — it maps real people to signer roles and generates unique signing links.

### `GET` `/api/v1/documents/{id}/submissions`

 — Scopes: `read:submissions`

### `POST` `/api/v1/documents/{id}/submissions`

Create a submission with signers and pre-fill — Scopes: `write:submissions`

### `DELETE` `/api/v1/documents/{id}/submissions/{sid}`

Delete a submission — Scopes: `write:submissions`

## Create a submission

**Request Body**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signers` | `SignerInput[]` | Yes | One entry per signer role. Must cover all required roles. |

**SignerInput fields**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | No | Full name of the signer |
| `email` | `email` | No | Triggers invitation email if mail integration is configured |
| `phone` | `string` | No | Phone for SMS notifications |
| `password` | `string` | No | Protect the signing link with a password |
| `prefill` | `object` | No | Map of fieldId → value. Creates DocumentResponse records immediately. |
| `prefillReadonly` | `boolean` | No | If true, prefilled fields are locked for editing by the signer |

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents/DOC_ID/submissions \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "signers": [
      {
        "role": "signer_1",
        "name": "John Doe",
        "email": "john@acme.com",
        "prefill": {
          "field-uuid-1": "John Doe",
          "field-uuid-2": "Acme Corp Ltd",
          "field-uuid-3": "2026-03-01"
        },
        "prefillReadonly": true
      }
    ]
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const submission = await client.documents
  .byDocumentId("DOC_ID")
  .submissions.post({
    signers: [{
      role: "signer_1",
      name: "John Doe",
      email: "john@acme.com",
      prefill: {
        "field-uuid-1": "John Doe",
        "field-uuid-2": "Acme Corp Ltd",
      },
      prefillReadonly: true,
    }],
  })

const signerLink = submission.signers[0].link
```

**Python SDK**
```python
# See SDK setup guide for client initialization

signer = SignerInput()
signer.role = "signer_1"
signer.name = "John Doe"
signer.email = "john@acme.com"
signer.prefill = {"field-uuid-1": "John Doe"}
signer.prefill_readonly = True

body = CreateSubmissionBody()
body.signers = [signer]

submission = await client.api.v1.documents.by_document_id("DOC_ID").submissions.post(body)
signer_link = submission.signers[0].link
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var submission = await client.Api.V1.Documents["DOC_ID"].Submissions.PostAsync(new() {
    Signers = new() {
        new() {
            Role = "signer_1",
            Name = "John Doe",
            Email = "john@acme.com",
            Prefill = new() {
                ["field-uuid-1"] = "John Doe",
            },
            PrefillReadonly = true,
        },
    },
});
var signerLink = submission.Signers[0].Link;
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.CreateSubmissionBody{
    Signers: []models.SignerInput{{
        Role:            to.Ptr("signer_1"),
        Name:            to.Ptr("John Doe"),
        Email:           to.Ptr("john@acme.com"),
        Prefill:         map[string]string{"field-uuid-1": "John Doe"},
        PrefillReadonly: to.Ptr(true),
    }},
}
submission, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Submissions().Post(context.Background(), body, nil)
signerLink := submission.Signers[0].Link
```

**Response** (201)
```json
{
  "success": true,
  "data": {
    "id": "submission-uuid",
    "documentId": "doc-uuid",
    "status": "pending",
    "createdAt": "2026-02-25T10:00:00.000Z",
    "completedAt": null,
    "completedPdfUrl": null,
    "signers": [
      {
        "id": "signer-uuid",
        "role": "signer_1",
        "label": "Client",
        "name": "John Doe",
        "email": "john@acme.com",
        "token": "abc123xyz",
        "status": "pending",
        "link": "https://www.formalingo.com/d/abc123xyz",
        "order": 0,
        "createdAt": "2026-02-25T10:00:00.000Z"
      }
    ]
  }
}
```

## The signing link

Each signer in the response has a `link` — share it with the signer for them to review and sign the document. Once all signers complete signing, the submission status becomes `completed` and a `completedPdfUrl` is available.
