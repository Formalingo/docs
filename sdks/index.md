# SDKs

Formalingo provides official SDKs for **TypeScript**, **Python**, **.NET**, and **Go**, generated from the OpenAPI spec using [Microsoft Kiota](https://learn.microsoft.com/en-us/openapi/kiota/).

The SDKs provide type-safe request builders, automatic serialization, and built-in authentication — so you can skip the boilerplate of raw HTTP calls.

## Available SDKs

| Language | Package | Install |
|---|---|---|
| TypeScript / Node.js | [`@formalingo/sdk`](https://www.npmjs.com/package/@formalingo/sdk) | `npm install @formalingo/sdk` |
| Python | [`formalingo-sdk`](https://github.com/Formalingo/sdk-python) | `pip install git+https://github.com/Formalingo/sdk-python.git` |
| .NET | [`Formalingo.Sdk`](https://www.nuget.org/packages/Formalingo.Sdk) | `dotnet add package Formalingo.Sdk` |
| Go | [`sdk-go`](https://github.com/Formalingo/sdk-go) | `go get github.com/Formalingo/sdk-go` |

## Quick comparison

### curl

```bash
curl https://app.formalingo.com/api/v1/forms \
  -H "Authorization: Bearer af_live_YOUR_KEY"
```

### TypeScript SDK

```typescript
import { createClient } from "@formalingo/sdk"

const client = createClient("af_live_YOUR_KEY")
const forms = await client.forms.get()
```

### Python SDK

```python
from formalingo import create_client

client = create_client("af_live_YOUR_KEY")
forms = await client.api.v1.forms.get()
```

### .NET SDK

```csharp
using Formalingo.Sdk;

var client = FormalingoClientFactory.CreateClient("af_live_YOUR_KEY");
var forms = await client.Api.V1.Forms.GetAsync();
```

### Go SDK

```go
import formalingo "github.com/Formalingo/sdk-go"

client, _ := formalingo.NewClient("af_live_YOUR_KEY")
forms, err := client.Api().V1().Forms().Get(context.Background(), nil)
```

## Next steps

- [TypeScript SDK guide](/docs/sdks/typescript) — installation, setup, and examples
- [Python SDK guide](/docs/sdks/python) — installation, setup, and examples
- [.NET SDK guide](/docs/sdks/dotnet) — installation, setup, and examples
- [Go SDK guide](/docs/sdks/go) — installation, setup, and examples
