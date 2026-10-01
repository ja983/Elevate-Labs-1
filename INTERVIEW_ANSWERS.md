# Interview Questions — Short Answers

**1. What is CI/CD?**
Continuous Integration automatically builds and tests code on every change. Continuous Delivery/Deployment automatically releases the tested build to staging/production. Together they shorten feedback loops and reduce manual errors.

**2. How do GitHub Actions work?**
A workflow (YAML in `.github/workflows/`) is triggered by an event (push, PR, schedule). It contains jobs, each running on a runner, and each job is a sequence of steps that run shell commands or reusable actions.

**3. What are runners?**
Machines that execute jobs. GitHub-hosted runners (ubuntu/windows/macos) are fresh VMs per job; self-hosted runners are your own machines, useful for special hardware or private networks.

**4. Difference between jobs and steps?**
Jobs run on separate runners, in parallel by default (use `needs:` to order them). Steps run sequentially inside one job and share the same filesystem.

**5. How to secure secrets in GitHub Actions?**
Store them in Settings → Secrets (never in code), reference as `${{ secrets.NAME }}`, use least-privilege tokens (e.g. Docker Hub access token, not password), restrict `GITHUB_TOKEN` permissions, use environment protection rules, pin third-party actions to versions/SHAs, and avoid echoing secrets.

**6. How to handle deployment errors?**
Read the job logs, fail fast with `needs:` and test gates, add retries/health checks, keep immutable tagged images (commit SHA) so you can roll back, send notifications on failure, and use `if: failure()` steps for cleanup/alerts.

**7. Explain the Docker build-push workflow.**
Checkout code → log in to the registry → `docker build` using the Dockerfile and tag the image (`user/app:sha`, `latest`) → `docker push` to Docker Hub → deployment targets pull that tag.

**8. How can you test a CI/CD pipeline locally?**
Use [`act`](https://github.com/nektos/act) to run workflows in Docker locally, run the same commands manually (`npm test`, `docker build`), lint the YAML (`actionlint`), and test on a feature branch or with `workflow_dispatch` before merging to `main`.
