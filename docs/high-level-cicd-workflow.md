# High-level CI/CD workflow (Azure DevOps)

## 1) Code commit
- Developer raises PR to `main`.
- PR policies run lint/unit/integration validation.
- Merge to `main` triggers the release pipeline.

## 2) Build
- Restore dependencies.
- Compile/build the application.
- Produce version metadata (build ID, git SHA).

## 3) Test
- Run unit tests.
- Run integration/API tests.
- Fail fast if quality gates are not met.

## 4) Artifact creation
- Package deployable output.
- Publish immutable artifact to Azure DevOps artifacts/feed.
- Reuse the same artifact in all downstream environments.

## 5) Deployment
- **PPR1**: automatic deployment + smoke tests.
- **PPR2**: automatic deployment + smoke tests.
- **PROD**: approval gate + deployment of the exact artifact validated in PPR.

## 6) Health checks (bonus)
- Execute post-deployment health endpoint checks (`/health`) and synthetic smoke checks.
- Configure rollback/on-failure strategy:
  - fail stage if health check fails,
  - trigger rollback to previous successful release,
  - notify release channel (Teams/Slack/email).

## Suggested stage order
Commit/PR -> Build -> Test -> Publish Artifact -> Deploy PPR1 and PPR2 (parallel) -> PROD Approval -> Deploy PROD -> PROD Health Check
