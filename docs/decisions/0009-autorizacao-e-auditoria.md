# ADR-009: Autorização e auditoria — papéis fixos e permissões explícitas

**Contexto:** o BEC armazena dados pessoais e registra operações que afetam a circulação do acervo. Categoria de leitor serve para definir prazo e limite de empréstimo; ela não pode determinar acesso administrativo. É necessário que uma concessão, revogação ou operação sensível seja rastreável.

**Decisão:** separar circulação de autorização.

- `CATEGORIA_USUARIO` classifica apenas leitores para `REGRA_EMPRESTIMO`.
- `USUARIO.papel_sistema` é um enum permitido pelo domínio e aplicado pelo backend. Papéis iniciais: `ADMIN` e `BIBLIOTECARIO`; novos papéis exigem alteração de código, teste e revisão.
- `PERMISSAO` e `PAPEL_PERMISSAO` registram o catálogo de capacidades reconhecidas pelo backend. A autorização exige papel ativo e permissão correspondente; a interface nunca é a barreira de segurança.
- Toda operação administrativa, de empréstimo, devolução, renovação, reserva atendida, mudança de papel ou alteração de dados pessoais deve gerar evento de auditoria imutável com ator, instante, ação, recurso e resultado. Dados sensíveis não devem ser gravados integralmente no evento.
- Uma conta desativada perde acesso imediatamente. A gestão de papéis exige permissão administrativa e não pode conceder ao próprio ator privilégio superior ao seu.

**Alternativas consideradas:**

- Usar `CATEGORIA_USUARIO` para autorizar: descartada, pois um perfil de circulação não é um privilégio de sistema e mudanças administrativas poderiam elevar acesso indevidamente.
- Permissões totalmente configuráveis no banco: descartada para a primeira versão; uma permissão desconhecida pelo código não é uma política verificável. O catálogo é semeado e versionado nas migrações.
- Confiar em esconder botões no frontend: descartada; clientes podem chamar a API diretamente.

**Consequências:** há mais entidades e verificações de autorização, mas regras ficam separadas, revisáveis e testáveis. A implementação deve aplicar autorização no backend, manter uma trilha de auditoria com controle de acesso e cobrir negação de acesso e revogação com testes de integração.

**Status:** decidido.
