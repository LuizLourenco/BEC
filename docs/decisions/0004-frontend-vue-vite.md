# ADR-004: Framework de frontend — Vue 3 + Vite

**Contexto:** entre os frameworks candidatos para construir a PWA (React, Vue, Angular), é preciso escolher um que equilibre: leveza (bundle final pequeno, bom para hardware modesto), curva de aprendizado suave (importante para atrair voluntários de perfis variados) e bom suporte a PWA.

**Decisão:** usar **Vue 3** com **Vite** como bundler, e **`vite-plugin-pwa`** para a camada de PWA (manifest + service worker).

**Alternativas consideradas:**
- **React + Vite:** ecossistema enorme e também viável, mas exige mais decisões acessórias (roteador, gerenciamento de estado, etc. não vêm "oficiais"), aumentando a superfície de decisões para um projeto voluntário pequeno.
- **Angular:** framework robusto e completo, mas mais pesado (bundle maior) e mais opinativo/verboso — maior barreira de entrada para colaboradores iniciantes e menos alinhado ao objetivo de leveza.
- **Svelte/SvelteKit:** considerado por gerar bundles muito pequenos, mas descartado por ora por ter ecossistema/comunidade menor no Brasil, o que pode dificultar encontrar voluntários com experiência prévia.

**Consequências:** o ecossistema oficial do Vue (Vue Router, Pinia) cobre roteamento e estado sem exigir escolhas adicionais; a comunidade Vue no Brasil é relevante, o que facilita encontrar contribuidores.

**Status:** decidido — sujeito a revisão caso surjam limitações práticas.
