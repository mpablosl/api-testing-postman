# Observações técnicas

## OBS-001 — Campo `password` retornado na consulta de usuários

### Classificação

| Campo | Informação |
|---|---|
| ID | OBS-001 |
| Tipo | Segurança / Exposição de dado sensível |
| Severidade sugerida | Alta |
| Prioridade sugerida | Alta |
| Status | Pendente de confirmação |

### Endpoint analisado

| Campo | Informação |
|---|---|
| Método | GET |
| Endpoint | `/usuarios` |
| Autenticação | Não necessária |
| Status code observado | 200 |

### Pré-condição

Existir pelo menos um usuário cadastrado no ServeRest.

### Passos para observar

1. Abrir o Postman.
2. Criar uma request utilizando o método `GET`.
3. Informar o endpoint `/usuarios`.
4. Executar a request.
5. Analisar os objetos retornados dentro da propriedade `usuarios`.

### Resultado atual

Foi identificado o campo `password` nos objetos de usuário retornados
no response body.

### Resultado esperado

A resposta de uma consulta de usuários não deveria expor senhas ou outros
dados sensíveis.

### Impacto potencial

A exposição desse campo pode revelar informação sensível e aumentar o risco
de comprometimento das contas dos usuários.

### Evidência

- Status code observado: `200`.
- Campo `password` identificado no response body.
- Não armazenar senhas reais ou o response completo no repositório.

### Observação

Esta ocorrência deve ser confirmada contra o contrato ou requisito oficial
da API antes de ser classificada definitivamente como defeito.

Até essa confirmação, o registro será tratado como uma observação técnica
ou possível vulnerabilidade.