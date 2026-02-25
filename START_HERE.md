# 🎯 START HERE - PicoClaw Coolify Deployment

## Welcome! 👋

Este projecto está **100% pronto** para fazer deploy no Coolify. Este ficheiro é o teu ponto de partida.

---

## ⚡ Versão Rápida (5 minutos de leitura)

```
1. Tens chaves/tokens? (OpenRouter, Telegram, GitHub PAT)
   ↓
2. Executa: bash /tmp/push-to-github.sh
   ↓
3. Vai a: https://pico.louvps01.my.id:3000 (Coolify Dashboard)
   ↓
4. Segue: COOLIFY_DEPLOYMENT.md
   ↓
5. Deploy automático, bot Telegram funciona! ✅
```

---

## 📚 Documentação (Ordem de Leitura)

### 1️⃣ **QUICK_START.txt** ← COMEÇA AQUI
- Visão geral dos 4 passos principais
- Tempo estimado: 40 minutos
- Comandos úteis e troubleshooting rápido

### 2️⃣ **GITHUB_PUSH.md**
- Como fazer push para GitHub
- Opções de autenticação
- Verificação de sucesso

### 3️⃣ **COOLIFY_DEPLOYMENT.md** ← GUIA PRINCIPAL
- Passo-a-passo detalhado do setup no Coolify
- Screenshots/descrições de cada campo
- Troubleshooting completo
- FAQ

### 4️⃣ **DEPLOYMENT_SUMMARY.md**
- Arquitetura técnica detalhada
- Segurança e boas práticas
- Timeline e próximos passos

---

## 🔑 Pré-requisitos (Chaves/Tokens)

Precisas de 3 coisas:

### 1. GitHub PAT (Personal Access Token)
```
Vai a: https://github.com/settings/tokens
├─ Generate new token (classic)
├─ Nome: picoclaw-coolify
├─ Scope: repo
└─ Copia o token (só vês uma vez!)
```

### 2. OpenRouter API Key
```
Vai a: https://openrouter.ai/keys
├─ Criar conta
├─ Gerar nova API key
└─ Copia a chave
```

### 3. Telegram Bot Token
```
Vai a: https://t.me/BotFather
├─ /start
├─ /newbot
├─ Segue instruções
└─ Copia o token
```

---

## ✅ O Que Já Está Pronto

```
✓ Código compilável (Go + Docker)
✓ Dockerfile otimizado
✓ config.json configurado para Telegram + OpenRouter
✓ .env.example com variáveis necessárias
✓ Documentação completa (4 ficheiros)
✓ Commit já feito (aguarda push)
✓ Segurança implementada (API keys só em Coolify)
```

---

## 🚀 Próximos Passos (Ordem)

### Passo 1: GitHub Push
```bash
bash /tmp/push-to-github.sh
```
Tempo: ~2 minutos

### Passo 2: Setup Coolify
1. Abre: https://pico.louvps01.my.id:3000
2. Lê: COOLIFY_DEPLOYMENT.md
3. Segue guia passo-a-passo
Tempo: ~15 minutos

### Passo 3: Build & Deploy
Coolify faz automaticamente
Tempo: ~10 minutos

### Passo 4: Teste
```bash
curl https://pico.louvps01.my.id:18790/health  # Health check
# Envia mensagem no Telegram                     # Teste bot
```
Tempo: ~5 minutos

**TOTAL: ~40 minutos**

---

## 📋 Ficheiros Importantes

```
PicoClaw/
├── QUICK_START.txt              ← Resumo rápido (ESTE)
├── COOLIFY_DEPLOYMENT.md        ← Guia principal ⭐
├── GITHUB_PUSH.md              ← Como fazer push
├── DEPLOYMENT_SUMMARY.md        ← Detalhes técnicos
├── config/config.json           ← Config local (não commitado)
├── .env.example                 ← Template de vars
├── Dockerfile                   ← Build config
└── ...
```

---

## 🔐 Segurança (Importante!)

```
GitHub (Público)              Coolify UI (Privada)
────────────────              ──────────────────
config.json com               OPENROUTER_API_KEY
placeholders                  TELEGRAM_BOT_TOKEN
${OPENROUTER_API_KEY}         TZ
${TELEGRAM_BOT_TOKEN}

✓ API keys NUNCA entram no Git
✓ Substituição automática em runtime
✓ Ficheiros sensíveis em .gitignore
```

---

## ❓ FAQ Rápido

**P: Posso ver exemplos do setup?**
R: Vê COOLIFY_DEPLOYMENT.md

**P: Onde estão as minhas API keys?**
R: Guardadas apenas em Coolify UI (seguro)

**P: E se eu alterar código depois?**
R: Git push → Coolify detecta → Redeploy automático

**P: Como faço backup?**
R: Coolify tem backup integrado (S3-compatible)

---

## 📞 Ajuda

Se ficares preso:

1. **Lê QUICK_START.txt** - Troubleshooting rápido
2. **Lê COOLIFY_DEPLOYMENT.md** - Guia passo-a-passo
3. **Verifica logs em Coolify Dashboard**
4. **Google o erro específico**

---

## 🎉 Resumo

Fizemos:
- ✅ Análise completa do projeto
- ✅ Plano de deployment robusto
- ✅ Configuração segura
- ✅ Documentação profissional
- ✅ Commit preparado

Você precisa de:
- 1️⃣ Fazer push para GitHub
- 2️⃣ Setup no Coolify
- 3️⃣ Testar
- 🎉 Aproveitar o bot!

---

## ✨ Próximo Passo

**Lê: QUICK_START.txt** ou vai directamente para **COOLIFY_DEPLOYMENT.md** se já tiveres feito push para GitHub.

---

**Boa sorte! 🚀**

*PicoClaw + OpenRouter + Telegram + Coolify = Automação IA no teu Home Server*
