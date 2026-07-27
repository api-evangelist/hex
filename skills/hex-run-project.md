---
name: Run a Hex project and collect results
description: Trigger a run of a published Hex project, poll for completion, and handle kernel-limit backpressure.
api: openapi/hex-openapi-original.json
operations: [ListProjects, GetProject, RunProject, GetRunStatus, GetProjectRuns, CancelRun]
---

# Run a Hex project and collect results

Use the Hex External API (base `https://app.hex.tech/api/v1`, `Authorization: Bearer <hxtp_/hxtw_ token>`).

1. Find the project. Call `ListProjects` (cursor pagination via `pagination.after`) or `GetProject` with a known `projectId`. Only **published** projects can be run — an unpublished project returns `422`.
2. Start a run. `POST` `RunProject` (`/v1/projects/{projectId}/runs`) with any input parameters. It returns `201` with a `runId` and a run URL. **Runs are not idempotent** — capture the `runId`; do not blindly retry, or you will launch duplicate runs.
3. Handle backpressure. If `RunProject` returns `503`, the workspace has hit the 25 concurrent-kernel limit; wait and retry. Respect the 60 requests/minute rate limit.
4. Poll status. Call `GetRunStatus` (`/v1/projects/{projectId}/runs/{runId}`) until the status is terminal. Use `GetProjectRuns` to list recent runs for the project.
5. Cancel if needed. `CancelRun` (`DELETE`) stops an in-flight run (`204`).

Errors follow the `TsoaErrorResponsePayload` envelope (`reason`, `details`, `traceId`) — log the `traceId` for Hex Support. See `conventions/hex-conventions.yml` and `errors/hex-problem-types.yml`.
