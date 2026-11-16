# Recipients

Create and manage form recipients with optional pre-filled answers.

## Endpoints

## Phone normalization

Phone inputs accept up to 80 characters.

- International input may include spaces, parentheses, and hyphens, but must include `+`.
- National input uses the workspace default phone country.
- Successful creates and edits return and store E.164, for example `+12025550100`.
- Invalid phone input returns `details.field`, `details.code`, and an optional import row or signer index.
- Existing rows are normalized only when used; they are not bulk rewritten.

Validation failures always return `{ success: false, error }`; phone failures additionally include optional `details` with `field`, `code`, and an import/bulk `row` or signer `index` when applicable.

### `GET` `/api/v1/forms/{id}/recipients`

 — Scopes: `recipients:read`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.get()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).recipients.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Recipients.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Recipients().Get(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/recipients`

 — Scopes: `recipients:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.post(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).recipients.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Recipients.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Recipients().Post(ctx, body, nil)
```

### `POST` `/api/v1/forms/{id}/recipients/bulk`

 — Scopes: `recipients:write`, `recipients:bulk`

**TypeScript SDK**
```typescript
createBulkRecipients(client, "ID", body, "YOUR_STABLE_IDEMPOTENCY_KEY")
```

**Python SDK**
```python
from kiota_abstractions.base_request_configuration import RequestConfiguration
from kiota_abstractions.headers_collection import HeadersCollection

configuration = RequestConfiguration(headers=HeadersCollection())
configuration.headers.add("Idempotency-Key", "YOUR_STABLE_IDEMPOTENCY_KEY")
await client.api.v1.forms.by_id(UUID("ID")).recipients.bulk.post(body, configuration)
```

**.NET SDK**
```csharp
await client.Api.V1.Forms[Guid.Parse("ID")].Recipients.Bulk.PostAsync(body, configuration => configuration.Headers.Add("Idempotency-Key", "YOUR_STABLE_IDEMPOTENCY_KEY"))
```

**Go SDK**
```go
// Import abstractions "github.com/microsoft/kiota-abstractions-go".
headers := abstractions.NewRequestHeaders()
headers.Add("Idempotency-Key", "YOUR_STABLE_IDEMPOTENCY_KEY")
configuration := &abstractions.RequestConfiguration[abstractions.DefaultQueryParameters]{Headers: headers}
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Recipients().Bulk().Post(ctx, body, configuration)
```

### `PUT` `/api/v1/forms/{id}/recipients/{rid}`

 — Scopes: `recipients:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.byRid("RID").put(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).recipients.by_rid(UUID("RID")).put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Recipients[Guid.Parse("RID")].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Recipients().ByRid(uuid.MustParse("RID")).Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}/recipients/{rid}`

 — Scopes: `recipients:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.byRid("RID").delete()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).recipients.by_rid(UUID("RID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Recipients[Guid.Parse("RID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Recipients().ByRid(uuid.MustParse("RID")).Delete(ctx, nil)
```

### `GET` `/api/v1/forms/{id}/recipients/{rid}/responses`

 — Scopes: `recipients:read`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.byRid("RID").responses.get()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).recipients.by_rid(UUID("RID")).responses.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Recipients[Guid.Parse("RID")].Responses.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Recipients().ByRid(uuid.MustParse("RID")).Responses().Get(ctx, nil)
```

### `PATCH` `/api/v1/forms/{id}/recipients/{rid}/responses`

 — Scopes: `recipients:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.byRid("RID").responses.patch(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).recipients.by_rid(UUID("RID")).responses.patch(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Recipients[Guid.Parse("RID")].Responses.PatchAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Recipients().ByRid(uuid.MustParse("RID")).Responses().Patch(ctx, body, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
