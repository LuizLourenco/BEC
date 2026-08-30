# [FEATURE] Reservar por obra e alocar exemplar para retirada

**Labels a aplicar:** `enhancement`  
**Prioridade proposta:** alta  
**Dependências:** máquina de estados e concorrência da circulação; deve preceder as migrações de reservas.

## O problema que essa funcionalidade resolve

A reserva é corretamente criada para uma `OBRA`, mas, quando a fila chega à primeira pessoa, o modelo não identifica a cópia que foi separada. Sem essa alocação, um operador pode emprestar o mesmo exemplar a outra pessoa, não há como conferir a retirada por código de barras e expiração/cancelamento podem deixar a cópia bloqueada indefinidamente.

## Solução proposta

Manter a intenção da reserva por título e tornar explícita a alocação efêmera do exemplar quando uma cópia elegível ficar livre.

- Adicionar em `RESERVA` o FK opcional `exemplar_alocado_id`. Ele é nulo em `ATIVA` e obrigatório em `DISPONIVEL` e `ATENDIDA`.
- Quando houver exemplar elegível, selecionar a reserva ativa mais antiga da obra (FIFO), bloquear exemplar e reserva na mesma transação e mudar a reserva para `DISPONIVEL`, preenchendo `data_disponibilizacao`, `data_limite_retirada` e `exemplar_alocado_id`.
- Considerar elegível somente exemplar ativo, emprestável, sem empréstimo aberto e sem outra retenção válida. O prazo de retirada deve ser uma regra configurada/documentada antes da implementação.
- A retirada deve usar exatamente o `exemplar_alocado_id`; ao atender, criar o empréstimo e mudar a reserva para `ATENDIDA` na mesma transação.
- Cancelamento ou expiração de reserva disponível libera a cópia e tenta atender a próxima reserva FIFO na mesma obra. A decisão de preservar o vínculo histórico após a liberação deve ser tomada nesta issue e refletida no modelo.
- Proibir reserva ativa/disponível duplicada para o mesmo leitor e obra, e impedir que um exemplar esteja alocado a mais de uma reserva disponível.

## Decisões e alternativas consideradas

1. **FK opcional em `RESERVA` (proposta).** Resolve o vínculo com a cópia com baixo custo e preserva a fila por obra; é suficiente enquanto cada reserva tiver no máximo uma alocação vigente.
2. **Entidade `ALOCACAO_RESERVA` separada.** Preserva cada tentativa e troca de exemplar com maior detalhe, mas adiciona complexidade sem um caso atual que exija histórico de múltiplas alocações.
3. **Reservar diretamente um exemplar.** Simplifica a retirada, porém piora a experiência para obras com várias cópias e contraria a regra documentada de “próxima cópia livre”; rejeitada.

## Critérios de aceite

- [ ] A reserva continua sendo criada para `OBRA`, nunca para exemplar escolhido antecipadamente.
- [ ] `RESERVA.exemplar_alocado_id` é obrigatório para os estados que retêm ou registram a retirada e não aponta para exemplar de outra obra.
- [ ] Não existe mais de uma reserva ativa/disponível para o mesmo par leitor–obra, nem mais de uma alocação vigente para o mesmo exemplar.
- [ ] A seleção respeita FIFO e só escolhe exemplar elegível; o comportamento com várias cópias livres possui teste.
- [ ] O empréstimo de retirada somente aceita o exemplar alocado àquela reserva; tentativa com outra cópia é recusada.
- [ ] Expiração e cancelamento liberam a cópia transacionalmente e permitem atender a próxima reserva, quando houver.
- [ ] Testes de integração cobrem corrida entre devolução, alocação, retirada, cancelamento e expiração.
- [ ] Cada disponibilização, atendimento, cancelamento e expiração gera o evento de auditoria definido na issue 001.

## Fora de escopo

- Notificação por e-mail, SMS ou WhatsApp.
- Reserva de edição rara/cópia específica.

## Contexto adicional

Base: `docs/DATA_MODEL.md`, análise 001 e M3 do roadmap. A regra de prazo de retirada precisa de dono de negócio antes de migrar o campo correspondente.

