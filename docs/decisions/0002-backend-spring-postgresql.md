# ADR-002: Backend com Spring + PostgreSQL

**Contexto:** precisamos de um backend robusto, com ecossistema maduro, boa documentação e que suporte bem controle de acesso, persistência relacional e testes — mas sem exigir infraestrutura pesada para rodar.

**Decisão:** usar **Spring (Java)** para o backend e **PostgreSQL** como banco de dados.

**Alternativas consideradas:**
- Node.js (Express/NestJS): ecossistema também maduro, porém a escolha por Spring reflete familiaridade da equipe mantenedora e o desejo de um ecossistema fortemente tipado e com Spring Data JPA facilitando modelagem relacional.
- MySQL/MariaDB no lugar de Postgres: rejeitado — Postgres tem melhor suporte a tipos avançados (ex: full-text search nativo, útil para busca de acervo) e é gratuito, robusto e amplamente hospedável em VPS simples.
- Banco NoSQL (MongoDB): rejeitado — o domínio (acervo, leitores, empréstimos) é fortemente relacional; SQL é a modelagem natural. E MongoDB costuma exigir recursos de memória para um bom funcionamento, o que contraria as premissas iniciais.

**Consequências:** exige JVM em produção (consumo de memória deve ser monitorado em instâncias muito modestas — considerar tuning de heap); Postgres é uma dependência de infraestrutura adicional (mitigada pela ampla disponibilidade em provedores de baixo custo e via Docker).

**Status:** decidido.
