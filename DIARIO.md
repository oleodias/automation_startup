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

### Convenção de branches (definida em 21/07)
Repositório consolidado: **a `main` é a raiz única** com o projeto inteiro. Arquivos de teste antigos (`teste.txt`, `valeo.html`, do PR `matheus-teste`) foram removidos. Fluxo acordado: o Claude commita na branch de trabalho `claude/automation-startup-consulting-68gigr` e, ao fim de cada sessão, sincroniza tudo na `main`. **Para os sócios: olhem sempre a `main`** — ela reflete o estado completo e atual.

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

---

## ✅ MARCO — A1 (Clínica Sorriso) funcionando de ponta a ponta

**Restrição do WhatsApp expirou** e o número voltou a enviar/receber normalmente. A automação **A1 está completa e testada**:

- **A1.1 Disparo D-1** — lê a Agenda, filtra consultas de amanhã com status `agendada`, monta a mensagem, **envia pela Evolution**, marca `aguardando resposta` e grava no Log ✅
- **A1.2 Recepção de respostas** — webhook da Evolution → extrai telefone/texto → acha a consulta pendente → Switch roteia `1`/`2`/`3` → atualiza status na planilha → **responde no WhatsApp**. Três trilhas testadas: CONFIRMA, REMARCA, CANCELA ✅
- **A1.4 Resumo diário** e **A1.5 Healthcheck** — importados e montados no mesmo projeto ("Automação - Clínica Sorriso", publicado no n8n)

### Aprendizados técnicos desta etapa (valem para toda automação futura)
1. **Nó HTTP Request da Evolution:** no Body, **Name = nome fixo do campo** (`number`, `text`) e **Value = a expressão**. Inverter isso gera `Bad request - instance requires property number/text` — errei/erramos isso 2×, virou checklist.
2. **Nono dígito do WhatsApp:** o número que chega no webhook pode vir **sem um 9** em relação ao cadastrado. Solução aplicada no filtro: comparar só os **últimos 8 dígitos** (`String(x).replace(/\D/g,'').slice(-8)`). **Usar esse padrão em toda automação que casa telefone.**
3. **Webhook da Evolution:** o toggle **"Webhook by Events" precisa ficar DESLIGADO** — ligado, ele muda a URL (acrescenta `/messages-upsert`) e o n8n nunca recebe.
4. **Test URL vs Production URL:** `webhook-test/...` só funciona com "Listen for test event" ativo; `webhook/...` exige workflow **ativo**. Cadastrar na Evolution a URL correspondente ao modo em uso.
5. **"Execute step" roda só um nó** — não encadeia. Para testar o fluxo inteiro: "Listen for test event" (e mandar a mensagem real) ou ativar o workflow.
6. **Google Sheets Update Row:** sempre `Column to match on = row_number`, e em "Values to Update" deixar **somente** a coluna que muda — as demais em branco apagariam os dados.
7. Depois de um HTTP Request, o `row_number` sai do fluxo: referenciar o nó anterior pelo nome (`$('Consulta pendente').item.json.row_number`).

### Backlog da A1 (melhorias possíveis — NÃO fazer agora)
Priorizado por valor de venda, para quando fizer sentido (ex.: pedido de um cliente real):
1. **A1.3 Lista de espera** (~6h) — horário liberado por cancelamento é oferecido ao próximo da fila. *É a melhoria com maior impacto em R$: transforma cancelamento em consulta preenchida.*
2. **Reagendamento automático de verdade** (~8h) — hoje o `2` marca `remarcada` e avisa a equipe; a v2 ofereceria 3 horários livres e remarcaria sozinha.
3. **Delay aleatório entre envios** (~1h) — nó Wait 20–60s no A1.1; higiene anti-banimento quando o volume crescer.
4. **Ativar A1.5 Healthcheck** (~1h) — configurar credencial Gmail e deixar ATIVO; é o que sustenta o SLA prometido em contrato.
5. **Registro de "falta"** (~3h) — marcar quem confirmou e não apareceu, alimentando a métrica de no-show (o número que vende a automação).

---

## ✅ MARCO — A2 (Móveis Premium) + Roteador funcionando

**Duas automações completas rodando no mesmo número de WhatsApp.** Tudo testado de ponta a ponta:

- **A2.1 Varredura de orçamentos** — cron 9h, classifica quem merece follow-up (2+ dias → follow1; 7+ dias com follow1 → follow2), envia pelo WhatsApp, marca `ultimo_toque` e grava no Log. Máximo 2 toques por orçamento, limite de 10 envios por execução ✅
- **A2.2 Cliente respondeu** — webhook → acha o orçamento aberto → status `respondeu` → **alerta o vendedor da linha** com contexto (cliente, item, valor, o que ele disse) ✅
- **A2.3 Relatório semanal** — sexta 17h, resumo com **R$ recuperado**, R$ em jogo e taxa de resposta ao follow-up ✅
- **A0 Roteador v2** — resolve o limite de 1 webhook por instância da Evolution, distribuindo mensagens entre clínica e orçamentos ✅

### Organização dos workflows no n8n (convenção adotada)
| Workflow | Conteúdo | Regra |
|---|---|---|
| `Automação - Clínica Sorriso` | A1.1, A1.2, A1.4, A1.5 | agrupado por cliente |
| `Automação - Móveis Premium` | A2.1, A2.2, A2.3 | agrupado por cliente |
| `A0 - Roteador (infra)` | só o roteador v2 | **infra compartilhada fica isolada** |

Motivo de isolar o roteador: publicar/despublicar é **por workflow**. Se o roteador morasse junto com os crons de um cliente, desligar aquele cliente derrubaria o atendimento do outro.

### Aprendizados desta etapa
1. **`Import from File` NÃO cria workflow novo** — ele cola os nós no workflow aberto. Sempre criar um **Blank workflow** antes de importar. Um arquivo = um workflow.
2. **Produção exige workflow publicado.** Webhook `/webhook/...` só responde com o fluxo publicado; `/webhook-test/...` só com "Listen for test event" ativo. O erro `webhook não registrado` é quase sempre isso.
3. **Nós "Update Row" precisam de ajuste manual após importar** (o n8n cai em "Map Automatically"): modo manual, match por `row_number`, e só a coluna que muda preenchida. Sintoma do erro: `Unable to parse range: Aba!Fundefined`.
4. **Roteamento por presença em planilha é frágil.** O mesmo telefone pode ter consulta pendente E orçamento aberto. A v2 desempata pelo **conteúdo da mensagem** (`1`/`2`/`3` = resposta de menu → clínica) e devolve um campo `motivo` para depuração.
5. **Nota de arquitetura:** em produção, cada cliente terá sua **própria instância** na Evolution (número próprio), e esse conflito desaparece. O roteador é essencial em dois casos: o ambiente de demonstração (um número simulando vários clientes) e um cliente real com **várias automações no mesmo número**.
6. Copiar/colar nós entre workflows (`Ctrl+C`/`Ctrl+V`) **preserva parâmetros e credenciais** — melhor que reimportar quando já se configurou algo.

---

## 🎉 MARCO — PORTFÓLIO DE 3 AUTOMAÇÕES COMPLETO

| Bloco | Automação | Estado |
|---|---|---|
| 1 | **A1** — Confirmação e reagendamento de consultas (clínicas) | ✅ funcionando |
| 2 | **A2** — Resgate de orçamentos parados (comércio/serviços) | ✅ funcionando |
| 3 | **A3** — Relatório gerencial do dono, com gráfico | ✅ funcionando |
| — | A0 — Roteador central de mensagens (infra) | ✅ funcionando |
| — | Demo interativa da landing page | ⏳ ~8h |

### A3 — decisões de produto (mudanças em relação ao plano original)
1. **Não consolida clínica + loja.** O plano previa um relatório único somando as duas; descartado porque **são clientes diferentes** — o dono da clínica não quer ver orçamentos de móveis. O relatório da loja já é o **A2.3**.
2. **O público é o DONO, não a secretária.** É o que justifica A3 e A1.4 coexistirem:

| | A1.4 | A3 |
|---|---|---|
| Para quem | secretária | **dono** |
| Quando | 7h | 19h |
| Natureza | operacional ("quem ligar") | **gerencial** ("quanto vale") |
| Formato | texto | **gráfico + R$** |

Na venda: *"sua recepção recebe a lista de trabalho; você recebe o resultado em reais."*

3. **Envio de imagem** via `POST /message/sendMedia/{instance}` da Evolution (body plano: `number`, `mediatype`, `mimetype`, `fileName`, `media`, `caption`). Gráfico gerado pelo **QuickChart** (grátis, via URL) nas cores da marca.

### ⚠️ Aprendizado importante: número inventado destrói a demo
A primeira versão usava `FALTAS_BASELINE = 5` (fixo) para calcular "faltas evitadas". No primeiro teste real, o relatório afirmou *"~5 faltas evitadas — R$ 750 preservados"* numa semana com **2 consultas e 0 faltas**. Qualquer dono faz essa conta de cabeça e passa a duvidar do resto do relatório.

**Correção:** o cálculo passou a ser **proporcional ao movimento real** (`TAXA_FALTA_ANTES = 0.20`, ou seja, 20% das consultas viravam falta antes da automação). Testado: semana de 2 consultas → linha **não aparece**; 22 consultas com 2 faltas → "~2 evitadas, R$ 300 preservados"; 20 consultas com 6 faltas → não aparece (a automação não teria o que comemorar).

**Regra para todas as automações:** nenhum número mostrado ao cliente pode ser fixo/estimado disfarçado de medição. Se não há dado para sustentar, não exibe.

### Próximo passo definido
**Dados de demonstração ricos + gravação dos 3 vídeos** (60–90s cada) para a landing page. As automações funcionam, mas as planilhas estão com pouco movimento — um relatório com "nenhuma consulta agendada" não vende. Antes de gravar: encher a Agenda e os Orçamentos com um cenário realista (~20 consultas na semana, mix de status, alguns orçamentos recuperados). Depois: demo interativa (~8h) e a landing page recebendo os vídeos.
