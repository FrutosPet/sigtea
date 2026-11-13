# Guia de Padronização da API REST — SIGTEA 2.0

Este documento define o contrato técnico e os padrões de comunicação HTTP entre o Frontend e a API Backend do **SIGTEA 2.0**, seguindo a especificação **OpenAPI 3.0**.

---

## 1. Estrutura de URLs e Recursos

* **Versionamento na URL:** Todas as rotas devem conter o prefixo da versão atual (`/api/v1/...`).
* **Substantivos no Plural:** Utilize substantivos no plural e em português para representar recursos. Nunca utilize verbos na URL.
* **Padrão de Escrita:** Utilize `kebab-case` (letras minúsculas separadas por hífen) para recursos compostos.

| Operação | Método HTTP | Rota Padrão | Exemplo Prático |
| :--- | :--- | :--- | :--- |
| Listar todos | `GET` | `/api/v1/<recursos>` | `GET /api/v1/unidades-saude` |
| Buscar por ID | `GET` | `/api/v1/<recursos>/{id}` | `GET /api/v1/unidades-saude/42` |
| Criar registro | `POST` | `/api/v1/<recursos>` | `POST /api/v1/unidades-saude` |
| Atualizar completo | `PUT` | `/api/v1/<recursos>/{id}` | `PUT /api/v1/unidades-saude/42` |
| Atualizar parcial | `PATCH` | `/api/v1/<recursos>/{id}` | `PATCH /api/v1/unidades-saude/42` |
| Remover registro | `DELETE` | `/api/v1/<recursos>/{id}` | `DELETE /api/v1/unidades-saude/42` |

---

## 2. Formato de Dados (JSON)

* **Content-Type:** Todas as requisições com corpo e todas as respostas da API devem utilizar `application/json; charset=utf-8`.
* **Nomenclatura de Chaves:** Todas as propriedades dos objetos JSON devem seguir o padrão **`camelCase`** (ex.: `nomeUnidade`, `codigoCnes`, `dataCadastro`).
* **Datas e Horários:** Devem trafegar estritamente no padrão **ISO 8601** (`YYYY-MM-DD` para datas e `YYYY-MM-DDTHH:mm:ssZ` para data e hora em UTC).

---

## 3. Envelope Padrão de Resposta

Toda resposta retornada pela API deve obedecer a uma estrutura previsível para facilitar o consumo pelo Frontend.

### 3.1 Resposta de Sucesso (Item Único)

```json
{
  "sucesso": true,
  "mensagem": "Unidade de saúde cadastrada com sucesso.",
  "dados": {
    "id": 15,
    "nomeUnidade": "CAPS Infantil Centro",
    "codigoCnes": "2078451",
    "ativo": true,
    "criadoEm": "2026-11-13T14:30:00Z"
  }
}
```

### 3.2 Resposta de Sucesso com Paginação (Listagens)

Listagens devem suportar os parâmetros de consulta `?pagina=1&limite=10` e retornar metadados de paginação:

```json
{
  "sucesso": true,
  "dados": [
    {
      "id": 15,
      "nomeUnidade": "CAPS Infantil Centro",
      "codigoCnes": "2078451"
    }
  ],
  "paginacao": {
    "paginaAtual": 1,
    "itensPorPagina": 10,
    "totalItens": 48,
    "totalPaginas": 5
  }
}
```

### 3.3 Resposta de Erro Padronizada

```json
{
  "sucesso": false,
  "erro": {
    "codigo": "VALIDACAO_DADOS",
    "mensagem": "Os dados enviados contêm erros de validação.",
    "detalhes": [
      {
        "campo": "codigoCnes",
        "mensagem": "O código CNES deve conter exatamente 7 dígitos numéricos."
      }
    ]
  }
}
```

---

## 4. Códigos de Status HTTP Obrigatórios

| Código | Significado | Quando Utilizar no SIGTEA 2.0 |
| :--- | :--- | :--- |
| **`200 OK`** | Sucesso | Consultas (`GET`), atualizações (`PUT`/`PATCH`) e exclusões (`DELETE`) concluídas com êxito. |
| **`201 Created`** | Criado | Criação de um novo recurso (`POST`) concluída com sucesso. |
| **`400 Bad Request`** | Requisição Inválida | Erro de sintaxe no JSON, parâmetros obrigatórios ausentes ou formato inválido. |
| **`401 Unauthorized`** | Não Autenticado | Token JWT ausente, inválido ou expirado. |
| **`403 Forbidden`** | Proibido | Usuário autenticado, mas sem perfil de permissão para acessar o recurso. |
| **`404 Not Found`** | Não Encontrado | O registro solicitado pelo ID ou a rota não existe. |
| **`409 Conflict`** | Conflito | Tentativa de cadastrar registro duplicado (ex.: CPF ou CNES já existente). |
| **`422 Unprocessable Entity`** | Regra de Negócio | JSON bem formado, mas viola uma regra de domínio do sistema. |
| **`500 Internal Server Error`** | Erro Interno | Falha inesperada no servidor ou perda de conexão com o banco de dados. |
