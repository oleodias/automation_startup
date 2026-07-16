# Consultoria — Startup de Automação (Vale do Taquari/RS)

*Resposta ao prompt v3, revisada (v2) em 16/07/2026 com o novo objetivo dos fundadores: os 90 dias entregam **estrutura da startup + landing page + portfólio de automações demonstráveis** — fechar cliente vira bônus, não meta. Preços verificados na web em 15/07/2026; onde há faixa, é estimativa e está sinalizada. O plano de construção detalhado das automações está em [`AUTOMACOES.md`](AUTOMACOES.md).*

## Quadro-resumo de decisões

| Decisão | Recomendação |
|---|---|
| **Meta dos 90 dias** | Estrutura da startup pronta + landing page no ar com 3 automações demonstráveis (vídeo + demo interativa no WhatsApp); cliente pagante = bônus |
| **Nicho-alvo do portfólio** | Clínicas e consultórios pequenos de Lajeado/Estrela/Encantado (as automações 2 e 3 também cobrem comércio/serviços) |
| **Oferta de entrada** | Confirmação e reagendamento automático de consultas pelo WhatsApp, que corta faltas (no-show) pela metade — instalado em 2 semanas na agenda que a clínica já usa |
| **Cobrança (para o trimestre 2)** | Híbrido: setup R$ 1.200–1.800 + mensalidade R$ 350 (piso absoluto p/ 1º cliente: R$ 800 + R$ 250) |
| **Infra** | n8n self-hosted + Evolution API em VPS Hostinger (~R$ 55–100/mês) — não usar n8n Cloud agora |
| **1ª ação (amanhã)** | Contratar o VPS e subir Docker + n8n + Evolution API com chip de teste dedicado (checklist do dia 1 em `AUTOMACOES.md`) — sem isso nenhuma automação sai do papel |

---

## 0. Reality check — as 5 premissas mais frágeis

> **Nota da revisão v2 (obrigatória de ler):** trocar a meta de "1 cliente pagante" por "landing page + portfólio" é uma decisão legítima — reduz a pressão comercial e joga a favor do ponto forte de vocês (construir). Mas ela **aumenta** o risco do erro nº 1 do item 9: construir 90 dias no vácuo e descobrir no dia 91 que o portfólio responde dores que ninguém paga para resolver. Dois antídotos ficaram embutidos no plano e não são negociáveis: (a) **5 conversas de validação na quinzena 1** — os números colhidos nelas viram a copy da landing page; (b) a landing page nasce com **captura de leads** (a demo interativa registra quem testou), para que o dia 91 comece com uma lista de interessados, não do zero. E fique claro: landing page é *apoio* de venda, não *canal* de venda — para PME do interior, quem vende continua sendo indicação + demo no celular do dono (item 5, agora agendado para o trimestre 2).

**1. "Ticket mínimo R$ 800" está mal formulado, não baixo.** R$ 800 como projeto único é armadilha: vocês entregam, o fluxo quebra em 3 semanas (WhatsApp muda, API cai) e vocês dão suporte de graça para sempre. O que fazer: R$ 800 é o *piso do setup*; a meta real é **setup + mensalidade ≥ R$ 250**. Um cliente de R$ 800 único é pior que um de R$ 500 + R$ 300/mês. A meta de 90 dias, reformulada assim, é realista.

**2. O gargalo não é técnico, é agenda comercial.** Donos de PME atendem em horário comercial — exatamente quando vocês dois estão empregados. As 10–15h/semana existem, mas caem à noite e no fim de semana, quando ninguém compra. O que fazer: reservar deliberadamente horários de almoço e sábados de manhã para visitas/conversas, e usar WhatsApp assíncrono (áudio + vídeo-demo) para vender fora do horário. Sem isso, o plano inteiro trava no item 5.

**3. Vocês assumem que dor manual = disposição a pagar.** PME do interior convive há 15 anos com processo manual e acha normal ("a secretária liga confirmando"). Ela não paga por "automação"; paga por faltas a menos, orçamentos recuperados, horas da secretária liberadas. O que fazer: toda conversa e proposta em R$ e horas, nunca em "fluxos n8n". A fase de descoberta (quinzena 1) existe para achar quem *já sente* a dor — não para convencer quem não sente.

**4. Nenhum dos dois tem experiência de vendas — e vender é 60% deste plano.** A complementaridade de vocês (código + automação) é toda do lado da entrega. O que fazer: tratar venda como disciplina de estudo (item 8), definir já quem é o "rosto comercial", e compensar inexperiência com canal morno (indicação reduz a barreira de confiança que vocês ainda não sabem construir do zero).

**5. "n8n + WhatsApp de graça" não é infra de produção.** Sem PC 24/7 e sem VPS, hoje vocês não conseguem nem manter uma demo no ar. E a API não-oficial de WhatsApp (Evolution API) **pode ter o número banido pela Meta** — aceitável na sua demo, arriscado no número do cliente sem avisar. O que fazer: contratar VPS na quinzena 2 (custo no item 10), usar Evolution API para demo e pilotos *com o risco declarado em contrato*, e dominar a API oficial da Meta como caminho de produção para clientes que dependem do número (item 8).

---

## 1. Nichos e dores de mercado

| Nicho | Dor específica | Automação que resolve | Valor gerado (estimativa) | Como validar em 1 semana |
|---|---|---|---|---|
| **Clínicas/consultórios pequenos** (odonto, estética, fisio, vet) | 10–25% de faltas; secretária gasta 1–2h/dia ligando para confirmar | Confirmação D-1 via WhatsApp com resposta 1/2/3 → reagenda sozinha → resumo diário | Falta = R$ 100–400/consulta perdida; 5 faltas evitadas/sem ≈ R$ 2–6 mil/mês + ~30h/mês de secretária | Perguntar a 5 clínicas: "quantas faltas por semana?" e "quem confirma hoje, como?" — se a resposta vier com número na ponta da língua, a dor é real |
| **Escritórios de contabilidade** | Caçar documentos de clientes todo mês (notas, extratos); onboarding manual | Cobrança automática de documentos via WhatsApp com checklist e escalonamento | 20–60h/mês da equipe; atraso de obrigações = multa | Perguntar a 3 contadores: "quantos clientes atrasam documento e quanto tempo a equipe gasta cobrando?" |
| **Comércio com orçamento** (materiais de construção, móveis, autopeças) | Orçamentos enviados e nunca respondidos; vendedor não faz follow-up | Follow-up automático em 48h/7d + alerta ao vendedor + relatório do dono | Recuperar 5–10% dos orçamentos parados; ticket local R$ 500–5.000 | Pedir a 3 lojistas: "quantos orçamentos você manda por semana e quantos respondem?" — silêncio constrangedor = dor |
| **Oficinas e prestadores de serviço** | Cliente some após orçamento; agendamento por telefone caótico | Agendamento + lembrete + status do serviço via WhatsApp | 3–8h/sem do dono + retenção | 3 conversas presenciais em Lajeado; ciclo de decisão é rápido, mas ticket tolerado é baixo |
| **Agroindústrias/cooperativas** | Processos internos manuais (pedidos, romaneios, integração de sistemas) | Integrações ERP ↔ planilhas ↔ WhatsApp | Alto (R$ 5–20 mil/projeto) | **Descartar como entrada**: ciclo de venda de meses, exige CNPJ maduro, compliance — e a rede Sicredi está vetada para vocês |

**Recomendação: clínicas e consultórios pequenos.** Justificativa: (a) a dor é *quantificável na primeira conversa* — o dono sabe quantas faltas tem; (b) o ROI cabe exatamente na faixa de preço de vocês (mensalidade de R$ 350 contra R$ 2 mil+/mês de faltas evitadas); (c) densidade alta em Lajeado/Estrela/Encantado, dá para visitar a pé/carro; (d) o Leonardo já vive o setor saúde pelo lado da distribuição farmacêutica — vocabulário e empatia com o segmento, *sem* usar contatos da CIAMED; (e) é o nicho onde a entrega é mais padronizável — o mesmo fluxo se replica de clínica em clínica, que é o que torna isso uma startup e não um freela.

**Quem já resolve essa dor hoje (concorrência honesta):** (1) a própria secretária ligando — o concorrente nº 1 é "sempre foi assim"; (2) softwares de gestão de clínica com lembrete incluso (iClinic, Simples Dental, Amplimed — R$ 150–400/mês, estimativa), que mandam lembrete *unidirecional* mas raramente reagendam sozinhos e são genéricos; (3) agências de chatbot/marketing de Porto Alegre e freelancers, que vendem caro e somem depois do setup. **Nossa diferenciação honesta:** não somos mais baratos que ferramenta de prateleira nem mais robustos que a iClinic — somos os únicos que *aparecem na clínica*, integram com a agenda que ela já usa (Google Agenda, planilha, ou o próprio sistema), fazem a confirmação ser *bidirecional de verdade* (reagenda, não só lembra) e atendem em português de gente, presencialmente, em 24h. Para clínica pequena do interior sem TI, isso vale mais que feature.

**Hipótese do WhatsApp: validada, com uma ressalva.** WhatsApp é de fato o canal dominante de comunicação comercial de PME no Brasil — a hipótese está correta e a percepção de valor de automação sobre ele é imediata *quando a demo roda no celular do dono*. A ressalva: o dono já recebeu spam de bot ruim; "chatbot" tem cheiro de coisa irritante. Vendam "confirmação automática que reduz faltas", nunca "chatbot". E lembrem da premissa frágil nº 5: WhatsApp automatizado barato = API não-oficial = risco de banimento que precisa ser declarado.

---

## 2. Plano de 90 dias (v2 — meta: estrutura + landing page + portfólio)

Orçamento de horas: 2 pessoas × 10–15h/sem × 13 semanas ≈ **260–390h disponíveis**. O plano abaixo soma **~180h** — folga proposital de ~30% para prova na UNIVATES, imprevisto no emprego e retrabalho. O detalhamento técnico de cada automação está em `AUTOMACOES.md`.

**Quinzena 1 (dias 1–14) — Fundação técnica + validação mínima. ~30h.**
Fazer: **dia 1** — contratar VPS, subir Docker + n8n + PostgreSQL + Evolution API com HTTPS e conectar um **chip de teste dedicado** (~R$ 20 pré-pago; nunca o número pessoal — se a Meta banir, que banha um chip descartável); fluxo "hello world" webhook → WhatsApp. Nos demais dias: nome da startup + logo simples (Canva grátis) + número WhatsApp Business da empresa; e **5 conversas de 20 min** com donos conhecidos (o dentista/fisio de vocês, comerciantes de quem já são clientes) com uma pergunta central: *"quanto isso te custa por mês?"* — faltas, orçamentos sem resposta, tempo de secretária.
Entregável: infra 24/7 no ar respondendo no WhatsApp + nome/identidade definidos + 3 dores confirmadas **com números reais** (que viram a copy da landing page).

**Quinzena 2 (dias 15–28) — Automação A1: confirmação de consultas. ~40h.**
Fazer: construir A1 completa (5 fluxos: disparo D-1, interpretação de respostas com IA, lista de espera, resumo diário, healthcheck) sobre a clínica fictícia; divisão em `AUTOMACOES.md` — você nos fluxos de infra/cron, Leonardo nos fluxos de conversa/IA.
Entregável: A1 rodando de ponta a ponta com "pacientes" fictícios + roteiro do vídeo escrito. **Critério: funciona sem vocês tocarem em nada.**

**Quinzena 3 (dias 29–45) — Automações A2 e A3. ~35h.**
Fazer: A2 (resgate de orçamentos parados) e A3 (relatório diário do dono), reaproveitando ~70% da base de A1; colocar A1 em "operação assistida" — rodando todo dia com pacientes fictícios, monitorada pelo healthcheck.
Entregável: 3 automações funcionais + A1 com 7 dias seguidos de operação sem intervenção manual.

**✅ CHECKPOINT DIA 45 — critérios objetivos:**
- A1 rodou **7 dias seguidos sem intervenção manual**? Se não, pare tudo e estabilize — demo que falha na frente de visita mata a credibilidade da página inteira.
- A2 funcional e A3 encaminhada? Se A1 atrasou, **corte A3 do escopo dos 90 dias** — duas automações sólidas valem mais que três capengas.
- As 5 conversas da quinzena 1 aconteceram? Se não, façam agora — escrever a landing page sem os números reais é escrever ficção.

**Quinzena 4 (dias 46–60) — Landing page com Claude Code. ~30h.**
Fazer: construir a página (estrutura completa no item 7) com Claude Code — estática, hospedagem grátis, domínio `.com.br` (~R$ 40/ano, agora deixa de ser adiável); montar a **demo interativa**: botão "teste no seu WhatsApp" → link `wa.me` do chip de demo → n8n roda um roteiro guiado de 2 min simulando o paciente → salva o lead em planilha.
Entregável: página no ar em domínio próprio, com os 3 blocos de portfólio e a demo interativa capturando leads.

**Quinzena 5 (dias 61–75) — Vídeos, case e polimento. ~25h.**
Fazer: gravar os 3 vídeos de 60–90s (celular na mão, tela + WhatsApp lado a lado); montar 1 case **simulado e rotulado como simulação** (dados da clínica fictícia — honestidade aqui é diferencial, case inventado como se fosse real destrói a confiança na primeira pergunta); **teste com 5 pessoas leigas**: mostrar a página por 60 segundos e perguntar "o que a gente faz? quanto custa? o que você faria agora?" — se não souberem responder, a copy falhou e volta pra bancada; analytics básico (Cloudflare/Umami, grátis).
Entregável: página completa, validada por leigos, com vídeos e case no ar.

**Quinzena 6 (dias 76–90) — Soft launch + preparação comercial. ~20h.**
Fazer: divulgar na rede pessoal e nos grupos locais ("lançamos, dá uma olhada — pra quem você mandaria isso?"); acompanhar os leads capturados pela demo; preparar o **kit comercial do trimestre 2**: one-pager, proposta modelo (item 4) e cadência de prospecção (item 5) prontos para uso; decidir formalização (item 6) conforme o primeiro interessado aparecer.
Entregável: página publicada e divulgada + lista de leads/indicações + kit comercial pronto — **o dia 91 começa a venda com tudo na mão, não do zero**.

**Plano B (dia 91):** o risco do plano v2 é o inverso do v1 — chegar ao dia 91 com tudo bonito e **zero tração**. Se ninguém de fora da rede pessoal testou a demo nem pediu contato, o problema quase nunca é a página: é distribuição. Não gastem o trimestre 2 "melhorando o site"; ativem o funil do item 5 (indicação + visita presencial + demo no celular do dono), que continua integralmente válido — a landing page vira o material de apoio que faltava no plano v1. Se, além disso, as conversas do trimestre 2 mostrarem que as dores do portfólio não geram proposta, o pivô é de *oferta* dentro do nicho (checkpoint do plano v1), não de refazer a página.

---

## 3. Portfólio sem clientes — agora o entregável central

*(v2: este item deixou de ser apoio e virou o coração dos 90 dias. O detalhamento de construção — arquitetura, fluxos n8n, divisão de trabalho, checklist do dia 1 — está em `AUTOMACOES.md`. Aqui fica o escopo e o roteiro de demo.)*

**P1/A1 — Confirmação de consultas "Clínica Sorriso" (a demo principal). ~30h.**
Stack: n8n self-hosted + Evolution API (WhatsApp) + Google Sheets como agenda (ou Google Calendar) + Claude/GPT via API para interpretar respostas fora do padrão ("não vou poder, meu filho adoeceu" → intenção: remarcar).
Escopo mínimo: agenda em planilha → D-1 às 10h dispara mensagem → paciente responde 1 (confirmo) / 2 (remarcar) / 3 (cancelar) → planilha atualiza + horário liberado é oferecido à lista de espera → 7h do dia seguinte, resumo para a "secretária".

**P2/A2 — Resgate de orçamentos parados. ~15h.**
Stack: n8n + Evolution API + Google Sheets.
Escopo mínimo: planilha de orçamentos → 48h sem resposta dispara follow-up personalizado → 7 dias, segunda tentativa com gatilho de escassez → alerta ao vendedor quando o cliente responde. Serve para comércio E para clínicas (orçamento de tratamento) — mesma demo, dois mercados.

**P3/A3 — Relatório do dono. ~10h.** Todo dia às 19h, resumo do dia (agendamentos, confirmações, faltas, orçamentos recuperados) com 1 gráfico no WhatsApp do dono. Barato de fazer, é o vídeo mais bonito da landing page e é o que o dono mostra pros amigos — gera indicação. *(v2: promovida de "opcional" a parte do portfólio; é a primeira a ser cortada se A1 atrasar — ver checkpoint do dia 45.)*

**Roteiro exato da demo de 10 min (dono leigo — guardar para o trimestre 2 e usar nas visitas):**
- **Min 0–1 — a dor em R$:** "Quantas consultas marcadas por semana? E quantas faltas? [ele responde 5] Então: 5 faltas × R$ 150 × 4 semanas = R$ 3.000/mês indo embora. É isso que a gente ataca."
- **Min 1–4 — o celular DELE na mão:** você cadastra o número dele na planilha como "paciente de amanhã". Ele recebe a mensagem de confirmação na hora, no próprio WhatsApp. Peça: "responde 2". Ele vê o sistema oferecer horários e remarcar sozinho. *Esse é o momento da venda — tudo antes e depois é cerimônia.*
- **Min 4–6 — a agenda se atualizando sozinha:** projete a planilha/agenda: o horário dele mudou sem ninguém tocar em nada. Frase: "sua secretária não ligou pra ninguém".
- **Min 6–8 — o dia a dia:** mostre o resumo matinal da secretária e uma resposta "torta" de paciente sendo entendida pela IA. Antecipe a objeção: "e se o paciente escrever qualquer coisa? Olha o que acontece."
- **Min 8–10 — preço e fechamento:** "Isso instalado na agenda que você já usa, em 2 semanas: R$ 1.500 de implantação + R$ 350/mês com suporte e monitoramento. Se em 60 dias suas faltas não caírem, você cancela sem multa e sem custo." **Frase de fechamento: "Faz sentido eu montar isso na SUA agenda? Preciso de 1h da sua secretária e do acesso à agenda — semana que vem eu começo."**

---

## 4. Modelo de negócio e precificação

**Oferta de entrada em 1 frase:** *Para clínicas e consultórios pequenos do Vale do Taquari, instalamos em 2 semanas a confirmação e o reagendamento automático de consultas pelo WhatsApp, reduzindo faltas em 30–50%, por R$ 1.500 de implantação + R$ 350/mês.*

**Modelo recomendado para os 3 primeiros clientes: híbrido (setup + mensalidade).** Projeto fechado puro deixa vocês reféns de suporte gratuito eterno; mensalidade pura sem setup atrai cliente descompromissado e não paga as horas de implantação. O híbrido cobra a implantação (R$ 800–2.500 conforme complexidade; comece em R$ 1.200–1.500) e cria receita recorrente (R$ 250–600/mês; comece em R$ 350) — e mensalidade é o que transforma isso em negócio em vez de bico. Faixas realistas para PME do interior do RS (estimativas): abaixo de R$ 250/mês ninguém leva a sério; acima de R$ 600/mês, clínica pequena compara com "meio salário de recepcionista" e trava.

**Proposta comercial de 1 página (seções):**
1. *Sua situação hoje* — 2 frases com OS NÚMEROS QUE ELE TE DEU na conversa ("~5 faltas/semana ≈ R$ 3.000/mês").
2. *O que vamos instalar* — 3 bullets em português de gente (confirmação D-1, reagendamento sozinho, resumo diário). Zero jargão: nada de n8n, API, fluxo.
3. *O que você ganha* — meta numérica: "reduzir faltas de 5/semana para ≤ 2, liberando ~R$ 1.800/mês".
4. *Investimento* — setup + mensalidade, o que a mensalidade inclui (suporte, monitoramento, ajustes pequenos) e o que NÃO inclui (novos fluxos = novo orçamento).
5. *Prazo e o que precisamos de você* — 2 semanas; acesso à agenda; 1h da secretária.
6. *Garantia e suporte* — garantia de 60 dias (cancela sem multa se faltas não caírem) + SLA abaixo.

**Suporte com vocês dois em horário comercial — o que prometer sem mentir:** canal único de suporte (um número de WhatsApp da empresa); **resposta em até 12h úteis; problema crítico (automação parada) — solução ou contorno em até 24h**. Nunca prometam "suporte imediato" nem telefone. O truque técnico que torna isso cumprível: montem um workflow de *healthcheck* no n8n que testa os fluxos dos clientes a cada 30 min e avisa VOCÊS no WhatsApp antes de o cliente perceber — na prática vocês responderão em minutos à noite, mas o contrato diz 12h/24h e vocês nunca estarão inadimplentes com a própria promessa.

---

## 5. Aquisição dos 3 primeiros clientes

*(v2: este item sai dos 90 dias e vira o plano do **trimestre 2**, começando no dia 91 — com a landing page, os vídeos e a demo interativa como munição. Tudo abaixo permanece válido.)*

**Quem abordar:** dono, sócio ou gerente administrativo de clínica com 2–8 profissionais e recepção própria, **sem TI interno** — em Lajeado, Estrela, Encantado, Arroio do Meio e Teutônia. Nessa faixa, quem decide é quem sente a dor no caixa. Clínica de 1 profissional não tem volume; acima de 10, já tem software robusto e processo de compra lento.

**Ordem dos canais:** (1º) indicação da rede pessoal — inclui colegas e professores da UNIVATES, cujas famílias têm negócios na região, e os profissionais de saúde de quem vocês já são pacientes; (2º) presencial a frio bem-feito (entrar na clínica fora do horário de pico e pedir 5 min com o responsável); (3º) WhatsApp morno (com nome de quem indicou). Cold WhatsApp puro: só se o funil secar. **Sobre Sicredi/CIAMED:** respeitem o veto a prospectar os contatos das empresas — mas o conhecimento setorial é de vocês (o Leonardo entende a cadeia da saúde; você entende como cooperativa/PME pensa TI), e a rede *civil* de colegas de trabalho (o dentista do colega, a esposa fisioterapeuta de alguém) é legítima desde que a abordagem não use a relação comercial do empregador. Bônus regional: inscrevam-se na incubadora/ecossistema do Tecnovates (UNIVATES) — custa zero, dá mentoria, credibilidade e rede local exatamente no público que vocês querem.

**Mensagem pronta — WhatsApp (indicação morna):**
> Oi, [Nome]! Aqui é o [seu nome], o [indicador] me passou seu contato. Eu e um sócio montamos um sistema que confirma consultas automaticamente pelo WhatsApp — o paciente responde 1 pra confirmar ou 2 pra remarcar, e a agenda se ajusta sozinha. Nas clínicas, isso costuma cortar as faltas pela metade, sem a recepção ligar pra ninguém. Posso te mostrar funcionando no teu próprio celular? São 10 minutos, eu vou até a clínica no horário que for melhor. Quarta ou sexta te atende?

**Abordagem presencial / pedido de indicação:**
> "Oi, tudo bem? Sou o [nome], a gente atende clínicas aqui da região com um sistema que confirma consultas sozinho pelo WhatsApp — o paciente responde e a agenda se remarca sem a recepção ligar. Sei que agora é corrido: consigo 10 minutos com o [dono/responsável] essa semana pra mostrar no celular dele? Qual o melhor dia?" — e, para pedir indicação a qualquer conhecido: "Quem você conhece que tem clínica ou consultório e vive brigando com paciente que falta?"

**Cadência de follow-up:** D0 mensagem → D3 follow-up com o vídeo de 3 min ("te mando 3 minutos de vídeo, mais fácil que explicar por texto") → D10 último toque com um número ("na clínica X isso liberou R$ 1.800/mês — se fizer sentido, me chama") → silêncio por 30 dias → um único reengajamento com o case do cliente 1. **Máximo 3 toques por ciclo; nunca cobrar resposta com "e aí, viu minha mensagem?"** — cada toque precisa carregar algo novo (vídeo, número, case).

**Matemática reversa do funil** (taxas conservadoras para PME + canal morno): 1 fechamento ← ~3 propostas (fechamento ~30–35% em rede morna) ← ~10 conversas/demos (30% pedem proposta) ← ~40–50 abordagens (20–25% viram conversa quando há indicação; a frio seria 5–10%, ou seja, o triplo de abordagens — por isso o canal morno é a decisão mais importante do plano). Distribuído nas ~9 semanas de prospecção (quinzenas 3–6): **~5 abordagens/semana, ~1 conversa/semana, ~1 proposta a cada 2–3 semanas ≈ 3–5h/semana da dupla.** Cabe nas horas — apertado nas semanas de demo ao vivo, com folga nas demais. Se a rede morna de vocês tiver menos de ~30 nomes viáveis, o funil não fecha só com ela: é o primeiro número a verificar (pergunta 1 do fim).

**Desqualificar rápido (não gastar hora escassa) — descarte se:** (1) o dono não sabe dizer quantas faltas/orçamentos perde — quem não mede a dor não paga para resolvê-la; (2) você não consegue falar com quem decide em 2 toques — clínica onde "vou passar pro doutor" eterniza; (3) pediu "manda uma proposta por escrito" antes de ver a demo — proposta sem demo não fecha, insista na demo ou descarte; (4) quer "um sistema completo pra clínica" (prontuário, financeiro…) — isso é concorrer com a iClinic, não é a oferta; (5) chorou o preço abaixo de R$ 500 de setup ou quer "testar de graça uns meses" — piloto pago de R$ 500 é o desconto máximo, de graça nunca.

---

## 6. Formalização mínima

**MEI está descartado — não por opção, por lei.** As ocupações de desenvolvimento de software e TI (CNAEs 6201, 6202, 6209 etc.) **não constam na lista de atividades permitidas ao MEI** (Anexo XI da Resolução CGSN 140/2018); há projeto de lei para mudar isso, mas não foi aprovado. Não tentem contornar com CNAE de "reparação de computadores" — desenquadramento retroativo custa caro. Como vocês são dois, o caminho é **Sociedade Limitada (LTDA) com os dois sócios, no Simples Nacional**, usando o Fator R (pró-labore ≥ 28% da receita) para tributar pelo Anexo III (~6% no início). Custos estimados: abertura no RS ~R$ 300–800 em taxas (Junta Comercial + prefeitura; contabilidades online costumam não cobrar honorário de abertura) + **contador online R$ 130–300/mês** (Contabilizei, Agilize e similares) — só a partir da abertura.

**Quando abrir:** nem antes do primeiro contato, nem só na assinatura — **no "sim" verbal do primeiro cliente**. Abertura leva 1–3 semanas; começar no sim verbal significa ter CNPJ e conta PJ prontos na assinatura. Prospectar e demonstrar não exige CNPJ. (Alternativa de emitir como pessoa física via RPA existe, mas o cliente PJ paga ~20% de INSS patronal sobre o valor — na prática inviabiliza; não contem com ela.)

**Contrato de prestação de serviço — cláusulas mínimas:** escopo fechado **com lista explícita do que NÃO está incluído**; prazo de implantação e condição de início (acessos entregues pelo cliente); valores, vencimento e reajuste anual; SLA de suporte (os 12h/24h do item 4); propriedade e acesso — fluxos e credenciais documentados, cliente recebe cópia se sair (isso desarma a objeção "e se vocês sumirem?"); confidencialidade e LGPD; rescisão com aviso de 30 dias sem multa após os 60 dias de garantia; **limitação de responsabilidade** (automação pode falhar; vocês respondem pelo reparo, não por lucros cessantes); foro de Lajeado. Comecem com um modelo adaptado (Sebrae tem modelos gratuitos) e invistam R$ 300–800 (estimativa) numa revisão de advogado quando o cliente 1 assinar — não antes.

**LGPD — o mínimo que importa:** vocês serão *operadores* de dados dos clientes deles. Em clínica, atenção redobrada: **dado de saúde é dado sensível**. Regras práticas: os fluxos só tocam nome, telefone e horário — nunca prontuário ou motivo clínico da consulta; logs do n8n com retenção curta (7–14 dias) e sem conteúdo de mensagem além do necessário; credenciais em cofre (Bitwarden, grátis); VPS com acesso por chave SSH e firewall; cláusula de tratamento de dados no contrato (modelos gratuitos de DPA simples resolvem no início). Isso custa R$ 0 e elimina 90% do risco real.

---

## 7. Presença digital — a landing page como entregável central (v2)

*(Revisado: no plano v1 o site era secundário; com o novo objetivo, a landing page é um dos três entregáveis dos 90 dias. A recomendação cética permanece registrada: página não substitui o funil 1-a-1 do item 5 — ela o municia.)*

**O que a página PRECISA ter (nesta ordem, de cima para baixo):**
1. **Hero com a dor em número** — "Sua clínica perde R$ 3.000/mês com pacientes que faltam?" (o número vem das 5 conversas da quinzena 1, não de achismo) + 1 frase do que fazemos + botão de WhatsApp. Nada de "soluções inovadoras em automação inteligente".
2. **Portfólio — 3 blocos, um por automação (A1, A2, A3):** cada bloco = a dor em 1 frase + vídeo de 60–90s + botão **"Teste agora no seu WhatsApp"** (link `wa.me` do chip de demo que dispara o roteiro guiado de 2 min via n8n). A demo interativa é o diferencial da página inteira — nenhum concorrente regional deixa o visitante *sentir* a automação; e cada teste vira lead salvo em planilha.
3. **Quem somos** — foto, nomes, Lajeado/RS. Rosto local e endereço na região vendem no interior mais que qualquer selo.
4. **Faixa de investimento** — "a partir de R$ X de implantação + R$ Y/mês". Assusta curioso e qualifica lead; esconder preço só gera conversa perdida.
5. **CTA única e repetida** — falar no WhatsApp. Sem formulário longo, sem "agende uma call".

**O que NÃO precisa:** blog, SEO além de título/descrição básicos, múltiplas páginas, chatbot no site (ironia à parte — o WhatsApp é o chatbot), animações pesadas, identidade visual paga.

**Stack recomendada:** página estática (HTML/CSS/JS) gerada com Claude Code, hospedada de graça (GitHub Pages ou Cloudflare Pages), domínio `.com.br` ~R$ 40/ano (com o novo objetivo, deixa de ser adiável), captura de leads via webhook do próprio n8n (R$ 0), analytics leve e gratuito (Cloudflare Web Analytics ou Umami). Custo total da página: ~R$ 40/ano.

**Critério de pronto (quinzena 5):** 5 pessoas leigas olham a página por 60 segundos e respondem certo a "o que eles fazem? quanto custa? o que você faria agora?". Enquanto errarem, a copy volta pra bancada.

**Complemento que continua valendo:** WhatsApp Business da empresa bem montado (perfil completo, respostas rápidas, catálogo) — é para onde a página manda todo mundo.

---

## 8. Lacunas de estudo priorizadas (próximos 90 dias)

| Pessoa | O que estudar | Por que destrava receita | Fonte gratuita | Horas |
|---|---|---|---|---|
| Você | n8n na prática — construir P1 e P2 inteiros com as próprias mãos | O fluxo de confirmação É o produto; hoje o know-how de n8n está concentrado no Leonardo (risco de bus factor na entrega) | Docs oficiais n8n + cursos gratuitos da comunidade/YouTube | 20h |
| Você | Docker + deploy e operação de Evolution API + backup/restore no VPS | Sem infra estável não existe demo nem produção; é sua praia natural (Sicredi/redes), só falta o stack específico | Docs Evolution API + docs n8n self-hosting | 10h |
| Você | WhatsApp Cloud API oficial (Meta): templates, pricing por mensagem, verificação | É o plano B obrigatório do banimento e o caminho para clientes maiores; quem domina a API oficial cobra mais caro | Documentação Meta for Developers | 6h |
| Leonardo | Vendas consultivas e entrevista de descoberta (The Mom Test, SPIN Selling) | O gargalo do plano é vender, não construir (reality check nº 4); descoberta bem feita = proposta que se escreve sozinha | Resumos e vídeos de The Mom Test/SPIN no YouTube | 10h |
| Leonardo | Escopo e precificação de projetos de automação (proposta, change request, o que não incluir) | Evita o erro nº 4 do item 9 (escopo infinito) e protege a margem do 1º cliente | Conteúdo de agências n8n/automação no YouTube + comunidade n8n | 6h |
| Ambos | LGPD básica para prestadores que tratam dados de saúde | Clínica vai perguntar; responder bem fecha negócio, responder mal mata | Materiais gratuitos Sebrae + gov.br (ANPD, guias para pequenos agentes) | 4h |

~28h por pessoa em 13 semanas ≈ 2h/semana — cabe dentro (não além) das 10–15h, porque estudar aqui É construir o produto e o funil.

---

## 9. Erros comuns (e como saber que estamos cometendo)

**1. Construir meses antes de validar.** Sinal de alerta: 3 semanas seguidas de commits e zero conversas com donos de clínica. Correção: a regra do plano é descoberta ANTES da demo (quinzena 1 antes da 2); se inverteu, pare de codar e marque 3 conversas esta semana.

**2. Vender "automação/IA/n8n" em vez de resultado.** Sinal: o dono responde "que legal" e some; vocês se pegam explicando o que é um fluxo. Correção: reescrever o pitch começando pelo número dele ("suas 5 faltas/semana") e proibir jargão na proposta.

**3. Precificar baixo ou por hora "porque estamos começando".** Sinal: fechar setup abaixo de R$ 500, ou cliente pedindo "só mais um ajustezinho" grátis toda semana. Correção: piso de R$ 800 + R$ 250/mês; todo pedido fora do escopo responde-se com "consigo sim — te mando o orçamento desse ajuste".

**4. Escopo infinito no 1º cliente (dizer sim a tudo para não perder).** Sinal: o projeto de 2 semanas está na 6ª semana e o cliente ainda não pagou a 1ª mensalidade. Correção: cláusula do que NÃO está incluído + go-live com o escopo mínimo funcionando, melhorias viram fase 2 cobrada.

**5. Prometer disponibilidade que dois empregados CLT não têm (e depender de API não-oficial em silêncio).** Sinal: cliente ligando em horário comercial e ninguém responde; ou o número do cliente banido e vocês descobrindo pelo cliente. Correção: SLA por escrito de 12h/24h + healthcheck automático (item 4) + risco da API não-oficial declarado em contrato com plano de migração para a API oficial.

---

## 10. Mapa de investimento inicial

Refletindo a decisão de infra: **n8n self-hosted em VPS** (recomendado) em vez de n8n Cloud — o plano Starter do n8n Cloud custa €24/mês (~R$ 150/mês) e não resolve o WhatsApp; o VPS custa um terço disso e roda n8n + Evolution API juntos, e vocês têm perfil técnico de sobra para operá-lo.

| Item | Para que serve | Mínimo (R$) | Confortável (R$) | Quando se torna inevitável |
|---|---|---|---|---|
| VPS Hostinger KVM 1–2 (n8n + Evolution API) | Demo e produção 24/7 | ~R$ 55/mês (KVM1, plano longo)* | ~R$ 100/mês (KVM2 mensal, sem fidelidade) | Quinzena 2 (sem isso não há demo) |
| n8n self-hosted + Evolution API | Motor de automação + WhatsApp | R$ 0 | R$ 0 | — |
| API de IA (Claude/OpenAI) p/ interpretar respostas | Demo P1 e produção | R$ 20–40/mês (uso baixo) | R$ 60/mês | Quinzena 2 |
| WhatsApp Cloud API oficial (Meta) | Produção séria/plano B do banimento | R$ 0 agora (conversas de utilidade na janela de 24h são grátis; templates ~centavos/msg) | R$ 50/mês repassável ao cliente | Só ao migrar cliente p/ API oficial |
| Domínio .com.br + página (GitHub Pages) | Legitimidade | R$ 0 (subdomínio grátis) | R$ 40/ano | Adiável indefinidamente |
| Abertura LTDA (taxas Junta/prefeitura RS) | Existir juridicamente e faturar | R$ 300–800 (único, estimativa) | R$ 800 | No "sim" verbal do cliente 1 |
| Contador online (Contabilizei etc.) | Obrigações da LTDA | R$ 130–200/mês | R$ 300/mês | A partir da abertura do CNPJ |
| Revisão de contrato por advogado | Não assinar bomba | R$ 0 (modelo Sebrae adaptado) | R$ 300–800 (único, estimativa) | Confortável: na assinatura do cliente 1 |
| Prospecção (combustível, cafés, impressão do one-pager) | Visitas em Lajeado/Estrela/Encantado | R$ 50/mês | R$ 150/mês | Quinzena 1 |
| Cofre de senhas (Bitwarden), Canva free, Sheets | Operação | R$ 0 | R$ 0 | — |

\* preço promocional exige contratar 12–24 meses adiantado; no mensal sem fidelidade estimem R$ 80–110/mês.

**Totais:**
- **Mínimo absoluto até o 1º cliente** (VPS ~2,5 meses + IA + prospecção; CNPJ e contador só disparam com o "sim", entram aqui como gatilho do fechamento): **~R$ 350–500 até o "sim" + ~R$ 700–1.100 no fechamento (abertura + 1º mês de contador)** → na prática, **~R$ 1.100–1.600 no total do ciclo**.
- **Confortável para os 90 dias completos** (KVM2 mensal, IA com folga, advogado, abertura, 2 meses de contador, prospecção): **~R$ 2.800–3.500**.

Nos dois cenários, o primeiro contrato (R$ 1.500 + R$ 350/mês) praticamente paga o investimento do trimestre — o risco financeiro disso é baixo; o investimento real é as ~190 horas.

---

## Perguntas para a próxima conversa

1. **Qual número de WhatsApp vai rodar as demos?** Recomendo chip pré-pago dedicado (~R$ 20) — um banimento da Meta não pode derrubar o número pessoal de nenhum dos dois. Já compraram?
2. **Nome da startup:** têm candidatos? É pré-requisito do domínio e da página (quinzena 1/4) — travar nisso atrasa tudo; nome "bom o suficiente" ganha de nome perfeito.
3. **As 5 conversas de validação da quinzena 1: com quem, exatamente?** Citem os nomes — se não saírem 5 nomes em 2 minutos, esse é o primeiro problema a resolver.
4. **Quem aparece?** Quem grava a voz/rosto dos vídeos e assina o "quem somos" da página — e essa pessoa assume o funil de vendas do item 5 no dia 91?
5. As clínicas de que vocês já são pacientes usam **qual agenda hoje** (papel, Google Agenda, iClinic)? Isso define quanto de A1 é integração vs. planilha — e a resposta pode mudar o escopo antes de amanhã.

---

*Fontes de preços consultadas (jul/2026): [n8n Pricing](https://n8n.io/pricing/) e [guia de preços n8n 2026](https://www.lowcode.agency/blog/n8n-pricing); [VPS Hostinger Brasil 2026](https://kildaryoliver.com.br/quanto-custa-vps-hostinger-brasil/) e [comparativo de VPS de entrada](https://guiadohost.com/2026/05/12/vps-barato-no-brasil-2026-compare-6-planos-de-entrada-e-descubra-o-melhor-custo-beneficio/); [custo da API oficial do WhatsApp 2026](https://blog.umbler.com/br/custo-api-oficial-do-whatsapp-2026/) e [comparativo de APIs de WhatsApp](https://blog.zapsterapi.com/post/melhores-api-para-whatsapp-2026-comparativo-completo); [MEI 2026 — atividades e DAS](https://buscadorncm.com.br/blog/mei-atividades-permitidas-cnae-tributacao-guia) e [programador pode ser MEI em 2026?](https://wetax.com.br/programador-pode-ser-mei-em-2026).*
