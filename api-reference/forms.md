# Forms

Manage forms — create, update, publish, and delete forms in your workspace.

## Endpoints

### `GET` `/api/v1/forms`

 — Scopes: `forms:read`

**TypeScript SDK**
```typescript
client.forms.get()
```

**Python SDK**
```python
await client.api.v1.forms.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().Get(ctx, nil)
```

### `POST` `/api/v1/forms`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.post(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().Post(ctx, body, nil)
```

### `GET` `/api/v1/forms/{id}`

 — Scopes: `forms:read`

**TypeScript SDK**
```typescript
client.forms.byId("ID").get()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Get(ctx, nil)
```

### `PUT` `/api/v1/forms/{id}`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").put(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}`

 — Scopes: `forms:delete`

**TypeScript SDK**
```typescript
client.forms.byId("ID").delete()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Delete(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/publish`

 — Scopes: `forms:write`, `forms:publish`

**TypeScript SDK**
```typescript
client.forms.byId("ID").publish.post({})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).publish.post()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Publish.PostAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Publish().Post(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
