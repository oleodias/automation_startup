# Plano de Construção das Automações do Portfólio

*Especificação para começar a construir amanhã. Complementa `CONSULTORIA-90-DIAS.md` (itens 2, 3 e 7). Estimativas de horas incluem aprendizado e retrabalho — vocês estão aprendendo o stack enquanto constroem, e isso é proposital (item 8 da consultoria).*

## Ordem de construção e por quê

**A1 (confirmação de consultas) → A2 (resgate de orçamentos) → A3 (relatório do dono) → demo interativa da landing page.**

A1 vem primeiro porque é a vitrine do nicho recomendado e porque constrói toda a base que as outras reaproveitam (~70%): conexão WhatsApp, padrão de webhook, integração com planilha, healthcheck. A2 e A3 depois são rápidas. A demo interativa só faz sentido quando A1 estiver estável.

**Total estimado: ~75h de construção** (A1 ~35h + A2 ~18h + A3 ~10h + demo interativa ~8h + fundação ~4h), distribuídas nas quinzenas 1–4 do plano de 90 dias.

---

## Dia 1 (amanhã) — checklist de fundação (~3–4h, de preferência os dois juntos)

1. **Contratar VPS** Hostinger KVM 2, Ubuntu 24.04 LTS (~R$ 80–110/mês no plano mensal sem fidelidade; cai para ~R$ 55 se toparem 12–24 meses adiantados — decisão de vocês, eu começaria mensal e migraria para o plano longo quando a página estiver no ar).
2. **Hardening básico** (líder: quem tem a base de infra/redes): acesso só por chave SSH (desabilitar senha), `ufw` liberando apenas 22/80/443, `fail2ban`, atualizações automáticas de segurança.
3. **Instalar Docker + Docker Compose.**
4. **Subir a stack via `docker-compose.yml`** (versionar o arquivo neste repositório, sem credenciais):
   - `n8n` (com fila simples, sem exagerar na arquitetura)
   - `postgres` (banco do n8n e, depois, dados das automações)
   - `evolution-api` (gateway WhatsApp não-oficial)
   - `caddy` (ou Traefik) para HTTPS automático — usar subdomínio gratuito (ex.: DuckDNS) até o domínio próprio existir
5. **Comprar chip pré-pago dedicado (~R$ 20)** e conectar na Evolution API via QR Code. **Nunca o número pessoal:** API não-oficial tem risco real de banimento pela Meta; um chip descartável transforma o pior cenário em "R$ 20 e meia hora perdidos".
6. **Fluxo "hello world":** webhook no n8n → envia "estamos no ar" pelo WhatsApp. Quando essa mensagem chegar, a fundação está pronta e o dia 1 acabou.

> Regra desde o dia 1: credenciais no Bitwarden (grátis), nunca hardcoded em fluxo nem commitadas; export semanal dos fluxos (`.json`) para este repositório como backup e versionamento.

---

## A1 — Confirmação e reagendamento de consultas (~35h, quinzena 2)

**Cenário demonstrável:** "Clínica Sorriso", clínica fictícia com agenda em Google Sheets.

**Por que Google Sheets e não Google Calendar/sistema de clínica na v1:** é o que clínica pequena entende e consegue operar sem treinamento; a migração para Calendar ou integração com sistema real é incremental e vira argumento de venda ("integramos com o que você já usa"), não pré-requisito da demo.

**Modelo de dados (aba `Agenda`):** paciente | telefone | data | hora | profissional | status (`agendada` / `aguardando resposta` / `confirmada` / `remarcada` / `cancelada` / `falta`) | observação. Aba `Espera`: paciente | telefone | preferência de horário.

**Os 5 fluxos n8n:**

1. **Disparo D-1** (cron, 10h): busca consultas de amanhã com status `agendada` → envia mensagem de confirmação ("responda **1** para confirmar, **2** para remarcar, **3** para cancelar") → status `aguardando resposta`.
2. **Recepção de respostas** (webhook da Evolution API — o coração da automação):
   - `1` → status `confirmada` + mensagem de confirmação com data/hora.
   - `2` → busca 3 horários livres na agenda → oferece em lista → paciente escolhe → remarca e libera o horário antigo.
   - `3` → status `cancelada` + horário liberado.
   - **Texto livre** ("não vou poder, meu filho adoeceu") → Claude API classifica a intenção (`confirmar` / `remarcar` / `cancelar` / `dúvida` / `humano`) → segue o ramo certo ou escala para a "secretária" com o contexto. Prompt curto, resposta em JSON, temperatura baixa.
3. **Lista de espera:** horário liberado (por 2 ou 3) → oferece ao primeiro da aba `Espera` → se aceitar em 2h, agenda; senão, passa ao próximo.
4. **Resumo diário** (cron, 7h): para o número da "secretária": consultas do dia, confirmadas, pendentes, remarcadas, quem não respondeu (para ligação manual — a automação não precisa resolver 100%, precisa reduzir o manual a exceção).
5. **Healthcheck** (cron, 30 min): testa Evolution API conectada + n8n vivo + última execução dos crons; falhou → alerta no WhatsApp de vocês dois. *Este fluxo vira produto depois: é o que sustenta o SLA de 12h/24h prometido em contrato (item 4 da consultoria).*

**Divisão de trabalho sugerida:** fluxos 1, 4 e 5 + infra → quem vem de código/infra; fluxos 2 e 3 (conversa + IA) → Leonardo, que já arquitetou fluxos n8n com IA. Cada um revisa o fluxo do outro no fim da quinzena — é o antídoto do bus factor apontado no item 8.

**Critério de pronto:** 7 dias seguidos rodando com "pacientes" fictícios (vocês, amigos, familiares respondendo de verdade) sem nenhuma intervenção manual. Só então gravar o vídeo.

---

## A2 — Resgate de orçamentos parados (~18h, quinzena 3)

**Cenário demonstrável:** loja fictícia de móveis planejados (a mesma automação serve para clínica — orçamento de tratamento — e é isso que o bloco da landing page vai dizer).

**Modelo de dados (aba `Orçamentos`):** cliente | telefone | descrição | valor | data de envio | status (`enviado` / `respondeu` / `recuperado` / `perdido`) | vendedor.

**Fluxos (3):**
1. **Varredura diária** (cron, 9h): orçamentos `enviado` há mais de 48h → follow-up 1, pessoal e curto ("Oi [nome], conseguiu ver o orçamento do [item]? Ficou alguma dúvida que eu possa resolver?"); há mais de 7 dias → follow-up 2 com gatilho honesto (condição válida até sexta, disponibilidade de agenda). Máximo 2 toques automáticos — mais que isso é spam e queima a marca do cliente.
2. **Resposta do cliente** (webhook): status `respondeu` + alerta imediato ao "vendedor" com o contexto do orçamento — a automação reabre a conversa, o humano fecha a venda.
3. **Relatório semanal** (cron, sexta 17h): quantos orçamentos varridos, quantos responderam, R$ recuperado — o número que vende a automação.

**Reaproveita de A1:** conexão Evolution, padrão webhook, padrão de planilha, healthcheck (só adicionar os novos crons ao fluxo 5).

---

## A3 — Relatório diário do dono (~10h, quinzena 3)

**Fluxo único** (cron, 19h): lê as planilhas de A1 e A2 → monta o resumo do dia (consultas de amanhã, % de confirmação, faltas da semana, orçamentos recuperados no mês) → gera 1 gráfico simples (QuickChart, grátis, via URL) → envia imagem + texto ao "dono".

É a automação mais barata e o vídeo mais bonito da landing page — o dono leigo entende em 10 segundos. **É também a primeira a ser cortada se A1 atrasar** (checkpoint do dia 45).

---

## Demo interativa da landing page (~8h, quinzena 4)

O diferencial da página: o visitante não assiste — **sente**.

**Fluxo:** botão "Teste agora no seu WhatsApp" → link `wa.me` do chip de demo com texto pré-preenchido ("QUERO TESTAR") → n8n reconhece o gatilho → roteiro guiado de ~2 min no papel de paciente: recebe a confirmação da "Clínica Sorriso", responde `2`, vê a remarcação acontecer, recebe o mini-resumo "do dono" → mensagem final: *"Isso foi uma demonstração. Quer isso rodando na sua empresa? É só responder aqui."* → nome + telefone salvos na planilha `Leads` com data e origem.

**Cuidados:** limitar a 1 demo por número/dia (anti-abuso barato via planilha), e deixar explícito na primeira mensagem que é uma demo automática — transparência primeiro.

---

## Regras de ouro durante a construção

- **Português nos fluxos e mensagens**, nomes descritivos, uma linha de descrição em cada fluxo — daqui a 6 meses vocês não vão lembrar por que aquele nó existe.
- **Credenciais só no Bitwarden** e nas credenciais nativas do n8n; nada em texto plano, nada no git.
- **Export semanal dos fluxos para este repositório** (`/fluxos/*.json`) — backup, versionamento e, de quebra, portfólio técnico.
- **LGPD desde o dia 1, mesmo com dados fictícios:** logs de execução com retenção curta (7–14 dias), fluxos tocando só nome/telefone/horário — o hábito que vocês criarem agora é o que vai valer quando o dado for real e sensível.
- **Estabilidade > feature.** Toda vez que surgir "seria legal se...", anotem num `IDEIAS.md` e voltem ao escopo. O checkpoint do dia 45 mede dias sem intervenção, não quantidade de nós.
