---
name: Delete a host from a Termius vault
description: >-
  Remove a host from a Termius vault by the external_id you assigned it, via the
  self-hosted API Bridge.
api: openapi/termius-api-bridge-openapi-original.yml
operations: [deleteHost]
---

# Delete a host from a Termius vault

Use this skill to remove a host you previously pushed through the API Bridge.

## Prerequisites
- Termius **Team** plan with a provisioned API Bridge (default `http://localhost:8080`).

## Steps
1. Identify the host's `external_id` (the ID from your own system, e.g. `vm-1234`).
2. Call **`deleteHost`** — `DELETE /v1/host/{external_id}/`.
3. A `204` confirms deletion. The call is idempotent on `external_id` — deleting an
   already-removed host is safe. A `400` means "check the request" (see
   `errors/termius-problem-types.yml`).

## Notes
- Deletion targets the host keyed by `external_id`; there is no bulk delete.
