# https://juliabox.stouffcapital.com

## deploy with helm

### `config.yaml`

```
hub:
  cookieSecret: "<openssl rand -hex 32>"
  db:
    type: sqlite-pvc
    pvc:
      accessModes:
        - ReadWriteOnce
      storage: 1Gi
      storageClassName: rook-block

singleuser:
  image:
    name: jupyter/datascience-notebook
    tag: 177037d09156
  storage:
    type: dynamic
    capacity: 5Gi
    dynamic:
      storageClass: rook-block

proxy:
  secretToken: "<openssl rand -hex 32>"
  service:
    type: NodePort

auth:
  type: github
  github:
    clientId: "<clientId>"
    clientSecret: "<clientSecret>"
    callbackUrl: "https://juliabox.stouffcapital.com/hub/oauth_callback"
    org_whitelist:
      - "stouff-capital"
  scopes:
    - "read:user"

```

`helm install jupyterhub/jupyterhub --version=v0.7.0 --name=juliabox --namespace=juliabox -f config.yaml --timeout=10000`

`watch kubectl --namespace=juliabox get po`
