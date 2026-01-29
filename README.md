# HashiCorp Vault GitOps Deploy

Este repositório contém a configuração para o deploy do HashiCorp Vault no Kubernetes utilizando ArgoCD.

## Estrutura do Repositório

- `argocd/`: Manifestos do ArgoCD (Application).
- `charts/vault/`: Configurações customizadas do Helm Chart (values.yaml).
- `docs/adrs/`: Registros de Decisões Arquiteturais.

## Como fazer o Deploy

1. Garanta que o ArgoCD está instalado no seu cluster.
2. Aplique o manifesto da aplicação:

```bash
kubectl apply -f argocd/vault-application.yaml
```

3. O ArgoCD irá provisionar o Vault no namespace `vault`.

## Notas de Configuração

- O deploy padrão está configurado como **Standalone** com persistência de 10Gi.
- A UI está habilitada via ClusterIP.
