# Contrato da API - SIGTEA 2.0

`openapi.yaml` é a fonte da verdade da API. Mudou uma rota? Mude aqui primeiro, no mesmo PR.

Nomes de campos, branches e commits seguem o [guia de padronização](../INICIO.md).
Mudanças só no contrato usam commits do tipo `docs(api): ...`.

## Estrutura

```
docs/api/
├── openapi.yaml        # arquivo principal: aponta para todos os outros
├── parametros.yaml     # pagina e tamanho_pagina
├── respostas.yaml      # 401, 403, 404, 409 e 422 padrão
├── codigos-erro        # catálogo de códigos de erro (T3)
├── schemas/
│   ├── erro.yaml       # status, code, message, details (todos obrigatórios)
│   └── paginacao.yaml  # metadados de paginação
└── paths/
    ├── instituicao.yaml   # T2
    ├── unidades.yaml      # T2
    ├── referencias.yaml   # T3
    └── autenticacao.yaml  # T4
```

## Como usar os padrões numa rota nova

```yaml
get:
  tags: [Unidades]
  summary: Lista as unidades da instituição
  parameters:
    - $ref: '../openapi.yaml#/components/parameters/Pagina'
    - $ref: '../openapi.yaml#/components/parameters/TamanhoPagina'
  responses:
    '200':
      description: Lista paginada
      content:
        application/json:
          schema:
            type: object
            required: [dados, paginacao]
            properties:
              dados:
                type: array
                items:
                  $ref: '../schemas/unidade.yaml'
              paginacao:
                $ref: '../schemas/paginacao.yaml'
    '401':
      $ref: '../openapi.yaml#/components/responses/NaoAutenticado'
    '403':
      $ref: '../openapi.yaml#/components/responses/AcessoNegado'
```

Rotas públicas (sem token) declaram `security: []`.

## Validar antes de abrir o PR

```bash
npx @redocly/cli lint docs/api/openapi.yaml
npx @redocly/cli preview-docs docs/api/openapi.yaml   # documentação navegável
```

Os arquivos de `paths/` vêm vazios na T1 para o contrato validar desde já.
A T2, a T3 e a T4 só preenchem o conteúdo deles.
