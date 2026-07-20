#  OrangeHRM-QA

Repositório de QA para validação do OrangeHRM. Apresenta plano de testes detalhado, mapeamento de vulnerabilidades de UI/Data Validation e cobertura automatizada de regressão usando Cypress.

Este projeto de automação de testes de ponta a ponta (E2E) foi desenvolvido para demonstrar a aplicação prática de engenharia de qualidade, integrando o planejamento analítico de cenários à execução automatizada para garantir a resiliência dos fluxos críticos do sistema.

---

##  Tecnologias e Ferramentas Utilizadas

*   **Cypress** (Framework principal de automação de testes E2E)
*   **Jira** (Gerenciamento do ciclo de vida dos testes, mapeamento de histórias de usuário e reporte de bugs)
*   **Node.js** (Ambiente de execução e gerenciamento de pacotes via npm)
*   **JavaScript** (Linguagem utilizada na escrita dos scripts de teste)


## Requisitos
- Node.js 16+ (recomendado)
- npm ou yarn
- Versão do Cypress usada: a que consta em `package.json` (devDependency)

---

## Instalação

Instale dependências:

```bash
npm install
```

---

## Scripts úteis

- Abrir o Test Runner interativo:

```bash
npm run cy:open
```

- Executar todos os testes em modo headless:

```bash
npx cypress run
```

- Executar um spec específico:

```bash
npx cypress run --spec "cypress/e2e/my_info.cy.js"
```

---

## Estrutura do repositório

- `cypress/e2e/` — arquivos de teste (specs)
- `cypress/fixtures/` — dados de teste (JSON)
- `cypress/support/commands.js` — comandos reutilizáveis (ex.: `cy.login()`)
- `cypress/support/e2e.js` — importações globais e hooks
- `cypress.config.js` — configuração do Cypress

Links rápidos:
- [cypress/support/commands.js](cypress/support/commands.js)
- [cypress/support/e2e.js](cypress/support/e2e.js)
- [cypress/e2e/my_info.cy.js](cypress/e2e/my_info.cy.js)

---

## Uso dos comandos de suporte

O projeto já inclui comandos utilitários em [cypress/support/commands.js](cypress/support/commands.js):

- `cy.login([username], [password])` — realiza login com credenciais (padrão `Admin` / `admin123`).
- `cy.removeDuplicateByName(name)` — (opcional) remove elementos duplicados no DOM com o mesmo `name` (usar com cuidado).

Recomendações:
- Prefira criar seletores específicos (atributo `data-cy`) para evitar ambiguidade.
- Quando `cy.get()` retornar múltiplos elementos use `filter(':visible')`, `eq()` ou `first()` para atingir um único elemento.

Exemplo:

```js
cy.get('[data-cy=firstName]').clear().type('João');
// ou quando não houver data-cy
cy.get('[name="firstName"]').filter(':visible').first().clear().type('João');
```

---

## Erros comuns e soluções

- `cy.type() can only be called on a single element` → significa que o seletor retornou vários elementos. Soluções:
    - usar `filter(':visible').first()`
    - usar `eq(index)` para um elemento específico
    - criar `data-cy` único para o campo
    - (workaround) `cy.removeDuplicateByName('firstName')` — apenas para debugging/local

- Falhas intermitentes por tempo de carregamento: use assertions de espera como `cy.findBy...` ou `cy.get(...).should('be.visible')` antes de digitar.

---

## Como adicionar um novo teste

1. Criar um arquivo em `cypress/e2e/` terminando com `.cy.js`.
2. Reutilizar `cy.login()` quando necessário.
3. Preferir seletores `data-cy` e centralizar seletores em `cypress/support` ou `cypress/selectors.js` (ainda não criado).

Exemplo mínimo:

```js
describe('Meu fluxo', () => {
    it('faz login e navega', () => {
        cy.login();
        cy.contains('My Info').click();
    });
});
```

---

## Contribuições

Abra issues para problemas, e crie PRs para melhorias de testes, novos comandos e correções de seletores.

---

## Contato

Qualquer dúvida, me marca no código ou abre uma issue explicando o cenário e o erro reproduzível.
