# API Testing com Postman e ServeRest

## Objetivo

Este projeto demonstra a aplicação prática de testes de API utilizando Postman, ServeRest, JavaScript e Git.

O projeto contempla:

- organização de Collections;
- parametrização de URLs com variables;
- testes positivos e negativos;
- assertions automatizadas;
- criação e consulta de usuários;
- geração de dados dinâmicos;
- request chaining;
- execução de Collections;
- versionamento com Git e GitHub.

---

## Tecnologias e ferramentas

- Postman
- ServeRest
- JavaScript
- Git
- GitHub
- Markdown

---

## API utilizada

[ServeRest](https://serverest.dev/)

---

## Escopo atual

Até o momento, foram implementados testes para:

- consulta de usuários;
- criação de usuários;
- validações automatizadas de responses;
- geração dinâmica de dados para cadastro;
- organização da Collection;
- utilização de variável para a URL base.
- consulta de usuário por ID;
- reutilização do identificador retornado pelo cadastro;
- request chaining entre criação e consulta.
- atualização de usuário existente;
- validação da resposta de alteração;
- reutilização do identificador do usuário.
- cenário negativo de atualização com email duplicado;
- validação de rejeição de dados inválidos;
- confirmação de que o registro original não foi alterado.
- consulta de produtos;
- validação da estrutura da lista de produtos.
- criação de usuário administrador;
- autenticação com login;
- captura e reutilização de token;
- criação autenticada de produtos;
- consulta de produto por ID;
- reutilização do identificador do produto;
- atualização de produtos;
- validação dos dados atualizados;
- exclusão de produtos;
- confirmação de que o produto excluído não pode mais ser consultado.

---

## Estrutura da Collection

```text
ServeRest - API Testing
├── Authentication
│   └── Login
├── Users
│   ├── List Users
│   ├── Get User
│   ├── Create User
│   ├── Update User
│   └── Delete User
└── Products
    ├── List Products
    ├── Get Product
    ├── Create Product
    ├── Update Product
    └── Delete Product
```

As requests foram organizadas por domínio funcional:

- `Authentication`: autenticação;
- `Users`: operações relacionadas a usuários;
- `Products`: operações relacionadas a produtos.

---

## Environment

Foi criado o Environment:

```text
ServeRest - QA
```

Variável configurada:

| Variável | Valor |
|---|---|
| `baseurl` | URL base da API |
| `userId` | Identificador do usuário criado |
| `adminUserName` | Nome do administrador de teste |
| `adminUserEmail` | Email do administrador de teste |
| `adminUserPassword` | Senha do administrador de teste |
| `authToken` | Token usado nas requisições autenticadas |
| `productId` | Identificador do produto criado |

> Valores de senha e token são utilizados somente para testes e não devem ser publicados no repositório.

As requests utilizam a variável da seguinte forma:

```text
{{baseurl}}/usuarios
```

---

## Casos de teste

### CT-001 — Consultar usuários existentes

| Campo | Informação |
|---|---|
| Método | GET |
| Endpoint | `/usuarios` |
| Autenticação | Não necessária |
| Request body | Ausente |
| Status esperado | 200 |
| Status obtido | 200 |
| Response | JSON |
| Resultado | Aprovado |

#### Validações automatizadas

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- A propriedade `quantidade` deve existir.
- A propriedade `usuarios` deve existir.
- A propriedade `usuarios` deve ser um array.
- `quantidade` deve ser um número.
- `quantidade` deve corresponder ao tamanho do array `usuarios`.
- Cada usuário deve possuir `nome`.
- Cada usuário deve possuir `email`.
- `nome` e `email` devem ser textos.

---

### CT-002 — Criar usuário com dados válidos

| Campo | Informação |
|---|---|
| Método | POST |
| Endpoint | `/usuarios` |
| Autenticação | Não necessária |
| Request body | JSON |
| Status esperado | 201 |
| Status obtido | 201 |
| Resultado | Aprovado |

#### Dados utilizados

Os dados de cadastro são gerados dinamicamente antes da execução da request.
---
### CT-003 — Consultar usuário criado por ID

| Campo | Informação |
|---|---|
| ID | CT-003 |
| Cenário | Consultar usuário criado por ID |
| Método | GET |
| Endpoint | `/usuarios/{id}` |
| Autenticação | Não necessária |
| Request body | Ausente |
| Status esperado | 200 |
| Resultado esperado | Dados do usuário criado são retornados |
| Resultado | A preencher após a execução |

### Pré-condição

O caso depende da execução prévia do `CT-002`, que salva o `_id`
retornado pela API na variável `userId`.

#### Dados utilizados

Os dados de cadastro são gerados dinamicamente antes da execução da request.

Exemplo de script:

```javascript
const timestamp = Date.now();

pm.variables.set("newUserName", `QA User ${timestamp}`);
pm.variables.set("newUserEmail", `qa${timestamp}@example.com`);
pm.variables.set("newUserPassword", "SenhaTeste@123");
pm.variables.set("newUserAdmin", "false");
```

#### Validações automatizadas

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- A response deve conter o `_id` consultado.
- O `_id` retornado deve ser igual ao valor armazenado em `userId`.
- A response deve conter `nome`.
- A response deve conter `email`.

---
### CT-004 — Atualizar usuário existente

| Campo | Informação |
|---|---|
| ID | CT-004 |
| Cenário | Atualizar usuário existente |
| Método | PUT |
| Endpoint | `/usuarios/{id}` |
| Autenticação | Não necessária |
| Request body | JSON |
| Status esperado | 200 |
| Resultado esperado | Usuário atualizado com sucesso |
| Resultado | A preencher após a execução |

### Pré-condição

O caso depende da execução prévia do `CT-002`, que cria o usuário
e salva o `_id` na variável `userId`.

### Validações

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- A response deve conter `message`.
- A mensagem deve ser `Registro alterado com sucesso`.
---
### CT-005 — Atualizar usuário com email duplicado

| Campo | Informação |
|---|---|
| ID | CT-005 |
| Cenário | Atualizar usuário usando email já cadastrado |
| Método | PUT |
| Endpoint | `/usuarios/{id}` |
| Autenticação | Não necessária |
| Request body | JSON |
| Status esperado | 400 |
| Resultado esperado | Atualização rejeitada |
| Resultado | A preencher após a execução |

### Pré-condições

- Deve existir um usuário criado.
- Deve existir outro usuário com email diferente.
- O `userId` deve representar o usuário que será atualizado.
- O email enviado no body deve pertencer a outro usuário.

### Validações

- O status code deve ser `400`.
- A response deve estar em formato JSON.
- A response deve informar erro relacionado ao email.
- O usuário original não deve ser alterado.
---
## CT-006 — Excluir usuário existente

| Campo | Informação |
|---|---|
| ID | CT-006 |
| Cenário | Excluir usuário existente |
| Método | DELETE |
| Endpoint | `/usuarios/{id}` |
| Autenticação | Não necessária |
| Request body | Ausente |
| Status esperado | 200 |
| Resultado esperado | Usuário excluído com sucesso |
| Resultado | A preencher após a execução |

### Pré-condição

O `userId` deve corresponder a um usuário criado durante o fluxo de teste.

### Validações

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- A response deve conter `message`.
- A mensagem deve ser `Registro excluído com sucesso`.
---
## CT-007 — Consultar usuário excluído

| Campo | Informação |
|---|---|
| ID | CT-007 |
| Cenário | Consultar usuário após exclusão |
| Método | GET |
| Endpoint | `/usuarios/{id}` |
| Autenticação | Não necessária |
| Request body | Ausente |
| Status esperado | 400 |
| Resultado esperado | Usuário não encontrado |
| Resultado | A preencher após a execução |

### Pré-condição

O usuário deve ter sido excluído pelo caso `CT-006`.

### Validações

- O status code deve ser `400`.
- A response deve estar em formato JSON.
- A mensagem deve ser `Usuário não encontrado`.
---
## CT-008 — Consultar produtos existentes

| Campo | Informação |
|---|---|
| ID | CT-008 |
| Cenário | Consultar produtos existentes |
| Método | GET |
| Endpoint | `/produtos` |
| Autenticação | Não necessária |
| Request body | Ausente |
| Status esperado | 200 |
| Resultado esperado | Lista de produtos retornada em JSON |
| Resultado | A preencher após a execução |

### Validações

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- A propriedade `quantidade` deve existir.
- A propriedade `produtos` deve existir.
- `produtos` deve ser um array.
- `quantidade` deve ser um número.
- `quantidade` deve corresponder ao tamanho do array `produtos`.
---
### CT-009 — Criar usuário administrador

| Campo | Informação |
|---|---|
| ID | CT-009 |
| Cenário | Criar usuário administrador |
| Método | POST |
| Endpoint | `/usuarios` |
| Autenticação | Não necessária |
| Request body | JSON |
| Status esperado | 201 |
| Resultado esperado | Administrador criado com sucesso |
| Resultado | Aprovado |

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- A response deve conter `authorization`.
- O token deve ser salvo na variável `authToken`.
---
### CT-011 — Criar produto com dados válidos

| Campo | Informação |
|---|---|
| ID | CT-011 |
| Cenário | Criar produto autenticado |
| Método | POST |
| Endpoint | `/produtos` |
| Autenticação | Bearer token |
| Request body | JSON |
| Status esperado | 201 |
| Resultado esperado | Produto criado com sucesso |
| Resultado | Aprovado |

### Pré-condições
- Deve existir um usuário administrador.
- O login deve ter sido realizado com sucesso.
- O token deve estar salvo na variável `authToken`.

### Validações
- O status code deve ser `201`.
- A response deve estar em formato JSON.
- A response deve conter `message`.
- A mensagem deve ser `Cadastro realizado com sucesso`.
- A response deve conter `_id`.
- O `_id` deve ser um texto não vazio.
- O identificador deve ser salvo na variável `productId`.
---
### CT-012 — Consultar produto criado por ID

| Campo | Informação |
|---|---|
| ID | CT-012 |
| Cenário | Consultar produto criado por ID |
| Método | GET |
| Endpoint | `/produtos/{id}` |
| Autenticação | Não necessária |
| Request body | Ausente |
| Status esperado | 200 |
| Resultado esperado | Dados do produto criado são retornados |
| Resultado | Aprovado |

### Pré-condições

O caso depende da execução prévia do CT-011, que salva o `_id` retornado pela API na variável `productId`.

### Validações

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- O `_id` retornado deve ser igual ao valor armazenado em `productId`.
- O produto deve conter `nome`.
- O produto deve conter `preco`.
- O produto deve conter `descricao`.
- O produto deve conter `quantidade`.
- `preco` deve ser numérico.
- `quantidade` deve ser numérica.
---
## CT-013 — Atualizar produto existente

| Campo | Informação |
|---|---|
| ID | CT-013 |
| Cenário | Atualizar produto existente |
| Método | PUT |
| Endpoint | `/produtos/{id}` |
| Autenticação | Bearer token |
| Request body | JSON |
| Status esperado | 200 |
| Resultado esperado | Produto atualizado com sucesso |
| Resultado | Aprovado |

### Pré-condições

- Deve existir um usuário administrador.
- O login deve ter sido realizado com sucesso.
- O token deve estar salvo em `authToken`.
- Deve existir um produto criado.
- O identificador deve estar salvo em `productId`.

### Validações

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- A mensagem deve ser `Registro alterado com sucesso`.
- Os dados atualizados devem ser confirmados com `GET /produtos/{id}`.
---

## CT-014 — Excluir produto existente

| Campo | Informação |
|---|---|
| ID | CT-014 |
| Cenário | Excluir produto existente |
| Método | DELETE |
| Endpoint | `/produtos/{id}` |
| Autenticação | Bearer token |
| Request body | Ausente |
| Status esperado | 200 |
| Resultado esperado | Produto excluído com sucesso |
| Resultado | Aprovado |

### Validações

- O status code deve ser `200`.
- A response deve estar em formato JSON.
- A response deve conter `message`.
- A mensagem deve ser `Registro excluído com sucesso`.

---

## CT-015 — Consultar produto excluído

| Campo | Informação |
|---|---|
| ID | CT-015 |
| Cenário | Consultar produto após exclusão |
| Método | GET |
| Endpoint | `/produtos/{id}` |
| Autenticação | Não necessária |
| Request body | Ausente |
| Status esperado | 400 |
| Resultado esperado | Produto não encontrado |
| Resultado | Aprovado |

### Pré-condição

O produto deve ter sido excluído pelo caso `CT-014`.

### Validações

- O status code deve ser `400`.
- A response deve estar em formato JSON.
- A mensagem deve ser `Produto não encontrado`.

---
#### Observação

As senhas utilizadas são exclusivamente dados de teste e não devem ser publicadas como credenciais reais.

---

## Endpoints implementados

| Método | Endpoint | Descrição | Status |
|---|---|---|---|
| GET | `/usuarios` | Consultar usuários | Implementado |
| POST | `/usuarios` | Criar usuário | Implementado |
| GET | `/usuarios/{id}` | Consultar usuário por ID | Implementado |
| PUT | `/usuarios/{id}` | Atualizar usuário | Implementado |
| PUT | `/usuarios/{id}` | Atualizar usuário com email duplicado | Implementado |
| DELETE | `/usuarios/{id}` | Excluir usuário | Implementado |
| GET | `/usuarios/{id}` | Validar usuário excluído | Implementado |
| POST | `/login` | Realizar login | Implementado |
| GET | `/produtos` | Consultar produtos | Implementado |
| POST | `/produtos` | Criar produto autenticado | Implementado |
| GET | `/produtos/{id}` | Consultar produto por ID | Implementado |
| PUT | `/produtos/{id}` | Atualizar produto | Implementado |
| DELETE | `/produtos/{id}` | Excluir produto | Implementado |
| GET | `/produtos/{id}` | Validar produto excluído | Implementado |

---

## Scripts utilizados

### `List Users`

A request possui assertions para validar:

- status code;
- formato da response;
- propriedades obrigatórias;
- tipos de dados;
- consistência entre `quantidade` e `usuarios`.

### `Create User`

A request possui:

- geração de dados dinâmicos;
- body JSON parametrizado;
- validação de cadastro bem-sucedido;
- validação do `_id` retornado.

### `Login`

A request possui:

- body JSON com email e senha parametrizados;
- validação do status code;
- validação da presença de `authorization`;
- armazenamento do token na variável `authToken`;
- reutilização do token em requests autenticadas.

### `List Products`

A request possui assertions para validar:

- status code;
- formato JSON;
- propriedade `quantidade`;
- propriedade `produtos`;
- tipo da lista;
- consistência entre `quantidade` e a quantidade de itens retornados.
---

### `Create Product`

A request possui:

- autenticação com Bearer token;
- body JSON parametrizado;
- validação do status code;
- validação da mensagem de sucesso;
- validação do `_id` retornado;
- armazenamento do identificador na variável `productId`.

### `Get Product`

A request possui:

- reutilização da variável `productId`;
- validação do status code;
- validação do formato JSON;
- validação da identidade do produto consultado;
- validação dos campos `nome`, `preco`, `descricao` e `quantidade`.

## Observações técnicas

Durante a análise inicial da consulta de usuários, foi identificado o campo `password` na response body.

Essa ocorrência foi registrada para análise separada em:

```text
documentation/observations.md
```

A presença de dados sensíveis ou potencialmente sensíveis em responses deve ser avaliada conforme:

- necessidade funcional;
- exposição indevida;
- mascaramento;
- política de segurança;
- contrato esperado da API.

---

## Estrutura do projeto

```text
api-testing-postman/
├── documentation/
│   ├── test-cases.md
│   └── observations.md
├── collections/
│   └── ServeRest - API Testing.postman_collection.json
├── environments/
├── test-data/
├── evidence/
├── README.md
└── .gitignore
```

---

## Próximas etapas

1. Executar a Collection completa.
2. Atualizar evidências e documentação.
3. Revisar variáveis e credenciais antes do commit.
4. Integrar a execução ao processo de versionamento.

---

## Status do projeto

O projeto está em desenvolvimento incremental.

### Implementado

- Collection organizada;
- Environment configurado;
- variável `baseurl`;
- CRUD de usuários;
- cenários positivos e negativos para usuários;
- criação de usuário administrador;
- autenticação com login;
- captura e reutilização de token;
- CRUD de produtos;
- consulta de produto após atualização;
- confirmação de produto excluído;
- assertions automatizadas;
- geração de dados dinâmicos;
- request chaining com `userId`, `authToken` e `productId`;
- documentação dos casos de teste.

### Em desenvolvimento

- execução completa da Collection;
- atualização das evidências;
- revisão final do projeto;
- integração da execução ao processo de versionamento.