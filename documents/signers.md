# Managing Signers

### `GET` `/api/v1/documents/{id}/submissions/{sid}/signers`

 — Scopes: `read:submissions`

### `PUT` `/api/v1/documents/{id}/submissions/{sid}/signers/{signerId}`

Update a signer — Scopes: `write:submissions`

### `DELETE` `/api/v1/documents/{id}/submissions/{sid}/signers/{signerId}`

Remove a signer — Scopes: `write:submissions`

## Update a signer

Use this to correct contact details or update expiry:

**Request Body (all optional)**

| Name | Type | Required | Description |
|------|------|----------|-------------|

**curl**
```bash
curl -X PUT https://app.formalingo.com/api/v1/documents/DOC_ID/submissions/SUB_ID/signers/SIGNER_ID \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "email": "new-email@acme.com",
    "expiresAt": "2026-04-30T23:59:59Z"
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

await client.documents
  .byDocumentId("DOC_ID")
  .submissions.bySubmissionId("SUB_ID")
  .signers.bySignerId("SIGNER_ID")
  .put({
    email: "new-email@acme.com",
    expiresAt: "2026-04-30T23:59:59Z",
  })
```

**Python SDK**
```python
from formalingo import create_client

client = create_client("af_live_YOUR_KEY")

body = UpdateSignerBody()
body.email = "new-email@acme.com"
body.expires_at = "2026-04-30T23:59:59Z"

result = await client.api.v1.documents.by_document_id("DOC_ID").submissions.by_submission_id("SUB_ID").signers.by_signer_id("SIGNER_ID").put(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

await client.Api.V1.Documents["DOC_ID"]
    .Submissions["SUB_ID"]
    .Signers["SIGNER_ID"]
    .PutAsync(new() {
        Email = "new-email@acme.com",
        ExpiresAt = "2026-04-30T23:59:59Z",
    });
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.UpdateSignerBody{
    Email:     to.Ptr("new-email@acme.com"),
    ExpiresAt: to.Ptr("2026-04-30T23:59:59Z"),
}
_, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Submissions().BySubmissionId("SUB_ID").Signers().BySignerId("SIGNER_ID").Put(context.Background(), body, nil)
```

## Remove a signer

**curl**
```bash
curl -X DELETE https://app.formalingo.com/api/v1/documents/DOC_ID/submissions/SUB_ID/signers/SIGNER_ID \\
  -H "Authorization: Bearer af_live_YOUR_KEY"
```

**TypeScript SDK**
```typescript
await client.documents
  .byDocumentId("DOC_ID")
  .submissions.bySubmissionId("SUB_ID")
  .signers.bySignerId("SIGNER_ID")
  .delete()
```

**Python SDK**
```python
await client.api.v1.documents.by_document_id("DOC_ID").submissions.by_submission_id("SUB_ID").signers.by_signer_id("SIGNER_ID").delete()
```

**.NET SDK**
```csharp
await client.Api.V1.Documents["DOC_ID"]
    .Submissions["SUB_ID"]
    .Signers["SIGNER_ID"]
    .DeleteAsync();
```

**Go SDK**
```go
err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Submissions().BySubmissionId("SUB_ID").Signers().BySignerId("SIGNER_ID").Delete(context.Background(), nil)
```

> **Note:** Removing a signer that has already completed signing has no effect on the audit trail.
