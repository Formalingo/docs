# Form Settings

### `PUT` `/api/v1/forms/{id}/settings`

 — Scopes: `write:forms`

**Request Body (all optional)**

| Name | Type | Required | Description |
|------|------|----------|-------------|

**curl**
```bash
curl -X PUT https://app.formalingo.com/api/v1/forms/FORM_ID/settings \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "display_mode": "one_at_a_time",
    "language": "en"
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

await client.forms.byFormId("FORM_ID").settings.put({
  displayMode: "one_at_a_time",
  language: "en",
})
```

**Python SDK**
```python
body = SettingsUpdateRequest()
body.display_mode = "one_at_a_time"
body.language = "en"

await client.api.v1.forms.by_form_id("FORM_ID").settings.put(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

await client.Api.V1.Forms["FORM_ID"].Settings.PutAsync(new() {
    DisplayMode = "one_at_a_time",
    Language = "en",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.SettingsUpdateRequest{
    DisplayMode: to.Ptr("one_at_a_time"),
    Language:    to.Ptr("en"),
}
err := client.Api().V1().Forms().ByFormId("FORM_ID").Settings().Put(context.Background(), body, nil)
```

### Example: enable quiz mode

```bash
curl -X PUT https://app.formalingo.com/api/v1/forms/FORM_ID/settings \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "quiz_mode": true
  }'
```

### Example: set redirect and expiry

```bash
curl -X PUT https://app.formalingo.com/api/v1/forms/FORM_ID/settings \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "redirect_url": "https://example.com/thank-you",
    "closes_at": "2026-12-31T23:59:59Z"
  }'
```
