# Revisions

Read published form and document revision history.

## Endpoints

### `GET` `/api/v1/forms/{id}/revisions`

 — Scopes: `revisions:read`

**TypeScript SDK**
```typescript
client.forms.byId("ID").revisions.get()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).revisions.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Revisions.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Revisions().Get(ctx, nil)
```

### `GET` `/api/v1/forms/{id}/revisions/{version}`

 — Scopes: `revisions:read`

**TypeScript SDK**
```typescript
client.forms.byId("ID").revisions.byVersion(1).get()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).revisions.by_version(1).get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Revisions[1].GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Revisions().ByVersion(1).Get(ctx, nil)
```

### `GET` `/api/v1/documents/{id}/revisions`

 — Scopes: `revisions:read`

**TypeScript SDK**
```typescript
client.documents.byId("ID").revisions.get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).revisions.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Revisions.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Revisions().Get(ctx, nil)
```

### `GET` `/api/v1/documents/{id}/revisions/{version}`

 — Scopes: `revisions:read`

**TypeScript SDK**
```typescript
client.documents.byId("ID").revisions.byVersion(1).get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).revisions.by_version(1).get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].Revisions[1].GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).Revisions().ByVersion(1).Get(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
