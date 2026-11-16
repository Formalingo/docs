# Form Settings

Configure form behavior, language, and visibility.

## Endpoints

### `PUT` `/api/v1/forms/{id}/settings`

 — Scopes: `forms:write`

**TypeScript SDK**
```typescript
client.forms.byId("ID").settings.put({})
```

**Python SDK**
```python
await client.api.v1.forms.by_id(UUID("ID")).settings.put()
```

**.NET SDK**
```csharp
client.Api.V1.Forms[Guid.Parse("ID")].Settings.PutAsync()
```

**Go SDK**
```go
client.Api().V1().Forms().ById(uuid.MustParse("ID")).Settings().Put(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
