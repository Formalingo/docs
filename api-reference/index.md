# API Reference

The full OpenAPI 3.1 specification is available at:

```
GET /api/v1/openapi.json
```

You can import this into:

- **Postman** — Import → URL → paste `https://app.formalingo.com/api/v1/openapi.json`
- **Insomnia** — Import → From URL
- **Swagger UI** — Paste the URL into [editor.swagger.io](https://editor.swagger.io)
- **Speakeasy / Stainless** — For SDK generation

## Auto-generated reference

The API reference pages in this docs site are auto-generated from the OpenAPI spec. Run the generation script to update them:

```bash
npm run docs:generate
```

This updates all pages in `/content/docs/api-reference/` without touching handwritten guide pages.

## Quick reference

### Authentication

```
Authorization: Bearer af_live_YOUR_KEY
```

### Base URL

```
https://app.formalingo.com/api/v1
```

### Forms

| Method | Path | Permissions |
|---|---|---|
| GET | /api/v1/forms | forms:read |
| POST | /api/v1/forms | forms:write |
| GET | `/api/v1/forms/{id}` | forms:read |
| PUT | `/api/v1/forms/{id}` | forms:write |
| DELETE | `/api/v1/forms/{id}` | forms:delete |
| POST | `/api/v1/forms/{id}/publish` | forms:write, forms:publish |
| GET | `/api/v1/forms/{id}/sections` | forms:read |
| POST | `/api/v1/forms/{id}/sections` | forms:write |
| GET | `/api/v1/forms/{id}/questions` | forms:read |
| POST | `/api/v1/forms/{id}/questions` | forms:write |
| GET | `/api/v1/forms/{id}/recipients` | recipients:read |
| POST | `/api/v1/forms/{id}/recipients` | recipients:write |
| DELETE | `/api/v1/forms/{id}/recipients/{rid}` | recipients:write |

### Documents

| Method | Path | Permissions |
|---|---|---|
| GET | /api/v1/documents | documents:read |
| POST | /api/v1/documents | documents:write |
| GET | `/api/v1/documents/{id}` | documents:read |
| PUT | `/api/v1/documents/{id}` | documents:write |
| DELETE | `/api/v1/documents/{id}` | documents:delete |
| POST | `/api/v1/documents/{id}/publish` | documents:write, documents:publish |
| GET | `/api/v1/documents/{id}/fields` | documents:read |
| POST | `/api/v1/documents/{id}/fields` | documents:write |
| GET | `/api/v1/documents/{id}/signer-roles` | documents:read |
| POST | `/api/v1/documents/{id}/signer-roles` | documents:write |
| GET | `/api/v1/documents/{id}/submissions` | submissions:read |
| POST | `/api/v1/documents/{id}/submissions` | submissions:write |
