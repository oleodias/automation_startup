# Decisão: em que avançar agora (30/07/2026)

*Material para Leonardo e Matheus decidirem juntos. Situação: **dia 14 de 90**, portfólio de 3 automações pronto — ~30 dias à frente do cronograma na parte técnica, e 0 conversas de validação feitas.*

---

## Opção A — Dados de demonstração + roteiro dos vídeos

**O que eu entrego**
- As duas planilhas recalibradas para um cenário realista: Clínica com ~25 consultas na semana (mix de confirmadas, aguardando, canceladas e 2–3 faltas) e 6–8 consultas para amanhã; Móveis Premium com ~15 orçamentos espalhados pelos estágios, com R$ recuperado plausível.
- Roteiro dos **3 vídeos de 60–90s**, cena por cena: o que aparece na tela, em que ordem, o que a voz fala, quanto dura cada trecho e a frase de fechamento.
- Checklist de gravação: o que abrir antes, como enquadrar celular + tela, e **o que nunca pode aparecer** (chaves de API, números pessoais, nomes reais).

**O que vocês fazem:** trocar as planilhas (10 min), rodar as automações para popular os dados, gravar os 3 vídeos (2–3h contando refilmagens) e uma edição mínima (1–2h).

**O que isso destrava:** é o **pré-requisito da landing page**. Hoje as automações funcionam, mas são invisíveis para quem está fora — não existe um único artefato que vocês possam mandar para alguém. Depois disso, existe.

**Custo de adiar:** as automações continuam sendo um projeto interno. Nada para mostrar, nada para encaminhar.

---

## Opção B — Demo interativa no WhatsApp

**O que eu entrego**
- Fluxo n8n completo (JSON pronto): o visitante manda "QUERO TESTAR" → recebe a confirmação como se fosse paciente da Clínica Sorriso → responde → vê a remarcação acontecer → recebe o mini-relatório do dono → o sistema pede nome/empresa e **salva o lead numa planilha**.
- Proteção anti-abuso (1 demo por número/dia) e aviso claro de que é uma demonstração automática.
- O trecho de código do botão `wa.me` para a landing page.

**O que vocês fazem:** importar, configurar, testar (2–3h).

**O que isso destrava:** o diferencial competitivo da página — nenhum concorrente regional deixa o visitante *sentir* a automação. E cada teste vira um lead registrado. **Bônus que eu não tinha considerado:** serve também na conversa presencial — em vez de mostrar vídeo, você manda o link e o dono testa no celular dele ali, na hora.

**Custo de fazer agora:** a página ainda não está no ar, então o uso "visitante do site" tem público zero por enquanto. É a opção tecnicamente mais divertida — e por isso a armadilha mais fácil.

---

## Opção C — Robustez para cliente real

**O que eu entrego**
- Healthcheck revisado (alerta por WhatsApp **e** e-mail, cobrindo as três automações).
- **Delay aleatório entre envios** (loop com Wait de 20–60s) na A1.1 e A2.1 — higiene anti-banimento para quando o volume crescer.
- Export automático dos fluxos do n8n para o repositório (backup e versionamento sem esforço manual).
- Um **runbook**: o que fazer quando cada coisa quebra, em ordem de checagem — o documento que salva a noite quando um cliente ligar.

**O que vocês fazem:** importar e ajustar (2–3h).

**O que isso destrava:** poder colocar um cliente pagante sem susto, e sustentar por escrito o SLA de 12h/24h da proposta comercial.

**Custo de adiar:** enquanto não há cliente, o risco é teórico. O problema é se um cliente entrar antes disso estar pronto — aí vocês descobrem as falhas na pele dele, que é o pior lugar possível.

---

## Opção D — Kit das conversas de validação

**O que eu entrego**
- Roteiro das **6 perguntas** na ordem certa, com o que escutar em cada resposta (e o que significa quando a pessoa hesita).
- Planilha de registro das conversas, para os números não se perderem.
- Mensagem pronta para marcar a conversa, e como **fechar pedindo indicação** — mesmo quando a pessoa não é cliente potencial.
- Como transformar a conversa em demo no momento em que a pessoa se interessa (a transição mais difícil de fazer no improviso).

**O que vocês fazem:** 5 conversas de 30 min (~3h, mais o agendamento).

**O que isso destrava:** três coisas de uma vez — (1) **os números do hero da landing page** ("sua clínica perde R$ X por mês"), que hoje vocês não têm e sem os quais a copy é ficção; (2) a confirmação (ou o pivô) do nicho **antes** de investir mais 30 dias nele; (3) possivelmente o primeiro lead real, já que agora vocês têm o que mostrar.

**Custo de adiar:** é o risco nº 1 do plano — construir 90 dias no vácuo e descobrir no dia 91 que a dor escolhida não paga. A folga de 30 dias no cronograma é boa notícia, mas se ela virar mais construção em vez de contato com o mercado, ela se transforma em 30 dias de invisibilidade.

---

## Minha recomendação

**A + D em paralelo, dividindo entre vocês dois.** O motivo é que são trabalhos de natureza diferente e não competem por tempo:

| Quem | Etapa | Natureza |
|---|---|---|
| Um dos sócios | **A** — planilhas e gravação | trabalho de teclado, à noite, sozinho |
| O outro | **D** — as 5 conversas | trabalho de telefone, horário comercial/sábado |

Depois: **B** (demo interativa, quando a página estiver perto de ir ao ar) e **C** (robustez, quando aparecer o primeiro "sim" verbal).

E isso responde a uma pergunta que ficou aberta desde a primeira conversa: **quem é o rosto comercial da ModernEasy?** A etapa D é a resposta prática. Quem fizer as conversas agora é quem vai liderar a venda no trimestre 2 — e é melhor descobrir isso agora, com conhecidos, do que no dia 91 na frente de um cliente.

---

## Se vocês quiserem escolher só UMA

Escolham a **D**. É a mais desconfortável, a mais barata em horas e a única que vocês **não conseguem terceirizar para mim** — eu preparo o roteiro, mas a conversa tem que ser de vocês. Todas as outras eu consigo adiantar quase inteiras.
