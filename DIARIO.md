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
1. **Chip pré-pago físico** (~R$ 15–30) → segundo slot de um celular → WhatsApp Business com perfil ModernEasy → escanear QR no manager (`/manager`, instância `demo`)
2. **Hello world** (Passo 7 do `GUIA-SETUP.md`): mensagem "Estamos no ar! 🚀" via n8n
3. **Chave SSH do Matheus** no servidor (Passo 2 do guia, parte do segundo sócio) — e só depois desativar login por senha
4. **Snapshot do VPS** no painel da Hostinger (se não foi feito no fechamento)
5. Aquecer o número por 2–3 dias (uso manual normal) antes de ligar automações

### Lições da noite (para não repetir)
- Comando com `apt`, `ufw`, `docker` = **dentro do servidor** (prompt `root@srv...`); `ssh`, `ssh-keygen`, `type` = no Windows (prompt `PS C:\...`)
- Todo `docker compose ...` roda dentro de `~/automation_startup/infra`
- DuckDNS preenche o IP do visitante por padrão — sempre conferir se está o IP do VPS
- URL no navegador não leva texto de instrução junto; chave de API entra no formulário, não no endereço
- Print com chave/senha não sai da conversa com o Claude

### Depois do hello world
Começa a construção da **A1 (confirmação de consultas)** — plano em `AUTOMACOES.md`.
