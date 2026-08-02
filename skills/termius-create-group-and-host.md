---
name: Create a group and add a host to it
description: >-
  Create a group in a Termius vault, then add a host under that group — the
  correct order for nested organization through the API Bridge.
api: openapi/termius-api-bridge-openapi-original.yml
operations: [createGroup, createHost]
---

# Create a group and add a host to it

Groups must exist **before** hosts reference them. This skill does both in order.

## Prerequisites
- Termius **Team** plan with a provisioned API Bridge (default `http://localhost:8080`).

## Steps
1. Choose a group `external_id` (e.g. `client-123`).
2. Call **`createGroup`** — `POST /v1/group/{external_id}/` with:
   - `vault`: the vault name (e.g. `Team`), or `group` for a parent group's external_id.
   - `label`: the group's display name.
   - optional `ssh`/`telnet` defaults (port + credentials, including a shared `key`)
     inherited by hosts in the group.
3. Choose a host `external_id` (e.g. `vm-1234`).
4. Call **`createHost`** — `POST /v1/host/{external_id}/` with `group` set to the
   group's `external_id` (instead of `vault`), plus `address` and connection
   details.
5. Both operations are idempotent on `external_id` — re-running updates in place
   (see `conventions/termius-conventions.yml`); `400` means check the body.

## Notes
- Create the parent group first; a nested group also uses `group` to point at its parent.
