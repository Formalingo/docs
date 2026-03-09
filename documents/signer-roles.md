# Signer Roles

Signer roles are **templates** — they define who will sign (e.g. "Client", "Witness") without specifying actual people. Real signers are added when you [create a submission](/docs/documents/submissions).

### `GET` `/api/v1/documents/{id}/signer-roles`

 — Scopes: `read:documents`

### `POST` `/api/v1/documents/{id}/signer-roles`

Create a signer role — Scopes: `write:documents`

### `DELETE` `/api/v1/documents/{id}/signer-roles/{rid}`

Delete a role — Scopes: `write:documents`

## Create a signer role

The API auto-assigns a color and a role identifier (`signer_1`, `signer_2`, etc.).

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/documents/DOC_ID/signer-roles \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{ "label": "Client" }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

// See SDK setup guide for client initialization
const role = await client.documents.byDocumentId("DOC_ID").signerRoles.post({
  label: "Client",
})
```

**Python SDK**
```python
body = CreateSignerRoleBody()
body.label = "Client"

role = await client.api.v1.documents.by_document_id("DOC_ID").signer_roles.post(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var role = await client.Api.V1.Documents["DOC_ID"].SignerRoles.PostAsync(new() {
    Label = "Client",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.CreateSignerRoleBody{
    Label: to.Ptr("Client"),
}
role, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").SignerRoles().Post(context.Background(), body, nil)
```

**Response** (201)
```json
{
  "success": true,
  "data": {
    "id": "role-uuid",
    "documentId": "doc-uuid",
    "role": "signer_1",
    "label": "Client",
    "color": "#2563EB",
    "order": 0
  }
}
```

The `role` value (e.g. `signer_1`) is what you reference when [placing fields](/docs/documents/fields) (`assigneeRole`) and when [creating submissions](/docs/documents/submissions).
