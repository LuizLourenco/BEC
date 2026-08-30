# [FEATURE] Modelar auditoria imutável e minimizada para operações sensíveis

**Labels a aplicar:** `enhancement`  
**Prioridade proposta:** alta  
**Dependência:** deve ser decidida antes das migrações de identidade e circulação.

## O problema que essa funcionalidade resolve

O ADR-009 exige rastreabilidade para operações administrativas, de circulação, autorização e alteração de dados pessoais. O modelo atual não possui uma entidade de auditoria nem define um vocabulário de ações, a retenção, o limite de dados graváveis ou a proteção contra alteração. Implementar migrações antes dessa decisão pode espalhar eventos incompatíveis, expor dados pessoais nos logs ou deixar ações críticas sem evidência verificável.

## Solução proposta

Definir o contrato de auditoria e incluí-lo no modelo lógico antes de qualquer migração que crie operações auditáveis.

- Criar o agregado técnico `EVENTO_AUDITORIA`, somente de inserção, com identificador, `ocorrido_em timestamptz`, ator opcional (para processos do sistema), tipo e identificador do recurso, ação, resultado, identificador de correlação e metadados permitidos.
- Padronizar ações iniciais: autenticação, criação/alteração de leitor, mudança de papel, empréstimo, devolução, renovação, reserva disponibilizada, reserva atendida e cancelamento.
- Definir resultado explícito (`SUCESSO` ou `NEGADO`); falhas técnicas só serão registradas se a transação puder persistir o evento sem alterar o erro original.
- Manter metadados em estrutura com *allowlist*. É proibido registrar senha, hash de senha, token, documento, endereço, telefone, e-mail ou valores integrais antes/depois de campos pessoais. Quando indispensável, registrar somente identificadores internos ou valores mascarados.
- Restringir leitura da trilha a permissões administrativas específicas e impedir `UPDATE` e `DELETE` pela aplicação e pelo papel operacional do banco.
- Documentar prazo de retenção, responsável institucional e fluxo de resposta a solicitações do titular. A definição institucional pendente não pode ser inventada na migração.

## Decisões e alternativas consideradas

1. **Evento append-only com metadados minimizados (proposta).** Preserva evidência útil e reduz exposição LGPD; exige disciplina de vocabulário e permissões de banco.
2. **Registrar snapshots completos antes/depois.** Facilita investigação, mas duplica dados pessoais e amplia risco de vazamento e retenção; rejeitado.
3. **Usar somente logs de aplicação.** É simples, porém não oferece contrato, controle de acesso ou durabilidade adequados; rejeitado.

## Critérios de aceite

- [ ] ADR-009, `DATA_MODEL.md` e `PRIVACIDADE_E_SEGURANCA.md` passam a referenciar o mesmo contrato de auditoria.
- [ ] O modelo define campos, vocabulário inicial de ações/resultados e metadados permitidos ou proibidos.
- [ ] Eventos usam `timestamptz` em UTC e permitem correlação sem armazenar conteúdo sensível.
- [ ] Uma operação sensível bem-sucedida gera exatamente um evento associado ao ator e ao recurso; uma operação negada gera evento `NEGADO` quando houver ator autenticado.
- [ ] Tentativas de alterar ou remover evento pelo papel da aplicação falham; essa regra possui teste de integração com PostgreSQL.
- [ ] Testes garantem que documentos, credenciais, tokens e valores pessoais integrais não aparecem nos metadados serializados.
- [ ] A documentação registra a pendência institucional de retenção e o dono necessário para fechá-la antes de produção com dados reais.

## Fora de escopo

- Interface de consulta/exportação de auditoria.
- SIEM, monitoramento externo ou retenção legal já configurada.
- Decidir a base legal ou prazo de retenção em nome da instituição.

## Contexto adicional

Base: `docs/decisions/0009-autorizacao-e-auditoria.md`, `docs/PRIVACIDADE_E_SEGURANCA.md` e análise 001. A implementação posterior deve ser revisada por pessoa responsável por segurança/LGPD.

