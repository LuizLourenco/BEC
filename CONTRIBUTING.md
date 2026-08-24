# Como Contribuir — BEC

Obrigado por considerar contribuir com o BEC! Este documento é o guia passo a passo para configurar o ambiente, escolher uma tarefa e enviar sua contribuição.

Antes de começar, dê uma olhada em:
- [VISION.md](docs/VISION.md) — para entender o que o projeto é e não é.
- [ARCHITECTURE.md](docs/ARCHITECTURE.md) — para entender as decisões técnicas.
- [AGILE_WORKFLOW.md](docs/AGILE_WORKFLOW.md) — para entender como o trabalho flui.
- [AI_GUIDELINES.md](docs/AI_GUIDELINES.md) — se for usar IA para ajudar na contribuição (comum e bem-vindo por aqui).

## Configurando o ambiente

> ⚠️ Este passo a passo será detalhado assim que o esqueleto de backend e frontend for criado no repositório. Abaixo, o previsto:

### Pré-requisitos

- Java 21+
- Maven ou Gradle (conforme definido no módulo backend)
- PostgreSQL 15+ (local ou via Docker)
- Node.js 20+ e npm/pnpm (para o frontend)
- Git

### Passos

```bash
# 1. Fork o repositório e clone o seu fork
git clone git@github.com:SEU_USUARIO/BEC.git
cd BEC

# 2. Configure o backend
cd backend
# instruções específicas serão adicionadas aqui (application.yml, banco local, etc.)

# 3. Configure o frontend
cd ../frontend
npm install
npm run dev
```

Se algum passo estiver desatualizado ou incompleto, sinta-se à vontade para abrir uma issue ou já corrigir via PR — isso também é uma contribuição válida.

## Como escolher uma tarefa

1. Acesse o [board do projeto](https://github.com/LuizLourenco/BEC) (GitHub Projects).
2. Procure issues na coluna **Ready**.
3. Se é sua primeira contribuição, procure a label **`good first issue`**.
4. Comente na issue dizendo que vai trabalhar nela, para evitar duplicidade de esforço.
5. Se a tarefa que você quer fazer não existe como issue ainda, abra uma antes de começar a codar — isso evita retrabalho e alinha expectativas.

## Fluxo de branch e commit

- Branch principal: `main` (protegida — não aceita push direto).
- Crie uma branch a partir da `main` com o padrão:
  ```
  tipo/descricao-curta
  ```
  Exemplos: `feat/cadastro-leitor`, `fix/erro-devolucao-emprestimo`, `docs/atualiza-readme`.

- Tipos recomendados (inspirados em [Conventional Commits](https://www.conventionalcommits.org/pt-br/)):
  - `feat` — nova funcionalidade
  - `fix` — correção de bug
  - `docs` — documentação
  - `refactor` — refatoração sem mudança de comportamento
  - `test` — testes
  - `chore` — tarefas de manutenção (build, dependências, etc.)

- Mensagens de commit devem seguir o mesmo padrão sempre que possível:
  ```
  feat: adiciona cadastro de leitor com validação de CPF
  fix: corrige cálculo de data de devolução de empréstimo
  ```

## Como abrir um Pull Request (PR)

1. Certifique-se de que sua branch está atualizada com a `main`.
2. Abra o PR usando o [template padrão](./.github/PULL_REQUEST_TEMPLATE.md) — ele será preenchido automaticamente.
3. Vincule o PR à issue correspondente (ex: `Closes #12`).
4. Se usou IA de forma significativa na implementação, aplique a label `ai-assisted` e siga as orientações de transparência do [AI_GUIDELINES.md](docs/AI_GUIDELINES.md).
5. Garanta que o PR atende à [Definition of Done](docs/AGILE_WORKFLOW.md#definition-of-done-dod) antes de marcar como pronto para revisão.

## Como funciona a revisão

- Todo PR precisa de **pelo menos uma aprovação humana** antes do merge — mesmo em um projeto pequeno com um único mantenedor, isso significa que o mantenedor revisa antes de mergear o próprio PR quando possível, ou aguarda revisão de outro colaborador quando disponível.
- O que se espera de uma revisão:
  - O código resolve o problema descrito na issue?
  - Há testes cobrindo o comportamento novo/alterado?
  - O código é razoavelmente legível e segue os padrões do restante do projeto?
  - Não há segredos, chaves ou dados pessoais reais expostos no código ou em exemplos.
- Feedback de revisão deve ser construtivo e específico. Este é um projeto voluntário — trate quem contribui com paciência e respeito (ver [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)).

## Dúvidas?

Abra uma issue com a label `needs discussion` ou entre em contato com o mantenedor via GitHub ([@LuizLourenco](https://github.com/LuizLourenco)).
