# Geralzão da Landing Page — o que melhorar

> **Feito em:** 18/09/2026 · branch `claude/landing-page-startup-umwqcx` (commit `2da7b96`).
> **Método:** página instrumentada no Chromium (1440px e 390px), medindo em vez de opinar: alturas, espaçamentos, escala tipográfica, fontes efetivamente renderizadas, peso, animações, foco, links e vocabulário. Cada achado traz o número que o sustenta.

## Resumo

**O que está forte:** identidade consistente (verde, âmbar só em ação — apenas 2 botões âmbar na página inteira), copy sem nenhuma palavra proibida do manual, zero erros de JavaScript, nenhum link quebrado, sem rolagem horizontal no celular, e as animações têm propósito (a rede do hero, a corda do "Como funciona", a conversa do portfólio).

**O que puxa para baixo:** a página foi montada em blocos que vieram de fontes diferentes (nosso código, dois designs do Claude Design) e **cada bloco trouxe seu próprio ritmo** — espaçamentos, tamanhos de título e alinhamento mudam de seção para seção. Somado a isso, a seção de Integrações promete 15 ferramentas quando cinco existem, o celular não tem menu, e um título está na fonte errada.

| Área | Achados |
|---|---|
| A · Honestidade e marca | 3 |
| B · Ritmo e estrutura | 4 |
| C · Tipografia | 2 |
| D · Navegação e fluxo | 4 |
| E · Performance | 3 |
| F · Qualidade visual | 1 |
| G · Acessibilidade | 1 |

---

## A · Honestidade e marca

### A-1 · Integrações mostra 15 ferramentas; existem 5 — **o mais importante desta lista**
A constelação exibe Claude, ChatGPT, Gemini, WhatsApp, Gmail, Telegram, Lovable, Outlook, Slack, Google Sheets, Drive, Docs, Agenda, Notion e Trello. O que está construído e documentado em `AUTOMACOES.md` e `A1-CONSTRUCAO.md`: **WhatsApp (Evolution), Google Sheets, Gmail, Claude API e QuickChart**. Google Agenda foi explicitamente deixada fora da v1 ("Sheets e não Calendar").

O pilar da marca é *prova, não promessa*. Um dono de clínica que pergunta "vocês integram com o Trello?" e ouve "ainda não" perde a confiança na página inteira. E colocar **ChatGPT e Gemini** como "o cérebro por trás" quando o cérebro é o Claude é impreciso.

**Sugestão:** manter só o que existe (5–6 ícones ficam ótimos numa constelação menor) e, se quiserem sinalizar expansão, uma linha honesta: *"Novas integrações entram conforme os clientes pedem."*

### A-2 · O case 03 anuncia uma automação que não existe
"Lembrete de pagamento sem constrangimento" linka para a página do **Relatório do dono** (A3). Já sinalizado antes; continua aberto. Ou o card vira o relatório, ou a cobrança é construída.

### A-3 · "Cases" é a única palavra em inglês da navegação
O manual pede "português de dono de PME". O rodapé chama a mesma seção de **Portfólio**; a navbar, de **Cases**. Escolher um nome (sugiro *Automações* ou *Portfólio*) e usar nos dois lugares.

---

## B · Ritmo e estrutura

### B-1 · Cada seção tem um espaçamento diferente
Medido em 1440px (padding superior / inferior): hero 120/80 · cases 96/104 · como 120/130 · integ 110/140 · chamada 108/90. Não há uma régua — cada bloco veio com a sua. O olho não sabe dizer por quê, mas sente que "não encaixa".

**Sugestão:** um token único (`--secao-y: clamp(88px, 10vw, 128px)`) aplicado em todas.

### B-2 · Os títulos de seção têm quatro tamanhos
H2 renderizados: 46px (cases) · 50px (como) · 52px (integ) · 50px (chamada). Os H3 dos cards: 34px no portfólio, 29px no "Como funciona". Uma escala só (ex.: H2 = `clamp(30px, 3.6vw, 48px)`) resolve.

### B-3 · Alinhamento alterna sem critério
Hero e Chamada são alinhados à esquerda; Portfólio, Como funciona e Integrações são centralizados. Alternar é legítimo se for uma decisão; hoje é herança de origem. Sugiro: **cabeçalhos de seção sempre à esquerda** (casa com o hero e com a marca "alinhada à logo" que o Leo pediu), conteúdo pode centralizar quando é diagrama (constelação).

### B-4 · Quatro blocos escuros seguidos, contra a regra do manual
O manual diz *"escuro abre, claro explica, escuro fecha — nunca dois blocos escuros seguidos"*. Hoje: hero (escuro) → cases (verde-mata) → como (escuro) → integ (escuro) → chamada (escuro). A página tem **6.124px de altura no desktop e 7.844px no celular**, e sem um respiro claro tudo vira uma parede verde. "Como funciona" nasceu clara (areia) e foi escurecida por causa da corda; vale reconsiderar — a corda funciona em fundo claro com traço verde-mata.

---

## C · Tipografia

### C-1 · O título da seção de contato está na fonte errada (bug)
"Enquanto você lê isto…" renderiza em **Figtree**, não em Sora. Causa: usa `var(--font-title)`, mas essa variável é declarada dentro do escopo do portfólio (linha ~502), não no `:root`. Fora dali ela não existe e cai no fallback. Correção de uma linha — mover as duas variáveis para `:root`.

### C-2 · Corpo de texto varia entre 14,5 e 17px sem regra
Não é grave, mas junto com B-2 contribui para a sensação de "colcha". Dois tamanhos bastam: corpo (17px) e apoio (15px).

---

## D · Navegação e fluxo

### D-1 · No celular não existe menu
Em 390px a navbar esconde os três links e sobra só o botão WhatsApp. **Zero links de navegação visíveis.** Quem abre pelo telefone — a maioria dos donos de PME — não tem como ir a "Como funciona" ou "Contato" sem rolar 7.800px. Falta um menu compacto (ícone que abre os links).

### D-2 · A intro toca em toda visita, sempre
São ~3,5s de animação de entrada (timeline `T`, saída em 2.900ms + fade) **a cada carregamento**, inclusive ao voltar da página "passo a passo". Não há `sessionStorage`. Na primeira visita é um cartão de visitas; na terceira, é uma porta que demora a abrir. Sugestão: tocar uma vez por sessão.

### D-3 · Nomes diferentes para as mesmas coisas
Navbar: *O que fazemos · Cases · Contato*. Rodapé: *Portfólio · Como funciona · Integrações · Fale conosco*. Chips da caixa de mensagem: *Fale com a gente* (vindo do rodapé antigo). Um vocabulário só, em toda a página.

### D-4 · "O que fazemos" leva ao topo da página
Funciona, mas o destino é o hero, que o visitante acabou de ver. Se a seção "Como funciona" é a resposta a "o que fazemos", o link devia ir para lá — e a navbar ganharia "Integrações" que hoje só existe no rodapé.

---

## E · Performance

### E-1 · 236 KB de HTML, dos quais 126 KB são imagens em base64
Vinte e uma imagens estão coladas dentro do HTML. Isso impede cache (toda visita baixa tudo de novo) e trava o *first paint* até o arquivo inteiro chegar. As logos das integrações e os avatares do portfólio deviam ser arquivos em `landingPage/assets-marca/`.

### E-2 · O mesmo símbolo em base64 aparece 3 vezes
O avatar do portfólio é a mesma imagem repetida em cada card (~30 KB × 3). Um arquivo `simbolo.png` referenciado três vezes = baixado uma.

### E-3 · 24 elementos animando ao mesmo tempo no primeiro quadro
Rede do hero, símbolo 3D, glow do portfólio, "digitando…", corda, partículas, pulsos da constelação. Em notebook fraco ou celular de entrada isso esquenta. Sugestão: pausar animações de seções fora da tela (`IntersectionObserver` — o "Como funciona" já faz isso; estender às outras).

---

## F · Qualidade visual

### F-1 · O símbolo 3D do hero está borrado
O arquivo `simbolo.png` tem **256px** de largura e é exibido a **574px** — esticado 2,2×. Em tela Retina, 4,5×. É a peça mais vistosa do hero e é a única imagem sem nitidez. Precisa de uma exportação em SVG (ideal — o símbolo é vetorial por natureza) ou PNG de 1200px.

---

## G · Acessibilidade

### G-1 · Três botões sem indicação de foco
Os chips "Confirmar consultas / Responder orçamentos / Outra rotina" da caixa de mensagem não mostram contorno ao navegar por teclado. Um `:focus-visible` resolve. Os demais 20+ elementos interativos estão corretos.

---

## Status (18/09/2026)

Aplicado no commit seguinte a este documento: **onda 1** (B-1, B-2, C-1, D-3 e A-3 — "Cases" virou "Portfólio", e "Como funciona" entrou na navbar), **onda 3** inteira (D-1 menu do celular, D-2 intro uma vez por sessão, G-1 foco) e da **onda 4** o E-1 e o E-2 (as 21 imagens em base64 viraram 16 arquivos em `landingPage/assets-marca/integracoes/` + o `simbolo.png` reaproveitado; HTML de 240 KB para 140 KB). O workflow de publicação passou a copiar a `passo-a-passo.html` e as logos.

**Ficam esperando decisão de vocês:** A-1 (Integrações só com o que existe), A-2 (case 03), B-3 (alinhamento dos cabeçalhos), B-4 (ritmo claro/escuro), D-4 (destino de "O que fazemos"), E-3 (pausar animações fora da tela) e F-1 (precisa do símbolo em SVG).

## Plano, em ondas

| Onda | O quê | Resolve | Esforço |
|---|---|---|---|
| **1 · Coerência** (1 sessão) | Tokens únicos de espaçamento e escala de título; `--font-title` no `:root`; alinhamento dos cabeçalhos; vocabulário único da navegação | B-1, B-2, B-3, C-1, C-2, D-3 | baixo — é CSS |
| **2 · Honestidade** (decisão de vocês + 1 sessão) | Integrações só com o que existe; case 03; "Cases" → português | A-1, A-2, A-3 | baixo, mas precisa da decisão |
| **3 · Celular e entrada** (1 sessão) | Menu mobile; intro uma vez por sessão; foco nos chips | D-1, D-2, G-1 | médio |
| **4 · Peso e nitidez** (1 sessão) | Imagens para arquivos; símbolo vetorial; pausar animações fora da tela | E-1, E-2, E-3, F-1 | médio — precisa do SVG do símbolo |
| **5 · Ritmo claro/escuro** (decisão do Leo) | Voltar "Como funciona" para o claro, ou clarear Integrações | B-4 | médio |

A onda 1 é a que mais muda a percepção de "profissional" por menos esforço: é ela que transforma cinco blocos em uma página.
