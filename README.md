# API Testing Automation

Projeto de automação de testes de API desenvolvido com **Postman, JavaScript e Newman**, utilizando a API pública **DummyJSON**.

O objetivo é demonstrar uma abordagem prática de Quality Assurance aplicada a APIs REST, cobrindo cenários funcionais, CRUD, autenticação, validação de tokens, dados dinâmicos, cenários negativos, documentação e execução automatizada em CI/CD.

---

## Status do projeto

![API Tests](https://github.com/eumesmooliveira/api-testing-automation/actions/workflows/api-tests.yml/badge.svg)

### Última execução validada

| Métrica | Resultado |
|---|---:|
| Requests | 23 |
| Assertions | 133 |
| Falhas | 0 |
| Test Scripts | 23 |
| Pre-request Scripts | 2 |
| Execução local | 7.4s |
| Tempo médio de resposta | 293ms |

A suíte também é executada automaticamente através do **GitHub Actions**.

---

## Tecnologias

- Postman
- JavaScript
- Newman
- Newman Reporter HTML Extra
- Node.js
- npm
- Git
- GitHub
- GitHub Actions
- REST API
- JSON

---

## Cobertura da suíte

A automação possui **23 cenários**, organizados por domínio.

### Products

- CT01 - Listar produtos
- CT02 - Buscar produto por ID
- CT03 - Buscar produto inexistente
- CT04 - Buscar produtos por termo
- CT05 - Criar produto
- CT06 - Atualizar produto
- CT07 - Excluir produto

### Authentication

- CT08 - Login válido
- CT09 - Login inválido
- CT10 - Login sem senha
- CT11 - Validar usuário autenticado
- CT12 - Token inválido

### Users

- CT13 - Listar usuários
- CT14 - Buscar usuário por ID
- CT15 - Buscar usuário inexistente
- CT16 - Buscar usuários por termo
- CT17 - Criar usuário
- CT18 - Atualizar usuário
- CT19 - Excluir usuário

### Negative Scenarios

- CT20 - Criar produto sem título
- CT21 - Atualizar produto inexistente
- CT22 - Criar usuário com e-mail inválido
- CT23 - Buscar usuários sem resultado

---

## O que é validado

A suíte contempla validações de:

- status HTTP;
- Content-Type;
- estrutura JSON;
- tipos de dados;
- campos essenciais;
- IDs retornados;
- mensagens de erro;
- autenticação;
- access token e refresh token;
- comportamento de endpoints protegidos;
- criação, atualização e exclusão;
- pesquisa e filtros;
- cenários negativos;
- tempo de resposta;
- consistência entre dados enviados e retornados.

---

## Dados dinâmicos

Alguns cenários utilizam geração dinâmica de dados através de **Pre-request Scripts**.

Exemplos:

- título de produto com timestamp;
- e-mail de usuário dinâmico;
- username de usuário dinâmico.

Exemplo:

```javascript
const timestamp = Date.now();

pm.environment.set(
    "dynamicUserEmail",
    `felipe.qa.${timestamp}@example.com`
);

pm.environment.set(
    "dynamicUsername",
    `felipeqa${timestamp}`
);