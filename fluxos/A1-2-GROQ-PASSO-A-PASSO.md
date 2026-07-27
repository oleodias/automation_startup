# A1.2 — Trocar a IA de Anthropic para Groq (grátis)

> Contexto: o nó que interpreta respostas em texto livre ("não vou poder, meu filho
> adoeceu") usava a API da Anthropic, que é paga. Trocamos pela **Groq**, que tem plano
> gratuito de verdade (sem cartão de crédito). O fluxo `A1-2-recepcao-respostas.json`
> neste repositório **já está atualizado** — quem importar a versão nova só precisa colar
> a chave.

## Por que Groq

- Free tier sem cartão: `llama-3.1-8b-instant` tem **14.400 requisições/dia** e
  30.000 tokens/minuto. A Clínica Sorriso não chega perto disso (o nó só roda quando o
  paciente responde algo que não é `1`, `2` ou `3`).
- É **muito rápida** (LPU) — a resposta chega em fração de segundo, o que importa porque
  o paciente está esperando no WhatsApp.
- A API é **compatível com a da OpenAI**. Se um dia quisermos trocar de provedor
  (OpenAI, OpenRouter, Together...), muda só a URL, a chave e o nome do modelo. O resto
  do fluxo continua igual.

## Passo a passo

### 1. Criar a chave na Groq

1. Entrar em **https://console.groq.com** e criar conta (dá para usar login do Google).
   Não pede cartão.
2. Menu lateral → **API Keys** (`https://console.groq.com/keys`) → **Create API Key**.
3. Dar um nome (ex.: `n8n-clinica-sorriso`) e **copiar a chave** — ela começa com `gsk_`
   e **só aparece uma vez**. Guardar no gerenciador de senhas dos sócios.

> ⚠️ A chave não entra no repositório, nem em print, nem em conversa. No JSON do fluxo
> ela é o placeholder `SUA_CHAVE_GROQ_AQUI`.

### 2. Puxar a versão nova do fluxo

No n8n: **Workflows → Import from File** e escolher
`fluxos/A1-2-recepcao-respostas.json` (versão atual da `main`).

Se você já tem o fluxo montado no n8n e não quer reimportar, dá para editar o nó à mão —
ver a seção "Editando à mão" no fim.

### 3. Colar a chave no nó

1. Abrir o nó **`Groq — classificar intenção`** (é o HTTP Request no ramo de fallback).
2. Em **Headers**, no header `Authorization`, o valor está como
   `Bearer SUA_CHAVE_GROQ_AQUI`.
3. Trocar **só** o `SUA_CHAVE_GROQ_AQUI` pela chave real. **Manter a palavra `Bearer` e
   o espaço depois dela.** Fica assim:

   ```
   Authorization: Bearer gsk_xxxxxxxxxxxxxxxxxxxxxxxx
   ```

   Esse é o erro nº 1 de quem configura Groq pela primeira vez — colar a chave sem o
   `Bearer ` na frente devolve `401 Invalid API Key`.

### 4. Conferir os outros nós (o de sempre)

- Os 4 nós **Google Sheets**: selecionar credencial + documento + aba `Agenda`.
- Os nós **📤 EVOLUTION** continuam placeholders até o número do WhatsApp voltar.

### 5. Testar

No nó **Webhook Evolution** → **Listen for test event**, e mandar este POST (trocar o
telefone por um que esteja na aba `Agenda` com status `aguardando resposta`):

```bash
curl -X POST https://moderneasyn8n.duckdns.org/webhook-test/evolution-demo \
  -H 'content-type: application/json' \
  -d '{"data":{"key":{"remoteJid":"5551999990000@s.whatsapp.net"},
       "message":{"conversation":"não vou poder ir, meu filho adoeceu"}}}'
```

O texto livre cai no fallback → vai para a Groq → deve voltar `remarcar` → a planilha
muda o status para `remarcada`.

Testar também: `"pode confirmar sim"` → `confirmar`; `"não quero mais"` → `cancelar`;
`"o consultório fica onde?"` → `duvida` (vai para o aviso à secretária).

### 6. (Opcional) Testar a Groq isolada, fora do n8n

Se o nó der erro e você quiser saber se é a chave ou o fluxo:

```bash
curl https://api.groq.com/openai/v1/chat/completions \
  -H "Authorization: Bearer $GROQ_API_KEY" \
  -H "content-type: application/json" \
  -d '{"model":"llama-3.1-8b-instant","max_tokens":20,"temperature":0,
       "messages":[
         {"role":"system","content":"Responda APENAS uma palavra: confirmar, remarcar, cancelar, duvida, humano."},
         {"role":"user","content":"não vou poder ir, meu filho adoeceu"}]}'
```

## O que mudou no JSON (para quem quiser editar à mão)

No nó de IA:

| Campo | Antes (Anthropic) | Agora (Groq) |
|---|---|---|
| URL | `https://api.anthropic.com/v1/messages` | `https://api.groq.com/openai/v1/chat/completions` |
| Header de auth | `x-api-key: SUA_CHAVE...` | `Authorization: Bearer SUA_CHAVE_GROQ_AQUI` |
| Header extra | `anthropic-version: 2023-06-01` | *(removido — não existe na Groq)* |
| Modelo | `claude-haiku-4-5-20251001` | `llama-3.1-8b-instant` |
| Prompt de sistema | campo `system` no topo do body | primeira mensagem com `"role": "system"` |
| Nome do nó | `Claude — classificar intenção` | `Groq — classificar intenção` |

Body novo:

```json
{
  "model": "llama-3.1-8b-instant",
  "max_tokens": 20,
  "temperature": 0,
  "messages": [
    { "role": "system", "content": "Classifique a mensagem de um paciente respondendo à confirmação de uma consulta. Responda APENAS uma destas palavras, sem pontuação e sem explicação: confirmar, remarcar, cancelar, duvida, humano." },
    { "role": "user", "content": "<texto do paciente>" }
  ]
}
```

E no nó seguinte, **`Extrair intenção`**, o caminho da resposta mudou — a Anthropic
devolve `content[0].text`, a Groq devolve `choices[0].message.content`:

```js
{{ (($json.choices?.[0]?.message?.content || '').toLowerCase().match(/confirmar|remarcar|cancelar|duvida|humano/) || ['humano'])[0] }}
```

O `.match(...)` é uma proteção a mais: se o modelo responder `"Remarcar."` ou
`"A intenção é remarcar"`, ainda assim extraímos a palavra certa. E se não vier nenhuma
das cinco palavras (ou a API falhar), cai em `humano` — ou seja, **a secretária é
avisada**, que é o comportamento seguro. Nunca marca nada errado na agenda por causa de
uma resposta estranha da IA.

## Cuidados

- **Não commitar a chave.** Ela vive só no n8n. Se vazar, revogar em
  console.groq.com/keys e gerar outra.
- **Modelo pode ser descontinuado.** Provedores de LLM aposentam modelos. Se um dia o nó
  começar a dar `400 model_decommissioned`, é só trocar o nome do modelo no body por
  outro da lista em console.groq.com/docs/models.
- **Free tier tem limite por minuto** (30 req/min). Se um dia estourar (não vai, nesta
  escala), a Groq devolve `429` — o nó pode ganhar "Retry on Fail" no n8n.
- **Qualidade:** o `llama-3.1-8b-instant` é um modelo pequeno. Para esta tarefa
  (classificar em 5 palavras) ele é suficiente. Se nos testes ele errar demais, subir
  para um modelo maior da Groq (ex.: `llama-3.3-70b-versatile`) — só trocar o nome no
  body, que também está no free tier, com limite diário menor.
