# Conditional Logic

Define conditional show/hide rules for questions and sections.

## Endpoints

### `GET` `/api/v1/forms/{id}/rules`

 — Scopes: `forms:read`

**TypeScript SDK**
```typescript
client.forms.byId("ID").rules.get()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).rules.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Rules.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Rules().Get(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/rules`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").rules.post({})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).rules.post()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Rules.PostAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Rules().Post(ctx, nil)
```

### `PUT` `/api/v1/forms/{id}/rules/{rid}`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").rules.byRid("RID").put({})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).rules.by_rid(UUID("RID")).put()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Rules[Guid.Parse("RID")].PutAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Rules().ByRid(uuid.MustParse("RID")).Put(ctx, nil)
```

### `DELETE` `/api/v1/forms/{id}/rules/{rid}`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").rules.byRid("RID").delete()
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).rules.by_rid(UUID("RID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Rules[Guid.Parse("RID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Rules().ByRid(uuid.MustParse("RID")).Delete(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
