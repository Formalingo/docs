# Questions

### `GET` `/api/v1/forms/{id}/questions`

 — Scopes: `read:forms`

### `POST` `/api/v1/forms/{id}/questions`

Create a question — Scopes: `write:forms`

### `PUT` `/api/v1/forms/{id}/questions/{qid}`

Update a question — Scopes: `write:forms`

### `DELETE` `/api/v1/forms/{id}/questions/{qid}`

Delete a question — Scopes: `write:forms`

## Question types

### Text

| Type | Description |
|---|---|
| `short_text` | Single-line text input |
| `long_text` | Multi-line text area |
| `email` | Email address with validation |
| `phone` | Phone number |
| `url` | URL with validation |

### Choice

| Type | Description |
|---|---|
| `mcq` | Multiple choice (single answer) |
| `multi_select` | Multiple choice (multiple answers) |
| `dropdown` | Dropdown select |
| `yes_no` | Boolean yes/no |
| `checkbox` | Single checkbox (e.g. agreement/consent) |
| `picture_choice` | Image-based choice with optional labels |
| `ranking` | Drag-to-rank a list of items |
| `matrix` | Grid of rows and columns |

### Rating & Scale

| Type | Description |
|---|---|
| `rating` | Star or numeric rating |
| `opinion_scale` | Numbered scale (e.g. 0–10) with labels |
| `slider` | Draggable slider with min/max/step |

### Number & Calculation

| Type | Description |
|---|---|
| `number` | Numeric input with optional min/max |
| `currency` | Currency amount with currency code selector |
| `calculation` | Computed field using formulas referencing other fields |

### Date & Time

| Type | Description |
|---|---|
| `date` | Date picker |
| `time` | Time picker (12h or 24h format) |
| `date_range` | Start and end date range picker |

### Contact

| Type | Description |
|---|---|
| `address` | Structured address with configurable sub-fields |

### Upload & Signature

| Type | Description |
|---|---|
| `file_upload` | File attachment |
| `digital_signature` | Draw or upload signature |
| `initials` | Draw initials |

### Payment

| Type | Description |
|---|---|
| `payment` | Collect payments via Stripe |

### Layout

| Type | Description |
|---|---|
| `heading` | Section heading (no input collected) |
| `divider` | Visual separator (no input collected) |

---

## Type-specific options reference

Each question type accepts an `options` object with type-specific fields. All fields are optional unless noted.

### `short_text` / `long_text` / `email` / `phone` / `url`

| Field | Type | Description |
|---|---|---|
| `min_length` | integer | Minimum character count |
| `max_length` | integer | Maximum character count |
| `pattern` | string | Custom regex pattern for validation (max 200 chars) |
| `validation_message` | string | Custom error message when validation fails |

### `mcq` / `multi_select` / `dropdown`

| Field | Type | Description |
|---|---|---|
| `choices` | string[] | **Required.** Array of option labels |
| `max_selections` | integer | (`multi_select` only) Maximum selections allowed |

### `picture_choice`

| Field | Type | Description |
|---|---|---|
| `picture_choices` | object[] | **Required.** Array of `{ label, image_url? }` objects |

Each item in `picture_choices`:

| Field | Type | Description |
|---|---|---|
| `label` | string | **Required.** Display text for the choice |
| `image_url` | string | Image URL (optional — text-only if omitted) |

### `yes_no`

| Field | Type | Description |
|---|---|---|
| `yes_label` | string | Custom "yes" text (default: `"Yes"`) |
| `no_label` | string | Custom "no" text (default: `"No"`) |

### `rating`

| Field | Type | Description |
|---|---|---|
| `min` | integer | Scale minimum (default: `1`) |
| `max` | integer | Scale maximum (default: `5`) |
| `min_label` | string | Label for lowest value (e.g. `"Poor"`) |
| `max_label` | string | Label for highest value (e.g. `"Excellent"`) |

### `opinion_scale`

| Field | Type | Description |
|---|---|---|
| `min` | integer | Scale minimum (default: `1`) |
| `max` | integer | Scale maximum (default: `10`) |
| `min_label` | string | Label for lowest value |
| `max_label` | string | Label for highest value |

### `slider`

| Field | Type | Description |
|---|---|---|
| `min_value` | number | Slider minimum (default: `0`) |
| `max_value` | number | Slider maximum (default: `100`) |
| `step` | number | Increment size (default: `1`) |
| `show_value` | boolean | Show current value while dragging (default: `true`) |

### `number`

| Field | Type | Description |
|---|---|---|
| `min_value` | number | Minimum allowed value |
| `max_value` | number | Maximum allowed value |

### `currency`

| Field | Type | Description |
|---|---|---|
| `currency_code` | string | ISO 4217 code (default: `"USD"`) |
| `decimal_places` | integer | Precision 0–3 (default: `2`) |
| `min_value` | number | Minimum amount |
| `max_value` | number | Maximum amount |

### `date`

| Field | Type | Description |
|---|---|---|
| `min_date` | string | Earliest selectable date (`YYYY-MM-DD`) |
| `max_date` | string | Latest selectable date (`YYYY-MM-DD`) |

### `date_range`

| Field | Type | Description |
|---|---|---|
| `min_date` | string | Earliest start date allowed (`YYYY-MM-DD`) |
| `max_date` | string | Latest end date allowed (`YYYY-MM-DD`) |

Response value: `{ "start": "YYYY-MM-DD", "end": "YYYY-MM-DD" }`

### `time`

| Field | Type | Description |
|---|---|---|
| `time_format` | string | `"12h"` or `"24h"` (default: `"24h"`) |

### `address`

| Field | Type | Description |
|---|---|---|
| `address_fields` | string[] | Sub-fields to include: `"street"`, `"city"`, `"state"`, `"zip"`, `"country"`. All included by default. |

Response value: `{ "street": "...", "city": "...", "state": "...", "zip": "...", "country": "..." }`

### `ranking`

| Field | Type | Description |
|---|---|---|
| `choices` | string[] | **Required.** Items to rank via drag-and-drop |

Response value: ordered array of choice labels.

### `matrix`

| Field | Type | Description |
|---|---|---|
| `rows` | string[] | **Required.** Row labels |
| `columns` | string[] | **Required.** Column labels |

Response value: `{ "row_label": "selected_column", ... }`

### `file_upload`

| Field | Type | Description |
|---|---|---|
| `allowed_types` | string[] | Accepted extensions: `pdf`, `doc`, `docx`, `jpg`, `jpeg`, `png`, `gif`, `xlsx`, `csv` |
| `max_size_mb` | number | Max file size in MB (1–100) |

### `digital_signature` / `initials`

| Field | Type | Description |
|---|---|---|
| `signature_mode` | string | `"draw"`, `"upload"`, or `"both"` (default: `"both"`) |

### `calculation`

| Field | Type | Description |
|---|---|---|
| `formula` | string | **Required.** Arithmetic expression referencing other fields via `{{Field Label}}`. Supports `+`, `-`, `*`, `/`, `()`. |
| `decimal_places` | integer | Precision 0–3 (default: `2`) |

Calculation questions are display-only and should not be marked as required.

### `payment`

| Field | Type | Description |
|---|---|---|
| `stripe_publishable_key` | string | **Required.** Stripe publishable key (`pk_live_…` or `pk_test_…`) |
| `amount` | integer | **Required.** Amount in cents (e.g. `2999` = $29.99) |
| `currency_code` | string | ISO 4217 code (default: `"USD"`) |
| `payment_description` | string | Description shown to customer (defaults to question text) |

### `heading` / `divider` / `checkbox`

No options. `heading` uses `question_text` as the heading content. `checkbox` uses `question_text` as the agreement statement.

---

## Quiz mode options

When quiz mode is enabled in form settings (`quiz_mode: true`), scorable question types accept additional fields in `options`:

### Choice-based types (`mcq`, `dropdown`, `multi_select`, `picture_choice`, `yes_no`)

| Field | Type | Description |
|---|---|---|
| `correct_answers` | string[] | Correct option label(s) |
| `choice_scores` | object | Label-to-points mapping: `{ "Option A": 10, "Option B": 5 }` |
| `correct_answer_points` | number | Flat points for a correct answer (used when `choice_scores` is not set) |

### Numeric types (`number`, `currency`, `rating`, `slider`, `opinion_scale`)

| Field | Type | Description |
|---|---|---|
| `correct_answer_value` | number | Exact correct numeric answer |
| `correct_answer_points` | number | Points awarded for a correct answer |

### Text type (`short_text`)

| Field | Type | Description |
|---|---|---|
| `correct_answer_text` | string | Expected answer (case-insensitive match) |
| `correct_answer_points` | number | Points awarded for a correct answer |

---

## Create a question

**Request Body**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | `QuestionType` | Yes | One of the question types above |
| `section_id` | `uuid` | No | Assign to a section |
| `order` | `integer` | No | Position within the form (auto-appended if omitted) |
| `options` | `object` | No | Type-specific options — see reference above |

## Examples

### Text question

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "question_text": "What is your full name?",
    "type": "short_text",
    "is_required": true
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const question = await client.forms.byFormId("FORM_ID").questions.post({
  questionText: "What is your full name?",
  type: "short_text",
  isRequired: true,
})
```

**Python SDK**
```python
body = CreateQuestionBody()
body.question_text = "What is your full name?"
body.type = "short_text"
body.is_required = True

question = await client.api.v1.forms.by_form_id("FORM_ID").questions.post(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var question = await client.Api.V1.Forms["FORM_ID"].Questions.PostAsync(new() {
    QuestionText = "What is your full name?",
    Type = "short_text",
    IsRequired = true,
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.CreateQuestionBody{
    QuestionText: to.Ptr("What is your full name?"),
    Type:         to.Ptr("short_text"),
    IsRequired:   to.Ptr(true),
}
question, err := client.Api().V1().Forms().ByFormId("FORM_ID").Questions().Post(context.Background(), body, nil)
```

### Multiple choice

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "How did you hear about us?",
    "type": "mcq",
    "options": { "choices": ["Social media", "Friend", "Search engine", "Other"] }
  }'
```

### Picture choice

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Which design do you prefer?",
    "type": "picture_choice",
    "options": {
      "picture_choices": [
        { "label": "Modern", "image_url": "https://example.com/modern.jpg" },
        { "label": "Classic", "image_url": "https://example.com/classic.jpg" },
        { "label": "Minimal" }
      ]
    }
  }'
```

### Rating

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Rate your overall experience",
    "type": "rating",
    "options": { "min": 1, "max": 5, "min_label": "Poor", "max_label": "Excellent" }
  }'
```

### Slider

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "How likely are you to recommend us?",
    "type": "slider",
    "options": { "min_value": 0, "max_value": 100, "step": 5, "show_value": true }
  }'
```

### Matrix

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Rate each aspect of our service",
    "type": "matrix",
    "options": {
      "rows": ["Speed", "Quality", "Support"],
      "columns": ["Poor", "Fair", "Good", "Excellent"]
    }
  }'
```

### Opinion scale

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "How satisfied are you?",
    "type": "opinion_scale",
    "options": { "min": 0, "max": 10, "min_label": "Not at all", "max_label": "Extremely" }
  }'
```

### Ranking

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Rank these features by importance",
    "type": "ranking",
    "options": { "choices": ["Performance", "Ease of use", "Price", "Support"] }
  }'
```

### Currency

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "What is your monthly budget?",
    "type": "currency",
    "options": { "currency_code": "EUR", "decimal_places": 2, "min_value": 0 }
  }'
```

### Address

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Shipping address",
    "type": "address",
    "is_required": true,
    "options": { "address_fields": ["street", "city", "state", "zip", "country"] }
  }'
```

### File upload

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Upload your resume",
    "type": "file_upload",
    "is_required": true,
    "options": { "allowed_types": ["pdf", "doc", "docx"], "max_size_mb": 10 }
  }'
```

### Digital signature

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Please sign below",
    "type": "digital_signature",
    "is_required": true,
    "options": { "signature_mode": "both" }
  }'
```

### Calculation

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Total Cost",
    "type": "calculation",
    "options": { "formula": "{{Quantity}} * {{Unit Price}}", "decimal_places": 2 }
  }'
```

### Payment

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Registration fee",
    "type": "payment",
    "is_required": true,
    "options": {
      "stripe_publishable_key": "pk_live_YOUR_KEY",
      "amount": 4999,
      "currency_code": "USD",
      "payment_description": "Conference registration"
    }
  }'
```

### Date range

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "Preferred travel dates",
    "type": "date_range",
    "options": { "min_date": "2026-04-01", "max_date": "2026-12-31" }
  }'
```

### Quiz: scored multiple choice

```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/questions \
  -H "Authorization: Bearer af_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question_text": "What is the capital of France?",
    "type": "mcq",
    "is_required": true,
    "options": {
      "choices": ["London", "Berlin", "Paris", "Madrid"],
      "correct_answers": ["Paris"],
      "choice_scores": { "Paris": 10, "London": 0, "Berlin": 0, "Madrid": 0 }
    }
  }'
```
