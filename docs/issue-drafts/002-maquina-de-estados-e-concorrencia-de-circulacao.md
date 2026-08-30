# [FEATURE] Fechar máquina de estados e concorrência da circulação

**Labels a aplicar:** `enhancement`  
**Prioridade proposta:** alta  
**Dependências:** decisões de reserva/alocação e de auditoria; deve preceder as migrações de circulação.

## O problema que essa funcionalidade resolve

Hoje os estados de `EMPRESTIMO`, `RESERVA` e `EXEMPLAR` não têm transições ou fonte de verdade formalizadas. Além disso, a concorrência aparece como requisito, sem estratégia transacional. Dois atendimentos simultâneos podem emprestar ou separar a mesma cópia, gerando perda de integridade e uma trilha de auditoria contraditória.

## Solução proposta

Especificar uma máquina de estados de circulação e implementá-la em serviço de aplicação transacional, com restrições no PostgreSQL que impeçam violações mesmo em caso de concorrência.

- Formalizar estados, comandos, pré-condições, efeitos e estados terminais para empréstimo, reserva e disponibilidade do exemplar.
- Adotar as transições iniciais: empréstimo `ABERTO → DEVOLVIDO`; reserva `ATIVA → DISPONIVEL → ATENDIDA`, ou `ATIVA|DISPONIVEL → CANCELADA|EXPIRADA`; exemplar disponível, emprestado, retido para reserva e indisponível por condição física/administrativa.
- Definir se a disponibilidade do exemplar é persistida com atualização transacional ou derivada. Não pode coexistir uma terceira fonte de verdade.
- Para emprestar, devolver, renovar, disponibilizar, atender, expirar ou cancelar reserva, bloquear a linha do exemplar com `SELECT ... FOR UPDATE` dentro de uma única transação. Bloquear demais linhas sempre em ordem determinística para evitar deadlocks.
- Criar índice/constraint parcial que impeça mais de um empréstimo aberto por exemplar. Validar na transação o limite do leitor, a permissão do tipo de item e a prioridade de reserva.
- Normalizar conflitos de banco para erro de domínio explícito (por exemplo, `EXEMPLAR_INDISPONIVEL`), sem tratar colisão como sucesso.

## Decisões e alternativas consideradas

1. **Bloqueio pessimista por exemplar + constraint parcial (proposta).** É direto para a operação de balcão e o banco garante o invariante final; mantém transações curtas.
2. **Versão otimista no exemplar.** Pode reduzir bloqueios, mas impõe tentativas repetidas e é mais fácil errar nos fluxos que alteram reserva e empréstimo; não é a primeira escolha para este domínio.
3. **Somente validação na aplicação.** Falha sob concorrência; rejeitada.

## Critérios de aceite

- [ ] Há uma tabela de transições válida para cada estado de empréstimo, reserva e exemplar, incluindo causas de rejeição.
- [ ] O modelo define uma única fonte de verdade para disponibilidade do exemplar e regras para item inativo, não emprestável ou retido.
- [ ] A migração cria constraint/índice parcial que impede dois empréstimos abertos para o mesmo exemplar.
- [ ] Dois comandos concorrentes para emprestar o mesmo exemplar resultam em um único empréstimo aberto; o outro recebe erro de domínio previsível.
- [ ] Dois comandos concorrentes para reservar/alocar o mesmo exemplar não criam duas retenções válidas.
- [ ] Devolução, expiração e cancelamento liberam o exemplar apenas pela transição definida e geram a auditoria correspondente.
- [ ] Há testes de integração concorrentes usando PostgreSQL real; testes unitários cobrem as transições inválidas.

## Fora de escopo

- Interface de balcão e notificações ao leitor.
- Calendário de funcionamento ou cálculo de feriados.

## Contexto adicional

Base: regras de circulação em `docs/DATA_MODEL.md`, M3 em `docs/ROADMAP.md` e análise 001. A definição de `DISPONIVEL` para a reserva depende da alocação tratada na issue 003.

