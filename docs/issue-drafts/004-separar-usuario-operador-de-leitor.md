# [FEATURE] Separar identidade de usuário operador e leitor

**Labels a aplicar:** `enhancement`  
**Prioridade proposta:** alta  
**Dependências:** auditoria; deve preceder as migrações de identidade e todas as FKs de operador em circulação.

## O problema que essa funcionalidade resolve

No modelo atual, `USUARIO.leitor_id` é simultaneamente chave primária e chave estrangeira para `LEITOR`. Isso obriga cada operador a ser leitor, mistura identidade de autenticação com perfil de circulação e faz FKs operacionais parecerem apontar a um leitor em vez da pessoa que executou a ação. A separação é importante para menor privilégio, revogação, auditoria e minimização LGPD.

## Solução proposta

Dar identidade própria a `USUARIO` e tornar seu vínculo com `LEITOR` opcional e um-para-um.

- Criar `USUARIO.id` como chave primária independente; `leitor_id` passa a ser FK opcional com unicidade quando preenchido.
- Renomear FKs operacionais para `registrado_por_usuario_id` em empréstimos e renovações, e usar `usuario_id` como ator em auditoria. Nenhuma delas deve depender de `LEITOR`.
- Manter categoria de leitor exclusivamente nas regras de circulação. Papéis e permissões continuam em `USUARIO`, conforme ADR-009.
- Definir ciclo de vida independente: desativar usuário revoga autenticação sem excluir seu identificador; bloquear/inativar leitor não deve apagar nem promover privilégios de usuário.
- Definir as regras de exclusão: históricos/auditoria devem preservar referência ao identificador do operador; remoção física só é permitida se as obrigações de retenção a admitirem.

## Decisões e alternativas consideradas

1. **Usuário com PK própria e `leitor_id` opcional/único (proposta).** Representa operadores que não são leitores e leitores que não possuem login, mantendo vínculo voluntário quando fizer sentido.
2. **Manter `USUARIO` como extensão obrigatória de `LEITOR`.** Tem menos colunas, mas mistura conceitos e obriga cadastro pessoal desnecessário para operadores; rejeitada.
3. **Separar totalmente sem vínculo possível.** Maximiza isolamento, mas impede modelar legitimamente uma mesma pessoa que opera e também utiliza a biblioteca; rejeitada nesta fase.

## Critérios de aceite

- [ ] O diagrama e o schema possuem `USUARIO.id` independente e `USUARIO.leitor_id` opcional com unicidade adequada.
- [ ] É possível cadastrar operador ativo sem `LEITOR` e leitor sem `USUARIO`.
- [ ] Uma pessoa vinculada não pode ter duas contas apontando ao mesmo leitor, salvo decisão explícita documentada que altere esta regra.
- [ ] Empréstimos, renovações e auditoria apontam para `USUARIO`, não para uma chave com semântica de leitor.
- [ ] A desativação de uma conta revoga seu acesso no backend imediatamente; ela não altera automaticamente a categoria ou a situação do leitor.
- [ ] Testes de integração cobrem autorização negada para conta inativa e preservação do ator nos históricos após desativação.
- [ ] ADR-009 e `DATA_MODEL.md` deixam de conter relações contraditórias entre usuário, leitor e registros de circulação.

## Fora de escopo

- SSO, recuperação de senha, MFA e gestão de sessão.
- Migração de dados legados: ainda não há schema implantado; se isso mudar, a migração deverá ser planejada em issue própria com rollback.

## Contexto adicional

Base: ADR-009, `DATA_MODEL.md` e análise 001. Esta issue é estrutural; o PR de implementação deve manter migração reversível enquanto não houver dados reais.

