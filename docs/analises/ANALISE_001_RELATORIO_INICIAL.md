# Análise 001 — Relatório de análise inicial do BEC

**Data:** 25 de agosto de 2026  
**Escopo:** documentação e modelo de dados iniciais do projeto  
**Status:** recomendações para tratar antes das migrações dos módulos de identidade e circulação.

## Conclusão executiva

O BEC tem uma base documental coerente e acima da média para o estágio inicial. Stack, escopo, ADRs e roadmap estão alinhados. O modelo ainda não está pronto para implementação da circulação com segurança: há lacunas críticas em auditoria, reserva/alocação de exemplar e concorrência. A aderência à LGPD é **parcialmente desenhada**, mas depende de decisões institucionais e de controles ainda não modelados.

Esta é uma análise técnica e não substitui orientação jurídica.

## Pontos fortes

- Separar `CATEGORIA_USUARIO` de autorização é correto: regras de empréstimo não concedem privilégios administrativos.
- O uso de tabelas de junção para autores evita duplicação e suporta coautoria adequadamente.
- Reserva por obra, tipo de item por exemplar e histórico de renovação são escolhas coerentes com a operação real de uma biblioteca.
- Há preocupação explícita com unicidade, normalização, estados controlados e índices.
- Sessão segura sob mesma origem, CSRF e revogação de conta são escolhas prudentes e simples para o porte do projeto.
- O PWA não permitirá escrita offline, reduzindo risco de empréstimo duplicado e conflito de sincronização.

## Achados prioritários

| Prioridade | Achado | Impacto e recomendação |
|---|---|---|
| Crítica | **Auditoria é exigida, mas não está modelada.** | O ADR exige evento imutável para operações sensíveis, com ator, instante, ação, recurso e resultado, mas não há `EVENTO_AUDITORIA` no ER. Criar a entidade antes do M1, com payload minimizado/mascarado, correlação e proteção contra alteração. |
| Crítica | **Reserva não identifica o exemplar separado para retirada.** | `RESERVA` aponta apenas para `OBRA`; ao torná-la disponível, não existe vínculo com a cópia reservada. Modelar `exemplar_alocado_id` opcional ou uma entidade de alocação; definir status do exemplar e liberação por expiração/cancelamento. |
| Crítica | **Concorrência está como requisito, não como solução.** | Usar transação com bloqueio do exemplar (`SELECT … FOR UPDATE` ou estratégia otimista consistente) e índice/constraint parcial para impedir mais de um empréstimo aberto por exemplar. |
| Alta | **Conta de sistema está acoplada ao leitor.** | `USUARIO.leitor_id` como PK obriga todo operador a ser leitor. Preferir `USUARIO` com identidade própria e relação opcional `leitor_id UNIQUE`. Manter o vínculo obrigatório é mais simples, porém semanticamente pior. |
| Alta | **Estados e transições não foram fechados.** | Formalizar máquinas de estado e constraints: devolução efetiva só em empréstimo devolvido; exemplar indisponível enquanto houver empréstimo aberto ou retenção; item inativo não pode ser emprestado. |
| Alta | **Regras de reserva e renovação estão incompletas.** | Definir duplicidade de reservas, prioridade da fila, renovação com fila ativa, múltiplas cópias e múltiplas reservas disponíveis. Cobrir esses cenários com testes de domínio. |
| Média | **Histórico depende de regras mutáveis.** | Persistir no empréstimo o prazo/regra efetivamente aplicada — ou referência versionada — para evitar reinterpretação histórica. |
| Média | **Modelagem bibliográfica tem decisões abertas.** | `TRADUTOR` e `ILUSTRADOR` suportam somente uma pessoa por livro. Decidir se é limitação de MVP; `CONTRIBUIDOR` com papel reduz duplicações, mas adiciona associação genérica. |
| Média | **Sem política temporal consistente.** | Padronizar eventos operacionais e auditoria em `timestamptz`, com UTC no banco e conversão para o fuso da instituição na interface. |
| Média | **`INSTITUICAO` não tem papel operacional definido.** | Para o MVP, tornar a configuração singleton explicitamente. Só introduzir `instituicao_id` se houver chance real de múltiplas bibliotecas na mesma instalação. |

## Segurança e práticas de mercado

- Usar hash de senha adaptativo, rate limit para login/recuperação e registro de tentativas sem gravar segredo.
- Usar índices únicos normalizados: e-mail case-insensitive e documento com coluna normalizada/índice parcial.
- Implementar `CHECK` para valores não negativos, datas coerentes e estados também no banco.
- Tratar exclusão de catálogo como inativação; para dados pessoais, implementar anonimização sob política de retenção.
- Garantir que o service worker não armazene respostas autenticadas, dados pessoais ou endpoints de leitores.
- Testar autorização no backend, IDOR, CSRF, concorrência de empréstimos e exportação/anonimização.

## LGPD

O projeto já reconhece minimização, finalidade, direitos do titular, retenção, backup e incidentes. Isso se alinha à LGPD, que exige informação clara sobre finalidade, controlador, contato, duração e direitos do titular, além de medidas técnicas e administrativas de segurança. Ver [Lei nº 13.709/2018](https://planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm).

Antes de dados reais, devem ser concluídos:

1. Registro por instituição do controlador, canal ao titular, base legal por finalidade, operadores/suboperadores e política de retenção.
2. Fluxo de acesso, correção, exportação e anonimização, incluindo o histórico de empréstimos e auditoria.
3. Auditoria sem documentos, senha, tokens, endereço ou valores integrais de campos pessoais.
4. Gestão de incidentes, backup criptografado/restrito, restauração testada e descarte.
5. Registro simplificado das operações de tratamento, conforme a [Resolução CD/ANPD nº 2/2022](https://www.gov.br/anpd/pt-br/acesso-a-informacao/institucional/atos-normativos/regulamentacoes_anpd/resolucao-cd-anpd-no-2-de-27-de-janeiro-de-2022).

## Próximo passo recomendado

Antes de iniciar migrações, abrir e refinar quatro issues: **auditoria**, **máquina de estados e concorrência de circulação**, **reserva com alocação de exemplar** e **separação entre usuário operador e leitor**. Elas reduzem os principais riscos de integridade, segurança e LGPD antes que o modelo se espalhe pela aplicação.
