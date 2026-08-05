# Prompt para o Claude Design — Página "Ver o passo a passo" (demonstração das automações)

> **Como usar:** copie TUDO abaixo da linha e cole no Claude Design.
> **Anexe junto:** `landingPage/assets-marca/simbolo.png`, `logo-principal.png`,
> `logo-negativa-para-fundo-escuro.png` e, se possível, as páginas **CORES**, **TIPOGRAFIA** e
> **GRAFISMOS** do `Manual de Identidade Visual - ModernEasy.pdf`.
> **Print útil de anexar:** a seção de portfólio já publicada
> (`https://oleodias.github.io/automation_startup/`) — é o irmão visual desta página.

---

Você é um **diretor de arte e engenheiro front-end** especialista em páginas de produto
premiadas (nível Awwwards), com forte domínio de **animação com propósito didático** — o tipo
de animação que **ensina**, não a que apenas decora.

## Missão

Criar a página **"Ver o passo a passo"**: a página para onde o visitante vai ao clicar num
case do portfólio da ModernEasy. Ela precisa fazer o visitante **entender e sentir** como uma
automação funciona, em menos de dois minutos, sem nenhum vocabulário técnico.

## A marca — ModernEasy

Empresa de **Encantado, Vale do Taquari (RS, Brasil)** que instala **automações com IA para
pequenas e médias empresas**, rodando dentro do **WhatsApp e das planilhas que o cliente já
usa**. Posicionamento: *"automação sem complicação, feita aqui no Vale"*.

**Público desta página:** dono ou dona de PME do interior — clínica, loja de móveis, mercado.
Não é técnico, não quer virar técnico, e desconfia de promessa grande. Ele precisa olhar e
pensar **"ah, então é isso que acontece"**, não *"que bonito, mas não entendi"*.

**Três atributos que toda peça respira:** próxima (local, humana, fala simples), competente
(técnica, organizada, cumpre o que promete), concreta (mostra o processo e o número, nunca
jargão).

**Pilar de tom de voz que rege esta página inteira:** *prova, não promessa*. A página mostra o
processo acontecendo em vez de adjetivar o resultado.

## O conceito central que queremos

A tela é dividida em duas metades que **conversam entre si**:

- **De um lado, uma conversa de WhatsApp que se digita sozinha**, em cards animados: o cliente
  final (a paciente, o comprador) trocando mensagens com a automação. Bolhas entrando uma a
  uma, indicador de "digitando…", ritmo de conversa real.
- **Do outro lado, o painel que explica** o que está acontecendo **naquele exato momento** da
  conversa. Quando a mensagem de confirmação sai, o painel mostra que o sistema leu a agenda
  do dia seguinte na planilha. Quando a paciente responde "2", o painel mostra a automação
  procurando horários livres.

**Esse sincronismo é o coração da página** e o que a torna diferente de um vídeo: a conversa
avança e a explicação acompanha, passo a passo, destacando a etapa ativa. O visitante vê a
mágica e, ao lado, a engrenagem — ao mesmo tempo.

Queremos **controle nas mãos do visitante**: poder pausar, voltar, avançar etapa a etapa ou
clicar direto numa etapa da explicação e ver a conversa saltar para aquele ponto. Quem tem
pressa pula; quem se interessou assiste de novo.

Surpreenda-nos no acabamento. Buscamos algo **moderno, fluido e caro**, com microinterações
bem resolvidas — mas onde cada animação **existe para explicar alguma coisa**.

## Conteúdo real (use exatamente este material — não invente automação)

São **três automações construídas e rodando em produção**. A página é a mesma para as três,
mudando só o conteúdo. **Preencha as três**, com a **A1 como padrão ao abrir**.

### A1 · Confirmação de consultas — *piloto real, Clínica Sorriso*

> Rótulo da página: **Piloto · Clínica Sorriso** · dados fictícios, fluxo real.

**A dor:** a recepção passa horas ligando uma por uma para confirmar as consultas do dia
seguinte, e mesmo assim aparecem cadeiras vazias.

**A conversa (paciente Maria, véspera da consulta):**

1. *(automação → Maria, 10h)* "Olá, Maria! Sua consulta com a Dra. Ana é amanhã às 14h.
   Responda **1** para confirmar, **2** para remarcar, **3** para cancelar."
2. *(Maria)* "2"
3. *(automação)* "Sem problema! Tenho estes horários livres: **1)** amanhã às 16h30 ·
   **2)** quinta às 9h · **3)** quinta às 15h. Qual fica melhor?"
4. *(Maria)* "pode ser quinta de manhã"
5. *(automação)* "Pronto, Maria! Remarquei para **quinta, 9h**, com a Dra. Ana. O horário de
   amanhã já foi liberado para outra pessoa. Até lá!"

**As etapas para o painel explicativo:**

- **Todo dia às 10h, sozinho** — o sistema lê na planilha as consultas de amanhã que ainda
  estão como "agendada" e dispara a confirmação. Ninguém aperta nada.
- **A resposta chega e é entendida** — número (1, 2, 3) ou texto solto. Se a paciente escreve
  *"não vou poder, meu filho adoeceu"*, uma IA lê a intenção e encaminha para o caminho certo,
  ou chama a recepção quando é caso de gente.
- **Os horários livres saem da própria agenda** — o sistema consulta a planilha e oferece três
  opções que realmente existem.
- **A remarcação acontece nos dois lados** — o novo horário entra na agenda e o antigo é
  liberado. O horário vago é oferecido a quem está na lista de espera: se aceitar em duas
  horas, entra; senão, passa para o próximo.
- **A recepção só recebe o que importa** — às 7h chega um resumo do dia: confirmadas,
  pendentes, remarcadas e quem não respondeu. O trabalho manual vira exceção, não rotina.
- **E se algo parar?** — a cada 30 minutos um monitor testa se tudo está de pé e avisa a
  ModernEasy antes do cliente perceber.

### A2 · Resgate de orçamentos parados

> Rótulo: **Automação · Atendimento** · demonstração com dados fictícios.

**A dor:** o orçamento foi enviado, o cliente não respondeu, ninguém teve tempo de cobrar — e
a venda morreu em silêncio.

**A conversa (cliente João, orçamento de móveis planejados):**

1. *(automação → João, 48h após o envio)* "Oi, João! Conseguiu ver o orçamento dos planejados
   da cozinha? Ficou alguma dúvida que eu possa resolver?"
2. *(João)* "Vi sim, achei um pouco acima do que eu esperava"
3. *(automação)* "Entendi, João. Vou passar para o Rodrigo, que fez o seu orçamento — ele te
   chama em instantes para ver o que dá para ajustar."
4. *(alerta interno → vendedor Rodrigo)* "**João Prates** respondeu o orçamento de
   **R$ 18.400** (cozinha planejada, enviado dia 12). Comentou que achou acima do esperado."

**As etapas para o painel:**

- **Varredura diária, às 9h** — o sistema procura na planilha os orçamentos enviados há mais
  de 48 horas que continuam sem resposta.
- **No máximo dois toques** — um lembrete leve aos 2 dias e outro aos 7, com motivo honesto.
  Mais que isso é insistência, e insistência queima a marca do cliente.
- **Respondeu? O humano assume** — a automação reabre a conversa e entrega o contexto pronto
  ao vendedor. Quem fecha a venda é gente.
- **Sexta às 17h, o placar** — quantos orçamentos foram varridos, quantos voltaram a responder
  e quanto isso representa em reais.

### A3 · Relatório do dono

> Rótulo: **Automação · Gestão** · demonstração com dados fictícios.

**A dor:** o dono só descobre que a semana foi ruim quando o mês fecha.

**A conversa (automação → dono, 19h):** uma mensagem só, com **imagem de gráfico** e legenda:

1. *(automação → dono, 19h)* **[gráfico de rosca com a divisão do dia]** "Boa noite! A agenda
   de amanhã soma **R$ 4.820**. Confirmação da semana: **86%**. Nas últimas duas semanas, a
   confirmação automática evitou **11 faltas** — cerca de **R$ 3.100** que não escorreram pelo
   ralo. Bom descanso!"

**As etapas para o painel:**

- **Todo dia às 19h** — o sistema lê a agenda e calcula o valor do dia seguinte, a taxa de
  confirmação da semana e quantas faltas foram evitadas.
- **O gráfico é gerado na hora**, nas cores da marca, e vai junto da mensagem.
- **Chega pronto no WhatsApp do dono** — sem painel para acessar, sem senha, sem relatório
  para abrir. A informação vai até ele.
- **Complementa o resumo da recepção** — às 7h a equipe recebe a lista operacional; às 19h o
  dono recebe o retrato em reais. Cada um recebe o que usa.

### Detalhe de bastidor (opcional, para quem clicar em "por dentro")

Todas as automações compartilham um **roteador**: o WhatsApp aceita só um ponto de entrada por
número, então uma peça central lê cada mensagem que chega e decide qual automação deve
responder. Sem ela, a segunda automação nunca receberia nada. É o tipo de detalhe que mostra
competência sem virar aula de tecnologia — use com parcimônia, num nível secundário.

## Honestidade (regra inegociável)

A página **precisa deixar claro, sem letra miúda escondida**, que a conversa mostrada é uma
**demonstração com dados fictícios de um fluxo que roda de verdade**. A Clínica Sorriso é o
piloto e deve aparecer rotulada como piloto. Nada de inventar cliente, depoimento, logo de
empresa ou número que não esteja aqui.

## Restrições técnicas (rigorosas)

- **Página completa e autossuficiente:** um único arquivo HTML com **CSS e JS inline**. **Sem
  bibliotecas, sem CDN, sem fontes ou imagens externas.** Ela será publicada como página
  estática no GitHub Pages, irmã do `index.html`.
- **Fontes:** **Sora** (títulos) e **Figtree** (texto), via Google Fonts — as duas únicas da
  marca. Não introduza uma terceira.
- **Conteúdo em estrutura de dados:** as três automações devem viver num **objeto JavaScript
  no topo do arquivo** (roteiro da conversa + etapas do painel + rótulos), e a página renderiza
  a que estiver selecionada. Assim adicionamos a quarta automação sem mexer no layout.
- **Navegação entre as três** dentro da própria página, e ela deve conseguir abrir já numa
  automação específica (por exemplo via `?a=a1` ou `#a1`), porque cada card do portfólio vai
  linkar direto para a sua.
- **Foco em desktop** (~1440–1920px), mas **não pode quebrar nem gerar rolagem horizontal** no
  celular — o dono de PME abre link no telefone o tempo todo.
- **Respeite `prefers-reduced-motion`:** quem pediu menos movimento vê a conversa **inteira já
  montada** e as etapas todas visíveis, sem digitação automática — a página continua completa e
  compreensível, só não se move.
- **Acessibilidade:** contraste AA, navegação por teclado nos controles, `aria-label` nos
  botões de ícone, e a conversa legível por leitor de tela.
- **Performance:** ~60fps, sem travar, sem esquentar máquina.
- **Deixe comentados os parâmetros ajustáveis** (velocidade da digitação, pausa entre
  mensagens, intensidade das animações) para calibrarmos depois.

## Sistema de design da marca

| Papel | Cor | Hex |
|---|---|---|
| Fundo escuro principal | Verde-abissal | `#06251C` |
| Fundo escuro secundário / blocos | Verde-mata | `#0B3D2E` |
| Destaque, ícones, sinal | Verde-vivo | `#1FA36C` |
| **Ação (só CTA)** | Âmbar | `#F5A524` |
| Fundo claro | Areia | `#FAF7F2` |
| Texto sobre claro | Grafite | `#1C1C1C` |
| Apoio, legendas | Cinza-verde | `#5C6B64` |

**Regras de cor que não se negociam:**

- **O âmbar aparece uma única vez por dobra**, sempre como fundo de botão com texto grafite.
  Nunca como texto sobre areia, nunca no fundo, nunca decorativo. *Se tudo grita, nada grita.*
- **Nunca use o verde oficial do WhatsApp** (`#25D366`) nas bolhas. Vizinhança sim, imitação
  não — as bolhas usam o **nosso** verde-vivo.
- Proporção geral: ~60% do ambiente base, 25% verdes escuros, 10% verde-vivo, 5% âmbar.

**Grafismos da marca disponíveis** (use se fizerem sentido, sem empilhar):
o **símbolo é uma linha ascendente que conecta três nós** — pode virar divisor, trilho de
progresso da conversa ou marca-d'água gigante cortada na borda a 5–8% de opacidade (uma por
tela, num canto, nunca atrás de texto denso). Rótulos técnicos curtos em caixa-alta com
prefixo `//` são parte da linguagem visual.

**Ritmo da página:** a marca trabalha com alternância — **escuro abre, claro explica, escuro
fecha**. Você tem liberdade para orquestrar, desde que nunca empilhe dois blocos escuros
seguidos sem respiro.

## O que evitar

- **Vocabulário proibido na marca:** "disruptivo", "soluções inovadoras", "IA de ponta",
  "transformação digital", "chatbot", "robô". Também evite "usuário" — é *o cliente*, *a
  paciente*, *o dono*.
- Explicação em linguagem de programador: "webhook", "cron", "API", "payload", "nó". O painel
  fala **em português de dono de PME**. (No máximo, um nível secundário opcional pode nomear a
  ferramenta.)
- Clichês visuais: placa de circuito, cérebro de IA, código Matrix, foguete, robôzinho, aperto
  de mão de banco de imagem, azul corporativo.
- Animação decorativa que não explica nada, ou tão rápida que não dá tempo de ler.
- Emoji em títulos e em textos institucionais. Nas bolhas de conversa, no máximo um por
  mensagem — é assim que gente escreve no WhatsApp.
- Prometer resultado que não está neste documento.

## Entregáveis

1. **Dois ou três conceitos** para a página, cada um em um parágrafo curto: a ideia central,
   como a conversa e a explicação se sincronizam, e por que combina com a marca.
2. **Implemente o melhor conceito** como artifact HTML completo e funcional, com as **três
   automações preenchidas** e a A1 abrindo por padrão.
3. Uma **lista curta dos parâmetros** que deixamos ajustáveis e onde encontrá-los no arquivo.

## Critérios de aceite (confira antes de entregar)

- [ ] A conversa se anima sozinha e a explicação ao lado **acompanha a etapa ativa**.
- [ ] Dá para **pausar, repetir e navegar** entre as etapas; clicar numa etapa move a conversa.
- [ ] As **três automações** estão completas e alcançáveis por link direto.
- [ ] Está claro que é **demonstração com dados fictícios** de um fluxo real.
- [ ] **Um único âmbar por dobra**, e as bolhas não usam o verde do WhatsApp.
- [ ] Sora nos títulos, Figtree no texto, nenhuma terceira fonte.
- [ ] Arquivo único, sem biblioteca externa, sem CDN, sem imagem externa.
- [ ] `prefers-reduced-motion` entrega a página inteira legível e parada.
- [ ] Sem rolagem horizontal em 390px de largura.
- [ ] Uma pessoa leiga entende o que a automação faz **sem ler nenhuma palavra técnica**.

Capriche ao máximo. Esta página é onde o visitante decide se confia na ModernEasy: ele acabou
de ver a promessa no portfólio e clicou justamente para conferir se existe substância atrás
dela. Queremos algo digno de premiação — e, acima disso, **claro o bastante para o dono de uma
clínica do interior entender sozinho, na primeira vez.**
