---
layout: post
title: "Over‑Engineering vs. Simplicidade: Testando o Google de Duas Formas"
date: 2025-08-06 10:30:00 -0300
author: "Junior Schmitz"
tags: [práticas, automação, design, simplicidade]
---
# Over‑Engineering vs. Simplicidade: Testando o Google de Duas Formas

Como engenheiros de teste, somos tentados a aplicar padrões sofisticados até em cenários triviais. No entanto, nem sempre a complexidade adicional traz benefícios proporcionais. Neste artigo comparo duas abordagens para o mesmo problema: verificar a busca do Google.

## A arquitetura sobrecarregada

Imagine um projeto corporativo completo com múltiplos módulos, fábricas de drivers, estratégias de espera e relatórios customizados. A hierarquia de pacotes se parece com um produto em produção:

```
google-search-test-framework/
├── config/                # configurações de navegador e ambiente
├── core/                  # driver factory, wait strategies, reporting
├── pages/                 # Page Objects e componentes
└── screenplay/            # atores, tarefas e perguntas
```

Testar uma simples busca exige instanciar várias classes e configurar diversos serviços. Embora essa abordagem seja adequada para grandes aplicações, ela pode ser exagerada para um caso tão simples.

## O caminho enxuto

Para fins exploratórios ou scripts pontuais, um teste direto usando apenas a biblioteca de automação (como Selenium ou Playwright) pode ser suficiente:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get("https://www.google.com")
search_box = driver.find_element(By.NAME, "q")
search_box.send_keys("JUnit 5")
search_box.submit()
assert "JUnit 5" in driver.title
driver.quit()
```

Esse código é fácil de entender e não exige infraestrutura pesada. Se algo falhar, você ajusta rapidamente.

## Quando escolher cada um

- Use **arquiteturas robustas** quando houver requisitos de escalabilidade, reuso entre times e integração com pipelines.
- Prefira **soluções simples** para prototipagem, POCs ou testes isolados sem previsão de crescimento.

A complexidade deve ser proporcional ao problema. A melhor prática é começar simples e evoluir conforme a necessidade, evitando o excesso de engenharia em nome da sofisticação.

## Conclusão

O equilíbrio entre over‑engineering e simplicidade é delicado. Entender o contexto e o objetivo do teste é essencial para tomar a decisão certa. Ao ponderar custo de manutenção, tempo de desenvolvimento e valor gerado, você cria soluções elegantes e eficazes.
