# ADR-007: Estratégia de deploy — Docker Compose + CI simples + Oracle Cloud (free tier)

**Contexto:** app pensada para rodar localmente ou em servidor de nuvem, mantida por um mantenedor solo, com orçamento próximo de zero.

**Decisão:** empacotar com Docker Compose (backend + Postgres + frontend estático), usar GitHub Actions só para build/test e publicar a imagem no GitHub Container Registry, com deploy manual (`docker compose pull && up -d` via SSH). Provedor recomendado: Oracle Cloud Free Tier (instância ARM Always Free, região São Paulo).

**Alternativas consideradas:**
- **Kubernetes:** descartado — complexidade de operação incompatível com mantenedor solo.
- **CD totalmente automatizado:** descartado por ora — prefere-se controle manual do momento do deploy com um único mantenedor validando cada release.
- **DigitalOcean/Hetzner:** mais simples de configurar, mas pagos desde o início e sem região no Brasil (mais latência). Alternativa caso o free tier da Oracle se mostre inviável.
- **AWS/GCP/Azure:** descartados por ora — free tier mais limitado (tempo ou créditos) e maior complexidade de configuração.

**Consequências:** custo de infraestrutura zero enquanto o free tier da Oracle for suficiente; mesmo Docker Compose funciona local e em produção, simplificando a documentação de setup; deploy manual exige rodar o comando de atualização a cada release (aceitável na escala atual).

**Status:** decidido — sujeito a revisão se os limites do free tier forem atingidos ou a equipe crescer.
