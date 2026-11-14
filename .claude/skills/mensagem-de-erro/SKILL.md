---
name: mensagem-de-erro
description: Como escrever mensagem de erro neste projeto (diz o que aconteceu e o que fazer, em português, no corpo { erro }). Use sempre que for criar ou alterar uma resposta de erro ou um teste que confere mensagem.
---
# Mensagem de erro no padrão do projeto

## Regras
- O corpo do erro é sempre `{ erro: '<mensagem>' }`. O status segue a convenção do AGENTS.md (400, 404, 409).
- A mensagem tem duas partes: **o que aconteceu** e **o que fazer**. Ex.: `'X aconteceu: faça Y'`.
- Em português, para quem usa a API (professor, secretaria). Sem jargão interno, nome de variável ou stack trace.
- Cite o valor que causou o erro quando ajudar a corrigir (a sala, o campo, o horário). Nunca cite segredo nem dado de outra pessoa.
- Uma mensagem por motivo. Dois erros diferentes não podem ter o mesmo texto, senão o teste não sabe qual regra recusou.
- O teste confere a mensagem com uma palavra-chave estável (`assert.match(r.erro, /passado/i)`), não com o texto inteiro.
- Mudou o texto de uma mensagem? Rode `npm test`: a palavra-chave que o teste confere precisa continuar no texto.

## Modelo
```js
// Ruim: só diz o que aconteceu
return { status: 400, erro: 'Não é possível reservar no passado' };

// Bom: o que aconteceu + o que fazer
return { status: 400, erro: 'Não é possível reservar no passado: escolha um início a partir de agora' };

// Bom, citando o valor que causou o erro
return { status: 404, erro: `Sala não encontrada: ${pedido.sala}. Veja as salas em GET /salas` };
```

## Antes de entregar
1. Leia a mensagem como se fosse quem recebeu o erro: dá para corrigir o pedido sem perguntar a ninguém?
2. Confira que o teste do critério verifica o status **e** a palavra-chave da mensagem (lição L-01).
