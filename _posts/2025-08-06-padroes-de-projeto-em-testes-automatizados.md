---
layout: post
title: "Padrões de Projeto em Testes Automatizados: Do Page Object ao Screenplay e Além"
date: 2025-08-06 10:00:00 -0300
author: "Junior Schmitz"
tags: [automação, testes, padrões-de-projeto, qualidade]
---
# Padrões de Projeto em Testes Automatizados

Introdução
Em projetos de automação de testes, a clareza e a manutenibilidade do código são tão importantes quanto a cobertura. Um bom desenho arquitetural permite que a base de testes evolua com o sistema sem acumular dívidas técnicas. Neste artigo exploro alguns padrões de projeto que uso no dia a dia para estruturar testes em Web, API e mobile.

## Page Object Model (POM)

O Page Object Model encapsula o comportamento de uma página ou componente em uma classe. Em vez de espalhar seletores e interações por vários testes, centralizamos essas operações em um único lugar.

### Exemplo de uso em Java

```java
public class LoginPage {
    private WebDriver driver;
    @FindBy(id = "username")
    private WebElement usernameField;
    @FindBy(id = "password")
    private WebElement passwordField;
    @FindBy(xpath = "//button[@type='submit']")
    private WebElement loginButton;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    public LoginPage enterUsername(String username) {
        usernameField.sendKeys(username);
        return this;
    }

    public LoginPage enterPassword(String password) {
        passwordField.sendKeys(password);
        return this;
    }

    public HomePage clickLogin() {
        loginButton.click();
        return new HomePage(driver);
    }
}
```

Ao isolar a lógica de interação, ganhamos reutilização e clareza. Mudanças no layout exigem atualização apenas na classe da página.

## Screenplay

O padrão Screenplay organiza a interação em torno de *atores* que executam *tarefas* e respondem a *perguntas*. Em vez de chamar diretamente métodos de páginas, descrevemos o comportamento em termos de intenções de um usuário.

```java
Actor user = Actor.named("Usuário").whoCan(BrowseTheWeb.with(driver));
user.attemptsTo(
    NavigateTo.url("https://exemplo.com"),
    EnterText.into(LoginPage.USERNAME, "teste"),
    EnterText.into(LoginPage.PASSWORD, "segredo"),
    Click.on(LoginPage.LOGIN_BUTTON)
);
```

O código acima dá foco nas ações, facilitando a leitura por pessoas não técnicas. A Screenplay cresce bem em suites grandes.

## Construtores e Builders

Para preparar dados de teste legíveis, uso o padrão *Builder*. Ele permite construir objetos complexos de forma fluente.

```java
User user = UserBuilder.create()
    .withName("Test User")
    .withEmail("test@example.com")
    .withPassword("Segredo123")
    .build();
```

Esse padrão evita a criação de construtores gigantes e melhora a clareza dos testes.

## Fábricas e Estratégias para Ambientes

Em cenários com múltiplos navegadores ou configurações, aplico os padrões *Factory* e *Strategy* para encapsular as diferenças. Uma fábrica de drivers centraliza a criação de instâncias e uma estratégia define como cada ambiente se comporta.

```java
public interface EnvironmentStrategy {
    WebDriver createDriver();
    String getBaseUrl();
}

public class ProductionEnvironment implements EnvironmentStrategy {
    public WebDriver createDriver() {
        return new ChromeDriver();
    }
    public String getBaseUrl() {
        return "https://app.empresa.com";
    }
}
```

## Fluent Interface e DSL

Quando os testes precisam contar uma história, uma *domain specific language* (DSL) pode torná-los expressivos. Em vez de chamadas de baixo nível, criamos métodos que espelham a linguagem do domínio.

```java
pedido
    .paraCliente("Maria")
    .comProduto("Plano Premium")
    .finalizar();
```

## Conclusão

Os padrões de projeto não são uma receita única. Escolha aquele que melhor se ajusta ao contexto e mantenha o foco na legibilidade. Em times grandes, a padronização ajuda a crescer uma base de testes compartilhada. Em casos simples, um design enxuto evita complexidade desnecessária. O equilíbrio entre organização e simplicidade é a chave para uma automação eficiente.
