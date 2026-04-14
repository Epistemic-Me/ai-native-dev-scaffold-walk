# Related Repos

> **Stub file.** Cross-repo context. Which sibling repos exist, what they own, and how they relate to this one.

## This Repo

**Name**: your-project
**Role**: [e.g., "backend API", "frontend web app", "data pipeline"]
**Primary language**: ...
**Owners**: ...

## Sibling Repos

| Repo | Role | Owners | Notes |
|---|---|---|---|
| example-frontend | React SPA consuming this API | Frontend team | Lives at `github.com/your-org/example-frontend` |
| example-infra | Terraform + Kubernetes configs | Platform team | Deploys this repo |

## Shared Concerns

Things that cut across multiple repos — design tokens, API contracts, auth flows, observability config. Where is the source of truth for each?

## How changes flow

If a change here affects a sibling repo, describe the contract (API versioning, schema migrations, shared library bumps).
