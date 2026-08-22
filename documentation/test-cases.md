# Casos de teste

## CT-001 — Consultar usuários existentes

| Campo | Informação |
|---|---|
| ID | CT-001 |
| Cenário | Consultar usuários existentes no ServeRest |
| Método | GET |
| Endpoint | `/usuarios` |
| Autenticação | Não necessária |
| Request body | Ausente |
| Query parameters | Não utilizados |
| Status code esperado | 200 |
| Response esperada | JSON contendo as propriedades `quantidade` e `usuarios` |
| Resultado obtido | Status code `200` e response em JSON |
| Resultado | Aprovado parcialmente |
| Observação | Ver `OBS-001` sobre o campo `password` retornado na response |

### Pré-condições

- API ServeRest disponível.
- Existir pelo menos um usuário cadastrado.

### Passos para execução

1. Abrir o Postman.
2. Criar uma request utilizando o método `GET`.
3. Informar o endpoint `/usuarios`.
4. Não informar request body.
5. Não informar token.
6. Executar a request.
7. Analisar o status code e o response body.

### Resultado esperado

- A API deve retornar status code `200`.
- A response deve estar em formato JSON.
- A response deve conter a propriedade `quantidade`.
- A response deve conter a propriedade `usuarios`.
- A propriedade `usuarios` deve representar uma lista de usuários.

### Resultado obtido

- Status code retornado: `200`.
- Response retornada em formato JSON.
- Propriedade `quantidade` identificada.
- Propriedade `usuarios` identificada.
- Campos `nome` e `email` identificados nos usuários.
- Tempo observado: aproximadamente `189 ms`.

### Validações ainda pendentes

- Confirmar que `usuarios` é um array.
- Confirmar que `quantidade` é um número.
- Validar os campos obrigatórios de cada usuário.
- Validar os tipos dos dados.
- Comparar o valor de `quantidade` com a quantidade de itens do array.
- Criar assertions automatizadas no Postman.

### Conclusão

A requisição retornou `200 OK` e uma response JSON com as propriedades
`quantidade` e `usuarios`. O resultado é compatível com o cenário positivo
de consulta de usuários.

A aprovação completa ainda depende da validação automatizada da estrutura,
dos campos obrigatórios, dos tipos de dados e da consistência entre a
quantidade informada e os itens retornados.