# Errors

All error responses follow the same format:

```json
{
  "success": false,
  "error": "Human-readable error message",
  "hint": "Optional guidance on how to fix the issue"
}
```

## HTTP Status Codes

| Code | Meaning |
|---|---|
| `200` | Success |
| `201` | Created — resource was successfully created |
| `400` | Bad Request — validation failed or invalid input |
| `401` | Unauthorized — missing or invalid API key |
| `403` | Forbidden — API key lacks the required scope |
| `404` | Not Found — resource doesn't exist or doesn't belong to your workspace |
| `429` | Too Many Requests — rate limit exceeded |
| `500` | Internal Server Error — something went wrong on our end |

## Common Errors

### `401 Unauthorized`

```json
{ "success": false, "error": "Invalid API key" }
```

Check that your key starts with `af_live_` and hasn't been revoked.

### `403 Forbidden — missing scope`

```json
{
  "success": false,
  "error": "Forbidden",
  "hint": "This action requires the 'write:forms' scope. Add it to your API key."
}
```

Go to Settings → API Keys and add the required scope to your key.

### `400 Validation Error`

```json
{ "success": false, "error": "String must contain at least 1 character(s)" }
```

Check the request body against the API schema.

### `404 Not Found`

```json
{ "success": false, "error": "Form not found" }
```

The resource either doesn't exist or doesn't belong to your workspace.
