# Archiving & Deleting Forms

### `DELETE` `/api/v1/forms/{id}`

 — Scopes: `delete:forms`

## Soft delete (default)

By default, `DELETE /api/v1/forms/{id}` soft-deletes the form — it moves it to Trash and can be restored from the dashboard. Existing recipient links and responses are preserved.

**curl**
```bash
curl -X DELETE https://app.formalingo.com/api/v1/forms/FORM_ID \\
  -H "Authorization: Bearer af_live_YOUR_KEY"
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

await client.forms.byFormId("FORM_ID").delete()
```

**Python SDK**
```python
await client.api.v1.forms.by_form_id("FORM_ID").delete()
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

await client.Api.V1.Forms["FORM_ID"].DeleteAsync();
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

err := client.Api().V1().Forms().ByFormId("FORM_ID").Delete(context.Background(), nil)
```

## Archive first

You can also archive a form without deleting it by updating its status:

```bash
curl -X PUT https://app.formalingo.com/api/v1/forms/FORM_ID \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{ "status": "archived" }'
```

Archived forms are still accessible to existing recipients but closed for new submissions.

## Permanent delete

Add `?permanent=true` to irreversibly delete the form and all its data:

```bash
curl -X DELETE "https://app.formalingo.com/api/v1/forms/FORM_ID?permanent=true" \
  -H "Authorization: Bearer af_live_YOUR_KEY"
```

> **Warning:** Permanent deletion cannot be undone. All questions, sections, recipients, and responses are permanently removed.

The `delete:forms` scope is required for both operations.
