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

| Method | Path | Scope |
|---|---|---|
| GET | /api/v1/forms | read:forms |
| POST | /api/v1/forms | write:forms |
| GET | `/api/v1/forms/{id}` | read:forms |
| PUT | `/api/v1/forms/{id}` | write:forms |
| DELETE | `/api/v1/forms/{id}` | delete:forms |
| GET | `/api/v1/forms/{id}/sections` | read:forms |
| POST | `/api/v1/forms/{id}/sections` | write:forms |
| GET | `/api/v1/forms/{id}/questions` | read:forms |
| POST | `/api/v1/forms/{id}/questions` | write:forms |
| GET | `/api/v1/forms/{id}/recipients` | read:recipients |
| POST | `/api/v1/forms/{id}/recipients` | write:recipients |
| DELETE | `/api/v1/forms/{id}/recipients/{rid}` | delete:recipients |

### Documents

| Method | Path | Scope |
|---|---|---|
| GET | /api/v1/documents | read:documents |
| POST | /api/v1/documents | write:documents |
| GET | `/api/v1/documents/{id}` | read:documents |
| PUT | `/api/v1/documents/{id}` | write:documents |
| DELETE | `/api/v1/documents/{id}` | delete:documents |
| GET | `/api/v1/documents/{id}/fields` | read:documents |
| POST | `/api/v1/documents/{id}/fields` | write:documents |
| GET | `/api/v1/documents/{id}/signer-roles` | read:documents |
| POST | `/api/v1/documents/{id}/signer-roles` | write:documents |
| GET | `/api/v1/documents/{id}/submissions` | read:submissions |
| POST | `/api/v1/documents/{id}/submissions` | write:submissions |
