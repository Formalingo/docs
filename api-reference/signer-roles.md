# Signer Roles

Define signer role templates for a document.

## Endpoints

### `GET` `/api/v1/documents/{id}/signer-roles`

List signer roles — Scopes: `read:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").signerRoles.get()
```

**Python SDK**
```python
client.documents.by_id("ID").signer_roles.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].SignerRoles.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").SignerRoles().Get(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/signer-roles`

Create signer role — Scopes: `write:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").signerRoles.post(body)
```

**Python SDK**
```python
client.documents.by_id("ID").signer_roles.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].SignerRoles.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").SignerRoles().Post(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}/signer-roles/{rid}`

Delete signer role — Scopes: `write:documents`

**TypeScript SDK**
```typescript
client.documents.byId("ID").signerRoles.byRid("RID").delete()
```

**Python SDK**
```python
client.documents.by_id("ID").signer_roles.by_rid("RID").delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents["ID"].SignerRoles["RID"].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById("ID").SignerRoles().ByRid("RID").Delete(ctx, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
