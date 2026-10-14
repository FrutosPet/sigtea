# DOCUMENTAÇÃO PARA DESENVOLVIMENTO

**Guia de Padronização de Código e Commits: SIGTEA 2.0**

**Público-alvo:** Desenvolvedores (Squads 1, 2 e 3)

1. **Nomenclatura e Estilo (Clean Code)**

**1.1 Variáveis e Funções (JavaScript)**

- **Regra:** Use camelCase.
- **Por que?** É o padrão universal da comunidade JavaScript.
- **Bom:** const pacienteId = 10;, function buscarHistoricoClinico() { ... }
- **Ruim:** const PacienteID = 10;, function buscar_historico() { ... }

**1.2 Componentes de Interface (React/Web)**

- **Regra:** Use PascalCase e nomes descritivos.
- **Bom:** FormularioAvaliacao.jsx, BotaoSalvar.jsx
- **Ruim:** vbox1.jsx, tela_1.jsx

**1.3 Banco de Dados (PostgreSQL)**

- **Regra:** Use snake_case e nomes no singular para tabelas.
- **Bom:** Tabela: atendimento, Coluna: data_inicio.
- **Ruim:** Tabela: Atendimentos, Coluna: DataInicio.
1. **Padrão de Commits (Conventional Commits)**

Os alunos devem entender que o histórico de commits é a **documentação da evolução do software**.

**Estrutura:** tipo(escopo): descrição curta em português

- feat: Quando você adiciona algo novo. (Ex: feat(auth): adiciona login com google)
- fix: Quando você conserta algo que estava quebrado. (Ex: fix(calculo): corrige soma do score SNAP-IV)
- docs: Mudanças apenas em textos/documentação. (Ex: docs(api): descreve parametros do endpoint de pacientes)
- style: Mudanças de visual (CSS) ou formatação que não alteram a lógica.
- refactor: Melhorar um código que já funciona, sem mudar o que ele faz.
1. **Fluxo de Trabalho (Git Flow Acadêmico)**

Para evitar que um aluno apague o código do outro:

1. **Branch main:** É o "Santuário". Ninguém mexe aqui sem o docente.
2. **Branch develop:** Onde as Squads se encontram. É o código que está sendo testado.
3. **Branch feature/nome-da-tarefa:** O aluno cria esta branch para trabalhar.
    - *Exemplo:* feature/eliel-login-social ou feature/walter-escala-cars.

**O Ritual do Pull Request (PR):** Ao terminar a tarefa, o aluno abre um PR da sua feature para a develop. Outro membro da squad ou um **Docente** deve revisar o código antes do merge.

1. **Arquitetura de Pastas**

Para que todos saibam onde encontrar cada arquivo:

**Backend (Node.js/Express)**

- /src/models: Definição das tabelas (Postgres).
- /src/controllers: Onde a lógica de "receber requisição e enviar resposta" acontece.
- /src/services: **Onde fica a Regra de Negócio** (cálculos de escalas, lógica do e-SUS).
- /src/routes: Onde as URLs são definidas.

**Frontend (JavaScript Web)**

- /src/components: Componentes pequenos e reusáveis (inputs, botões).
- /src/pages: As telas completas do sistema.
- /src/services: Funções que fazem o fetch ou axios para o backend.
1. **Regras de Ouro de Segurança**
- **Nunca suba senhas ou chaves de API para o GitHub.** Use arquivos .env.
- **Validação Dupla:** Se você validou que o CPF está correto no Frontend, valide novamente no Backend.
- **Tratamento de Erros:** Todo acesso ao banco de dados deve estar dentro de um bloco try/catch