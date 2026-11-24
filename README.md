<p align="center">
  <img src="assets/moltbot-migration.jpg" alt="ClawdBot â†’ MoltBot" width="100%">
</p>

# ðŸ¤– Docker Moltbot

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


<p align="center">
  <img src="assets/moltbot-robot.jpg" alt="MoltBot" width="400">
</p>

[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![Node.js](https://img.shields.io/badge/Node.js-22-339933?logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Security Hardened](https://img.shields.io/badge/Security-Hardened-green?logo=shieldsdotio)](SECURITY.md)
[![Docs](https://img.shields.io/badge/Docs-docs.molt.bot-blue)](https://docs.molt.bot)
[![Discord](https://img.shields.io/badge/Discord-Join-5865F2?logo=discord&logoColor=white)](https://discord.gg/clawd)

Setup Docker para o [Moltbot](https://molt.bot) â€” um assistente pessoal de IA com seguranÃ§a reforÃ§ada de fÃ¡brica.

> **Moltbot** (antigo Clawdbot) Ã© um assistente de IA que roda na sua prÃ³pria mÃ¡quina. Ele se conecta a LLMs (Claude, GPT, Gemini, etc.), tem ferramentas (busca na web, execuÃ§Ã£o de cÃ³digo, gerenciamento de arquivos, controle de navegador) e conversa com vocÃª via Telegram, WhatsApp, Discord, webchat e mais. Este repo te dÃ¡ um setup Docker pronto pra rodar com boas prÃ¡ticas de seguranÃ§a jÃ¡ configuradas.

> âš ï¸ **Nota (Jan 2026):** O rebrand para Moltbot estÃ¡ em andamento. O pacote npm `moltbot` Ã© atualmente um shim de compatibilidade. Este setup Docker instala o `clawdbot` (o runtime completo) e vai trocar automaticamente para `moltbot` quando o pacote completo for publicado. NÃ£o precisa fazer nada â€” Ã© sÃ³ rebuildar quando uma nova versÃ£o for anunciada.

---

## âœ¨ Funcionalidades

- ðŸ”’ **SeguranÃ§a reforÃ§ada** â€” segue o [Checklist Top 10 de SeguranÃ§a](SECURITY.md)
- ðŸ³ **Setup em um comando** â€” `docker compose up -d`
- ðŸ” **Secrets via variÃ¡veis de ambiente** â€” sem credenciais em texto puro nos arquivos de config
- ðŸ‘¤ **Container nÃ£o-root** â€” roda como usuÃ¡rio sem privilÃ©gios `moltbot`
- ðŸ“ **Logging habilitado** â€” trilha de auditoria por padrÃ£o
- ðŸ“± **Multi-canal** â€” Telegram, WhatsApp, Discord, Slack, webchat e mais
- ðŸŽ™ï¸ **TranscriÃ§Ã£o de Ã¡udio** â€” Faster Whisper incluso (opcional)
- ðŸ› ï¸ **Ferramentas integradas** â€” acesso ao shell, busca na web, controle de navegador, execuÃ§Ã£o de cÃ³digo
- ðŸ§  **200+ modelos via OpenRouter** â€” Claude, GPT, Gemini, Llama, DeepSeek e mais com uma Ãºnica API key
- ðŸªŸ **CompatÃ­vel com Windows** â€” `.gitattributes` forÃ§a terminaÃ§Ãµes LF, Dockerfile corrige CRLF
- ðŸ”„ **Auto-restart** â€” polÃ­tica de reinÃ­cio `unless-stopped`
- ðŸŒ **Rede segura** â€” gateway vinculado ao loopback, isolamento de rede Docker disponÃ­vel

---

## ðŸ“‹ PrÃ©-requisitos

| Plataforma | Requisito | InstalaÃ§Ã£o |
|------------|-----------|------------|
| **Windows** | Docker Desktop | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) |
| **Windows** | Git | [git-scm.com](https://git-scm.com/download/win) |
| **Mac** | Docker Desktop | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) |
| **Linux** | Docker Engine + Compose | `curl -fsSL https://get.docker.com \| sh` |

> âš ï¸ **UsuÃ¡rios Windows:** Certifique-se de que o **Docker Desktop estÃ¡ rodando** antes de continuar. Verifique o Ã­cone do Docker (ðŸ³) na bandeja do sistema. Se o WSL mostrar `docker-desktop Stopped`, abra o Docker Desktop pelo menu Iniciar e espere atÃ© aparecer "Docker is running".

> âš ï¸ **UsuÃ¡rios Windows:** Se vocÃª nunca usou Docker antes, talvez precise habilitar o **WSL 2** primeiro. O Docker Desktop vai pedir para instalar â€” Ã© sÃ³ seguir as instruÃ§Ãµes e reiniciar o computador quando solicitado.

---

## ðŸš€ InÃ­cio RÃ¡pido

### Passo 1: Clone o repo

**Linux / Mac:**
```bash
git clone https://github.com/linnoff88-dev/docker-moltbot.git
cd docker-moltbot
```

**Windows (CMD):**
```cmd
git clone https://github.com/linnoff88-dev/docker-moltbot.git
cd docker-moltbot
```

**Windows (PowerShell):**
```powershell
git clone https://github.com/linnoff88-dev/docker-moltbot.git
cd docker-moltbot
```

### Passo 2: Configure o ambiente

**Linux / Mac:**
```bash
cp .env.example .env
nano .env  # Fill in your API keys
```

**Windows (CMD):**
```cmd
copy .env.example .env
notepad .env
```

**Windows (PowerShell):**
```powershell
Copy-Item .env.example .env
notepad .env
```

> âš ï¸ **IMPORTANTE:** VocÃª **PRECISA** criar E editar o arquivo `.env` antes de rodar `docker compose up`. O container nÃ£o vai funcionar com os valores de exemplo.

Abra o arquivo `.env` e substitua os valores de exemplo pelas suas chaves reais:

```env
# âŒ ERRADO â€” esses sÃ£o placeholders, nÃ£o vÃ£o funcionar:
OPENROUTER_API_KEY=sk-or-your-key-here
GATEWAY_AUTH_TOKEN=your-secure-token-here

# âœ… CERTO â€” suas chaves reais:
OPENROUTER_API_KEY=sk-or-v1-abc123-your-actual-real-key
GATEWAY_AUTH_TOKEN=a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4e5f6
DEFAULT_MODEL=anthropic/claude-sonnet-4-5
```

### Passo 3: Gere um token seguro para o gateway

O `GATEWAY_AUTH_TOKEN` protege a API do seu gateway contra acesso nÃ£o autorizado. Gere um aleatÃ³rio:

**Linux / Mac:**
```bash
openssl rand -hex 24
```

**Windows (PowerShell):**
```powershell
-join ((1..48) | ForEach-Object { '{0:x}' -f (Get-Random -Max 16) })
```

**Ou** use qualquer string aleatÃ³ria longa (pelo menos 24 caracteres). VocÃª pode usar um gerenciador de senhas para gerar uma.

Copie o token gerado para o seu arquivo `.env` como `GATEWAY_AUTH_TOKEN`.

### Passo 4: Configure sua chave OpenRouter

Este setup usa **OpenRouter** como gateway unificado de LLMs. Com uma Ãºnica API key, vocÃª tem acesso a 200+ modelos:

| Categoria | Modelos DisponÃ­veis |
|-----------|---------------------|
| **Claude** | Sonnet 4.5, Opus 4, Haiku 3.5, etc. |
| **GPT** | GPT-4o, GPT-4 Turbo, o1-preview, etc. |
| **Gemini** | Gemini 2.0 Flash (FREE), Pro, etc. |
| **Open Source** | Llama 3.3 70B (FREE), DeepSeek R1 (FREE), Qwen, etc. |
| **Especializados** | Mistral, Claude, Grok, e mais |

**Obtenha sua chave:**
1. Acesse [openrouter.ai](https://openrouter.ai/)
2. FaÃ§a login com Google/GitHub
3. VÃ¡ para [Keys](https://openrouter.ai/keys) e crie uma nova API key
4. Copie a key (comeÃ§a com `sk-or-v1-...`)
5. Cole no seu `.env` como `OPENROUTER_API_KEY`

**Escolha seu modelo padrÃ£o:**

Edite `DEFAULT_MODEL` no `.env` para escolher qual modelo usar:

```env
# OpÃ§Ã£o 1: Claude Sonnet 4.5 (melhor qualidade, recomendado)
DEFAULT_MODEL=anthropic/claude-sonnet-4-5

# OpÃ§Ã£o 2: Claude 3.5 Sonnet (Ã³timo custo-benefÃ­cio)
DEFAULT_MODEL=anthropic/claude-3.5-sonnet

# OpÃ§Ã£o 3: GPT-4o (excelente para cÃ³digo)
DEFAULT_MODEL=openai/gpt-4o

# OpÃ§Ã£o 4: Gemini 2.0 Flash (GRÃTIS, muito rÃ¡pido)
DEFAULT_MODEL=google/gemini-2.0-flash-exp

# OpÃ§Ã£o 5: Llama 3.3 70B (GRÃTIS, open source)
DEFAULT_MODEL=meta-llama/llama-3.3-70b-instruct
```

> ðŸ’¡ **Dica:** VocÃª pode trocar de modelo a qualquer momento editando o `.env` e executando `docker compose restart`

> ðŸ’° **Modelos GrÃ¡tis:** Gemini 2.0 Flash, Llama 3.3 70B e DeepSeek R1 sÃ£o totalmente gratuitos no OpenRouter. Perfeito para testar!

> ðŸ“Š **PreÃ§os e limites:** Consulte [openrouter.ai/models](https://openrouter.ai/models) para ver preÃ§os, limites de contexto e velocidade de cada modelo.

### Passo 5: Build e execuÃ§Ã£o

```bash
docker compose up -d
```

> ðŸ’¡ **Primeira execuÃ§Ã£o** leva alguns minutos para buildar a imagem (baixa Node.js, FFmpeg, Python, etc). As execuÃ§Ãµes seguintes iniciam instantaneamente.

> âš ï¸ **Erro no Windows `open //./pipe/dockerDesktopLinuxEngine`?** O Docker Desktop nÃ£o estÃ¡ rodando. Abra-o pelo menu Iniciar e espere atÃ© mostrar "Docker is running", depois tente novamente.

> âœ¨ **ConfiguraÃ§Ã£o automÃ¡tica:** O script `entrypoint.sh` detecta automaticamente qual provedor de LLM vocÃª configurou no `.env` e cria a configuraÃ§Ã£o ideal. VocÃª **nÃ£o precisa** editar arquivos JSON manualmente!

### Passo 6: Acesse o Webchat

Abra no seu navegador:
```
http://localhost:18789/chat
```

Quando solicitado, digite seu `GATEWAY_AUTH_TOKEN` do arquivo `.env` para autenticar.

> ðŸ’¡ **Dica:** VocÃª tambÃ©m pode acessar diretamente com: `http://localhost:18789/?token=SEU_TOKEN`

### Passo 7: Verifique o status

```bash
# Acompanhe os logs (Ctrl+C para parar)
docker compose logs -f

# Ou veja sÃ³ as Ãºltimas 50 linhas
docker compose logs --tail 50
```

VocÃª deve ver uma saÃ­da como:
```
ðŸ¤– First run â€” creating config from template...
ðŸ”‘ Setting gateway auth token...
ðŸ“± Enabling Telegram...
ðŸŒ Setting gateway bind to lan (required for Docker port mapping)...
ðŸ“ Enabling logging...
ðŸ¤– Starting Moltbot...
```

### Passo 8: ConfiguraÃ§Ã£o pÃ³s-instalaÃ§Ã£o

Depois que o container estiver rodando, use esses comandos para ajustar seu setup:

```bash
# Auto-detecte e corrija problemas de config
docker exec moltbot clawdbot doctor --fix

# Verifique a saÃºde geral
docker exec moltbot clawdbot status

# Execute uma auditoria de seguranÃ§a
docker exec moltbot clawdbot security audit
```

| Comando | O que faz |
|---------|-----------|
| `clawdbot doctor --fix` | Auto-detecta e corrige problemas de config (ex: Telegram configurado mas nÃ£o habilitado) |
| `clawdbot doctor` | Mesma verificaÃ§Ã£o, mas sÃ³ **mostra** os problemas sem corrigir |
| `clawdbot status` | Mostra status do gateway, canais conectados, info do modelo |
| `clawdbot security audit` | Verifica seu setup contra boas prÃ¡ticas de seguranÃ§a |

> ðŸ’¡ **Nota:** O comando CLI ainda Ã© `clawdbot` nesta versÃ£o (2026.1.24-3). A migraÃ§Ã£o para `moltbot` estÃ¡ em andamento.

---

## ðŸ“± ConfiguraÃ§Ã£o do Telegram

O Telegram Ã© a forma mais fÃ¡cil de falar com seu Moltbot de qualquer lugar.

### Passo a passo:

1. **Crie um bot** â€” Abra o Telegram e mande mensagem pro [@BotFather](https://t.me/BotFather)
2. **Envie `/newbot`** â€” Siga as instruÃ§Ãµes para nomear seu bot
3. **Copie o token** â€” O BotFather te dÃ¡ um token tipo `123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11`
4. **Adicione ao `.env`:**
   ```env
   TELEGRAM_BOT_TOKEN=123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11
   ```
5. **Reinicie o container:**
   ```bash
   docker compose restart
   ```
6. **Mande mensagem pro seu bot** no Telegram â€” ele vai te dar um **cÃ³digo de pareamento**
7. **Aprove o pareamento** dentro do container:
   ```bash
   docker exec moltbot clawdbot pairing approve telegram <code>
   ```

> ðŸ’¡ O sistema de pareamento garante que sÃ³ usuÃ¡rios aprovados possam falar com seu bot. Ã‰ uma funcionalidade de seguranÃ§a â€” sem aprovaÃ§Ã£o, o bot nÃ£o responde a estranhos.

### Opcional: Configure as opÃ§Ãµes do BotFather

Ainda no BotFather, vocÃª tambÃ©m pode:
- `/setdescription` â€” Adicionar uma descriÃ§Ã£o pro seu bot
- `/setabouttext` â€” Adicionar texto "Sobre"
- `/setuserpic` â€” Definir uma foto de perfil
- `/setcommands` â€” Definir comandos do bot (opcional)

---

## ðŸ“² ConfiguraÃ§Ã£o do WhatsApp

VocÃª pode conectar o Moltbot ao WhatsApp via pareamento por QR code.

### Passo a passo:

1. **Execute o comando de login:**
   ```bash
   docker exec -it moltbot clawdbot channels login whatsapp
   ```
2. **Escaneie o QR code** com seu WhatsApp (ConfiguraÃ§Ãµes â†’ Aparelhos Conectados â†’ Conectar um Aparelho)
3. **Pronto!** Seu Moltbot agora estÃ¡ conectado ao WhatsApp

### âš ï¸ NÃºmero Pessoal vs. NÃºmero Dedicado

**Usando seu nÃºmero pessoal:**
- O bot vai ver mensagens de todos os seus contatos
- Por padrÃ£o, a polÃ­tica de DM Ã© `pairing` â€” outros podem receber um prompt de pareamento
- **Recomendado:** Defina `dmPolicy: allowlist` com apenas o seu nÃºmero:

```json
{
  "channels": {
    "whatsapp": {
      "selfChatMode": true,
      "dmPolicy": "allowlist",
      "allowFrom": ["+5511999999999"]
    }
  }
}
```

**Usando um nÃºmero dedicado (recomendado):**
- Pegue um chip prÃ©-pago barato ou um nÃºmero do Google Voice
- Instale o WhatsApp num celular secundÃ¡rio ou use o WhatsApp Web
- SeparaÃ§Ã£o mais limpa entre mensagens pessoais e do bot

> ðŸ’¡ **Modo self-chat:** Fale consigo mesmo no WhatsApp â€” mensagens para o seu prÃ³prio nÃºmero vÃ£o pro Moltbot. NinguÃ©m mais Ã© afetado.

---

## ðŸ”’ SeguranÃ§a

Este setup Docker implementa **7 de 10** medidas de hardening de seguranÃ§a automaticamente. Veja o [SECURITY.md](SECURITY.md) para o checklist completo.

### O que o Docker faz automaticamente:
| ProteÃ§Ã£o | Status |
|----------|--------|
| Gateway vinculado apenas ao `127.0.0.1` (lado do host) | âœ… AutomÃ¡tico |
| PolÃ­tica de DM requer aprovaÃ§Ã£o de pareamento | âœ… AutomÃ¡tico |
| Arquivos de config com `chmod 600` | âœ… AutomÃ¡tico |
| Container roda como usuÃ¡rio nÃ£o-root | âœ… AutomÃ¡tico |
| Sem escalaÃ§Ã£o de privilÃ©gios (`no-new-privileges`) | âœ… AutomÃ¡tico |
| Logging e diagnÃ³sticos habilitados | âœ… AutomÃ¡tico |
| Secrets via variÃ¡veis de ambiente | âœ… AutomÃ¡tico |

### O que VOCÃŠ deve fazer:
- [ ] Configurar o `AGENTS.md` para bloquear comandos perigosos (veja [SECURITY.md](SECURITY.md))
- [ ] Revisar o acesso a ferramentas MCP e restringir ao mÃ­nimo necessÃ¡rio
- [ ] Considerar rede `internal: true` se nÃ£o precisar de internet (bloqueia chamadas de API tambÃ©m)

### Auditoria de seguranÃ§a:
```bash
docker exec moltbot clawdbot security audit
```

### Modelo de ameaÃ§as (simplificado)

**O que o Moltbot pode fazer:**
- Executar comandos shell no container
- Ler/escrever arquivos no workspace
- Fazer requisiÃ§Ãµes HTTP (chamadas de API, busca na web)
- Controlar um navegador (se configurado)
- Enviar mensagens nos canais conectados

**O que atacantes podem tentar:**
- **InjeÃ§Ã£o de prompt:** Enganar o bot via conteÃºdo web ou mensagens maliciosas
- **Escape de shell:** Fazer o bot rodar comandos perigosos (`rm -rf /`, `curl | bash`)
- **Roubo de token:** Roubar chaves de API dos configs ou logs
- **Acesso nÃ£o autorizado:** Mandar mensagem pro bot sem aprovaÃ§Ã£o
- **ExposiÃ§Ã£o de rede:** Acessar o gateway de fora do localhost

### Vulnerabilidades comuns e mitigaÃ§Ãµes:

| Vulnerabilidade | Risco | MitigaÃ§Ã£o |
|----------------|-------|-----------|
| **InjeÃ§Ã£o de prompt** | MÃ©dio | Moltbot envolve conteÃºdo nÃ£o-confiÃ¡vel em tags de seguranÃ§a; configure o AGENTS.md para bloquear padrÃµes perigosos |
| **Acesso ao shell** | MÃ©dio | Isolamento do container + usuÃ¡rio nÃ£o-root; bloqueie `rm -rf`, `curl \| bash` no AGENTS.md |
| **Logs de sessÃ£o em texto puro** | Baixo | Logs ficam dentro de volumes Docker com permissÃµes restritas; habilite `redactSensitive` na config |
| **Plugins nÃ£o verificados** | Baixo | Instale plugins apenas de fontes confiÃ¡veis; revise permissÃµes de ferramentas MCP |
| **WhatsApp com nÃºmero pessoal** | MÃ©dio | Use dmPolicy `allowlist` ou um nÃºmero dedicado |
| **ExposiÃ§Ã£o de rede** | Baixo | Gateway vinculado a 127.0.0.1; use tÃºnel SSH ou Tailscale para acesso remoto |
| **Controle de navegador** | MÃ©dio | Navegador roda em sandbox; restrinja apenas a sites confiÃ¡veis |

---

## ðŸ“¦ Volumes

Volumes Docker persistem seus dados entre reinÃ­cios e rebuilds do container.

| Volume | Caminho no Container | Finalidade |
|--------|---------------------|------------|
| `moltbot-data` | `/home/moltbot/.clawdbot` | Config, dados de sessÃ£o, tokens de auth, info de pareamento |
| `moltbot-workspace` | `/home/moltbot/workspace` | Workspace do agente â€” AGENTS.md, arquivos de memÃ³ria, arquivos de projeto |
| `moltbot-logs` | `/home/moltbot/logs` | Arquivos de log (NÃƒO em /tmp â€” sobrevive a reinÃ­cios) |

> ðŸ’¡ **Nota:** O diretÃ³rio de config ainda Ã© `~/.clawdbot` nesta versÃ£o. SerÃ¡ migrado para `~/.moltbot` em versÃµes futuras.

### FaÃ§a backup dos seus dados:
```bash
# Backup de todos os volumes
docker run --rm -v moltbot-data:/data -v $(pwd):/backup alpine tar czf /backup/moltbot-data.tar.gz -C /data .
docker run --rm -v moltbot-workspace:/data -v $(pwd):/backup alpine tar czf /backup/moltbot-workspace.tar.gz -C /data .
```

### Resetar tudo:
```bash
docker compose down -v  # âš ï¸ Apaga TODOS os dados incluindo config e workspace
```

---

## ðŸ› ï¸ Comandos Ãšteis

```bash
# === Ciclo de vida ===
docker compose up -d              # Start in background
docker compose down               # Stop and remove container
docker compose restart            # Restart
docker compose stop               # Stop without removing

# === Logs ===
docker compose logs -f            # Follow logs (Ctrl+C to stop)
docker compose logs --tail 100    # Last 100 lines

# === Acesso ao shell ===
docker compose exec moltbot bash  # Open shell inside container

# === Clawdbot CLI ===
docker exec moltbot clawdbot status          # Gateway status
docker exec moltbot clawdbot doctor --fix    # Auto-fix issues
docker exec moltbot clawdbot security audit  # Security check

# === Atualizar Moltbot ===
docker compose build --no-cache   # Rebuild image (pulls latest moltbot)
docker compose up -d              # Restart with new image

# === Limpeza ===
docker system prune -a            # Remove unused images (reclaim disk space)
```

---

## ðŸŒ Isolamento de Rede

Por padrÃ£o, o container tem acesso Ã  internet â€” isso Ã© **necessÃ¡rio** para chamadas de API Ã  Anthropic, OpenAI, etc.

### Isolamento total (sem internet):
```yaml
# No docker-compose.yml, altere:
networks:
  moltbot-net:
    internal: true  # No internet access
```

> âš ï¸ **AtenÃ§Ã£o:** Isso bloqueia TODAS as conexÃµes de saÃ­da, incluindo chamadas de API para provedores de LLM. Use apenas se vocÃª tiver um **setup de modelo local** (ex: Ollama rodando na mesma rede).

### Isolamento parcial (permitir apenas hosts especÃ­ficos):
Para usuÃ¡rios avanÃ§ados, use polÃ­ticas de rede Docker ou regras iptables para permitir apenas endpoints de API especÃ­ficos.

---

## ðŸ“¡ Canais de Acesso

O Moltbot suporta mÃºltiplos canais de comunicaÃ§Ã£o simultaneamente. Todos os canais compartilham o mesmo agente, memÃ³ria e workspace.

| Canal | Tipo | Acesso | Dificuldade de Setup | Melhor Para |
|-------|------|--------|---------------------|-------------|
| ðŸ“± **Telegram** | Mensagens | Qualquer lugar (mobile/desktop) | â­ FÃ¡cil | Uso diÃ¡rio, acesso rÃ¡pido |
| ðŸ“² **WhatsApp** | Mensagens | Qualquer lugar (mobile/desktop) | â­ FÃ¡cil | Se vocÃª jÃ¡ usa WhatsApp |
| ðŸ’¬ **Webchat** | Interface Web | Rede local / VPN | â­ FÃ¡cil | Interface rica, upload de arquivos |
| ðŸŒ **Webchat (pÃºblico)** | Interface Web | Qualquer lugar | â­â­â­ AvanÃ§ado | Bot pÃºblico |
| ðŸ”’ **Tailscale** | VPN | Qualquer lugar (zero-trust) | â­â­ MÃ©dio | Acesso remoto mais seguro |
| ðŸ’œ **Discord** | Mensagens | Qualquer lugar | â­â­ MÃ©dio | Times, comunidades |
| ðŸ’¼ **Slack** | Mensagens | Qualquer lugar | â­â­ MÃ©dio | Trabalho/empresarial |
| ðŸ”µ **Signal** | Mensagens | Qualquer lugar | â­â­â­ AvanÃ§ado | Privacidade mÃ¡xima |
| ðŸŸ¢ **Matrix** | Mensagens | Qualquer lugar | â­â­â­ AvanÃ§ado | Self-hosted, federado |

### Qual devo usar?

- **Setup mais simples:** Telegram â€” um token de bot e pronto
- **Mais privado:** Signal ou Tailscale + Webchat
- **Acesso de qualquer lugar sem apps extras:** Telegram + WhatsApp (vocÃª jÃ¡ tem)
- **Melhor para times/trabalho:** Slack ou Discord
- **Webchat remoto mais seguro:** Tailscale â€” VPN zero-trust, sem portas abertas

### Multi-canal

VocÃª pode habilitar **mÃºltiplos canais simultaneamente**. Todos os canais compartilham o mesmo agente, memÃ³ria e workspace. Mensagens de qualquer canal chegam no mesmo assistente.

> âš ï¸ **Mensagens entre canais sÃ£o restritas** por design â€” o bot nÃ£o vai vazar dados de conversa entre canais.

---

## ðŸ–¥ï¸ Acesso ao Webchat (Remoto)

O gateway Ã© vinculado ao `127.0.0.1` (apenas loopback). Para acessar o webchat de outra mÃ¡quina, use um **tÃºnel SSH**:

```bash
# Na sua mÃ¡quina local (a que tem o navegador):
ssh -L 18789:localhost:18789 user@your-server-ip

# Depois abra no navegador:
# http://127.0.0.1:18789/chat
```

Esta Ã© a forma mais segura de acessar a interface web remotamente â€” sem portas expostas, todo trÃ¡fego criptografado via SSH.

### Alternativa: Tailscale (recomendado para uso frequente)

Se vocÃª acessa o webchat com frequÃªncia, configure o [Tailscale](https://tailscale.com) para acesso VPN transparente:

```bash
# No seu servidor:
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up

# Nos seus dispositivos:
# Instale o app Tailscale, faÃ§a login com a mesma conta
# Acesse o webchat via: http://<tailscale-ip>:18789/chat
```

---

## ðŸ§° Ferramentas e Skills Recomendados

Turbine seu Moltbot com estas ferramentas adicionais:

### ðŸ›  Ferramentas CLI

| Ferramenta | InstalaÃ§Ã£o | Finalidade |
|------------|------------|------------|
| [Codex CLI](https://github.com/openai/codex) | `npm i -g @openai/codex` | Agente de cÃ³digo com IA (OpenAI) |
| [agent-browser](https://github.com/vercel-labs/agent-browser) | `npm i -g agent-browser` | AutomaÃ§Ã£o de navegador headless |
| FFmpeg | PrÃ©-instalado na imagem Docker | Processamento de Ã¡udio/vÃ­deo |
| Faster Whisper | PrÃ©-instalado na imagem Docker | TranscriÃ§Ã£o local de Ã¡udio |

### ðŸŽ¨ ServiÃ§os de API

| ServiÃ§o | Finalidade | PreÃ§o |
|---------|------------|-------|
| [OpenRouter](https://openrouter.ai) | Gateway para mÃºltiplos LLMs (modelos gratuitos disponÃ­veis) | Tier gratuito + pay-per-use |
| [Kie.ai](https://kie.ai) | GeraÃ§Ã£o de imagem, vÃ­deo e mÃºsica (Veo 3.1, Flux, Suno) | CrÃ©ditos |
| [ElevenLabs](https://elevenlabs.io) | Text-to-speech (vozes realistas) | Tier gratuito + pago |
| [Gamma](https://gamma.app) | ApresentaÃ§Ãµes e documentos com IA | Tier gratuito + pago |
| [HeyGen](https://heygen.com) | Avatares de vÃ­deo com IA | CrÃ©ditos |

### ðŸ“š Skills (para Codex / Claude Code)

| Skill | InstalaÃ§Ã£o | Finalidade |
|-------|------------|------------|
| [Remotion Skills](https://github.com/linnoff88-dev/remotion-skills) | Copiar para `.codex/skills/` | Criar vÃ­deos programaticamente com React |

```bash
# Instalar Remotion Skills para Codex
docker exec moltbot bash -c '
  git clone https://github.com/linnoff88-dev/remotion-skills.git /tmp/remotion-skills
  mkdir -p .codex/skills
  cp -r /tmp/remotion-skills/skills/remotion .codex/skills/
'
```

---

## ðŸ§  OrganizaÃ§Ã£o de LLMs

EstratÃ©gia recomendada de modelos para diferentes tarefas:

| Modelo | Provedor | Nome do Modelo | Caso de Uso | Custo |
|--------|----------|----------------|-------------|-------|
| **Claude Opus 4.5** | Anthropic | `anthropic/claude-opus-4-5` | Assistente principal â€” conversas, tarefas complexas (padrÃ£o) | Pago (API) |
| **Claude 3.5 Sonnet** | Anthropic | `anthropic/claude-3-5-sonnet-20241022` | Alternativa mais rÃ¡pida e econÃ´mica | Pago (API) |
| **GPT-4** | OpenAI | `openai/gpt-4` | GeraÃ§Ã£o de cÃ³digo, anÃ¡lise | Pago (API) |
| **Gemini 2.0 Flash** | Google | `google/gemini-2.0-flash` | Tarefas rÃ¡pidas, consultas simples | Tier gratuito disponÃ­vel |

> âš ï¸ **Importante:** Use os **nomes exatos** dos modelos listados acima. Modelos como `claude-sonnet-4.5` (sem versÃ£o) nÃ£o existem e causarÃ£o erro.

> ðŸ’¡ **PadrÃ£o:** Se vocÃª nÃ£o especificar um modelo, o Anthropic usarÃ¡ automaticamente `claude-opus-4-5` como padrÃ£o.

### Como mudar o modelo

Se vocÃª quiser usar um modelo diferente do padrÃ£o, use o script `change-model.sh`:

```bash
# Mudar para Claude 3.5 Sonnet (mais rÃ¡pido e econÃ´mico)
./change-model.sh "anthropic/claude-3-5-sonnet-20241022"

# Ou mudar diretamente:
docker exec moltbot sh -c "cat > /tmp/change-model.js << 'EOJS'
const fs = require('fs');
const cfg = JSON.parse(fs.readFileSync('/home/moltbot/.clawdbot/clawdbot.json', 'utf8'));
cfg.agents = cfg.agents || {};
cfg.agents.defaults = cfg.agents.defaults || {};
cfg.agents.defaults.model = { primary: 'anthropic/claude-3-5-sonnet-20241022' };
fs.writeFileSync('/home/moltbot/.clawdbot/clawdbot.json', JSON.stringify(cfg, null, 2));
EOJS
node /tmp/change-model.js"

docker compose restart
```

> âš ï¸ **Use nomes exatos:** Modelos devem incluir o prefixo do provedor (ex: `anthropic/`, `openai/`, `google/`) e a versÃ£o completa.

---

## ðŸ’» Requisitos

| Recurso | MÃ­nimo | Recomendado |
|---------|--------|-------------|
| **RAM** | 2 GB | 4 GB (com Whisper) |
| **Disco** | 5 GB | 10+ GB |
| **CPU** | 1 core | 2+ cores |
| **Docker** | Engine 24+ / Compose v2+ | Ãšltima versÃ£o estÃ¡vel |
| **SO** | Linux, macOS, Windows 10+ | Ubuntu 22.04+ / macOS 13+ |
| **Rede** | Acesso Ã  internet | Banda larga estÃ¡vel |

---

## ðŸ”§ SoluÃ§Ã£o de Problemas

### Problemas no Windows

| Erro | Causa | SoluÃ§Ã£o |
|------|-------|---------|
| `open //./pipe/dockerDesktopLinuxEngine: O sistema nÃ£o pode encontrar o arquivo` | Docker Desktop nÃ£o estÃ¡ rodando | Abra o Docker Desktop pelo menu Iniciar, espere por "Docker is running" |
| `.env not found` | Arquivo de config faltando | Execute `copy .env.example .env` e depois `notepad .env` |
| `the attribute version is obsolete` | Formato antigo do docker-compose | Aviso inofensivo â€” ignore (este repo nÃ£o usa `version:`) |
| `WSL docker-desktop Stopped` | WSL nÃ£o iniciou | Abra o Docker Desktop â€” ele inicia o WSL automaticamente |
| Build trava ou falha | RAM insuficiente | Docker Desktop â†’ Settings â†’ Resources â†’ aumente para 4GB+ |
| `exec entrypoint.sh: no such file or directory` | TerminaÃ§Ãµes de linha CRLF do Windows | Clone o repo novamente: `git config --global core.autocrlf input` e depois `git clone` |
| `npm ERR! Error while executing` | Problemas de rede/proxy | Verifique sua conexÃ£o com a internet; se estiver atrÃ¡s de proxy, configure as opÃ§Ãµes de proxy do Docker |

### Problemas no Linux / Mac

| Erro | Causa | SoluÃ§Ã£o |
|------|-------|---------|
| `permission denied` | NÃ£o estÃ¡ no grupo docker | `sudo usermod -aG docker $USER` e depois **faÃ§a logout e login novamente** |
| `port already in use` | Outro serviÃ§o na porta 18789 | Mude a porta no `docker-compose.yml` ou pare o outro serviÃ§o |
| `no space left on device` | Disco cheio | `docker system prune -a` para limpar imagens antigas |
| Build falha em Mac ARM | Incompatibilidade de arquitetura | Geralmente funciona; se tiver problemas, tente `docker compose build --platform linux/amd64` |

### Problemas do Docker / Container

| Erro | Causa | SoluÃ§Ã£o |
|------|-------|---------|
| `exec entrypoint.sh: no such file or directory` | TerminaÃ§Ãµes de linha CRLF no entrypoint.sh | **Corrigido automaticamente** pelo Dockerfile (`sed -i 's/\r$//'`). Se ainda acontecer: abra o `entrypoint.sh` no VS Code â†’ mude CRLF para LF (canto inferior direito) â†’ salve â†’ rebuild |
| `error: unknown option '--foreground'` | Sintaxe de comando antiga | O CMD deve ser `["moltbot", "gateway", "run"]` â€” atualize seu Dockerfile |
| `npm error: spawn git ENOENT` | Git nÃ£o estÃ¡ na imagem Docker | Git estÃ¡ incluÃ­do neste Dockerfile. Se estiver usando uma imagem customizada, adicione `git` no `apt-get install` |
| Container fica reiniciando | VÃ¡rios â€” verifique os logs | `docker compose logs --tail 50` e procure o erro |
| Gateway vincula a 127.0.0.1 dentro do container | Bind padrÃ£o Ã© loopback | **Corrigido automaticamente** pelo entrypoint.sh (define `bind: "lan"`). Docker precisa de 0.0.0.0 dentro, mas o `docker-compose.yml` restringe acesso do host a 127.0.0.1 |
| Logs em /tmp desaparecem | tmpfs limpa ao reiniciar | Logs sÃ£o armazenados no volume `/home/moltbot/logs` (NÃƒO em /tmp). Isso jÃ¡ Ã© o padrÃ£o. |

### Problemas Gerais

| Problema | SoluÃ§Ã£o |
|----------|---------|
| Bot nÃ£o responde mensagens | Verifique os logs: `docker compose logs -f`. Confirme que as chaves de API e token do bot estÃ£o corretos. |
| Erros de API / rate limiting | Verifique se as chaves de API no `.env` estÃ£o corretas e tÃªm crÃ©ditos |
| NÃ£o consegue acessar o webchat remotamente | Use tÃºnel SSH: `ssh -L 18789:localhost:18789 user@server` |
| Bot responde devagar | Verifique sua conexÃ£o com a internet; considere um modelo de LLM mais rÃ¡pido |
| Mensagem "Pairing required" | Isso Ã© esperado â€” aprove com `clawdbot pairing approve <channel> <code>` |
| MudanÃ§as de config nÃ£o aplicadas | Reinicie: `docker compose restart` |

### Problemas de ConfiguraÃ§Ã£o

| Problema | Causa | SoluÃ§Ã£o |
|----------|-------|---------|
| **"Unknown model: anthropic/claude-sonnet-4.5"** | Nome de modelo invÃ¡lido | Use `anthropic/claude-opus-4-5` ou `anthropic/claude-3-5-sonnet-20241022`. O modelo `claude-sonnet-4.5` nÃ£o existe. |
| **Bot nÃ£o responde apÃ³s mudar o modelo** | Config corrompida ou modelo invÃ¡lido | Execute `docker compose down -v` (âš ï¸ apaga dados), depois `docker compose up -d` para comeÃ§ar do zero |
| **MudanÃ§as no .env nÃ£o aplicadas** | `docker compose restart` nÃ£o recarrega variÃ¡veis de ambiente | Use `docker compose down && docker compose up -d` para recarregar o `.env` |
| **Config manual nÃ£o funciona** | EdiÃ§Ã£o manual causou inconsistÃªncias | âš ï¸ **NÃƒO edite** `/home/moltbot/.clawdbot/clawdbot.json` manualmente. Deixe o `entrypoint.sh` configurar automaticamente via variÃ¡veis de ambiente no `.env` |
| **OpenRouter nÃ£o funciona** | Esta versÃ£o tem suporte limitado ao OpenRouter | Use Anthropic, OpenAI ou Google diretamente. OpenRouter serÃ¡ melhor suportado em versÃµes futuras. |

> ðŸ’¡ **Melhor prÃ¡tica:** Configure tudo via arquivo `.env` e deixe o `entrypoint.sh` fazer a configuraÃ§Ã£o automÃ¡tica. Evite editar `clawdbot.json` manualmente!

---

## ðŸ”„ MigraÃ§Ã£o do Clawdbot

Se vocÃª estÃ¡ atualizando do setup antigo `docker-clawdbot`, aqui estÃ¡ o que mudou:

### O que Ã© diferente:

| Antigo (Clawdbot) | Novo (Moltbot) |
|-------------------|----------------|
| Pacote: `clawdbot` (npm) | Pacote: `moltbot` (npm) |
| CLI: `clawdbot` | CLI: `moltbot` |
| Comando: `clawdbot gateway start --foreground` | Comando: `moltbot gateway run` |
| Repo: `linnoff88-dev/docker-clawdbot` | Repo: `linnoff88-dev/docker-moltbot` |
| Docs: `docs.clawd.bot` | Docs: `docs.molt.bot` (redirects funcionam) |
| Dir de config: `~/.clawdbot` | Dir de config: `~/.moltbot` (com fallback para `~/.clawdbot`) |
| UsuÃ¡rio: `clawdbot` | UsuÃ¡rio: `moltbot` |
| Container: `clawdbot` | Container: `moltbot` |

### Passos da migraÃ§Ã£o:

1. **FaÃ§a backup dos seus dados:**
   ```bash
   # Do setup antigo
   cd docker-clawdbot
   docker run --rm -v clawdbot-data:/data -v $(pwd):/backup alpine tar czf /backup/clawdbot-data-backup.tar.gz -C /data .
   docker run --rm -v clawdbot-workspace:/data -v $(pwd):/backup alpine tar czf /backup/clawdbot-workspace-backup.tar.gz -C /data .
   ```

2. **Pare o container antigo:**
   ```bash
   cd docker-clawdbot
   docker compose down
   ```

3. **Clone o novo repo:**
   ```bash
   git clone https://github.com/linnoff88-dev/docker-moltbot.git
   cd docker-moltbot
   ```

4. **Copie seu .env:**
   ```bash
   cp ../docker-clawdbot/.env .env
   ```

5. **Inicie o novo container:**
   ```bash
   docker compose up -d
   ```

6. **Restaure os dados (opcional):**
   ```bash
   # Restaurar workspace
   docker run --rm -v moltbot-workspace:/data -v $(pwd):/backup alpine tar xzf /backup/clawdbot-workspace-backup.tar.gz -C /data
   ```

### Notas de compatibilidade:
- O pacote npm antigo `clawdbot` agora Ã© um **shim** que redireciona para `moltbot`
- Seus arquivos de config existentes sÃ£o compatÃ­veis â€” o Moltbot faz fallback para `~/.clawdbot` se `~/.moltbot` nÃ£o existir
- Tokens de bot do Telegram, aprovaÃ§Ãµes de pareamento e chaves de API sÃ£o mantidos sem alteraÃ§Ã£o
- Pode ser necessÃ¡rio **re-parear** em alguns canais apÃ³s a migraÃ§Ã£o

### Problemas conhecidos corrigidos nesta versÃ£o:
- âœ… Pacote `git` incluÃ­do no Dockerfile (estava faltando â†’ npm install falhava)
- âœ… TerminaÃ§Ãµes de linha CRLF corrigidas automaticamente pelo Dockerfile + `.gitattributes` (causava "no such file" no Windows)
- âœ… Gateway vincula a `lan` dentro do container (estava vinculando a 127.0.0.1 â†’ inacessÃ­vel do host)
- âœ… Comando correto: `moltbot gateway run` (era `clawdbot gateway start --foreground`)
- âœ… Logs em volume `/home/moltbot/logs` (estava em /tmp â†’ perdido ao reiniciar)
- âœ… `.dockerignore` e `.gitignore` incluÃ­dos

---

## ðŸ¤ Contribuindo

PRs sÃ£o bem-vindos! Siga o checklist de seguranÃ§a em [SECURITY.md](SECURITY.md).

1. FaÃ§a fork do repo
2. Crie uma branch de feature: `git checkout -b my-feature`
3. FaÃ§a commit das suas mudanÃ§as: `git commit -m 'Add my feature'`
4. FaÃ§a push: `git push origin my-feature`
5. Abra um Pull Request

### Diretrizes:
- Mantenha as boas prÃ¡ticas de seguranÃ§a em mente
- Teste no Linux e Windows se possÃ­vel
- Atualize a documentaÃ§Ã£o para qualquer mudanÃ§a voltada ao usuÃ¡rio
- Siga o estilo de cÃ³digo existente

---

## ðŸ“œ LicenÃ§a

[MIT](LICENSE) â€” use como quiser.

---

<p align="center">
  <a href="https://molt.bot">molt.bot</a> â€¢
  <a href="https://docs.molt.bot">DocumentaÃ§Ã£o</a> â€¢
  <a href="https://discord.gg/clawd">Discord</a> â€¢
  <a href="https://github.com/moltbot/moltbot">GitHub</a>
</p>

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>