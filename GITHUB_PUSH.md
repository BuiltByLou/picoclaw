# 📤 Como Fazer Push para GitHub

## Pré-requisitos
- Commit já foi criado ✅
- Tens um GitHub Personal Access Token

## Passo 1: Obter GitHub Personal Access Token (PAT)

Se ainda não tens, cria um novo:

1. Vai a: https://github.com/settings/tokens
2. Clica "Generate new token" → "Generate new token (classic)"
3. Nome: `picoclaw-coolify`
4. Scope: Marca apenas `repo` (acesso a repositórios)
5. Clica "Generate token"
6. **Copia o token** (só consegues ver uma vez!)

## Passo 2: Fazer Push

### Opção A: Usando o script (Recomendado)

```bash
bash /tmp/push-to-github.sh
```

Depois cola o teu PAT quando pedir.

### Opção B: Manual com Git

```bash
cd /home/lou/Dev/picoclaw

# Substitui SEU_PAT_AQUI com o teu token real
git push https://SEU_PAT_AQUI@github.com/BuiltByLou/picoclaw.git main
```

### Opção C: Guardar Token Localmente (Mais Seguro)

```bash
# 1. Configura git credential helper
git config --global credential.helper store

# 2. Faz push (irá pedir token, guarda para futuro)
cd /home/lou/Dev/picoclaw
git push origin main

# 3. Preenche quando pedir:
# Username: BuiltByLou
# Password: SEU_PAT_AQUI
```

## Verificar Push

Depois de fazer push, verifica em:

```
https://github.com/BuiltByLou/picoclaw/commits/main
```

Deves ver o commit:
```
feat: add Coolify deployment configuration
```

## Troubleshooting

**Erro: "Invalid username or token"**
- Token expirou? Cria um novo em https://github.com/settings/tokens
- Token errado? Verifica se copiaste corretamente
- Escopo insuficiente? Cria novo com scope `repo`

**Erro: "Authentication failed"**
- Verifica username: `BuiltByLou` (case-sensitive)
- Verifica se tens internet
- Tenta novamente

## Depois do Push ✅

Podes começar o setup no Coolify!

Vê: `COOLIFY_DEPLOYMENT.md`
