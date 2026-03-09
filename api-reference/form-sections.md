# Form Sections

Group questions into sections within a form.

## Endpoints

### `GET` `/api/v1/forms/{id}/sections`

List sections — Scopes: `read:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").sections.get()
```

**Python SDK**
```python
client.forms.by_id("ID").sections.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Sections.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Sections().Get(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/sections`

Create section — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").sections.post(body)
```

**Python SDK**
```python
client.forms.by_id("ID").sections.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Sections.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Sections().Post(ctx, body, nil)
```

### `PUT` `/api/v1/forms/{id}/sections/{sid}`

Update section — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").sections.bySid("SID").put(body)
```

**Python SDK**
```python
client.forms.by_id("ID").sections.by_sid("SID").put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Sections["SID"].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Sections().BySid("SID").Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}/sections/{sid}`

Delete section — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").sections.bySid("SID").delete()
```

**Python SDK**
```python
client.forms.by_id("ID").sections.by_sid("SID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Sections["SID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Sections().BySid("SID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
