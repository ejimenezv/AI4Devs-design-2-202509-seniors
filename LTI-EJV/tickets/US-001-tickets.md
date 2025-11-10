# Descomposición Técnica: US-001 - Parseo Inteligente de CV con IA

**Proyecto:** LTI (Leading Talent Intelligence)
**Sprint:** Sprint 1
**Prioridad:** P1 (Alta) - MUST HAVE
**Estimación Total:** L (2-3 semanas)
**Fecha de Creación:** 2025-01-09

---

## 📋 Resumen de la User Story Original

**Como** Recruiter
**Quiero** que el sistema extraiga automáticamente información estructurada de los CVs que suben los candidatos (PDF, Word, LinkedIn)
**Para** ahorrar tiempo de entrada manual de datos y garantizar que todos los perfiles estén completos y normalizados desde el primer momento

### Criterios de Aceptación Principales
- Soporte para PDF, DOCX, TXT, y URLs de LinkedIn
- Precisión >85% en extracción de datos críticos
- Procesamiento <30 segundos para 95% de CVs
- Normalización automática de habilidades
- Detección de duplicados
- Vista editable de datos extraídos
- Fallback a revisión manual en casos de baja confianza

---

## 🏗️ Arquitectura Técnica de la Solución

### Componentes del Sistema

```mermaid
graph TB
    subgraph "Frontend - React 19.2"
        UI[CV Upload UI]
        Preview[Preview & Edit Component]
        Status[Upload Status Dashboard]
    end

    subgraph "API Gateway - Node.js + TypeScript"
        API[REST API Gateway]
        Auth[Authentication Middleware]
        Upload[File Upload Handler]
    end

    subgraph "Storage Layer"
        S3[AWS S3<br/>CV Storage]
        MySQL[(MySQL Database<br/>Structured Data)]
    end

    subgraph "Message Queue"
        Queue[RabbitMQ<br/>Parse Job Queue]
    end

    subgraph "CV Parser Service - Python + FastAPI"
        Parser[CV Parser Orchestrator]
        PDFParser[PDF Parser<br/>PyPDF2/pdfplumber]
        DOCXParser[DOCX Parser<br/>python-docx]
        LinkedInParser[LinkedIn Parser]
        NLPEngine[NLP Engine<br/>spaCy NER]
        Normalizer[Skills Normalizer]
        OCR[AWS Textract<br/>Fallback OCR]
    end

    subgraph "Support Services"
        Cache[Redis Cache<br/>Parsed Results]
        Logger[Logging Service]
    end

    UI -->|Upload CV| API
    API -->|Store File| S3
    API -->|Enqueue Job| Queue
    Queue -->|Process| Parser
    Parser -->|Extract Text| PDFParser
    Parser -->|Extract Text| DOCXParser
    Parser -->|Scrape Data| LinkedInParser
    PDFParser -->|Complex PDF| OCR
    Parser -->|Extract Entities| NLPEngine
    NLPEngine -->|Normalize| Normalizer
    Parser -->|Save Structured Data| MySQL
    Parser -->|Cache Result| Cache
    Parser -->|Notify| API
    API -->|Update UI| Preview

    style UI fill:#e1f5ff
    style API fill:#fff4e6
    style Parser fill:#f3e5f5
    style MySQL fill:#e8f5e9
    style Queue fill:#fce4ec
```

### Stack Tecnológico Detallado

| Componente | Tecnología | Justificación |
|------------|------------|---------------|
| **Frontend** | React 19.2 + TypeScript | UI moderna con type safety |
| **API Gateway** | Node.js 20 + Express + TypeScript | Unificación del stack backend |
| **Parser Service** | Python 3.11 + FastAPI | Ecosistema NLP/ML maduro |
| **Database** | MySQL 8.0 | Almacenamiento relacional estructurado |
| **File Storage** | AWS S3 (o MinIO local) | Escalable, versionado de CVs |
| **Message Queue** | RabbitMQ | Procesamiento asíncrono confiable |
| **Cache** | Redis 7 | Evitar reprocesamiento de CVs |
| **NLP Engine** | spaCy 3.7 (es_core_news_lg/en_core_web_lg) | NER pre-entrenado, extensible |
| **OCR** | AWS Textract API | PDFs escaneados/complejos |
| **Container** | Docker + Docker Compose | Desarrollo y despliegue consistente |

---

## 🎫 Tickets Técnicos Descompuestos

### TICKET US-001-01: Database Schema - Candidate & Skills Tables

**Tipo**: Technical
**Componente**: Database
**Asignado a**: Backend Dev
**Estimación**: 2 días

#### Descripción
Diseñar e implementar el schema de base de datos MySQL para almacenar candidatos, skills, y CVs parseados. Debe soportar datos estructurados y semi-estructurados (JSON para parsed_resume).

#### Tareas Técnicas
- [ ] Diseñar schema completo con relaciones (CANDIDATE, CANDIDATE_SKILL, SKILLS, PARSED_CV_CACHE)
- [ ] Crear tabla `candidates` con campos: id (UUID), email (UNIQUE), full_name, phone, location, current_title, current_company, years_experience, linkedin_url, resume_url (S3), parsed_resume (JSON), created_at, updated_at
- [ ] Crear tabla `skills` con diccionario maestro: id, skill_name (UNIQUE), category, synonyms (JSON), created_at
- [ ] Crear tabla `candidate_skills`: id, candidate_id (FK), skill_id (FK), proficiency_level (enum), years_experience, is_verified, source (parsed/manual)
- [ ] Crear tabla `parsed_cv_cache`: id, file_hash (UNIQUE), parsed_data (JSON), confidence_score, created_at, expires_at
- [ ] Crear índices: idx_candidates_email, idx_candidates_created_at, idx_skills_name, idx_candidate_skills_composite
- [ ] Implementar migrations con herramienta (Knex.js, TypeORM migrations, o similar)
- [ ] Seed de diccionario inicial de skills técnicos (mínimo 200 skills comunes en tech)

#### Criterios de Aceptación Técnicos
- [ ] Todas las tablas creadas con constraints (FK, UNIQUE, NOT NULL apropiados)
- [ ] Migrations ejecutan correctamente en MySQL 8.0
- [ ] Rollback de migrations funciona sin errores
- [ ] Índices creados mejoran queries en >50% (medido con EXPLAIN)
- [ ] JSON schema válido para campos parsed_resume y synonyms
- [ ] Seed data carga correctamente 200+ skills

#### Dependencias
- Depende de: Ninguna (primer ticket)
- Bloquea a: US-001-02, US-001-03, US-001-06

#### Definición Técnica de Hecho
- [ ] Schema documentado en archivo `schema.sql` con comentarios
- [ ] Migrations versionadas (ej: `001_create_candidates_table.ts`)
- [ ] Tests de integración para constraints (FK violations, unique violations)
- [ ] Script de seed data en `seeds/001_initial_skills.ts`
- [ ] ER Diagram exportado (usando herramienta como dbdiagram.io)
- [ ] README con instrucciones de setup de DB
- [ ] Code review aprobado

#### Notas de Implementación
- **Tipo de datos para JSON**: Usar tipo `JSON` nativo de MySQL 8.0 (no TEXT)
- **UUIDs**: Usar `CHAR(36)` o `BINARY(16)` con funciones UUID_TO_BIN/BIN_TO_UUID
- **Particionamiento**: Considerar partition por `created_at` si se anticipa alto volumen (>1M registros)
- **Soft deletes**: Agregar campo `deleted_at` para candidatos (no borrado físico)
- **Skill synonyms format**: JSON array: `["React", "ReactJS", "React.js", "react"]`

#### Artefactos
- [ ] `backend/database/migrations/001_create_candidates.ts`
- [ ] `backend/database/migrations/002_create_skills.ts`
- [ ] `backend/database/migrations/003_create_candidate_skills.ts`
- [ ] `backend/database/migrations/004_create_parsed_cv_cache.ts`
- [ ] `backend/database/seeds/001_initial_skills.ts`
- [ ] `backend/database/schema.sql` (consolidated schema)
- [ ] `docs/database/er-diagram.png`

---

### TICKET US-001-02: Backend API - File Upload Endpoints

**Tipo**: Feature
**Componente**: Backend
**Asignado a**: Backend Dev
**Estimación**: 3 días

#### Descripción
Implementar endpoints REST en Node.js/Express para manejar upload de CVs (PDF, DOCX, TXT), validación de archivos, y storage en S3. Incluye generación de URLs firmadas y metadata.

#### Tareas Técnicas
- [ ] Configurar multer middleware para manejo de multipart/form-data (límite 10MB)
- [ ] Implementar POST `/api/v1/candidates/upload-cv` con validación de tipo de archivo
- [ ] Validar extensiones permitidas: .pdf, .docx, .doc, .txt (whitelist MIME types)
- [ ] Generar hash SHA-256 del archivo para deduplicación
- [ ] Configurar AWS SDK v3 para S3 (o MinIO para desarrollo local)
- [ ] Upload archivo a S3 bucket con estructura: `cvs/{year}/{month}/{uuid}-{hash}.{ext}`
- [ ] Generar presigned URL con expiración de 24h para acceso temporal
- [ ] Crear registro en tabla `candidates` con status "pending_parse"
- [ ] Publicar mensaje a RabbitMQ queue `cv-parse-jobs` con metadata
- [ ] Implementar POST `/api/v1/candidates/upload-linkedin` para URLs de LinkedIn
- [ ] Validar formato de URL de LinkedIn (regex)
- [ ] Responder con `201 Created` y job_id para tracking de progreso

#### Criterios de Aceptación Técnicos
- [ ] Archivos >10MB rechazados con error 413 Payload Too Large
- [ ] Tipos de archivo no permitidos rechazados con error 400 Bad Request
- [ ] Upload exitoso retorna job_id y status URL (`/api/v1/jobs/{job_id}`)
- [ ] Duplicados detectados por hash retornan datos cached (evitar re-parsing)
- [ ] S3 upload falla gracefully con retry (3 intentos, exponential backoff)
- [ ] Rate limiting configurado: max 10 uploads/minuto por usuario
- [ ] Validación de autenticación con JWT middleware
- [ ] Logs estructurados con correlation IDs (winston + morgan)

#### Dependencias
- Depende de: US-001-01 (Database Schema)
- Bloquea a: US-001-04, US-001-05

#### Definición Técnica de Hecho
- [ ] Código implementado en `backend/src/routes/candidates.routes.ts`
- [ ] Controller en `backend/src/controllers/candidates.controller.ts`
- [ ] Service layer en `backend/src/services/upload.service.ts`
- [ ] Tests unitarios para validaciones (>80% cobertura)
- [ ] Tests de integración con S3 mock (usando localstack o aws-sdk-mock)
- [ ] Tests E2E con Supertest simulando upload real
- [ ] Swagger/OpenAPI documentation actualizada
- [ ] Variables de entorno documentadas en `.env.example`
- [ ] Error handling con códigos HTTP apropiados
- [ ] Security audit: validación de MIME type real (no solo extensión)

#### Notas de Implementación
- **Multipart parsing**: Usar `multer` con storage en memoria (no disco), luego stream a S3
- **S3 Bucket naming**: `lti-cvs-{env}` (lti-cvs-dev, lti-cvs-prod)
- **File hash**: Calcular con `crypto.createHash('sha256').update(fileBuffer).digest('hex')`
- **Deduplicación**: Buscar en `parsed_cv_cache` por file_hash antes de parsear
- **LinkedIn URL validation**: Regex: `^https://www\.linkedin\.com/in/[\w-]+/?$`
- **RabbitMQ message format**: `{jobId, candidateId, fileUrl, fileType, uploadedBy, timestamp}`
- **Presigned URLs**: Usar `@aws-sdk/s3-request-presigner` con expiración configurable

#### Artefactos
- [ ] `backend/src/routes/candidates.routes.ts`
- [ ] `backend/src/controllers/candidates.controller.ts`
- [ ] `backend/src/services/upload.service.ts`
- [ ] `backend/src/middleware/file-validation.middleware.ts`
- [ ] `backend/src/config/s3.config.ts`
- [ ] `backend/src/utils/file-hash.util.ts`
- [ ] `backend/tests/integration/upload.test.ts`
- [ ] `backend/docs/api/upload-endpoints.md`

---

### TICKET US-001-03: Backend API - Job Status & Results Endpoints

**Tipo**: Feature
**Componente**: Backend
**Asignado a**: Backend Dev
**Estimación**: 2 días

#### Descripción
Implementar endpoints para consultar estado de jobs de parseo y recuperar resultados parseados. Incluye endpoints para editar datos extraídos antes de confirmar.

#### Tareas Técnicas
- [ ] Implementar GET `/api/v1/jobs/{jobId}` para estado de job (pending, processing, completed, failed)
- [ ] Implementar GET `/api/v1/candidates/{candidateId}/parsed-data` para datos extraídos
- [ ] Implementar PUT `/api/v1/candidates/{candidateId}/parsed-data` para ediciones manuales
- [ ] Implementar POST `/api/v1/candidates/{candidateId}/confirm` para confirmar y persistir datos
- [ ] Implementar GET `/api/v1/candidates/{candidateId}/duplicates` para candidatos similares
- [ ] Crear tabla `parse_jobs`: id, candidate_id, status, progress (%), error_message, started_at, completed_at
- [ ] Implementar lógica de detección de duplicados (email exact match, nombre+teléfono fuzzy match)
- [ ] Soporte para paginación en listado de candidatos duplicados
- [ ] WebSocket endpoint (opcional) para real-time updates de progreso

#### Criterios de Aceptación Técnicos
- [ ] GET /jobs/{jobId} retorna status actualizado en <200ms
- [ ] Datos parseados incluyen confidence score por campo (0-100)
- [ ] Ediciones manuales marcan campos como "manually_edited: true" en JSON
- [ ] Confirmación dispara evento "candidate_confirmed" para US-005 (Screening)
- [ ] Duplicados detectados con algoritmo fuzzy (Levenshtein distance <3 para nombre)
- [ ] Response time <500ms para detección de duplicados en base de 10k candidatos
- [ ] Validación de ownership: solo el usuario que creó el candidato puede editarlo

#### Dependencias
- Depende de: US-001-01 (Database Schema), US-001-02 (Upload API)
- Bloquea a: US-001-07 (Frontend UI)

#### Definición Técnica de Hecho
- [ ] Endpoints implementados con OpenAPI spec completa
- [ ] Tests unitarios para lógica de duplicados (>80% cobertura)
- [ ] Tests de integración para flujo completo: upload → poll status → get data → edit → confirm
- [ ] Rate limiting: max 100 requests/minuto por usuario para polling
- [ ] Caching de parsed data con Redis (TTL 1 hora)
- [ ] Logs de auditoría para ediciones manuales
- [ ] Documentation actualizada con ejemplos de payloads
- [ ] Code review aprobado

#### Notas de Implementación
- **Job status enum**: `PENDING | PROCESSING | COMPLETED | FAILED | CANCELLED`
- **Progress tracking**: Actualizar progress en intervalos: 0% (queued), 25% (parsing), 50% (NLP), 75% (normalization), 100% (done)
- **Fuzzy matching**: Usar biblioteca `fuzzball` o `leven` para Levenshtein distance
- **WebSocket (opcional)**: Usar `socket.io` con rooms por jobId: `socket.join(`job:${jobId}`)`
- **Confidence score calculation**: Agregar scores por campo: `{full_name: 95, email: 100, skills: 78}`
- **Manual edits tracking**: `{field: "skills", original_value: [...], edited_value: [...], edited_by: userId, edited_at: timestamp}`

#### Artefactos
- [ ] `backend/src/routes/jobs.routes.ts`
- [ ] `backend/src/routes/candidates-data.routes.ts`
- [ ] `backend/src/services/job-status.service.ts`
- [ ] `backend/src/services/duplicate-detection.service.ts`
- [ ] `backend/src/utils/fuzzy-match.util.ts`
- [ ] `backend/tests/unit/duplicate-detection.test.ts`
- [ ] `backend/docs/api/job-status-endpoints.md`

---

### TICKET US-001-04: Message Queue - RabbitMQ Setup & Consumers

**Tipo**: Technical
**Componente**: Backend/Infrastructure
**Asignado a**: Backend Dev
**Estimación**: 2 días

#### Descripción
Configurar RabbitMQ para procesamiento asíncrono de jobs de parseo. Implementar producer (desde API) y consumer (que delega a Parser Service). Incluye dead letter queues y retry logic.

#### Tareas Técnicas
- [ ] Setup RabbitMQ server en Docker Compose (imagen oficial `rabbitmq:3.12-management`)
- [ ] Crear exchange `cv-processing-exchange` (tipo: direct)
- [ ] Crear queue `cv-parse-jobs` con binding a exchange
- [ ] Crear dead letter queue `cv-parse-jobs-dlq` para mensajes fallidos
- [ ] Implementar producer en Node.js usando `amqplib` (publicar mensaje en upload exitoso)
- [ ] Implementar consumer en Node.js que escucha `cv-parse-jobs`
- [ ] Consumer hace HTTP request al Parser Service (Python) con file URL
- [ ] Configurar retry logic: 3 reintentos con exponential backoff (1min, 5min, 15min)
- [ ] Mensajes con 3 fallos van a DLQ y marcan job como "failed"
- [ ] Implementar monitoring de queue depth (alarma si >100 mensajes pendientes)
- [ ] Configurar prefetch count = 5 (procesar max 5 mensajes concurrentes)

#### Criterios de Aceptación Técnicos
- [ ] Mensajes persisten en disco (durable: true) para evitar pérdida en crash
- [ ] Consumer ACK manual (autoAck: false) solo después de parsing exitoso
- [ ] Dead letter queue captura mensajes fallidos con metadata de error
- [ ] Retry logic implementado con headers `x-retry-count`
- [ ] RabbitMQ Management UI accesible en http://localhost:15672
- [ ] Healthcheck endpoint `/api/v1/health/rabbitmq` retorna estado de conexión
- [ ] Graceful shutdown: consumer procesa mensajes en flight antes de terminar

#### Dependencias
- Depende de: US-001-02 (Upload API - producer), US-001-05 (Parser Service - consumer target)
- Bloquea a: Ninguno (habilita procesamiento asíncrono)

#### Definición Técnica de Hecho
- [ ] RabbitMQ configurado en `docker-compose.yml` con volumes persistentes
- [ ] Producer implementado en `backend/src/services/queue/producer.service.ts`
- [ ] Consumer implementado en `backend/src/services/queue/consumer.service.ts`
- [ ] Tests de integración: publicar mensaje → consumer procesa → ACK
- [ ] Tests de retry logic con mensaje fallido simulado
- [ ] Monitoring script que consulta queue depth cada 1 minuto
- [ ] Documentation de arquitectura de colas
- [ ] Variables de entorno para configuración (RABBITMQ_URL, QUEUE_NAME, etc.)
- [ ] Code review aprobado

#### Notas de Implementación
- **Connection string**: `amqp://user:password@localhost:5672`
- **Message payload format**:
  ```json
  {
    "jobId": "uuid-v4",
    "candidateId": "uuid-v4",
    "fileUrl": "https://s3.../cv.pdf",
    "fileType": "application/pdf",
    "uploadedBy": "userId",
    "timestamp": "ISO-8601",
    "attemptNumber": 1
  }
  ```
- **Retry headers**: `x-retry-count`, `x-first-death-queue`, `x-first-death-reason`
- **Exponential backoff**: Usar `message TTL` con valores crecientes: 60000ms, 300000ms, 900000ms
- **Graceful shutdown**: Escuchar `SIGTERM` y llamar `channel.close()` después de `channel.waitForConfirms()`
- **Connection recovery**: Auto-reconnect con `heartbeat: 60` seconds

#### Artefactos
- [ ] `docker-compose.yml` (RabbitMQ service)
- [ ] `backend/src/services/queue/producer.service.ts`
- [ ] `backend/src/services/queue/consumer.service.ts`
- [ ] `backend/src/config/rabbitmq.config.ts`
- [ ] `backend/tests/integration/queue.test.ts`
- [ ] `docs/architecture/message-queue.md`
- [ ] `scripts/rabbitmq-monitor.sh` (monitoring script)

---

### TICKET US-001-05: Parser Service - Python FastAPI Core

**Tipo**: Feature
**Componente**: AI/Parser Service
**Asignado a**: Backend Dev (Python)
**Estimación**: 5 días

#### Descripción
Desarrollar servicio de parseo en Python + FastAPI que extrae datos estructurados de CVs (PDF, DOCX, TXT). Incluye extracción de texto, NER con spaCy, normalización de skills, y cálculo de confidence scores.

#### Tareas Técnicas
- [ ] Crear proyecto FastAPI con estructura modular (routers, services, models)
- [ ] Implementar POST `/parse` endpoint que recibe URL de archivo S3
- [ ] Download archivo desde S3 usando boto3
- [ ] **PDF Parsing**: Implementar con PyPDF2 para PDFs simples, pdfplumber para tablas
- [ ] **DOCX Parsing**: Implementar con python-docx
- [ ] **TXT Parsing**: Lectura directa con encoding detection (chardet)
- [ ] **NLP Extraction**: Usar spaCy NER para extraer:
  - Nombre completo (PERSON entities)
  - Email (regex: `r'[\w\.-]+@[\w\.-]+\.\w+'`)
  - Teléfono (regex con variaciones internacionales)
  - Ubicación (GPE entities)
  - Skills técnicos (custom entity recognizer)
  - Educación (custom patterns: universidad, degree, año)
  - Experiencia laboral (custom patterns: empresa, cargo, fechas)
- [ ] Implementar normalización de skills usando diccionario de MySQL (consulta via HTTP a API backend)
- [ ] Calcular confidence score por campo (basado en número de matches y posición en CV)
- [ ] Guardar resultado parseado en MySQL (via API backend POST /candidates/{id}/parsed-data)
- [ ] Cachear resultado en Redis por file_hash (TTL 7 días)
- [ ] Manejo de errores: OCR fallback para PDFs escaneados (AWS Textract API)

#### Criterios de Aceptación Técnicos
- [ ] Parseo completo de CV simple (<5 páginas) en <10 segundos
- [ ] Precisión >85% en campos críticos (nombre, email, skills) medida en dataset de 100 CVs
- [ ] Manejo de PDFs multi-columna sin pérdida de contexto
- [ ] Extracción de mínimo 10 skills por CV (si existen en texto)
- [ ] Response JSON incluye confidence scores por campo
- [ ] OCR fallback se activa automáticamente para PDFs con <50% texto extraíble
- [ ] Service responde con 503 Service Unavailable si spaCy model no cargado

#### Dependencias
- Depende de: US-001-01 (Database), US-001-02 (S3 URLs)
- Bloquea a: US-001-04 (Consumer depende de este servicio), US-001-06 (NLP Training)

#### Definición Técnica de Hecho
- [ ] FastAPI service corriendo en contenedor Docker
- [ ] Endpoint `/parse` documentado con OpenAPI (auto-generado por FastAPI)
- [ ] Tests unitarios para cada parser (PDF, DOCX, TXT) con >80% cobertura
- [ ] Tests de integración con archivos reales de diferentes formatos
- [ ] Performance testing: 100 CVs procesados en <15 minutos (paralelizado)
- [ ] spaCy models pre-descargados en imagen Docker (no download en runtime)
- [ ] Logging estructurado con correlation IDs (loguru)
- [ ] Health check endpoint `/health` con status de dependencies
- [ ] Dockerfile optimizado con multi-stage build (<500MB imagen final)
- [ ] Code review aprobado

#### Notas de Implementación
- **spaCy models**: Descargar `es_core_news_lg` y `en_core_web_lg` en Dockerfile
- **Custom NER training**: Opcional para skills, ver US-001-06 si es necesario
- **Skills extraction fallback**: Si NER falla, usar keyword matching con lista de 500+ skills comunes
- **Confidence score formula**:
  ```python
  confidence = min(100, (
      (num_matches / expected_matches) * 50 +
      (field_found_in_first_half ? 30 : 10) +
      (exact_match ? 20 : fuzzy_match ? 10 : 0)
  ))
  ```
- **OCR activation**: Si `len(extracted_text) < len(pdf_pages) * 100` caracteres
- **Textract cost**: ~$1.50 per 1000 pages, usar solo cuando necesario
- **Response format**:
  ```json
  {
    "candidateId": "uuid",
    "parsedData": {
      "full_name": {"value": "John Doe", "confidence": 95},
      "email": {"value": "john@example.com", "confidence": 100},
      "skills": [
        {"name": "Python", "normalized": "Python", "confidence": 90},
        {"name": "React.js", "normalized": "React", "confidence": 85}
      ],
      "experience": [...],
      "education": [...]
    },
    "overallConfidence": 87,
    "processingTime": 8.3
  }
  ```

#### Artefactos
- [ ] `parser-service/app/main.py` (FastAPI app)
- [ ] `parser-service/app/routers/parse.py`
- [ ] `parser-service/app/services/pdf_parser.py`
- [ ] `parser-service/app/services/docx_parser.py`
- [ ] `parser-service/app/services/nlp_extractor.py`
- [ ] `parser-service/app/services/skills_normalizer.py`
- [ ] `parser-service/app/utils/confidence_calculator.py`
- [ ] `parser-service/Dockerfile`
- [ ] `parser-service/requirements.txt`
- [ ] `parser-service/tests/test_parsers.py`
- [ ] `parser-service/tests/fixtures/sample_cvs/` (PDFs, DOCXs de prueba)
- [ ] `parser-service/docs/api-spec.yaml`

---

### TICKET US-001-06: NLP Training - Custom spaCy Entity Recognizer

**Tipo**: Technical
**Componente**: AI/ML
**Asignado a**: ML Engineer / Backend Dev (Python)
**Estimación**: 3 días

#### Descripción
Entrenar modelo custom de spaCy NER para mejorar detección de skills técnicos, títulos de trabajo, y empresas en CVs. Fine-tuning sobre modelos pre-entrenados.

#### Tareas Técnicas
- [ ] Recolectar dataset de entrenamiento: 100+ CVs anonimizados etiquetados manualmente
- [ ] Etiquetar entidades custom: SKILL, JOB_TITLE, COMPANY, DEGREE, CERTIFICATION
- [ ] Usar herramienta de anotación (Label Studio, Prodigy, o Doccano)
- [ ] Exportar anotaciones en formato spaCy training (JSON)
- [ ] Implementar training script con spaCy CLI o API de entrenamiento
- [ ] Fine-tune `es_core_news_lg` y `en_core_web_lg` con datos custom
- [ ] Evaluar modelo en test set: medir precision, recall, F1-score por entidad
- [ ] Target: F1 >0.80 para SKILL, >0.75 para JOB_TITLE
- [ ] Versionar modelos con MLflow o DVC
- [ ] Integrar modelo entrenado en Parser Service (reemplazar base model)

#### Criterios de Aceptación Técnicos
- [ ] Dataset de training: mínimo 100 CVs, 1000+ entidades etiquetadas
- [ ] Modelo entrenado mejora F1 en >10% vs modelo base para SKILL entities
- [ ] Training pipeline automatizado con script reproducible
- [ ] Modelo entrenado <200MB (para no inflar imagen Docker)
- [ ] Evaluación en test set (20% de datos) documentada con métricas
- [ ] Modelo versionado con tag (ej: `skill-ner-v1.0`)

#### Dependencias
- Depende de: US-001-05 (Parser Service base)
- Bloquea a: Ninguno (mejora incremental)

#### Definición Técnica de Hecho
- [ ] Dataset de training en `parser-service/training/data/annotated_cvs.json`
- [ ] Training script en `parser-service/training/train_ner.py`
- [ ] Modelo entrenado en `parser-service/models/skill_ner_v1/`
- [ ] Evaluation report en `parser-service/training/evaluation_report.md`
- [ ] Tests con modelo nuevo vs modelo base (comparación de métricas)
- [ ] Documentation de proceso de training y actualización de modelo
- [ ] Modelo integrado en Parser Service y desplegado
- [ ] Code review aprobado

#### Notas de Implementación
- **Annotation tool**: Usar Prodigy (si hay licencia) o Label Studio (open-source)
- **Training config**:
  ```python
  config = {
      "model": "es_core_news_lg",  # base model
      "new_entity_labels": ["SKILL", "JOB_TITLE", "COMPANY", "DEGREE"],
      "iterations": 30,
      "dropout": 0.2,
      "batch_size": 32
  }
  ```
- **Data augmentation**: Considerar técnicas de augmentation para skills (sinónimos, abreviaciones)
- **Evaluation metrics**: Usar `spacy evaluate` o implementar custom con sklearn.metrics
- **Versionado**: Nombrar modelos con semantic versioning: `skill_ner_v{major}.{minor}.{patch}`
- **Deployment**: Reemplazar modelo en Dockerfile con `COPY models/skill_ner_v1 /app/models/`

#### Artefactos
- [ ] `parser-service/training/data/annotated_cvs.json`
- [ ] `parser-service/training/train_ner.py`
- [ ] `parser-service/training/evaluate_ner.py`
- [ ] `parser-service/models/skill_ner_v1/` (modelo entrenado)
- [ ] `parser-service/training/evaluation_report.md`
- [ ] `parser-service/docs/model-training-guide.md`

---

### TICKET US-001-07: Frontend UI - CV Upload Component

**Tipo**: Feature
**Componente**: Frontend
**Asignado a**: Frontend Dev
**Estimación**: 4 días

#### Descripción
Desarrollar componente React para upload de CVs con drag-and-drop, validación de archivos, barra de progreso, y preview de datos parseados. Incluye UI para edición manual de campos extraídos.

#### Tareas Técnicas
- [ ] Crear componente `CVUploadZone` con drag-and-drop usando `react-dropzone`
- [ ] Validación client-side: tipo de archivo, tamaño <10MB
- [ ] Mostrar preview de archivo seleccionado con metadata (nombre, tamaño, tipo)
- [ ] Implementar upload con `axios` o `fetch` a `/api/v1/candidates/upload-cv`
- [ ] Mostrar barra de progreso durante upload (usando `onUploadProgress`)
- [ ] Polling de estado de job con intervalo de 2 segundos (GET `/api/v1/jobs/{jobId}`)
- [ ] Mostrar spinner con mensaje "Parsing CV... 45%" durante procesamiento
- [ ] Componente `ParsedDataPreview` para mostrar datos extraídos en cards/sections
- [ ] Highlight de campos con baja confianza (<70%) en color naranja
- [ ] Componente `EditableField` para editar manualmente cualquier campo
- [ ] Botón "Confirm & Save" que llama POST `/api/v1/candidates/{id}/confirm`
- [ ] Manejo de errores: mostrar mensaje si parsing falla con opción de "Upload another"
- [ ] Detección de duplicados: modal de advertencia si se encuentran candidatos similares

#### Criterios de Aceptación Técnicos
- [ ] Drag-and-drop funciona en Chrome, Firefox, Safari, Edge
- [ ] Validaciones client-side previenen upload de archivos inválidos
- [ ] Progress bar actualiza suavemente (no saltos bruscos)
- [ ] Polling se detiene automáticamente cuando job status = "completed" o "failed"
- [ ] UI responsive: funciona en desktop (1920x1080) y tablet (768x1024)
- [ ] Loading states apropiados en todos los async calls
- [ ] Accesibilidad: navegación con teclado, aria-labels en controles
- [ ] Ediciones manuales se guardan localmente antes de confirmar (prevent data loss)

#### Dependencias
- Depende de: US-001-02 (Upload API), US-001-03 (Status API)
- Bloquea a: US-001-09 (E2E Tests)

#### Definición Técnica de Hecho
- [ ] Componente implementado en `frontend/src/components/CVUpload/`
- [ ] Storybook stories para componente (aislado, diferentes estados)
- [ ] Tests unitarios con React Testing Library (>80% cobertura)
- [ ] Tests de integración con MSW (Mock Service Worker) para APIs
- [ ] Accesibilidad auditada con axe-core (0 violations)
- [ ] Performance: Time to Interactive <3 segundos en 3G
- [ ] Code review aprobado
- [ ] Documentation en Storybook con ejemplos de uso

#### Notas de Implementación
- **Libraries**:
  - `react-dropzone` para drag-and-drop
  - `axios` con interceptors para progress
  - `react-query` para polling y cache de status
  - `react-hook-form` para edición de campos
  - UI library: Material-UI o Chakra UI (consistente con design system)
- **Polling strategy**: Usar `setInterval` que se limpia en cleanup, o `react-query` con `refetchInterval`
- **Optimistic UI**: Mostrar datos parseados inmediatamente cuando confidence >90%, editable de todos modos
- **Error messages**: Mensajes user-friendly, no exponer detalles técnicos
- **Duplicate modal**: Mostrar lista de duplicados con botones "Use existing" o "Create new anyway"
- **File preview**: Mostrar ícono según tipo de archivo (PDF, Word, etc.)

#### Artefactos
- [ ] `frontend/src/components/CVUpload/CVUploadZone.tsx`
- [ ] `frontend/src/components/CVUpload/ParsedDataPreview.tsx`
- [ ] `frontend/src/components/CVUpload/EditableField.tsx`
- [ ] `frontend/src/components/CVUpload/DuplicateWarningModal.tsx`
- [ ] `frontend/src/hooks/useCVUpload.ts` (custom hook para lógica)
- [ ] `frontend/src/hooks/useJobPolling.ts`
- [ ] `frontend/src/api/candidates.api.ts` (API client)
- [ ] `frontend/src/components/CVUpload/CVUpload.stories.tsx`
- [ ] `frontend/src/components/CVUpload/__tests__/CVUpload.test.tsx`
- [ ] `frontend/docs/components/cv-upload.md`

---

### TICKET US-001-08: Integration - LinkedIn Profile Scraper

**Tipo**: Feature
**Componente**: Parser Service
**Asignado a**: Backend Dev (Python)
**Estimación**: 3 días

#### Descripción
Implementar scraper para extraer datos de perfiles públicos de LinkedIn. Usar API oficial si está disponible, fallback a scraping autorizado. Incluye rate limiting y manejo de anti-bot.

#### Tareas Técnicas
- [ ] Investigar LinkedIn API oficial (requiere partnership) - prioridad 1
- [ ] Si API disponible: implementar OAuth flow y calls a Profile API
- [ ] Si API NO disponible: implementar scraper con `playwright` o `selenium`
- [ ] Scraper debe extraer: nombre, headline, ubicación, empresa actual, educación, skills, experiencia
- [ ] Implementar rate limiting: max 10 profiles/minuto (evitar ban)
- [ ] Manejo de CAPTCHA: detección y fallback a manual review
- [ ] Guardar HTML source en S3 para auditoría (opcional, considerar privacidad)
- [ ] Parsear HTML extraído con BeautifulSoup o lxml
- [ ] Normalizar datos extraídos al mismo formato que parser de PDF
- [ ] Implementar caching de profiles por URL (evitar re-scraping)

#### Criterios de Aceptación Técnicos
- [ ] Extracción exitosa de >90% de campos en perfiles públicos estándar
- [ ] Detección de perfiles privados/no accesibles con mensaje claro
- [ ] Rate limiting implementado con queue interna (no exceder límites)
- [ ] Scraper resiliente a cambios de estructura HTML (usar selectores robustos)
- [ ] Timeout de 30 segundos por profile (evitar hang indefinido)
- [ ] Logs de cada scraping attempt (success, failed, reason)

#### Dependencias
- Depende de: US-001-02 (API para recibir URL), US-001-05 (Parser Service base)
- Bloquea a: Ninguno (feature complementaria)

#### Definición Técnica de Hecho
- [ ] LinkedIn scraper implementado en `parser-service/app/services/linkedin_scraper.py`
- [ ] Tests unitarios con HTML fixtures (>80% cobertura)
- [ ] Tests de integración con perfiles públicos reales (3-5 ejemplos)
- [ ] Documentation de limitaciones y requirements (API key si necesario)
- [ ] Error handling para casos edge (profile deleted, private, etc.)
- [ ] Compliance check: scraping autorizado, respeta robots.txt
- [ ] Code review aprobado

#### Notas de Implementación
- **API vs Scraping**: Preferir API oficial si está disponible (más estable, legal)
- **Scraping libraries**:
  - `playwright` (recomendado, headless browser con anti-detection)
  - `selenium` (alternativa)
  - `beautifulsoup4` + `lxml` para parsing
- **Selectors**: Usar data attributes o classes estables, evitar IDs generados
- **Rate limiting**: Implementar token bucket con `asyncio.Semaphore` o `ratelimit` library
- **CAPTCHA handling**: Si detectado, marcar job como "manual_review_required"
- **Legal compliance**: Revisar Terms of Service de LinkedIn, scraping de datos públicos generalmente OK pero gris area
- **User-Agent**: Usar User-Agent realista: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36`
- **Cookies**: Considerar login con cuenta de servicio para acceder a más datos (requiere mantenimiento de sesión)

#### Artefactos
- [ ] `parser-service/app/services/linkedin_scraper.py`
- [ ] `parser-service/app/utils/linkedin_parser.py`
- [ ] `parser-service/tests/test_linkedin_scraper.py`
- [ ] `parser-service/tests/fixtures/linkedin_profiles.html`
- [ ] `parser-service/docs/linkedin-integration.md`

---

### TICKET US-001-09: Testing - E2E & Performance Tests

**Tipo**: Testing
**Componente**: Full Stack
**Asignado a**: QA / FullStack Dev
**Estimación**: 3 días

#### Descripción
Implementar suite completa de tests end-to-end que cubren flujo completo de upload de CV hasta confirmación de datos. Incluye performance tests para validar SLA de <30 segundos.

#### Tareas Técnicas
- [ ] Setup Playwright o Cypress para E2E tests
- [ ] Implementar test: "Upload PDF CV → Poll status → View parsed data → Edit field → Confirm"
- [ ] Implementar test: "Upload DOCX CV → Verify all fields extracted → Check duplicates"
- [ ] Implementar test: "Upload invalid file → Verify error message"
- [ ] Implementar test: "Upload LinkedIn URL → Verify scraping → Confirm data"
- [ ] Performance test: Upload 10 CVs concurrentemente, verificar <30s cada uno
- [ ] Performance test: Upload 100 CVs, medir throughput (CVs/minuto)
- [ ] Load test con Artillery o k6: simular 50 usuarios concurrentes uploading
- [ ] Implementar smoke tests para validar deployment: "Can upload and parse basic CV"
- [ ] Integrar E2E tests en CI/CD pipeline (GitHub Actions o similar)

#### Criterios de Aceptación Técnicos
- [ ] E2E tests cubren happy path y 3+ error scenarios
- [ ] Tests pasan consistentemente (>95% success rate en 10 runs)
- [ ] Performance tests validan SLA: 95% de CVs parseados en <30 segundos
- [ ] Load tests identifican límite de capacidad (ej: max 200 concurrent uploads)
- [ ] Tests ejecutan en <10 minutos totales (paralelizados)
- [ ] Screenshots/videos capturados en fallos para debugging
- [ ] Reportes de tests generados en formato HTML (Playwright report, Mochawesome, etc.)

#### Dependencias
- Depende de: US-001-07 (Frontend UI), US-001-05 (Parser Service), US-001-02 (Backend API)
- Bloquea a: Ninguno (completa testing suite)

#### Definición Técnica de Hecho
- [ ] E2E tests implementados en `tests/e2e/cv-upload.spec.ts`
- [ ] Performance tests en `tests/performance/cv-parsing-load.js` (k6 o Artillery)
- [ ] Tests integrados en CI pipeline (ejecutan en cada PR)
- [ ] Test data fixtures en `tests/fixtures/sample_cvs/`
- [ ] Documentation de cómo ejecutar tests localmente
- [ ] Threshold configurado en CI: tests deben pasar para merge
- [ ] Code coverage report generado (combine frontend + backend)
- [ ] Code review aprobado

#### Notas de Implementación
- **E2E tool selection**: Playwright (recomendado, multi-browser, video recording) vs Cypress (mejor DX pero solo Chromium)
- **Test data**: Usar CVs anonimizados o generados sintéticamente
- **Performance thresholds**: Configurar en k6 script:
  ```javascript
  export let options = {
    thresholds: {
      'http_req_duration': ['p(95)<30000'], // 95% under 30s
      'http_req_failed': ['rate<0.05'], // <5% failures
    }
  };
  ```
- **CI integration**: Usar GitHub Actions con service containers para MySQL, RabbitMQ, Redis
- **Flaky tests prevention**: Usar waiters apropiados (waitForSelector, waitForResponse), evitar sleeps arbitrarios
- **Parallelization**: Ejecutar E2E tests en paralelo con sharding (Playwright workers, Cypress parallel)

#### Artefactos
- [ ] `tests/e2e/cv-upload.spec.ts`
- [ ] `tests/e2e/cv-duplicate-detection.spec.ts`
- [ ] `tests/performance/cv-parsing-load.js`
- [ ] `tests/fixtures/sample_cvs/` (10+ CVs de diferentes formatos)
- [ ] `.github/workflows/e2e-tests.yml` (CI config)
- [ ] `tests/README.md` (guía de testing)
- [ ] `playwright.config.ts` o `cypress.config.ts`

---

### TICKET US-001-10: DevOps - Docker Compose & Local Development Setup

**Tipo**: Technical
**Componente**: DevOps
**Asignado a**: DevOps / FullStack Dev
**Estimación**: 2 días

#### Descripción
Crear configuración completa de Docker Compose para desarrollo local que incluya todos los servicios: frontend, backend, parser service, MySQL, RabbitMQ, Redis, S3 (MinIO). Incluye scripts de inicialización y seeding.

#### Tareas Técnicas
- [ ] Crear `docker-compose.yml` con todos los servicios
- [ ] Service `mysql`: imagen oficial, volume para persistencia, init script para schema
- [ ] Service `rabbitmq`: imagen con management plugin, puertos 5672 (AMQP) y 15672 (UI)
- [ ] Service `redis`: imagen oficial, volume para persistencia (opcional)
- [ ] Service `minio`: S3-compatible storage, crear bucket `lti-cvs-dev` en startup
- [ ] Service `backend`: Node.js app, hot-reload con volumes, depends_on mysql+rabbitmq+redis
- [ ] Service `parser-service`: Python FastAPI, hot-reload con volumes, depends_on mysql+s3
- [ ] Service `frontend`: React app en dev mode, proxy a backend, hot-reload
- [ ] Health checks para todos los servicios (verificar ready antes de depends_on)
- [ ] Networking: todos los servicios en misma red `lti-network`
- [ ] Environment variables en `.env.example` con valores por defecto
- [ ] Script `scripts/dev-setup.sh` que ejecuta migrations, seeds, y crea bucket S3
- [ ] README con instrucciones paso a paso: `docker-compose up`, acceder a servicios

#### Criterios de Aceptación Técnicos
- [ ] `docker-compose up` inicia todos los servicios sin errores
- [ ] Health checks pasan para todos los servicios en <2 minutos
- [ ] Frontend accesible en http://localhost:3000
- [ ] Backend API accesible en http://localhost:4000
- [ ] Parser Service accesible en http://localhost:8000
- [ ] RabbitMQ Management UI en http://localhost:15672 (user: guest, pass: guest)
- [ ] MinIO Console en http://localhost:9001 (user: minioadmin, pass: minioadmin)
- [ ] Hot-reload funciona: cambio en código se refleja sin reiniciar contenedor
- [ ] Volúmenes persisten data: `docker-compose down` + `up` no pierde datos

#### Dependencias
- Depende de: US-001-01 (DB Schema), US-001-02 (Backend), US-001-05 (Parser), US-001-07 (Frontend)
- Bloquea a: Ninguno (habilita desarrollo local fácil)

#### Definición Técnica de Hecho
- [ ] `docker-compose.yml` configurado con todos los servicios
- [ ] `docker-compose.override.yml` para configuraciones de desarrollo
- [ ] `.env.example` con todas las variables necesarias
- [ ] `scripts/dev-setup.sh` ejecuta migrations y seeds
- [ ] `scripts/dev-teardown.sh` limpia volumes y data
- [ ] README actualizado con sección "Local Development Setup"
- [ ] Troubleshooting guide para problemas comunes
- [ ] Code review aprobado

#### Notas de Implementación
- **Docker Compose version**: Usar v3.8+ (soporta health checks mejorados)
- **Volumes**: Nombrar volumes explícitamente (no anonymous): `mysql_data`, `rabbitmq_data`, `minio_data`
- **Hot-reload**:
  - Backend: Volume mount `./backend:/app`, usar `nodemon` o `ts-node-dev`
  - Frontend: Volume mount `./frontend:/app`, usar `vite` o `webpack-dev-server`
  - Parser: Volume mount `./parser-service:/app`, usar `uvicorn --reload`
- **Health checks**:
  ```yaml
  healthcheck:
    test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
    interval: 10s
    timeout: 5s
    retries: 5
  ```
- **MinIO bucket creation**: Usar `mc` (MinIO Client) en init script o usar entrypoint script
- **Depends_on with condition**: Usar `condition: service_healthy` para ordenar startup
- **Network**: Crear custom network con `driver: bridge` para DNS interno

#### Artefactos
- [ ] `docker-compose.yml`
- [ ] `docker-compose.override.yml`
- [ ] `.env.example`
- [ ] `scripts/dev-setup.sh`
- [ ] `scripts/dev-teardown.sh`
- [ ] `docker/mysql/init.sql` (schema + seeds)
- [ ] `docker/minio/create-buckets.sh`
- [ ] `README.md` (updated with dev setup)
- [ ] `docs/TROUBLESHOOTING.md`

---

## 📊 Diagrama de Dependencias entre Tickets

```mermaid
graph TD
    T01[US-001-01<br/>DB Schema]
    T02[US-001-02<br/>Upload API]
    T03[US-001-03<br/>Status API]
    T04[US-001-04<br/>RabbitMQ]
    T05[US-001-05<br/>Parser Service]
    T06[US-001-06<br/>NLP Training]
    T07[US-001-07<br/>Frontend UI]
    T08[US-001-08<br/>LinkedIn Scraper]
    T09[US-001-09<br/>E2E Tests]
    T10[US-001-10<br/>Docker Compose]

    T01 -->|Habilita| T02
    T01 -->|Habilita| T03
    T01 -->|Habilita| T05
    T02 -->|Habilita| T03
    T02 -->|Habilita| T04
    T02 -->|Habilita| T05
    T03 -->|Habilita| T07
    T04 -->|Habilita| T05
    T05 -->|Habilita| T06
    T05 -->|Habilita| T08
    T02 -->|Habilita| T07
    T07 -->|Habilita| T09
    T05 -->|Habilita| T09
    T01 -->|Habilita| T10
    T02 -->|Habilita| T10
    T05 -->|Habilita| T10
    T07 -->|Habilita| T10

    style T01 fill:#e8f5e9
    style T02 fill:#fff3e0
    style T03 fill:#fff3e0
    style T04 fill:#f3e5f5
    style T05 fill:#f3e5f5
    style T06 fill:#e1f5fe
    style T07 fill:#e1f5ff
    style T08 fill:#e1f5fe
    style T09 fill:#fce4ec
    style T10 fill:#e0f2f1
```

**Leyenda de colores:**
- 🟢 Verde: Database/Infraestructura base
- 🟠 Naranja: Backend APIs
- 🟣 Morado: Servicios de procesamiento
- 🔵 Azul: AI/ML y Features avanzadas
- 🔴 Rosa: Testing
- ⚪ Cian: DevOps

---

## 🗓️ Orden de Implementación Sugerido

### Semana 1 (Días 1-5)

**Track 1: Backend + Database**
1. **Día 1-2**: US-001-01 (Database Schema)
2. **Día 3-5**: US-001-02 (Upload API)

**Track 2: Parser Service**
1. **Día 1-5**: US-001-05 (Parser Service Core) - Iniciar en paralelo

### Semana 2 (Días 6-10)

**Track 1: Backend APIs**
1. **Día 6-7**: US-001-03 (Status & Results API)
2. **Día 8-9**: US-001-04 (RabbitMQ Setup)

**Track 2: Frontend**
1. **Día 6-9**: US-001-07 (Frontend UI)

**Track 3: DevOps**
1. **Día 8-9**: US-001-10 (Docker Compose)

### Semana 3 (Días 11-15)

**Polish & Enhancement**
1. **Día 11-13**: US-001-06 (NLP Training) - Opcional, si hay tiempo
2. **Día 11-13**: US-001-08 (LinkedIn Scraper) - Opcional, si hay tiempo
3. **Día 13-15**: US-001-09 (E2E & Performance Tests)

**Milestone**: US-001 completada y lista para integración con US-005

---

## ⚠️ Riesgos Técnicos Identificados

| # | Riesgo | Probabilidad | Impacto | Mitigación |
|---|--------|--------------|---------|------------|
| 1 | **Precisión de NLP <85%** en campos críticos | Media | Alto | - Entrenar modelo custom (US-001-06)<br/>- Fallback a revisión manual<br/>- Validaciones adicionales client-side |
| 2 | **Parsing de PDFs complejos** (multi-columna, escaneados) falla | Alta | Medio | - Implementar OCR con AWS Textract<br/>- Soportar edición manual de todos los campos<br/>- Iteración incremental de parsers |
| 3 | **Rate limits de LinkedIn** o cambios en estructura HTML | Alta | Medio | - Priorizar API oficial si disponible<br/>- Implementar rate limiting agresivo<br/>- Cache de profiles<br/>- Fallback a upload manual de CV |
| 4 | **Performance del Parser Service** <30s no se cumple para CVs largos | Media | Medio | - Optimizar parsers (lazy loading, streaming)<br/>- Timeouts configurables<br/>- Process en background con notifications<br/>- Sharding de jobs pesados |
| 5 | **Costo de AWS Textract** se dispara con muchos PDFs escaneados | Media | Bajo | - Detectar PDFs escaneados antes de enviar a OCR<br/>- Limitar OCR a X requests/día<br/>- Usar alternativa open-source (Tesseract) primero |
| 6 | **Normalización de skills** inconsistente (sinónimos no capturados) | Alta | Bajo | - Diccionario extenso con crowdsourcing<br/>- Algoritmo de fuzzy matching<br/>- Permitir edición manual siempre<br/>- Iteración continua del diccionario |
| 7 | **Dependencia de servicios externos** (S3, Textract, RabbitMQ) causa downtime | Media | Alto | - Implementar circuit breakers<br/>- Health checks robustos<br/>- Fallbacks y retries con exponential backoff<br/>- Monitoring y alerting |
| 8 | **Duplicados no detectados** correctamente (falsos negativos) | Media | Medio | - Algoritmo de detección multi-factor (email + fuzzy name)<br/>- UI clara de duplicados para revisión manual<br/>- Merge de duplicados post-facto |

---

## 📈 Métricas de Éxito

### Métricas Técnicas
- **Parsing Accuracy**: >85% en campos críticos (nombre, email, skills)
- **Processing Time**: <30 segundos para 95% de CVs (<5 páginas)
- **Uptime**: >99% de disponibilidad del servicio de upload
- **Error Rate**: <5% de parsings fallidos (excluyendo PDFs inválidos)
- **Throughput**: Capacidad de procesar >100 CVs/hora concurrentemente

### Métricas de Calidad de Código
- **Test Coverage**: >80% en backend, >75% en frontend, >80% en parser
- **Code Review**: 100% de PRs revisados antes de merge
- **Security Scans**: 0 vulnerabilidades críticas (Snyk, npm audit)
- **Performance Budgets**: Time to Interactive <3s en frontend

### Métricas de UX
- **User Satisfaction**: NPS >7 en flujo de upload (medido con beta users)
- **Error Recovery**: Tasa de éxito >90% después de primera edición manual
- **Time to Completion**: Usuario completa upload + confirmación en <2 minutos

---

## 🔄 Siguientes Pasos

1. **Refinamiento con Equipo**: Revisar tickets en planning session, ajustar estimaciones
2. **Asignación**: Asignar tickets a desarrolladores según expertise
3. **Sprint Planning**: Distribuir tickets en Sprints 1-2 según roadmap
4. **Kickoff Técnico**: Architecture review session antes de comenzar
5. **Daily Standups**: Trackear progreso y bloqueadores diariamente
6. **Mid-Sprint Review**: Día 5 - revisar progreso y ajustar si es necesario
7. **Demo & Retrospective**: Final de Sprint 1 - demo de US-001 completa

---

## 📊 Estimaciones de Esfuerzo

### Metodología y Referencias de Calibración

Este análisis aplica **tres metodologías complementarias** de estimación ágil para garantizar precisión y consenso:

#### 1. Story Points (Fibonacci)
**Escala**: 1, 2, 3, 5, 8, 13, 21

**Referencias de Calibración para este Team:**
- **1 punto** (~2-4 horas): Cambio trivial en código existente, configuración simple, sin incertidumbre
  - Ejemplo: Actualizar una variable de entorno, agregar un campo simple a una tabla
- **2 puntos** (~4-8 horas): Tarea pequeña con lógica simple, requiere testing básico
  - Ejemplo: Crear un endpoint GET básico con query simple, componente UI de solo lectura
- **3 puntos** (~1 día): Implementación estándar con lógica moderada, testing unitario y de integración
  - Ejemplo: CRUD completo de una entidad, formulario con validaciones
- **5 puntos** (~2-3 días): Complejidad media, algunos unknowns, múltiples archivos afectados, integración con servicios externos
  - Ejemplo: Implementar autenticación JWT, integración con API externa con manejo de errores
- **8 puntos** (~1 semana): Alta complejidad, muchas dependencias, incertidumbre técnica, requiere investigación
  - Ejemplo: Implementar feature completo con backend + frontend + tests, lógica de negocio compleja
- **13 puntos** (~2 semanas): Muy complejo, múltiples sistemas, alto riesgo, considerar subdividir
  - Ejemplo: Sistema de procesamiento asíncrono completo con queue, workers, monitoring

**Factores considerados:**
- Complejidad técnica (algoritmos, arquitectura)
- Incertidumbre/riesgos (tecnologías nuevas, dependencias externas)
- Esfuerzo de testing (unitario, integración, E2E)
- Deuda técnica generada (refactoring futuro necesario)

#### 2. T-Shirt Sizing
**Escala**: XS, S, M, L, XL

**Mapeo aproximado**:
- **XS** (< 4 horas): Trivial, sin riesgo
- **S** (4-8 horas): Simple, riesgo mínimo
- **M** (1-2 días): Estándar, riesgo controlado
- **L** (3-5 días): Complejo, riesgo medio
- **XL** (1-2 semanas): Muy complejo, alto riesgo, considerar subdividir

#### 3. Horas Ideales → Horas Reales
**Horas Ideales**: Tiempo de trabajo sin interrupciones, en flujo perfecto

**Factor de Realidad**: **1.7x** (basado en experiencia del team)
- Meetings diarios (~1h/día)
- Code reviews y PR feedback
- Context switching
- Debugging y troubleshooting inesperado
- Documentation
- Interrupciones y comunicación

**Fórmula**: Horas Reales = Horas Ideales × 1.7

**Asunciones del Team:**
- Día laboral: 8 horas
- Horas productivas efectivas: ~5-6 horas/día
- 1 dev-día = ~6 horas efectivas
- Team: 3 full-stack devs (1 senior, 2 mid-level)

---

### Tabla de Estimaciones Consolidada

| Ticket ID | Título | Story Points | T-Shirt | Horas Ideales | Horas Reales | Dev-Días | Confianza | Riesgos Principales |
|-----------|--------|--------------|---------|---------------|--------------|----------|-----------|---------------------|
| **US-001-01** | Database Schema | **5** | **M** | 12h | 20h | 2.5 días | **Alto** | Schema changes post-deployment, índices no optimizados |
| **US-001-02** | File Upload API | **8** | **L** | 18h | 31h | 3.9 días | **Medio** | S3 upload failures, rate limiting edge cases |
| **US-001-03** | Job Status API | **5** | **M** | 10h | 17h | 2.1 días | **Alto** | Fuzzy matching performance, WebSocket complexity |
| **US-001-04** | RabbitMQ Setup | **5** | **M** | 12h | 20h | 2.5 días | **Medio** | Message loss en crash, retry logic bugs |
| **US-001-05** | Parser Service Core | **13** | **XL** | 32h | 54h | 6.8 días | **Bajo** | NLP accuracy, PDF parsing, performance <30s |
| **US-001-06** | NLP Training | **8** | **L** | 20h | 34h | 4.3 días | **Bajo** | Dataset quality, model accuracy, training time |
| **US-001-07** | Frontend UI | **8** | **L** | 22h | 37h | 4.6 días | **Medio** | Polling logic, responsive design, accessibility |
| **US-001-08** | LinkedIn Scraper | **8** | **L** | 18h | 31h | 3.9 días | **Bajo** | API availability, rate limits, CAPTCHA, legal |
| **US-001-09** | E2E & Perf Tests | **5** | **M** | 16h | 27h | 3.4 días | **Medio** | Flaky tests, performance thresholds, CI setup |
| **US-001-10** | Docker Compose | **3** | **M** | 10h | 17h | 2.1 días | **Alto** | Service dependencies, hot-reload issues |
| **TOTAL** | **US-001 Completo** | **68** | - | **170h** | **289h** | **36 días** | - | - |

**Resumen de Capacidad:**
- **Total Story Points**: 68 pts
- **Total Horas Reales**: 289 horas (~36 dev-días)
- **Duración con 3 devs en paralelo**: ~3.2 semanas (asumiendo paralelización óptima)
- **Duración secuencial (crítica path)**: ~5 semanas
- **Velocidad necesaria**: 68 pts / 3.2 semanas = ~21 pts/semana (team de 3 devs)

---

### Análisis Detallado por Ticket

#### US-001-01: Database Schema - Candidate & Skills Tables

**📊 Estimaciones:**
- **Story Points**: **5** (Complejidad media)
- **T-Shirt Size**: **M** (1-2 días)
- **Horas Ideales**: 12h → **Horas Reales**: 20h (~2.5 dev-días)

**Justificación de Story Points (5):**
- **Complejidad Técnica** (2/5): Schema design es estándar, pero requiere decisiones de normalización y tipos de datos JSON
- **Incertidumbre** (1/5): Baja incertidumbre, patrón bien conocido
- **Testing** (1/5): Tests de constraints, migrations, índices
- **Deuda Técnica** (1/5): Schema changes futuras son costosas, debe estar bien diseñado

**Factores de Complejidad:**
- Diseño de relaciones many-to-many (candidate_skills)
- Decisión de tipos JSON vs columnas estructuradas
- Diseño de índices compuestos para queries eficientes
- Normalización de skills con synonyms
- Soft deletes y auditoría
- Seed data de 200+ skills

**Riesgos de Estimación:**
- ⚠️ **Cambios en schema post-deployment**: Difíciles de migrar, requiere planificación cuidadosa (mitigación: review exhaustivo antes de merge)
- ⚠️ **Performance de índices**: Índices mal diseñados pueden no detectarse hasta carga real (mitigación: EXPLAIN queries en tests)
- ⚠️ **JSON schema validation**: MySQL 8.0 tiene soporte limitado vs PostgreSQL (mitigación: validar en application layer también)

**Supuestos:**
- Equipo tiene experiencia con MySQL y ORMs (TypeORM/Knex)
- No hay requirements de particionamiento en MVP
- Seed data de skills puede ser semi-automatizada (scraping de job boards)

**Nivel de Confianza**: **Alto (90%)** - Task bien definida, stack conocido, pocas incógnitas

---

#### US-001-02: Backend API - File Upload Endpoints

**📊 Estimaciones:**
- **Story Points**: **8** (Alta complejidad)
- **T-Shirt Size**: **L** (3-5 días)
- **Horas Ideales**: 18h → **Horas Reales**: 31h (~3.9 dev-días)

**Justificación de Story Points (8):**
- **Complejidad Técnica** (3/5): Upload multipart, streaming a S3, hash calculation, deduplicación
- **Incertidumbre** (2/5): Integración con S3, manejo de errores edge cases, rate limiting
- **Testing** (2/5): Tests unitarios, integración con S3 mock, tests de security
- **Deuda Técnica** (1/5): Código de infra que debe ser robusto desde el inicio

**Factores de Complejidad:**
- Configuración de multer con validaciones de seguridad
- Streaming de archivos grandes a S3 sin cargar en memoria
- Cálculo de hash SHA-256 eficiente
- Deduplicación por hash con query a cache
- Generación de presigned URLs con expiración
- Integración con RabbitMQ producer
- Validación de MIME types reales (no solo extensión)
- Rate limiting por usuario
- Manejo de errores y retries con S3

**Riesgos de Estimación:**
- ⚠️ **S3 upload failures**: Network issues, timeouts, permissions (mitigación: retry con exponential backoff)
- ⚠️ **Security vulnerabilities**: MIME type spoofing, path traversal (mitigación: security audit, penetration testing)
- ⚠️ **Rate limiting edge cases**: Distributed systems, Redis para contadores (mitigación: usar biblioteca probada como `express-rate-limit`)
- ⚠️ **File size validation**: Evasión de límites con chunked uploads (mitigación: validar tanto client-side como server-side)

**Supuestos:**
- AWS SDK v3 setup ya configurado (credentials, buckets creados)
- RabbitMQ producer service ya existe o se desarrolla en paralelo (US-001-04)
- MinIO funciona como drop-in replacement de S3 para local dev
- LinkedIn URL validation es simple regex (no scraping aún)

**Nivel de Confianza**: **Medio (70%)** - Múltiples integraciones externas, edge cases de seguridad

---

#### US-001-03: Backend API - Job Status & Results Endpoints

**📊 Estimaciones:**
- **Story Points**: **5** (Complejidad media)
- **T-Shirt Size**: **M** (1-2 días)
- **Horas Ideales**: 10h → **Horas Reales**: 17h (~2.1 dev-días)

**Justificación de Story Points (5):**
- **Complejidad Técnica** (2/5): CRUD estándar con lógica de fuzzy matching
- **Incertidumbre** (1/5): Algoritmo de duplicados puede requerir tuning
- **Testing** (1/5): Tests unitarios para fuzzy logic, integración para endpoints
- **Deuda Técnica** (1/5): Código relativamente simple, poco impacto futuro

**Factores de Complejidad:**
- Endpoints CRUD estándar para jobs y parsed data
- Implementación de fuzzy matching (Levenshtein distance)
- Detección de duplicados con múltiples criterios
- Ediciones manuales con tracking de cambios
- Confirmación con evento para downstream services
- Caching con Redis
- WebSocket opcional para real-time updates

**Riesgos de Estimación:**
- ⚠️ **Fuzzy matching performance**: Algoritmo Levenshtein O(n²) puede ser lento con 10k+ candidatos (mitigación: pre-filtrar por primera letra, usar índices)
- ⚠️ **WebSocket complexity**: Si se implementa, agrega complejidad significativa (mitigación: marcar como opcional para MVP)
- ⚠️ **Race conditions**: Ediciones concurrentes pueden causar data loss (mitigación: optimistic locking con `updated_at`)

**Supuestos:**
- WebSocket es opcional y puede omitirse en MVP (polling es suficiente)
- Algoritmo de fuzzy matching con threshold fijo (no ML)
- Duplicate detection es best-effort (puede tener falsos negativos/positivos)

**Nivel de Confianza**: **Alto (85%)** - Funcionalidad estándar, complejidad controlada

---

#### US-001-04: Message Queue - RabbitMQ Setup & Consumers

**📊 Estimaciones:**
- **Story Points**: **5** (Complejidad media)
- **T-Shirt Size**: **M** (1-2 días)
- **Horas Ideales**: 12h → **Horas Reales**: 20h (~2.5 dev-días)

**Justificación de Story Points (5):**
- **Complejidad Técnica** (2/5): Setup de RabbitMQ, exchanges, queues, DLQ
- **Incertidumbre** (2/5): Retry logic con exponential backoff puede tener edge cases
- **Testing** (1/5): Tests de integración con RabbitMQ, simulación de failures
- **Deuda Técnica** (0/5): Infraestructura crítica, debe ser robusta

**Factores de Complejidad:**
- Configuración de RabbitMQ en Docker Compose
- Diseño de exchanges y bindings
- Dead letter queues para mensajes fallidos
- Producer con confirmations
- Consumer con manual ACK
- Retry logic con headers y TTL
- Exponential backoff (1min, 5min, 15min)
- Graceful shutdown con mensajes en flight
- Monitoring de queue depth

**Riesgos de Estimación:**
- ⚠️ **Message loss en crash**: Si consumer crashea antes de ACK, mensaje puede perderse (mitigación: durable queues, persistent messages, manual ACK)
- ⚠️ **Retry logic bugs**: Mensajes pueden quedar en loop infinito o ir a DLQ prematuramente (mitigación: tests exhaustivos con diferentes escenarios de fallo)
- ⚠️ **Connection recovery**: Reconexión automática puede fallar en escenarios complejos (mitigación: usar biblioteca con auto-recovery probada)

**Supuestos:**
- RabbitMQ es el message broker elegido (no Kafka, SQS, etc.)
- Consumer simple (solo delega a Parser Service HTTP call)
- No se requiere partitioning o sharding en MVP
- Monitoring básico es suficiente (no Prometheus/Grafana aún)

**Nivel de Confianza**: **Medio (75%)** - Tecnología madura pero retry logic puede ser tricky

---

#### US-001-05: Parser Service - Python FastAPI Core

**📊 Estimaciones:**
- **Story Points**: **13** (Muy alta complejidad)
- **T-Shirt Size**: **XL** (1-2 semanas)
- **Horas Ideales**: 32h → **Horas Reales**: 54h (~6.8 dev-días)

**Justificación de Story Points (13):**
- **Complejidad Técnica** (5/5): Múltiples parsers (PDF, DOCX, TXT), NLP con spaCy, normalización, caching
- **Incertidumbre** (4/5): Precisión de NLP, performance <30s no garantizada, PDFs complejos
- **Testing** (2/5): Tests unitarios por parser, integración, performance testing
- **Deuda Técnica** (2/5): Código complejo que requerirá refactoring iterativo

**Factores de Complejidad:**
- Desarrollo de servicio FastAPI completo desde cero
- Implementación de 3 parsers diferentes (PDF, DOCX, TXT)
- Extracción de texto con PyPDF2, pdfplumber, python-docx
- Encoding detection con chardet
- NLP extraction con spaCy NER (nombre, email, teléfono, ubicación, skills, educación, experiencia)
- Custom entity recognizer para skills
- Normalización de skills con diccionario externo (HTTP call a backend)
- Cálculo de confidence scores por campo
- OCR fallback con AWS Textract para PDFs escaneados
- Caching con Redis por file_hash
- Manejo de errores y logging
- Performance optimization para <10s en CV simple

**Riesgos de Estimación:**
- 🔴 **NLP accuracy <85%**: spaCy base models pueden no ser precisos para CVs técnicos (mitigación: US-001-06 custom training, fallback a keyword matching)
- 🔴 **PDF parsing failures**: Multi-columna, tablas, formatos complejos (mitigación: pdfplumber para tablas, OCR fallback, permitir edición manual)
- 🔴 **Performance <30s no cumplido**: CVs largos (>10 páginas), spaCy lento (mitigación: lazy loading de modelos, streaming, timeouts configurables, procesamiento incremental)
- ⚠️ **OCR cost**: AWS Textract $1.50/1000 pages puede ser caro (mitigación: detectar PDFs escaneados antes de OCR, usar Tesseract primero)
- ⚠️ **Skills extraction inconsistent**: Skills con múltiples nombres (React, ReactJS, React.js) (mitigación: diccionario extenso, fuzzy matching)

**Supuestos:**
- Developer tiene experiencia con Python y NLP (si no, +30% tiempo)
- spaCy models (`es_core_news_lg`, `en_core_web_lg`) son suficientemente buenos para MVP
- Custom NER training (US-001-06) es opcional para MVP
- OCR fallback es edge case (<10% de CVs)
- Normalización de skills puede tener errores (best-effort en MVP)

**Nivel de Confianza**: **Bajo (60%)** - Ticket más complejo, múltiples unknowns, dependencia de calidad de NLP

**Recomendación**: Considerar **spike de 1 día** antes de comenzar implementación para:
- Validar precisión de spaCy con 10 CVs reales
- Probar parsing de PDFs complejos
- Medir performance baseline

---

#### US-001-06: NLP Training - Custom spaCy Entity Recognizer

**📊 Estimaciones:**
- **Story Points**: **8** (Alta complejidad)
- **T-Shirt Size**: **L** (3-5 días)
- **Horas Ideales**: 20h → **Horas Reales**: 34h (~4.3 dev-días)

**Justificación de Story Points (8):**
- **Complejidad Técnica** (3/5): Training de modelo NER, evaluación, versionado
- **Incertidumbre** (3/5): Calidad de dataset, accuracy del modelo, iteraciones necesarias
- **Testing** (1/5): Evaluation en test set, comparación con baseline
- **Deuda Técnica** (1/5): Training pipeline debe ser reproducible y versionado

**Factores de Complejidad:**
- Recolección de dataset: 100+ CVs anonimizados
- Anotación manual con herramienta (Label Studio, Prodigy, Doccano)
- Etiquetado de entidades: SKILL, JOB_TITLE, COMPANY, DEGREE, CERTIFICATION
- Exportación en formato spaCy training (JSON)
- Implementación de training script con spaCy API
- Fine-tuning de modelos pre-entrenados
- Evaluación con precision, recall, F1-score
- Iteración hasta alcanzar F1 >0.80 para SKILL
- Versionado con MLflow o DVC
- Integración en Parser Service

**Riesgos de Estimación:**
- 🔴 **Dataset quality**: 100 CVs pueden no ser suficientes, anotaciones inconsistentes (mitigación: inter-annotator agreement, más datos si es necesario)
- 🔴 **Model accuracy**: F1 >0.80 puede no lograrse con 100 CVs (mitigación: data augmentation, más iteraciones, considerar BERT)
- ⚠️ **Training time**: Múltiples iteraciones pueden consumir más tiempo (mitigación: usar GPU si disponible, empezar con subset pequeño)
- ⚠️ **Annotation effort**: Etiquetar 100 CVs puede tomar 10+ horas (mitigación: dividir entre equipo, usar pre-annotations)

**Supuestos:**
- Dataset de 100 CVs es suficiente para MVP (puede necesitar más iteraciones)
- Herramientas de anotación son open-source (Label Studio) o ya hay licencia (Prodigy)
- Fine-tuning es suficiente (no se requiere training from scratch)
- Target F1 >0.80 es realista con dataset small
- Developer tiene experiencia con ML training (si no, +50% tiempo)

**Nivel de Confianza**: **Bajo (65%)** - Alta dependencia de calidad de datos, iteraciones impredecibles

**Recomendación**: Marcar como **opcional para MVP**. Primero validar accuracy de modelo base (US-001-05), solo entrenar custom si accuracy <70%

---

#### US-001-07: Frontend UI - CV Upload Component

**📊 Estimaciones:**
- **Story Points**: **8** (Alta complejidad)
- **T-Shirt Size**: **L** (3-5 días)
- **Horas Ideales**: 22h → **Horas Reales**: 37h (~4.6 dev-días)

**Justificación de Story Points (8):**
- **Complejidad Técnica** (3/5): Drag-and-drop, polling, edición de campos, detección de duplicados
- **Incertidumbre** (2/5): Polling strategy, responsive design, accesibilidad
- **Testing** (2/5): Tests unitarios, integración con MSW, tests de accesibilidad
- **Deuda Técnica** (1/5): Código de UI que puede requerir refactoring para UX

**Factores de Complejidad:**
- Componente `CVUploadZone` con drag-and-drop (react-dropzone)
- Validación client-side (tipo, tamaño)
- Upload con progress bar (axios onUploadProgress)
- Polling de job status cada 2 segundos
- Componente `ParsedDataPreview` con múltiples sections
- Highlight de campos con baja confianza (<70%)
- Componente `EditableField` para edición manual
- Modal de duplicados con opciones
- Manejo de errores y loading states
- Responsive design (desktop + tablet)
- Accesibilidad (keyboard navigation, aria-labels)
- Storybook stories para cada componente

**Riesgos de Estimación:**
- ⚠️ **Polling logic bugs**: Memory leaks, polling no se detiene, race conditions (mitigación: usar react-query con refetchInterval, cleanup en useEffect)
- ⚠️ **Responsive design complexity**: Multi-device testing puede revelar issues (mitigación: usar mobile-first approach, testing en BrowserStack)
- ⚠️ **Accessibility violations**: axe-core puede encontrar violations que requieren refactoring (mitigación: tests de accesibilidad desde el inicio)
- ⚠️ **Progress bar UX**: Progress reporting inexacto puede confundir usuarios (mitigación: usar estimaciones conservadoras, messaging claro)

**Supuestos:**
- React 19.2 no tiene breaking changes vs 18 (o equipo ya conoce diferencias)
- UI library (Material-UI o Chakra UI) ya está configurada en proyecto
- Backend APIs (US-001-02, US-001-03) ya están funcionando o se mockean con MSW
- Storybook ya está configurado en proyecto

**Nivel de Confianza**: **Medio (75%)** - Funcionalidad estándar pero múltiples sub-componentes

---

#### US-001-08: Integration - LinkedIn Profile Scraper

**📊 Estimaciones:**
- **Story Points**: **8** (Alta complejidad)
- **T-Shirt Size**: **L** (3-5 días)
- **Horas Ideales**: 18h → **Horas Reales**: 31h (~3.9 dev-días)

**Justificación de Story Points (8):**
- **Complejidad Técnica** (3/5): Scraping con playwright, parsing HTML, rate limiting
- **Incertidumbre** (4/5): API availability, anti-bot, estructura HTML cambia, legal compliance
- **Testing** (1/5): Tests con HTML fixtures, integration tests con profiles reales
- **Deuda Técnica** (0/5): Código frágil que requerirá mantenimiento continuo

**Factores de Complejidad:**
- Investigación de LinkedIn API oficial (puede no estar disponible)
- Implementación de scraper con playwright/selenium
- Extracción de datos: nombre, headline, ubicación, empresa, educación, skills, experiencia
- Parsing HTML con BeautifulSoup/lxml
- Rate limiting: max 10 profiles/minuto
- Manejo de CAPTCHA (detección, fallback a manual review)
- Detección de perfiles privados/no accesibles
- Normalización de datos al formato estándar
- Caching de profiles por URL
- Error handling robusto

**Riesgos de Estimación:**
- 🔴 **LinkedIn API no disponible**: Requiere partnership, puede no ser viable (mitigación: usar scraping, pero tiene riesgos legales)
- 🔴 **Anti-bot detection**: LinkedIn puede bloquear scraping (mitigación: rate limiting agresivo, User-Agent realista, considerar proxies)
- 🔴 **HTML structure changes**: LinkedIn cambia frecuentemente (mitigación: usar selectors robustos, monitoring de cambios, fallback a manual)
- 🔴 **Legal compliance**: Scraping puede violar ToS de LinkedIn (mitigación: legal review, solo datos públicos, rate limiting)
- ⚠️ **CAPTCHA blocking**: Puede requerir intervención manual frecuente (mitigación: marcar como "manual_review_required", considerar servicio de CAPTCHA solving)

**Supuestos:**
- LinkedIn API oficial NO está disponible (scraping es necesario)
- Scraping de datos públicos es legal en jurisdicción del proyecto (requiere legal review)
- Rate limiting de 10 profiles/minuto es suficientemente conservador
- CAPTCHA es edge case (<10% de requests)
- HTML structure permanece estable por al menos 3-6 meses

**Nivel de Confianza**: **Bajo (55%)** - Alto riesgo técnico y legal, dependencia de servicio externo no controlado

**Recomendación**: Marcar como **OPCIONAL para MVP**. Considerar primero solo upload de PDF/DOCX. Si es crítico, hacer **spike de 1 día** para validar viabilidad técnica y legal.

---

#### US-001-09: Testing - E2E & Performance Tests

**📊 Estimaciones:**
- **Story Points**: **5** (Complejidad media)
- **T-Shirt Size**: **M** (1-2 días)
- **Horas Ideales**: 16h → **Horas Reales**: 27h (~3.4 dev-días)

**Justificación de Story Points (5):**
- **Complejidad Técnica** (2/5): Setup de Playwright/Cypress, implementación de tests E2E y performance
- **Incertidumbre** (2/5): Flaky tests, configuración de CI con service containers
- **Testing** (0/5): Este ticket ES testing
- **Deuda Técnica** (1/5): Tests requieren mantenimiento continuo

**Factores de Complejidad:**
- Setup de Playwright o Cypress para E2E
- Implementación de 4+ test scenarios (happy path + errors)
- Performance tests con k6 o Artillery
- Load test: 50 usuarios concurrentes
- Throughput test: 100 CVs
- Smoke tests para deployment
- Configuración de CI/CD (GitHub Actions)
- Service containers para MySQL, RabbitMQ, Redis en CI
- Test data fixtures (10+ CVs)
- Screenshots/videos en fallos
- Reportes HTML

**Riesgos de Estimación:**
- ⚠️ **Flaky tests**: Tests E2E pueden fallar intermitentemente (mitigación: waiters apropiados, retries, isolation de tests)
- ⚠️ **CI setup complexity**: Service containers en GitHub Actions pueden tener issues (mitigación: usar Docker Compose en CI, debugging incremental)
- ⚠️ **Performance thresholds**: SLA de <30s puede no cumplirse inicialmente (mitigación: marcar como warning, no blocker, iterar)
- ⚠️ **Test data management**: CVs reales pueden contener PII (mitigación: usar CVs sintéticos o anonimizados)

**Supuestos:**
- Frontend y backend ya están completamente implementados
- Tests E2E se ejecutan en ambiente integrado (no producción)
- Performance tests usan datos sintéticos (no producción)
- CI/CD pipeline (GitHub Actions) es la plataforma elegida

**Nivel de Confianza**: **Medio (75%)** - Complejidad estándar, pero flaky tests y CI setup pueden consumir tiempo extra

---

#### US-001-10: DevOps - Docker Compose & Local Development Setup

**📊 Estimaciones:**
- **Story Points**: **3** (Complejidad baja-media)
- **T-Shirt Size**: **M** (1-2 días)
- **Horas Ideales**: 10h → **Horas Reales**: 17h (~2.1 dev-días)

**Justificación de Story Points (3):**
- **Complejidad Técnica** (1/5): Configuración de Docker Compose con servicios existentes
- **Incertidumbre** (1/5): Hot-reload puede tener issues, depends_on timing
- **Testing** (0/5): Testing es manual (verificar que servicios levantan)
- **Deuda Técnica** (1/5): Docker Compose requiere mantenimiento cuando se agregan servicios

**Factores de Complejidad:**
- Creación de `docker-compose.yml` con 8 servicios
- Configuración de volumes para persistencia
- Health checks para cada servicio
- Depends_on con condition: service_healthy
- Hot-reload para backend, frontend, parser service
- Network configuration
- `.env.example` con variables
- Scripts de inicialización (migrations, seeds, buckets)
- Script de teardown
- README con instrucciones

**Riesgos de Estimación:**
- ⚠️ **Service dependencies timing**: depends_on puede no garantizar que servicio esté ready (mitigación: usar health checks, wait-for scripts)
- ⚠️ **Hot-reload issues**: Volumes en Windows pueden tener performance issues (mitigación: documentar workarounds, considerar WSL2)
- ⚠️ **Port conflicts**: Puertos pueden estar ocupados en máquinas de developers (mitigación: usar puertos no estándar, documentar)

**Supuestos:**
- Todos los servicios ya tienen Dockerfiles (frontend, backend, parser)
- Equipo tiene experiencia con Docker y Docker Compose
- Desarrollo en Linux/Mac (Windows puede tener issues adicionales)
- No se requiere orchestration complejo (no Kubernetes en local)

**Nivel de Confianza**: **Alto (90%)** - Task bien definida, riesgos controlados

---

### Resumen de Capacidad y Planificación

#### Distribución de Story Points por Tipo

| Tipo | Tickets | Story Points | % Total |
|------|---------|--------------|---------|
| **Backend** | US-001-02, US-001-03, US-001-04 | 18 pts | 26% |
| **AI/ML** | US-001-05, US-001-06 | 21 pts | 31% |
| **Frontend** | US-001-07 | 8 pts | 12% |
| **Integration** | US-001-08 | 8 pts | 12% |
| **Testing** | US-001-09 | 5 pts | 7% |
| **DevOps** | US-001-10 | 3 pts | 4% |
| **Database** | US-001-01 | 5 pts | 7% |
| **TOTAL** | 10 tickets | **68 pts** | 100% |

#### Distribución de Confianza

| Nivel de Confianza | Tickets | Story Points | Riesgo |
|-------------------|---------|--------------|--------|
| **Alto (>85%)** | US-001-01, US-001-03, US-001-10 | 13 pts (19%) | ✅ Bajo riesgo |
| **Medio (70-85%)** | US-001-02, US-001-04, US-001-07, US-001-09 | 26 pts (38%) | ⚠️ Riesgo controlado |
| **Bajo (<70%)** | US-001-05, US-001-06, US-001-08 | 29 pts (43%) | 🔴 Alto riesgo |

**Análisis**: 43% de los story points están en tickets de alta incertidumbre, principalmente AI/ML y scraping.

#### Critical Path Analysis

**Ruta Crítica** (no puede paralelizarse):
1. US-001-01 (DB Schema) → 5 pts
2. US-001-02 (Upload API) → 8 pts
3. US-001-04 (RabbitMQ) → 5 pts
4. US-001-05 (Parser Service) → 13 pts
5. US-001-03 (Status API) → 5 pts
6. US-001-07 (Frontend UI) → 8 pts
7. US-001-09 (E2E Tests) → 5 pts

**Total Critical Path**: 49 pts (~3 semanas con 1 dev dedicado)

**Tickets Paralelizables**:
- US-001-06 (NLP Training): Puede desarrollarse en paralelo con US-001-07 (Frontend)
- US-001-08 (LinkedIn Scraper): Puede desarrollarse en paralelo con US-001-07
- US-001-10 (Docker Compose): Puede configurarse incrementalmente durante el desarrollo

#### Recomendación de Sprint Planning

**Asumiendo:**
- Team de 3 devs full-stack (1 senior, 2 mid-level)
- Velocidad esperada: 40 pts / 2 semanas (sprint)
- Sprints de 2 semanas

**Sprint 1 (Semanas 1-2): Foundation**
- **Track 1 (Backend Dev 1)**: US-001-01 (5 pts) + US-001-02 (8 pts) = 13 pts
- **Track 2 (Backend Dev 2)**: US-001-05 (Parser Service) (13 pts) - En progreso
- **Track 3 (FullStack Dev 3)**: US-001-10 (3 pts) + US-001-04 (5 pts) = 8 pts
- **Total Sprint 1**: ~34 pts (capacidad: 40 pts) ✅

**Sprint 2 (Semanas 3-4): Integration & UX**
- **Track 1 (Backend Dev 1)**: US-001-03 (5 pts) + US-001-08 (LinkedIn Scraper) (8 pts) = 13 pts
- **Track 2 (Frontend Dev 2)**: US-001-07 (Frontend UI) (8 pts) + US-001-09 inicio = 10 pts
- **Track 3 (ML Dev 3)**: US-001-06 (NLP Training) (8 pts) + US-001-09 soporte = 10 pts
- **Total Sprint 2**: ~31 pts (capacidad: 40 pts) ✅

**Sprint 3 (Semana 5): Testing & Polish** (si es necesario)
- US-001-09 (E2E Tests completar) (5 pts)
- Bug fixes y polish (~8-10 pts)
- **Total Sprint 3**: ~15 pts

**Duración Total Estimada**: **2-2.5 sprints (4-5 semanas)**

#### Cuellos de Botella Identificados

1. **US-001-05 (Parser Service)**: 13 pts, en critical path, alto riesgo
   - **Mitigación**: Asignar a senior dev, considerar spike de 1 día, pair programming

2. **Dependencia de NLP accuracy**: US-001-05 y US-001-06 deben alcanzar >85% precisión
   - **Mitigación**: Empezar con modelo base, US-001-06 es mejora iterativa (no blocker)

3. **LinkedIn Scraper (US-001-08)**: Alto riesgo legal/técnico
   - **Mitigación**: Marcar como opcional para MVP, priorizar PDF/DOCX upload primero

4. **Testing (US-001-09)**: Requiere todos los servicios funcionando
   - **Mitigación**: Empezar tests unitarios e integración desde Sprint 1, E2E al final

---

### Análisis de Riesgos de Estimación

| Ticket | Riesgo | Probabilidad | Impacto | Estrategia de Mitigación |
|--------|--------|--------------|---------|--------------------------|
| **US-001-05** | NLP accuracy <85% | Alta (60%) | Alto (+8 pts) | Spike 1 día, custom training (US-001-06), fallback a keyword matching |
| **US-001-05** | Performance >30s | Media (40%) | Alto (+5 pts) | Profiling, lazy loading, timeouts configurables, procesamiento incremental |
| **US-001-06** | Dataset insuficiente | Media (50%) | Medio (+3 pts) | Recolectar más CVs, data augmentation, considerar BERT |
| **US-001-08** | LinkedIn blocking | Alta (70%) | Alto (+5 pts o eliminar) | Rate limiting agresivo, proxies, considerar eliminar de MVP |
| **US-001-02** | S3 upload failures | Media (30%) | Medio (+2 pts) | Retry con exponential backoff, circuit breaker, monitoring |
| **US-001-04** | Message loss | Baja (20%) | Alto (+3 pts) | Durable queues, manual ACK, tests exhaustivos |
| **US-001-09** | Flaky tests | Alta (60%) | Bajo (+2 pts) | Waiters apropiados, retries, isolation, debugging time |
| **US-001-07** | Responsive design issues | Media (40%) | Bajo (+2 pts) | Mobile-first, testing en dispositivos reales |

**Riesgo Total Agregado**: ~30 pts adicionales en worst case (44% overhead)

**Estimación Conservadora**: 68 pts + 30 pts buffer = **~98 pts** (~2.5 sprints con team de 3 devs)

---

### Planning Poker Simulation

Simulación de Planning Poker session con perspectivas de diferentes roles:

#### US-001-05 (Parser Service Core)

**Senior Backend Dev (Python expert)**:
- Estimación: **8 pts**
- Justificación: "He hecho parsers antes, spaCy es estándar, pero la precisión >85% puede requerir iteración. 8 pts es realista si uso bibliotecas probadas."

**Mid-Level FullStack Dev**:
- Estimación: **13 pts**
- Justificación: "No tengo experiencia con spaCy, múltiples parsers (PDF, DOCX), NLP, normalización, OCR fallback. Parece muy complejo, prefiero 13 pts."

**ML Engineer**:
- Estimación: **13 pts**
- Justificación: "La precisión de NLP es impredecible sin dataset de validación. spaCy base models pueden no ser suficientes para CVs técnicos. 13 pts incluye iteraciones."

**Consenso Final**: **13 pts** (mayoría vote, considerando incertidumbre de ML)

---

#### US-001-08 (LinkedIn Scraper)

**Senior Backend Dev**:
- Estimación: **13 pts**
- Justificación: "LinkedIn tiene anti-bot muy agresivo, estructura HTML compleja, riesgo legal. Puede consumir mucho tiempo en debugging y workarounds."

**Mid-Level Dev**:
- Estimación: **8 pts**
- Justificación: "Si usamos playwright es relativamente simple, pero el anti-bot es un unknown. 8 pts si funciona, 13 si hay muchos blockers."

**Product Owner**:
- Opinión: "¿Es crítico para MVP? Podemos empezar solo con PDF/DOCX upload."

**Consenso Final**: **8 pts** (asumiendo implementación) pero **marcar como OPCIONAL para MVP**

---

#### US-001-07 (Frontend UI)

**Senior Frontend Dev (React expert)**:
- Estimación: **5 pts**
- Justificación: "Drag-and-drop con react-dropzone es estándar, polling con react-query es trivial, edición de campos es CRUD. 5 pts es suficiente."

**Mid-Level FullStack Dev**:
- Estimación: **8 pts**
- Justificación: "Hay muchos sub-componentes: upload, preview, edición, duplicados. Responsive design y accesibilidad agregan complejidad. Prefiero 8 pts."

**UX Designer**:
- Opinión: "Necesitamos iteración en UX, el flujo de edición debe ser muy intuitivo. Consideren tiempo para ajustes."

**Consenso Final**: **8 pts** (incluye iteración de UX y tests de accesibilidad)

---

### Recomendaciones Finales

#### 1. Re-estimación Triggers

Re-estimar tickets si:
- **US-001-05**: Después del spike de 1 día, si accuracy de spaCy base <70%
- **US-001-06**: Después de anotar primeros 20 CVs, si velocidad de anotación >30min/CV
- **US-001-08**: Después de investigación de API oficial (día 1), si no está disponible
- **US-001-09**: Después de setup de CI, si hay issues con service containers

#### 2. Estrategia de Mitigación de Riesgos

**Alto Riesgo (US-001-05, US-001-06, US-001-08)**:
- ✅ Asignar a developers senior o con experiencia relevante
- ✅ Hacer spikes de 1 día antes de commitment
- ✅ Pair programming en partes críticas
- ✅ Reviews técnicos frecuentes (cada 2 días)
- ✅ Tener Plan B definido (fallbacks, simplificaciones)

**Medio Riesgo (US-001-02, US-001-04, US-001-07)**:
- ✅ Daily sync para identificar blockers temprano
- ✅ Tests automatizados desde el inicio
- ✅ Monitoring y logging robusto

**Bajo Riesgo (US-001-01, US-001-03, US-001-10)**:
- ✅ Seguir process estándar de desarrollo
- ✅ Code reviews normales

#### 3. Optimizaciones de Scope para MVP

**Si hay presión de tiempo, considerar:**
- ❌ **Eliminar US-001-08** (LinkedIn Scraper): Riesgo alto, valor medio → Backlog futuro
- ❌ **Eliminar US-001-06** (NLP Training): Opcional, usar spaCy base primero → Mejora post-MVP
- ⚠️ **Simplificar US-001-05**: Omitir OCR fallback (AWS Textract) en MVP → Agregar después
- ⚠️ **Simplificar US-001-07**: WebSocket real-time updates → Usar solo polling

**Con optimizaciones**: 68 pts - 8 pts (US-001-08) - 8 pts (US-001-06) = **52 pts** (~1.5 sprints)

#### 4. Métricas de Éxito de Estimaciones

Trackear durante desarrollo:
- **Accuracy de estimaciones**: Story points reales vs estimados (target: ±20%)
- **Burn-down rate**: Completar ~20 pts/semana (team de 3 devs)
- **Tickets con re-work**: Target <10% de tickets requieren re-apertura
- **Velocity del team**: Medir en primeros 2 sprints para calibrar estimaciones futuras

---

## 📚 Referencias y Recursos

### Documentación Técnica
- [spaCy NER Documentation](https://spacy.io/usage/linguistic-features#named-entities)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [React 19 Documentation](https://react.dev/)
- [RabbitMQ Tutorials](https://www.rabbitmq.com/getstarted.html)
- [AWS Textract API Reference](https://docs.aws.amazon.com/textract/)

### Bibliotecas Clave
- **Python**: spaCy, FastAPI, PyPDF2, pdfplumber, python-docx, boto3, chardet
- **Node.js**: Express, TypeORM/Knex, multer, amqplib, @aws-sdk/client-s3
- **React**: react-dropzone, axios, react-query, react-hook-form

### Tools & Platforms
- **Development**: Docker, Docker Compose, VS Code, Postman
- **Testing**: Playwright, Jest, pytest, k6
- **CI/CD**: GitHub Actions (TBD)
- **Monitoring**: (TBD - considerar Datadog, New Relic, o Prometheus)

---

**Documento generado:** 2025-01-09
**Versión:** 1.0
**Próxima Revisión:** Después de Sprint 1 retrospective
