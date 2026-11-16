# .NET SDK

The `Formalingo.Sdk` package provides a type-safe client for the Formalingo API, generated with [Microsoft Kiota](https://learn.microsoft.com/en-us/openapi/kiota/).

## Installation

```bash
dotnet add package Formalingo.Sdk
```

## Setup

```csharp
using Formalingo.Sdk;
using Formalingo.Sdk.Generated.Models;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");
```

To use a custom base URL (e.g. for local development):

```csharp
var client = FormalingoClientFactory.CreateClient(
    "af_live_YOUR_KEY",
    "http://localhost:3000"
);
```

## Forms

### List forms

```csharp
var forms = await client.Api.V1.Forms.GetAsync();
```

### Create a form

```csharp
var form = await client.Api.V1.Forms.PostAsync(new() {
    Title = "Customer Satisfaction Survey",
    Description = "Help us improve our service.",
});
```

### Get a form

```csharp
var form = await client.Api.V1.Forms["FORM_ID"].GetAsync();
```

### Update a form

```csharp
var form = await client.Api.V1.Forms["FORM_ID"].PutAsync(new() {
    Status = "published",
});
```

### Delete a form

```csharp
await client.Api.V1.Forms["FORM_ID"].DeleteAsync();
```

## Sections

```csharp
// List sections
var sections = await client.Api.V1.Forms["FORM_ID"].Sections.GetAsync();

// Create a section
var section = await client.Api.V1.Forms["FORM_ID"].Sections.PostAsync(new() {
    Title = "Personal Information",
});
```

## Questions

```csharp
// Create a question
var question = await client.Api.V1.Forms["FORM_ID"].Questions.PostAsync(new() {
    QuestionText = "What is your full name?",
    Type = "short_text",
    IsRequired = true,
});

// Create a multiple choice question
var mcq = await client.Api.V1.Forms["FORM_ID"].Questions.PostAsync(new() {
    QuestionText = "How did you hear about us?",
    Type = "mcq",
    Options = new() {
        Choices = new List<string> { "Social media", "Friend", "Search engine", "Other" },
    },
});
```

## Recipients

```csharp
using Formalingo.Sdk.Generated.Api.V1.Forms.Item.Recipients.Bulk;

// Create a recipient with pre-fill
var recipient = await client.Api.V1.Forms["FORM_ID"].Recipients.PostAsync(new() {
    Label = "John Doe",
    Email = "john@acme.com",
    Prefill = new Dictionary<string, string> {
        ["question-uuid-1"] = "John Doe",
        ["question-uuid-2"] = "Acme Corp",
    },
});

Console.WriteLine(recipient.Link);
// → https://www.formalingo.com/f/TOKEN

// Bulk create with one caller-owned retry key for the exact request
var bulkBody = new BulkPostRequestBody {
    ConfirmBulk = true,
    Recipients = [
        new BulkPostRequestBody_recipients {
            Label = "Alice",
            Email = "alice@example.com",
        },
    ],
};
var response = await client.Api.V1.Forms[
    Guid.Parse("00000000-0000-0000-0000-000000000001")
].Recipients.Bulk.PostAsync(bulkBody,
    configuration => configuration.Headers.Add("Idempotency-Key", "recipient-bulk-create-7f3f"));
var recipients = response!.Data;
```

The idempotency key is required. Reuse it only when retrying the exact same serialized request body.
On `idempotency_request_in_progress`, retry the exact body with the same key. A different body returns `idempotency_key_conflict`; recipient erasure returns `idempotency_replay_unavailable`.

## Branding

```csharp
await client.Api.V1.Forms["FORM_ID"].Branding.PutAsync(new() {
    PrimaryColor = "#6366F1",
    WelcomeHeading = "Welcome!",
    ThankYouHeading = "Thank you!",
    ThankYouMessage = "We appreciate your feedback.",
});
```

## Documents

```csharp
// List documents
var docs = await client.Api.V1.Documents.GetAsync();

// Create a submission with signers
var body = new CreateSubmissionBody {
    Signers = [
        new SignerInput {
            Role = "signer_1",
            Name = "John Doe",
            Email = "john@acme.com",
            Prefill = new SignerInput_prefill {
                AdditionalData = new Dictionary<string, object> {
                    ["field-uuid-1"] = "John Doe",
                },
            },
            PrefillReadonly = true,
        },
    ],
};
var response = await client.Api.V1.Documents[
    Guid.Parse("00000000-0000-0000-0000-000000000001")
].Submissions.PostAsync(body,
    configuration => configuration.Headers.Add("Idempotency-Key", "document-create-7f3f"));
var submission = response!.Data;

Console.WriteLine(submission.Signers?[0].Link);
// → https://www.formalingo.com/d/TOKEN
```

The required idempotency key makes retries safe. The generated request builder
returns an API envelope; signing links are under `response.Data.Signers`.

### Supporting attachments

Create required signer upload requests through existing document field CRUD;
they stay out of the rendered document and signed PDF. Supporting requests omit
page coordinates and default to required. Set `isRequired` to false for an
optional upload. Ordinary placed fields still require positioning. Use existing
signer role keys from your document; the example edits between `signer_1` and
`signer_2`.

```csharp
using Formalingo.Sdk.Generated.Models;
using Formalingo.Sdk.Generated.Api.V1.Documents.Item.Fields.Item;

var fields = client.Api.V1.Documents[Guid.Parse(documentId)].Fields;
var created = await fields.PostAsync(new CreateFieldBody {
    Label = "Proof of address",
    Type = CreateFieldBody_type.File_upload,
    AssigneeRole = "signer_1",
    Options = new CreateFieldBody_options { SupportingAttachment = true },
});
var fieldId = created!.Data!.Id!.Value;

// A label-only edit preserves requiredness.
await fields[fieldId].PutAsync(new WithFPutRequestBody { Label = "Updated proof" });
await fields[fieldId].PutAsync(new WithFPutRequestBody {
    IsRequired = false,
    AssigneeRole = "signer_2",
});
var definitions = await fields.GetAsync();
await fields[fieldId].DeleteAsync();
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

```csharp
try {
    var form = await client.Api.V1.Forms["non-existent"].GetAsync();
} catch (ApiException ex) {
    Console.Error.WriteLine($"API error ({ex.ResponseStatusCode}): {ex.Message}");
}
```
