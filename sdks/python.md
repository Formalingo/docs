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

# Bulk create with one caller-owned retry key for the exact request
from uuid import UUID
from formalingo.api.v1.forms.item.recipients.bulk.bulk_post_request_body import BulkPostRequestBody
from formalingo.api.v1.forms.item.recipients.bulk.bulk_post_request_body_recipients import BulkPostRequestBody_recipients

bulk_body = BulkPostRequestBody(
    confirm_bulk=True,
    recipients=[
        BulkPostRequestBody_recipients(
            label="Alice",
            email="alice@example.com",
        ),
    ],
)
from kiota_abstractions.base_request_configuration import RequestConfiguration
from kiota_abstractions.headers_collection import HeadersCollection

configuration = RequestConfiguration(headers=HeadersCollection())
configuration.headers.add("Idempotency-Key", "recipient-bulk-create-7f3f")
response = await client.api.v1.forms.by_id(
    UUID("00000000-0000-0000-0000-000000000001")
).recipients.bulk.post(bulk_body, configuration)
recipients = response.data
```

The idempotency key is required. Reuse it only when retrying the exact same serialized request body.
On `idempotency_request_in_progress`, retry the exact body with the same key. A different body returns `idempotency_key_conflict`; recipient erasure returns `idempotency_replay_unavailable`.

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
from uuid import UUID

from formalingo.models.create_submission_body import CreateSubmissionBody
from formalingo.models.signer_input import SignerInput
from formalingo.models.signer_input_prefill import SignerInput_prefill

prefill = SignerInput_prefill(
    additional_data={"field-uuid-1": "John Doe"},
)
signer = SignerInput(
    role="signer_1",
    name="John Doe",
    email="john@acme.com",
    prefill=prefill,
    prefill_readonly=True,
)

body = CreateSubmissionBody(signers=[signer])

from kiota_abstractions.base_request_configuration import RequestConfiguration
from kiota_abstractions.headers_collection import HeadersCollection

configuration = RequestConfiguration(headers=HeadersCollection())
configuration.headers.add("Idempotency-Key", "document-create-7f3f")
response = await client.api.v1.documents.by_id(
    UUID("00000000-0000-0000-0000-000000000001")
).submissions.post(body, configuration)
submission = response.data
print(submission.signers[0].link)
# → https://www.formalingo.com/d/TOKEN
```

The required idempotency key makes retries safe. The generated request builder
returns an API envelope; signing links are under `response.data.signers`.

### Supporting attachments

Create required signer upload requests through existing document field CRUD;
they stay out of the rendered document and signed PDF. Supporting requests omit
page coordinates and default to required. Set `isRequired` to false for an
optional upload. Ordinary placed fields still require positioning. Use existing
signer role keys from your document; the example edits between `signer_1` and
`signer_2`.

```python
from uuid import UUID, uuid4
from formalingo.models.create_field_body import CreateFieldBody
from formalingo.models.create_field_body_options import CreateFieldBody_options
from formalingo.models.create_field_body_type import CreateFieldBody_type
from formalingo.api.v1.documents.item.fields.item.with_f_put_request_body import WithFPutRequestBody

fields = client.api.v1.documents.by_id(UUID(document_id)).fields
created = await fields.post(CreateFieldBody(
    id=uuid4(),  # Keep this UUID when retrying the same request.
    label="Proof of address",
    type=CreateFieldBody_type.File_upload,
    assignee_role="signer_1",
    options=CreateFieldBody_options(supporting_attachment=True),
))
field_id = created.data.id

# A label-only edit preserves requiredness.
await fields.by_fid(field_id).put(WithFPutRequestBody(label="Updated proof"))
await fields.by_fid(field_id).put(WithFPutRequestBody(
    is_required=False,
    assignee_role="signer_2",
))
definitions = await fields.get()
await fields.by_fid(field_id).delete()
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

```python
try:
    form = await client.api.v1.forms.by_form_id("non-existent").get()
except Exception as error:
    print(f"API error: {error}")
```
