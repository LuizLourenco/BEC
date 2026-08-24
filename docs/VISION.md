# Visão do Projeto — BEC

## Problema que resolve

Muitas bibliotecas de instituições espíritas no Brasil são geridas de forma manual ou com ferramentas genéricas (planilhas, cadernos, sistemas de biblioteca comerciais complexos demais para a realidade do voluntariado espírita). Isso gera:

- Dificuldade em saber o que existe no acervo e onde está.
- Perda de controle sobre empréstimos e devoluções.
- Barreira de entrada alta para voluntários sem formação técnica.
- Custo de infraestrutura incompatível com o orçamento de um centro espírita (geralmente mantido por doações).

O **BEC** existe para oferecer uma alternativa **leve, gratuita, simples de operar e simples de hospedar ou instalar em computadores geralmente oriundo de doações**, feita sob medida para essa realidade.

## Escopo

### O que o BEC É

- Um sistema web (PWA) para **catalogação de acervo** (livros, revistas, cartas e, futuramente, outros materiais espíritas).
- Um sistema de **cadastro de leitores/frequentadores** da biblioteca.
- Um sistema de **controle de empréstimos e devoluções** (com histórico e pendências).
- Um software pensado para rodar em **infraestrutura modesta** (VPS simples, hardware de cliente antigo).
- Um projeto **aberto e comunitário**, com processo de contribuição documentado, para que voluntários com conhecimento técnico de qualquer instituição espírita possam colaborar.

### O que o BEC NÃO é (por enquanto)

- ❌ Não é um sistema de gestão financeira ou administrativa da instituição espírita como um todo.
- ❌ Não é um sistema de gestão de eventos, palestras ou atendimentos fraternos.
- ❌ Não é um e-commerce ou plataforma de venda de livros.
- ❌ Não é (inicialmente) um sistema multi-tenant complexo com hierarquias entre federações/instituições — o foco inicial é **uma instalação por biblioteca/instituição**.
- ❌ Não pretende competir em funcionalidades com sistemas de biblioteca acadêmicos/corporativos complexos (ex: integração com normas MARC21 completas, empréstimo interbibliotecas automatizado, etc.) — ao menos não nas fases iniciais.

> Este escopo pode evoluir. Mudanças de escopo devem ser discutidas via issue e refletidas aqui antes de virarem trabalho.

## Princípios de design

1. **Simplicidade antes de recursos.** Preferimos fazer pouco e bem feito a fazer muito e mal.
2. **Leveza operacional.** O software deve rodar em hardware/infraestrutura modesta sem sacrificar usabilidade.
3. **Acessível a não-técnicos.** Tanto o uso (bibliotecário) quanto, na medida do possível, a operação (deploy) devem ser simples.
4. **Comunidade aberta.** O projeto é pensado desde o início para receber contribuições externas de voluntários de outras instituições.
