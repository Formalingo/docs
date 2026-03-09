# Pagination

List endpoints support offset-based pagination using `page` and `limit` query parameters.

## Parameters

**Query Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|

## Response

Paginated responses include a `meta` object:

```json
{
  "success": true,
  "data": [...],
  "meta": {
    "total": 143,
    "page": 2,
    "limit": 20
  }
}
```

## Example

```bash
# Get the third page of forms, 10 per page
curl "https://app.formalingo.com/api/v1/forms?page=3&limit=10" \
  -H "Authorization: Bearer af_live_YOUR_KEY"
```
