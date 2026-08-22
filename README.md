# API Testing com Postman e ServeRest

## Objetivo

Este projeto tem como objetivo demonstrar a aplicação prática de testes
de API utilizando o Postman e o ServeRest.

O projeto será desenvolvido progressivamente, começando pelos fundamentos
de HTTP e API Testing e evoluindo para testes automatizados, autenticação,
request chaining, execução de collections e integração contínua.

## Tecnologias e ferramentas

- Postman
- ServeRest
- Git
- GitHub
- Markdown

## API utilizada

ServeRest.

## Escopo atual

Nesta primeira etapa, foi realizada uma consulta de usuários utilizando
o método `GET`.

### Endpoint testado

| Método | Endpoint | Descrição |
|---|---|---|
| GET | `/usuarios` | Consultar usuários |

## Cenário executado

### CT-001 — Consultar usuários existentes

- Método: `GET`
- Endpoint: `/usuarios`
- Autenticação: não necessária
- Request body: ausente
- Status esperado: `200`
- Status obtido: `200`
- Response: JSON
- Resultado: aprovado parcialmente

## Validações realizadas

- Verificação do método HTTP.
- Verificação do endpoint.
- Verificação da ausência de request body.
- Verificação da necessidade de autenticação.
- Validação do status code `200`.
- Validação do formato JSON.
- Identificação da propriedade `quantidade`.
- Identificação da propriedade `usuarios`.
- Identificação dos campos `nome` e `email`.

## Validações pendentes

- Criar assertions automatizadas no Postman.
- Validar os tipos dos campos.
- Validar a estrutura dos objetos.
- Validar a consistência entre `quantidade` e a lista `usuarios`.
- Criar cenários negativos.
- Organizar a Collection do Postman.

## Observações técnicas

Foi identificado o campo `password` no response body da consulta de usuários.
Essa ocorrência foi registrada para análise separada.

Consulte:

- [Casos de teste](documentation/test-cases.md)
- [Observações técnicas](documentation/observations.md)

## Estrutura do projeto

```text
api-testing-postman/
├── documentation/
│   ├── test-cases.md
│   └── observations.md
├── collections/
├── environments/
├── test-data/
├── evidence/
├── README.md
└── .gitignore
```

## Próximas etapas

1. Criar uma Collection no Postman.
2. Adicionar a request de consulta de usuários.
3. Criar as primeiras assertions.
4. Testar o cadastro de usuários.
5. Criar cenários positivos e negativos.
6. Trabalhar com autenticação e tokens.
7. Implementar request chaining.
8. Executar a Collection automaticamente.