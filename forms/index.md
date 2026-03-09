# Forms

Forms are the core resource in Formalingo. A form contains sections, questions, branding, settings, and recipients.

## Form lifecycle

```
draft → published → archived
```

- **draft** — visible only to workspace members, not accessible via public link
- **published** — accessible to recipients via their unique link
- **archived** — closed for new responses

## Structure

```
Form
├── Sections (optional groupings)
│   └── Questions
├── Questions (not in a section)
├── Recipients (with pre-filled answers)
├── Rules (conditional logic)
├── Branding (colors, logo, messaging)
└── Settings (navigation, language, visibility)
```

## Available endpoints

### `GET` `/api/v1/forms`

List all forms in your workspace — Scopes: `read:forms`

### `POST` `/api/v1/forms`

Create a new form — Scopes: `write:forms`

### `GET` `/api/v1/forms/{id}`

Get a form with its sections and questions — Scopes: `read:forms`

### `PUT` `/api/v1/forms/{id}`

Update form title, description, or status — Scopes: `write:forms`

### `DELETE` `/api/v1/forms/{id}`

Soft-delete or permanently delete a form — Scopes: `delete:forms`
