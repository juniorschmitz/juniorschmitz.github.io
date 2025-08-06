---
layout: post
title: "Property-Based Testing: Testando com Dados Gerados Automaticamente"
date: 2025-08-06 10:50:00 -0300
author: "Junior Schmitz"
tags: [property-based-testing, testes, qualidade]
---
# Property‑Based Testing: Testando com Dados Gerados Automaticamente

A maioria dos testes é baseada em exemplos específicos que imaginamos. Isso limita a criatividade e ignora muitos casos de contorno. **Property‑based testing** inverte a perspectiva: em vez de listar entradas concretas, definimos propriedades que devem ser verdadeiras para qualquer entrada válida e deixamos a ferramenta gerar centenas de casos.

## Por que ir além dos exemplos

Imagine um teste de ordenação que verifica apenas três listas pequenas. Ele pode passar mesmo que a implementação falhe com valores repetidos ou negativos. Bugs reais permanecem ocultos até aparecerem em produção.

## Conceitos básicos

- **Propriedade:** uma regra geral que a função deve obedecer (por exemplo, “ordenar uma lista deve produzir uma lista do mesmo tamanho em ordem crescente”).
- **Gerador:** componente que cria entradas aleatórias dentro de um domínio.
- **Shrinking:** processo de reduzir um caso complexo falho para o menor exemplo reproduzível, facilitando o diagnóstico.

## Exemplo em Python com Hypothesis

```python
from hypothesis import given, strategies as st

@given(st.lists(st.integers()))
def test_sort_preserves_order(lst):
    sorted_lst = sorted(lst)
    assert len(sorted_lst) == len(lst)
    assert sorted_lst == sorted(lst)  # lista está ordenada
```

A biblioteca **Hypothesis** gerará automaticamente listas de inteiros de diversos tamanhos, incluindo casos vazios, repetidos e negativos. Se a implementação falhar em alguma propriedade, a ferramenta exibirá um caso mínimo que reproduz o problema.

## Ferramentas disponíveis

- **Hypothesis** (Python)
- **QuickCheck** (Haskell, Erlang, Elm)
- **FsCheck** (C#)
- **jqwik** (Java)

Cada linguagem possui bibliotecas similares, mas o conceito permanece: testar propriedades ao invés de exemplos.

## Vantagens

- **Cobertura abrangente:** milhares de entradas exercitam caminhos inesperados.
- **Descoberta de edge cases:** encontra casos que você não teria imaginado.
- **Documentação do comportamento:** as propriedades atuam como especificações.

## Quando adotar

Use property‑based testing para algoritmos puros, validação de dados, funções matemáticas e bibliotecas onde invariantes são claras. Para fluxos complexos ou interfaces de usuário, combine com testes baseados em exemplos.

## Conclusão

Property‑based testing é uma poderosa adição ao arsenal de qualidade. Ele complementa testes tradicionais ao verificar comportamentos gerais, ajudando a revelar defeitos sutis. Experimente aplicar essa técnica a partes críticas do seu código e observe a diferença na robustez dos seus testes.
