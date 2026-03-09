# Documents

Documents are PDFs or DOCX files with placed signature and data fields. Create a submission to send it to one or more signers.

## Document lifecycle

```
draft → sent → completed
            ↓
          expired
```

## Structure

```
Document
├── Fields (positioned on pages: signature, text, date, etc.)
├── Signer Roles (template: Client, Witness, etc.)
└── Submissions
    └── Signers (one per role, with unique signing links)
        └── Responses (field values)
```

## Available endpoints

### `GET` `/api/v1/documents`

List documents in your workspace — Scopes: `read:documents`

### `POST` `/api/v1/documents`

Upload a PDF or DOCX — Scopes: `write:documents`

### `GET` `/api/v1/documents/{id}`

Get document with fields and signer roles — Scopes: `read:documents`

### `PUT` `/api/v1/documents/{id}`

Update title or status — Scopes: `write:documents`

### `DELETE` `/api/v1/documents/{id}`

Soft-delete or permanently delete — Scopes: `delete:documents`
