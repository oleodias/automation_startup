# A1 — Guia de Construção do Molde (nó por nó)

*Objetivo desta sessão: construir a A1 (confirmação de consultas) completa no n8n, com os pontos de contato com o WhatsApp como **nós-placeholder** — quando o número sair da restrição e a Evolution conectar, trocamos ~4 nós e a automação nasce andando. Tudo aqui roda e é testável HOJE, sem WhatsApp.*

**Convenção do placeholder:** todo envio de WhatsApp vira um nó **Set (Edit Fields)** de cor vermelha, nomeado `📤 EVOLUTION (pendente) — <o que enviaria>`, com dois campos: `para` (o telefone) e `mensagem` (o texto final). Assim o teste mostra exatamente o que seria enviado, e a troca futura é mecânica: substituir cada um por um HTTP Request `POST https://moderneasyevo.duckdns.org/message/sendText/demo` (header `apikey`, body `{number, text}`).

---

## Pré-requisito: a planilha "Clinica Sorriso — Agenda"

No Drive da ModernEasy, com 3 abas:

**`Agenda`** — colunas exatas (linha 1): `paciente | telefone | data | hora | profissional | status | observacao`
- `data` como **texto** no formato `DD/MM/AAAA` (formatem a coluna como "texto simples" para o Sheets não converter) — evita o inferno de fusos e formatos.
- `telefone` no formato `5551XXXXXXXXX` (só dígitos).
- `status`: usar exatamente `agendada`, `aguardando resposta`, `confirmada`, `remarcada`, `cancelada`, `falta`.
- 12 linhas fictícias: metade com data de amanhã, metade depois; telefones = os de vocês dois.

**`Espera`** — `paciente | telefone | preferencia`. 3 linhas.

**`Log`** — `quando | fluxo | paciente | acao`. Vazia (o n8n escreve). Serve de rastro e alimenta o resumo diário.

---

## Fluxo 1 — `A1.1 Disparo D-1` (construir e testar hoje, inteiro)

| # | Nó | Configuração |
|---|---|---|
| 1 | **Schedule Trigger** | Cron: `0 10 * * *` (10h, todo dia). Para testar hoje, executem manualmente com o botão "Execute workflow" |
| 2 | **Google Sheets — Get Row(s)** | Documento: Clinica Sorriso; aba `Agenda`; retorna todas as linhas (o nó devolve `row_number`, guardem — é ele que permite atualizar a linha certa depois) |
| 3 | **Filter** | Duas condições (AND): `status` equals `agendada` **e** `data` equals expressão `{{ $now.plus({ days: 1 }).setZone('America/Sao_Paulo').toFormat('dd/MM/yyyy') }}` |
| 4 | **Set** — `Montar mensagem` | Campo `mensagem`: `Olá, {{ $json.paciente }}! Aqui é da Clínica Sorriso 😊 Sua consulta com {{ $json.profissional }} está marcada para amanhã ({{ $json.data }}) às {{ $json.hora }}. Responda: 1️⃣ Confirmo · 2️⃣ Preciso remarcar · 3️⃣ Preciso cancelar`. Campo `para`: `{{ $json.telefone }}` |
| 5 | **📤 EVOLUTION (pendente) — confirmação D-1** | Placeholder (ver convenção) |
| 6 | **Google Sheets — Update Row** | Aba `Agenda`, localizar por `row_number` (vem do nó 2), setar `status` = `aguardando resposta` |
| 7 | **Google Sheets — Append Row** | Aba `Log`: `quando` = `{{ $now.toISO() }}`, `fluxo` = `disparo-d1`, `paciente`, `acao` = `mensagem de confirmação enviada` |

**Teste de aceite (hoje):** rodar manualmente → só os pacientes de amanhã com status `agendada` passam pelo filtro → o placeholder mostra as mensagens certinhas → a planilha muda para `aguardando resposta` → o Log ganha linhas. Rodar de novo: **zero itens** (ninguém mais está `agendada` para amanhã) — isso prova que não haverá disparo duplicado.

## Fluxo 2 — `A1.2 Recepção de respostas` (construir a estrutura hoje)

| # | Nó | Configuração |
|---|---|---|
| 1 | **Webhook** | POST, path `evolution-demo`. **Já anotem a URL de produção** (`https://moderneasyn8n.duckdns.org/webhook/evolution-demo`) — é ela que vamos cadastrar na Evolution depois |
| 2 | **Set** — `Extrair dados` | Normaliza o payload da Evolution: `telefone` = `{{ $json.body.data.key.remoteJid.split('@')[0] }}`, `texto` = `{{ $json.body.data.message.conversation }}` *(a estrutura exata do payload a gente confirma no primeiro teste real; por isso este nó existe — o resto do fluxo não depende do formato cru)* |
| 3 | **Google Sheets — Get Row(s)** | Busca na `Agenda` a linha com `telefone` = o extraído e `status` = `aguardando resposta` |
| 4 | **Switch** — `Interpretar resposta` | Regra 1: `texto` = `1` → CONFIRMA · Regra 2: `2` → REMARCA · Regra 3: `3` → CANCELA · Fallback → IA |
| 5a | CONFIRMA: **Update Row** (`status` = `confirmada`) → **📤 EVOLUTION (pendente) — "Confirmado! Te esperamos 😊"** → **Append Log** | |
| 5b | REMARCA (v1 simples): **Update Row** (`status` = `remarcada`, `observacao` = `reagendar manualmente`) → **📤 EVOLUTION (pendente) — "Sem problema! Nossa equipe já vai te chamar para achar um novo horário 😉"** → **Append Log** | *(oferta automática de 3 horários fica para a v1.1 — molde primeiro, sofisticação depois)* |
| 5c | CANCELA: **Update Row** (`status` = `cancelada`) → **📤 EVOLUTION (pendente) — "Tudo bem, cancelado. Quando quiser remarcar é só chamar!"** → **Append Log** | |
| 5d | FALLBACK → **HTTP Request — Claude API** (ver box abaixo) → **Switch** pela intenção retornada (`confirmar`/`remarcar`/`cancelar` reaproveitam 5a-c; `duvida`/`humano` → **📤 EVOLUTION (pendente) — aviso à secretária**) | |

> **Nó de IA (5d):** `POST https://api.anthropic.com/v1/messages`, headers `x-api-key` (criar conta em console.anthropic.com — coloquem US$ 5 de crédito, uso desse fluxo custa centavos) e `anthropic-version: 2023-06-01`. Body: model `claude-haiku-4-5-20251001` (barato e rápido, perfeito para classificação), max_tokens 50, e um prompt system: *"Classifique a mensagem de um paciente respondendo a uma confirmação de consulta. Responda APENAS uma palavra: confirmar, remarcar, cancelar, duvida ou humano."* Se preferirem deixar para depois, este nó também pode ser um placeholder hoje — mas dá para testar de verdade, ele não toca o WhatsApp.

**Teste de aceite (hoje):** usar o botão "Listen for test event" do Webhook e mandar um POST manual (ou usar o próprio botão de teste com JSON colado) simulando "1", "2", "3" e um texto livre → planilha muda certo em cada caso.

## Fluxo 3 — `A1.4 Resumo diário` (construir e testar hoje, inteiro)

Schedule Trigger (`0 7 * * *`) → **Get Row(s)** da `Agenda` → **Filter** (consultas de HOJE) → **Code/Set** agregando: total do dia, confirmadas, aguardando, canceladas + lista "quem não respondeu (ligar!)" → **📤 EVOLUTION (pendente) — resumo p/ secretária**.

## Fluxo 4 — `A1.5 Healthcheck` (construir e LIGAR hoje — com alerta por e-mail)

Schedule Trigger (a cada 30 min) → **HTTP Request** `GET https://moderneasyevo.duckdns.org/instance/connectionState/demo` (header `apikey`) → **IF** estado ≠ `open` → **Gmail node** (conta ModernEasy) com assunto `⚠️ Evolution desconectada`. 

Este fluxo fica **ativo desde já** — alerta por e-mail hoje, e quando o WhatsApp voltar adicionamos o alerta por mensagem. Ele vai, inclusive, avisar vocês no dia em que a instância `demo` conectar. *(Bônus: é o embrião do produto de SLA prometido em contrato.)*

---

## Checklist de encerramento da sessão
- [ ] 4 workflows salvos com os nomes `A1.1` a `A1.5` (pulamos o 3 — lista de espera é v1.1)
- [ ] Fluxo 1 e Resumo testados com a planilha fictícia; Webhook testado com POST manual
- [ ] Healthcheck ATIVO com alerta por e-mail
- [ ] Exportar os 4 fluxos (menu ⋯ → Download) e commitar em `fluxos/` neste repositório
- [ ] O que fica para o retorno do número: trocar placeholders por HTTP Request da Evolution, cadastrar o webhook na Evolution (Settings da instância → Webhook → URL do Fluxo 2, evento `messages.upsert`), teste ponta a ponta com vocês de "pacientes"
