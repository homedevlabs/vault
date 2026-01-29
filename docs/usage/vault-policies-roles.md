# Guia de Uso: Policies e Roles no HashiCorp Vault

Este guia descreve como gerenciar permissões no Vault através de Políticas (Policies) e como vinculá-las a identidades usando Roles.

## 1. Políticas (Policies)

As políticas no Vault controlam o que um usuário ou aplicação pode acessar. Elas são escritas em HCL (HashiCorp Configuration Language).

### Estrutura Básica
Caminhos no Vault seguem o formato `path "caminho" { capabilities = ["permissões"] }`.

Permissões comuns:
- `create`: Criar dados.
- `read`: Ler dados.
- `update`: Alterar dados existentes.
- `delete`: Apagar dados.
- `list`: Listar caminhos.
- `sudo`: Permite ignorar restrições (use com cautela).

### Exemplos Práticos

#### Política de Leitura para Aplicações (App-Read)
Permite ler segredos em um caminho específico (ex: `secret/data/backstage/*`).
```hcl
path "secret/data/backstage/*" {
  capabilities = ["read", "list"]
}
```

#### Política de Admin de Segredos
Permite controle total sobre o engine de segredos KV.
```hcl
path "secret/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}
```

### Como Aplicar uma Política
```bash
# Via arquivo
vault policy write nome-da-politica arquivo.hcl

# Via CLI direta
echo 'path "secret/data/myapp/*" { capabilities = ["read"] }' | vault policy write myapp-policy -
```

---

## 2. Roles (Kubernetes Auth)

Roles são o "vínculo" entre uma identidade externa (como uma ServiceAccount do Kubernetes) e uma Política do Vault.

### Como criar uma Role para K8s
Para que um Pod no Kubernetes consiga ler segredos, você mapeia a ServiceAccount dele para uma Role.

```bash
vault write auth/kubernetes/role/nome-da-role \
    bound_service_account_names="nome-da-sa-do-k8s" \
    bound_service_account_namespaces="namespace-da-app" \
    policies="politica-associada" \
    ttl=24h
```

**Exemplo para o ExternalSecrets:**
```bash
vault write auth/kubernetes/role/backstage-role \
    bound_service_account_names="*" \
    bound_service_account_namespaces="backstage" \
    policies="backstage-policy" \
    ttl=24h
```

---

## 3. Mapeamento de Times (GitHub Auth)

No caso de seres humanos usando o GitHub, usamos o mapeamento de times em vez de Roles individuais.

### Como Mapear um Time
```bash
# Mapeia o time 'Administrators' para ter controle total
vault write auth/github/map/teams/Administrators value=admin-policy

# Mapeia o time 'Default' para acesso básico
vault write auth/github/map/teams/Default value=default
```

---

## 4. Comandos de Inspeção

Para listar e validar o que foi criado:

```bash
# Ver políticas existentes
vault policy list

# Ler o conteúdo de uma política
vault policy read backstage-policy

# Ver uma Role de Kubernetes
vault read auth/kubernetes/role/backstage-role

# Ver mapeamentos do GitHub
vault list auth/github/map/teams
```

---

> [!IMPORTANT]
> Lembre-se que o Vault utiliza o modelo **Deny by Default**. Se algo não está explicitamente permitido em uma política, o acesso será negado.
