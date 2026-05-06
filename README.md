# Demo Helm Public Repo

> A public Helm chart repository hosted on GitHub Pages, with automated chart releases via GitHub Actions.

## Repository Structure

```
.
├── .github/workflows/
│   └── release.yml        # Automated Helm chart release workflow
└── web-demo/              # Helm chart source
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
```

## Using the Helm Chart

### 1. Add the Helm Repository

```sh
helm repo add simonangel-fong https://simonangel-fong.github.io/Demo_Helm_Public_Repo/
helm repo update
```

### 2. Install the Chart

```sh
helm install web-demo simonangel-fong/web-demo
```

### 3. Install with Custom Values

```sh
helm install web-demo simonangel-fong/web-demo \
  --set replicaCount=2 \
  --set image.repository=nginx \
  --set image.tag=latest
```

Or provide a custom values file:

```sh
helm install web-demo simonangel-fong/web-demo -f my-values.yaml
```

### 4. Upgrade a Release

```sh
helm upgrade web-demo simonangel-fong/web-demo -f my-values.yaml
```

### 5. Uninstall

```sh
helm uninstall web-demo
```

## Chart Configuration

Key values in [web-demo/values.yaml](web-demo/values.yaml):

| Parameter             | Description                              | Default        |
| --------------------- | ---------------------------------------- | -------------- |
| `replicaCount`        | Number of pod replicas                   | `3`            |
| `image.repository`    | Container image repository               | `nginx`        |
| `image.tag`           | Image tag (defaults to chart appVersion) | `""`           |
| `image.pullPolicy`    | Image pull policy                        | `IfNotPresent` |
| `service.type`        | Kubernetes service type                  | `ClusterIP`    |
| `service.port`        | Service port                             | `80`           |
| `ingress.enabled`     | Enable Ingress                           | `false`        |
| `httpRoute.enabled`   | Enable Gateway API HTTPRoute             | `false`        |
| `autoscaling.enabled` | Enable Horizontal Pod Autoscaler         | `false`        |

## Automated Releases

This repo uses [helm/chart-releaser-action](https://github.com/helm/chart-releaser-action) to automate chart publishing.

**How it works:**

1. Update the `version` field in `web-demo/Chart.yaml`
2. Push or merge to `master`
3. GitHub Actions packages the chart, creates a GitHub Release, and updates the Helm index on the `gh-pages` branch automatically
