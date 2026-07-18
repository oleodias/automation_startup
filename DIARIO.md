# Diário de Bordo — ModernEasy Automation

## Noite 1 — 17→18/07/2026 (setup de infraestrutura)

**Status: infraestrutura 100% no ar.** Feito pelo Leonardo (Matheus acompanhou o início).

### O que está pronto
- ✅ VPS Hostinger contratado (Ubuntu 24.04, IP `93.127.212.39`)
- ✅ Segurança: sistema atualizado, firewall (só portas 22/80/443), fail2ban, atualizações automáticas, chave SSH do Leonardo instalada
- ✅ Docker + Docker Compose instalados
- ✅ DNS no DuckDNS: `moderneasyn8n.duckdns.org` e `moderneasyevo.duckdns.org` → IP do VPS
- ✅ Stack completa rodando (`infra/docker-compose.yml`): Caddy (HTTPS) + PostgreSQL + Redis + n8n + Evolution API
- ✅ n8n acessível com conta de dono criada
- ✅ Manager da Evolution acessível; instância `demo` criada (aguardando QR)
- ✅ Gmail da startup + Bitwarden com todas as chaves/senhas

### Pendências (próxima sessão)
1. ~~Chip~~ → resolvido com eSIM Claro (ver Dia 2)
2. **Hello world** — aguardando conexão da Evolution (ver Dia 2)
3. ~~Chave SSH do Matheus~~ → feito (Dia 2)
4. **Snapshot do VPS** no painel da Hostinger (confirmar se foi feito)
5. Aquecer o número por 2–3 dias antes de ligar automações → **virou obrigatório** após a restrição (ver Dia 2)

---

## Dia 2 — 18/07/2026 (linha da empresa + Google + segurança concluída)

### O que foi feito
- ✅ **Chave SSH do Matheus instalada e login por senha desativado** — segurança do servidor 100% concluída (só entra quem tem chave)
- ✅ **eSIM Claro contratado e ativo**: plano Pré 21GB/R$ 30 por 30 dias, eSIM R$ 5,99 (total R$ 35,99) — titular: Leonardo. Custo recorrente da linha: R$ 30/mês
- ✅ **WhatsApp Business registrado** no número novo, perfil ModernEasy
- ✅ **Google Cloud + OAuth configurado**: projeto criado, Sheets/Drive API ativas, credencial funcionando no n8n (erro 403 resolvido adicionando o Gmail como usuário de teste)
- ⚠️ **Restrição de 6h no WhatsApp Business** logo após a primeira mensagem (padrão de "número novo com comportamento atípico" do algoritmo da Meta). Expira sozinha; sem dano permanente — mas o número está sob observação

### Pendências e plano (em ordem)
1. **Aquecer o número por 2–3 dias — agora obrigatório, não opcional**: mensagens manuais entre os sócios, áudio, figurinha, um grupo; nada de API, nada de "conectar aparelho" enquanto isso
2. Só depois do aquecimento: **conectar a instância `demo` da Evolution** (manager → Get QR Code → WhatsApp Business → Dispositivos conectados)
3. Em seguida: **hello world** pelo n8n (workflow já montado, é 1 clique)
4. Confirmar **snapshot do VPS**
5. Enquanto aquece (não depende do WhatsApp): planilha da Clínica Sorriso (abas `Agenda` e `Espera` com dados fictícios) + fluxo D-1 da A1 em modo ensaio (Schedule Trigger → ler Sheets → filtrar amanhã → nó de rascunho no lugar do envio)

### Lição do dia
- Primeira mensagem de número novo tem que ser banal ("Oi, tudo bem?") — nada de links, textões ou formatação estranha. A restrição de 6h foi o aviso barato; a versão cara é o banimento. Com o número da empresa: devagar e sempre.

### Lições da noite (para não repetir)
- Comando com `apt`, `ufw`, `docker` = **dentro do servidor** (prompt `root@srv...`); `ssh`, `ssh-keygen`, `type` = no Windows (prompt `PS C:\...`)
- Todo `docker compose ...` roda dentro de `~/automation_startup/infra`
- DuckDNS preenche o IP do visitante por padrão — sempre conferir se está o IP do VPS
- URL no navegador não leva texto de instrução junto; chave de API entra no formulário, não no endereço
- Print com chave/senha não sai da conversa com o Claude

### Depois do hello world
Começa a construção da **A1 (confirmação de consultas)** — plano em `AUTOMACOES.md`.
