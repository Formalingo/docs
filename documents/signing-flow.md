# End-to-End Signing Flow

This guide walks through the complete workflow from uploading a document to getting a signed PDF.

## Signature evidence

Completed signing workflows keep a signer and document evidence package alongside the PDF:

- Explicit e-sign consent before submission
- Viewed, consent, field completion, submission, and PDF-generated audit events
- Signer timestamps, IP address, and user agent where available
- Completed PDF hash for later byte-for-byte comparison
- Cryptographic verification seal over the signer, audit, and document evidence
- Public verification certificate at `/verify/{submissionId}`

Formalingo is designed to support ESIGN/UETA-style electronic signature workflows in the US. Internationally, it supports standard electronic signature evidence trails. It is not a qualified trust service provider and does not create eIDAS qualified electronic signatures (QES), which require qualified certificates and qualified trust-service workflows.

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
  .byId("DOC_ID")
  .signerRoles.post({ label: "Client" })
```

**Python SDK**
```python
body = CreateSignerRoleBody()
body.label = "Client"
role = await client.api.v1.documents.by_id(UUID("00000000-0000-0000-0000-000000000001")).signer_roles.post(body)
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
role, err := client.Api().V1().Documents().ById(uuid.MustParse("00000000-0000-0000-0000-000000000001")).SignerRoles().Post(context.Background(), body, nil)
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
const field = await client.documents.byId("DOC_ID").fields.post({
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

field = await client.api.v1.documents.by_id(UUID("00000000-0000-0000-0000-000000000001")).fields.post(body)
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
field, err := client.Api().V1().Documents().ById(uuid.MustParse("00000000-0000-0000-0000-000000000001")).Fields().Post(context.Background(), body, nil)
```

### Request supporting files

Use **Supporting attachments** → **Add attachment** in the editor, or create a
request through the existing public field endpoint and SDKs before publishing:

```json
{
  "label": "Proof of address",
  "type": "file_upload",
  "assigneeRole": "signer_1",
  "options": { "supporting_attachment": true }
}
```

Send this body to `POST /api/v1/documents/DOC_ID/fields`. Supporting requests
need no page coordinates and are required by default; `isRequired: false` makes
one optional. Read, edit, and delete them through existing field CRUD. The signer
uploads files during signing; missing required uploads prevent completion. Files
stay outside the document body and signed PDF, while ordinary placed File fields
retain their rendering. See [supporting attachment authoring](/docs/documents/fields#supporting-attachments)
for retry IDs, editing constraints, and typed SDK examples.

API keys use `documents:write` for authoring and `documents:read` for definitions.
Actual uploaded-file values/downloads require the separate
`submissions:download_files` permission.

## Step 4: Create a submission

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents/DOC_ID/submissions \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -H "Idempotency-Key: signing-flow-customer-42" \\
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
const sub = await createDocumentSubmission(
  client,
  "00000000-0000-0000-0000-000000000001",
  {
    signers: [{
      role: "signer_1",
      name: "Jane Smith",
      email: "jane@client.com",
      prefill: { COMPANY_FIELD_ID: "Client Corp Ltd" },
    }],
  },
  "signing-flow-customer-42",
)

const signingLink = sub.signers?.[0]?.link
```

**Python SDK**
```python
prefill = SignerInput_prefill(
    additional_data={"COMPANY_FIELD_ID": "Client Corp Ltd"},
)
signer = SignerInput(
    role="signer_1",
    name="Jane Smith",
    email="jane@client.com",
    prefill=prefill,
)
body = CreateSubmissionBody(signers=[signer])

from kiota_abstractions.base_request_configuration import RequestConfiguration
from kiota_abstractions.headers_collection import HeadersCollection

configuration = RequestConfiguration(headers=HeadersCollection())
configuration.headers.add("Idempotency-Key", "signing-flow-customer-42")
response = await client.api.v1.documents.by_id(
    UUID("00000000-0000-0000-0000-000000000001")
).submissions.post(body, configuration)
sub = response.data
signing_link = sub.signers[0].link
```

**.NET SDK**
```csharp
var body = new CreateSubmissionBody {
    Signers = [
        new SignerInput {
            Role = "signer_1",
            Name = "Jane Smith",
            Email = "jane@client.com",
            Prefill = new SignerInput_prefill {
                AdditionalData = new Dictionary<string, object> {
                    ["COMPANY_FIELD_ID"] = "Client Corp Ltd",
                },
            },
        },
    ],
};
var response = await client.Api.V1.Documents[
    Guid.Parse("00000000-0000-0000-0000-000000000001")
].Submissions.PostAsync(body,
    configuration => configuration.Headers.Add("Idempotency-Key", "signing-flow-customer-42"));
var sub = response!.Data;
var signingLink = sub.Signers?[0].Link;
```

**Go SDK**
```go
prefill := models.NewSignerInput_prefill()
prefill.SetAdditionalData(map[string]any{
    "COMPANY_FIELD_ID": "Client Corp Ltd",
})
signer := models.NewSignerInput()
role, name, email := "signer_1", "Jane Smith", "jane@client.com"
signer.SetRole(&role)
signer.SetName(&name)
signer.SetEmail(&email)
signer.SetPrefill(prefill)
body := models.NewCreateSubmissionBody()
body.SetSigners([]models.SignerInputable{signer})

// Import abstractions "github.com/microsoft/kiota-abstractions-go".
headers := abstractions.NewRequestHeaders()
headers.Add("Idempotency-Key", "signing-flow-customer-42")
configuration := &abstractions.RequestConfiguration[abstractions.DefaultQueryParameters]{Headers: headers}
response, err := client.Api().V1().Documents().ById(
    uuid.MustParse("00000000-0000-0000-0000-000000000001"),
).Submissions().Post(context.Background(), body, configuration)
sub := response.GetData()
signingLink := *sub.GetSigners()[0].GetLink()
```

## Step 5: Share the signing link

Send `data.signers[0].link` from the HTTP response to your signer. The TypeScript
helper returns `data` directly; the other examples assign the generated API
envelope's data to `sub` before reading the signing link.

## Step 6: Wait for completion

When all signers complete signing:

- Submission `status` → `completed`
- A signed PDF is generated and stored
- A completed PDF hash and verification seal are recorded
- A `document_all_complete` webhook event fires (if configured)

You can either **poll** the submissions endpoint or **listen** to the `document_all_complete` webhook.

### Option A: Poll for completion

**curl**
```bash
curl https://app.formalingo.com/api/v1/documents/DOC_ID/submissions \\
  -H "Authorization: Bearer af_live_YOUR_KEY"
# Look for status: "completed" in the response
```

**TypeScript SDK**
```typescript
const subs = await client.documents.byId("DOC_ID").submissions.get()
const completed = subs.data.find(s => s.status === "completed")
```

**Python SDK**
```python
subs = await client.api.v1.documents.by_id(UUID("00000000-0000-0000-0000-000000000001")).submissions.get()
completed = next((s for s in subs.data if s.status == "completed"), None)
```

**.NET SDK**
```csharp
var subs = await client.Api.V1.Documents["DOC_ID"].Submissions.GetAsync();
var completed = subs.Data.FirstOrDefault(s => s.Status == "completed");
```

**Go SDK**
```go
subs, err := client.Api().V1().Documents().ById(uuid.MustParse("00000000-0000-0000-0000-000000000001")).Submissions().Get(context.Background(), nil)
// Find submission with status "completed"
```

### Option B: Webhook

Configure a `document_all_complete` webhook — see [Webhook Events](/docs/webhooks/events) for payload details.

## Step 7: Verify the evidence

After completion, use the verification certificate to review the signing evidence before or after downloading the signed PDF.

- Public certificate: `https://www.formalingo.com/verify/{submissionId}`
- Public API: `GET /api/verify/document/{submissionId}`
- Creator API: `GET /api/documents/{id}/verify?submissionId={submissionId}`

Verification checks whether the current PDF bytes match the recorded hash and whether the stored evidence payload matches the cryptographic seal. Older submissions generated before sealing may need PDF regeneration before hash and seal verification are available.

## Step 8: Download the signed PDF

Once the submission is completed, download the signed PDF:

### `GET` `/api/v1/documents/{id}/submissions/{sid}/pdf`

 — Scopes: `submissions:download_pdf`

Returns a short-lived **presigned download URL** (5-minute expiry). Use the URL to download the file directly.

**curl**
```bash
# Get the download URL
curl https://app.formalingo.com/api/v1/documents/DOC_ID/submissions/SUB_ID/pdf \\
  -H "Authorization: Bearer af_live_YOUR_KEY"

# Response: { "data": { "downloadUrl": "https://...", "expiresIn": 300 } }

# Download the PDF
curl -o signed-contract.pdf "DOWNLOAD_URL_FROM_RESPONSE"
```

**TypeScript SDK**
```typescript
const pdf = await client.documents
  .byId("DOC_ID")
  .submissions.bySubmissionId("SUB_ID")
  .pdf.get()

// pdf.data.downloadUrl — presigned URL (5-min expiry)
// Download it with fetch:
const response = await fetch(pdf.data.downloadUrl)
const buffer = await response.arrayBuffer()
fs.writeFileSync("signed-contract.pdf", Buffer.from(buffer))
```

**Python SDK**
```python
pdf = await client.api.v1.documents.by_id(UUID("00000000-0000-0000-0000-000000000001")) \\
    .submissions.by_submission_id("SUB_ID").pdf.get()

# pdf.data.download_url — presigned URL (5-min expiry)
import httpx
async with httpx.AsyncClient() as http:
    resp = await http.get(pdf.data.download_url)
    with open("signed-contract.pdf", "wb") as f:
        f.write(resp.content)
```

**.NET SDK**
```csharp
var pdf = await client.Api.V1.Documents["DOC_ID"]
    .Submissions["SUB_ID"].Pdf.GetAsync();

// pdf.Data.DownloadUrl — presigned URL (5-min expiry)
using var http = new HttpClient();
var bytes = await http.GetByteArrayAsync(pdf.Data.DownloadUrl);
File.WriteAllBytes("signed-contract.pdf", bytes);
```

**Go SDK**
```go
pdf, err := client.Api().V1().Documents().ById(uuid.MustParse("00000000-0000-0000-0000-000000000001")).
    Submissions().BySubmissionId("SUB_ID").Pdf().Get(context.Background(), nil)

// pdf.Data.DownloadUrl — presigned URL (5-min expiry)
resp, _ := http.Get(*pdf.Data.DownloadUrl)
defer resp.Body.Close()
out, _ := os.Create("signed-contract.pdf")
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

## Retrieve supporting files

As the document creator, open the submission's menu and choose **Supporting
files** to see and download the original uploads. Other signers and recipient
copies do not receive them.

Configured completion webhooks include supporting files as authenticated file
descriptors, separately from `submission.signedPdf`. Use a trusted API key with
`submissions:download_files` to request the descriptor's `downloadUrl`, then
fetch the short-lived storage URL from the response. See [supporting files in
completion webhooks](/docs/webhooks/events#supporting-files-in-completion-webhooks).
