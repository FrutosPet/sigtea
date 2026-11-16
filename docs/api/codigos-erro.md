# Catálogo de códigos de erro – SIGTEA API

Lista única de todos os `code` que a API pode devolver no formato de erro
`{status, code, message, details}` (RNF15).

**Regras**

- O `code` é sempre em MAIÚSCULAS, com `_`, em português, e **nunca muda**, mesmo que a mensagem mude. É ele que o frontend usa.
- Todo `code` usado no contrato (`openapi.yaml`, `paths/`, `components/`) precisa existir aqui, e todo `code` daqui precisa ser usado no contrato.
- Para erros de campo, a mensagem específica vai em `details`.

| code | HTTP | Mensagem para o usuário | Onde aparece |
| --- | --- | --- | --- |
| TOKEN_INVALIDO | 401 | Sua sessão expirou. Entre novamente. | todas as rotas com token |
| TOKEN_EXPIRADO | 401 | Sua sessão expirou. Entre novamente. | HU03 |
| CREDENCIAIS_INVALIDAS | 401 | E-mail ou senha inválidos. | HU01 |
| CONTA_BLOQUEADA | 429 | Muitas tentativas. Tente novamente em 15 minutos. | HU07 |
| ACESSO_NEGADO | 403 | Você não tem permissão para esta ação. | todas |
| NAO_ENCONTRADO | 404 | Registro não encontrado. | todas com {id} |
| DADOS_INVALIDOS | 422 | (mensagem do campo, em details) | todas com corpo ou parâmetros |
| CNPJ_INVALIDO | 422 | CNPJ inválido. | HU17 |
| CNES_DUPLICADO | 409 | Já existe unidade com este CNES. | HU18 |
| NOME_UNIDADE_DUPLICADO | 409 | Já existe uma unidade com este nome. | HU18 |

