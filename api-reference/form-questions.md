# Form Questions

Add and manage questions within a form.

## Endpoints

### `GET` `/api/v1/forms/{id}/questions`

List questions — Scopes: `read:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").questions.get()
```

**Python SDK**
```python
client.forms.by_id("ID").questions.get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Questions.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Questions().Get(ctx, nil)
```

### `POST` `/api/v1/forms/{id}/questions`

Create question — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").questions.post(body)
```

**Python SDK**
```python
client.forms.by_id("ID").questions.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Questions.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Questions().Post(ctx, body, nil)
```

### `PUT` `/api/v1/forms/{id}/questions/{qid}`

Update question — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").questions.byQid("QID").put(body)
```

**Python SDK**
```python
client.forms.by_id("ID").questions.by_qid("QID").put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Questions["QID"].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Questions().ByQid("QID").Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}/questions/{qid}`

Delete question — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").questions.byQid("QID").delete()
```

**Python SDK**
```python
client.forms.by_id("ID").questions.by_qid("QID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Questions["QID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Questions().ByQid("QID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
