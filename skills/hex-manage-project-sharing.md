---
name: Manage Hex project sharing
description: Grant or revoke access to a Hex project for users, groups, collections, and the workspace/public web.
api: openapi/hex-openapi-original.json
operations: [GetProject, EditProjectSharingUsers, EditProjectSharingGroups, EditProjectSharingCollections, EditProjectSharingOrgAndPublic, ListGroups, ListCollections]
---

# Manage Hex project sharing

Governs who can access a project. Bearer-authenticated against `https://app.hex.tech/api/v1`.

1. Inspect current sharing. `GetProject` (`/v1/projects/{projectId}`) returns a `sharing` object spanning `users`, `groups`, `collections`, `workspace`, and `publicWeb`.
2. Resolve principals. Use `ListGroups` (`/v1/groups`) and `ListCollections` (`/v1/collections`) to look up the `groupId` / `collectionId` you want to grant.
3. Apply the grant with the targeted PATCH:
   - `EditProjectSharingUsers` — per-user access (`/v1/projects/{projectId}/sharing/users`).
   - `EditProjectSharingGroups` — group access.
   - `EditProjectSharingCollections` — add the project to collections.
   - `EditProjectSharingOrgAndPublic` — workspace-wide and public-web access (`/sharing/workspaceAndPublic`).

Each PATCH returns `200`; a missing project returns `404`, insufficient permission `403`. Errors carry the `traceId` (see `errors/hex-problem-types.yml`).
