---
kind: phase
name: phase-03-videos
status: dirty
issue_count: 3
sources_mtime:
  docs/phases/phase-03-videos/context.md: "2026-06-26T20:00:00-03:00"
  docs/decisions/technical-decisions-phase-03-videos.md: "2026-06-26T19:55:55-03:00"
issues:
  - id: DG-1
    category: dependency-gap
    summary: "Libs novas (BullMQ, AWS SDK v3, nanoid) sem versão confirmada/fixada via context7."
  - id: DG-2
    category: dependency-gap
    summary: "Infra nova (Redis, MinIO, FFmpeg) sem tags/versões pinadas para o Compose e Dockerfile do worker."
  - id: DG-3
    category: dependency-gap
    summary: "Incompatibilidade ESM do nanoid v5 vs CommonJS do backend não confirmada; linha CJS (v3) a validar."
advisories:
  - id: AD-1
    summary: "Conjunto de chaves de env novas (REDIS_*, S3_*) será enumerado no Technical Spec do plano (plan-build) e validado por Joi."
---

# phase-03-videos — Validation (pass 1)

## Findings

### Inconsistencies

_None._ A aparente tensão entre TD-03 (arquivo de 10GB não passa pela API) e TD-08 (streaming proxiado pela API) é resolvida na própria decisão: upload move o arquivo inteiro (direto ao storage), streaming lê apenas ranges pequenos (206), e download do arquivo inteiro também delega ao storage (TD-09). Sem contradição.

### Ambiguities

_None._ Modelo de dados (status enum, `public_id`, chaves de storage, `upload_id`, duração/metadados) está determinado pelas TD-02, TD-07 e TD-10; o detalhamento de colunas é trabalho do `plan-build` (Data Model).

### Missing Decisions

_None._ Todas as 9 capabilities da Fase 03 têm ao menos uma TD cobrindo (ver `context.md` § Capability Coverage). Toda TD rastreia a uma capability literal do `project-plan.md`.

### Dependency Gaps

- **DG-1 — Versões das libs novas não confirmadas.** As decisões fixam as bibliotecas (`@nestjs/bullmq`, `bullmq`, `@aws-sdk/client-s3`, `@aws-sdk/s3-request-presigner`, `nanoid`), mas as versões exatas compatíveis com NestJS 11 / Node 22 / CommonJS ainda não foram verificadas via context7 nem registradas em `library-refs.md`. **Resolver em `plan-resolve`.**
- **DG-2 — Infra nova não pinada.** Redis e MinIO entram no Compose e o FFmpeg na imagem do worker; as tags de imagem e a forma de instalação do FFmpeg precisam ser fixadas. **Resolver em `plan-resolve`.**
- **DG-3 — Caveat ESM/CJS do nanoid.** TD-07 assume que `nanoid` v5+ é ESM-puro (incompatível com o runtime CommonJS do backend) e que a linha v3 é CommonJS. Confirmar via context7 antes de pinar. **Resolver em `plan-resolve`.**

### Inherited Constraint Conflicts

_None._ A Fase 03 reusa guard JWT global, `@CurrentUser()`, filtro de exceções de domínio, `ValidationPipe` e padrões de migration/config das Fases 01–02 sem reabri-los.

### Unresolved Open Questions

_None._

### UI Coverage Gaps

_N/A._ Nenhuma superfície de UI nesta fase (frontend diferido).

## Resolved Issues

_Nenhum resolvido ainda — ver pass 2 após `plan-resolve`._
