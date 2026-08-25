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
| `baseurl` | `https://serverest.dev` |

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

Exemplo de script:

```javascript
const timestamp = Date.now();

pm.variables.set("newUserName", `QA User ${timestamp}`);
pm.variables.set("newUserEmail", `qa${timestamp}@example.com`);
pm.variables.set("newUserPassword", "SenhaTeste@123");
pm.variables.set("newUserAdmin", "false");
```

#### Validações automatizadas

- O status code deve ser `201`.
- A response deve estar em formato JSON.
- A mensagem deve indicar que o cadastro foi realizado com sucesso.
- A response deve conter `_id`.
- O `_id` deve ser um texto não vazio.

#### Observação

As senhas utilizadas são exclusivamente dados de teste e não devem ser publicadas como credenciais reais.

---

## Endpoints implementados

| Método | Endpoint | Descrição | Status |
|---|---|---|---|
| GET | `/usuarios` | Consultar usuários | Implementado |
| POST | `/usuarios` | Criar usuário | Implementado |
| GET | `/usuarios/{id}` | Consultar usuário por ID | Próximo passo |
| PUT | `/usuarios/{id}` | Atualizar usuário | Pendente |
| DELETE | `/usuarios/{id}` | Excluir usuário | Pendente |
| POST | `/login` | Realizar login | Pendente |
| GET | `/produtos` | Consultar produtos | Pendente |
| POST | `/produtos` | Criar produto | Pendente |

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

---

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

1. Capturar o `_id` retornado pelo cadastro.
2. Consultar o usuário criado com `GET /usuarios/{id}`.
3. Implementar atualização de usuário.
4. Implementar exclusão de usuário.
5. Criar cenários negativos para usuários.
6. Implementar login.
7. Capturar e reutilizar token.
8. Testar produtos.
9. Executar a Collection completa.
10. Atualizar evidências e documentação.
11. Integrar a execução ao processo de versionamento.

---

## Status do projeto

O projeto está em desenvolvimento incremental.

### Implementado

- Collection organizada;
- Environment configurado;
- variável `baseurl`;
- consulta de usuários;
- assertions automatizadas;
- criação de usuário;
- dados dinâmicos para cadastro;
- documentação inicial.

### Em desenvolvimento

- request chaining;
- consulta por ID;
- atualização e exclusão;
- autenticação;
- testes de produtos;
- execução completa da Collection.