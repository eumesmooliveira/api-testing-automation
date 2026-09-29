# Test Cases

Este documento descreve os principais cenários automatizados da suíte de testes de API desenvolvida com Postman, JavaScript e Newman utilizando a DummyJSON.

A suíte contempla cenários funcionais, positivos e negativos relacionados a produtos, usuários e autenticação.

---

## Products

### CT01 - Listar produtos

**Método:** GET  
**Endpoint:** `/products`

**Objetivo:**  
Validar que a API retorna corretamente a lista de produtos.

**Validações:**
- Status code 200.
- Content-Type JSON.
- Existência da propriedade `products`.
- `products` deve ser um array.
- A lista deve possuir pelo menos um produto.
- Produtos devem conter `id`, `title`, `price` e `category`.
- Tempo de resposta abaixo do limite definido.

---

### CT02 - Buscar produto por ID

**Método:** GET  
**Endpoint:** `/products/{{productId}}`

**Objetivo:**  
Validar a consulta de um produto existente utilizando seu identificador.

**Validações:**
- Status code 200.
- Content-Type JSON.
- ID retornado deve ser igual ao `productId` enviado.
- Produto deve conter campos essenciais.
- Preço deve ser maior que zero.
- Tempo de resposta dentro do limite.

---

### CT03 - Buscar produto inexistente

**Método:** GET  
**Endpoint:** `/products/{{invalidProductId}}`

**Objetivo:**  
Validar o comportamento da API ao consultar um produto inexistente.

**Validações:**
- Status code 404.
- Content-Type JSON.
- Existência da propriedade `message`.
- Mensagem deve indicar produto não encontrado.
- Mensagem deve conter o ID consultado.
- Tempo de resposta dentro do limite.

---

### CT04 - Buscar produtos por termo

**Método:** GET  
**Endpoint:** `/products/search?q={{searchTerm}}`

**Objetivo:**  
Validar a funcionalidade de busca de produtos.

**Validações:**
- Status code 200.
- Content-Type JSON.
- Existência da lista `products`.
- Busca deve retornar pelo menos um produto.
- Produtos devem conter campos essenciais.
- Pelo menos um resultado deve estar relacionado ao termo pesquisado.
- Tempo de resposta dentro do limite.

---

### CT05 - Criar produto

**Método:** POST  
**Endpoint:** `/products/add`

**Objetivo:**  
Validar a criação simulada de um produto utilizando dados dinâmicos.

**Características:**
- Título gerado dinamicamente no Before Request.
- Dados armazenados em variáveis de ambiente.

**Validações:**
- Status code 201.
- Content-Type JSON.
- Produto criado deve possuir ID.
- Título retornado deve corresponder ao valor enviado.
- Descrição retornada deve corresponder ao valor enviado.
- Preço retornado deve corresponder ao valor enviado.
- Categoria retornada deve corresponder ao valor enviado.
- Tempo de resposta dentro do limite.

---

### CT06 - Atualizar produto

**Método:** PUT  
**Endpoint:** `/products/{{productId}}`

**Objetivo:**  
Validar a atualização de dados de um produto existente.

**Validações:**
- Status code 200.
- Content-Type JSON.
- ID retornado deve corresponder ao `productId`.
- Título deve ser atualizado corretamente.
- Preço deve ser atualizado corretamente.
- Campos essenciais devem permanecer presentes.
- Tempo de resposta dentro do limite.

---

### CT07 - Excluir produto

**Método:** DELETE  
**Endpoint:** `/products/{{productId}}`

**Objetivo:**  
Validar a exclusão simulada de um produto.

**Validações:**
- Status code 200.
- Content-Type JSON.
- ID retornado deve corresponder ao produto solicitado.
- `isDeleted` deve ser `true`.
- `deletedOn` deve estar presente.
- Tempo de resposta dentro do limite.

---

## Authentication

### CT08 - Login válido

**Método:** POST  
**Endpoint:** `/auth/login`

**Objetivo:**  
Validar autenticação utilizando credenciais válidas.

**Validações:**
- Status code 200.
- Content-Type JSON.
- Retorno de `accessToken`.
- Retorno de `refreshToken`.
- Dados do usuário autenticado.
- Tempo de resposta dentro do limite.

**Pós-condição:**
- `accessToken` salvo no environment.
- `refreshToken` salvo no environment.

---

### CT09 - Login inválido

**Método:** POST  
**Endpoint:** `/auth/login`

**Objetivo:**  
Validar tentativa de login utilizando senha incorreta.

**Validações:**
- Status code 400.
- Content-Type JSON.
- Mensagem `Invalid credentials`.
- Resposta não deve conter `accessToken`.
- Resposta não deve conter `refreshToken`.
- Tempo de resposta dentro do limite.

---

### CT10 - Login sem senha

**Método:** POST  
**Endpoint:** `/auth/login`

**Objetivo:**  
Validar tentativa de autenticação sem informar a senha.

**Validações:**
- Status code 400.
- Content-Type JSON.
- Mensagem `Username and password required`.
- Ausência de `accessToken`.
- Ausência de `refreshToken`.
- Tempo de resposta dentro do limite.

---

### CT11 - Validar usuário autenticado

**Método:** GET  
**Endpoint:** `/auth/me`

**Autenticação:**  
Bearer Token utilizando `{{accessToken}}`.

**Objetivo:**  
Validar que o token gerado no login permite acessar um endpoint protegido.

**Validações:**
- Status code 200.
- Content-Type JSON.
- Existência dos dados do usuário autenticado.
- Username esperado.
- Informações de perfil presentes.
- Tempo de resposta dentro do limite.

---

### CT12 - Token inválido

**Método:** GET  
**Endpoint:** `/auth/me`

**Objetivo:**  
Validar tentativa de acesso ao endpoint protegido utilizando token inválido.

**Validações:**
- Status code 401.
- Content-Type JSON.
- Mensagem `Invalid/Expired Token!`.
- Tempo de resposta dentro do limite.

---

## Users

### CT13 - Listar usuários

**Método:** GET  
**Endpoint:** `/users`

**Objetivo:**  
Validar a listagem de usuários cadastrados.

**Validações:**
- Status code 200.
- Content-Type JSON.
- Existência da propriedade `users`.
- `users` deve ser um array.
- Lista deve conter usuários.
- Usuários devem possuir campos essenciais.
- Tempo de resposta dentro do limite.

---

### CT14 - Buscar usuário por ID

**Método:** GET  
**Endpoint:** `/users/{{userId}}`

**Objetivo:**  
Validar a consulta de um usuário existente utilizando ID.

**Validações:**
- Status code 200.
- Content-Type JSON.
- ID retornado deve corresponder ao `userId`.
- Usuário deve conter campos essenciais.
- E-mail deve possuir formato válido.
- Tempo de resposta dentro do limite.

---

### CT15 - Buscar usuário inexistente

**Método:** GET  
**Endpoint:** `/users/{{invalidUserId}}`

**Objetivo:**  
Validar o comportamento da API ao consultar um usuário inexistente.

**Validações:**
- Status code 404.
- Content-Type JSON.
- Mensagem deve indicar usuário não encontrado.
- Mensagem deve conter o ID consultado.
- Tempo de resposta dentro do limite.

---

### CT16 - Buscar usuários por termo

**Método:** GET  
**Endpoint:** `/users/search?q={{userSearchTerm}}`

**Objetivo:**  
Validar a funcionalidade de pesquisa de usuários.

**Validações:**
- Status code 200.
- Content-Type JSON.
- Existência da lista `users`.
- Busca deve retornar pelo menos um usuário.
- Usuários devem conter campos essenciais.
- Resultados devem estar relacionados ao termo pesquisado.
- Tempo de resposta dentro do limite.

---

### CT17 - Criar usuário

**Método:** POST  
**Endpoint:** `/users/add`

**Objetivo:**  
Validar a criação simulada de um usuário utilizando dados dinâmicos.

**Características:**
- E-mail gerado dinamicamente.
- Username gerado dinamicamente.

**Validações:**
- Status code 201.
- Content-Type JSON.
- Usuário criado deve possuir ID.
- Nome, sobrenome e idade devem corresponder aos valores enviados.
- E-mail retornado deve corresponder ao valor dinâmico enviado.
- Username retornado deve corresponder ao valor dinâmico enviado.
- Formato do e-mail deve ser válido.
- Tempo de resposta dentro do limite.

---

### CT18 - Atualizar usuário

**Método:** PUT  
**Endpoint:** `/users/{{userId}}`

**Objetivo:**  
Validar atualização de dados de um usuário existente.

**Validações:**
- Status code 200.
- Content-Type JSON.
- ID retornado deve corresponder ao usuário solicitado.
- Nome deve ser atualizado corretamente.
- Idade deve ser atualizada corretamente.
- Campos essenciais devem ser preservados.
- Tempo de resposta dentro do limite.

---

### CT19 - Excluir usuário

**Método:** DELETE  
**Endpoint:** `/users/{{userId}}`

**Objetivo:**  
Validar a exclusão simulada de um usuário.

**Validações:**
- Status code 200.
- Content-Type JSON.
- ID retornado deve corresponder ao usuário solicitado.
- `isDeleted` deve ser `true`.
- `deletedOn` deve estar presente.
- Tempo de resposta dentro do limite.

---

## Negative Scenarios

### CT20 - Criar produto sem título

**Método:** POST  
**Endpoint:** `/products/add`

**Objetivo:**  
Avaliar o comportamento da API quando o campo `title` não é enviado.

**Resultado observado:**  
A API aceita a requisição e retorna `201 Created`.

**Validações:**
- Status code 201.
- Content-Type JSON.
- Produto deve possuir ID.
- Resposta não deve conter `title`.
- Demais dados enviados devem ser mantidos.
- Tempo de resposta dentro do limite.

**Observação:**  
O cenário demonstra que a API simulada não exige o campo `title` durante a criação do produto.

---

### CT21 - Atualizar produto inexistente

**Método:** PUT  
**Endpoint:** `/products/{{invalidProductId}}`

**Objetivo:**  
Validar tentativa de atualização de um produto inexistente.

**Validações:**
- Status code 404.
- Content-Type JSON.
- Mensagem deve indicar produto não encontrado.
- Mensagem deve conter o ID consultado.
- Tempo de resposta dentro do limite.

---

### CT22 - Criar usuário com e-mail inválido

**Método:** POST  
**Endpoint:** `/users/add`

**Objetivo:**  
Avaliar o comportamento da API ao enviar um e-mail em formato inválido.

**Resultado observado:**  
A API aceita o valor inválido e retorna `201 Created`.

**Validações:**
- Status code 201.
- Content-Type JSON.
- Usuário criado deve possuir ID.
- E-mail retornado deve permanecer igual ao valor enviado.
- Demais dados enviados devem ser preservados.
- Tempo de resposta dentro do limite.

**Observação:**  
O endpoint não realiza validação de formato do campo `email` neste cenário simulado.

---

### CT23 - Buscar usuários sem resultado

**Método:** GET  
**Endpoint:** `/users/search?q=usuarioquenaoexiste999`

**Objetivo:**  
Validar o comportamento da API quando uma busca não possui correspondências.

**Validações:**
- Status code 200.
- Content-Type JSON.
- `users` deve ser um array vazio.
- `total` deve ser igual a zero.
- `limit` deve ser igual a zero.
- Tempo de resposta dentro do limite.

---

## Resumo da suíte

| Categoria | Cenários |
|---|---:|
| Products | 7 |
| Authentication | 5 |
| Users | 7 |
| Negative Scenarios | 4 |
| **Total** | **23** |

A suíte contempla testes funcionais, CRUD, autenticação, validação de tokens, pesquisa, dados dinâmicos, cenários negativos e validação de comportamento da API.