# Test Strategy

## 1. Objetivo

Este projeto tem como objetivo demonstrar uma abordagem prática de automação de testes de API utilizando Postman, JavaScript e Newman.

A suíte foi construída sobre a API pública DummyJSON e contempla cenários funcionais, positivos, negativos, autenticação, manipulação de dados, validação de respostas e execução automatizada.

Além da validação técnica dos endpoints, o projeto busca demonstrar boas práticas de organização, reutilização de variáveis, documentação e preparação para execução em pipeline CI/CD.

---

## 2. Escopo

A automação cobre três áreas principais da API:

### Products
- Listagem de produtos.
- Busca por ID.
- Busca por termo.
- Criação.
- Atualização.
- Exclusão.
- Consulta de recurso inexistente.
- Criação sem campo de título.
- Atualização de produto inexistente.

### Authentication
- Login com credenciais válidas.
- Login com credenciais inválidas.
- Login sem senha.
- Validação de usuário autenticado.
- Acesso com token inválido.

### Users
- Listagem de usuários.
- Busca por ID.
- Busca por termo.
- Criação.
- Atualização.
- Exclusão.
- Consulta de usuário inexistente.
- Criação com e-mail inválido.
- Busca sem resultados.

---

## 3. Fora de escopo

Não fazem parte do objetivo atual deste projeto:

- Testes de carga e performance em larga escala.
- Testes de segurança aprofundados.
- Testes de concorrência.
- Testes de banco de dados.
- Testes de interface gráfica.
- Validação de persistência real dos dados.
- Testes de infraestrutura da API.
- Testes de disponibilidade contínua.

A DummyJSON simula várias operações de criação, atualização e exclusão. Por isso, alguns recursos retornados não são realmente persistidos no servidor.

---

## 4. Ferramentas utilizadas

### Postman
Utilizado para criação, organização e execução das requisições.

### JavaScript
Utilizado nos scripts de Before Request e After Response para:

- geração de dados dinâmicos;
- manipulação de variáveis;
- validação de status HTTP;
- validação de estruturas JSON;
- validação de dados;
- validação de regras de comportamento;
- armazenamento de tokens.

### Newman
Utilizado para execução automatizada da collection via linha de comando.

### newman-reporter-htmlextra
Utilizado para geração de relatórios HTML com resultados de execução.

### Git
Utilizado para versionamento do projeto.

### GitHub
Utilizado para hospedagem do repositório e futura integração com pipeline CI/CD.

---

## 5. Estrutura da suíte

Os cenários estão organizados em quatro grupos:

- Products
- Users
- Auth
- Negative Scenarios

Cada cenário possui:

- método HTTP;
- endpoint;
- dados de entrada quando aplicável;
- validações automatizadas;
- critérios de sucesso;
- comportamento esperado ou observado.

A documentação detalhada dos cenários está disponível em:

`docs/test-cases.md`

---

## 6. Ambientes e variáveis

O projeto utiliza um environment denominado:

`DummyJSON - Dev`

Principais variáveis:

| Variável | Finalidade |
|---|---|
| `baseUrl` | URL base da API |
| `productId` | ID válido de produto |
| `invalidProductId` | ID de produto inexistente |
| `searchTerm` | Termo utilizado em busca de produtos |
| `userId` | ID válido de usuário |
| `invalidUserId` | ID de usuário inexistente |
| `userSearchTerm` | Termo utilizado em busca de usuários |
| `dynamicTitle` | Título gerado dinamicamente |
| `dynamicPrice` | Preço utilizado em criação de produto |
| `dynamicCategory` | Categoria utilizada em criação de produto |
| `dynamicDescription` | Descrição utilizada em criação de produto |
| `dynamicUserEmail` | E-mail dinâmico utilizado em criação de usuário |
| `dynamicUsername` | Username dinâmico utilizado em criação de usuário |
| `accessToken` | Token de acesso gerado no login |
| `refreshToken` | Token de renovação gerado no login |

Tokens sensíveis não devem ser versionados com valores reais no repositório.

---

## 7. Estratégia de dados

A suíte utiliza dados estáticos e dinâmicos.

### Dados estáticos
Utilizados em cenários onde previsibilidade é importante, como:

- IDs conhecidos.
- credenciais de teste.
- valores fixos utilizados em atualização.

### Dados dinâmicos
Utilizados principalmente em cenários de criação.

Exemplos:

- título de produto gerado com timestamp;
- e-mail de usuário gerado dinamicamente;
- username de usuário gerado dinamicamente.

Essa abordagem reduz repetição de dados e melhora a reutilização da suíte.

---

## 8. Tipos de validação

A suíte contempla diferentes níveis de validação.

### Status HTTP
Exemplos:

- 200 OK
- 201 Created
- 400 Bad Request
- 401 Unauthorized
- 404 Not Found

### Headers
Validação de:

`Content-Type: application/json`

### Estrutura da resposta
Validação de:

- propriedades obrigatórias;
- arrays;
- objetos;
- tipos de dados.

### Conteúdo
Validação de:

- IDs;
- nomes;
- preços;
- e-mails;
- usernames;
- mensagens de erro.

### Regras funcionais
Exemplos:

- produto consultado deve corresponder ao ID solicitado;
- login inválido não deve retornar token;
- recurso excluído deve apresentar `isDeleted: true`;
- busca sem resultado deve retornar lista vazia;
- mensagem de erro deve referenciar o ID consultado.

### Tempo de resposta
Os cenários utilizam limites de tempo para identificar respostas excessivamente lentas.

A maioria utiliza:

`< 2000 ms`

Alguns cenários possuem limites ligeiramente maiores quando necessário para evitar instabilidade em uma API pública.

---

## 9. Cenários negativos

A estratégia inclui cenários negativos para avaliar como a API reage a entradas inválidas ou recursos inexistentes.

Exemplos:

- produto inexistente;
- usuário inexistente;
- credenciais inválidas;
- senha ausente;
- token inválido;
- produto sem título;
- usuário com e-mail inválido;
- busca sem resultados.

Nem todos os cenários negativos retornam erro.

Durante os testes foi observado que alguns endpoints da DummyJSON aceitam dados que normalmente poderiam exigir validação adicional.

Esses comportamentos são documentados como características observadas da API simulada.

---

## 10. Critérios de sucesso

Um cenário é considerado aprovado quando:

- o status HTTP corresponde ao comportamento esperado;
- a estrutura da resposta está correta;
- os campos necessários estão presentes;
- os valores retornados correspondem aos dados enviados ou esperados;
- mensagens de erro são coerentes;
- regras funcionais são atendidas;
- o tempo de resposta permanece dentro do limite definido.

---

## 11. Critérios de falha

Um cenário é considerado reprovado quando:

- o status HTTP diverge do esperado;
- a resposta não possui estrutura JSON esperada;
- campos essenciais estão ausentes;
- os valores retornados divergem dos valores esperados;
- tokens são retornados em cenários de autenticação inválida;
- mensagens de erro não correspondem ao comportamento esperado;
- o tempo de resposta ultrapassa o limite definido.

---

## 12. Limitações conhecidas

A DummyJSON é uma API simulada utilizada para desenvolvimento e testes.

Algumas limitações relevantes:

- operações de POST, PUT e DELETE podem ser simuladas;
- recursos criados podem não ser persistidos;
- validações de negócio são simplificadas;
- alguns campos inválidos podem ser aceitos;
- comportamento pode diferir de uma API de produção real;
- tempos de resposta podem variar por depender de serviço externo.

Essas limitações são consideradas na interpretação dos resultados.

---

## 13. Execução automatizada

A suíte será executada via Newman utilizando os scripts definidos no `package.json`.

Execução padrão:

```bash
npm run test:api