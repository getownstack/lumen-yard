# AGENTS.md

## Scope

These instructions apply to the `lumen-yard` repository.

Lumen Yard is the canonical simple demo app for Ownstack. Keep it boring, reliable, and dependency-free so it can act as a clean smoke test for the DevOps platform.

## Purpose

This repo should demonstrate the application contract expected by `ownstack-cluster-template`:

- A tiny `Jenkinsfile`.
- A Dockerfile at `infrastructure/<app-id>/Dockerfile`.
- A Helm chart at `infrastructure/<app-id>/helm`.
- A static app that listens on port `80`.
- No credentials, backend calls, database, or external services.

The app ID is `lumen-yard`.

## Runtime

The app is a static Nginx site:

- Source files live in `src/`.
- `infrastructure/lumen-yard/Dockerfile` copies `src/` into `/usr/share/nginx/html/`.
- Nginx serves the site on port `80`.

## Pipeline Contract

`infrastructure/Jenkinsfile` must remain:

```groovy
runPipeline('lumen-yard')
```

The Ownstack shared pipeline expects:

- Dockerfile: `infrastructure/lumen-yard/Dockerfile`
- Helm chart: `infrastructure/lumen-yard/helm`
- Chart entrypoint: `templates/app.yaml`
- Helm helper: `{{ include "public-url-application" . }}`

Current default hostname:

```text
demo.perfectalgorithms.com
```

## Change Guidelines

- Keep the site static.
- Do not introduce npm, Python, API calls, or server-side rendering.
- Do not add secrets or environment-specific values.
- Preserve app ID `lumen-yard` unless the Jenkinsfile, Dockerfile path, and Helm path are all intentionally changed together.
- Keep the visual design polished enough to show that the pipeline deployed a real site, but do not turn this into a product app.

## Validation

Static syntax checks are enough for most changes:

```bash
python3 -m http.server 8080 --directory src
```

Container build check:

```bash
docker build -f infrastructure/lumen-yard/Dockerfile .
```
