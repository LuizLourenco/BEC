# Fluxo de Trabalho — BEC

Este documento descreve como o trabalho é organizado no projeto: onde as tarefas vivem, como são dimensionadas, qual o papel da IA no fluxo e qual a cadência de trabalho.

## Board

Usamos **GitHub Projects** (view Kanban) vinculado ao repositório [`LuizLourenco/BEC`](https://github.com/LuizLourenco/BEC).

Colunas padrão:

| Coluna | Significado |
|---|---|
| **Backlog** | Ideia/tarefa identificada, ainda não refinada |
| **Ready** | Tarefa refinada, com critérios de aceite claros, pronta para ser pega |
| **In Progress** | Alguém está trabalhando ativamente |
| **In Review** | PR aberto, aguardando revisão |
| **Done** | Mergeado e validado |

Toda issue nova deve, no mínimo, ter um título claro e uma descrição do problema/necessidade. Issues em **Ready** devem ter critérios de aceite explícitos.

## Cadência: Kanban contínuo

O projeto **não usa sprints**. Para um time pequeno/voluntário com disponibilidade variável, sprints com prazo fixo tendem a gerar pressão artificial e desalinhamento. Optamos por **kanban contínuo**:

- Tarefas fluem conforme disponibilidade de quem contribui.
- Prioridade é sinalizada por labels (`priority: alta/média/baixa`) e pela posição na coluna.
- Não há "fechamento de sprint" — o progresso é visível continuamente no board.

Isso pode ser revisto se o time crescer e precisar de cadência mais estruturada.

## Tamanho de tarefas

- Issues devem ser **pequenas o suficiente para caber em um PR revisável** (idealmente, algo que dá para revisar em até ~20-30 minutos).
- Tarefas grandes (ex: "implementar módulo de empréstimos") devem ser quebradas em issues menores antes de irem para **Ready** (ex: "modelar entidade Empréstimo", "endpoint de criar empréstimo", "tela de registrar empréstimo").
- Se uma issue está em progresso há muito tempo sem PR, é sinal de que deveria ter sido quebrada — vale reabrir a discussão.

## Definition of Done (DoD)

Uma tarefa só é considerada concluída quando:

- [ ] O código atende aos critérios de aceite descritos na issue.
- [ ] Há testes automatizados cobrindo o comportamento novo/alterado (quando aplicável).
- [ ] O código passou por **revisão humana** e foi aprovado (ver [CONTRIBUTING.md](CONTRIBUTING.md)).
- [ ] Documentação relevante foi atualizada (README, ARCHITECTURE, comentários de API, etc.), se a mudança impactar algum desses.
- [ ] O PR foi mergeado na branch principal sem quebrar o build/CI.

## Papel da IA no fluxo

A IA (ex: Claude Code, GitHub Copilot) é uma colaboradora ativa no projeto, mas **nunca substitui a revisão humana**. O fluxo recomendado:

1. **Geração de rascunho de issue:** a IA pode ajudar a redigir/detalhar uma issue a partir de uma ideia solta (ex: "preciso de uma tela de cadastro de leitor" → IA ajuda a estruturar critérios de aceite).
2. **Revisão humana da issue:** antes de ir para **Ready**, a pessoa responsável revisa e ajusta o que a IA sugeriu.
3. **Implementação assistida por IA:** o contribuidor pode usar IA para gerar/apoiar a implementação do código.
4. **Revisão humana obrigatória antes do merge:** todo PR — gerado com ou sem apoio de IA — passa por revisão humana. Não há merge automático de código gerado por IA sem revisão.

Regras detalhadas sobre uso de IA (o que pode ser autônomo, cuidados com segurança, transparência) estão em [AI_GUIDELINES.md](AI_GUIDELINES.md).

## Labels usadas

| Label | Uso |
|---|---|
| `good first issue` | Boa entrada para novos colaboradores |
| `bug` | Comportamento incorreto |
| `enhancement` | Nova funcionalidade ou melhoria |
| `documentation` | Mudanças de documentação |
| `priority: alta/média/baixa` | Prioridade relativa |
| `needs discussion` | Requer alinhamento antes de virar trabalho |
| `ai-assisted` | PR/issue com apoio relevante de IA (ver AI_GUIDELINES.md) |

## Revisão deste processo

Este documento deve ser revisitado periodicamente (sugestão: a cada poucas semanas ou quando o time mudar de tamanho) e simplificado se algo aqui não estiver sendo usado na prática.
