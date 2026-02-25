# 🚀 Guia de Deployment: PicoClaw no Coolify

## Visão Geral

Este guia mostra como fazer deploy do PicoClaw no Coolify com integração GitHub automática.

**Fluxo:**
```
GitHub (repo)  →  Coolify  →  Telegram Bot
   ↓
Auto-deploy em cada push
```

---

## FASE 1: Preparação no GitHub

### Pré-requisitos
- ✅ Repositório GitHub: https://github.com/BuiltByLou/picoclaw
- ✅ Ficheiro `config.json` com placeholders (já criado)
- ✅ `.env.example` atualizado (já criado)

### Verificação
```bash
cd /home/lou/Dev/picoclaw

# Verificar ficheiros de config
git status

# Deves ver:
# - config/config.json (novo/modificado)
# - .env.example (modificado)
# - Nenhum ficheiro `.env` real!
```

---

## FASE 2: Fazer Push para GitHub

```bash
cd /home/lou/Dev/picoclaw

# Adicionar os ficheiros
git add config/config.json .env.example

# Commit
git commit -m "feat: add Coolify deployment configuration with OpenRouter and Telegram"

# Push para main
git push origin main
```

**Resultado esperado:**
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 450 bytes | 450.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
To https://github.com/BuiltByLou/picoclaw.git
   abc1234..def5678  main -> main
```

---

## FASE 3: Setup no Coolify UI

### Passo 1: Ir ao Dashboard Coolify

```
Abre browser → http://localhost:3000 (ou o IP do teu home server)
Login no Coolify
```

### Passo 2: Criar Novo Projeto (se não existe)

```
Dashboard → Projects
├─ Create New Project
└─ Nome: "PicoClaw" (ou outro)
```

### Passo 3: Criar Aplicação (New Resource)

```
Projects → PicoClaw → Create New Resource
```

**Opções de deployment:**
- ✅ **Public Repository** ← ESCOLHE ISTO
- Deploy Key (alternativa)
- Github App (alternativa)

### Passo 4: Configurar Git Repository

**Campo:**
```
Repository URL (copy & paste)
→ https://github.com/BuiltByLou/picoclaw
```

**Resultado esperado:**
Coolify valida a URL e detecta a branch `main`

### Passo 5: Escolher Build Pack

```
Build Pack Selector:
Defaut: Nixpacks → Muda para: Dockerfile ✅
```

**Configuração:**
```
Branch:           main
Base Directory:   /
Dockerfile:       ./Dockerfile
```

**Clica: Continue**

### Passo 6: Configurar Portas & Network

```
Port Exposed:     18790
Domain:           (deixa em branco se só acesso local)
Hostname:         (não preenchido)
```

**Clica: Continue**

### Passo 7: Adicionar Environment Variables

**Na tab: Environment Variables**

```
Variable Name              | Value                          | Build Var?
─────────────────────────────────────────────────────────────────────
OPENROUTER_API_KEY        | sk-or-v1-xxxxx...             | ☐
TELEGRAM_BOT_TOKEN        | 123456:ABCDEFghij...          | ☐
TZ                        | Europe/Lisbon                 | ☐
```

**Como adicionar:**
1. Clica "Add Environment Variable"
2. Preenche "Variable Name"
3. Preenche "Value"
4. IMPORTANTE: NÃO marca "Build Variable" (estes são runtime vars)
5. Clica ✓ para confirmar
6. Repete para as 3 variáveis

**Resultado esperado:**
```
✓ OPENROUTER_API_KEY = sk-or-v1-xxxxx...
✓ TELEGRAM_BOT_TOKEN = 123456:ABCDEFghij...
✓ TZ = Europe/Lisbon
```

### Passo 8: Configurar Storage (Volumes)

**Na tab: Persistent Storage**

```
Clica: Add Persistent Volume
├─ Volume Name: workspace
└─ Mount Path: /home/picoclaw/.picoclaw/workspace

Clica: Save
```

**Resultado esperado:**
```
✓ workspace → /home/picoclaw/.picoclaw/workspace
```

### Passo 9: Deploy!

```
Clica: "Deploy" ou "Deploy Now"

Status esperado:
1. Building...  (5-10 minutos)
2. Deploying... (1-2 minutos)
3. ✓ Running    (green checkmark)
```

---

## FASE 4: Verificar o Deploy

### Verificação 1: Health Check

```bash
# No teu home server, testa:
curl http://localhost:18790/health

# Resultado esperado:
# {"status": "ok"} ou similar
```

### Verificação 2: Logs do Coolify

```
Coolify Dashboard → PicoClaw App → Logs
```

Procura por:
```
✓ "PicoClaw started successfully"
✓ "Telegram bot connected"
✓ "Workspace initialized"
✗ "ERROR" ou "FAILED" → Problema!
```

### Verificação 3: Testar no Telegram

1. Abre Telegram
2. Encontra o bot que criaste (https://t.me/BotFather)
3. Envia mensagem: `/start`
4. Envia pergunta: `Olá, quem és?`
5. Espera resposta do PicoClaw

**Sucesso!** 🎉

---

## FASE 5: Updates Automáticos

### Como atualizar o código

```bash
# No teu home server:
cd /home/lou/Dev/picoclaw

# 1. Faz as tuas alterações
vim pkg/agent/agent.go  # Exemplo

# 2. Commit e push
git add .
git commit -m "fix: melhorar resposta do agent"
git push origin main

# 3. Coolify detecta automaticamente!
#    → Vai fazer rebuild e redeploy
#    → Telegram continua funcionando (quase sem downtime)
```

**Monitorizar deploy:**
```
Coolify Dashboard → PicoClaw → Deployments
```

---

## Troubleshooting

### Problema: "Build Failed"

**Solução:**
1. Verifica os logs (Coolify → Logs tab)
2. Procura por `ERROR`
3. Common issues:
   - Dockerfile corrompido? Valida com `docker build .`
   - Falta de espaço disco? Verifica: `df -h`
   - Go version? Dockerfile usa `golang:1.25-alpine`

### Problema: "No Available Server" / 503 Error

**Solução:**
1. Verifica se health check passa: `curl http://localhost:18790/health`
2. Verifica logs: Aplicação está a crash?
3. Aguarda 30 segundos (health check demora)

### Problema: Telegram Bot não responde

**Solução:**
1. Verifica `TELEGRAM_BOT_TOKEN` correto
2. Verifica se está habilitado em `config.json`: `"enabled": true`
3. Testa: `curl -X POST http://localhost:18790/health` (vê se app está vivo)
4. Verifica logs para mensagens de erro

### Problema: Dados sumiram após restart

**Solução:**
- Isso significa volume `workspace` não foi criado corretamente
- Vai a: Coolify → PicoClaw → Persistent Storage
- Verifica se volume `workspace` existe
- Se não, cria novamente e redeploy

---

## FAQ

**P: As minhas API keys estão seguras?**
R: Sim! Estão apenas no Coolify (ui privada do home server). Nunca entram no Git. O `config.json` tem apenas placeholders `${OPENROUTER_API_KEY}`.

**P: Como mudo de OpenRouter para OpenAI?**
R: 
1. Edita `config/config.json` e muda o `model_name`
2. Adiciona `OPENAI_API_KEY` em Coolify
3. Git push → Coolify redeploy automático

**P: Posso usar múltiplos bots (Discord + Telegram)?**
R: Sim! Edita `config.json` e habilita mais channels. Depois adiciona as respetivas API keys em Coolify.

**P: O que acontece se o meu home server cair?**
R: Container para. Quando liga novamente, Coolify reinicia automaticamente. Dados do workspace (persistidos no volume) são preservados.

---

## Próximos Passos

- ✅ Deploy iniciado no Coolify
- ✅ Telegram bot funcionando
- ⏭️ Adicionar mais providers (Discord, Slack, etc)
- ⏭️ Configurar backups do workspace
- ⏭️ Explorar features avançadas do PicoClaw (skills, cron jobs, etc)

---

**Última atualização:** 2026-02-25
**PicoClaw version:** latest (from GitHub)
