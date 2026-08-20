# Diretrizes de Uso de IA — BEC

O BEC é desenvolvido com apoio ativo de ferramentas de Inteligência Artificial. Este documento existe para que o uso seja **transparente, seguro e consistente** entre todos os colaboradores.

## Ferramentas aceitas/recomendadas

- **Claude Code** / **Claude** (Anthropic) — uso recomendado para geração de código, documentação e revisão.
- **GitHub Copilot** — aceito como assistente inline durante a escrita de código.
- Outras ferramentas de IA generativa de código são aceitas, desde que respeitem as regras abaixo. Em caso de dúvida sobre uma ferramenta específica, abra uma issue com a label `needs discussion`.

Não há obrigatoriedade de uso de IA — contribuições feitas sem apoio de IA são igualmente bem-vindas.

## O que a IA pode fazer de forma mais autônoma

- Gerar rascunhos de issues a partir de ideias soltas.
- Gerar código de implementação (funções, componentes, testes) a partir de uma issue com critérios de aceite claros.
- Gerar/atualizar documentação técnica (comentários, docs, READMEs de módulo).
- Sugerir refatorações e apontar problemas em revisões preliminares.
- Gerar dados de teste/fixtures fictícios.

## O que **sempre** exige revisão humana antes de virar código definitivo

- **Todo PR**, independentemente de ter sido gerado com apoio de IA, precisa de aprovação humana antes do merge (ver [CONTRIBUTING.md](CONTRIBUTING.md) e [AGILE_WORKFLOW.md](AGILE_WORKFLOW.md)).
- Decisões de **arquitetura** (ex: mudanças registradas em ARCHITECTURE.md) — a IA pode sugerir e ajudar a redigir, mas a decisão final é humana.
- Qualquer código que lide com **autenticação, autorização, dados pessoais de leitores (LGPD) ou segurança** deve ter revisão humana redobrada, mesmo que gerado por IA.
- Migrações de banco de dados (schema) — revisão humana obrigatória antes de aplicar em qualquer ambiente compartilhado.

## Transparência: como sinalizar código gerado por IA

- PRs com contribuição significativa de IA devem usar a label **`ai-assisted`**.
- Na descrição do PR, inclua uma linha simples indicando o uso, por exemplo:
  ```
  Gerado com apoio de: Claude Code (implementação inicial), revisado e ajustado manualmente.
  ```
- Isso não é burocracia punitiva — é para que quem revisa saiba onde prestar atenção redobrada e para que novos colaboradores entendam que faz parte do processo normal do projeto.

## Cuidados obrigatórios ao usar IA

1. **Nunca cole segredos, chaves de API, senhas ou dados reais de leitores/instituições em prompts** de ferramentas de IA — mesmo em ferramentas "confiáveis". Use sempre dados fictícios/mascarados.
2. **Revise licenças de sugestões de código.** Se uma IA sugerir um trecho que pareça copiado de uma biblioteca/projeto com licença incompatível com MIT (ex: GPL sem compatibilidade), não incorpore sem verificar.
3. **Não confie cegamente em explicações da IA sobre comportamento de bibliotecas/frameworks** — valide contra a documentação oficial, especialmente para Spring Security, PostgreSQL e APIs de PWA/service worker, que mudam de versão para versão.
4. **Dados pessoais (LGPD):** o sistema lida com dados de leitores (nome, contato, histórico de empréstimo). Ao gerar código ou exemplos, nunca use dados reais de pessoas — sempre fictícios.
5. **Teste o que a IA gerar.** Código gerado por IA deve passar pelos mesmos testes e critérios de qualidade de qualquer outro código (ver Definition of Done em AGILE_WORKFLOW.md).

## Por que isso importa

Ferramentas de IA aceleram muito o desenvolvimento, especialmente para um projeto mantido majoritariamente por voluntários com tempo limitado. Mas confiança se constrói com transparência: um colaborador novo que entra no projeto precisa conseguir confiar no código existente, e saber "isso foi gerado por IA e revisado por fulano" ajuda mais do que atrapalha.
