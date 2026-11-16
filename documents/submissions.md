# Submissions

A submission instantiates a signing workflow — it maps real people to signer roles and generates unique signing links.

### `GET` `/api/v1/documents/{id}/submissions`

 — Scopes: `submissions:read`

### `POST` `/api/v1/documents/{id}/submissions`

Create a submission with signers and pre-fill — Scopes: `submissions:write`

### `DELETE` `/api/v1/documents/{id}/submissions/{sid}`

Delete a submission — Scopes: `submissions:write`

## Create a submission

**Required Header**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `Idempotency-Key` | `string` | Yes | A caller-generated 1–255 character printable ASCII key. Reuse it only to retry the same logical request. |

**Request Body**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signers` | `SignerInput[]` | Yes | 1–100 entries. Must cover all required signer roles. |
| `suppress_notifications` | `boolean` | No | Defaults to true. Set false to send signer invitations; this additionally requires submissions:send_notifications. |

The serialized request body is limited to 1,048,576 UTF-8 bytes. Each signer prefill may contain at most 100 fields, 10 levels of nesting, and 65,536 serialized UTF-8 bytes.

**SignerInput fields**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | No | Full name of the signer |
| `email` | `email` | No | Triggers invitation email if mail integration is configured |
| `phone` | `string` | No | Phone for SMS notifications |
| `password` | `string` | No | Protect the signing link with a password |
| `prefill` | `object` | No | Map of field ID or field label → value. Labels are resolved against fields assigned to this signer's role. If a label matches multiple fields, the request is rejected with disambiguation details. |
| `prefillReadonly` | `boolean` | No | If true, prefilled fields are locked for editing by the signer |
| `readonlyFieldIds` | `string[]` | No | List of field IDs or labels to mark as read-only for this signer. Labels are resolved against fields assigned to this signer's role. |

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents/DOC_ID/submissions \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -H "Idempotency-Key: document-create-7f3f" \\
  -d '{
    "signers": [
      {
        "role": "signer_1",
        "name": "John Doe",
        "email": "john@acme.com",
        "prefill": {
          "Full Name": "John Doe",
          "Company": "Acme Corp Ltd",
          "Date": "2026-03-01"
        },
        "prefillReadonly": true
      }
    ]
  }'
```

**TypeScript SDK**
```typescript
import {
  createClient,
  createDocumentSubmission,
} from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const submission = await createDocumentSubmission(
  client,
  "00000000-0000-0000-0000-000000000001",
  {
    signers: [{
      role: "signer_1",
      name: "John Doe",
      email: "john@acme.com",
      prefill: {
        "Full Name": "John Doe",
        "Company": "Acme Corp Ltd",
      },
      prefillReadonly: true,
    }],
  },
  "document-create-7f3f",
)

const signerLink = submission.signers?.[0]?.link
```

**Python SDK**
```python
from uuid import UUID

from formalingo import create_document_submission
from formalingo.models.create_submission_body import CreateSubmissionBody
from formalingo.models.signer_input import SignerInput
from formalingo.models.signer_input_prefill import SignerInput_prefill

prefill = SignerInput_prefill(
    additional_data={"Full Name": "John Doe"},
)
signer = SignerInput(
    role="signer_1",
    name="John Doe",
    email="john@acme.com",
    prefill=prefill,
    prefill_readonly=True,
)

body = CreateSubmissionBody(signers=[signer])

submission = await create_document_submission(
    client,
    UUID("00000000-0000-0000-0000-000000000001"),
    body,
    "document-create-7f3f",
)
signer_link = submission.signers[0].link
```

**.NET SDK**
```csharp
using Formalingo.Sdk;
using Formalingo.Sdk.Generated.Models;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var body = new CreateSubmissionBody {
    Signers = [
        new() {
            Role = "signer_1",
            Name = "John Doe",
            Email = "john@acme.com",
            Prefill = new SignerInput_prefill {
              AdditionalData = new Dictionary<string, object> {
                ["Full Name"] = "John Doe",
              },
            },
            PrefillReadonly = true,
        },
    ],
};
var submission = await FormalingoClientFactory.CreateDocumentSubmissionAsync(
    client,
    Guid.Parse("00000000-0000-0000-0000-000000000001"),
    body,
    "document-create-7f3f");
var signerLink = submission.Signers?[0].Link;
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

prefill := models.NewSignerInput_prefill()
prefill.SetAdditionalData(map[string]any{"Full Name": "John Doe"})
signer := models.NewSignerInput()
role, name, email, readonly := "signer_1", "John Doe", "john@acme.com", true
signer.SetRole(&role)
signer.SetName(&name)
signer.SetEmail(&email)
signer.SetPrefill(prefill)
signer.SetPrefillReadonly(&readonly)
body := models.NewCreateSubmissionBody()
body.SetSigners([]models.SignerInputable{signer})

submission, err := formalingo.CreateDocumentSubmission(
    context.Background(),
    client,
    uuid.MustParse("00000000-0000-0000-0000-000000000001"),
    body,
    "document-create-7f3f",
)
signerLink := *submission.GetSigners()[0].GetLink()
```

**Response** (201)
```json
{
  "success": true,
  "data": {
    "submissionId": "submission-uuid",
    "dispatchId": "dispatch-uuid",
    "dispatchReused": false,
    "linksCreated": true,
    "signers": [
      {
        "id": "signer-uuid",
        "role": "signer_1",
        "label": "Client",
        "name": "John Doe",
        "color": "#13A373",
        "link": "https://www.formalingo.com/d/abc123xyz",
        "order": 0
      }
    ]
  }
}
```

The first successful request returns `201`. Retrying the same normalized request with the same key returns `200` and `dispatchReused: true`. Reusing a key with a different request returns `409`.

## The signing link

Each signer in the response has a `link` — share it with the signer for them to review and sign the document.

## Label-based prefill

Prefill keys can be **field UUIDs** or **field labels**. When using labels, the server resolves them against fields assigned to this signer's role:

- If the label matches exactly one field for the role, it resolves to that field's UUID
- If the label matches a field with no assigned role (unassigned), it falls back to that match
- If the label matches **multiple** fields for the same role, the request is rejected with the matching field IDs so you can disambiguate
- You can mix UUIDs and labels in the same prefill object

The `readonlyFieldIds` array also accepts labels with the same resolution rules.

## Download the signed PDF

Once all signers complete, the submission status becomes `completed` and the signed PDF is available for download.

The completed submission also records signature evidence used by verification:

- Explicit e-sign consent and signer metadata
- Viewed, consent, field completion, submission, and PDF-generated audit events
- Completed PDF hash for later byte-for-byte comparison
- Cryptographic verification seal over the signer, audit, and document evidence

### `GET` `/api/v1/documents/{id}/submissions/{sid}/pdf`

 — Scopes: `submissions:download_pdf`

**curl**
```bash
curl https://app.formalingo.com/api/v1/documents/DOC_ID/submissions/SUB_ID/pdf \\
  -H "Authorization: Bearer af_live_YOUR_KEY"

# → { "data": { "downloadUrl": "https://...", "expiresIn": 300 } }
# Use the downloadUrl to fetch the PDF file directly
```

**TypeScript SDK**
```typescript
const pdf = await client.documents
  .byDocumentId("DOC_ID")
  .submissions.bySubmissionId("SUB_ID")
  .pdf.get()

const response = await fetch(pdf.data.downloadUrl)
const buffer = await response.arrayBuffer()
fs.writeFileSync("signed.pdf", Buffer.from(buffer))
```

**Python SDK**
```python
pdf = await client.api.v1.documents.by_document_id("DOC_ID") \\
    .submissions.by_submission_id("SUB_ID").pdf.get()

import httpx
async with httpx.AsyncClient() as http:
    resp = await http.get(pdf.data.download_url)
    with open("signed.pdf", "wb") as f:
        f.write(resp.content)
```

**.NET SDK**
```csharp
var pdf = await client.Api.V1.Documents["DOC_ID"]
    .Submissions["SUB_ID"].Pdf.GetAsync();

using var http = new HttpClient();
var bytes = await http.GetByteArrayAsync(pdf.Data.DownloadUrl);
File.WriteAllBytes("signed.pdf", bytes);
```

**Go SDK**
```go
pdf, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").
    Submissions().BySubmissionId("SUB_ID").Pdf().Get(context.Background(), nil)

resp, _ := http.Get(*pdf.Data.DownloadUrl)
defer resp.Body.Close()
out, _ := os.Create("signed.pdf")
defer out.Close()
io.Copy(out, resp.Body)
```

**Response** (200)
```json
{
  "success": true,
  "data": {
    "submissionId": "submission-uuid",
    "completedAt": "2026-03-11T14:30:00.000Z",
    "downloadUrl": "https://storage.supabase.co/...signed.pdf?token=...",
    "expiresIn": 300
  }
}
```

The `downloadUrl` is a presigned URL that expires in 5 minutes. Call the endpoint again to get a fresh URL if needed. Returns `404` if the submission is not yet completed or the PDF is still generating.

## Verify a completed submission

Use the verification certificate when you need to prove what happened or check whether a PDF still matches the recorded evidence.

- Public certificate: `https://www.formalingo.com/verify/{submissionId}`
- Public API: `GET /api/verify/document/{submissionId}`
- Creator API: `GET /api/documents/{id}/verify?submissionId={submissionId}`

The certificate can show signer status, consent and completion timestamps, the audit event timeline, the PDF hash match result, and the seal verification result. Older submissions generated before sealing may need PDF regeneration before hash and seal verification are available.

Formalingo is designed to support ESIGN/UETA-style electronic signature workflows in the US. It supports standard electronic signature evidence trails internationally, but it is not a qualified trust service provider and does not create eIDAS qualified electronic signatures (QES).

## Cancel an unsigned signing link

Send `POST /api/v1/documents/{id}/submissions/{sid}/expire` with a JSON body such as `{"reason":"Replaced by a new signing link"}` and the `submissions:write` permission. This expires every active signer link on a completely unsigned, unsealed submission, while retaining answers, consent and audit evidence. Repeating the request does not reopen or extend a link. Partially signed or completed submissions remain protected and return 409.
