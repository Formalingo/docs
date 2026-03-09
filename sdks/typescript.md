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
import { createClient } from "@formalingo/sdk"

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
```

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
const submission = await client.documents
  .byDocumentId("DOC_ID")
  .submissions.post({
    signers: [
      {
        role: "signer_1",
        name: "John Doe",
        email: "john@acme.com",
        prefill: { "field-uuid-1": "John Doe" },
        prefillReadonly: true,
      },
    ],
  })

console.log(submission.signers[0].link)
// → https://www.formalingo.com/d/TOKEN
```

## Error Handling

```typescript
try {
  const form = await client.forms.byFormId("non-existent").get()
} catch (error) {
  // Kiota throws typed errors with status code and message
  console.error("API error:", error)
}
```
