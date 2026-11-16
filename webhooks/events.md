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
  "event": "document.all_complete",
  "eventType": "document_all_complete",
  "timestamp": "2026-02-25T10:00:00.000Z",
  "workspaceId": "workspace-uuid",
  "document": { "id": "doc-uuid", "title": "Service Agreement" },
  "submission": {
    "id": "submission-uuid",
    "completedPdfUrl": "https://app.formalingo.com/api/v1/files/file-uuid/download",
    "signedPdf": {
      "fileId": "file-uuid",
      "name": "Service Agreement.pdf",
      "contentType": "application/pdf",
      "sizeBytes": 65536,
      "downloadUrl": "https://app.formalingo.com/api/v1/files/file-uuid/download",
      "downloadMethod": "GET",
      "authorization": "Bearer Formalingo API key"
    }
  },
  "signers": [
    {
      "id": "signer-uuid",
      "name": "Jane Smith",
      "email": "jane@client.com",
      "role": "signer_1",
      "answers": []
    }
  ]
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
  "event": "document.signer_completed",
  "eventType": "document_signer_completed",
  "timestamp": "2026-02-25T10:00:00.000Z",
  "workspaceId": "workspace-uuid",
  "document": { "id": "doc-uuid", "title": "Service Agreement" },
  "signer": {
    "id": "signer-uuid",
    "name": "Jane Smith",
    "role": "signer_1"
  },
  "answers": []
}
```

## Supporting files in completion webhooks

Supporting attachments requested in the document editor are included in the
configured completion webhook even though they stay out of the rendered document
and signed PDF. In the default full-payload webhook:

- `document.signer_completed` (`eventType: document_signer_completed`) includes
  the completed signer's uploads in `answers[].value`.
- `document.all_complete` (`eventType: document_all_complete`) includes each
  signer's uploads in `signers[].answers[].value`.

Each file answer uses the existing descriptor shape. This is an answer fragment,
not a complete event envelope:

```json
{
  "fieldId": "attachment-request-uuid",
  "label": "Proof of address",
  "type": "file_upload",
  "value": {
    "fileId": "file-uuid",
    "name": "proof-of-address.pdf",
    "contentType": "application/pdf",
    "sizeBytes": 482193,
    "downloadUrl": "https://app.formalingo.com/api/v1/files/file-uuid/download",
    "downloadMethod": "GET",
    "authorization": "Bearer Formalingo API key"
  }
}
```

The JSON contains a descriptor, not the file bytes. Call `downloadUrl` with
`Authorization: Bearer YOUR_API_KEY`; the key must explicitly have
`submissions:download_files`. The response's `data.downloadUrl` is a short-lived
storage URL for downloading the original file. This uses the same download flow
as signed-PDF descriptors, with the upload-download permission instead of
`submissions:download_pdf`.

Custom webhook templates must include the answers they need. Supporting answers
are excluded from other delivery channels and signer copies. Interactive access
is limited to the document creator; explicitly permitted workspace API keys are
trusted integration credentials.
