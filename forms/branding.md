# Branding

### `PUT` `/api/v1/forms/{id}/branding`

 — Scopes: `write:forms`

**Request Body (all fields optional)**

| Name | Type | Required | Description |
|------|------|----------|-------------|

**curl**
```bash
curl -X PUT https://app.formalingo.com/api/v1/forms/FORM_ID/branding \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "primary_color": "#6366F1",
    "welcome_heading": "Welcome!",
    "welcome_subtitle": "This will only take a minute.",
    "thank_you_heading": "Thank you!",
    "thank_you_message": "We appreciate your feedback."
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

await client.forms.byFormId("FORM_ID").branding.put({
  primaryColor: "#6366F1",
  welcomeHeading: "Welcome!",
  welcomeSubtitle: "This will only take a minute.",
  thankYouHeading: "Thank you!",
  thankYouMessage: "We appreciate your feedback.",
})
```

**Python SDK**
```python
body = BrandingUpdateRequest()
body.primary_color = "#6366F1"
body.welcome_heading = "Welcome!"
body.thank_you_heading = "Thank you!"
body.thank_you_message = "We appreciate your feedback."

await client.api.v1.forms.by_form_id("FORM_ID").branding.put(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

await client.Api.V1.Forms["FORM_ID"].Branding.PutAsync(new() {
    PrimaryColor = "#6366F1",
    WelcomeHeading = "Welcome!",
    ThankYouHeading = "Thank you!",
    ThankYouMessage = "We appreciate your feedback.",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.BrandingUpdateRequest{
    PrimaryColor:    to.Ptr("#6366F1"),
    WelcomeHeading:  to.Ptr("Welcome!"),
    ThankYouHeading: to.Ptr("Thank you!"),
    ThankYouMessage: to.Ptr("We appreciate your feedback."),
}
err := client.Api().V1().Forms().ByFormId("FORM_ID").Branding().Put(context.Background(), body, nil)
```
