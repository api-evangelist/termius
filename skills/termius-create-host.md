---
name: Create a host in a Termius vault
description: >-
  Add a single host (SSH or Telnet endpoint) to a Termius Team vault through the
  self-hosted API Bridge, keyed idempotently on your own external_id.
api: openapi/termius-api-bridge-openapi-original.yml
operations: [createHost]
---

# Create a host in a Termius vault

Use this skill to push one host into a Termius **Team** vault via the API Bridge.

## Prerequisites
- A Termius **Team** plan with an API Bridge provisioned (Team → API Bridge page).
- The API Bridge container running (default `http://localhost:8080`).

## Steps
1. Pick an `external_id` — the host's ID **in your own system** (e.g. `vm-1234`).
   Re-using the same `external_id` updates the same host instead of duplicating it,
   so the call is safe to retry (see `conventions/termius-conventions.yml`).
2. Call **`createHost`** — `POST /v1/host/{external_id}/` with a JSON body:
   - `vault`: the target vault name (e.g. `Team`), **or** `group` for a group's external_id.
   - `address`: the host address (required).
   - `label`: optional display name.
   - `ssh.port` + `ssh.credentials.username` / `password`, and optionally
     `ssh.credentials.key` (`label` + `private` PEM) for key auth. Use `telnet`
     instead of `ssh` for Telnet hosts.
3. A `200` returns the create response. A `400` means "check the request body"
   (see `errors/termius-problem-types.yml`).

## Notes
- Never invent credentials; pass real values only when actually provisioning.
- The bridge encrypts data locally before syncing it into the vault.
