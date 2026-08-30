# Arquitetura e Decisões Técnicas — BEC

Este documento dá a visão geral da arquitetura do projeto. As decisões técnicas em si — com contexto, alternativas consideradas e consequências — ficam registradas individualmente em [`docs/decisions/`](decisions/README.md).

---

## Visão geral da arquitetura

```
┌─────────────────────┐        HTTPS/REST/JSON        ┌──────────────────────┐
│   Frontend (PWA)    │ ─────────────────────────────▶│   Backend (Spring)   │
│   Vue 3 + Vite      │◀───────────────────────────── │   API REST           │
└─────────────────────┘                               └──────────┬───────────┘
                                                                 │
                                                                 ▼
                                                        ┌──────────────────┐
                                                        │   PostgreSQL     │
                                                        └──────────────────┘
```

- Aplicação web servida como PWA (instalável, funciona offline em partes críticas — ver [ADR-008](decisions/0008-pwa-offline.md)).
- Backend expõe API REST consumida pelo frontend.
- Banco de dados relacional único (sem multi-tenancy entre instituições — ver VISION.md); uma instituição pode conter mais de uma biblioteca dentro da mesma instalação (`Instituicao` 1 — 1..* `Biblioteca` em `DATA_MODEL.md`).

## Decisões registradas

Ver [`docs/decisions/`](decisions/README.md) para a lista completa e o histórico de cada uma.

## Decisões em aberto

As decisões abaixo ainda não foram tomadas e devem ser resolvidas antes ou durante o início da implementação correspondente:

- [x] Modelagem de dados detalhada (acervo, leitores, empréstimos) — ver [`docs/DATA_MODEL.md`](DATA_MODEL.md). Passou por revisão externa e é a fonte de verdade conceitual do domínio; segue como diagrama de classes (não esquema físico) e deve evoluir junto das migrações e dos testes de domínio.
- [x] Estratégia de autorização e auditoria — ver [ADR-009](decisions/0009-autorizacao-e-auditoria.md), consistente com o modelo `Usuario`–`Papel`–`Permissao` (muitos-para-muitos nos dois relacionamentos) definido em `DATA_MODEL.md`; `Papel` é tabela por normalização, mas novo papel exige alteração de código — não é configurável em runtime. Deve ser implementada antes de expor operações administrativas.
- [x] **Reconciliação com o diagrama de classes (`DATA_MODEL.md` como fonte fixa):** `ADR-009` e `ROADMAP.md` citavam `CATEGORIA_USUARIO`/`REGRA_EMPRESTIMO`, entidades que não existem no diagrama — ambos foram revisados para não presumir uma entidade inexistente, mantendo o princípio de segurança (classificação de circulação, se um dia existir, nunca autoriza acesso de sistema) sem afirmar que ela já está modelada. `EVENTO_AUDITORIA`, a alocação de exemplar em `RESERVA` e a máquina de estados de circulação **não** são lacunas do diagrama — o próprio `DATA_MODEL.md` declara que prazo, renovação, fila de reserva, auditoria, retenção e concorrência são regras operacionais propositalmente fora do diagrama, a decidir à parte; é exatamente o que as issues [001](issue-drafts/001-auditoria-imutavel-e-minimizacao-lgpd.md), [002](issue-drafts/002-maquina-de-estados-e-concorrencia-de-circulacao.md) e [003](issue-drafts/003-reserva-por-obra-com-alocacao-de-exemplar.md) fazem.
- [ ] **Proposta ainda em aberto (não é inconsistência):** a issue [004](issue-drafts/004-separar-usuario-operador-de-leitor.md) propõe `USUARIO.leitor_id` como FK opcional e única para `LEITOR` — o diagrama atual não tem essa relação (`Usuario` e `Leitor` são hoje totalmente independentes, o que já satisfaz o objetivo central da issue de não obrigar operador a ser leitor). Falta decidir se o vínculo opcional deve ser adicionado ao modelo antes das migrações de identidade.

> Novas decisões técnicas relevantes devem ser adicionadas como um novo ADR em `docs/decisions/`, seguindo o mesmo formato — ver [`docs/decisions/README.md`](decisions/README.md).



