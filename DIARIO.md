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

---

## Dia 3 (noite de construção) — restrição de 7 dias + molde da A1 + identidade

### Situação
- ⚠️ **Restrição de 7 DIAS no WhatsApp Business** (segunda punição do número, após a de 6h). Sem conexão de aparelhos (nem WhatsApp Web, nem Evolution) até expirar. Causa exata desconhecida; padrão provável: número novo + comportamento atípico reincidente aos olhos do algoritmo.
- Decisão: usar a semana para construir o que não depende do WhatsApp — **molde completo da A1** + **identidade visual**.

### Protocolo do número a partir de agora (inegociável)
1. Durante a restrição: **não tentar conectar nada**, nem reinstalar, nem trocar de aparelho. Verificar no app se há opção "solicitar análise" e usá-la uma vez.
2. Ao expirar: 5–7 dias de vida civil — conversas manuais bidirecionais com contatos que salvaram o número, áudio, figurinha, grupo; **quem recebe primeiro sempre iniciou conversa antes**; zero links.
3. Só depois: conectar Evolution e começar com **5–10 mensagens/dia**, todas para números que já conversaram com o número, crescendo ao longo de semanas ([cronogramas de aquecimento de referência](https://www.socialhub.pro/blog/evitar-banimento-whatsapp/)).
4. Nos fluxos: delay aleatório entre envios (nó Wait 20–60s), texto com nome do destinatário e pequenas variações — nunca rajada de mensagens idênticas.
5. **Se houver 3ª punição, o número está queimado**: plano B é linha nova + migração da demo para a **API oficial da Meta (Cloud API)**, que elimina o risco de banimento por automação.

### Construído nesta sessão (guias no repositório)
- `A1-CONSTRUCAO.md` — guia nó por nó do molde da A1: fluxos Disparo D-1, Recepção (webhook + switch + IA), Resumo diário e Healthcheck; envios de WhatsApp como nós-placeholder padronizados (`📤 EVOLUTION (pendente)`), troca futura mecânica
- `IDENTIDADE.md` — guia de marca v1: posicionamento, paleta (verde-mata #0B3D2E + âmbar #F5A524), tipografia (Manrope + Inter), direção do logo, banco de frases de impacto, aplicações
- Healthcheck com alerta por e-mail fica ATIVO — inclusive avisará quando a instância `demo` conectar

### Próximos marcos
1. Importar os 4 fluxos prontos de `fluxos/*.json` no n8n (Workflows → Import from File) + configurar credenciais/planilha + testar
2. Logo no Canva (3 versões) + tagline escolhida
3. Expirada a restrição → protocolo de aquecimento → conectar Evolution → trocar placeholders → teste ponta a ponta da A1

### Incidente resolvido (20/07)
Leonardo sem acesso ao servidor de casa (ping/SSH/HTTPS bloqueados) enquanto Matheus acessava normal. Diagnóstico por eliminação: servidor ok, fail2ban zero bans, ufw limpo, DNS resolvendo → problema na rota local. **Solução: reiniciar o roteador de casa** (IP dinâmico/CGNAT renovado). Lição: quando "só um de nós" não acessa, o suspeito é a ponta local — testar via 4G/5G isola o problema em 1 minuto.

### Lições da noite (para não repetir)
- Comando com `apt`, `ufw`, `docker` = **dentro do servidor** (prompt `root@srv...`); `ssh`, `ssh-keygen`, `type` = no Windows (prompt `PS C:\...`)
- Todo `docker compose ...` roda dentro de `~/automation_startup/infra`
- DuckDNS preenche o IP do visitante por padrão — sempre conferir se está o IP do VPS
- URL no navegador não leva texto de instrução junto; chave de API entra no formulário, não no endereço
- Print com chave/senha não sai da conversa com o Claude

### Depois do hello world
Começa a construção da **A1 (confirmação de consultas)** — plano em `AUTOMACOES.md`.
