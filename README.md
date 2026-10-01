# nodejs-demo-app — CI/CD with GitHub Actions

A small Express app with a GitHub Actions pipeline that **tests → builds → pushes** a Docker image to Docker Hub on every push to `main`.

## Project structure
```
.
├── .github/workflows/main.yml   # CI/CD pipeline
├── test/app.test.js             # tests (Node built-in test runner)
├── app.js / server.js           # Express app
├── Dockerfile
└── package.json
```

## Pipeline (`.github/workflows/main.yml`)
| Job | What it does |
|---|---|
| `test` | Checkout → setup Node 20 → `npm install` → `npm test` |
| `build-and-push` | (needs `test`) Buildx → Docker Hub login → build image → push `latest` and `<commit-sha>` tags |

Trigger: push to `main` (plus manual `workflow_dispatch`).

## Setup
1. Create a Docker Hub account and an **access token** (Account Settings → Security).
2. In the GitHub repo: **Settings → Secrets and variables → Actions → New repository secret**
   - `DOCKERHUB_USERNAME` — your Docker Hub username
   - `DOCKERHUB_TOKEN` — the access token
3. Push to `main` and watch the **Actions** tab.

## Run locally
```bash
npm install
npm test
npm start                      # http://localhost:3000

docker build -t nodejs-demo-app .
docker run -p 3000:3000 nodejs-demo-app
```

## Pull the deployed image
```bash
docker run -p 3000:3000 <your-dockerhub-username>/nodejs-demo-app:latest
```

## Screenshots
Add screenshots of the successful Actions run and the Docker Hub repo here.
