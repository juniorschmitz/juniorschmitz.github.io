---
layout: post
title: "Mutation Testing: Garantindo a Qualidade dos Seus Testes"
date: 2025-08-06 10:40:00 -0300
author: "Junior Schmitz"
tags: [mutation-testing, qualidade, testes]
---
# Mutation Testing: Garantindo a Qualidade dos Seus Testes

Cobertura de código é uma métrica útil, mas enganosa. Ter 90 % de linhas executadas não significa que seus testes capturam 90 % dos defeitos. **Mutation testing** vem para medir a eficácia real dos testes ao introduzir alterações artificiais no código e observar se a suite detecta os erros.

## Como funciona

1. **Geração de mutantes:** pequenas mudanças são aplicadas ao código, como inverter uma condição ou alterar um operador.
2. **Execução da suite:** rodamos todos os testes contra cada mutante.
3. **Avaliação:** se um teste falha, o mutante é considerado “morto”; caso contrário, ele “sobrevive” e indica que a suite não detectou o erro.
4. **Score de mutação:** a razão entre mutantes mortos e o total expressa a qualidade da suite. Um score baixo sugere testes fracos.

## Exemplo simples em Java

```java
public class Calculator {
    public int divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Divisão por zero");
        }
        return a / b;
    }
}

@Test
public void testDivide() {
    Calculator calc = new Calculator();
    int result = calc.divide(10, 2);
    assertEquals(5, result);
}
```

Esse teste obtém 100 % de cobertura, mas um mutante que altera `return a / b` para `return a * b` sobreviveria, pois o resultado ainda seria 5. Mutation testing revelaria a fragilidade do teste e incentivaria a verificação de outros cenários.

## Ferramentas

Para Java, ferramentas como **PIT** automatizam o processo de geração e execução de mutantes. Em Python, bibliotecas como **mutmut** e **cosmic-ray** oferecem funcionalidades semelhantes. Integre essas ferramentas ao pipeline para receber feedback contínuo.

## Benefícios

- **Mensuração real de qualidade:** vai além da cobertura, avaliando a habilidade dos testes em detectar bugs.
- **Identificação de lacunas:** aponta áreas onde faltam asserts ou cenários.
- **Melhoria contínua:** incentiva a escrita de testes mais abrangentes.

## Conclusão

Mutation testing acrescenta um nível extra de garantia à sua base de testes. Embora seja mais demorado que métricas tradicionais, ele oferece insights valiosos sobre a eficácia da sua suite. Ao aplicar essa técnica, você fortalece a confiança no software que entrega.
