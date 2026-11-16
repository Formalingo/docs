# Form Sections

Group questions into sections within a form.

## Endpoints

### `GET` `/api/v1/forms/{id}/sections`

 — Scopes: `forms:read`

**TypeScript SDK**
```typescript
client.forms.byId("ID").sections.get()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).sections.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Sections.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Sections().Get(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/sections`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").sections.post(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).sections.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Sections.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Sections().Post(ctx, body, nil)
```

### `PUT` `/api/v1/forms/{id}/sections/{sid}`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").sections.bySid("SID").put(body, {})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).sections.by_sid(UUID("SID")).put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Sections[Guid.Parse("SID")].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Sections().BySid(uuid.MustParse("SID")).Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}/sections/{sid}`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").sections.bySid("SID").delete()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).sections.by_sid(UUID("SID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Sections[Guid.Parse("SID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Sections().BySid(uuid.MustParse("SID")).Delete(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
