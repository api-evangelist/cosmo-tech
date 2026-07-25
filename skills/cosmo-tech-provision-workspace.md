---
name: Provision a Cosmo Tech organization and workspace
description: Stand up an organization, a workspace, a solution and a dataset as the foundation for simulations.
api: openapi/cosmo-tech-organization-openapi-original.yaml
operations: [createOrganization, createWorkspace, createSolution, createDataset, createOrganizationAccessControl]
generated: '2026-07-18'
method: generated
---

# Provision a Cosmo Tech organization and workspace

Operating instructions for bootstrapping the Cosmo Tech Cloud Platform resource hierarchy. All
operationIds are verified against the `openapi/` service specs.

## Auth
OAuth2 authorization-code (Keycloak / OIDC), scheme `oAuth2AuthCode`; bearer token on every call.

## Resource hierarchy
`Organization` -> `Workspace` -> (`Runner` -> `Run`); `Solution` and `Dataset` attach in.
See `data-model/cosmo-tech-data-model.yml`.

## Steps
1. **Create organization** — `createOrganization` (POST `/organizations`). Capture `organization_id`.
2. **Grant access** — `createOrganizationAccessControl`
   (POST `/organizations/{organization_id}/security/access`) to add identities/roles (RBAC).
3. **Create a solution** — `createSolution` (POST `/organizations/{organization_id}/solutions`).
   Capture `solution_id`; define parameters, parameter groups and run templates on it.
4. **Create a workspace** — `createWorkspace`
   (POST `/organizations/{organization_id}/workspaces`). Capture `workspace_id`.
5. **Create a dataset** — `createDataset`
   (POST `/organizations/{organization_id}/workspaces/{workspace_id}/datasets`) and add dataset parts.

The workspace + solution + dataset are the inputs to the `Run a Cosmo Tech simulation` skill.

## Conventions
- List endpoints use `page` + `size`. Errors are status-only (400/403/404/422). No idempotency contract.
- Every resource carries its own `/security` RBAC surface (`getXSecurity`, `updateXDefaultSecurity`, access control).
