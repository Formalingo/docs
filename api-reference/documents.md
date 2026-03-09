# Documents

Upload and manage PDF/DOCX documents for digital signing.

## Endpoints

### `GET` `/api/v1/documents`

List documents — Scopes: `read:documents`

**TypeScript SDK**
```typescript
client.documents.get()
```

**Python SDK**
```python
client.documents.get()
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

Upload document — Upload a PDF or DOCX via multipart/form-data. Max 50 MB. — Scopes: `write:documents`

**TypeScript SDK**
```typescript
client.documents.post(body)
```

**Python SDK**
```python
client.documents.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().Post(ctx, body, nil)
```

### `GET` `/api/v1/documents/{id}`

Get document — Scopes: `read:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").get()
```

**Python SDK**
```python
client.documents.by_id("ID").get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Get(ctx, nil)
```

### `PUT` `/api/v1/documents/{id}`

Update document — Scopes: `write:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").put(body)
```

**Python SDK**
```python
client.documents.by_id("ID").put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Put(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}`

Delete document — Scopes: `delete:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").delete()
```

**Python SDK**
```python
client.documents.by_id("ID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
