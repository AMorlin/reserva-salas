# Diário de bordo do grupo

Uma entrada por aula. Vale 20% da nota e é avaliado pela **honestidade**, não pela ausência de erro.
Quem não registra o erro, repete o erro.

- **Grupo:** <nomes>
- **Tema do projeto:** <reserva-salas ou o tema do grupo>
- **Ferramenta(s):** <Claude Code, Copilot, Gemini CLI…>

---

## Entrada N · Aula N · <data>

**O que pedimos ao agente (os 3 prompts mais importantes, copiados):**
1. "Leia o @AGENTS.md e a SPEC-001. O que falta? Não altere nada"
2. "Escreva o teste primeiro e me mostre falhando. Ainda não implemente."
3. "Agora implemente o mínimo para passar. Rode o npm teste."

**3 acertos do agente:**
- **Seguiu o "teste antes do código":** escreveu 3 testes do CA-3 (passado, borda `inicio == agora` e fuso) e mostrou 2 falhando (`201 !== 400`) antes de mexer em `src/reservas.js`. Avisou por conta própria que o teste de borda já passava antes do código e explicou o motivo: ele protege contra quem escrever `<=` em vez de `<`.
- **Implementação mínima, sem passar do escopo:** uma linha (`if (inicio < agora) return { status: 400, … }`), usando o `agora` injetado e seguindo a decisão da SPEC (`inicio < agora`). O TODO do CA-2 ficou intacto e nenhum teste antigo mudou. Resultado: 13/13 no `npm test`.
- **Apontou ambiguidades da SPEC antes de codar:** o texto da mensagem do 409, a ordem das validações (passado + sala inexistente = 400 ou 404?), data sem fuso aceita pelo código. Também chamou o `revisor` depois de implementar, como manda o CLAUDE.md, sem a gente pedir.

**Erros do agente (pelo menos 1, com evidência):**
- **Teste incompleto:** no 1º teste do CA-3 ele verificou status + mensagem (`assert.match(r.erro, /passado/i)`); no teste de fuso, só o status. Um 400 vindo de outra validação passaria despercebido. Quem pegou foi o subagente `revisor`, e a correção só entrou quando pedimos. → virou a lição `docs/licoes/L-01-teste-de-erro-sem-checar-mensagem.md`
- **Seguiu sem a nossa resposta:** propôs 3 decisões para confirmarmos (mensagem solta, manter o teste de fuso, ordem das validações). Quando pedimos "escreva o teste", ele aplicou as próprias sugestões sem a gente ter respondido. Avisou no final, mas o AGENTS.md diz "se algo estiver ambíguo, pergunte antes de codar". → virou a lição `docs/licoes/L-02-seguiu-sem-resposta-das-perguntas.md`

**O que nós decidimos (e o agente não podia decidir):**
- Seguir sem a revisão por IA no CI (sem `ANTHROPIC_API_KEY`): o job `revisar` aparece verde, mas foi pulado. A revisão foi feita pelo subagente `revisor` local, que aprovou o diff duas vezes.
- Aplicar ou não as sugestões do revisor (atualizar a "Situação" da SPEC, reforçar o teste de fuso).
- Fazer o commit (o agente criou o branch `feat/ca-3-reserva-no-passado` em vez de commitar na `main`).

**Custo e tempo:** US$ 1,39 (`/cost` no Claude Code) · ~45 min de sessão (2m52s de API) · 24 linhas adicionadas, 3 removidas

**O que faríamos diferente:**
- …
