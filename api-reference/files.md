# Files

Create temporary API-key-gated download URLs for response uploads and signed PDFs.

## Endpoints

### `GET` `/api/v1/files/{fileId}/download`

 — Scopes: `submissions:download_files`

**TypeScript SDK**
```typescript
client.files.byFileId("FILEID").download.get()
```

**Python SDK**
```python
await client.api.v1.files.by_file_id(UUID("FILEID")).download.get()
```

**.NET SDK**
```csharp
client.Api.V1.Files[Guid.Parse("FILEID")].Download.GetAsync()
```

**Go SDK**
```go
client.Api().V1().Files().ByFileId(uuid.MustParse("FILEID")).Download().Get(ctx, nil)
```

---

*This page is auto-generated from the shared API endpoint catalog. Run `yarn docs:generate` to update.*
