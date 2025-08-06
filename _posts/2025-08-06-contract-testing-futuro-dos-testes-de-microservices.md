---
layout: post
title: "Contract Testing: O Futuro dos Testes de Microservices"
date: 2025-08-06 10:10:00 -0300
author: "Junior Schmitz"
tags: [microservices, testes, contract-testing, qualidade]
---
# Contract Testing: O Futuro dos Testes de Microservices

## Por que contratos importam

Em ambientes distribuídos, cada serviço evolui de forma independente. Testes end‑to‑end tradicionais verificam fluxos completos, mas são lentos e frágeis. Quando uma equipe altera seu serviço, todas as demais ficam bloqueadas aguardando integrações.

O *contract testing* resolve esse gargalo ao definir expectativas claras entre consumidores e provedores. Em vez de rodar toda a stack, verificamos se o serviço cumpre os contratos que declarou.

## Conceitos principais

Um contrato descreve como um serviço deve se comportar: que solicitações ele aceita e quais respostas retorna. Ferramentas como Pact permitem que consumidores gerem contratos no formato de testes. O provedor então verifica se atende a esses contratos.

### Fluxo típico

1. **Consumidor define o contrato**: ao escrever um teste, descreve a requisição esperada e a resposta desejada.
2. **Contrato é publicado**: o contrato é enviado para um broker (ex.: PactFlow) que gerencia versões.
3. **Provedor valida o contrato**: o serviço roda testes contra todos os contratos pendentes no broker.
4. **Deployment é liberado**: somente versões compatíveis passam para produção.

## Exemplo em JavaScript com Pact

```javascript
const { Pact } = require('@pact-foundation/pact');
const provider = new Pact({
  consumer: 'Frontend',
  provider: 'UserService',
});

describe('Contrato de usuário', () => {
  before(() => provider.setup());
  after(() => provider.finalize());

  it('deve recuperar dados de um usuário', async () => {
    await provider.addInteraction({
      state: ' usuário existe',
      uponReceiving: 'uma requisição GET em /users/1',
      withRequest: {
        method: 'GET',
        path: '/users/1',
        headers: { Accept: 'application/json' },
      },
      willRespondWith: {
        status: 200,
        headers: { 'Content-Type': 'application/json' },
        body: { id: 1, name: 'Maria' },
      },
    });

    // chamada ao cliente real aqui...

    await provider.verify();
  });
});
```

No pipeline de integração contínua, os consumidores geram contratos e publicam no broker. Os provedores, por sua vez, buscam esses contratos e executam a validação antes de liberar uma nova versão.

## Benefícios

- **Feedback rápido**: testes isolados rodam em segundos.
- **Independência entre equipes**: mudanças são detectadas pelo contrato, não em grandes suites end‑to‑end.
- **Comunicação clara**: contratos atuam como documentação viva.

## Considerações finais

Contract testing não elimina a necessidade de alguns testes de integração; ele complementa sua estratégia. Ao garantir que cada serviço honre seus compromissos, você reduz regressões e acelera o desenvolvimento. Em arquiteturas orientadas a microservices, esse padrão se torna indispensável.
