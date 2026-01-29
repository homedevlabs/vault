# HashiCorp Vault GitOps Deploy

Este repositório contém a configuração para o deploy do HashiCorp Vault no Kubernetes utilizando ArgoCD.

## Estrutura do Repositório

- `docs/usage/`: [Guias de Uso (Policies, Roles)](docs/usage/vault-policies-roles.md).
- `docs/adrs/`: Architecture Decision Records.
- `k8s/argocd/`: Manifestos do ArgoCD (Application).
- `k8s/vault/`: Configuração do Helm Chart do Vault.

## Guias Rápidos

- [Gestão de Políticas e Roles](docs/usage/vault-policies-roles.md)
- [ADR 001: Estrutura k8s/](docs/adrs/001-deploy-vault-argocd.md)
