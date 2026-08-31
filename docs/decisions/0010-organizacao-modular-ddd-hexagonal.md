# ADR-0010: Organização modular — DDD tático seletivo, Clean Architecture como princípio, Ports & Adapters como estrutura

**Contexto:** o backend (Spring + PostgreSQL, ADR-002) cresce por funcionalidade (catálogo, circulação, identidade/acesso) e precisa de uma organização de pacotes que mantenha regra de negócio isolada de infraestrutura, seja testável sem subir Spring/banco para a maior parte dos casos, e continue compreensível para voluntários externos que nunca viram o código — tudo isso sustentável por um mantenedor solo (ADR-007). DDD, Clean Architecture e Ports & Adapters descrevem essencialmente a mesma forma (núcleo isolado, cercado por contratos, cercado por infraestrutura), mas com vocabulário e nível de compromisso diferentes; adotar qualquer uma das três por completo, sem dosagem, tende a pesar mais do que o projeto suporta ou a ficar implícito demais para ser reconhecível no código.

**Decisão:** monólito modular — um único deploy (compatível com ADR-007), dividido internamente em um pacote por contexto delimitado: `catalogo`, `circulacao`, `identidade` e `shared` (código comum entre contextos). Dentro de cada contexto, a mesma estrutura de quatro camadas se repete:

- `domain/`: entidades, agregados e value objects — sem nenhum import de Spring, JPA ou HTTP.
- `application/port/in/`: interfaces que qualquer adapter de entrada deve respeitar para acionar um caso de uso (ex: `RegistrarEmprestimoUseCase`).
- `application/port/out/`: interfaces que o núcleo exige do mundo externo (ex: `EmprestimoRepositorio`, `AuditoriaDeCirculacaoPort`).
- `application/service/`: implementa as portas de entrada, orquestra o domínio e usa as portas de saída.
- `infrastructure/adapter/in/web/`: controllers REST — só conhecem a porta de entrada, nunca o serviço concreto.
- `infrastructure/adapter/out/persistence/`: implementações JPA das portas de saída.

A dosagem das três abordagens:

1. **Ports & Adapters (hexagonal) é a estrutura de pastas em si** — `port/in`, `port/out`, `adapter/in`, `adapter/out` explícitos. É o que torna a regra de dependência visível e enforçável no código, não apenas uma intenção na cabeça de quem escreveu.
2. **Clean Architecture entra como princípio** (a dependência sempre aponta para dentro do domínio), não como os quatro anéis completos de Uncle Bob — não se adota uma camada de Presenters separada de Controllers; DTO + `@RestController` do Spring já resolve essa tradução sem ganho em criar uma classe a mais.
3. **DDD entra em dois níveis diferentes.** Estratégico (bounded contexts com linguagem ubíqua) é adotado por completo — é o que já define os três pacotes acima e não tem custo real. Tático (agregados, invariantes protegidas no construtor/métodos de domínio) é adotado **seletivamente**, só onde existe uma invariante de negócio real a proteger: `Emprestimo` (um exemplar não pode ter dois empréstimos ativos) e `Usuario` (gestão de papel não pode conceder ao próprio ator privilégio superior, ADR-009). Entidades sem invariante relevante — como `Livro`/`Exemplar` — não precisam de agregado formal; um service de aplicação direto com DTOs já resolve, sem anemia nem cerimônia desnecessária.

Comunicação entre contextos acontece só por porta: quando `circulacao` precisa registrar um evento de auditoria (ADR-009), ela declara `AuditoriaDeCirculacaoPort` em `circulacao/application/port/out`; quem implementa de verdade é um adapter dentro de `identidade/infrastructure/adapter/out`. Nenhum contexto importa pacote de infraestrutura de outro.

**Alternativas consideradas:**

- **DDD tático completo em todas as entidades** (agregados, value objects e domain events para cada conceito do domínio): descartada — a maioria das entidades do catálogo não tem invariante que justifique essa cerimônia; aplicar em tudo tornaria o código mais difícil de entender para voluntários, não mais fácil.
- **Clean Architecture com os quatro anéis completos** (Entities/Use Cases/Interface Adapters — com Controllers, Presenters e Gateways como papéis distintos — /Frameworks & Drivers): descartada — Presenters separados de Controllers adicionam uma camada de tradução que o par DTO + `@RestController` do Spring já cobre.
- **Arquitetura em camadas tradicional** (`Controller → Service → Repository`, sem portas explícitas — o service injeta `JpaRepository` diretamente): descartada — sem interface entre o service e a infraestrutura, o domínio depende de detalhe de persistência, dificultando teste unitário puro e qualquer troca futura de tecnologia de dados.
- **Um microsserviço por contexto delimitado**: descartada — incompatível com mantenedor solo e com a estratégia de deploy único do ADR-007; o isolamento por pacote dentro de um monólito já entrega a maior parte do benefício de fronteira, sem o custo operacional de rede, versionamento de API entre serviços e infraestrutura distribuída.

**Consequências:** cada caso de uso ganha mais um arquivo (a interface da porta de entrada) além do serviço que a implementa, mas em troca a fronteira entre entrada, núcleo e saída fica testável sem subir Spring nem banco — a maior parte da suíte de testes roda em milissegundos, e só os adapters (persistência, controllers) pagam o custo de teste de integração com Testcontainers. A estrutura de pastas comunica o papel de cada classe por si só, o que reduz a curva de entrada para voluntários que nunca viram o projeto. Se já existir código organizado de outra forma, a migração deve ser incremental — funcionalidade nova já nasce no novo formato; código existente é reorganizado quando for tocado, não em um esforço único de reescrita.

**Status:** decidido.

## Referência complementar

O guia visual com o exemplo completo (árvore de pastas, código de cada camada do contexto de Circulação, tabela de tradução de vocabulário entre DDD/Clean Architecture/Hexagonal e o diagrama de fluxo de uma requisição) está publicado como artefato interativo do projeto e deve ser tratado como material de apoio a este ADR, não como decisão em si.
