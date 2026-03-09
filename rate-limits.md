# Rate Limits

## How it works

Rate limits are enforced per API key using a fixed-window counter. Each minute window resets independently.

## Response headers

Every API response includes these headers:

| Header | Description |
|---|---|
| `X-RateLimit-Limit` | Maximum requests allowed in the window |
| `X-RateLimit-Remaining` | Requests remaining in the current window |
| `X-RateLimit-Reset` | Unix timestamp when the window resets |

## 429 Response

When the limit is exceeded:

```json
{
  "success": false,
  "error": "Too Many Requests",
  "hint": "Rate limit of 100 requests per minute exceeded. Retry after 2026-02-25T12:00:00.000Z."
}
```

The `Retry-After` header contains the number of seconds until you can retry.

## Tips

- Batch operations where possible (e.g. `POST /fields/bulk` instead of many individual calls)
- Implement exponential backoff when you receive a 429
- Monitor `X-RateLimit-Remaining` and slow down proactively
