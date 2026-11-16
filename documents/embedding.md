# Embedding Documents

Document signing pages can be embedded on external websites using an **iframe** or the **JavaScript SDK**.
By default, document signing pages allow embedding from any domain (no whitelist required).

## Quick Start — iFrame

```html

```

Replace `SIGNER_TOKEN` with the signer's token from the submission response.

## Quick Start — JavaScript SDK

```html

```

The SDK automatically resizes the iframe to fit the document content.

## SDK Options Reference

**Formalingo.createDocument(options)**

| Name | Type | Required | Description |
|------|------|----------|-------------|

The `createDocument` method returns an object with:
- `iframe` — the HTMLIFrameElement reference
- `destroy()` — removes the iframe and cleans up event listeners

## postMessage Events

Embedded documents emit events to the parent window via `postMessage`. All events are prefixed with `formalingo.` and have this structure:

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

```javascript
window.addEventListener('message', function(event) {
  var data = event.data;
  if (!data || !data.type || data.type.indexOf('formalingo.') !== 0) return;

  switch (data.type) {
    case 'formalingo.documentSigned':
      console.log('Document signed:', data.payload);
      break;
    case 'formalingo.fieldAnswered':
      console.log('Field completed:', data.payload.completedFields + '/' + data.payload.totalFields);
      break;
    case 'formalingo.documentHeight':
      document.querySelector('iframe').style.height = data.payload.height + 'px';
      break;
  }
});
```

## Domain Whitelisting

By default, document signing pages can be embedded from **any domain**. To restrict embedding to specific domains, update the document settings:

```bash
curl -X PUT https://app.formalingo.com/api/v1/documents/DOC_ID \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "settings": {
      "embed_allowed_domains": ["example.com", "*.company.io"]
    }
  }'
```

Set `embed_allowed_domains` to `null` to re-allow embedding from any domain (the default).

## Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| Document doesn't load in iframe | Domain blocked by whitelist | Check the document's `embed_allowed_domains` in settings |
| No auto-resize | SDK script not loaded | Verify the `embed.js` script tag loads correctly |
| Console error: `Refused to display in a frame` | CSP `frame-ancestors` restriction | Add your domain to the document's allowed embed domains |
| Signing events not firing | Using old SDK version | Ensure you're loading the latest `embed.js` |
