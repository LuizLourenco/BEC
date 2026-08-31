# ADR-008: Suporte offline no PWA — leitura do acervo, sem empréstimo offline

**Contexto:** o PWA (Vue 3 + Vite, ADR-004) pode oferecer diferentes níveis de suporte offline, do mais simples (cache de leitura do acervo) ao mais complexo (registrar empréstimo offline com sincronização e resolução de conflitos posterior). Mantido por um mantenedor solo, é preciso limitar o escopo para algo implementável com segurança.

**Decisão:** suporte offline limitado a leitura do acervo — catálogo e disponibilidade ficam acessíveis offline.

Para garantir que a consulta funcione de forma consistente e segura, sem falsas expectativas de disponibilidade ou falhas na busca offline, adotaremos as seguintes diretrizes:

1. **Estratégia de Sincronização por Índice Minimalista:** Em vez de tentar cachear respostas dinâmicas de requisições de busca HTTP via Service Worker (estratégia que falharia para termos nunca antes pesquisados), o backend fornecerá um endpoint para download de um índice compactado e simplificado do catálogo (ex: formato JSON unificado contendo apenas `id`, `titulo`, `autor`, `situacao_exemplar` e `localizacao`). O frontend armazenará esse índice localmente (via IndexedDB/localStorage) para viabilizar buscas 100% offline.
2. **Indicador Visual de Sincronização (UX):** A interface do catálogo offline exibirá obrigatoriamente um aviso visual proeminente com o carimbo de data/hora (timestamp) da última sincronização bem-sucedida (ex: *"Visualizando catálogo offline sincronizado há 2 horas. A disponibilidade física real pode divergir"*).
3. **Escrita Bloqueada:** Ações que alteram dados (registrar empréstimo, devolução, cadastro) continuam exigindo conexão ativa com a internet. Se o usuário estiver offline, tais botões/ações serão desabilitados visualmente com mensagens explicativas claras, sem filas de sincronização em background neste momento.

**Alternativas consideradas:**
- **Cache de requisições REST dinâmicas (stale-while-revalidate puro):** descartada — buscas offline falhariam para qualquer palavra-chave que o usuário ainda não tivesse pesquisado enquanto online, gerando uma experiência frustrante e inconsistente.
- **Empréstimo offline com sincronização posterior:** descartada por ora — exige fila de sincronização (IndexedDB + background sync) e resolução de conflitos (ex: dois voluntários offline registram o mesmo livro emprestado a pessoas diferentes). Complexidade e risco de dado inconsistente incompatíveis com mantenedor solo neste momento.
- **Sem suporte offline algum:** descartada — perde o principal benefício de ser PWA (consultar o acervo mesmo com internet instável, comum em sedes de instituições).

**Consequências:** cobre o caso de uso mais comum (consultar se um livro existe e onde fica) sem exigir lógica de conflito; a busca funciona perfeitamente mesmo sem internet para todo o catálogo; a disponibilidade do acervo pode estar ligeiramente desatualizada, mas o usuário final é devidamente alertado pelo indicador de sincronização da interface; empréstimo offline com sincronização fica como evolução futura caso haja real demanda e braço técnico para implementação segura.

**Status:** decidido — sujeito a revisão se houver relatos frequentes de instituições sem conexão estável no momento do empréstimo.

---

**Nota de revisão (2026-08-23):** esta versão substitui a estratégia anterior baseada em cache de requisições HTTP via service worker (Workbox, stale-while-revalidate), que cobria apenas buscas já realizadas enquanto online. A nova estratégia de índice minimalista sincronizado resolve essa lacuna, permitindo busca offline em qualquer termo do catálogo. Compatível com as referências existentes em `ARCHITECTURE.md`/`ROADMAP.md` (M4 — "cache do catálogo conforme ADR-008"); nenhuma outra alteração de escopo foi necessária. Relevante caso a implementação de M4 já tenha começado com a estratégia antiga.