# Form Questions

Add and manage questions within a form.

## Endpoints

### `GET` `/api/v1/forms/{id}/questions`

 — Scopes: `forms:read`

**TypeScript SDK**
```typescript
client.forms.byId("ID").questions.get()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).questions.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Questions.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Questions().Get(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/questions`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").questions.post(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).questions.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Questions.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Questions().Post(ctx, body, nil)
```

### `PUT` `/api/v1/forms/{id}/questions/{qid}`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").questions.byQid("QID").put(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).questions.by_qid(UUID("QID")).put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Questions[Guid.Parse("QID")].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Questions().ByQid(uuid.MustParse("QID")).Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}/questions/{qid}`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").questions.byQid("QID").delete()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).questions.by_qid(UUID("QID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Questions[Guid.Parse("QID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Questions().ByQid(uuid.MustParse("QID")).Delete(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
