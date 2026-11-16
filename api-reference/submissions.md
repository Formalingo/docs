# Submissions

Create signing submissions and manage signers with pre-fill support.

## Endpoints

## Phone normalization

Phone inputs accept up to 80 characters.

- International input may include spaces, parentheses, and hyphens, but must include `+`.
- National input uses the workspace default phone country.
- Successful creates and edits return and store E.164, for example `+12025550100`.
- Invalid phone input returns `details.field`, `details.code`, and an optional import row or signer index.
- Existing rows are normalized only when used; they are not bulk rewritten.

Validation failures always return `{ success: false, error }`; phone failures additionally include optional `details` with `field`, `code`, and an import/bulk `row` or signer `index` when applicable.

### `GET` `/api/v1/documents/{id}/submissions`

 — Scopes: `submissions:read`, `submissions:download_pdf`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().Get(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/submissions`

 — Scopes: `submissions:write`

**TypeScript SDK**
```typescript
createDocumentSubmission(client, "ID", body, "YOUR_STABLE_IDEMPOTENCY_KEY")
```

**Python SDK**
```python
from kiota_abstractions.base_request_configuration import RequestConfiguration
from kiota_abstractions.headers_collection import HeadersCollection

configuration = RequestConfiguration(headers=HeadersCollection())
configuration.headers.add("Idempotency-Key", "YOUR_STABLE_IDEMPOTENCY_KEY")
await client.api.v1.documents.by_id(UUID("ID")).submissions.post(body, configuration)
```

**.NET SDK**
```csharp
await client.Api.V1.Documents[Guid.Parse("ID")].Submissions.PostAsync(body, configuration => configuration.Headers.Add("Idempotency-Key", "YOUR_STABLE_IDEMPOTENCY_KEY"))
```

**Go SDK**
```go
// Import abstractions "github.com/microsoft/kiota-abstractions-go".
headers := abstractions.NewRequestHeaders()
headers.Add("Idempotency-Key", "YOUR_STABLE_IDEMPOTENCY_KEY")
configuration := &abstractions.RequestConfiguration[abstractions.DefaultQueryParameters]{Headers: headers}
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().Post(ctx, body, configuration)
```

### `GET` `/api/v1/documents/{id}/submissions/{sid}`

 — Scopes: `submissions:read`, `submissions:download_pdf`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Get(ctx, nil)
```

### `DELETE` `/api/v1/documents/{id}/submissions/{sid}`

 — Scopes: `submissions:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").delete()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Delete(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/submissions/{sid}/expire`

 — Scopes: `submissions:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").expire.post(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).expire.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].Expire.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Expire().Post(ctx, body, nil)
```

### `GET` `/api/v1/documents/{id}/submissions/{sid}/signers`

 — Scopes: `submissions:read`, `submissions:download_pdf`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").signers.get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).signers.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].Signers.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Signers().Get(ctx, nil)
```

### `PUT` `/api/v1/documents/{id}/submissions/{sid}/signers/{signerId}`

 — Scopes: `submissions:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").signers.bySignerId("SIGNERID").put(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).signers.by_signer_id(UUID("SIGNERID")).put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].Signers[Guid.Parse("SIGNERID")].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Signers().BySignerId(uuid.MustParse("SIGNERID")).Put(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}/submissions/{sid}/signers/{signerId}`

 — Scopes: `submissions:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").signers.bySignerId("SIGNERID").delete()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).signers.by_signer_id(UUID("SIGNERID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].Signers[Guid.Parse("SIGNERID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Signers().BySignerId(uuid.MustParse("SIGNERID")).Delete(ctx, nil)
```

### `GET` `/api/v1/documents/{id}/submissions/{sid}/pdf`

 — Scopes: `submissions:download_pdf`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").pdf.get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).pdf.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].Pdf.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Pdf().Get(ctx, nil)
```

### `GET` `/api/v1/documents/{id}/submissions/{sid}/signers/{signerId}/responses`

 — Scopes: `submissions:read`, `submissions:download_pdf`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").signers.bySignerId("SIGNERID").responses.get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).signers.by_signer_id(UUID("SIGNERID")).responses.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].Signers[Guid.Parse("SIGNERID")].Responses.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Signers().BySignerId(uuid.MustParse("SIGNERID")).Responses().Get(ctx, nil)
```

### `PATCH` `/api/v1/documents/{id}/submissions/{sid}/signers/{signerId}/responses`

 — Scopes: `submissions:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").signers.bySignerId("SIGNERID").responses.patch(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).submissions.by_sid(UUID("SID")).signers.by_signer_id(UUID("SIGNERID")).responses.patch(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Submissions[Guid.Parse("SID")].Signers[Guid.Parse("SIGNERID")].Responses.PatchAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Submissions().BySid(uuid.MustParse("SID")).Signers().BySignerId(uuid.MustParse("SIGNERID")).Responses().Patch(ctx, body, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
