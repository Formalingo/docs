# Go SDK

The `sdk-go` module provides a type-safe client for the Formalingo API, generated with [Microsoft Kiota](https://learn.microsoft.com/en-us/openapi/kiota/).

## Installation

```bash
go get github.com/Formalingo/sdk-go
```

## Setup

```go
import formalingo "github.com/Formalingo/sdk-go"

client, err := formalingo.NewClient("af_live_YOUR_KEY")
```

The `NewClient` helper wires up authentication and the HTTP adapter automatically. You can optionally pass a custom base URL:

```go
client, err := formalingo.NewClient("af_live_YOUR_KEY", "http://localhost:3000")
```

> **Note:** The Kiota-generated client includes the full API path: `client.Api().V1().Forms()`, `client.Api().V1().Documents()`, etc.

## Forms

### List forms

```go
ctx := context.Background()
forms, err := client.Api().V1().Forms().Get(ctx, nil)
```

### Create a form

```go
body := &models.CreateFormBody{
    Title:       to.Ptr("Customer Satisfaction Survey"),
    Description: to.Ptr("Help us improve our service."),
}

form, err := client.Api().V1().Forms().Post(ctx, body, nil)
```

### Get a form

```go
form, err := client.Api().V1().Forms().ByFormId("FORM_ID").Get(ctx, nil)
```

### Update a form

```go
body := &models.UpdateFormBody{
    Status: to.Ptr("published"),
}

form, err := client.Api().V1().Forms().ByFormId("FORM_ID").Put(ctx, body, nil)
```

### Delete a form

```go
err := client.Api().V1().Forms().ByFormId("FORM_ID").Delete(ctx, nil)
```

## Sections

```go
// List sections
sections, err := client.Api().V1().Forms().ByFormId("FORM_ID").Sections().Get(ctx, nil)

// Create a section
body := &models.CreateSectionBody{
    Title: to.Ptr("Personal Information"),
}
section, err := client.Api().V1().Forms().ByFormId("FORM_ID").Sections().Post(ctx, body, nil)
```

## Questions

```go
// Create a question
body := &models.CreateQuestionBody{
    QuestionText: to.Ptr("What is your full name?"),
    Type:         to.Ptr("short_text"),
    IsRequired:   to.Ptr(true),
}
question, err := client.Api().V1().Forms().ByFormId("FORM_ID").Questions().Post(ctx, body, nil)
```

## Recipients

```go
// Create a recipient with pre-fill
body := &models.CreateRecipientBody{
    Label: to.Ptr("John Doe"),
    Email: to.Ptr("john@acme.com"),
    Prefill: map[string]string{
        "question-uuid-1": "John Doe",
        "question-uuid-2": "Acme Corp",
    },
}
recipient, err := client.Api().V1().Forms().ByFormId("FORM_ID").Recipients().Post(ctx, body, nil)
fmt.Println(recipient.GetLink())
// → https://www.formalingo.com/f/TOKEN

// Import sdkapi "github.com/Formalingo/sdk-go/client/api",
// abstractions "github.com/microsoft/kiota-abstractions-go", and "github.com/google/uuid".
// Bulk create with one caller-owned retry key for the exact request
bulkRecipient := sdkapi.NewV1FormsItemRecipientsBulkPostRequestBody_recipients()
label, email := "Alice", "alice@example.com"
bulkRecipient.SetLabel(&label)
bulkRecipient.SetEmail(&email)
bulkBody := sdkapi.NewV1FormsItemRecipientsBulkPostRequestBody()
confirmBulk := true
bulkBody.SetConfirmBulk(&confirmBulk)
bulkBody.SetRecipients(
    []sdkapi.V1FormsItemRecipientsBulkPostRequestBody_recipientsable{
        bulkRecipient,
    },
)
headers := abstractions.NewRequestHeaders()
headers.Add("Idempotency-Key", "recipient-bulk-create-7f3f")
configuration := &abstractions.RequestConfiguration[abstractions.DefaultQueryParameters]{Headers: headers}
response, err := client.Api().V1().Forms().ById(
    uuid.MustParse("00000000-0000-0000-0000-000000000001"),
).Recipients().Bulk().Post(ctx, bulkBody, configuration)
recipients := response.GetData()
```

The idempotency key is required. Reuse it only when retrying the exact same serialized request body.
On `idempotency_request_in_progress`, retry the exact body with the same key. A different body returns `idempotency_key_conflict`; recipient erasure returns `idempotency_replay_unavailable`.

## Branding

```go
body := &models.BrandingUpdateRequest{
    PrimaryColor:    to.Ptr("#6366F1"),
    WelcomeHeading:  to.Ptr("Welcome!"),
    ThankYouHeading: to.Ptr("Thank you!"),
    ThankYouMessage: to.Ptr("We appreciate your feedback."),
}
err := client.Api().V1().Forms().ByFormId("FORM_ID").Branding().Put(ctx, body, nil)
```

## Documents

```go
import (
    formalingo "github.com/Formalingo/sdk-go"
    "github.com/Formalingo/sdk-go/client/models"
    "github.com/google/uuid"
    abstractions "github.com/microsoft/kiota-abstractions-go"
)

// List documents
docs, err := client.Api().V1().Documents().Get(ctx, nil)

// Create a submission with signers
prefill := models.NewSignerInput_prefill()
prefill.SetAdditionalData(map[string]any{"field-uuid-1": "John Doe"})
signer := models.NewSignerInput()
role, name, email, readonly := "signer_1", "John Doe", "john@acme.com", true
signer.SetRole(&role)
signer.SetName(&name)
signer.SetEmail(&email)
signer.SetPrefill(prefill)
signer.SetPrefillReadonly(&readonly)
body := models.NewCreateSubmissionBody()
body.SetSigners([]models.SignerInputable{signer})

headers := abstractions.NewRequestHeaders()
headers.Add("Idempotency-Key", "document-create-7f3f")
configuration := &abstractions.RequestConfiguration[abstractions.DefaultQueryParameters]{Headers: headers}
response, err := client.Api().V1().Documents().ById(
    uuid.MustParse("00000000-0000-0000-0000-000000000001"),
).Submissions().Post(ctx, body, configuration)
submission := response.GetData()
fmt.Println(*submission.GetSigners()[0].GetLink())
// → https://www.formalingo.com/d/TOKEN
```

The required idempotency key makes retries safe. The generated request builder
returns an API envelope; signing links are under `response.GetData().GetSigners()`.

### Supporting attachments

Create required signer upload requests through existing document field CRUD;
they stay out of the rendered document and signed PDF. Supporting requests omit
page coordinates and default to required. Set `isRequired` to false for an
optional upload. Ordinary placed fields still require positioning. Use existing
signer role keys from your document; the example edits between `signer_1` and
`signer_2`.

```go
import (
    "context"
    "github.com/Formalingo/sdk-go/client/api"
    "github.com/Formalingo/sdk-go/client/models"
    "github.com/google/uuid"
)

ctx := context.Background()
fields := client.Api().V1().Documents().ById(uuid.MustParse(documentID)).Fields()
body := models.NewCreateFieldBody()
label, role := "Proof of address", "signer_1"
fieldType, supporting := models.FILE_UPLOAD_CREATEFIELDBODY_TYPE, true
body.SetLabel(&label)
body.SetAssigneeRole(&role)
body.SetTypeEscaped(&fieldType)
options := models.NewCreateFieldBody_options()
options.SetSupportingAttachment(&supporting)
body.SetOptions(options)
created, err := fields.Post(ctx, body, nil)
if err != nil { return err }
fieldID := *created.GetData().GetId()

// A label-only edit preserves requiredness.
edit := api.NewV1DocumentsItemFieldsItemWithFPutRequestBody()
updatedLabel := "Updated proof"
edit.SetLabel(&updatedLabel)
if _, err = fields.ByFid(fieldID).Put(ctx, edit, nil); err != nil { return err }
optional, secondRole := false, "signer_2"
edit = api.NewV1DocumentsItemFieldsItemWithFPutRequestBody()
edit.SetIsRequired(&optional)
edit.SetAssigneeRole(&secondRole)
if _, err = fields.ByFid(fieldID).Put(ctx, edit, nil); err != nil { return err }
definitions, err := fields.Get(ctx, nil)
if err != nil { return err }
_ = definitions
_, err = fields.ByFid(fieldID).Delete(ctx, nil)
if err != nil { return err }
```

Use a stable optional UUID `id` when retrying the same creation request. A
request's supporting/ordinary kind cannot be converted after creation. Draft
changes affect future published submissions; existing submissions keep their
pinned requirements.

API keys need `documents:write` to create/edit/delete requests and
`documents:read` to read definitions. Actual uploaded-file values/downloads need
separate `submissions:download_files` permission. Completion webhooks use the
existing authenticated file descriptors; see [file retrieval](/docs/webhooks/events#supporting-files-in-completion-webhooks).

## Error Handling

```go
form, err := client.Api().V1().Forms().ByFormId("non-existent").Get(ctx, nil)
if err != nil {
    fmt.Fprintf(os.Stderr, "API error: %v\n", err)
}
```
