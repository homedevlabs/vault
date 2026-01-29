# ADR 001: Deploy do HashiCorp Vault via ArgoCD

## Status
Proposto

## Contexto
O objetivo é automatizar o deploy do HashiCorp Vault em um cluster Kubernetes utilizando práticas de GitOps e o ArgoCD como ferramenta de sincronização.

## Decisão
Utilizaremos o Helm Chart oficial da HashiCorp (`https://helm.releases.hashicorp.com`) como a fonte da verdade para o deploy. O manifesto do ArgoCD `Application` apontará para este repositório Helm e utilizará um arquivo `values.yaml` customizado armazenado no repositório Git do projeto.

Justificativa:
1. **Padronização:** O Helm Chart oficial é mantido pela HashiCorp e segue as melhores práticas de segurança e configuração.
2. **Manutenibilidade:** Facilita atualizações de versão do Vault alterando apenas o `targetRevision` no ArgoCD.
3. **Customização:** Permite o uso de `values.yaml` para configurar as necessidades específicas do ambiente (ex: Ingress, persistência, modo HA).

## Consequências
- A configuração do Vault (segredos, políticas) não é necessariamente gerenciada via ArgoCD (apenas o deploy da infraestrutura).
- É necessário que o cluster Kubernetes tenha recursos suficientes para os pods do Vault e seus mecanismos de persistência (StorageClasses).
