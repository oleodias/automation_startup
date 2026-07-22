# Prompt para o Claude Design — Animação de fundo do Hero (ModernEasy)

> Copie o texto abaixo e cole no Claude. **Anexe junto os arquivos `simbolo.png` e
> `logo-principal.png`** (estão em `landingPage/assets-marca/`) para o Claude enxergar a logo.

---

Você é um designer front-end especialista em **animações web premium**. Sua missão é
**conceber e implementar uma animação de fundo para o hero (o topo) de uma landing page**,
derivada da nossa logo. Quero que você pense como diretor de arte: primeiro as ideias,
depois a execução impecável.

## Sobre a marca — ModernEasy

Somos uma empresa do **Vale do Taquari (RS, Brasil)** que instala **automações com IA para
pequenas e médias empresas** — coisas como confirmação de agendamentos, resposta a
orçamentos e cobrança, funcionando dentro do WhatsApp e das planilhas que o cliente já usa.

Nosso posicionamento: **"automação sem complicação, feita aqui no Vale"**. Precisamos
transmitir **confiança, competência e clareza** — "gente séria daqui que resolve". Somos
premium e tecnológicos, mas **calorosos e próximos**. NÃO somos, e não queremos parecer,
uma "startup fria do Vale do Silício".

## A logo (ver anexos: `simbolo.png` e `logo-principal.png`)

O **símbolo** é uma **linha verde em ziguezague ascendente** — visual de gráfico de
crescimento subindo — que **conecta nós (pontos)** e termina num **círculo cheio, maior, no
canto superior direito**. A leitura é: *um fluxo que sobe, conectando pontos.* Isso combina
com nosso conceito: **nós conectados = etapas de uma automação; a subida = o resultado
crescendo.**

O logotipo é "moderneasy" em minúsculas, com "modern" em verde-escuro e "easy" em
verde-vivo.

**Quero que a animação NASÇA desse símbolo** — que ela seja, de alguma forma, esse fluxo de
nós ascendente ganhando vida e se multiplicando pelo fundo. Não é um enfeite genérico posto
atrás do texto; é a **própria ideia da marca em movimento**.

## A sensação que buscamos

Sofisticada, fluida, elegante — "gourmet corporativo". Tecnológica **e** humana. Algo que
faça o visitante pensar "isso aqui é sério e bem-feito" no primeiro segundo.

**Referência de acabamento:** o hero de `atompoint.com` (uma malha animada fluindo ao
fundo). Gostamos do **nível de polimento e da fluidez** deles — mas a nossa versão deve ter
**significado** (o nosso símbolo é um fluxo de automação), enquanto a deles é decoração
abstrata. Roube a técnica, não a alma.

## Restrições técnicas (rigorosas)

- **Fundo escuro:** verde-abissal `#06251C`.
- **Cor da animação:** verde-vivo `#1FA36C` como principal (variações e tons de verde são
  bem-vindos, inclusive verdes mais claros para brilho). **NÃO use âmbar/laranja `#F5A524`
  no fundo** — essa cor é reservada exclusivamente para botões.
- **Foco em DESKTOP** (~1440px de largura). Não precisa otimizar para celular agora.
- **Legibilidade manda:** o texto (título grande em branco/areia `#FAF7F2`) fica **por cima,
  alinhado à esquerda**. A animação **não pode competir com a leitura** — pense em algo
  discreto, de baixa opacidade, ou com mais densidade/energia concentrada à direita e mais
  calma atrás do texto.
- **Self-contained:** HTML + CSS + JavaScript puro (pode usar SVG ou `<canvas>`), **sem
  nenhuma biblioteca externa, sem CDN, sem imagens externas** — tudo inline, para eu colar
  dentro de um único `index.html`.
- **Performance:** ~60fps, leve, **loop contínuo e sem "corte" perceptível**.
- **Acessibilidade:** respeite `prefers-reduced-motion` — quem pediu menos movimento vê uma
  versão estática ou bem calma.
- **Entregue como um artifact** que eu possa pré-visualizar rodando, já com o fundo escuro e
  um **texto de exemplo grande por cima** (à esquerda) para provar que a leitura fica limpa.

## Paleta completa (para referência)

| Papel | Hex |
|---|---|
| Verde-abissal (fundo) | `#06251C` |
| Verde-mata | `#0B3D2E` |
| Verde-vivo (animação) | `#1FA36C` |
| Areia (texto claro) | `#FAF7F2` |
| Âmbar (SÓ botões — proibido no fundo) | `#F5A524` |

## O que evitar

- Clichês de "tecnologia": placa de circuito, "cérebro de IA", código Matrix, partículas
  genéricas sem relação com a nossa logo.
- Qualquer coisa que atrapalhe a leitura do título.
- Âmbar/laranja no fundo.
- Animação pesada, que trave ou esquente o notebook.
- **Vocabulário proibido** (não escreva nada disso em textos de exemplo): "disruptivo",
  "soluções inovadoras", "IA de ponta", "transformação digital", "chatbot".

## O que quero de você (entregáveis)

1. **2 a 3 conceitos distintos**, cada um explicado em um parágrafo curto: qual é a ideia,
   como ela deriva do nosso símbolo (o fluxo ascendente de nós) e por que combina com a
   marca.
2. **Implemente o melhor conceito** como um artifact HTML self-contained e funcional, com o
   fundo escuro + texto de exemplo por cima para provar a legibilidade.
3. No código, deixe **comentados os parâmetros fáceis de ajustar** (velocidade, densidade de
   nós, opacidade, etc.), para eu calibrar depois.

Capriche — este é o primeiro segundo que o nosso cliente vê da empresa.
