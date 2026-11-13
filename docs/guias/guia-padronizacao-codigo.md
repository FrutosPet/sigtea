# Guia de Padronização de Código e Versionamento — SIGTEA 2.0

Este documento estabelece as convenções obrigatórias de escrita de código, estrutura de diretórios e fluxo de versionamento (Git) para toda a equipe de desenvolvimento do **SIGTEA 2.0** (Frontend e Backend).

---

## 1. Convenções de Nomenclatura

| Elemento | Padrão | Exemplo Correto | Exemplo Incorreto |
| :--- | :--- | :--- | :--- |
| **Variáveis e Funções** | `camelCase` | `buscarUnidadePorCnes`, `listaPacientes` | `buscar_unidade`, `ListaPacientes` |
| **Constantes Globais** | `UPPER_SNAKE_CASE` | `MAX_TENTATIVAS_LOGIN`, `PORTA_SERVIDOR` | `maxTentativas`, `portaServidor` |
| **Classes e Componentes** | `PascalCase` | `UnidadeSaudeController`, `CardAgendamento` | `unidadeSaudeController`, `card-agendamento` |
| **Arquivos (Backend)** | `kebab-case` | `unidade-saude.controller.js`, `auth.middleware.js` | `UnidadeSaudeController.js`, `auth_middleware.js` |
| **Arquivos (Frontend)** | `PascalCase` (Componentes) / `kebab-case` (Utils) | `FormularioCadastro.jsx`, `formatar-cpf.js` | `formulariocadastro.jsx`, `FormatarCPF.js` |
| **Tabelas e Colunas (Banco)** | `snake_case` | `unidades_saude`, `data_nascimento`, `criado_em` | `unidadesSaude`, `DataNascimento` |

---

## 2. Boas Práticas de Escrita (JavaScript / Node.js)

* **Declaração de Variáveis:** Utilize sempre `const` por padrão. Use `let` apenas quando o valor precisar ser reatribuído. **Nunca utilize `var`.**
* **Assincronismo:** Prefira `async/await` acompanhado de blocos `try/catch` para operações assíncronas (consultas ao banco ou chamadas HTTP), evitando o encadeamento excessivo de `.then()`.
* **Comparações:** Utilize sempre igualdade estrita (`===` e `!==`) em vez de igualdade solta (`==` e `!=`).
* **Tratamento de Erros:** Nunca deixe blocos `catch` vazios. Registre o erro ou repasse-o para o middleware global de tratamento de exceções.
* **Idioma:** Nomes de variáveis, funções, rotas e comentários técnicos devem ser escritos em **Português (PT-BR)**, evitando abreviações obscuras (ex.: prefira `quantidadeAtendimentos` em vez de `qtdAtd`).

### Exemplo de Código Padronizado (Backend)

```javascript
const buscarUnidadePorId = async (req, res, next) => {
  try {
    const { id } = req.params;

    if (!id) {
      return res.status(400).json({
        sucesso: false,
        mensagem: 'O identificador da unidade é obrigatório.'
      });
    }

    const unidade = await unidadeRepository.buscarPorId(id);

    if (!unidade) {
      return res.status(404).json({
        sucesso: false,
        mensagem: 'Unidade de saúde não encontrada.'
      });
    }

    return res.status(200).json({
      sucesso: true,
      dados: unidade
    });
  } catch (erro) {
    return next(erro);
  }
};
