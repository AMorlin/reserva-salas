# L-02 · O agente fez perguntas no plano e seguiu sem esperar a resposta

- **Data e lab:** 13/11, Lab 1 (CA-3 da SPEC-001)
- **Ferramenta e modelo:** Claude Code, Claude Opus 5.5

**O que aconteceu (com evidência):** no plano de teste do CA-3, o agente terminou com "Decisões para você
confirmar" e listou 3 perguntas:
1. a mensagem de erro é conferida com `/passado/i` ou só o status?
2. ordem das validações: um pedido com sala inexistente **e** no passado recebe 404 ou 400?
3. mantém ou corta o teste de fuso?

Nosso prompt seguinte foi "Escreva o teste primeiro e me mostre falhando. Ainda não implemente.", sem responder
nenhuma. O agente seguiu com as próprias sugestões e só avisou no fim: "Você não respondeu às 3 decisões do
plano, então segui o que eu tinha proposto".

**Esperado:** o AGENTS.md diz "Se algo estiver ambíguo, pergunte antes de codar". Se o agente fez a pergunta, a
resposta faz parte do pré-requisito. Ele deveria ter parado e repetido as perguntas antes de escrever o teste,
ou ao menos avisado **antes** de começar.

**Por que aconteceu:** duas causas.
- O agente tratou um pedido de ação ("escreva o teste") como permissão para as decisões em aberto.
- Do nosso lado, demos o comando sem responder às perguntas.

O dano foi pequeno: as escolhas eram razoáveis, foram avisadas e tinham conserto fácil. Mas, numa decisão de
regra de negócio (por exemplo, a ordem 400 × 404), a escolha teria ficado com o agente e não com o dono da SPEC.

**Sugestão:**
- No AGENTS.md, em "Como trabalhar", passo 2: "Se o plano tiver perguntas em aberto, não altere arquivos até
  todas estarem respondidas. Um 'pode seguir' não responde perguntas: repita-as."
- No fluxo do grupo: responder às perguntas do plano antes de dar o próximo comando.

**Virou regra?** não (proposta acima, aguardando decisão do grupo)
