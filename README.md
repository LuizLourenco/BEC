# BEC — Biblioteca Espírita Compartilhada

> Sistema de gestão de bibliotecas para instituições espíritas do Brasil.

## O que é

O **BEC** é um software livre voltado a apoiar centros e instituições espíritas na gestão de suas bibliotecas: catalogação de acervo, cadastro de leitores e controle de empréstimos. O projeto nasce voltado para atender as necessidades da instituição espírita FAK - Fundação Allan Kardec em Manaus, mas é desenvolvido para ser usado por qualquer instituição espírita do Brasil.

## Para quem

- Bibliotecários e voluntários de bibliotecas espíritas, muitas vezes sem formação técnica em TI.
- Instituições com infraestrutura simples (computadores antigos, conexões modestas, hospedagem de baixo custo).

## Por que existe

Bibliotecas espíritas costumam operar com controles manuais (cadernos, planilhas soltas) ou softwares genéricos que não refletem a realidade de operação de um centro espírita (voluntariado, baixo orçamento, hardware modesto) ou das obras e literatura espirita que custa conter peculiaridades. O BEC busca ser uma ferramenta simples, leve e gratuita, pensada para essa realidade.

Veja o [VISION.md](docs/VISION.md) para entender o escopo completo do que o projeto é e não é.

## Status atual

🚧 **Em desenvolvimento inicial.** Ainda não há release estável. Decisões de arquitetura e processo estão sendo formalizadas antes do início da implementação.

## Stack (resumo)

- **Backend:** Spring (Java) + PostgreSQL
- **Frontend:** Vue 3 + Vite, como Progressive Web App (PWA)
- **Infraestrutura alvo:** computadores locais ou instâncias de nuvem simples, mas sempre visando hardware modesto.

Veja o racional completo de cada escolha em [ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Como rodar localmente

> ⚠️ Setup ainda em construção — este passo a passo será atualizado conforme o backend e o frontend forem inicializados no repositório.

Pré-requisitos previstos:
- Java 21+ e Maven
- PostgreSQL 15+
- Node.js 20+ (para o frontend)

```bash
git clone git@github.com:LuizLourenco/BEC.git
cd BEC
# instruções de backend e frontend serão detalhadas aqui assim que
# os módulos forem criados
```

## Documentação do projeto

| Documento | Conteúdo |
|---|---|
| [VISION.md](docs/VISION.md) | Problema, escopo (o que é / o que não é) |
| [ARCHITECTURE.md](docs/ARCHITECTURE.md) | Decisões técnicas e seus porquês (ADRs) |
| [AGILE_WORKFLOW.md](docs/AGILE_WORKFLOW.md) | Como o trabalho flui, board, definition of done |
| [AI_GUIDELINES.md](docs/AI_GUIDELINES.md) | Regras de uso de IA no desenvolvimento |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Como contribuir, passo a passo |
| [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) | Código de conduta da comunidade |
| [CHANGELOG.md](CHANGELOG.md) | Histórico de versões |

## Licença

Este projeto é distribuído sob a licença [MIT](LICENSE).

## Contato

Mantenedor: Luiz Lourenço ([@LuizLourenco] (https://github.com/LuizLourenco)) ou e-mail: [lourenzzo.am@gmail.com](mailto:lourenzzo.am@gmail.com)

