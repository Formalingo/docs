# Archiving & Deleting Documents

### `DELETE` `/api/v1/documents/{id}`

 — Scopes: `delete:documents`

## Soft delete (default)

Moves the document to Trash. Original file is removed from storage. Existing signing links stop working.

**curl**
```bash
curl -X DELETE https://app.formalingo.com/api/v1/documents/DOC_ID \\
  -H "Authorization: Bearer af_live_YOUR_KEY"
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

// See SDK setup guide for client initialization
await client.documents.byDocumentId("DOC_ID").delete()
```

**Python SDK**
```python
await client.api.v1.documents.by_document_id("DOC_ID").delete()
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

await client.Api.V1.Documents["DOC_ID"].DeleteAsync();
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Delete(context.Background(), nil)
```

## Permanent delete

Permanently deletes the document and all associated data (fields, submissions, audit events):

**curl**
```bash
curl -X DELETE "https://app.formalingo.com/api/v1/documents/DOC_ID?permanent=true" \\
  -H "Authorization: Bearer af_live_YOUR_KEY"
```

**TypeScript SDK**
```typescript
await client.documents.byDocumentId("DOC_ID").delete({
  queryParameters: { permanent: true },
})
```

**Python SDK**
```python
from kiota_abstractions.request_configuration import RequestConfiguration

config = RequestConfiguration()
config.query_parameters = {"permanent": True}

await client.api.v1.documents.by_document_id("DOC_ID").delete(request_configuration=config)
```

**.NET SDK**
```csharp
await client.Api.V1.Documents["DOC_ID"].DeleteAsync(config => {
    config.QueryParameters.Permanent = true;
});
```

**Go SDK**
```go
config := &documents.DocumentItemRequestBuilderDeleteRequestConfiguration{
    QueryParameters: &documents.DocumentItemRequestBuilderDeleteQueryParameters{
        Permanent: to.Ptr(true),
    },
}
err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Delete(context.Background(), config)
```

> **Warning:** Permanent deletion cannot be undone.

The `delete:documents` scope is required for both operations.
