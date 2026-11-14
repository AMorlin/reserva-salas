# SPEC-002 · Cancelar reserva

- **Situação:** em revisão
- **Dono:** coordenação do curso

## Problema
Professores não possuem opção de cancelar uma sala reservada. A sala fica ocupada no sistema, mas livre fisicamente.

## Quem usa
Professores cancelam a reserva, secretaria visualiza a atualizacao das salas com o cancelamento e abertura de novo horário na sala

## Critérios de aceite
Um critério por comportamento. Sempre no formato Dado / Quando / Então, com o resultado observável.
- **CA-1 · Dado uma reserva do lab-1 amanhã das 14h às 16h quando o cancelamento dessa reserva é solicitado, então recebe 200 e a reserva aparece como cancelada em GET /reservas.  Horário encostado (16h às 18h) não é conflito. Outra sala no mesmo horário não é conflito.
- **CA-2** · Dado uma reserva do lab-1 das 14h às 16h que foi cancelada, quando outra pessoa reserva o lab-1 das 14h às 16h, então a reserva é criada (201).
- **CA-3** · Dado agora = 08h00 e uma reserva que começa às 08h30, quando o cancelamento é efetuado, então recebe 400 com uma mensagem que diz o motivo. Uma reserva que começa às 08h31 é cancelada.

## Fora de escopo
  - aviso ou notificação à secretaria;
  - cancelamento pela secretaria ou por um admin;
  - alterar horário em vez de cancelar;
  - cancelar uma série recorrente;
  - motivo do cancelamento;
  - desfazer o cancelamento.

## Riscos e perguntas
Reservas passadas não devem ser canceladas.
Login ainda fora do escopo.
Fuso horário: todas as datas em ISO 8601 com fuso GMT -03:00.

---
Conferência antes de aprovar (ou rode `/revisar-spec` no Claude Code):
- [ ] Todo critério vira um teste automático sem interpretação
- [ ] Há pelo menos um caso de erro e um caso de borda
- [ ] "Fora de escopo" tem pelo menos um item
- [ ] Nenhuma pergunta aberta bloqueia o primeiro critério
