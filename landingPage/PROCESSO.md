# Processo de Construção da Landing Page — ModernEasy

> **Para que serve este arquivo:** registrar cada decisão, raciocínio e mudança da
> landing page, para que qualquer Claude (o do Leo ou o do Matheus) entre no meio do
> trabalho e saiba exatamente onde estamos e por quê. **Leia este arquivo antes de mexer
> na landing page.** Atualizar a cada seção.

---

## Regras do jogo (combinadas com os sócios)

- **Fonte de verdade:** `Manual de Identidade Visual - ModernEasy.pdf` (nesta pasta) +
  tipografia **Sora** (títulos) e **Figtree** (texto). Não introduzir uma terceira fonte.
- **Paleta:** verde-abissal `#06251C`, verde-mata `#0B3D2E`, verde-vivo `#1FA36C`,
  âmbar `#F5A524` (**só** em CTA/ação), areia `#FAF7F2`, grafite `#1C1C1C`,
  cinza-verde `#5C6B64`.
- **Tom de voz — PROIBIDO:** "soluções inovadoras", "disruptivo", "chatbot",
  "IA de ponta", "transformação digital". Sempre concreto, simples, em português de
  dono de PME. Público: **PMEs em geral** (não só clínicas).
- **Assets:** `landingPage/assets-marca/`.
- **Como trabalhamos:** construção **seção por seção**, direto no `index.html`, na branch
  `claude/landing-page-startup-umwqcx`. A cada seção **aprovada**, merge na `main` →
  atualiza o GitHub Pages (`https://oleodias.github.io/automation_startup/`).
- **Preview durante a construção:** cada sócio abre o `index.html` no próprio navegador
  (VSCode → Sync para puxar → duplo-clique no arquivo). O Pages só reflete o que já foi
  para a `main` (seções aprovadas).
- **Referências:** `landingPage/referencias/` (cada um dropa `.txt` + prints + links).

---

## Direção de design (a partir das referências do Leo)

Referências em `landingPage/referencias/entrada_landingPage and hero/`:
- **atompoint.com** — dark premium, "gourmet corporativo". Elementos desejados:
  (1) elemento animado de fundo no hero, (2) título com **palavra destacada trocando**,
  (3) seção de **cards de projetos** rotativos levando a páginas de case.
- **steven.com** — **animação de entrada**: a logo sendo "escrita/desenhada" ao carregar.

### Princípio que rege tudo: "roubar a técnica, trocar a alma"
A atompoint vende para empresas de tecnologia (inglês, "Vale do Silício", copy abstrata —
destaca literalmente a palavra "Disruptive", que o **nosso** manual proíbe). Nós vendemos
para o dono de PME do interior. **Copiamos o acabamento** (dark premium, animações fluidas,
ritmo cinematográfico) mas **preenchemos com copy concreta, em português, sem jargão e sem
estatística na entrada.** Vantagem nossa: o **símbolo da ModernEasy já é um fluxo de nós**,
então nossa animação de fundo *significa* algo (uma automação sendo montada), não é enfeite.

### Ordem de construção aprovada
1. **Hero** (fundo animado + título com palavra alternando) ← estamos aqui
2. **Animação de entrada** (logo desenhada, estilo steven) — abraça o hero pronto
3. **Seção de cases** (cards → páginas de projeto). Primeiro case = **Clínica Sorriso como
   piloto/demonstração** (rotulado honestamente). Estrutura vira multi-página no Pages.

### Decisões dos sócios (2026-07-21)
- **Sem estatística no hero** (nada de "R$ X/mês", "−14h" no topo). Topo mais limpo/premium.
- **Público do hero:** PMEs em geral.
- **Primeiro case:** Clínica Sorriso como piloto.

---

## Registro por seção

### Seção 1 — HERO — v1 (em construção, ainda não aprovada)

**Arquivo:** `index.html` (reconstruído do zero; o mold antigo foi descartado).

**O que montei:**
- **Header** fixo (verde-abissal): logo negativa + navegação mínima (O que fazemos / Cases /
  Contato) + botão WhatsApp.
- **Hero dark** (verde-abissal) com:
  - **Fundo animado em SVG:** uma rede de nós verde-vivo conectados por linhas, com
    "pontos de dados fluindo" ao longo das conexões (stroke-dashoffset animado) + nós
    pulsando + leve flutuação. É o nosso conceito de fluxo de automação, ecoando a malha
    da atompoint mas com significado. Opacidade baixa para não atrapalhar a leitura.
  - **Título com palavra alternando** (destaque em **verde-vivo**, não âmbar — âmbar é só
    do botão). Estrutura em 3 linhas, igual à atompoint:
    > A ModernEasy cuida da **[confirmação de agendamentos / resposta aos orçamentos /
    > cobrança dos clientes / papelada do dia a dia / rotina no WhatsApp]** enquanto você
    > cuida do cliente.
    (As palavras que giram são todas femininas para casar com "cuida da". Cada troca é um
    serviço real nosso.)
  - **Subtítulo** concreto, sem estatística.
  - **CTA âmbar** "Teste no seu WhatsApp →" (link wa.me) + nota curta de confiança.
- **Rodapé** slim provisório (contato) só para a página não terminar no ar durante o preview.

**Acessibilidade / performance:** animações respeitam `prefers-reduced-motion` (quem pediu
menos movimento vê o hero estático e sem rotação de palavras). Tudo em CSS/SVG/JS puro, sem
biblioteca externa — carrega rápido no 4G.

**Aberto para lapidar com os sócios:**
- O texto exato do título e a lista de palavras que giram.
- Intensidade/velocidade da animação de fundo.
- Texto do subtítulo e da nota de confiança sob o botão.

**Refinamentos já feitos (com verificação visual via screenshot no Chromium):**
1. *Bug de layout pego no teste:* quando a palavra girando trocava de 2 linhas
   ("confirmação de agendamentos") para 1 linha ("resposta aos orçamentos"), o bloco
   inteiro do hero **pulava** para cima. Primeira tentativa: reservar a altura da palavra
   mais alta — resolveu o pulo mas criou um **vão morto** sob palavras curtas. Solução
   final: um **medidor invisível** mede a altura da próxima palavra e a altura do bloco
   **desliza suavemente** (`transition: height .45s`) junto com o fade — sem pulo e sem
   vão. (É o tipo de fluidez da referência atompoint.)
2. Rede de fundo estava tímida demais → opacidade das linhas de `.38` para `.5` e traço
   de `1.2` para `1.4`. Continua discreta de propósito (o texto manda no hero).

**Como verificar qualquer mudança:** abrir o `index.html` no navegador (ou, para o
Claude: Playwright + Chromium tirando screenshot em 1440px e 390px — script de exemplo
não versionado, é trivial de recriar). Esperar ~3s para ver a troca de palavra.

**Status:** v1 pronta para reação dos sócios. **Ainda não aprovada, ainda não foi para a
`main`** (o Pages continua mostrando o site antigo até a aprovação desta seção).

**Próximo passo:** Leo/Matheus abrem o `index.html` no navegador, reagem, ajustamos
palavra por palavra. Quando o Hero for aprovado → merge na `main` e partimos para a
animação de entrada (item 2 da ordem).
