---
name: Ask the Hex agent via a Thread
description: Start a conversational analytics Thread, add follow-up questions, and read back the agent's messages and results.
api: openapi/hex-openapi-original.json
operations: [CreateThread, ContinueThread, GetThread, GetThreadMessages, ListThreads]
---

# Ask the Hex agent via a Thread

Threads are Hex's conversational, agentic analytics surface. Authenticate with a bearer token (`Authorization: Bearer <token>`) against `https://app.hex.tech/api/v1`.

1. Start a thread. `POST` `CreateThread` (`/v1/threads`) with the question/prompt. It returns the thread `id`.
2. Read the result. Poll `GetThread` (`/v1/threads/{id}`) and `GetThreadMessages` (`/v1/threads/{threadId}/messages`) to retrieve the agent's messages and results.
3. Follow up. `POST` `ContinueThread` (`/v1/threads/{id}/followup`) to add a follow-up question in the same context.
4. Browse. `ListThreads` (`/v1/threads`) enumerates existing threads (cursor pagination).

This mirrors the four tools exposed by Hex's hosted MCP server (`search_projects`, `create_thread`, `get_thread`, `continue_thread`) — see `mcp/hex-mcp.yml`. Errors use the `TsoaErrorResponsePayload` envelope with a `traceId`.
