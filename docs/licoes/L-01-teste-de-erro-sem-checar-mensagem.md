# L-01 · O agente escreveu um teste de erro que conferia só o status, não o motivo

- **Data e lab:** 13/11, Lab 1 (CA-3 da SPEC-001)
- **Ferramenta e modelo:** Claude Code, Claude Opus 5.5, com o subagente `revisor`

**O que aconteceu (com evidência):** pedimos "Escreva o teste primeiro e me mostre falhando". O agente escreveu
3 testes do CA-3. O primeiro conferia o status e a mensagem; o de fuso conferia só o status:

```js
// test/reservas.test.js, CA-3 (passado)
assert.equal(r.status, 400);
assert.match(r.erro, /passado/i);

// test/reservas.test.js, CA-3 (fuso): faltava a segunda linha
assert.equal(r.status, 400);
```

`criarReserva` devolve 400 em quatro situações: campo faltando, data inválida, fim antes do início e início no
passado. Então o teste de fuso passaria mesmo se a recusa viesse de outra validação, por exemplo se o parse do
`Z` desse errado e caísse em "Datas inválidas". O agente não percebeu. Quem apontou foi o subagente `revisor`:
"o teste de fuso confere só o status, não a mensagem". A correção só entrou no commit `f7f0c62` porque nós
pedimos.

**Esperado:** todo teste que espera um erro confere o status **e** o motivo, como o próprio agente fez no
primeiro teste.

**Por que aconteceu:** teste fraco, e o padrão não está escrito em lugar nenhum. A skill `escrever-teste` diz
"um teste por critério" e "caso de borda junto", mas não diz que o motivo do erro precisa ser conferido. Sem a
regra, o agente aplicou o padrão num teste e esqueceu no seguinte. O `revisor` funcionou como segunda linha de
defesa.

**Sugestão:** adicionar às regras de `.claude/skills/escrever-teste/SKILL.md`:
> Teste que espera erro confere o status **e** a mensagem (`assert.match(r.erro, /…/)`). Vários erros
> compartilham o mesmo status (400), então só o status não prova qual regra recusou.

**Virou regra?** sim, em três lugares:
- `AGENTS.md`, em "Nunca faça" (commit `07237e9`);
- skill `mensagem-de-erro`, em "Antes de entregar" (commit `07237e9`);
- skill `escrever-teste`, em "Regras", como estava na sugestão (commit seguinte, junto com esta atualização).
