# Forms

Manage forms — create, update, publish, and delete forms in your workspace.

## Endpoints

### `GET` `/api/v1/forms`

List forms — Returns all forms in the workspace, sorted by most recently updated. — Scopes: `read:forms`

**TypeScript SDK**
```typescript
client.forms.get()
```

**Python SDK**
```python
client.forms.get()
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

Create a form — Creates a new form in draft status. — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.post(body)
```

**Python SDK**
```python
client.forms.post(body)
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

Get a form — Returns the form with all sections and questions. — Scopes: `read:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").get()
```

**Python SDK**
```python
client.forms.by_id("ID").get()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].GetAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Get(ctx, nil)
```

### `PUT` `/api/v1/forms/{id}`

Update a form — Update title, description, or status. — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").put(body)
```

**Python SDK**
```python
client.forms.by_id("ID").put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Put(ctx, body, nil)
```

### `DELETE` `/api/v1/forms/{id}`

Delete a form — Soft-deletes by default. Add ?permanent=true to permanently delete. — Scopes: `delete:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").delete()
```

**Python SDK**
```python
client.forms.by_id("ID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
