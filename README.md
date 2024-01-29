# cloudflare-helm-proxy

![deploy](https://github.com/ciiiii/cloudflare-helm-proxy/actions/workflows/deploy.yml/badge.svg)

A helm repo proxy run on cloudflare worker.

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ciiiii/cloudflare-helm-proxy)

> If you're looking for proxy for docker, maybe you can try [cloudflare-docker-proxy](https://github.com/ciiiii/cloudflare-docker-proxy).

## Rules example
- request based on `${cloudflare_worker_route}/${key}` will request to `${url}`.
- text of `*/index.yaml` will be handled with replaces rules.
- `$host` in replaces will be replace with cloudflare worker route.
```javascript
const routes = {
  stable: {
    url: 'https://charts.helm.sh/stable',
    replaces: {
      'charts.helm.sh': '$host',
    },
  },
  incubator: {
    url: 'https://charts.helm.sh/incubator',
    replaces: {
      'charts.helm.sh': '$host',
    },
  },
  grafana: {
    url: 'https://grafana.github.io/helm-charts',
    replaces: {
      'github.com': 'hub.fastgit.org',
    },
  },
  prometheus: {
    url: 'https://prometheus-community.github.io/helm-charts',
    replaces: {
      'prometheus-community.github.io/helm-charts': '$host/prometheus',
    },
  },
  'k8s-at-home': {
    url: 'https://k8s-at-home.com/charts/',
    replaces: {
      'github.com': 'hub.fastgit.org',
    },
  },
}
```

## Usage example
```bash
# helm usage
helm repo add stable https://${cloudflare_worker_route}/stable
helm search repo stable/zetcd

# curl test
curl https://${cloudflare_worker_route}/stable/index.yaml
curl https://${cloudflare_worker_route}/stable/packages/zetcd-0.1.2.tgz
```
