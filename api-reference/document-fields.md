# Document Fields

Place and manage signature and data fields on document pages.

## Endpoints

### `GET` `/api/v1/documents/{id}/fields`

List fields — Scopes: `read:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").fields.get()
```

**Python SDK**
```python
client.documents.by_id("ID").fields.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Fields.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Fields().Get(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/fields`

Create field — Scopes: `write:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").fields.post(body)
```

**Python SDK**
```python
client.documents.by_id("ID").fields.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Fields.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Fields().Post(ctx, body, nil)
```

### `PUT` `/api/v1/documents/{id}/fields/{fid}`

Update field — Scopes: `write:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").fields.byFid("FID").put(body)
```

**Python SDK**
```python
client.documents.by_id("ID").fields.by_fid("FID").put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Fields["FID"].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Fields().ByFid("FID").Put(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}/fields/{fid}`

Delete field — Scopes: `write:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").fields.byFid("FID").delete()
```

**Python SDK**
```python
client.documents.by_id("ID").fields.by_fid("FID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Fields["FID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Fields().ByFid("FID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
