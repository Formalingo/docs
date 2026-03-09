# Form Settings

Configure form behavior, language, and visibility.

## Endpoints

### `PUT` `/api/v1/forms/{id}/settings`

Update settings — Scopes: `write:forms`

**TypeScript SDK**
```typescript
client.forms.byId("ID").settings.put(body)
```

**Python SDK**
```python
client.forms.by_id("ID").settings.put(body)
```

**.NET SDK**
```csharp
client.Api.V1.Forms["ID"].Settings.PutAsync(body)
```

**Go SDK**
```go
client.Api().V1().Forms().ById("ID").Settings().Put(ctx, body, nil)
```

---

*This page is auto-generated from the OpenAPI spec. Run `npm run docs:generate` to update.*
