# ADR 001: Deploy do HashiCorp Vault via ArgoCD (Revisado)

## Status
Proposto

## Contexto
O usuário solicitou que a estrutura de deploy seja concentrada no subdiretório `k8s/` do repositório de vault, mantendo a compatibilidade com ArgoCD.

## Decisão
A estrutura de arquivos seguirá o padrão:
- `k8s/argocd/`: Manifestos de Application do ArgoCD.
- `k8s/vault/`: Chart Helm local que encapsula o Vault oficial como dependência.

## Consequências
- Organização mais limpa, evitando poluir a raiz do repositório.
- Necessidade de atualizar referências de caminhos (path) no manifesto do ArgoCD.
