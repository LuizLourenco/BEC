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
- Banco de dados relacional único (sem multi-tenancy inicial — ver VISION.md).

## Decisões registradas

Ver [`docs/decisions/`](decisions/README.md) para a lista completa e o histórico de cada uma.

## Decisões em aberto

As decisões abaixo ainda não foram tomadas e devem ser resolvidas antes ou durante o início da implementação correspondente:

- [x] Modelagem de dados detalhada (acervo, leitores, empréstimos) — ver [`docs/DATA_MODEL.md`](DATA_MODEL.md). A especificação inicial está concluída e deve evoluir junto das migrações e dos testes de domínio.
- [x] Estratégia de autorização e auditoria — ver [ADR-009](decisions/0009-autorizacao-e-auditoria.md). Deve ser implementada antes de expor operações administrativas.

> Novas decisões técnicas relevantes devem ser adicionadas como um novo ADR em `docs/decisions/`, seguindo o mesmo formato — ver [`docs/decisions/README.md`](decisions/README.md).



