# phase-03-videos — Progress

**Status:** in progress
**SIs:** 4/11 completed

### SI-03.1 — Dependências, Config Namespaces e Infra Docker (Redis, MinIO, Worker)
- **Status:** completed
- **Tests:** no tests (infra/config); baseline da suíte permanece verde
- **Observations:** db remapeado para `5433:5432` no host por conflito com um Postgres de outro projeto (acesso interno `db:5432` inalterado). Redis/MinIO não publicam portas host (acesso por nome de serviço). `S3_ACCESS_KEY/S3_SECRET_KEY` ficaram com default (não `required`) para manter a convenção do projeto de `validate({})` limpo. ffmpeg 5.1.9 instalado na imagem.

### SI-03.2 — Storage Service (S3/MinIO via AWS SDK v3)
- **Status:** completed
- **Tests:** 4/4 passing (storage.service.integration-spec.ts — roundtrip multipart, range/206, presigned download, ensureBucket idempotente) contra MinIO real
- **Observations:** AWS SDK instalado em `3.1075.0`. `getObjectRange` devolve stream + `Content-Range` para o streaming. `onModuleInit` garante o bucket.

### SI-03.3 — Entidade Video e Migration
- **Status:** completed
- **Tests:** 8/8 passing (video.entity.integration-spec.ts: 4; videos.module.spec.ts: 1; migrations.integration-spec.ts: 3)
- **Observations:** Migration `CreateVideos` gerada via CLI (enum `video_status`, índices `public_id` único/`channel_id`/`status`, FK para `channels`). Teste de migrations estendido para 5 tabelas e passou a dropar os enums no setup (idempotente em DB pré-migrado); drops serializados para evitar deadlock. Limpeza dos testes respeita a ordem de FK (videos antes de channels/users).

### SI-03.4 — Public ID, Máquina de Status e Exceções de Domínio
- **Status:** completed
- **Tests:** 12/12 passing (public-id.service.spec.ts: 4; video-status.spec.ts: 8)
- **Observations:** `nanoid@3` (linha CommonJS) em `customAlphabet` (11 chars, sem `-`/`_`). `assertTransition` valida o ciclo; exceções estendem `DomainException` (reuso do filtro da Fase 02).

### SI-03.5 — Queue Module e Contrato do Job (Producer)
- **Status:** pending
- **Tests:** —
- **Observations:** —

### SI-03.6 — Iniciação do Upload (POST /videos)
- **Status:** pending
- **Tests:** —
- **Observations:** —

### SI-03.7 — Finalização do Upload (POST /videos/:id/complete)
- **Status:** pending
- **Tests:** —
- **Observations:** —

### SI-03.8 — Worker: Bootstrap, Processor e Processamento FFmpeg
- **Status:** pending
- **Tests:** —
- **Observations:** —

### SI-03.9 — Streaming por Range (GET /videos/:publicId/stream)
- **Status:** pending
- **Tests:** —
- **Observations:** —

### SI-03.10 — Download Presigned e Leitura do Vídeo
- **Status:** pending
- **Tests:** —
- **Observations:** —

### SI-03.11 — OpenAPI, Wiring Final, Definition of Done e Documentação
- **Status:** pending
- **Tests:** —
- **Observations:** —
