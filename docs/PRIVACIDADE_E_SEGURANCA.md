# Privacidade e Segurança — BEC

Este documento define requisitos mínimos para o uso de dados de leitores antes de qualquer implantação com dados reais. Ele deve ser validado pelo responsável da instituição e revisado a cada mudança de finalidade ou fluxo de dados.

## Minimização e finalidade

- Coletar somente os dados necessários para identificar o leitor, comunicar devoluções e administrar empréstimos.
- Documento, telefone e endereço são opcionais, salvo decisão documentada da instituição que justifique sua necessidade.
- Não inserir dados reais em ambientes de desenvolvimento, demonstração, testes ou prompts de IA.
- O sistema deve informar, no cadastro, a finalidade de cada campo e o canal de contato da instituição responsável.

## Acesso e proteção

- O backend autoriza toda operação conforme o [ADR-009](decisions/0009-autorizacao-e-auditoria.md); o frontend não é controle de acesso.
- Credenciais usam hash resistente a força bruta; cookies de sessão usam `HttpOnly`, `Secure` em HTTPS e proteção CSRF.
- Produção usa HTTPS, segredos fora do repositório e backups protegidos com acesso restrito.
- Eventos de auditoria registram alterações e operações sensíveis sem expor documentos, senhas, tokens ou conteúdo integral de campos pessoais.

## Retenção, direitos e descarte

- A instituição deve definir e publicar o prazo de retenção para cadastro, histórico de empréstimos e logs de auditoria antes do primeiro uso real.
- Deve existir fluxo administrativo para corrigir dados, exportar informações do leitor e anonimizar/excluir dados quando cabível, preservando apenas obrigações legais e registros necessários de forma justificada.
- Backups devem ter prazo de retenção, teste periódico de restauração e procedimento documentado para descarte seguro.

## Incidentes

- Qualquer suspeita de acesso indevido, perda ou exposição deve ser registrada, contida e avaliada pelo responsável da instituição.
- O procedimento operacional de resposta deve definir responsáveis, evidências mínimas, comunicação e medidas corretivas antes do deploy público.

## Pendências institucionais

Antes de tratar dados reais, o mantenedor e a instituição precisam confirmar: responsável pelo tratamento, canal de contato, bases legais e avisos aplicáveis, prazos de retenção, política de backup e procedimento de incidentes. Este documento é requisito de produto e não substitui orientação jurídica.
