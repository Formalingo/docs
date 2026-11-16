# Document Fields

Place signature/data fields and manage supporting upload requests that stay outside the signed document.

## Endpoints

### `GET` `/api/v1/documents/{id}/fields`

 — Scopes: `documents:read`

**TypeScript SDK**
```typescript
client.documents.byId("ID").fields.get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).fields.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Fields.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Fields().Get(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/fields`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").fields.post(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).fields.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Fields.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Fields().Post(ctx, body, nil)
```

### `PUT` `/api/v1/documents/{id}/fields/{fid}`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").fields.byFid("FID").put(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).fields.by_fid(UUID("FID")).put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Fields[Guid.Parse("FID")].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Fields().ByFid(uuid.MustParse("FID")).Put(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}/fields/{fid}`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").fields.byFid("FID").delete()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).fields.by_fid(UUID("FID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Fields[Guid.Parse("FID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Fields().ByFid(uuid.MustParse("FID")).Delete(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
