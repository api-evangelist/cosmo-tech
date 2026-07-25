---
name: Run a Cosmo Tech simulation
description: Configure a runner against a solution + workspace and execute a simulation run, then poll status and fetch logs.
api: openapi/cosmo-tech-runner-openapi-original.yaml
operations: [createRunner, startRun, getRunStatus, getRunLogs, getRun, listRuns, stopRun]
generated: '2026-07-18'
method: generated
---

# Run a Cosmo Tech simulation

Operating instructions for driving a simulation on the Cosmo Tech Cloud Platform API. All
operationIds below are verified against the open-source `cosmotech-api` OpenAPI in `openapi/`.

## Auth
Authenticate with OAuth2 (authorization code, Keycloak / OIDC) — scheme `oAuth2AuthCode`. Send the
bearer token on every request. See `authentication/cosmo-tech-authentication.yml`.

## Preconditions
You need an existing `organization_id`, `workspace_id`, and a `solution_id` (with a run template).
Create them first if needed with `createOrganization`, `createWorkspace`, `createSolution`.

## Steps
1. **Create a runner** — `createRunner` (POST `/organizations/{organization_id}/workspaces/{workspace_id}/runners`).
   Bind the solution + run template + datasets. Capture the returned `runner_id`.
2. **Start the run** — `startRun` (POST `.../runners/{runner_id}/start`). Capture the returned `run_id`.
3. **Poll status** — `getRunStatus` (GET `.../runners/{runner_id}/runs/{run_id}/status`) until terminal.
4. **Fetch logs** — `getRunLogs` (GET `.../runs/{run_id}/logs`) and full detail with `getRun`.
5. **List history** — `listRuns` (page + size pagination) to see prior runs.
6. **Abort** if needed — `stopRun` (POST `.../runners/{runner_id}/stop`).

## Conventions
- Pagination is `page` + `size` query params (see `conventions/cosmo-tech-conventions.yml`).
- Errors are status-only: 400 bad request, 403 insufficient permissions, 404 not found, 422 unprocessable
  (see `errors/cosmo-tech-problem-types.yml`). No idempotency key — do not retry non-idempotent POSTs blindly.
- RBAC is enforced per resource; the caller needs the appropriate role on the workspace/runner.
