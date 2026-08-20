# Arquitetura e Decisões Técnicas — BEC

Este documento registra as decisões técnicas do projeto e, principalmente, **o porquê** delas — para que voluntários futuros (e o próprio mantenedor, meses depois) entendam o raciocínio, não só a escolha.

Cada decisão relevante é registrada como um ADR (*Architecture Decision Record*) simplificado: contexto, decisão, alternativas consideradas e consequências.

---

## Visão geral da arquitetura

```
┌─────────────────────┐        HTTPS/REST/JSON        ┌──────────────────────┐
│   Frontend (PWA)     │ ─────────────────────────────▶│   Backend (Spring)    │
│   Vue 3 + Vite        │◀───────────────────────────── │   API REST            │
└─────────────────────┘                                └──────────┬───────────┘
                                                                    │
                                                                    ▼
                                                          ┌──────────────────┐
                                                          │   PostgreSQL       │
                                                          └──────────────────┘
```

- Aplicação web servida como PWA (instalável, funciona offline em partes críticas quando possível).
- Backend expõe API REST consumida pelo frontend.
- Banco de dados relacional único (sem multi-tenancy inicial — ver VISION.md).

---

## ADR-001: Aplicação Web (não desktop, não mobile nativo)

**Contexto:** o software precisa alcançar instituições com hardware variado (PCs antigos, tablets, celulares) e minimizar esforço de instalação/manutenção.

**Decisão:** construir uma aplicação **Web**, acessível via navegador, sem necessidade de instalação de software nativo.

**Alternativas consideradas:**
- App desktop (Electron, JavaFX): rejeitado — exige instalação/atualização manual em cada máquina, contraria o objetivo de baixo custo operacional.
- App mobile nativo (Android/iOS): rejeitado como abordagem principal — exigiria manter duas bases de código e a operação de biblioteca é predominantemente feita em computador.

**Consequências:** dependemos de boa performance em navegadores antigos/hardware modesto; reforça a necessidade de um frontend leve (ver ADR-004).

---

## ADR-002: Backend com Spring + PostgreSQL

**Contexto:** precisamos de um backend robusto, com ecossistema maduro, boa documentação e que suporte bem controle de acesso, persistência relacional e testes — mas sem exigir infraestrutura pesada para rodar.

**Decisão:** usar **Spring (Java)** para o backend e **PostgreSQL** como banco de dados.

**Alternativas consideradas:**
- Node.js (Express/NestJS): ecossistema também maduro, porém a escolha por Spring reflete familiaridade da equipe mantenedora e o desejo de um ecossistema fortemente tipado e com Spring Data JPA facilitando modelagem relacional.
- MySQL/MariaDB no lugar de Postgres: rejeitado — Postgres tem melhor suporte a tipos avançados (ex: full-text search nativo, útil para busca de acervo) e é gratuito, robusto e amplamente hospedável em VPS simples.
- Banco NoSQL (MongoDB): rejeitado — o domínio (acervo, leitores, empréstimos) é fortemente relacional; SQL é a modelagem natural.

**Consequências:** exige JVM em produção (consumo de memória deve ser monitorado em instâncias muito modestas — considerar tuning de heap); Postgres é uma dependência de infraestrutura adicional (mitigada pela ampla disponibilidade em provedores de baixo custo e via Docker).

**Status:** decidido.

---

## ADR-003: Frontend Progressive Web App (PWA)

**Contexto:** o software deve ser leve, funcionar bem em hardware antigo, ser instalável como app (facilitando o uso por bibliotecários) e, idealmente, tolerar conexões instáveis.

**Decisão:** construir o frontend como **PWA** (Progressive Web App) — instalável, com service worker para cache de assets e, no futuro, suporte a operações offline básicas (ex: consulta a acervo).

**Consequências:** exige atenção a estratégias de cache e versionamento de service worker; navegadores muito antigos podem ter suporte parcial a recursos de PWA (aceitável — degradação graciosa para "só site normal").

**Status:** decidido.

---

## ADR-004: Framework de frontend — Vue 3 + Vite

**Contexto:** entre os frameworks candidatos para construir a PWA (React, Vue, Angular), é preciso escolher um que equilibre: leveza (bundle final pequeno, bom para hardware modesto), curva de aprendizado suave (importante para atrair voluntários de perfis variados) e bom suporte a PWA.

**Decisão:** usar **Vue 3** com **Vite** como bundler, e **`vite-plugin-pwa`** para a camada de PWA (manifest + service worker).

**Alternativas consideradas:**
- **React + Vite:** ecossistema enorme e também viável, mas exige mais decisões acessórias (roteador, gerenciamento de estado, etc. não vêm "oficiais"), aumentando a superfície de decisões para um projeto voluntário pequeno.
- **Angular:** framework robusto e completo, mas mais pesado (bundle maior) e mais opinativo/verboso — maior barreira de entrada para colaboradores iniciantes e menos alinhado ao objetivo de leveza.
- **Svelte/SvelteKit:** considerado por gerar bundles muito pequenos, mas descartado por ora por ter ecossistema/comunidade menor no Brasil, o que pode dificultar encontrar voluntários com experiência prévia.

**Consequências:** o ecossistema oficial do Vue (Vue Router, Pinia) cobre roteamento e estado sem exigir escolhas adicionais; a comunidade Vue no Brasil é relevante, o que facilita encontrar contribuidores.

**Status:** decidido — sujeito a revisão caso surjam limitações práticas.

---

## ADR-005: Licença MIT

**Contexto:** o projeto pretende receber contribuições externas de voluntários de diferentes instituições espíritas.

**Decisão:** licenciar o código sob **MIT**, permitindo uso, modificação e redistribuição ampla — inclusive por outras instituições que queiram adaptar o software às suas necessidades.

**Alternativas consideradas:**
- AGPL-3.0: consideraria mais "protetora" (obrigaria quem hospedar versões modificadas a também abrir o código), mas foi descartada por adicionar complexidade de compliance que pode afastar instituições leigas em software livre.

**Status:** decidido.

---

## Decisões em aberto

As decisões abaixo ainda não foram tomadas e devem ser resolvidas antes ou durante o início da implementação correspondente:

- [ ] Estratégia de autenticação/autorização (ex: Spring Security + JWT vs. sessão tradicional).
- [ ] Estratégia de deploy (Docker Compose simples? CI/CD? Qual provedor de nuvem recomendado na documentação?).
- [ ] Modelagem de dados detalhada (acervo, leitores, empréstimos) — deve virar um documento próprio em `docs/decisions/` ou um diagrama ER quando amadurecer.
- [ ] Suporte offline no PWA: até onde vai (só leitura de acervo? registro de empréstimo offline com sincronização posterior?).

> Novas decisões técnicas relevantes devem ser adicionadas a este arquivo como um novo ADR, seguindo o mesmo formato.
