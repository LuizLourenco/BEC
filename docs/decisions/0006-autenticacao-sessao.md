# ADR-006: Estratégia de autenticação/autorização — Sessão tradicional (Spring Security)

**Contexto:** o BAC terá um frontend SPA/PWA (Vue 3 + Vite, conforme ADR-004) consumindo um backend Spring, com uma instância implantada por instituição espírita, mantido por uma única pessoa neste momento. É preciso equilibrar segurança e simplicidade de implementação inicial.

**Decisão:** usar Spring Security com sessão tradicional (cookie `HttpOnly`, `Secure`, `SameSite=Lax`).

**Alternativas consideradas:**
- **JWT (access + refresh token):** permite escalar sem estado no servidor e desacopla melhor múltiplos clientes. Descartada por ora: exige implementar corretamente expiração, refresh, armazenamento seguro no cliente e revogação — várias peças de segurança para acertar sozinho, sem revisão de código por terceiros.
- **JWT sem refresh token:** mais simples, mas sem revogação de acesso antes da expiração — inaceitável para remover acesso de um usuário quando necessário.

**Consequências:** menos código de segurança escrito à mão (CSRF e revogação já resolvidos pelo Spring Security); exige estado de sessão no servidor, o que é aceitável na escala atual (uma instância por instituição). Migrar para JWT fica em aberto para quando houver app mobile, modelo multi-tenant, ou uma equipe maior revisando o código.

**Status:** decidido — sujeito a revisão nesses cenários futuros.
