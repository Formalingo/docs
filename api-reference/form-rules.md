# Conditional Logic

Define conditional show/hide rules for questions and sections.

## Endpoints

### `GET` `/api/v1/forms/{id}/rules`

List rules — Scopes: `read:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").rules.get()
```

**Python SDK**
```python
client.forms.by_id("ID").rules.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Rules.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Rules().Get(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/rules`

Create rule — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").rules.post(body)
```

**Python SDK**
```python
client.forms.by_id("ID").rules.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Rules.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Rules().Post(ctx, body, nil)
```

### `PUT` `/api/v1/forms/{id}/rules/{rid}`

Update rule — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").rules.byRid("RID").put(body)
```

**Python SDK**
```python
client.forms.by_id("ID").rules.by_rid("RID").put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Rules["RID"].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Rules().ByRid("RID").Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}/rules/{rid}`

Delete rule — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").rules.byRid("RID").delete()
```

**Python SDK**
```python
client.forms.by_id("ID").rules.by_rid("RID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Rules["RID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Rules().ByRid("RID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
