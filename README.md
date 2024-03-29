# k8s vault auto-binder

On k8s namespace creation configures vault-kubernetes binding, and removes it on namespace deletion.

## Usage

Deploy `vaultlink`, make sure it can authorise and talk to vault, then:

```sh
kubectl create namespace test
kubectl annotate namespace test --overwrite vault-link/bind=true
kubectl get namespace test -o yaml
```

and check it for:

```
apiVersion: v1
kind: Namespace
metadata:
  annotations:
    vault-link/bind: "true"
    vault-link/vault: VAULT_ADDR
    vault-link/vault.auth: k8s/docker/test
    vault-link/vault.policy: k8s/docker/test
    vault-link/vault.policy-path: team/test
```

and clean it up:

```sh
kubectl annotate namespace test --overwrite vault-link/bind=false
```

## Development

In skaffold folder:

Copy `etc/k8s-deploy.yaml` to skaffold root, provide `VAULT_TOKEN` (if required), have `skaffold` binary installed, and then:

```sh
skaffold dev
```
