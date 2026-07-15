# Consultoria — Startup de Automação (Vale do Taquari/RS)

*Resposta ao prompt v3 — julho/2026. Preços verificados na web em 15/07/2026; onde há faixa, é estimativa e está sinalizada.*

## Quadro-resumo de decisões

| Decisão | Recomendação |
|---|---|
| **Nicho** | Clínicas e consultórios pequenos (odonto, estética, fisioterapia, veterinária) de Lajeado/Estrela/Encantado |
| **Oferta de entrada** | Confirmação e reagendamento automático de consultas pelo WhatsApp, que corta faltas (no-show) pela metade — instalado em 2 semanas na agenda que a clínica já usa |
| **Cobrança** | Híbrido: setup R$ 1.200–1.800 + mensalidade R$ 350 (piso absoluto p/ 1º cliente: R$ 800 + R$ 250) |
| **Canal nº 1** | Rede pessoal morna + visita presencial (indicação > tudo; cold WhatsApp é último recurso) |
| **Infra** | n8n self-hosted + Evolution API em VPS Hostinger (~R$ 50–100/mês) — não usar n8n Cloud agora |
| **1ª ação (<1h, esta semana)** | Cada um lista 15 donos/gestores de negócio a até 1 grau de distância (família, colegas UNIVATES/TripleTen, comerciantes de quem já são clientes) e envia 3 mensagens pedindo 20 min de conversa sobre "como funciona a agenda/atendimento de vocês" — descoberta, não venda |

---

## 0. Reality check — as 5 premissas mais frágeis

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

## 2. Plano de 90 dias (nicho: clínicas)

Orçamento de horas: 2 pessoas × 10–15h/sem × 13 semanas ≈ **260–390h disponíveis**. O plano abaixo soma **~190h** — folga proposital de ~30% para prova na UNIVATES, imprevisto no emprego e retrabalho.

**Quinzena 1 (dias 1–14) — Descoberta. ~25h (12h/pessoa).**
Fazer: cada um lista 15 contatos a 1 grau (colegas de aula com família empreendedora, professores, o dentista/fisio de vocês mesmos); roteiro de 6 perguntas de descoberta (quantas faltas/sem? quem confirma? como? quanto custa uma falta? já tentou resolver? o que aconteceu?); realizar **≥ 8 conversas de 20–30 min** (presencial ou chamada), sem vender nada.
Entregável: planilha com 8+ conversas anotadas (números reais de faltas e processo atual) + decisão registrada: clínicas confirmado ou pivô justificado.

**Quinzena 2 (dias 15–28) — Demo e infra. ~40h.**
Fazer: contratar VPS Hostinger, subir Docker + n8n + Evolution API (você lidera — é sua praia de infra); Leonardo constrói o fluxo P1 do item 3 (confirmação com clínica fictícia); gravar vídeo-demo de 3 min com celular na mão; escrever one-pager da oferta com preço.
Entregável: demo funcionando 24/7 acessível de qualquer celular + vídeo de 3 min + one-pager. **Critério: a demo tem que rodar ao vivo sem vocês tocarem em nada.**

**Quinzena 3 (dias 29–45) — Prospecção onda 1. ~35h.**
Fazer: 30 abordagens (priorize os 16+ contatos mornos da quinzena 1 e as indicações que eles derem); meta de 10 conversas comerciais e **3 demos apresentadas ao vivo**; enviar ≥ 2 propostas (modelo do item 4).
Entregável: funil registrado em planilha: 30 abordados / conversas / demos / propostas.

**✅ CHECKPOINT DIA 45 — critérios objetivos:**
- ≥ 30 abordagens feitas E ≥ 8 conversas → o canal funciona.
- ≥ 2 propostas enviadas OU 1 em negociação → **manter** nicho e oferta.
- Muitas conversas, mas ninguém pediu proposta → **ajustar oferta/preço** (a dor existe, a solução ou o preço não convenceram): testar piloto de R$ 500/30 dias.
- < 8 conversas apesar de 30 abordagens → **trocar de nicho** (vocês não têm acesso a esse público): migrar para contabilidade, reaproveitando ~70% do fluxo (cobrança de documentos usa a mesma base técnica).

**Quinzena 4 (dias 46–60) — Fechamento. ~30h.**
Fazer: follow-up das propostas (cadência do item 5); se travar, oferta de fundador: setup com 40–50% de desconto em troca de case documentado + depoimento + 2 indicações; **iniciar abertura do CNPJ no dia em que houver "sim" verbal** (item 6 — leva 1–3 semanas, não pode começar depois da assinatura); onda 2: +20 abordagens.
Entregável: 1 contrato assinado (meta) + CNPJ em andamento.

**Quinzena 5 (dias 61–75) — Implantação do cliente 1. ~35h.**
Fazer: kickoff de 1h com dono + secretária; mapear a agenda real; adaptar o fluxo; rodar 1 semana em paralelo ao processo manual (a secretária confere); go-live; treinar a secretária (30 min + PDF de 1 página).
Entregável: automação em produção + medição baseline de faltas (antes) registrada.

**Quinzena 6 (dias 76–90) — Case e escala. ~25h.**
Fazer: medir faltas antes/depois; montar case de 1 página com números; pedir formalmente 3 indicações ao cliente 1 ("quem você conhece que sofre com faltas?"); onda 3 de prospecção liderada pelo case.
Entregável: case com número real + pipeline com ≥ 3 leads para o trimestre seguinte.

**Plano B (dia 91, zero clientes):** não recomeçar do zero — diagnosticar **onde o funil quebrou**, nesta ordem: (1) *Propostas enviadas e não fechadas* → problema de risco/confiança, não de nicho: voltar aos que disseram "quase" com piloto pago de R$ 500/30 dias e garantia de devolução; (2) *Conversas sem virar proposta* → dor fraca ou oferta errada: trocar a oferta dentro do mesmo nicho (ex.: resgate de orçamento de tratamento não fechado, que mexe em receita e não em custo); (3) *Abordagens sem virar conversa* → problema de canal/rede, não de mercado: trocar canal (pedir 1 indicação a cada conversa antiga, ir a eventos ACIL/CIC Vale do Taquari) antes de trocar de nicho. Só trocar de nicho se (3) persistir — e o nicho nº 2 é contabilidade.

---

## 3. Portfólio sem clientes

**P1 — Confirmação de consultas "Clínica Sorriso" (a demo principal). ~30h.**
Stack: n8n self-hosted + Evolution API (WhatsApp) + Google Sheets como agenda (ou Google Calendar) + Claude/GPT via API para interpretar respostas fora do padrão ("não vou poder, meu filho adoeceu" → intenção: remarcar).
Escopo mínimo: agenda em planilha → D-1 às 10h dispara mensagem → paciente responde 1 (confirmo) / 2 (remarcar) / 3 (cancelar) → planilha atualiza + horário liberado é oferecido à lista de espera → 7h do dia seguinte, resumo para a "secretária".

**P2 — Resgate de orçamentos parados. ~15h.**
Stack: n8n + Evolution API + Google Sheets.
Escopo mínimo: planilha de orçamentos → 48h sem resposta dispara follow-up personalizado → 7 dias, segunda tentativa com gatilho de escassez → alerta ao vendedor quando o cliente responde. Serve para comércio E para clínicas (orçamento de tratamento) — mesma demo, dois mercados.

**P3 (só se sobrar tempo) — Relatório do dono. ~8h.** Todo dia às 19h, resumo do dia (agendamentos, confirmações, faltas) no WhatsApp do dono. Barato de fazer e é o que o dono mostra pros amigos — gera indicação.

**Roteiro exato da demo de 10 min (dono leigo):**
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

## 7. Presença digital mínima

Resposta direta: **no estágio de vocês, site não é prioridade — canal 1-a-1 é.** A venda dos 3 primeiros clientes acontece por indicação e demo presencial; o site serve só como "verificação de legitimidade" quando o dono pesquisa vocês antes da reunião. Então: uma página única (GitHub Pages ou Carrd, R$ 0) com: quem somos (foto + nome + cidade — rosto local vende no interior), o que fazemos em 1 frase, o vídeo-demo de 3 min, 1 case quando existir, e botão de WhatsApp. Máximo 4h de trabalho, na quinzena 2, e não se toca mais nela. **NÃO precisa:** blog, SEO, identidade visual paga, domínio próprio no início (adiável; R$ ~40/ano quando quiserem).

**A UMA prioridade recomendada:** o **vídeo-demo de 3 minutos + WhatsApp Business bem montado** (perfil completo com foto, descrição, respostas rápidas e catálogo com a oferta). É o ativo que roda dentro do canal onde a venda acontece — um vídeo bom encaminhado por um indicador vale mais que qualquer site.

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

1. **Quantos nomes reais a rede morna de vocês tem?** Listem os 30 contatos (1ª ação) e me digam o número — se for < 20, o funil do item 5 precisa de outro canal desde o início.
2. **Quem é o rosto comercial?** Quem dos dois fará as visitas e demos presenciais — e essa pessoa consegue liberar 2 horários de almoço por semana em Lajeado?
3. Vocês toleram fechar o cliente 1 com **oferta de fundador** (setup ~R$ 800 + garantia de devolução) para acelerar, ou preferem defender o preço cheio e arriscar mais 30 dias de funil?
4. As clínicas de que vocês já são pacientes/têm proximidade usam **qual agenda hoje** (papel, Google Agenda, iClinic)? Isso define quanto do P1 é integração vs. planilha.
5. Existe algum cenário em que um de vocês **reduz o emprego em 12 meses** se houver 3+ clientes pagando, ou o teto de 10–15h/semana é fixo? Isso muda a meta de mensalidade acumulada que faz sentido perseguir.

---

*Fontes de preços consultadas (jul/2026): [n8n Pricing](https://n8n.io/pricing/) e [guia de preços n8n 2026](https://www.lowcode.agency/blog/n8n-pricing); [VPS Hostinger Brasil 2026](https://kildaryoliver.com.br/quanto-custa-vps-hostinger-brasil/) e [comparativo de VPS de entrada](https://guiadohost.com/2026/05/12/vps-barato-no-brasil-2026-compare-6-planos-de-entrada-e-descubra-o-melhor-custo-beneficio/); [custo da API oficial do WhatsApp 2026](https://blog.umbler.com/br/custo-api-oficial-do-whatsapp-2026/) e [comparativo de APIs de WhatsApp](https://blog.zapsterapi.com/post/melhores-api-para-whatsapp-2026-comparativo-completo); [MEI 2026 — atividades e DAS](https://buscadorncm.com.br/blog/mei-atividades-permitidas-cnae-tributacao-guia) e [programador pode ser MEI em 2026?](https://wetax.com.br/programador-pode-ser-mei-em-2026).*
