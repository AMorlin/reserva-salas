# reserva-salas · instruções para agentes de IA

Este arquivo é o contexto do projeto para qualquer agente (Claude Code, Copilot, Gemini CLI, Codex, Aider…).
Mantenha-o curto, verdadeiro e atualizado: contexto velho ensina o agente a errar.

## O que é
API HTTP para reservar salas da faculdade e acabar com o conflito de horário.
Especificações em `docs/specs/`. **Nenhum código sem SPEC; nenhum critério sem teste.**

## Stack e comandos
- Node.js 20+, JavaScript com ES modules. **Sem dependências externas.**
- Testes: `npm test` (usa `node:test`, nativo do Node)
- Rodar: `npm start` e abrir http://localhost:3000/salas
- Procurar segredo nos arquivos: `npm run segredos`

## Estrutura
- `src/reservas.js`: regras de negócio, funções puras (recebem `agora` como parâmetro)
- `src/app.js`: camada HTTP, sem regra de negócio
- `src/salas.js`: dados das salas
- `test/`: um arquivo de teste por módulo; cada teste cita o critério (`CA-1: …`)
- `docs/specs/`: SPECs; `docs/licoes/`: lições aprendidas; `diario/`: diário de bordo do grupo

## Convenções
**Status HTTP**
- `200` leitura ou alteração que deu certo · `201` reserva criada
- `400` o pedido sozinho já está errado: campo faltando, data inválida, fim antes do início, horário no passado ou fora do prazo
- `404` o que foi pedido não existe: sala, reserva ou rota
- `409` o pedido estaria certo, mas choca com o estado atual: horário ocupado, reserva já cancelada
- Na dúvida entre 400 e 409: se o mesmo pedido daria certo num sistema vazio, é 409.

**Corpo de erro:** sempre `{ "erro": "<mensagem em português>" }`. A mensagem diz o que aconteceu e o que fazer (skill `mensagem-de-erro`).

**Ordem das validações:** campos → existe (404) → regras do próprio pedido (400) → conflito com o estado (409). O primeiro erro encontrado é o que volta.

## Como trabalhar
1. Leia a SPEC da tarefa. Se algo estiver ambíguo, pergunte antes de codar.
2. Proponha o plano antes de alterar arquivos.
3. Escreva o teste do critério primeiro e mostre que ele **falha**.
4. Implemente o mínimo para passar. Rode `npm test` inteiro.
5. Mostre o diff e explique as decisões em português.
6. Quando um erro do agente for apontado, proponha uma lição em `docs/licoes/` (modelo `_modelo-licao.md`).

## Nunca faça
- Alterar ou apagar um teste para ele passar. Se o teste parece errado, pare e pergunte.
- Instalar dependência sem pedir.
- Gravar segredo, senha ou chave em arquivo. Use variável de ambiente.
- Mexer em `.github/` ou `.claude/settings.json` sem pedir.
- Implementar o que está em "Fora de escopo" na SPEC.
- Escrever teste de erro que confere só o status: confira o status **e** a mensagem. (lição L-01)
- Alterar arquivos com pergunta do plano em aberto: "pode seguir" não responde perguntas; repita-as. (lição L-02)
