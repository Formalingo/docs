# TypeScript SDK

The `@formalingo/sdk` package provides a type-safe client for the Formalingo API, generated with [Microsoft Kiota](https://learn.microsoft.com/en-us/openapi/kiota/).

## Installation

```bash
npm install @formalingo/sdk @microsoft/kiota-abstractions
```

Or with yarn:

```bash
yarn add @formalingo/sdk @microsoft/kiota-abstractions
```

## Setup

```typescript
import {
  createBulkRecipients,
  createClient,
  createDocumentSubmission,
} from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")
```

To use a custom base URL (e.g. for local development):

```typescript
const client = createClient("af_live_YOUR_KEY", "http://localhost:3000")
```

## Forms

### List forms

```typescript
const forms = await client.forms.get()
```

### Create a form

```typescript
const form = await client.forms.post({
  title: "Customer Satisfaction Survey",
  description: "Help us improve our service.",
})
```

### Get a form

```typescript
const form = await client.forms.byFormId("FORM_ID").get()
```

### Update a form

```typescript
const form = await client.forms.byFormId("FORM_ID").put({
  status: "published",
})
```

### Delete a form

```typescript
await client.forms.byFormId("FORM_ID").delete()
```

## Sections

```typescript
// List sections
const sections = await client.forms.byFormId("FORM_ID").sections.get()

// Create a section
const section = await client.forms.byFormId("FORM_ID").sections.post({
  title: "Personal Information",
})
```

## Questions

```typescript
// Create a question
const question = await client.forms.byFormId("FORM_ID").questions.post({
  questionText: "What is your full name?",
  type: "short_text",
  isRequired: true,
})

// Create a multiple choice question
const mcq = await client.forms.byFormId("FORM_ID").questions.post({
  questionText: "How did you hear about us?",
  type: "mcq",
  options: {
    choices: ["Social media", "Friend", "Search engine", "Other"],
  },
})
```

## Recipients

```typescript
// Create a recipient with pre-fill
const recipient = await client.forms.byFormId("FORM_ID").recipients.post({
  label: "John Doe",
  email: "john@acme.com",
  prefill: {
    "question-uuid-1": "John Doe",
    "question-uuid-2": "Acme Corp",
  },
})

console.log(recipient.link)
// → https://www.formalingo.com/f/TOKEN

// Bulk create with one retry-safe key for the whole request
const recipients = await createBulkRecipients(
  client,
  "00000000-0000-0000-0000-000000000001",
  {
    confirmBulk: true,
    recipients: [
      { label: "Alice", email: "alice@example.com" },
      { label: "Bob", phone: "+1 202-555-0100" },
    ],
  },
  "recipient-bulk-create-7f3f",
)
```

On `idempotency_request_in_progress`, retry the exact body with the same key. A different body returns `idempotency_key_conflict`; recipient erasure returns `idempotency_replay_unavailable`.

## Branding

```typescript
await client.forms.byFormId("FORM_ID").branding.put({
  primaryColor: "#6366F1",
  welcomeHeading: "Welcome!",
  thankYouHeading: "Thank you!",
  thankYouMessage: "We appreciate your feedback.",
})
```

## Documents

```typescript
// List documents
const docs = await client.documents.get()

// Create a submission with signers
const submission = await createDocumentSubmission(
  client,
  "00000000-0000-0000-0000-000000000001",
  {
    signers: [
      {
        role: "signer_1",
        name: "John Doe",
        email: "john@acme.com",
        prefill: { "field-uuid-1": "John Doe" },
        prefillReadonly: true,
      },
    ],
  },
  "document-create-7f3f",
)

console.log(submission.signers?.[0]?.link)
// → https://www.formalingo.com/d/TOKEN
```

Import `createDocumentSubmission` alongside `createClient`. The required idempotency key makes retries safe, and the helper unwraps the API envelope so `submission.signers` comes from `response.data.signers`.

### Supporting attachments

Create required signer upload requests through existing document field CRUD;
they stay out of the rendered document and signed PDF. Supporting requests omit
page coordinates and default to required. Set `isRequired` to false for an
optional upload. Ordinary placed fields still require positioning. Use existing
signer role keys from your document; the example edits between `signer_1` and
`signer_2`.

```typescript
const fields = client.documents.byId(documentId).fields

const created = await fields.post({
  id: crypto.randomUUID(), // Keep this UUID when retrying the same request.
  label: "Proof of address",
  type: "file_upload",
  assigneeRole: "signer_1",
  options: { supportingAttachment: true },
}, {})
const fieldId = created!.data!.id!

// A label-only edit preserves requiredness.
await fields.byFid(fieldId).put({ label: "Updated proof" }, {})
await fields.byFid(fieldId).put({
  isRequired: false,
  assigneeRole: "signer_2",
}, {})
const definitions = await fields.get()
await fields.byFid(fieldId).delete()
```

Pass the explicit empty request configuration `{}` for body calls as shown.
The typed `supportingAttachment` property serializes as `supporting_attachment`.

Use a stable optional UUID `id` when retrying the same creation request. A
request's supporting/ordinary kind cannot be converted after creation. Draft
changes affect future published submissions; existing submissions keep their
pinned requirements.

API keys need `documents:write` to create/edit/delete requests and
`documents:read` to read definitions. Actual uploaded-file values/downloads need
separate `submissions:download_files` permission. Completion webhooks use the
existing authenticated file descriptors; see [file retrieval](/docs/webhooks/events#supporting-files-in-completion-webhooks).

## Error Handling

```typescript
try {
  const form = await client.forms.byFormId("non-existent").get()
} catch (error) {
  // Kiota throws typed errors with status code and message
  console.error("API error:", error)
}
```
