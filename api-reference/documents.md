# Documents

Upload and manage PDF/DOCX documents for digital signing.

## Endpoints

### `GET` `/api/v1/documents`

 — Scopes: `documents:read`

**TypeScript SDK**
```typescript
client.documents.get()
```

**Python SDK**
```python
await client.api.v1.documents.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().Get(ctx, nil)
```

### `POST` `/api/v1/documents`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.post(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().Post(ctx, body, nil)
```

### `POST` `/api/v1/documents/import`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.import.post(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.import.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents.Import.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().Import().Post(ctx, body, nil)
```

### `GET` `/api/v1/documents/{id}`

 — Scopes: `documents:read`

**TypeScript SDK**
```typescript
client.documents.byId("ID").get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Get(ctx, nil)
```

### `GET` `/api/v1/documents/{id}/export`

 — Scopes: `documents:read`

**TypeScript SDK**
```typescript
client.documents.byId("ID").export.get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).export.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Export.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Export().Get(ctx, nil)
```

### `PUT` `/api/v1/documents/{id}`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").put(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Put(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}`

 — Scopes: `documents:delete`

**TypeScript SDK**
```typescript
client.documents.byId("ID").delete()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Delete(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/publish`

 — Scopes: `documents:write`, `documents:publish`

**TypeScript SDK**
```typescript
client.documents.byId("ID").publish.post({})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).publish.post()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Publish.PostAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Publish().Post(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
