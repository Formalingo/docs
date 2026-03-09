# Submissions

Create signing submissions and manage signers with pre-fill support.

## Endpoints

### `GET` `/api/v1/documents/{id}/submissions`

List submissions — Scopes: `read:submissions`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.get()
```

**Python SDK**
```python
client.documents.by_id("ID").submissions.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Submissions.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Submissions().Get(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/submissions`

Create submission — Creates a signing submission with optional pre-fill data. — Scopes: `write:submissions`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.post(body)
```

**Python SDK**
```python
client.documents.by_id("ID").submissions.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Submissions.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Submissions().Post(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}/submissions/{sid}`

Delete submission — Scopes: `write:submissions`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").delete()
```

**Python SDK**
```python
client.documents.by_id("ID").submissions.by_sid("SID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Submissions["SID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Submissions().BySid("SID").Delete(ctx, nil)
```

### `GET` `/api/v1/documents/{id}/submissions/{sid}/signers`

List signers — Scopes: `read:submissions`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").signers.get()
```

**Python SDK**
```python
client.documents.by_id("ID").submissions.by_sid("SID").signers.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Submissions["SID"].Signers.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Submissions().BySid("SID").Signers().Get(ctx, nil)
```

### `PUT` `/api/v1/documents/{id}/submissions/{sid}/signers/{signerId}`

Update signer — Scopes: `write:submissions`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").signers.bySignerId("SIGNERID").put(body)
```

**Python SDK**
```python
client.documents.by_id("ID").submissions.by_sid("SID").signers.by_signer_id("SIGNERID").put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Submissions["SID"].Signers["SIGNERID"].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Submissions().BySid("SID").Signers().BySignerId("SIGNERID").Put(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}/submissions/{sid}/signers/{signerId}`

Delete signer — Scopes: `write:submissions`

**TypeScript SDK**
```typescript
client.documents.byId("ID").submissions.bySid("SID").signers.bySignerId("SIGNERID").delete()
```

**Python SDK**
```python
client.documents.by_id("ID").submissions.by_sid("SID").signers.by_signer_id("SIGNERID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].Submissions["SID"].Signers["SIGNERID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").Submissions().BySid("SID").Signers().BySignerId("SIGNERID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
