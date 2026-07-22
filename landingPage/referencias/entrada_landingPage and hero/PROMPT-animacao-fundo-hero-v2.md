# Prompt v2 para o Claude Design — Fundo animado do Hero com marca-d'água do símbolo

> Copie o texto abaixo e cole no Claude. **Anexe junto:** `simbolo.png`, `logo-principal.png`
> (em `landingPage/assets-marca/`) e, se possível, a página "MARCA D'ÁGUA" do Manual de
> Identidade — para o Claude ver a regra e a logo reais.

---

Você é um **diretor de arte / engenheiro front-end** especialista em animações web premium,
do nível de sites premiados internacionalmente (Awwwards). Sua missão: **conceber e
implementar um fundo animado sofisticado para o hero (topo) de uma landing page**, com a
nossa logo integrada em grande formato como marca-d'água, e uma animação que interage com
ela criando profundidade e movimento — sem nunca prejudicar a leitura do texto.

## Sobre a marca — ModernEasy

Empresa do **Vale do Taquari (RS, Brasil)** que instala **automações com IA para pequenas e
médias empresas** (confirmação de agendamentos, resposta a orçamentos, cobrança), operando
dentro do WhatsApp e das planilhas que o cliente já usa. Posicionamento: **"automação sem
complicação, feita aqui no Vale"**. Deve transmitir **confiança, competência e clareza** —
tecnológica **e** calorosa, corporativa e limpa. **NÃO** somos "startup fria do Vale do
Silício".

## A logo / símbolo (ver anexos)

O **símbolo** é uma **linha verde em ziguezague ascendente** (visual de gráfico de
crescimento subindo) que **conecta nós (pontos)** e termina num **círculo cheio maior no
canto superior direito**. Leitura: *um fluxo que sobe conectando pontos* = etapas de uma
automação; a subida = o resultado crescendo. É esse símbolo que deve virar a marca-d'água.

## O conceito que quero

1. **Marca-d'água do símbolo, gigante, ao fundo** — seguindo a regra do nosso Manual de
   Identidade (INEGOCIÁVEL):
   > **"Símbolo gigante, cortado na borda, opacidade 5–8%. Um por peça, sempre num canto —
   > nunca atrás de texto denso."**
   Na prática: o símbolo entra **grande, cortado pela borda direita** da tela, tom sobre tom
   (um verde levemente distinto do fundo, quase imperceptível, ~5–8% de opacidade). O texto
   do hero fica **à esquerda**, então a marca-d'água mora **à direita** — nunca atrás do
   texto.
2. **Animação elegante, fluida e premium** que **interage com essa marca-d'água**, criando
   **profundidade e movimento**. A animação nasce do próprio símbolo (um fluxo de nós), não é
   enfeite genérico. Ideias de inspiração (escolha/original — não precisa usar todas): pulsos
   de luz percorrendo o contorno do símbolo; camadas em **parallax** dando profundidade; o
   símbolo "respirando" bem devagar; partículas/linhas que fluem em direção ao nó terminal e
   florescem nele; luz que atravessa a marca-d'água. Surpreenda com algo **original**.
3. **Legibilidade acima de tudo:** mais energia/movimento à **direita**; calmo e discreto à
   **esquerda**, atrás do texto branco. Nada pode competir com a leitura.

## Referência (só inspiração da TÉCNICA — NÃO copiar)

Anexo um print de `atompoint.com`. **Gostei apenas de como o fundo animado é construído** (a
fluidez, a profundidade). **NÃO** quero o design deles: nada do **azul**, nada dos grafismos
específicos, nada da estética "agência de tecnologia". Use a **nossa identidade (verde)**.
Princípio: **roube a técnica, não a alma.** (Detalhe revelador: a palavra que eles destacam,
"Disruptive", é **proibida** na nossa marca — não somos aquilo.)

## Restrições técnicas (rigorosas)

- **Fundo escuro:** verde-abissal `#06251C`.
- **Cores da animação e da marca-d'água:** apenas **verdes** (verde-vivo `#1FA36C` e
  variações/tons; a marca-d'água pode ser um verde quase-neutro, tom sobre tom). **Âmbar
  `#F5A524` é PROIBIDO no fundo** (reservado só para botões).
- **Foco em DESKTOP** (~1440–1920px). Não precisa otimizar celular agora.
- **Texto à esquerda, por cima:** título grande em branco/areia `#FAF7F2`, alinhado à
  esquerda, centralizado na vertical. A animação e a marca-d'água **não podem** reduzir o
  contraste do texto.
- **Self-contained:** HTML + CSS + JS puro (SVG e/ou `<canvas>`), **sem bibliotecas, sem
  CDN, sem imagens externas** — tudo inline, para colar num único `index.html`.
- **Performance:** ~60fps, leve, **loop contínuo sem "corte" perceptível**; não pode esquentar
  o notebook.
- **Acessibilidade:** respeite `prefers-reduced-motion` (versão estática/calma — a
  marca-d'água pode ficar, só o movimento para).
- **Entregue como um artifact** pré-visualizável, já com o fundo escuro + a marca-d'água + um
  **texto grande de exemplo à esquerda** provando que a leitura fica impecável.

## Paleta

| Papel | Hex |
|---|---|
| Verde-abissal (fundo) | `#06251C` |
| Verde-mata | `#0B3D2E` |
| Verde-vivo (animação) | `#1FA36C` |
| Areia (texto claro) | `#FAF7F2` |
| Âmbar (SÓ botões — proibido no fundo) | `#F5A524` |

## O que evitar

- Copiar a referência (azul, grafismos, estética de agência).
- Clichês: placa de circuito, "cérebro de IA", código Matrix, partículas genéricas sem
  relação com o nosso símbolo.
- Marca-d'água atrás do texto, ou opaca demais (fica pesada e prejudica a leitura).
- Âmbar/laranja no fundo.
- Animação pesada/travada.
- **Vocabulário proibido** (não escreva em textos de exemplo): "disruptivo", "soluções
  inovadoras", "IA de ponta", "transformação digital", "chatbot".

## Entregáveis

1. **2 a 3 conceitos distintos**, cada um em um parágrafo: a ideia, como deriva do nosso
   símbolo e da marca-d'água, e por que combina com a marca.
2. **Implemente o melhor conceito** como artifact HTML self-contained e funcional (fundo +
   marca-d'água + texto de exemplo à esquerda para provar a legibilidade).
3. Deixe **comentados os parâmetros ajustáveis** (velocidade, densidade, opacidade da
   marca-d'água, intensidade do movimento) para eu calibrar depois.

Capriche ao máximo — este é o primeiro segundo que o nosso cliente vê da empresa, e quero
algo digno de premiação, único, que fortaleça a nossa identidade.
