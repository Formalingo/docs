# Form Branding

Customize the visual appearance of a form.

## Endpoints

### `PUT` `/api/v1/forms/{id}/branding`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").branding.put({})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).branding.put()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Branding.PutAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Branding().Put(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
