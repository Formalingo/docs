# End-to-End Signing Flow

This guide walks through the complete workflow from uploading a document to getting a signed PDF.

## Step 1: Upload the document

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -F "file=@contract.pdf" \\
  -F "title=Service Agreement"
# → { "data": { "id": "DOC_ID", ... } }
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const doc = await client.documents.post({
  file: fs.createReadStream("contract.pdf"),
  title: "Service Agreement",
})
```

**Python SDK**
```python
from formalingo import create_client

client = create_client("af_live_YOUR_KEY")

# File upload requires multipart form data
body = CreateDocumentBody()
body.file = open("contract.pdf", "rb")
body.title = "Service Agreement"
doc = await client.api.v1.documents.post(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

// File upload via multipart form
using var stream = File.OpenRead("contract.pdf");
var doc = await client.Api.V1.Documents.PostAsync(new() {
    File = stream,
    Title = "Service Agreement",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

// File upload via multipart form
f, _ := os.Open("contract.pdf")
body := &models.CreateDocumentBody{
    File:  f,
    Title: to.Ptr("Service Agreement"),
}
doc, err := client.Api().V1().Documents().Post(context.Background(), body, nil)
```

## Step 2: Create signer roles

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents/DOC_ID/signer-roles \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{ "label": "Client" }'
# → { "data": { "role": "signer_1", ... } }
```

**TypeScript SDK**
```typescript
const role = await client.documents
  .byDocumentId("DOC_ID")
  .signerRoles.post({ label: "Client" })
```

**Python SDK**
```python
body = CreateSignerRoleBody()
body.label = "Client"
role = await client.api.v1.documents.by_document_id("DOC_ID").signer_roles.post(body)
```

**.NET SDK**
```csharp
var role = await client.Api.V1.Documents["DOC_ID"]
    .SignerRoles.PostAsync(new() { Label = "Client" });
```

**Go SDK**
```go
body := &models.CreateSignerRoleBody{
    Label: to.Ptr("Client"),
}
role, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").SignerRoles().Post(context.Background(), body, nil)
```

## Step 3: Place fields on the document

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents/DOC_ID/fields \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "label": "Client Signature",
    "type": "digital_signature",
    "pageNumber": 2,
    "x": 0.1, "y": 0.85,
    "width": 0.35, "height": 0.06,
    "assigneeRole": "signer_1",
    "isRequired": true
  }'
```

**TypeScript SDK**
```typescript
const field = await client.documents.byDocumentId("DOC_ID").fields.post({
  label: "Client Signature",
  type: "digital_signature",
  pageNumber: 2,
  x: 0.1,
  y: 0.85,
  width: 0.35,
  height: 0.06,
  assigneeRole: "signer_1",
  isRequired: true,
})
```

**Python SDK**
```python
body = CreateFieldBody()
body.label = "Client Signature"
body.type = "digital_signature"
body.page_number = 2
body.x = 0.1
body.y = 0.85
body.width = 0.35
body.height = 0.06
body.assignee_role = "signer_1"
body.is_required = True

field = await client.api.v1.documents.by_document_id("DOC_ID").fields.post(body)
```

**.NET SDK**
```csharp
var field = await client.Api.V1.Documents["DOC_ID"].Fields.PostAsync(new() {
    Label = "Client Signature",
    Type = "digital_signature",
    PageNumber = 2,
    X = 0.1f,
    Y = 0.85f,
    Width = 0.35f,
    Height = 0.06f,
    AssigneeRole = "signer_1",
    IsRequired = true,
});
```

**Go SDK**
```go
body := &models.CreateFieldBody{
    Label:        to.Ptr("Client Signature"),
    Type:         to.Ptr("digital_signature"),
    PageNumber:   to.Ptr(int32(2)),
    X:            to.Ptr(float64(0.1)),
    Y:            to.Ptr(float64(0.85)),
    Width:        to.Ptr(float64(0.35)),
    Height:       to.Ptr(float64(0.06)),
    AssigneeRole: to.Ptr("signer_1"),
    IsRequired:   to.Ptr(true),
}
field, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Fields().Post(context.Background(), body, nil)
```

## Step 4: Create a submission

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents/DOC_ID/submissions \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "signers": [{
      "role": "signer_1",
      "name": "Jane Smith",
      "email": "jane@client.com",
      "prefill": {
        "COMPANY_FIELD_ID": "Client Corp Ltd"
      }
    }]
  }'
# → { "data": { "signers": [{ "link": "https://www.formalingo.com/d/TOKEN", ... }] } }
```

**TypeScript SDK**
```typescript
const sub = await client.documents.byDocumentId("DOC_ID").submissions.post({
  signers: [{
    role: "signer_1",
    name: "Jane Smith",
    email: "jane@client.com",
    prefill: { COMPANY_FIELD_ID: "Client Corp Ltd" },
  }],
})
```

**Python SDK**
```python
signer = CreateSubmissionSigner()
signer.role = "signer_1"
signer.name = "Jane Smith"
signer.email = "jane@client.com"
signer.prefill = {"COMPANY_FIELD_ID": "Client Corp Ltd"}

body = CreateSubmissionBody()
body.signers = [signer]

sub = await client.api.v1.documents.by_document_id("DOC_ID").submissions.post(body)
```

**.NET SDK**
```csharp
var sub = await client.Api.V1.Documents["DOC_ID"].Submissions.PostAsync(new() {
    Signers = new[] {
        new CreateSubmissionSigner {
            Role = "signer_1",
            Name = "Jane Smith",
            Email = "jane@client.com",
            Prefill = new Dictionary<string, string> {
                ["COMPANY_FIELD_ID"] = "Client Corp Ltd",
            },
        },
    },
});
```

**Go SDK**
```go
body := &models.CreateSubmissionBody{
    Signers: []models.CreateSubmissionSigner{{
        Role:  to.Ptr("signer_1"),
        Name:  to.Ptr("Jane Smith"),
        Email: to.Ptr("jane@client.com"),
        Prefill: map[string]*string{
            "COMPANY_FIELD_ID": to.Ptr("Client Corp Ltd"),
        },
    }},
}
sub, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Submissions().Post(context.Background(), body, nil)
```

## Step 5: Share the signing link

Send `signers[0].link` to your signer. They open it in their browser, review the document, and sign.

## Step 6: Completion

When all signers complete signing:

- Submission `status` → `completed`
- `completedPdfUrl` is populated with the final signed PDF
- A `document_all_complete` webhook event fires (if configured)

You can poll `GET /api/v1/documents/DOC_ID/submissions` or listen to the webhook for completion.
