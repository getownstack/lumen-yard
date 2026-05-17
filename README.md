# Lumen Yard

Lumen Yard is the neutral static demo app for Ownstack.

It exists to prove the Ownstack DevOps path without depending on an external API, backend service, credentials, or database. The app is a small Nginx-served static site that exercises the same pipeline contract a real customer application would use:

```text
Jenkinsfile -> Docker build -> Harbor push -> Helm deploy -> Traefik ingress
```

## Local Preview

The site is plain static HTML and CSS:

```bash
cd src
python3 -m http.server 8080
```

Then open:

```text
http://localhost:8080
```

## Container Build

The Ownstack pipeline builds from the repository root with this Dockerfile:

```bash
docker build -f infrastructure/lumen-yard/Dockerfile .
```

The image serves files from `src/` on port `80`.

## Ownstack Deployment Layout

```text
infrastructure/Jenkinsfile
infrastructure/lumen-yard/Dockerfile
infrastructure/lumen-yard/helm/Chart.yaml
infrastructure/lumen-yard/helm/values.yaml
infrastructure/lumen-yard/helm/templates/app.yaml
```

The app ID is:

```text
lumen-yard
```

The default Helm hostname is:

```text
demo.perfectalgorithms.com
```
