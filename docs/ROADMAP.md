# Roadmap do BEC

Este roadmap é um **modelo de planejamento**, não um cronograma nem uma promessa de entrega. Os marcos indicam uma ordem de redução de risco: cada um só avança quando seus critérios de saída estiverem atendidos e as decisões de produto correspondentes estiverem confirmadas.

## Como usar

1. Transforme cada resultado abaixo em issues pequenas, com problema, critérios de aceite e prioridade.
2. Mova para **Ready** somente itens refinados e sem decisão de negócio bloqueadora.
3. Considere um marco concluído quando todos os seus critérios de saída forem atendidos, não quando apenas suas telas existirem.
4. Atualize este documento quando o escopo, a ordem ou uma decisão arquitetural relevante mudar.

## Visão dos marcos

| Marco | Resultado esperado | Estado inicial |
|---|---|---|
| M0 — Fundação técnica | Projeto executável, testável e reproduzível localmente | Planejado |
| M1 — Identidade e segurança | Acesso administrativo seguro e auditável | Planejado |
| M2 — Catálogo | Acervo e exemplares cadastráveis e consultáveis | Planejado |
| M3 — Circulação | Empréstimos, devoluções, renovações e reservas confiáveis | Planejado |
| M4 — Experiência PWA | Uso instalável e consulta limitada offline | Planejado |
| M5 — Operação e primeira versão | Deploy, backup e suporte mínimos para uso real | Planejado |

## M0 — Fundação técnica

**Objetivo:** criar uma base que qualquer colaborador consiga executar e verificar.

- Inicializar módulos backend e frontend conforme os ADRs vigentes.
- Configurar Docker Compose para desenvolvimento, variáveis de ambiente de exemplo e banco PostgreSQL local.
- Criar migrações versionadas e pipeline de build/testes no CI.
- Documentar setup, execução dos testes e convenções de contribuição que já estejam em uso.

**Critérios de saída:** clone limpo sobe localmente sem segredos versionados; CI executa build e testes; as migrações podem ser aplicadas desde um banco vazio em ambiente local; o README contém instruções executáveis.

## M1 — Identidade e segurança

**Objetivo:** permitir que operadores autorizados usem o sistema sem misturar acesso administrativo e categoria de leitor.

- Implementar usuário, sessão e recuperação de senha conforme ADR-006.
- Implementar papéis, permissões e auditoria conforme ADR-009.
- Validar entradas, tratamento explícito de falhas e controles de acesso no backend.
- Cobrir autenticação, revogação de acesso e negação de permissão com testes.

**Critérios de saída:** conta inativa não acessa a API; uma ação sem permissão é negada no backend; operações administrativas geram auditoria sem dados sensíveis; as políticas de privacidade e backup têm dono institucional definido.

## M2 — Catálogo

**Objetivo:** catalogar e localizar obras e cópias físicas com dados consistentes.

- Implementar o recorte inicial de obras, autores, editoras, classificações e exemplares definido no modelo de dados.
- Cadastrar, editar, inativar e pesquisar acervo com validação de unicidade.
- Priorizar busca por título, autor, classificação e código de barras/patrimônio.

**Critérios de saída:** um bibliotecário cadastra obra e exemplares sem duplicar identificadores; a busca encontra o acervo pelos critérios definidos; regras e restrições do modelo possuem testes de domínio e integração.

## M3 — Circulação

**Objetivo:** controlar o ciclo de empréstimo sem perder histórico ou permitir estados inválidos.

- Implementar categorias de leitor e regras de empréstimo por tipo de item.
- Implementar empréstimo, devolução, renovação auditável e cálculo de prazo.
- Implementar reservas FIFO, disponibilidade e expiração de retirada.
- Exibir pendências e disponibilidade atual do exemplar/obra.

**Critérios de saída:** o sistema bloqueia exemplar indisponível ou sem circulação; limites, prazos e renovações são aplicados; duas operações concorrentes não emprestam o mesmo exemplar; a fila de reserva segue a regra documentada.

## M4 — Experiência PWA

**Objetivo:** reduzir atrito no uso diário, preservando a integridade das operações.

- Implementar interface acessível para os fluxos concluídos.
- Configurar manifest, instalação e cache do catálogo conforme ADR-008.
- Exibir claramente indisponibilidade de ações de escrita quando não houver conexão.

**Critérios de saída:** a aplicação pode ser instalada; catálogo previamente sincronizado pode ser consultado offline; nenhuma ação de alteração é enfileirada ou simulada offline sem regra explícita de sincronização.

## M5 — Operação e primeira versão

**Objetivo:** disponibilizar uma versão utilizável e recuperável por uma instituição piloto.

- Entregar imagens, Compose de produção, configuração segura e runbook de deploy/rollback conforme ADR-007.
- Testar backup e restauração; definir monitoramento mínimo e canal de suporte.
- Executar piloto com dados autorizados e registrar feedback como issues priorizadas.

**Critérios de saída:** deploy e rollback foram ensaiados; restauração de backup foi comprovada; não há segredos no repositório; requisitos de privacidade, responsáveis e procedimentos de incidente foram aprovados pela instituição piloto.

## Itens fora do roadmap inicial

Multi-tenancy, integração bibliográfica avançada (MARC21 completo), empréstimo offline com sincronização, aplicativo móvel nativo e integrações financeiras permanecem fora do escopo até que exista demanda validada e um ADR correspondente.
