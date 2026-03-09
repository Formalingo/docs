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
```

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
// List documents
docs, err := client.Api().V1().Documents().Get(ctx, nil)

// Create a submission with signers
body := &models.CreateSubmissionBody{
    Signers: []models.SignerInput{
        {
            Role:            to.Ptr("signer_1"),
            Name:            to.Ptr("John Doe"),
            Email:           to.Ptr("john@acme.com"),
            Prefill:         map[string]string{"field-uuid-1": "John Doe"},
            PrefillReadonly:  to.Ptr(true),
        },
    },
}
submission, err := client.Api().V1().Documents().ByDocumentId("DOC_ID").Submissions().Post(ctx, body, nil)
fmt.Println(submission.GetSigners()[0].GetLink())
// → https://www.formalingo.com/d/TOKEN
```

## Error Handling

```go
form, err := client.Api().V1().Forms().ByFormId("non-existent").Get(ctx, nil)
if err != nil {
    fmt.Fprintf(os.Stderr, "API error: %v\n", err)
}
```
