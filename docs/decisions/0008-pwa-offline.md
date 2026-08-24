# ADR-008: Suporte offline no PWA — leitura do acervo, sem empréstimo offline

**Contexto:** o PWA (Vue 3 + Vite, ADR-004) pode oferecer diferentes níveis de suporte offline, do mais simples (cache de leitura do acervo) ao mais complexo (registrar empréstimo offline com sincronização e resolução de conflitos posterior). Mantido por um mantenedor solo, é preciso limitar o escopo para algo implementável com segurança.

**Decisão:** suporte offline limitado a leitura do acervo — catálogo (livros, disponibilidade na última sincronização) fica em cache via service worker (`vite-plugin-pwa`/Workbox, estratégia stale-while-revalidate) e pode ser consultado sem internet. Ações que alteram dado (registrar empréstimo, devolução, cadastro) exigem conexão; se o usuário estiver offline, a ação fica desabilitada com aviso claro.

**Alternativas consideradas:**
- **Empréstimo offline com sincronização posterior:** descartada por ora — exige fila de sincronização (IndexedDB + background sync) e resolução de conflitos (ex: dois voluntários offline registram o mesmo livro emprestado a pessoas diferentes). Complexidade e risco de dado inconsistente incompatíveis com mantenedor solo neste momento.
- **Sem suporte offline algum:** descartada — perde o principal benefício de ser PWA (consultar o acervo mesmo com internet instável, comum em sedes de instituições).

**Consequências:** cobre o caso de uso mais comum (consultar se um livro está disponível) sem exigir lógica de conflito; disponibilidade do acervo em cache pode estar desatualizada até a próxima sincronização (aceitável, já que é só leitura); empréstimo offline com sync fica como evolução futura caso haja demanda real e tempo do mantenedor para implementar com cuidado.

**Status:** decidido — sujeito a revisão se houver relatos frequentes de instituições sem conexão estável no momento do empréstimo.
