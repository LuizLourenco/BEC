# Decisões técnicas (ADRs)

Cada decisão relevante é registrada como um ADR (*Architecture Decision Record*) simplificado: contexto, decisão, alternativas consideradas e consequências — para que voluntários futuros (e o próprio mantenedor, meses depois) entendam o raciocínio, não só a escolha.

| ADR | Título | Status |
|---|---|---|
| [0001](0001-aplicacao-web.md) | Aplicação Web (não desktop, não mobile nativo) | decidido |
| [0002](0002-backend-spring-postgresql.md) | Backend com Spring + PostgreSQL | decidido |
| [0003](0003-frontend-pwa.md) | Frontend Progressive Web App (PWA) | decidido |
| [0004](0004-frontend-vue-vite.md) | Framework de frontend — Vue 3 + Vite | decidido |
| [0005](0005-licenca-mit.md) | Licença MIT | decidido |
| [0006](0006-autenticacao-sessao.md) | Autenticação/autorização — Sessão tradicional | decidido |
| [0007](0007-deploy-docker-compose.md) | Estratégia de deploy — Docker Compose + Oracle Cloud | decidido |
| [0008](0008-pwa-offline.md) | Suporte offline no PWA — leitura do acervo | decidido |

## Como criar um novo ADR

1. Copie o próximo número sequencial (4 dígitos) + um título curto em kebab-case, ex: `0009-modelagem-de-dados.md`.
2. Siga o formato: Contexto, Decisão, Alternativas consideradas, Consequências, Status.
3. Adicione a linha correspondente na tabela acima.
