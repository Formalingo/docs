# .NET SDK

The `Formalingo.Sdk` package provides a type-safe client for the Formalingo API, generated with [Microsoft Kiota](https://learn.microsoft.com/en-us/openapi/kiota/).

## Installation

```bash
dotnet add package Formalingo.Sdk
```

## Setup

```csharp
using Formalingo.Sdk;

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
```

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
var submission = await client.Api.V1.Documents["DOC_ID"]
    .Submissions.PostAsync(new() {
        Signers = new List<SignerInput> {
            new() {
                Role = "signer_1",
                Name = "John Doe",
                Email = "john@acme.com",
                Prefill = new Dictionary<string, string> {
                    ["field-uuid-1"] = "John Doe",
                },
                PrefillReadonly = true,
            },
        },
    });

Console.WriteLine(submission.Signers[0].Link);
// → https://www.formalingo.com/d/TOKEN
```

## Error Handling

```csharp
try {
    var form = await client.Api.V1.Forms["non-existent"].GetAsync();
} catch (ApiException ex) {
    Console.Error.WriteLine($"API error ({ex.ResponseStatusCode}): {ex.Message}");
}
```
