# Conditional Logic

Rules show or hide questions/sections based on answers to other questions.

### `GET` `/api/v1/forms/{id}/rules`

 — Scopes: `read:forms`

### `POST` `/api/v1/forms/{id}/rules`

Create a rule — Scopes: `write:forms`

### `PUT` `/api/v1/forms/{id}/rules/{rid}`

Update a rule — Scopes: `write:forms`

### `DELETE` `/api/v1/forms/{id}/rules/{rid}`

Delete a rule — Scopes: `write:forms`

## Create a rule

**Request Body**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `source_question_id` | `uuid` | Yes | The question whose answer triggers this rule |
| `operator` | `string` | Yes | equals | not_equals | contains | greater_than | less_than | is_empty | is_not_empty |
| `value` | `any` | No | The value to compare against (omit for is_empty/is_not_empty) |
| `action` | `show | hide` | Yes | What to do with the target |
| `target_question_id` | `uuid` | No | Target question (provide this or target_section_id) |
| `target_section_id` | `uuid` | No | Target section (provide this or target_question_id) |

### Example: hide a question unless "Other" is selected

**curl**
```bash
curl -X POST https://app.formalingo.com/api/v1/forms/FORM_ID/rules \\
  -H "Authorization: Bearer af_live_YOUR_KEY" \\
  -H "Content-Type: application/json" \\
  -d '{
    "source_question_id": "SOURCE_QUESTION_UUID",
    "operator": "equals",
    "value": "Other",
    "action": "show",
    "target_question_id": "TARGET_QUESTION_UUID"
  }'
```

**TypeScript SDK**
```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")

const rule = await client.forms.byFormId("FORM_ID").rules.post({
  sourceQuestionId: "SOURCE_QUESTION_UUID",
  operator: "equals",
  value: "Other",
  action: "show",
  targetQuestionId: "TARGET_QUESTION_UUID",
})
```

**Python SDK**
```python
body = RuleCreateRequest()
body.source_question_id = "SOURCE_QUESTION_UUID"
body.operator = "equals"
body.value = "Other"
body.action = "show"
body.target_question_id = "TARGET_QUESTION_UUID"

rule = await client.api.v1.forms.by_form_id("FORM_ID").rules.post(body)
```

**.NET SDK**
```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");

var rule = await client.Api.V1.Forms["FORM_ID"].Rules.PostAsync(new() {
    SourceQuestionId = "SOURCE_QUESTION_UUID",
    Operator = "equals",
    Value = "Other",
    Action = "show",
    TargetQuestionId = "TARGET_QUESTION_UUID",
});
```

**Go SDK**
```go
client, _ := formalingo.NewClient("af_live_YOUR_KEY")

body := &models.RuleCreateRequest{
    SourceQuestionId: to.Ptr("SOURCE_QUESTION_UUID"),
    Operator:         to.Ptr("equals"),
    Value:            to.Ptr("Other"),
    Action:           to.Ptr("show"),
    TargetQuestionId: to.Ptr("TARGET_QUESTION_UUID"),
}
rule, err := client.Api().V1().Forms().ByFormId("FORM_ID").Rules().Post(context.Background(), body, nil)
```
