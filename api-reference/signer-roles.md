# Signer Roles

Define signer role templates for a document.

## Endpoints

### `GET` `/api/v1/documents/{id}/signer-roles`

 — Scopes: `documents:read`

**TypeScript SDK**
```typescript
client.documents.byId("ID").signerRoles.get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).signer_roles.get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].SignerRoles.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).SignerRoles().Get(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/signer-roles`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").signerRoles.post(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).signer_roles.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].SignerRoles.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).SignerRoles().Post(ctx, body, nil)
```

### `PUT` `/api/v1/documents/{id}/signer-roles/{rid}`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").signerRoles.byRid("RID").put(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).signer_roles.by_rid(UUID("RID")).put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].SignerRoles[Guid.Parse("RID")].PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).SignerRoles().ByRid(uuid.MustParse("RID")).Put(ctx, body, nil)
```

### `DELETE` `/api/v1/documents/{id}/signer-roles/{rid}`

 — Scopes: `documents:write`

**TypeScript SDK**
```typescript
client.documents.byId("ID").signerRoles.byRid("RID").delete()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).signer_roles.by_rid(UUID("RID")).delete()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].SignerRoles[Guid.Parse("RID")].DeleteAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).SignerRoles().ByRid(uuid.MustParse("RID")).Delete(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
