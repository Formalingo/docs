# Recipients

Create and manage form recipients with optional pre-filled answers.

## Endpoints

### `GET` `/api/v1/forms/{id}/recipients`

List recipients — Scopes: `read:recipients`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.get()
```

**Python SDK**
```python
client.forms.by_id("ID").recipients.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Recipients.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Recipients().Get(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/recipients`

Create recipient — Creates a recipient with an optional prefill map. — Scopes: `write:recipients`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.post(body)
```

**Python SDK**
```python
client.forms.by_id("ID").recipients.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Recipients.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Recipients().Post(ctx, body, nil)
```

### `PUT` `/api/v1/forms/{id}/recipients/{rid}`

Update recipient — Scopes: `write:recipients`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.byRid("RID").put(body)
```

**Python SDK**
```python
client.forms.by_id("ID").recipients.by_rid("RID").put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Recipients["RID"].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Recipients().ByRid("RID").Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}/recipients/{rid}`

Delete recipient — Scopes: `delete:recipients`

**TypeScript SDK**
```typescript
client.forms.byId("ID").recipients.byRid("RID").delete()
```

**Python SDK**
```python
client.forms.by_id("ID").recipients.by_rid("RID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Recipients["RID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Recipients().ByRid("RID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
