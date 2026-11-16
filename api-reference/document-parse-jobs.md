# Document Parse Jobs

Start, poll, and apply AI-assisted document field detection jobs.

## Endpoints

### `POST` `/api/v1/documents/{id}/parse-jobs`

 — Scopes: `documents:write`, `documents:parse_ai`

**TypeScript SDK**
```typescript
client.documents.byId("ID").parseJobs.post(body, {})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).parse_jobs.post(body)
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].ParseJobs.PostAsync(body)
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).ParseJobs().Post(ctx, body, nil)
```

### `GET` `/api/v1/documents/{id}/parse-jobs/{jobId}`

 — Scopes: `documents:read`

**TypeScript SDK**
```typescript
client.documents.byId("ID").parseJobs.byJobId("JOBID").get()
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).parse_jobs.by_job_id(UUID("JOBID")).get()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].ParseJobs[Guid.Parse("JOBID")].GetAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).ParseJobs().ByJobId(uuid.MustParse("JOBID")).Get(ctx, nil)
```

### `POST` `/api/v1/documents/{id}/parse-jobs/{jobId}/apply`

 — Scopes: `documents:write`, `documents:parse_ai`

**TypeScript SDK**
```typescript
client.documents.byId("ID").parseJobs.byJobId("JOBID").apply.post({})
```

**Python SDK**
```python
await client.api.v1.documents.by_id(UUID("ID")).parse_jobs.by_job_id(UUID("JOBID")).apply.post()
```

**.NET SDK**
```csharp
client.Api.V1.Documents[Guid.Parse("ID")].ParseJobs[Guid.Parse("JOBID")].Apply.PostAsync()
```

**Go SDK**
```go
client.Api().V1().Documents().ById(uuid.MustParse("ID")).ParseJobs().ByJobId(uuid.MustParse("JOBID")).Apply().Post(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
