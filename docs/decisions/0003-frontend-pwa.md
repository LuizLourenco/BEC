# ADR-003: Frontend Progressive Web App (PWA)

**Contexto:** o software deve ser leve, funcionar bem em hardware antigo, ser instalável como app (facilitando o uso por bibliotecários) e, idealmente, tolerar conexões instáveis.

**Decisão:** construir o frontend como **PWA** (Progressive Web App) — instalável, com service worker para cache de assets e suporte a operações offline básicas de leitura do acervo (ver ADR-008 para o escopo detalhado).

**Consequências:** exige atenção a estratégias de cache e versionamento de service worker; navegadores muito antigos podem ter suporte parcial a recursos de PWA (aceitável — degradação graciosa para "só site normal").

**Status:** decidido.
