# Guia de Setup — Noite 1 (do zero à primeira mensagem no WhatsApp)

*Passo a passo para quem nunca configurou VPS/Docker. Tempo total estimado: **3h30–4h30**, incluindo imprevistos. Os arquivos de configuração já estão prontos em [`infra/`](infra/) neste repositório — vocês só preenchem senhas.*

**Como usar este guia:** façam juntos, na ordem, sem pular checkpoint. Quem tem base de infra (Matheus) dirige o terminal; o outro lê o passo em voz alta e confere o checkpoint. Se algo der erro, copiem a mensagem de erro exata e me mandem na conversa — não fiquem 1h presos tentando adivinhar.

---

## Antes de sentar (comprem/tenham em mãos)

- [ ] **Chip pré-pago novo (~R$ 20)** ativado num celular que possa ficar com vocês (pode ser um celular velho no Wi-Fi). É o número da demo — **nunca usem o número pessoal** na Evolution API (risco de banimento pela Meta).
- [ ] Cartão de crédito para o VPS.
- [ ] Um computador com terminal (Windows: use o PowerShell ou instale o Windows Terminal; o `ssh` já vem no Windows 10/11).

---

## Passo 0 — Contas da empresa (~30 min)

1. **Criem um Gmail da startup** (ex.: `nomedastartup@gmail.com`). TODAS as contas a seguir nascem nele, não no e-mail pessoal de nenhum dos dois — sócio não pode virar ponto único de falha.
2. **Criem uma conta no Bitwarden** (bitwarden.com, grátis) com esse Gmail e instalem a extensão no navegador dos dois. **Regra desde já: toda senha criada hoje vai para o Bitwarden na hora.** Compartilhem o acesso entre os dois (organização gratuita do Bitwarden permite 2 usuários).

✅ **Checkpoint:** os dois conseguem abrir o cofre do Bitwarden.

## Passo 1 — Contratar o VPS (~20 min)

1. Em [hostinger.com.br](https://www.hostinger.com.br/vps), escolham **KVM 2** (2 vCPU / 8 GB RAM é folga boa; o KVM 1 com 4 GB também roda esta stack, se quiserem economizar).
2. **Período: mensal, sem fidelidade** (~R$ 80–110/mês, estimativa). Sai mais caro por mês que o plano de 24 meses (~R$ 55), mas vocês ainda não validaram nada — migrem para o plano longo quando a landing page estiver no ar.
3. Na configuração: **Ubuntu 24.04 LTS "limpo"** (NÃO escolham os templates com painel/aplicativo pré-instalado — vamos montar a stack nós mesmos, do jeito que vamos saber consertar). Datacenter: **São Paulo/Brasil**, se oferecido.
4. Definam a **senha root** (gerada forte, direto pro Bitwarden). Se o painel oferecer adicionar **chave SSH**, pulem por enquanto — geramos no Passo 2.
5. Anotem o **IP do servidor** que aparece no painel (algo como `191.x.x.x`).

✅ **Checkpoint:** painel da Hostinger mostra o VPS "Rodando" e vocês têm IP + senha root no Bitwarden.

## Passo 2 — Primeiro acesso e segurança (~40 min)

No terminal do computador de vocês:

```bash
ssh root@IP_DO_SERVIDOR
```

(Responda `yes` à pergunta sobre fingerprint; digite a senha root — ela não aparece enquanto digita, é normal.)

Agora, no servidor, rodem em ordem:

```bash
# 1. Atualizar o sistema (pode levar alguns minutos)
apt update && apt upgrade -y

# 2. Firewall: só SSH, HTTP e HTTPS entram
apt install -y ufw fail2ban
ufw allow OpenSSH
ufw allow 80/tcp
ufw allow 443/tcp
ufw --force enable

# 3. fail2ban bloqueia quem fica chutando senha no SSH (já sobe ativo)
systemctl enable --now fail2ban

# 4. Atualizações de segurança automáticas
apt install -y unattended-upgrades
dpkg-reconfigure -plow unattended-upgrades   # escolha "Yes"
```

**Agora a chave SSH** (elimina login por senha, o maior vetor de invasão). No **computador de vocês** (novo terminal, sem fechar o do servidor):

```bash
ssh-keygen -t ed25519 -C "startup"      # Enter em tudo (ou definam uma passphrase e guardem no Bitwarden)
ssh-copy-id root@IP_DO_SERVIDOR         # Windows sem ssh-copy-id? Veja nota abaixo
ssh root@IP_DO_SERVIDOR                 # deve entrar SEM pedir senha
```

> **Windows:** se `ssh-copy-id` não existir, rodem `type $env:USERPROFILE\.ssh\id_ed25519.pub` no PowerShell, copiem a linha inteira e, no servidor, colem dentro de `~/.ssh/authorized_keys` (criem com `mkdir -p ~/.ssh && nano ~/.ssh/authorized_keys`).

Só depois que o login **sem senha funcionou**, desativem o login por senha — no servidor:

```bash
sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
systemctl restart ssh
```

> ⚠️ **Não fechem a sessão atual antes de testar**: abram um segundo terminal e confirmem que `ssh root@IP` ainda entra (pela chave). Se se trancarem para fora, o painel da Hostinger tem um "console de emergência" no navegador.

✅ **Checkpoint:** `ssh root@IP` entra sem pedir senha; `ufw status` mostra apenas 22, 80 e 443 liberadas.

## Passo 3 — Docker (~10 min)

No servidor:

```bash
curl -fsSL https://get.docker.com | sh
docker --version && docker compose version
```

✅ **Checkpoint:** os dois comandos respondem com números de versão.

## Passo 4 — Endereços grátis no DuckDNS (~10 min)

Enquanto não existe o domínio da startup, o DuckDNS dá subdomínios grátis com HTTPS:

1. Entrem em [duckdns.org](https://www.duckdns.org) (login com o Gmail da startup).
2. Criem **dois** subdomínios — ex.: `sorriso-n8n` e `sorriso-evo` (usem qualquer nome disponível; troca-se depois pelo domínio real).
3. Nos dois, coloquem o **IP do VPS** no campo "current ip" e salvem.

✅ **Checkpoint:** `ping sorriso-n8n.duckdns.org` (no computador de vocês) responde com o IP do VPS. Se não responder ainda, esperem 5 min — DNS demora a propagar.

## Passo 5 — Subir a stack (n8n + Evolution API + Postgres + Redis + HTTPS) (~30 min)

No servidor:

```bash
apt install -y git
git clone https://github.com/oleodias/automation_startup.git
cd automation_startup/infra
cp .env.example .env
nano .env
```

No `.env`, preencham (as instruções estão comentadas no próprio arquivo):
- `N8N_DOMAIN` e `EVO_DOMAIN` → os dois subdomínios do Passo 4;
- as três chaves/senhas → gerem cada uma com `openssl rand -hex 32` (rodem o comando três vezes, num segundo terminal) e **salvem as três no Bitwarden antes de continuar** — em especial a `N8N_ENCRYPTION_KEY`: perdê-la = perder as credenciais salvas no n8n.

Salvem (`Ctrl+O`, Enter, `Ctrl+X`) e subam tudo:

```bash
docker compose up -d
docker compose ps        # todos os serviços devem ficar "running"/"healthy" em ~1 min
```

Criem o banco da Evolution API (o compose só cria o do n8n):

```bash
docker compose exec postgres psql -U automacao -c 'CREATE DATABASE evolution;'
docker compose restart evolution
```

✅ **Checkpoint:** `https://SEU-n8n.duckdns.org` abre no navegador com cadeado (HTTPS) mostrando a tela de criação de conta do n8n, e `https://SEU-evo.duckdns.org` responde (um JSON de status). O certificado pode levar ~2 min no primeiro acesso — o Caddy o emite sozinho.
Criem a **conta de dono do n8n** (e-mail da startup + senha forte → Bitwarden).

> **Se algo não subir:** `docker compose logs NOME_DO_SERVICO --tail 50` mostra o erro. Copiem e me mandem.

## Passo 6 — Conectar o chip na Evolution API (~20 min)

1. No celular com o chip novo, instalem o **WhatsApp normal** e ativem o número (SMS de verificação). Preencham nome e foto de perfil — número "pelado" tem cara de spam.
2. No navegador: `https://SEU-evo.duckdns.org/manager` → login com a `EVOLUTION_API_KEY` do `.env`.
3. Criem uma instância chamada `demo` → aparece um **QR Code** → no celular do chip: WhatsApp → Configurações → Dispositivos conectados → Conectar dispositivo → escaneiem.

✅ **Checkpoint:** a instância `demo` aparece como **conectada** (verde) no manager.

> 🔥 **Aquecimento do número (importante):** nos primeiros 2–3 dias, usem o chip como pessoa normal — mandem mensagens manuais para os números de vocês, respondam, entrem num grupo. Número novo que já nasce disparando via API é o alvo nº 1 de banimento. Só liguem as automações de verdade nele a partir da semana que vem.

## Passo 7 — "Hello world": a primeira mensagem automática (~20 min)

1. No n8n (`https://SEU-n8n.duckdns.org`): **New workflow**.
2. Adicionem um nó **Manual Trigger** (quando clicar em "Execute", roda).
3. Adicionem um nó **HTTP Request** ligado a ele, configurado assim:
   - **Method:** `POST`
   - **URL:** `https://SEU-evo.duckdns.org/message/sendText/demo`
   - **Headers:** adicionar header `apikey` = a `EVOLUTION_API_KEY`
   - **Body (JSON):**
     ```json
     {
       "number": "5551XXXXXXXXX",
       "text": "Estamos no ar! 🚀 — enviada automaticamente pelo n8n"
     }
     ```
     (no `number`: o celular pessoal de um de vocês, com 55 + DDD, só dígitos)
4. Cliquem **Execute workflow**.

✅ **Checkpoint final da noite:** a mensagem chegou no WhatsApp pessoal. **A fundação está pronta.** 🎉

## Passo 8 — Fechamento (~15 min)

- [ ] No painel da Hostinger, criem um **snapshot/backup** do VPS (agora que está tudo configurado).
- [ ] Confirmem no Bitwarden: senha root, chaves SSH (onde estão), `.env` completo, senha do n8n, API key da Evolution — **tudo lá, os dois com acesso**.
- [ ] Me avisem aqui na conversa que o hello world passou → a próxima sessão começa a **A1 (confirmação de consultas)** direto, com o passo a passo dos 5 fluxos.

---

## Problemas comuns (consultar antes de entrar em pânico)

| Sintoma | Causa provável | Solução |
|---|---|---|
| `ssh: connection refused` | VPS ainda inicializando ou IP errado | Esperar 2 min; conferir IP no painel |
| Site abre sem HTTPS / erro de certificado | DNS ainda propagando ou porta 80/443 fechada | Esperar 5–10 min; `ufw status`; `docker compose logs caddy` |
| n8n abre mas Evolution não | Banco `evolution` não criado | Rodar o `CREATE DATABASE evolution;` do Passo 5 e `docker compose restart evolution` |
| QR Code não conecta / expira | QR velho ou celular sem internet | Gerar novo QR no manager; chip precisa de dados/Wi-Fi ativos |
| Imagem `evolution-api` não baixa | Nome/tag mudou | Conferir imagem atual em [doc.evolution-api.com](https://doc.evolution-api.com) e ajustar no compose |
| Mensagem do hello world não chega | Número mal formatado ou instância desconectada | `number` = 55 + DDD + número, só dígitos; manager deve mostrar `demo` verde |
| Tudo caiu depois de reiniciar o VPS | Containers sem restart | Não é o caso deste compose (`restart: unless-stopped`), mas: `cd ~/automation_startup/infra && docker compose up -d` |

**Comandos do dia a dia:** `docker compose ps` (estado), `docker compose logs -f n8n` (logs ao vivo), `docker compose restart NOME` (reiniciar um serviço), `docker compose pull && docker compose up -d` (atualizar versões — façam isso com calma, não em véspera de demo).
