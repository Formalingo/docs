# Python SDK

The `formalingo-sdk` package provides a type-safe client for the Formalingo API, generated with [Microsoft Kiota](https://learn.microsoft.com/en-us/openapi/kiota/).

## Installation

```bash
pip install git+https://github.com/Formalingo/sdk-python.git
```

Or with uv:

```bash
uv pip install git+https://github.com/Formalingo/sdk-python.git
```

## Setup

```python
from formalingo import create_client

client = create_client("af_live_YOUR_KEY")
```

The `create_client` helper wires up authentication and the HTTP adapter automatically. You can optionally pass a custom base URL:

```python
client = create_client("af_live_YOUR_KEY", base_url="https://staging.formalingo.com")
```

> **Note:** The Kiota-generated client includes the full API path: `client.api.v1.forms`, `client.api.v1.documents`, etc.

## Forms

### List forms

```python
forms = await client.api.v1.forms.get()
```

### Create a form

```python
from formalingo.models.create_form_body import CreateFormBody

body = CreateFormBody()
body.title = "Customer Satisfaction Survey"
body.description = "Help us improve our service."

form = await client.api.v1.forms.post(body)
```

### Get a form

```python
form = await client.api.v1.forms.by_form_id("FORM_ID").get()
```

### Update a form

```python
from formalingo.models.update_form_body import UpdateFormBody

body = UpdateFormBody()
body.status = "published"

form = await client.api.v1.forms.by_form_id("FORM_ID").put(body)
```

### Delete a form

```python
await client.api.v1.forms.by_form_id("FORM_ID").delete()
```

## Sections

```python
# List sections
sections = await client.api.v1.forms.by_form_id("FORM_ID").sections.get()

# Create a section
from formalingo.models.create_section_body import CreateSectionBody

body = CreateSectionBody()
body.title = "Personal Information"

section = await client.api.v1.forms.by_form_id("FORM_ID").sections.post(body)
```

## Questions

```python
from formalingo.models.create_question_body import CreateQuestionBody

# Create a text question
body = CreateQuestionBody()
body.question_text = "What is your full name?"
body.type = "short_text"
body.is_required = True

question = await client.api.v1.forms.by_form_id("FORM_ID").questions.post(body)
```

## Recipients

```python
from formalingo.models.create_recipient_body import CreateRecipientBody

body = CreateRecipientBody()
body.label = "John Doe"
body.email = "john@acme.com"
body.prefill = {
    "question-uuid-1": "John Doe",
    "question-uuid-2": "Acme Corp",
}

recipient = await client.api.v1.forms.by_form_id("FORM_ID").recipients.post(body)
print(recipient.link)
# → https://www.formalingo.com/f/TOKEN
```

## Branding

```python
from formalingo.models.branding_update_request import BrandingUpdateRequest

body = BrandingUpdateRequest()
body.primary_color = "#6366F1"
body.welcome_heading = "Welcome!"
body.thank_you_heading = "Thank you!"

await client.api.v1.forms.by_form_id("FORM_ID").branding.put(body)
```

## Documents

```python
# List documents
docs = await client.api.v1.documents.get()

# Create a submission
from formalingo.models.create_submission_body import CreateSubmissionBody
from formalingo.models.signer_input import SignerInput

signer = SignerInput()
signer.role = "signer_1"
signer.name = "John Doe"
signer.email = "john@acme.com"
signer.prefill = {"field-uuid-1": "John Doe"}
signer.prefill_readonly = True

body = CreateSubmissionBody()
body.signers = [signer]

submission = await client.api.v1.documents.by_document_id("DOC_ID").submissions.post(body)
print(submission.signers[0].link)
# → https://www.formalingo.com/d/TOKEN
```

## Error Handling

```python
try:
    form = await client.api.v1.forms.by_form_id("non-existent").get()
except Exception as error:
    print(f"API error: {error}")
```
