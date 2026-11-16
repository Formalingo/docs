# AI Skills & MCP

An **AI skill** is a self-contained markdown file that teaches AI coding assistants (Claude, Cursor, Copilot, etc.) everything they need to know about the Formalingo API — endpoints, schemas, authentication, SDKs, and common workflows.

Copy the skill below and paste it into your AI tool's context or project instructions. It is auto-generated from the same source of truth as our API reference, so it always stays current.

## Hosted MCP

Formalingo also provides a hosted MCP endpoint for AI clients that support remote MCP.
The MCP server exposes curated tools, resources, and prompts for forms and document signing.
It calls the public `/api/v1` API and accepts the same permission-scoped credentials:
API keys (`af_live_...`) or OAuth access tokens (`fl_at_...`) in the `Authorization: Bearer <token>` header.

See [Hosted MCP](/docs/mcp) for setup instructions and local testing.
