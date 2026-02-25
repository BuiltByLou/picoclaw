# 📋 Resumo de Deployment: PicoClaw no Coolify

**Data:** 2026-02-25
**Status:** ✅ Pronto para Deploy

---

## 🎯 O Que Fizemos

### ✅ Ficheiros Criados/Atualizados

```
/home/lou/Dev/picoclaw/
├── ✅ config/config.json
│   └─ Config customizado com placeholders ${VAR}
│      (não commitado - local only)
│
├── ✅ .env.example (ATUALIZADO)
│   └─ Template com 3 variáveis principais:
│      • OPENROUTER_API_KEY
│      • TELEGRAM_BOT_TOKEN
│      • TZ
│
├── ✅ COOLIFY_DEPLOYMENT.md (NOVO)
│   └─ Guia completo passo-a-passo (LEIA ISTO)
│
├── ✅ GITHUB_PUSH.md (NOVO)
│   └─ Como fazer push para GitHub
│
└── ✅ DEPLOYMENT_SUMMARY.md (ESTE FICHEIRO)
    └─ Overview de todo o setup
```

---

## 📊 Arquitetura de Deployment

```
┌─────────────────┐         ┌──────────────────────┐
│   GitHub Repo   │         │  Home Server         │
│                 │         │                      │
│ ├─ Dockerfile   │─ Pull ─→│  Coolify Dashboard   │
│ ├─ config.json* │         │  ├─ Build & Deploy   │
│ ├─ .env.example │         │  └─ Env Vars:        │
│ └─ código       │         │     • OPENROUTER_KEY │
└─────────────────┘         │     • TELEGRAM_TOKEN │
                            │     • TZ             │
                            └──────────────────────┘
                                      ↓
                            ┌──────────────────────┐
                            │  Container Running   │
                            │  port: 18790         │
                            │  volume: workspace/  │
                            └──────────────────────┘
                                      ↓
                            ┌──────────────────────┐
                            │  Telegram Bot        │
                            │  (Always On)         │
                            └──────────────────────┘

* config.json: Ignorado por git, criado localmente
  com valores reais (NUNCA push para GitHub)
```

---

## 🔐 Segurança: Como Protegemos as API Keys

```
GitHub (Público)           ↔    Coolify UI (Privada)
─────────────────────────────────────────────────────

config.json:               Coolify Environment:
{                          ✓ OPENROUTER_API_KEY
  "api_key":                  = sk-or-v1-xxxxx
  "${OPENROUTER_API_KEY}"   ✓ TELEGRAM_BOT_TOKEN
}                              = 123456:ABC...
                            ✓ TZ
Placeholders                   = Europe/Lisbon
SEM valores reais!


Fluxo em Runtime:
1. GitHub → Coolify pull
2. Coolify lê config.json (com placeholders)
3. Coolify substitui ${VAR} com valores da UI
4. Container inicia com valores reais
5. API calls funcionam ✅

Resultado: API keys NUNCA entram no Git!
```

---

## 🚀 Próximos Passos (Checklist)

### Fase 1: Push para GitHub
- [ ] Tens PAT (Personal Access Token) pronto?
- [ ] Correr: `bash /tmp/push-to-github.sh`
- [ ] Verificar: https://github.com/BuiltByLou/picoclaw/commits/main

### Fase 2: Setup no Coolify (LEIA: COOLIFY_DEPLOYMENT.md)
- [ ] Abrir Coolify Dashboard (http://localhost:3000)
- [ ] Criar novo projeto "PicoClaw"
- [ ] Adicionar nova aplicação (GitHub public repo)
- [ ] Configurar git URL, Dockerfile, ports
- [ ] Adicionar 3 Environment Variables
- [ ] Criar volume "workspace"
- [ ] Deploy!

### Fase 3: Verificação
- [ ] Health check: `curl http://localhost:18790/health`
- [ ] Ver logs no Coolify
- [ ] Testar Telegram: Enviar mensagem ao bot
- [ ] Confirmar resposta IA

---

## 📝 Ficheiros Importantes

### Para o Utilizador (LEIA ESTES)
1. **COOLIFY_DEPLOYMENT.md** ← GUIA PRINCIPAL
   - Passo-a-passo detalhado do setup no Coolify
   - Screenshots/descrições de cada passo
   - Troubleshooting

2. **GITHUB_PUSH.md** ← COMO FAZER PUSH
   - Obter GitHub PAT
   - Fazer push com segurança

### Para Referência (Técnico)
- **.env.example** - Template de variáveis
- **config/config.json** - Config para OpenRouter + Telegram
- **.gitignore** - Garante que `.env` real nunca entra no Git

---

## 🔧 Configuração Técnica

### LLM Provider
```
Provider: OpenRouter
Model: Claude 3.5 Sonnet (anthropic/claude-3.5-sonnet)
API Base: https://openrouter.ai/api/v1
Cost: ~$0.003/1K input tokens
```

### Chat Channel
```
Platform: Telegram
Bot Token: Configurado em Coolify UI
Health Check: Disponível em :18790/health
```

### Storage
```
Volume Name: workspace
Mount Path: /home/picoclaw/.picoclaw/workspace
Persistence: ✅ Sobrevive reboots/redeployments
```

---

## 💡 Features

### Automação
```
git push → GitHub → Coolify detects → Build → Deploy
         (⏱️ ~10 minutos)
```

### Escalabilidade
```
RAM: 32-64MB (muito leve)
CPU: 10-50m (quase nada)
Disco: 50MB + workspace storage
```

### Monitoring
```
Health Check: GET /health → OK
Logs: Acessível em Coolify Dashboard
Restart Policy: unless-stopped
```

---

## ❓ FAQ Rápido

**P: E se eu alterar o código?**
R: Git push → Coolify detecta → Redeploy automático ✅

**P: E se o servidor reiniciar?**
R: Container para → Coolify reinicia → Volume workspace preservado ✅

**P: Posso mudar para Discord?**
R: Sim! Edita config.json, adiciona DISCORD_BOT_TOKEN em Coolify, push. Redeploy automático ✅

**P: Onde posso ver os logs?**
R: Coolify Dashboard → PicoClaw App → Logs tab

**P: Como faço backup do workspace?**
R: Coolify tem backup integrado (S3-compatible). Ou copia manualmente: `/home/picoclaw/.picoclaw/workspace/`

---

## 📞 Suporte

Se tiver dúvidas durante o setup:

1. **Coolify não consegue fazer build?**
   - Vê logs em Coolify Dashboard → Logs
   - Valida Dockerfile: `docker build .`

2. **Telegram bot não responde?**
   - Verifica TELEGRAM_BOT_TOKEN em Coolify
   - Verifica se está habilitado em config.json

3. **Workspace está vazio?**
   - Verifica se volume foi criado em Coolify
   - Valida mount path: `/home/picoclaw/.picoclaw/workspace`

4. **Preciso de ajuda?**
   - PicoClaw Docs: https://github.com/sipeed/picoclaw
   - Coolify Docs: https://coolify.io/docs
   - Esta repo: https://github.com/BuiltByLou/picoclaw

---

## 🎉 Timeline Esperado

```
Agora         → Push para GitHub (5 minutos)
Fase 1        → Setup Coolify UI (15 minutos)
Fase 1 + 10m  → Build & Deploy (10 minutos)
Fase 1 + 25m  → ✅ Teste no Telegram (5 minutos)
─────────────────────────────────────────────
TOTAL: ~40 minutos
```

---

## ✅ Status Geral

| Item | Status | Nota |
|------|--------|------|
| Código | ✅ | Sem mudanças necessárias |
| Config Coolify | ✅ | Pronto em `config/config.json` |
| Env Variables | ✅ | Template em `.env.example` |
| Segurança | ✅ | API keys só em Coolify UI |
| Documentação | ✅ | COOLIFY_DEPLOYMENT.md completo |
| GitHub Push | ⏳ | Aguarda PAT + execução |
| Coolify Setup | ⏳ | Aguarda após push |
| Teste Telegram | ⏳ | Após deploy bem-sucedido |

---

**PRÓXIMO PASSO:** Lê `COOLIFY_DEPLOYMENT.md` e começa o setup no Coolify! 🚀
