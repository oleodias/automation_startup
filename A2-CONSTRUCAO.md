# A2 — Resgate de orçamentos parados (guia de montagem)

*Cenário demonstrável: **Móveis Premium**, loja fictícia de móveis planejados. A mesma automação serve para clínica (orçamento de tratamento não fechado), oficina, materiais de construção — é isso que o bloco da landing page vai dizer.*

**A dor que ela resolve:** a loja manda 20 orçamentos por semana, 12 nunca respondem, e ninguém faz follow-up porque o vendedor está atendendo o próximo cliente. Dinheiro pronto indo embora por falta de um lembrete.

---

## Arquivos prontos

| Arquivo | O que é |
|---|---|
| `dados/moveis-premium-orcamentos.xlsx` | Planilha com 10 orçamentos fictícios (4 disparam mensagem) |
| `fluxos/A2-1-varredura-orcamentos.json` | Follow-up automático em 2 toques (cron 9h) |
| `fluxos/A2-2-resposta-cliente.json` | Cliente respondeu → marca planilha + **alerta o vendedor** |
| `fluxos/A2-3-relatorio-semanal.json` | Sexta 17h: resumo com **R$ recuperado** para o dono |
| `fluxos/A0-roteador-mensagens.json` | **Porteiro** que distribui as mensagens entre A1.2 e A2.2 |

---

## ⚠️ O problema de arquitetura que o A0 resolve

A Evolution API tem **um único webhook por instância**. Ela não sabe se a mensagem que chegou é resposta de paciente (A1.2) ou de orçamento (A2.2) — simplesmente entrega tudo num endereço só.

Sem resolver isso, a segunda automação **nunca receberia nada**. A solução é o **A0 Roteador**: ele fica na frente, recebe toda mensagem, consulta a planilha de orçamentos e reenvia o payload original para o fluxo certo:

```
WhatsApp → Evolution → A0 Roteador ─┬─→ A2.2 (tem orçamento aberto com este telefone)
                                    └─→ A1.2 (caso contrário: clínica, comportamento padrão)
```

**Detalhe importante:** a A1.2 **não muda em nada** — continua ouvindo no mesmo endereço (`/webhook/evolution-demo`) e funcionando como já foi testado. Só a URL cadastrada na Evolution muda. Para desfazer, aponta-se a Evolution de volta ao endereço antigo e nada se perde.

*(Esse padrão é o que vai permitir a terceira, a quarta e a décima automação conviverem no mesmo número. Vale entender bem.)*

---

## Ordem de execução (do mais seguro para o mais delicado)

### Etapa 1 — Planilha (~10 min)
1. Subir `moveis-premium-orcamentos.xlsx` no Drive da ModernEasy → abrir → **Arquivo → Salvar como Planilhas Google**.
2. Renomear para **Moveis Premium — Orcamentos**.
3. **Trocar os telefones das 4 linhas amarelas** pelos celulares reais de vocês (são as únicas que disparam mensagem) e o `vendedor_telefone` dessas linhas também.
4. Conferir que `telefone`, `data_envio` e `ultimo_toque` continuam como **texto**.

### Etapa 2 — A2.1 Varredura (~20 min) — não toca em webhook, risco zero
Importar → configurar os 3 nós Google Sheets → colar a `apikey` no nó de envio → **Execute workflow**.

> ⚠️ **Todo nó "Update Row" precisa de ajuste manual depois de importar.** O n8n não preserva o modo de mapeamento no import: ele cai em **"Map Automatically"**, e aí tenta ler os campos do nó anterior (que, depois de um HTTP Request, é a resposta da Evolution — sem `row_number`). O sintoma é o erro `Unable to parse range: Aba!Fundefined`.
>
> **Correção padrão em qualquer nó Update Row:**
> 1. Mapping Column Mode → **Map Each Column Manually**
> 2. Column to match on → `row_number`
> 3. Valor do row_number → `{{ $('NOME DO NÓ QUE TEM O DADO').item.json.row_number }}` (no A2.1 é `Quem recebe follow-up hoje`; no A2.2 basta `{{ $json.row_number }}`)
> 4. Em Values to Update, preencher **somente a coluna que muda** — as outras em branco apagariam os dados
>
> Exceção: nós **Append Row** (os de Log) funcionam certo com **Map Automatically**, porque os campos que chegam já têm os nomes das colunas.

**Resultado esperado:** 4 mensagens enviadas (2 no tom "conseguiu ver o orçamento?", 2 no tom "a condição vale até sexta"), a coluna `ultimo_toque` atualizada e 4 linhas no Log. Rodar de novo no mesmo dia: **zero envios** (prova de que não há spam duplicado).

### Etapa 3 — A2.3 Relatório (~10 min) — também sem webhook
Importar → configurar o nó Sheets → trocar `NUMERO_DO_DONO` pelo seu celular → colar a `apikey` → **Execute workflow**. Deve chegar o resumo com R$ recuperado e taxa de resposta.

### Etapa 4 — A2.2 + A0 Roteador (~30 min) — aqui mexe no webhook
1. Importar **A2.2**, configurar Sheets + `apikey`, e **ATIVAR** o workflow.
2. Garantir que a **A1.2 está ATIVA**.
3. Importar **A0**, configurar o nó Sheets (aba `Orcamentos`), e **ATIVAR**.
4. Na Evolution (Events → Webhook): trocar a URL para
   `https://moderneasyn8n.duckdns.org/webhook/evolution-in`
   — mantendo `messages.upsert` marcado e **"Webhook by Events" DESLIGADO**.
5. **Teste duplo (o mais importante da etapa):**
   - Responder de um número que está na **Agenda** com `aguardando resposta` → deve cair na **clínica** (status muda, resposta automática chega).
   - Responder de um número que está em **Orcamentos** com status `enviado` → deve cair no **orçamento** (status vira `respondeu` e o alerta chega no "vendedor").

Se um dos dois falhar, olhar a execução do **A0** primeiro: o nó "Decidir destino" mostra qual caminho ele escolheu.

---

## Notas de projeto (decisões deliberadas)

- **Máximo 2 toques automáticos por orçamento.** Mais que isso é spam, queima a marca do cliente e o número de vocês. O limite está no código, não na sorte.
- **Limite de 10 envios por execução** — higiene anti-banimento enquanto o número ainda é novo. Está no nó Code (`LIMITE_POR_EXECUCAO`), fácil de ajustar quando o número estiver maduro.
- **A A2.2 não responde ao cliente.** De propósito: a automação **reabre a conversa**, o vendedor humano **fecha a venda**. Resposta automática nesse momento soaria robótica e esfriaria a negociação.
- **O alerta vai para o vendedor da linha** (`vendedor_telefone`), não para um número fixo — assim a automação já nasce funcionando para loja com vários vendedores.
- **Comparação de telefone pelos últimos 8 dígitos**, para sobreviver à variação do nono dígito (aprendizado da A1).

## Melhorias futuras (backlog, não fazer agora)
1. **Loop com delay aleatório** entre envios (Split In Batches + Wait 20–60s) — quando o volume passar de ~20 mensagens/dia.
2. **Classificar a resposta com IA** ("quero fechar" vs "achei caro" vs "só depois") e priorizar o alerta ao vendedor.
3. **Detectar orçamento sem resposta há 30 dias** → marcar `perdido` sozinho e tirar do relatório de "em jogo".
