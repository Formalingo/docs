# Embedding Forms

Formalingo forms can be embedded on external websites using two methods:
a simple **iframe snippet** or the **JavaScript SDK** with auto-resize and event callbacks.

## Quick Start — iFrame

The simplest way to embed a public form:

```html

```

Replace `PUBLIC_TOKEN` with your form's public token (found in the form editor Settings tab or the form detail page).

## Quick Start — JavaScript SDK

For richer control including auto-resize and event callbacks, use the embed SDK:

```html

```

The SDK automatically resizes the iframe to fit the form content, eliminating scrollbars.

## SDK Options Reference

**Formalingo.createForm(options)**

| Name | Type | Required | Description |
|------|------|----------|-------------|

The `createForm` method returns an object with:
- `iframe` — the HTMLIFrameElement reference
- `destroy()` — removes the iframe and cleans up event listeners (useful for SPAs)

## postMessage Events

Embedded forms emit events to the parent window via `postMessage`. All events are prefixed with `formalingo.` and have this structure:

```json
{
  "type": "formalingo.eventName",
  "payload": { ... }
}
```

### Event Reference

**Events**

| Name | Type | Required | Description |
|------|------|----------|-------------|

### Listening to Events Without the SDK

If you use the iframe embed directly (without the SDK), you can listen for events manually:

```javascript
window.addEventListener('message', function(event) {
  var data = event.data;
  if (!data || !data.type || data.type.indexOf('formalingo.') !== 0) return;

  switch (data.type) {
    case 'formalingo.formSubmitted':
      console.log('Form submitted:', data.payload);
      break;
    case 'formalingo.formHeight':
      // Auto-resize the iframe
      document.querySelector('iframe').style.height = data.payload.height + 'px';
      break;
  }
});
```

## Domain Whitelisting

Restrict which websites can embed your form for security. When enabled, browsers will block embedding from unlisted domains.

### Via Dashboard

1. Open the form editor **Settings** tab
2. Scroll to the **Embed** section
3. Toggle **Domain Whitelisting** on
4. Add allowed domains (e.g., `example.com`, `*.company.io`)

### Via API

**curl**
```bash
curl -X PUT https://app.formalingo.com/api/v1/forms/FORM_ID/settings \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "embed_allowed_domains": ["example.com", "*.company.io"]
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

await client.forms.byFormId("FORM_ID").settings.put({
    embedAllowedDomains: ["example.com", "*.company.io"],
})
```

**Python SDK**
```python
body = SettingsUpdateRequest()
body.embed_allowed_domains = ["example.com", "*.company.io"]

await client.api.v1.forms.by_form_id("FORM_ID").settings.put(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

await client.Api.V1.Forms["FORM_ID"].Settings.PutAsync(new() {
    EmbedAllowedDomains = new() { "example.com", "*.company.io" },
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.SettingsUpdateRequest{
    EmbedAllowedDomains: []string{"example.com", "*.company.io"},
}
err := client.Api().V1().Forms().ByFormId("FORM_ID").Settings().Put(context.Background(), body, nil)
```

Set to `null` to disable whitelisting and allow embedding from any domain:

**curl**
```bash
curl -X PUT https://app.formalingo.com/api/v1/forms/FORM_ID/settings \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "embed_allowed_domains": null
  }'
```

**TypeScript SDK**
```typescript
await client.forms.byFormId("FORM_ID").settings.put({
  embedAllowedDomains: null,
})
```

**Python SDK**
```python
body = SettingsUpdateRequest()
body.embed_allowed_domains = None

await client.api.v1.forms.by_form_id("FORM_ID").settings.put(body)
```

**.NET SDK**
```csharp
await client.Api.V1.Forms["FORM_ID"].Settings.PutAsync(new() {
    EmbedAllowedDomains = null,
});
```

**Go SDK**
```go
body := &models.SettingsUpdateRequest{
    EmbedAllowedDomains: nil,
}
err := client.Api().V1().Forms().ByFormId("FORM_ID").Settings().Put(context.Background(), body, nil)
```

## Embedding Private Forms

Private forms (shared via individual recipient links) can also be embedded using the recipient token URL:

```html

```

Or with the SDK:

```javascript
Formalingo.createForm({
  container: '#my-form',
  formUrl: 'https://www.formalingo.com/f/RECIPIENT_TOKEN'
});
```

All postMessage events work identically for private forms. Password-protected forms will display the password gate inside the iframe.

## Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| Form doesn't load in iframe | Domain blocked by whitelist | Check the form's `embed_allowed_domains` setting |
| No auto-resize | SDK script not loaded | Verify the `embed.js` script tag loads correctly |
| Redirect doesn't work when embedded | Embedded forms emit `formRedirect` instead of navigating | Use the `onRedirect` callback to handle redirects in the parent page |
| Console error: `Refused to display in a frame` | CSP `frame-ancestors` restriction | Add your domain to the form's allowed embed domains |
