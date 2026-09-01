# Auditoria de Segurança — ModernEasy

> **Feita em:** 01/09/2026 · branch `claude/landing-page-startup-umwqcx` (commit `8e86a24`), com a `main` em `f348f62`.
> **Escopo:** a landing page, o repositório inteiro (89 commits de histórico), o workflow de publicação e os arquivos de infraestrutura.
> **Fora do escopo:** teste de invasão no servidor e revisão dos fluxos que rodam **hoje** no n8n em produção — os `.json` do repositório podem estar defasados em relação a eles.

## Resumo

**O site em si está limpo.** Ele não coleta nada, não tem formulário, não usa nenhuma biblioteca de terceiros e não há caminho de XSS. O risco real está no que o repositório **público** conta sobre a automação que roda por trás.

| | Quantidade |
|---|---|
| 🔴 Risco alto | 2 |
| 🟠 Risco médio | 3 |
| ⚪ Risco baixo | 4 |
| ✅ Verificado e correto | 7 |

---

## 🔴 Risco alto

### S-01 · O webhook do n8n aceita qualquer um, e o endereço dele está publicado

O nó que recebe as mensagens do WhatsApp **não tem autenticação configurada** — no n8n isso significa autenticação `None`. Conferi todos os nós seguintes do fluxo: não há validação de origem, checagem de chave nem filtro por instância.

```
fluxos/A1-2-recepcao-respostas.json — nó "Webhook Evolution"
  httpMethod: POST · path: evolution-demo · authentication: AUSENTE

A1-CONSTRUCAO.md:43 — URL de produção publicada
  https://moderneasyn8n.duckdns.org/webhook/evolution-demo

github.com/oleodias/automation_startup — visibility: PUBLIC (confirmado via API)
```

**O que dá para fazer com isso.** Qualquer pessoa pode enviar uma mensagem forjada, como se fosse um paciente respondendo. O fluxo aceita, lê a planilha, e a partir daí:

- **escreve na agenda do Google** — marcar consulta como confirmada, remarcada ou cancelada;
- **consome créditos da API da Anthropic** — cada requisição forjada dispara o nó `Claude — classificar intenção`, que chama `api.anthropic.com`;
- **dispara envios de WhatsApp**, já que os fluxos estão em produção.

**Por que no caso de vocês é pior que o normal.** O número já levou duas punições da Meta (6 horas e depois 7 dias). O próprio `DIARIO.md` registra que a terceira queima o número de vez. Abuso desse webhook é exatamente o tipo de comportamento que dispara a terceira.

**Como fechar**
1. No nó Webhook, ligar **Header Auth** com uma credencial secreta e cadastrar esse mesmo header na Evolution ao registrar o webhook.
2. Trocar o caminho `evolution-demo` por um valor longo e aleatório.
3. No Caddy, limitar `/webhook/` ao IP do próprio servidor (a Evolution roda na mesma máquina).
4. Rede de segurança: um nó de validação logo após o webhook, descartando o que não vier no formato esperado da Evolution.

---

### S-02 · O repositório público entrega o mapa da infraestrutura

O repositório é público (confirmado pela API do GitHub). Os documentos internos **não** vão para o site, mas estão no GitHub, legíveis por qualquer pessoa e indexados pela busca.

```
DIARIO.md:8         — IP do VPS: 93.127.212.39
A1-CONSTRUCAO.md    — moderneasyn8n.duckdns.org · moderneasyevo.duckdns.org
                      /message/sendText/demo · /instance/connectionState/demo
                      nome da instância: "demo"
```

A Evolution API é, nas palavras do próprio `infra/.env.example` de vocês, *"a senha mestra da API de WhatsApp — quem tem ela manda mensagens pelo seu número"*. Publicar o endereço não entrega a chave, mas reduz o ataque a uma única etapa: descobrir a chave. E o n8n e a Evolution estão na internet aberta, sem restrição de IP e sem limite de tentativas.

**Como fechar**
1. Decidir se o repositório precisa ser público. Se for para portfólio, pode ficar **privado** — o site continua no ar, porque o Pages publica de um artefato próprio.
2. Se ficar público: tirar IP, domínios e caminhos dos documentos. **Apagar não basta** — o histórico do git guarda; é preciso reescrever o histórico ou rotacionar o que foi exposto.
3. No Caddy, permitir o painel do n8n só a partir dos IPs de vocês (ou pôr atrás de senha básica) e limitar a taxa de requisições na Evolution.

---

## 🟠 Risco médio

### S-03 · A imagem da Evolution não tem versão fixada

`infra/docker-compose.yml` usa `evoapicloud/evolution-api:latest`. Qualquer reinício pode trazer uma versão diferente sem vocês saberem — quebra a reprodutibilidade e, no pior caso, entrega código novo não revisado ao serviço que controla o WhatsApp. As outras imagens estão presas a versões maiores (`caddy:2`, `postgres:16`, `redis:7`), o que é aceitável.

**Como fechar:** fixar uma versão específica e atualizar de propósito, lendo o changelog antes.

### S-04 · A página não declara uma política de conteúdo (CSP)

O GitHub Pages não deixa configurar cabeçalhos HTTP, mas dá para declarar a política dentro da própria página. Hoje não há nenhuma. Se um dia alguém conseguisse injetar um script — via comprometimento do repositório ou do Actions — ele rodaria sem restrição. Uma política restritiva transforma esse cenário em nada.

**Como fechar:** uma linha no `<head>` das duas páginas, permitindo apenas o que o site usa (os próprios arquivos e o Google Fonts). Não muda nada visualmente.

### S-05 · A planilha da agenda está versionada no repositório público

`dados/clinica-sorriso-agenda.xlsx` está no repositório. Verifiquei o conteúdo: **são dados fictícios** — nomes inventados e telefones no padrão `5551999990…` —, então hoje não há exposição de dado pessoal real.

O problema é o hábito. No dia em que entrar a agenda de um cliente de verdade, com nome, telefone e horário de paciente, o mesmo gesto vira vazamento de dado sensível de saúde. O próprio `AUTOMACOES.md` já pede *"LGPD desde o dia 1, mesmo com dados fictícios"*.

**Como fechar:** adicionar `dados/` ao `.gitignore` agora, enquanto o custo é zero.

---

## ⚪ Risco baixo

### S-06 · Links externos sem `noreferrer`
Todos os sete links que abrem em nova aba já têm `rel="noopener"` — que é a parte de segurança, impede a página aberta de mexer na de vocês. Falta o `noreferrer`, que é privacidade: sem ele, WhatsApp e Instagram recebem de qual página o visitante veio.

### S-07 · O Google Fonts é o único terceiro da página
As fontes vêm do Google, então o IP de cada visitante chega até lá; se o serviço cair, o site troca para as fontes do sistema. Hospedar as duas fontes junto do site eliminaria o terceiro e deixaria a página mais rápida.

### S-08 · Telefone e e-mail em texto puro
Proposital — vocês querem ser encontrados. O efeito colateral é coleta automática por robôs de spam. Não recomendo esconder: atrapalharia o cliente.

### S-09 · Confirmar duas chaves na configuração do GitHub
Não consigo verificar daqui e valem trinta segundos de vocês:
- **"Enforce HTTPS"** está ligado nas configurações do Pages?
- A branch `main` tem proteção contra push direto e force-push? Como ela publica o site automaticamente, **quem escreve nela escreve no ar**.

---

## ✅ Verificado e correto

Cada item abaixo foi testado, e vários são coisas que a maioria dos sites pequenos erra.

| O quê | Detalhe |
|---|---|
| **Nenhum segredo vazado** | 89 commits de histórico varridos: nenhuma chave, senha ou token. O `.env` nunca foi commitado e o `.gitignore` está correto. |
| **Sem XSS** | Todo `innerHTML` usa texto fixo. O que o visitante digita só sai por `encodeURIComponent`, em URL de esquema fixo. O parâmetro `?a=` é validado contra lista fechada. |
| **Publicação enxuta** | O workflow monta uma pasta só com o HTML e as imagens da marca. Documentos internos, fluxos, planilha, PDF e `infra/` **não** vão para o ar. |
| **Permissão mínima no Actions** | `contents: read`, `pages: write`, `id-token: write`. Sem permissão de escrita no repositório. |
| **O site não coleta nada** | Sem cookie, sem armazenamento local, sem analytics, sem formulário que envie dados. A frase do rodapé é verdadeira. |
| **Sem cadeia de terceiros** | Zero bibliotecas de JavaScript, nenhum CDN de código. Foi por isso que valeu converter a página do passo a passo em vez de carregar o runtime React que veio junto. |
| **Banco protegido** | Postgres e Redis não expõem porta para fora do servidor — só conversam dentro da rede do Docker. |

---

## Plano, na ordem que eu faria

| # | Ação | Resolve | Prazo |
|---|---|---|---|
| 1 | Fechar o webhook do n8n (Header Auth + caminho aleatório) | S-01 | Esta semana |
| 2 | Decidir a visibilidade do repositório | S-02 | Esta semana |
| 3 | Restringir o painel do n8n e limitar taxa na Evolution (Caddy) | S-02 | Este mês |
| 4 | Fixar a versão da imagem da Evolution | S-03 | Este mês |
| 5 | Tirar `dados/` do git e declarar a CSP | S-04, S-05 | Este mês |
| 6 | Conferir HTTPS obrigatório e proteção da `main` | S-09 | Quando abrirem o GitHub |

---

## ⚠️ Achado que não é de segurança, mas trava a publicação

Encontrei lendo o workflow: o passo **"Montar o site"** copia apenas `index.html` e as imagens da marca. A `passo-a-passo.html` **não é copiada**, e o filtro `paths:` que dispara o deploy também não a inclui.

Do jeito que está, ao mesclar na `main` os três botões "Ver o passo a passo" vão dar **404**. É corrigível em duas linhas no `.github/workflows/deploy-pages.yml`.
