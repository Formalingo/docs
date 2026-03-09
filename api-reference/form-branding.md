# Form Branding

Customize the visual appearance of a form.

## Endpoints

### `PUT` `/api/v1/forms/{id}/branding`

Update branding — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").branding.put(body)
```

**Python SDK**
```python
client.forms.by_id("ID").branding.put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Branding.PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Branding().Put(ctx, body, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
