# Event Payloads

## form_submitted

```json
{
  "event": "form_submitted",
  "timestamp": "2026-02-25T10:00:00.000Z",
  "workspaceId": "workspace-uuid",
  "data": {
    "form": { "id": "form-uuid", "title": "Customer Feedback" },
    "recipient": {
      "id": "recipient-uuid",
      "label": "John Doe",
      "email": "john@example.com"
    }
  }
}
```

## document_all_complete

```json
{
  "event": "document_all_complete",
  "timestamp": "2026-02-25T10:00:00.000Z",
  "workspaceId": "workspace-uuid",
  "data": {
    "document": { "id": "doc-uuid", "title": "Service Agreement" },
    "submission": {
      "id": "submission-uuid",
      "completedPdfUrl": "https://storage.url/signed.pdf"
    }
  }
}
```

## signer_invite

```json
{
  "event": "signer_invite",
  "timestamp": "2026-02-25T10:00:00.000Z",
  "workspaceId": "workspace-uuid",
  "data": {
    "document": { "id": "doc-uuid", "title": "Service Agreement" },
    "signer": {
      "id": "signer-uuid",
      "name": "Jane Smith",
      "email": "jane@client.com",
      "role": "signer_1"
    },
    "link": "https://www.formalingo.com/d/TOKEN"
  }
}
```

## document_signer_completed

```json
{
  "event": "document_signer_completed",
  "timestamp": "2026-02-25T10:00:00.000Z",
  "workspaceId": "workspace-uuid",
  "data": {
    "document": { "id": "doc-uuid", "title": "Service Agreement" },
    "signer": {
      "id": "signer-uuid",
      "name": "Jane Smith",
      "role": "signer_1"
    }
  }
}
```
