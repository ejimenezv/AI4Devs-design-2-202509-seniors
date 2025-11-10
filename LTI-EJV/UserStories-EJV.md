# User Stories - LTI (Leading Talent Intelligence)

**Proyecto:** Sistema ATS con Inteligencia Artificial
**Product Owner:** Equipo LTI
**Versión:** 1.0
**Fecha:** 2025-01-09

---

# Product Backlog - LTI ATS

## Metodología de Priorización

Este backlog ha sido priorizado utilizando **tres metodologías complementarias** para garantizar una decisión robusta y balanceada:

1. **MoSCoW**: Clasifica las funcionalidades según su criticidad para el MVP y el negocio
2. **Value vs Effort Matrix**: Identifica Quick Wins y Big Bets basándose en ROI esperado
3. **WSJF (Weighted Shortest Job First)**: Calcula un score cuantitativo considerando valor de negocio, urgencia temporal y reducción de riesgo

**Proceso de Análisis:**
- Cada User Story fue evaluada por valor de negocio (impacto en usuarios, ROI, diferenciación competitiva)
- El esfuerzo técnico se estimó considerando complejidad, dependencias técnicas y riesgos de implementación
- Las dependencias entre User Stories fueron mapeadas para asegurar un flujo de desarrollo lógico
- Se consideró la viabilidad técnica y disponibilidad de recursos (APIs externas, infraestructura, datos)

**Criterios de Valor:**
- **Impacto en usuarios**: ¿Cuántos usuarios se benefician y con qué frecuencia?
- **Diferenciación competitiva**: ¿Es un feature único que nos distingue de ATS tradicionales?
- **ROI esperado**: ¿Genera valor inmediato medible (time saved, quality improvement)?
- **Habilitador de otras features**: ¿Desbloquea desarrollo de otras funcionalidades?

---

## Análisis de Priorización

### Enfoque 1: MoSCoW

| User Story | ID | Clasificación | Justificación |
|------------|-----|---------------|---------------|
| Parseo Inteligente de CV | US-001 | **MUST HAVE** | Foundational - Sin datos estructurados de candidatos no hay sistema. Habilita US-002, US-005, US-006 |
| Screening Automatizado | US-005 | **MUST HAVE** | Core value proposition - El scoring predictivo es el diferenciador principal del producto vs ATS tradicionales |
| Pipeline Visual Kanban | US-011 | **MUST HAVE** | Core UX - Visibilidad del pipeline es la interacción principal del Recruiter. Sin esto el sistema no es usable |
| Publicación Multi-Canal | US-003 | **MUST HAVE** | Acquisition crítica - Sin candidatos aplicando no hay pipeline que gestionar. Genera tráfico inicial |
| Evaluación con Scorecards | US-008 | **MUST HAVE** | Evaluación estructurada - Fundamental para decisiones de contratación basadas en datos (parte del core workflow) |
| Búsqueda Semántica | US-002 | **SHOULD HAVE** | Importante para reutilización de talento, pero el MVP puede funcionar con búsqueda básica inicialmente |
| Scheduling Automático | US-007 | **SHOULD HAVE** | Gran time-saver (80%) pero las entrevistas pueden coordinarse manualmente en MVP si es necesario |
| Feedback Consolidado | US-010 | **SHOULD HAVE** | Mejora significativa en quality de decisiones, pero los scorecards individuales pueden revisarse manualmente |
| Aprobación de Jobs | US-004 | **SHOULD HAVE** | Governance importante, pero no bloqueante - puede manejarse externamente en MVP |
| Auto-Tagging | US-006 | **COULD HAVE** | Efficiency feature - Muy útil pero los recruiters pueden filtrar/buscar de otras formas |
| Colaboración Real-Time | US-009 | **COULD HAVE** | Nice to have - La colaboración puede hacerse vía email/Slack inicialmente |
| Dashboard Analytics | US-012 | **COULD HAVE** | Insights valiosos pero no críticos para MVP - puede agregarse cuando haya datos históricos suficientes |

**Resultado MoSCoW:**
- **Must Have (5)**: US-001, US-005, US-011, US-003, US-008
- **Should Have (4)**: US-002, US-007, US-010, US-004
- **Could Have (3)**: US-006, US-009, US-012
- **Won't Have (0)**: Ninguna US descartada completamente

---

### Enfoque 2: Value vs Effort Matrix

| Cuadrante | User Story | ID | Valor | Esfuerzo | Razonamiento |
|-----------|------------|-----|-------|----------|--------------|
| **QUICK WINS** | Scorecards Estructurados | US-008 | Alto | Medio (M) | Alto impacto en decisiones, complejidad moderada. ROI inmediato |
| **QUICK WINS** | Feedback Consolidado | US-010 | Medio-Alto | Medio (M) | Mejora quality decisiones, implementación directa sobre US-008 |
| **QUICK WINS** | Auto-Tagging | US-006 | Medio | Medio (M) | Efficiency gain claro, rule engine relativamente simple |
| **QUICK WINS** | Aprobación de Jobs | US-004 | Medio | Medio (M) | Workflow estándar, sin integraciones complejas |
| **BIG BETS** | Parseo de CV con IA | US-001 | Muy Alto | Alto (L) | Foundational + diferenciador IA. Complejidad NLP significativa |
| **BIG BETS** | Screening Automatizado | US-005 | Muy Alto | Muy Alto (XL) | Core value prop. Requiere ML model, training data, feature engineering |
| **BIG BETS** | Pipeline Kanban | US-011 | Muy Alto | Alto (L) | Core UX esencial. Drag-and-drop + real-time moderadamente complejo |
| **BIG BETS** | Publicación Multi-Canal | US-003 | Alto | Muy Alto (XL) | Critical acquisition. Múltiples integraciones API (LinkedIn, Indeed, Glassdoor) |
| **BIG BETS** | Búsqueda Semántica | US-002 | Medio-Alto | Alto (L) | Reuse importante. Requiere Elasticsearch + embeddings |
| **FILL-INS** | Scheduling Automático | US-007 | Alto | Muy Alto (XL) | Gran time-saver pero esfuerzo desproporcionado (calendar + video APIs) |
| **TIME SINKS** | Colaboración Real-Time | US-009 | Medio | Alto (L) | Benefit marginal vs email. WebSockets + complejidad no justificada en MVP |
| **TIME SINKS** | Dashboard Analytics | US-012 | Medio | Muy Alto (XL) | Nice insights pero requiere Analytics Service completo + ETL. Mejor post-MVP |

**Matriz Visual:**

```
ALTO VALOR
    │  US-008 │ US-001, US-005
    │  US-010 │ US-011, US-003
    │  US-006 │ US-002
    │  US-004 │
────┼─────────┼──────────────── ESFUERZO
    │         │ US-007, US-009
    │         │ US-012
BAJO VALOR
    BAJO      ALTO
```

**Recomendación Value/Effort:**
1. **Priorizar primero**: Quick Wins (US-008, US-010, US-006, US-004)
2. **Luego**: Big Bets críticos (US-001, US-005, US-011, US-003)
3. **Evaluar**: US-007 (alto valor pero esfuerzo XL - considerar MVP simplificado)
4. **Postponer**: US-009, US-012 (mejor ROI en fases posteriores)

---

### Enfoque 3: WSJF (Weighted Shortest Job First)

**Fórmula WSJF**: (Business Value + Time Criticality + Risk Reduction) / Job Size

**Escala**: 1-10 para cada factor (10 = máximo)

| Rank | User Story | ID | Business Value | Time Criticality | Risk Reduction | Job Size | **WSJF Score** |
|------|------------|-----|----------------|------------------|----------------|----------|----------------|
| 1 | Scorecards Estructurados | US-008 | 8 | 8 | 7 | 5 | **4.60** |
| 2 | Parseo de CV con IA | US-001 | 10 | 10 | 9 | 8 | **3.625** |
| 3 | Feedback Consolidado | US-010 | 7 | 5 | 6 | 5 | **3.60** |
| 4 | Pipeline Kanban | US-011 | 10 | 10 | 8 | 8 | **3.50** |
| 5 | Screening Automatizado | US-005 | 10 | 10 | 10 | 10 | **3.00** |
| 6 | Auto-Tagging | US-006 | 6 | 4 | 3 | 5 | **2.60** |
| 7 | Publicación Multi-Canal | US-003 | 9 | 8 | 7 | 10 | **2.40** |
| 8 | Aprobación de Jobs | US-004 | 5 | 3 | 4 | 5 | **2.40** |
| 9 | Búsqueda Semántica | US-002 | 7 | 5 | 6 | 8 | **2.25** |
| 10 | Scheduling Automático | US-007 | 8 | 7 | 6 | 10 | **2.10** |
| 11 | Colaboración Real-Time | US-009 | 6 | 4 | 5 | 8 | **1.875** |
| 12 | Dashboard Analytics | US-012 | 6 | 3 | 4 | 10 | **1.30** |

**Desglose de Scores:**

**US-008 (Scorecards)**: WSJF = 4.60
- BV=8: Decisiones de calidad, evaluación estructurada (core workflow)
- TC=8: Necesario temprano para capturar feedback de entrevistas
- RR=7: Reduce riesgo de malas contrataciones, asegura consistencia
- JS=5: Medio - Templates + forms + validación

**US-001 (Parseo CV)**: WSJF = 3.625
- BV=10: Foundational - habilita todo el sistema
- TC=10: Sin esto no hay candidatos estructurados
- RR=9: Elimina riesgo de datos inconsistentes, calidad de data crítica
- JS=8: Alto - NLP, multiple formats, normalización

**US-010 (Feedback Consolidado)**: WSJF = 3.60
- BV=7: Mejora decisiones significativamente
- TC=5: Importante pero puede posponerse ligeramente
- RR=6: Reduce riesgo de decisiones basadas en data incompleta
- JS=5: Medio - Aggregation queries + charts + PDF export

**US-011 (Pipeline Kanban)**: WSJF = 3.50
- BV=10: Core UX, interacción principal
- TC=10: Sin visualización del pipeline el sistema no es usable
- RR=8: Reduce riesgo de candidatos perdidos, bottlenecks no identificados
- JS=8: Alto - Drag-and-drop + real-time + filtros

**US-005 (Screening)**: WSJF = 3.00
- BV=10: Core value prop, diferenciador #1
- TC=10: Define la propuesta de valor única
- RR=10: Reduce riesgo competitivo máximo, prueba de concepto AI
- JS=10: Muy alto - ML model, training pipeline, feature engineering, MLflow

**Interpretación WSJF:**
- **Top 4 (WSJF > 3.5)**: US-008, US-001, US-010, US-011 → Máxima prioridad
- **Mid-tier (WSJF 2.4-3.0)**: US-005, US-006, US-003, US-004 → Segunda ola
- **Lower priority (WSJF < 2.3)**: US-002, US-007, US-009, US-012 → Backlog futuro

---

## Backlog Priorizado Final

**Metodología de Síntesis:**
Combinamos los 3 enfoques ponderando:
- **MoSCoW**: 40% (define criticidad de negocio)
- **Value/Effort**: 35% (optimiza ROI)
- **WSJF**: 25% (balancea valor y tamaño)

Además consideramos:
- **Dependencias técnicas**: US que habilitan otras tienen boost de prioridad
- **Riesgo de integración**: Features con APIs externas complejas se escalonan
- **Capacidad del equipo**: Balancear XL stories con M/L para mantener velocity

| **Prioridad** | **US** | **Título** | **Sprint** | **Esfuerzo** | **MoSCoW** | **V/E** | **WSJF** | **Justificación** |
|---------------|--------|------------|------------|--------------|------------|---------|----------|-------------------|
| **P1** | US-001 | Parseo de CV con IA | Sprint 1 | L (2-3w) | Must | Big Bet | 3.62 | **FOUNDATIONAL** - Habilita US-002, US-005, US-006. Sin datos estructurados no hay sistema. Alta prioridad en los 3 enfoques |
| **P2** | US-011 | Pipeline Kanban | Sprint 1 | L (2-3w) | Must | Big Bet | 3.50 | **CORE UX** - Interacción principal del Recruiter. Must Have + WSJF alto. Desarrollar en paralelo con US-001 |
| **P3** | US-008 | Scorecards | Sprint 2 | M (1-2w) | Must | Quick Win | 4.60 | **QUICK WIN** - WSJF más alto (4.6). Evaluación estructurada crítica. Esfuerzo moderado, alto impacto |
| **P4** | US-005 | Screening Automatizado | Sprint 2 | XL (3-4w) | Must | Big Bet | 3.00 | **CORE VALUE PROP** - Diferenciador AI principal. Depende de US-001. Must Have crítico |
| **P5** | US-003 | Publicación Multi-Canal | Sprint 3 | XL (3-4w) | Must | Big Bet | 2.40 | **ACQUISITION** - Sin candidatos aplicando no hay pipeline. Complejidad alta pero crítico para traction |
| **P6** | US-010 | Feedback Consolidado | Sprint 3 | M (1-2w) | Should | Quick Win | 3.60 | **QUICK WIN** - WSJF alto (3.6). Depende de US-008. Mejora calidad decisiones significativamente |
| **P7** | US-006 | Auto-Tagging | Sprint 4 | M (1-2w) | Could | Quick Win | 2.60 | **EFFICIENCY** - Depende de US-005. Quick Win con effort moderado. Mejora UX post-MVP |
| **P8** | US-004 | Aprobación de Jobs | Sprint 4 | M (1-2w) | Should | Fill-In | 2.40 | **GOVERNANCE** - Should Have. Puede implementarse cuando hay flujo real de jobs. Effort bajo |
| **P9** | US-002 | Búsqueda Semántica | Sprint 5 | L (2-3w) | Should | Big Bet | 2.25 | **TALENT REUSE** - Depende de US-001. Importante cuando hay base de datos poblada (post-MVP) |
| **P10** | US-007 | Scheduling Automático | Backlog | XL (3-4w) | Should | Fill-In | 2.10 | **BIG TIME-SAVER** - Alto valor pero esfuerzo XL desproporcionado. Considerar MVP manual o v2 simplificada |
| **P11** | US-009 | Colaboración Real-Time | Backlog | L (2-3w) | Could | Time Sink | 1.87 | **NICE TO HAVE** - Colaboración puede ser vía email/Slack inicialmente. WebSockets es overhead para MVP |
| **P12** | US-012 | Dashboard Analytics | Backlog | XL (3-4w) | Could | Time Sink | 1.30 | **INSIGHTS** - Valioso cuando hay datos históricos (6+ meses). Requiere Analytics Service completo. Fase 2 |

---

## Roadmap de Sprints Sugerido

**Asunciones:**
- **Duración de Sprint**: 2 semanas
- **Velocidad del Equipo**: ~3-4 semanas de esfuerzo por sprint (team de 3-4 devs)
- **Paralelización**: US independientes pueden desarrollarse en paralelo

---

### **Sprint 1: MVP Foundation (Semanas 1-2)**
**Objetivo**: Establecer la fundación técnica y la UX core del sistema

**User Stories:**
- ✅ **US-001: Parseo de CV** (L - 2-3 semanas) - *Track 1: Backend + AI*
- ✅ **US-011: Pipeline Kanban** (L - 2-3 semanas) - *Track 2: Frontend*

**Entregables:**
- Sistema de parsing de CVs funcional (PDF, Word, LinkedIn)
- Base de datos de candidatos estructurada poblable
- Tablero Kanban visual con drag-and-drop
- Gestión básica de etapas de candidatos

**Riesgos:**
- Precisión del parser puede requerir iteraciones
- Integración LinkedIn API puede tener delays (requiere partnership)

**Mitigación:**
- Comenzar con parser básico (PDF/Word) y agregar LinkedIn incrementalmente
- Tener fallback a entrada manual si parsing falla

---

### **Sprint 2: Core Intelligence (Semanas 3-4)**
**Objetivo**: Implementar el diferenciador IA y evaluación estructurada

**User Stories:**
- ✅ **US-008: Scorecards** (M - 1-2 semanas) - *Quick Win*
- ✅ **US-005: Screening Automatizado** (XL - 3-4 semanas, **inicia aquí, termina Sprint 3**) - *Big Bet*

**Entregables:**
- Templates de scorecards por tipo de entrevista
- Formularios de evaluación estructurada
- Modelo ML de scoring entrenado (v1)
- Auto-scoring de candidatos funcionando

**Dependencias Críticas:**
- **US-005 requiere**: Dataset histórico etiquetado (mínimo 5k aplicaciones)
- **US-008 requiere**: Modelo de datos INTERVIEW completado

**Nota**: US-005 es XL (3-4 semanas), comenzará en Sprint 2 pero continuará en Sprint 3

---

### **Sprint 3: Acquisition & Decision Quality (Semanas 5-6)**
**Objetivo**: Habilitar acquisition de candidatos y mejorar quality de decisiones

**User Stories:**
- ✅ **US-005: Screening Automatizado** (continúa desde Sprint 2)
- ✅ **US-003: Publicación Multi-Canal** (XL - 3-4 semanas, **inicia aquí, termina Sprint 4**)
- ✅ **US-010: Feedback Consolidado** (M - 1-2 semanas) - *Quick Win*

**Entregables:**
- Match Score predictivo operacional
- Integraciones con LinkedIn, Indeed, Glassdoor (al menos 2)
- One-click job publishing funcional
- Vista consolidada de feedback de entrevistas
- Export a PDF de evaluaciones

**Riesgos:**
- APIs externas pueden tener sandboxes limitados
- Rate limits en job boards

**Mitigación:**
- Desarrollar adaptadores mock para testing
- Implementar queue-based publishing con retry logic

---

### **Sprint 4: Polish & Governance (Semanas 7-8)**
**Objetivo**: Refinamiento de UX y procesos de governance

**User Stories:**
- ✅ **US-003: Publicación Multi-Canal** (continúa desde Sprint 3)
- ✅ **US-006: Auto-Tagging** (M - 1-2 semanas)
- ✅ **US-004: Aprobación de Jobs** (M - 1-2 semanas)

**Entregables:**
- Multi-channel publishing completo con tracking UTM
- Sistema de tagging automático con 9+ reglas
- Workflow de aprobación HM → Recruiter
- MVP completo y funcional

**Milestone**: 🎉 **MVP READY** - Sistema listo para beta testing con primeros clientes

---

### **Sprint 5-6: Enhancement & Scale (Semanas 9-12) - Post-MVP**
**Objetivo**: Features de valor agregado y preparación para escala

**User Stories:**
- ✅ **US-002: Búsqueda Semántica** (L - 2-3 semanas)
- ✅ **US-007: Scheduling Automático** (XL - 3-4 semanas, **MVP simplificado**)

**Entregables:**
- Motor de búsqueda semántica con Elasticsearch
- Embeddings de skills implementados
- Scheduling automático con calendarios (versión simplificada: solo Google Calendar inicialmente)

**Consideraciones:**
- **US-007**: Implementar MVP con solo Google Calendar + Zoom (reduce esfuerzo de XL a L)
- Agregar Outlook + Teams en iteración futura
- Elasticsearch requiere infraestructura adicional - provisionar en Sprint 4

---

### **Backlog Futuro (Post-Sprint 6)**

**Features para Fase 2:**
- **US-009: Colaboración Real-Time** - Cuando hay múltiples usuarios activos simultáneamente
- **US-012: Dashboard Analytics** - Cuando hay 3-6 meses de datos históricos para métricas significativas

**Evoluciones Potenciales:**
- **US-007 completo**: Agregar Outlook + Teams, scheduling para panels de 5+ personas
- **US-002 avanzado**: Agregar ML recommendations "Candidatos similares a este"
- **US-005 v2**: Reentrenamiento automático del modelo cada semana
- **Integraciones adicionales**: HackerRank, Codility, background checks

---

## Tabla Comparativa de Metodologías

Análisis de cómo cada metodología impactó la priorización final:

| User Story | MoSCoW Rank | V/E Rank | WSJF Rank | **Final Rank** | Concordancia | Observaciones |
|------------|-------------|----------|-----------|----------------|--------------|---------------|
| US-008 (Scorecards) | 5 (Must) | 1 (QW) | **1** | **P3** | ⚠️ Divergente | WSJF lo coloca #1 pero dependencias técnicas lo mueven a Sprint 2 |
| US-001 (Parseo CV) | 1 (Must) | 5 (BB) | **2** | **P1** | ✅ Alta | Consenso en top 3 de todas las metodologías. Foundational |
| US-011 (Kanban) | 3 (Must) | 7 (BB) | **4** | **P2** | ✅ Alta | Must Have + Core UX. Concordancia en top 5 |
| US-005 (Screening) | 2 (Must) | 6 (BB) | **5** | **P4** | ✅ Alta | Core value prop. WSJF penalizado por job size (XL) |
| US-003 (Multi-Canal) | 4 (Must) | 8 (BB) | 7 | **P5** | ⚠️ Media | Must Have pero esfuerzo XL retrasa ejecución |
| US-010 (Feedback) | 8 (Should) | 2 (QW) | **3** | **P6** | ⚠️ Divergente | WSJF alto pero Should Have en MoSCoW. Depende de US-008 |
| US-006 (Tagging) | 10 (Could) | 3 (QW) | 6 | **P7** | ⚠️ Media | Could Have pero Quick Win con buen WSJF. Post-MVP |
| US-004 (Aprobación) | 9 (Should) | 4 (FI) | 8 | **P8** | ✅ Alta | Concordancia en tier medio. Governance importante pero no urgente |
| US-002 (Búsqueda) | 6 (Should) | 9 (BB) | 9 | **P9** | ✅ Alta | Should Have. Depende de US-001. Post-MVP |
| US-007 (Scheduling) | 7 (Should) | 10 (FI) | 10 | **P10** | ✅ Alta | Alto valor pero effort XL desproporcionado. Considerar MVP simplificado |
| US-009 (Collab RT) | 11 (Could) | 11 (TS) | 11 | **P11** | ✅ Alta | Consenso en baja prioridad. Nice to have para Fase 2 |
| US-012 (Analytics) | 12 (Could) | 12 (TS) | 12 | **P12** | ✅ Alta | Consenso unánime en backlog futuro. Requiere datos históricos |

**Leyenda:**
- QW: Quick Win | BB: Big Bet | FI: Fill-In | TS: Time Sink
- ✅ Alta concordancia (ranking similar en 2+ metodologías)
- ⚠️ Divergencia (rankings varían >3 posiciones entre metodologías)

---

## Insights del Análisis Multi-Metodológico

### 1. **Consenso Fuerte (Top Priorities)**
Las siguientes US tienen **alta concordancia** en las 3 metodologías:
- **US-001 (Parseo CV)**: Foundational indiscutible
- **US-005 (Screening)**: Core value prop reconocido
- **US-011 (Kanban)**: Core UX esencial
- **US-003 (Multi-Canal)**: Acquisition crítica

**Recomendación**: Estos 4 forman el **núcleo del MVP** (Sprints 1-3)

### 2. **Quick Wins Identificados**
Value/Effort matrix revela **oportunidades de ROI rápido**:
- **US-008 (Scorecards)**: M effort, alto impacto → Sprint 2
- **US-010 (Feedback)**: M effort, mejora decisiones → Sprint 3
- **US-006 (Tagging)**: M effort, efficiency gain → Sprint 4

**Recomendación**: Intercalar Quick Wins entre Big Bets para mantener momentum

### 3. **Divergencias Notables**

**US-008 (Scorecards)**:
- WSJF: Rank #1 (score 4.60)
- MoSCoW: Must Have (#5)
- V/E: Quick Win (#1)
- **Decisión**: Prioridad P3 (Sprint 2) - Balance entre WSJF alto y dependencias técnicas

**US-010 (Feedback Consolidado)**:
- WSJF: Rank #3 (score 3.60)
- MoSCoW: Should Have (#8)
- V/E: Quick Win (#2)
- **Decisión**: Prioridad P6 (Sprint 3) - Depende de US-008, pero quick win claro

**Análisis**: WSJF favorece features con menor job size (M vs XL), revelando oportunidades que MoSCoW puede subestimar

### 4. **Big Bets con ROI a Largo Plazo**

Features con **esfuerzo XL pero Must Have**:
- **US-005 (Screening)**: 3-4 semanas pero core differentiator
- **US-003 (Multi-Canal)**: 3-4 semanas pero habilita acquisition
- **US-007 (Scheduling)**: 3-4 semanas pero 80% time reduction

**Estrategia**:
- US-005 y US-003: Invertir el esfuerzo XL (son Must Have)
- US-007: Considerar **MVP simplificado** (solo Google Calendar) para reducir a L

### 5. **Features para Backlog Futuro**

**Consenso en postponer**:
- **US-009 (Colaboración RT)**: WebSockets es overhead innecesario en MVP
- **US-012 (Analytics)**: Sin datos históricos (6+ meses) las métricas no son significativas

**Trigger para reactivar**:
- US-009: Cuando >20 usuarios concurrentes activos
- US-012: Después de 3-6 meses de operación con datos reales

### 6. **Impacto de Dependencias Técnicas**

**Cadenas de dependencia críticas**:
1. **US-001 → US-005 → US-006**: Parseo habilita screening habilita tagging
2. **US-008 → US-010**: Scorecards habilitan consolidación de feedback
3. **US-001 → US-002**: Parseo habilita búsqueda semántica

**Implicación**: El ordenamiento final respeta estas cadenas incluso cuando WSJF sugiere otro orden

---

## Recomendaciones Finales

### Para Product Owner:
1. **Validar priorización** con stakeholders clave (Recruiters, Hiring Managers)
2. **Confirmar disponibilidad** de dataset histórico para US-005 (mínimo 5k aplicaciones etiquetadas)
3. **Negociar partnerships** con LinkedIn, Indeed, Glassdoor para APIs de US-003
4. **Definir métricas de éxito** por sprint (KPIs de adopción, quality, performance)

### Para Engineering Lead:
1. **Provisionar infraestructura** temprano: AWS S3 (CVs), Elasticsearch (búsqueda), MLflow (modelos)
2. **Recrutar/training** en ML para US-005: modelo XGBoost, feature engineering, MLOps
3. **Establecer arquitectura** de microservicios desde Sprint 1 para escalabilidad
4. **Configurar CI/CD** y ambientes (dev/staging/prod) antes de Sprint 1

### Para Sprint Planning:
1. **Sprint 1-2**: Enfoque en fundación (US-001, US-011) + quick win (US-008)
2. **Sprint 3-4**: Core differentiation (US-005) + acquisition (US-003)
3. **Sprint 5-6**: Enhancement features (US-002, US-007 simplificado)
4. **Backlog futuro**: Re-evaluar US-009, US-012 después de 6 meses de operación

### Métricas de Éxito del MVP (Post-Sprint 4):
- ✅ **Parsing accuracy**: >85% en campos críticos
- ✅ **Match Score AUC-ROC**: >0.75
- ✅ **Jobs publicados**: 50+ en primeros 30 días
- ✅ **Time-to-hire**: Reducción medible vs baseline manual
- ✅ **User adoption**: 15+ beta customers activos
- ✅ **NPS**: >40 en primeros usuarios

---

**Próxima Revisión**: Re-evaluar backlog después de Sprint 4 (MVP complete) basándose en feedback de beta users y métricas reales de adopción.

---

## Índice de User Stories

### A. Gestión de Candidatos
- [US-001: Parseo Inteligente de CV con IA](#us-001-parseo-inteligente-de-cv-con-ia)
- [US-002: Búsqueda Semántica en Base de Talentos](#us-002-búsqueda-semántica-en-base-de-talentos)

### B. Gestión de Vacantes
- [US-003: Publicación Multi-Canal de Ofertas de Trabajo](#us-003-publicación-multi-canal-de-ofertas-de-trabajo)
- [US-004: Flujo de Aprobación de Ofertas de Trabajo](#us-004-flujo-de-aprobación-de-ofertas-de-trabajo)

### C. Screening y Matching con IA
- [US-005: Screening Automatizado con Scoring Predictivo](#us-005-screening-automatizado-con-scoring-predictivo)
- [US-006: Etiquetado Inteligente Automático de Candidatos](#us-006-etiquetado-inteligente-automático-de-candidatos)

### D. Gestión de Entrevistas
- [US-007: Programación Automática de Entrevistas con Integración de Calendarios](#us-007-programación-automática-de-entrevistas-con-integración-de-calendarios)
- [US-008: Evaluación Estructurada con Scorecards](#us-008-evaluación-estructurada-con-scorecards)

### E. Colaboración y Toma de Decisiones
- [US-009: Evaluación Colaborativa en Tiempo Real](#us-009-evaluación-colaborativa-en-tiempo-real)
- [US-010: Vista Consolidada de Feedback de Entrevistas](#us-010-vista-consolidada-de-feedback-de-entrevistas)

### F. Gestión del Pipeline
- [US-011: Pipeline Visual Kanban para Gestión de Candidatos](#us-011-pipeline-visual-kanban-para-gestión-de-candidatos)

### G. Analítica y Reporting
- [US-012: Dashboard de Métricas de Reclutamiento en Tiempo Real](#us-012-dashboard-de-métricas-de-reclutamiento-en-tiempo-real)

---

## Leyenda de Prioridades

| Prioridad | Descripción | Criterio |
|-----------|-------------|----------|
| **Alta** | Funcionalidad crítica para MVP o valor de negocio inmediato | Debe estar en las primeras 2-3 iteraciones |
| **Media** | Funcionalidad importante pero no bloqueante | Puede desarrollarse en iteraciones 4-6 |
| **Baja** | Funcionalidad deseable, mejora la experiencia | Se puede posponer para fases posteriores |

## Leyenda de Estimaciones (T-Shirt Sizing)

| Tamaño | Descripción | Complejidad | Duración Estimada |
|--------|-------------|-------------|-------------------|
| **XS** | Tarea muy simple, cambio menor | Mínima | 1-2 días |
| **S** | Tarea simple con dependencias limitadas | Baja | 3-5 días |
| **M** | Tarea moderada, requiere diseño y testing | Media | 1-2 semanas |
| **L** | Tarea compleja, múltiples componentes | Alta | 2-3 semanas |
| **XL** | Épica o tarea muy compleja, considerar división | Muy alta | 3-4 semanas o más |

## Convenciones Utilizadas

- **Roles de Usuario:** Recruiter (Reclutador), Hiring Manager (Gerente de Contratación), Interviewer (Entrevistador), Admin (Administrador)
- **DoD:** Definition of Done (Definición de Hecho)
- **API:** Application Programming Interface
- **NLP:** Natural Language Processing
- **ML:** Machine Learning
- **RBAC:** Role-Based Access Control
- **SLA:** Service Level Agreement

---

# User Stories Detalladas

---

## US-001: Parseo Inteligente de CV con IA

**Como** Recruiter
**Quiero** que el sistema extraiga automáticamente información estructurada de los CVs que suben los candidatos (PDF, Word, LinkedIn)
**Para** ahorrar tiempo de entrada manual de datos y garantizar que todos los perfiles estén completos y normalizados desde el primer momento

### Criterios de Aceptación

- [ ] El sistema acepta múltiples formatos de CV: PDF, DOCX, TXT, y URLs de LinkedIn
- [ ] El parser extrae automáticamente con >85% de precisión: nombre completo, email, teléfono, ubicación, años de experiencia total, título actual, empresa actual, historial laboral (empresa, puesto, fechas), educación (institución, título, fechas), habilidades técnicas, y certificaciones
- [ ] Los datos extraídos se almacenan en formato estructurado en la base de datos (tabla CANDIDATE y JSON en campo parsed_resume)
- [ ] El sistema normaliza nombres de habilidades usando un diccionario (ej: "React.js" = "React" = "ReactJS")
- [ ] Si el parsing falla o tiene baja confianza (<60%), el sistema marca el candidato para revisión manual y notifica al Recruiter
- [ ] El sistema muestra una vista de "datos extraídos" editable donde el Recruiter puede corregir información incorrecta antes de guardar
- [ ] El proceso de parsing se completa en menos de 30 segundos para el 95% de los CVs
- [ ] El sistema detecta y alerta sobre CVs duplicados basándose en email o combinación nombre+teléfono

### Notas Técnicas

- **Stack tecnológico:** Python + FastAPI para servicio de IA, spaCy para NER (Named Entity Recognition), biblioteca PyPDF2/pdfplumber para PDFs, python-docx para Word
- **Modelo NLP:** Usar spaCy pre-entrenado en español/inglés con fine-tuning en dataset de CVs técnicos (mínimo 5,000 CVs etiquetados)
- **OCR para PDFs complejos:** Integrar AWS Textract o Google Vision API para ~10% de PDFs con formato complejo o escaneados
- **Normalización de habilidades:** Mantener diccionario maestro en tabla SKILLS con sinónimos y variaciones
- **API de LinkedIn:** Utilizar API oficial de LinkedIn (requiere partnership) o scraping autorizado con consentimiento del candidato
- **Caché:** Cachear resultados de parsing por hash del documento para evitar reprocesamiento
- **Cola asíncrona:** Usar RabbitMQ o AWS SQS para procesar parsing en background sin bloquear UI
- **Almacenamiento:** CVs originales en AWS S3 con versionado, datos estructurados en PostgreSQL + MongoDB para JSON
- **Fallback a LLM:** Para 5% de CVs con formato no estándar, usar GPT-4 o Claude API como fallback (costo-beneficio a evaluar)

### Definición de Hecho (DoD)

- [ ] Código implementado en servicio AI Screening con endpoints REST documentados en Swagger
- [ ] Tests unitarios con cobertura >80% para cada extractor (nombre, email, skills, etc.)
- [ ] Tests de integración con ejemplos reales de CVs en diferentes formatos (mínimo 50 casos de prueba)
- [ ] Performance testing: 100 CVs procesados en paralelo sin degradación (<30s p95)
- [ ] Documentación técnica: arquitectura del parser, modelos utilizados, diccionario de skills, instrucciones de fine-tuning
- [ ] Métricas de precisión medidas en dataset de validación (>85% accuracy por campo)
- [ ] Logs estructurados con trazabilidad completa (correlation IDs)
- [ ] Manejo de errores con reintentos exponenciales y circuit breaker
- [ ] Aprobado por Product Owner en demo con CVs reales del equipo

### Prioridad: Alta
### Estimación Inicial: L (2-3 semanas)
### Dependencias:
- Infraestructura de microservicios base (AI Screening Service)
- Modelo de datos CANDIDATE y CANDIDATE_SKILL implementado
- Almacenamiento S3 o equivalente configurado

---

## US-002: Búsqueda Semántica en Base de Talentos

**Como** Recruiter
**Quiero** buscar candidatos en la base de datos usando lenguaje natural y filtros semánticos (ej: "desarrollador full-stack con React en Madrid con más de 3 años de experiencia")
**Para** encontrar rápidamente candidatos relevantes para nuevas posiciones sin depender de palabras clave exactas, reutilizando el talento existente

### Criterios de Aceptación

- [ ] El sistema permite búsqueda en lenguaje natural procesando la query con NLP para extraer intención (skills, ubicación, experiencia, etc.)
- [ ] La búsqueda entiende sinónimos y variaciones de habilidades (ej: "JavaScript" encuentra también "JS", "Node.js", "React")
- [ ] Los resultados se ordenan por relevancia usando un score de similitud semántica (0-100)
- [ ] El sistema muestra en cada resultado: nombre, foto, título actual, años de experiencia, ubicación, top 5 skills, match score, y última interacción
- [ ] Filtros avanzados aplicables: ubicación (con radio geográfico), rango de años de experiencia, nivel educativo, disponibilidad, estado de autorización de trabajo, empresas previas, última aplicación (rango de fechas)
- [ ] La búsqueda devuelve resultados en menos de 2 segundos para queries sobre bases de hasta 100,000 candidatos
- [ ] El sistema guarda las búsquedas frecuentes y permite crear "Saved Searches" con alertas cuando nuevos candidatos coinciden
- [ ] La interfaz sugiere autocompletado de habilidades y ubicaciones mientras el usuario escribe

### Notas Técnicas

- **Motor de búsqueda:** Elasticsearch 8 con índices optimizados para full-text search y semantic search
- **Embeddings semánticos:** Generar vectores de skills usando modelos pre-entrenados (Sentence-BERT, all-MiniLM-L6-v2) y almacenar en Elasticsearch con campo dense_vector
- **Scoring híbrido:** Combinar BM25 (keyword matching) + cosine similarity (semantic matching) con pesos configurables (60% semántico, 40% keyword)
- **NLP para query parsing:** Usar spaCy NER para extraer entidades de la query (skills → etiquetas SKILL, ciudades → etiquetas GPE, números → años experiencia)
- **Sinónimos:** Mantener diccionario de sinónimos en Elasticsearch synonym filter (React: [react, reactjs, react.js])
- **Índices:** Crear índices compuestos en PostgreSQL para filtros comunes (location + years_experience, skills + availability)
- **Caché:** Redis para cachear queries frecuentes (TTL 5 minutos)
- **Geolocalización:** Usar Elasticsearch geo_distance query para búsquedas por radio geográfico
- **Paginación:** Implementar cursor-based pagination para grandes resultados (evitar OFFSET ineficiente)
- **Análisis de uso:** Trackear queries populares para mejorar sinónimos y sugerencias

### Definición de Hecho (DoD)

- [ ] API REST implementada con endpoints para search, autocomplete, saved searches
- [ ] Elasticsearch configurado con mappings personalizados, analyzers y synonym filters
- [ ] Tests unitarios para query parser y scoring algorithm (>80% cobertura)
- [ ] Tests de integración con dataset real de 10,000+ candidatos
- [ ] Performance testing: <2s para búsquedas en 100k candidatos (p95)
- [ ] UI implementada con campo de búsqueda, filtros laterales, resultados paginados, y opción de guardar búsqueda
- [ ] Documentación de API con ejemplos de queries complejas
- [ ] Métricas de relevancia evaluadas con usuarios reales (NDCG > 0.7)
- [ ] Logs de queries para análisis de uso y mejora continua
- [ ] Aprobado por Product Owner con demos de búsquedas complejas

### Prioridad: Alta
### Estimación Inicial: L (2-3 semanas)
### Dependencias:
- US-001 (Parseo de CV) - necesita base de datos poblada con candidatos
- Elasticsearch configurado y operacional
- Modelo de datos CANDIDATE y CANDIDATE_SKILL completo

---

## US-003: Publicación Multi-Canal de Ofertas de Trabajo

**Como** Recruiter
**Quiero** publicar una oferta de trabajo en múltiples plataformas externas (LinkedIn, Indeed, Glassdoor) con un solo clic
**Para** maximizar el alcance de mis vacantes sin tener que publicar manualmente en cada plataforma, ahorrando tiempo y garantizando consistencia

### Criterios de Aceptación

- [ ] Desde la vista de detalle de un Job (estado "Approved"), el Recruiter puede seleccionar múltiples canales de publicación mediante checkboxes (LinkedIn, Indeed, Glassdoor, sitio web corporativo)
- [ ] El sistema adapta automáticamente el formato de la oferta según los requisitos de cada plataforma (límites de caracteres, campos obligatorios, categorías)
- [ ] Antes de publicar, el sistema muestra una vista previa de cómo se verá la oferta en cada plataforma seleccionada
- [ ] Al confirmar publicación, el sistema publica en paralelo en todos los canales seleccionados y muestra un indicador de progreso
- [ ] Si alguna publicación falla, el sistema continúa con las demás, notifica el error con detalles específicos, y permite reintentar individualmente
- [ ] Cada publicación exitosa genera un registro en tabla JOB_POSTING con platform_job_id, URL directa, y timestamp
- [ ] El sistema configura automáticamente tracking UTM para cada canal (source, medium, campaign) para attribution
- [ ] El Recruiter puede ver el estado de todas las publicaciones activas en un panel consolidado con métricas (views, applies por plataforma)
- [ ] El sistema permite despublicar ofertas de todos los canales simultáneamente o individualmente

### Notas Técnicas

- **Arquitectura:** Integrations Service (Node.js) con adaptadores por plataforma (Strategy pattern)
- **APIs externas:**
  - LinkedIn Jobs API (requiere partnership)
  - Indeed API (API key requerida)
  - Glassdoor API (OAuth 2.0)
  - Web corporativo: endpoint interno
- **Manejo de rate limits:** Implementar exponential backoff y queue-based publishing para respetar límites de API
- **Formato de adaptación:** Mapper por plataforma que transforma Job model a formato específico (validación con JSON schemas)
- **Preview generation:** Templates HTML/CSS responsive que simulan el rendering de cada plataforma
- **Tracking UTM:** Patrón estándar: ?utm_source={platform}&utm_medium=job_board&utm_campaign={job_id}
- **Error handling:** Circuit breaker pattern para evitar cascading failures, logs detallados por plataforma
- **Webhooks:** Configurar webhooks de plataformas para recibir notificaciones de applies directamente
- **Credenciales:** Almacenar API keys y tokens en AWS Secrets Manager o HashiCorp Vault
- **Retry logic:** Queue con reintentos automáticos (3 intentos con backoff de 1min, 5min, 15min)
- **Bidirectional sync:** Job de sincronización cada hora para actualizar métricas (views, applies) desde plataformas

### Definición de Hecho (DoD)

- [ ] Integrations Service implementado con adaptadores para mínimo 3 plataformas (LinkedIn, Indeed, Glassdoor)
- [ ] Endpoints REST: POST /jobs/{id}/publish, DELETE /jobs/{id}/unpublish, GET /jobs/{id}/postings
- [ ] Tests unitarios para cada adapter con >80% cobertura
- [ ] Tests de integración con APIs reales en entorno sandbox/staging
- [ ] Tests end-to-end simulando publicación completa y manejo de errores
- [ ] UI implementada: modal de selección de canales, previews, indicador de progreso, panel de estado de publicaciones
- [ ] Documentación técnica: guía de integración por plataforma, troubleshooting común, configuración de API keys
- [ ] Monitoreo configurado: alertas para fallos de publicación, métricas de success rate por plataforma
- [ ] Logs estructurados con correlation IDs para debugging
- [ ] Demo exitosa con publicación real en 3 plataformas
- [ ] Aprobado por Product Owner

### Prioridad: Alta
### Estimación Inicial: XL (3-4 semanas)
### Dependencias:
- Modelo de datos JOB y JOB_POSTING implementado
- Partnerships o API keys obtenidos para plataformas externas
- Integrations Service base implementado

---

## US-004: Flujo de Aprobación de Ofertas de Trabajo

**Como** Hiring Manager
**Quiero** revisar y aprobar/rechazar ofertas de trabajo creadas por Recruiters antes de que se publiquen
**Para** garantizar que las descripciones y requisitos sean precisos, estén alineados con las necesidades del equipo y cumplan con políticas de la empresa

### Criterios de Aceptación

- [ ] Cuando un Recruiter completa la creación de un Job y lo marca como "Ready for Approval", el sistema cambia automáticamente el estado a "Pending Approval" y notifica al Hiring Manager asignado
- [ ] El Hiring Manager recibe una notificación (email + in-app) con link directo al Job para revisión
- [ ] En la vista de aprobación, el HM puede ver todos los detalles del Job: título, descripción completa, requisitos (required_skills y nice_to_have_skills), ubicación, salary range, tipo de empleo, departamento
- [ ] El HM puede realizar una de tres acciones: (1) Aprobar, (2) Rechazar con comentarios obligatorios, (3) Solicitar cambios con comentarios específicos
- [ ] Si el HM selecciona "Solicitar cambios", puede agregar comentarios inline en campos específicos y @mencionar al Recruiter
- [ ] Al aprobar, el Job cambia a estado "Approved" y se habilita el botón de "Publish" para el Recruiter
- [ ] Al rechazar, el Job cambia a estado "Rejected", se notifica al Recruiter con los comentarios, y el Job no puede publicarse
- [ ] Al solicitar cambios, el Job vuelve a estado "Draft", se notifica al Recruiter, quien puede editar y re-enviar para aprobación
- [ ] Todo el historial de aprobaciones/rechazos/cambios se registra en una tabla APPROVAL_HISTORY con timestamps y comentarios
- [ ] El sistema muestra un timeline visual con todas las transiciones de estado del Job

### Notas Técnicas

- **Estado del Job:** Agregar estados "Pending Approval", "Changes Requested", "Rejected" al enum de status en modelo JOB
- **Modelo de aprobación:** Tabla APPROVAL_HISTORY (job_id, approver_id, action [approved/rejected/changes_requested], comments, created_at)
- **Notificaciones:** Usar Notifications Service con templates para emails y in-app notifications
- **Comentarios inline:** Usar estructura JSON en campo comments con formato {field: "description", comment: "Necesita más detalle técnico"}
- **Menciones:** Parser de @username en comentarios, notificaciones automáticas a usuarios mencionados
- **Permisos:** Solo Hiring Managers asignados al Job pueden aprobar (validar RBAC en backend)
- **SLA tracking:** Opcional - trackear tiempo desde "Pending Approval" hasta decisión para métricas de eficiencia
- **Email templates:** Diseñar templates responsive con botones de acción (Approve/Review) que abren directamente el Job en LTI
- **Audit trail:** Todos los cambios de estado emitir eventos para logging centralizado
- **Workflow engine:** Considerar uso de biblioteca de state machine (xstate o similar) para gestión robusta de transiciones

### Definición de Hecho (DoD)

- [ ] Modelo de datos actualizado con nuevos estados y tabla APPROVAL_HISTORY
- [ ] API endpoints: POST /jobs/{id}/submit-for-approval, POST /jobs/{id}/approve, POST /jobs/{id}/reject, POST /jobs/{id}/request-changes
- [ ] Tests unitarios para lógica de state transitions con >80% cobertura
- [ ] Tests de integración para flujo completo: crear → enviar a aprobación → aprobar/rechazar
- [ ] Tests de permisos: usuarios sin rol HM no pueden aprobar
- [ ] UI implementada: vista de aprobación con detalles completos, botones de acción, campo de comentarios, timeline de historial
- [ ] Notificaciones configuradas y testeadas (email + in-app)
- [ ] Documentación de estados y transiciones permitidas
- [ ] Logs de auditoría verificados en entorno de staging
- [ ] Demo exitosa con flujo completo de aprobación
- [ ] Aprobado por Product Owner

### Prioridad: Media
### Estimación Inicial: M (1-2 semanas)
### Dependencias:
- Modelo de datos JOB base implementado
- Notifications Service operacional
- Sistema de permisos RBAC funcional
- UI de creación de Jobs completa

---

## US-005: Screening Automatizado con Scoring Predictivo

**Como** Recruiter
**Quiero** que el sistema evalúe automáticamente cada candidato que aplica y le asigne un Match Score (0-100) basado en qué tan bien encaja con los requisitos del puesto
**Para** priorizar rápidamente a los mejores candidatos y reducir el tiempo dedicado a screening manual de CVs no relevantes

### Criterios de Aceptación

- [ ] Cuando un candidato aplica a un Job (nuevo APPLICATION creado), el sistema automáticamente dispara el flujo de screening en menos de 5 segundos
- [ ] El AI Screening Service analiza el perfil del candidato vs requisitos del Job evaluando: match de habilidades técnicas (required y nice-to-have), años de experiencia relevante, nivel educativo, fit de industria/sector, señales de calidad (empresas reconocidas, progresión de carrera)
- [ ] El sistema genera un Match Score (número entero 0-100) usando modelo de ML entrenado con datos históricos de contrataciones exitosas
- [ ] El score se almacena en el campo match_score de la tabla APPLICATION junto con un breakdown detallado en JSON (ai_analysis: {skill_match: 85, experience_match: 90, education_match: 75, ...})
- [ ] En el dashboard del Recruiter, los candidatos se ordenan automáticamente por Match Score descendente al abrir un Job
- [ ] Candidatos con score >80 reciben automáticamente la etiqueta "High Priority" y generan una notificación al Recruiter
- [ ] El sistema muestra visualmente el desglose del score con indicadores por categoría (skills: 85/100, experiencia: 90/100, etc.)
- [ ] Si el modelo tiene baja confianza (<50% confidence), el sistema marca el candidato para revisión manual
- [ ] El Recruiter puede dar feedback en el score (thumbs up/down) que se usa para reentrenamiento del modelo

### Notas Técnicas

- **Modelo de ML:** XGBoost o LightGBM entrenado con dataset histórico (mínimo 5,000 aplicaciones con outcome: hired/rejected)
- **Features del modelo:**
  - Skill match percentage (jaccard similarity entre required_skills y candidate_skills)
  - Semantic skill match (embeddings similarity usando BERT)
  - Years of experience delta (actual vs requerido)
  - Education level score (PhD:5, Master:4, Bachelor:3, etc.)
  - Company quality score (basado en ranking de empresas: FAANG=5, startup conocido=4, etc.)
  - Career trajectory (promotions, title progression)
  - Industry overlap
- **Training pipeline:** Pipeline automatizado en Python con MLflow para versionado de modelos
- **Model serving:** Modelo desplegado en SageMaker o contenedor Docker con API REST
- **Feature store:** PostgreSQL table con features pre-computadas para serving rápido
- **Umbral de confianza:** Calibrar modelo para output de confidence score, marcar para revisión si <50%
- **Feedback loop:** Tabla SCREENING_FEEDBACK (application_id, predicted_score, recruiter_feedback, actual_outcome) para reentrenamiento
- **Reentrenamiento:** Job semanal o cada 10k aplicaciones nuevas para reentrenar con datos frescos
- **A/B testing:** Framework para probar nuevas versiones del modelo vs modelo actual (champion/challenger)
- **Explicabilidad:** Usar SHAP values para explicar por qué un candidato recibió cierto score
- **Performance:** Scoring debe completarse en <10 segundos para 95% de los casos

### Definición de Hecho (DoD)

- [ ] Modelo de ML entrenado con AUC-ROC >0.75 en test set
- [ ] AI Screening Service implementado con endpoint POST /screen-candidate
- [ ] Pipeline de features implementado para extraer y calcular todas las features necesarias
- [ ] Tests unitarios para cálculo de features con >80% cobertura
- [ ] Tests de integración con dataset real de 1000+ candidatos
- [ ] Performance testing: screening de 100 candidatos en paralelo <10s cada uno
- [ ] UI actualizada: vista de candidatos ordenada por score, breakdown visual del score, botón de feedback
- [ ] Modelo deployado en producción con versionado en MLflow
- [ ] Monitoring configurado: latencia de scoring, distribución de scores, feedback rate
- [ ] Documentación técnica: arquitectura del modelo, features utilizadas, proceso de reentrenamiento
- [ ] Logs estructurados con scores y breakdown para análisis
- [ ] Demo con candidatos reales mostrando scoring preciso
- [ ] Aprobado por Product Owner

### Prioridad: Alta
### Estimación Inicial: XL (3-4 semanas)
### Dependencias:
- US-001 (Parseo de CV) - necesita datos estructurados de candidatos
- Modelo de datos APPLICATION con campo match_score y ai_analysis (JSON)
- Dataset histórico de aplicaciones etiquetadas para entrenamiento
- Infraestructura de ML (MLflow, SageMaker o equivalente)

---

## US-006: Etiquetado Inteligente Automático de Candidatos

**Como** Recruiter
**Quiero** que el sistema asigne automáticamente etiquetas descriptivas a cada candidato (ej: "Top Performer", "Missing Skills: Python", "Career Changer")
**Para** filtrar y segmentar rápidamente candidatos por características clave sin tener que leer cada CV completo

### Criterios de Aceptación

- [ ] Cuando se completa el screening de un candidato, el sistema automáticamente genera y asigna etiquetas relevantes basadas en reglas de negocio y análisis de IA
- [ ] El sistema soporta las siguientes etiquetas predefinidas:
  - "Top Performer" (score >85 + experiencia en empresas top-tier)
  - "High Potential" (score 70-84 + progresión de carrera rápida)
  - "Career Changer" (cambio significativo de industria/rol en últimos 2 años)
  - "Overqualified" (años de experiencia >150% del requerido + senior titles)
  - "Missing Skills: X, Y" (skills críticos ausentes del perfil)
  - "Hot Lead" (score >80 + aplicación reciente <48h)
  - "Culture Fit Risk" (alta rotación laboral: >3 cambios en últimos 2 años)
  - "Local Talent" (ubicación match exacto con Job location)
  - "Relocation Required" (ubicación diferente a Job location)
- [ ] Las etiquetas se almacenan en el campo tags (JSON array) de la tabla APPLICATION
- [ ] En la UI, las etiquetas se muestran como badges de colores junto al nombre del candidato
- [ ] El Recruiter puede filtrar la lista de candidatos por una o múltiples etiquetas usando un filtro lateral
- [ ] El Recruiter puede agregar etiquetas personalizadas manualmente (custom tags)
- [ ] El Recruiter puede eliminar etiquetas auto-generadas que considere incorrectas
- [ ] Las etiquetas se actualizan automáticamente si cambia información del candidato (ej: actualización de CV)
- [ ] El sistema muestra un tooltip explicativo al hacer hover sobre cada etiqueta indicando por qué fue asignada

### Notas Técnicas

- **Motor de reglas:** Implementar rule engine con lógica configurable (puede usar biblioteca como json-rules-engine o implementación custom)
- **Reglas de negocio:** Definir reglas en formato JSON para fácil modificación sin código:
  ```json
  {
    "tag": "Top Performer",
    "conditions": {
      "all": [
        {"fact": "match_score", "operator": "greaterThan", "value": 85},
        {"fact": "company_tier", "operator": "in", "value": ["tier1", "tier2"]}
      ]
    }
  }
  ```
- **Skill gap detection:** Comparar required_skills del Job con candidate_skills, listar diferencias
- **Career trajectory analysis:** Calcular velocidad de progresión basado en cambios de título y seniority
- **Company tier classification:** Mantener diccionario de empresas clasificadas por tier (tier1: FAANG, unicorns; tier2: conocidas; tier3: resto)
- **Rotación laboral:** Calcular número de empleos en ventana de tiempo desde work history
- **Geolocalización:** Usar geocoding para calcular distancia entre candidate location y job location
- **Custom tags:** Almacenar en mismo array JSON con flag {tag: "Custom Tag", is_custom: true}
- **Performance:** Tag generation debe ejecutarse en <2 segundos como parte del screening workflow
- **Color coding:** Definir esquema de colores consistente (verde: positivo, rojo: warning, azul: neutral, amarillo: atención)
- **Auditoría:** Registrar cuando se crean/eliminan tags y por quién (AUTO_SYSTEM vs USER_ID)

### Definición de Hecho (DoD)

- [ ] Rule engine implementado con mínimo 9 reglas predefinidas
- [ ] Lógica de tagging integrada en AI Screening Service workflow
- [ ] Tests unitarios para cada regla de tagging con >80% cobertura
- [ ] Tests de integración con candidatos de ejemplo cubriendo todos los tipos de tags
- [ ] UI actualizada: badges visuales por candidato, filtro lateral multi-select, tooltips explicativos
- [ ] API endpoints: GET /applications/{id}/tags, POST /applications/{id}/tags (agregar custom), DELETE /applications/{id}/tags/{tag}
- [ ] Documentación de reglas de negocio con ejemplos
- [ ] Configuration file para reglas en formato JSON editable sin deployment
- [ ] Performance testing: tagging de 1000 candidatos en <5min
- [ ] Logs de tags asignados para análisis y mejora de reglas
- [ ] Demo con candidatos diversos mostrando tags correctos
- [ ] Aprobado por Product Owner

### Prioridad: Media
### Estimación Inicial: M (1-2 semanas)
### Dependencias:
- US-005 (Screening Automatizado) - el tagging se ejecuta post-screening
- Modelo de datos APPLICATION con campo tags (JSON)
- Diccionario de empresas clasificadas por tier
- Lógica de cálculo de skills gap implementada

---

## US-007: Programación Automática de Entrevistas con Integración de Calendarios

**Como** Recruiter
**Quiero** que el sistema encuentre automáticamente horarios disponibles en los calendarios de los entrevistadores y el candidato, y programe las entrevistas sin coordinación manual
**Para** eliminar el intercambio tedioso de emails de "¿cuándo puedes?" y reducir el tiempo de coordinación de entrevistas en 80%

### Criterios de Aceptación

- [ ] Desde la vista del candidato, el Recruiter puede hacer clic en "Schedule Interview" y seleccionar el tipo de entrevista (Phone/Technical/Manager/Panel) y los entrevistadores participantes (1-n usuarios)
- [ ] El sistema consulta automáticamente los calendarios de todos los participantes vía integración con Google Calendar y/o Outlook
- [ ] El sistema identifica slots de tiempo comunes disponibles en los próximos 7-14 días (configurable) respetando horario laboral (9am-6pm default, configurable)
- [ ] El sistema sugiere 3-5 opciones de horarios óptimos considerando: disponibilidad de todos, minimizar tiempo de espera, evitar slots muy tempranos/tardíos
- [ ] El Recruiter selecciona uno de los slots sugeridos o puede buscar manualmente otra fecha
- [ ] Al confirmar, el sistema automáticamente: (1) crea eventos en los calendarios de todos los participantes, (2) genera link de videollamada (Zoom/Teams), (3) envía invitaciones por email con agenda, link de video, perfil del candidato, y guía de entrevista
- [ ] El sistema crea un registro en tabla INTERVIEW con status "Scheduled" y todos los detalles (participants, scheduled_time, video_link, etc.)
- [ ] El sistema envía recordatorios automáticos a todos los participantes 24 horas y 1 hora antes de la entrevista
- [ ] Si hay conflicto (alguien cancela su disponibilidad), el sistema detecta y notifica al Recruiter sugiriendo reprogramar

### Notas Técnicas

- **Integraciones de calendario:**
  - Google Calendar API (OAuth 2.0, scopes: calendar.readonly + calendar.events)
  - Microsoft Graph API para Outlook (OAuth 2.0)
- **Availability detection:** FreeBusy query para obtener slots ocupados, calcular slots libres intersectando calendarios de todos
- **Timezone handling:** Usar biblioteca moment-timezone o date-fns-tz, almacenar timestamps en UTC, mostrar en timezone del usuario
- **Video conferencing:**
  - Zoom API para crear meetings programados (requiere Zoom account)
  - Microsoft Teams API para crear Teams meetings
  - Almacenar video_link en tabla INTERVIEW
- **Algoritmo de sugerencia:** Scoring de slots considerando:
  - Preferencia por bloques de mañana (score más alto)
  - Minimizar días de espera (score más alto para fechas más cercanas)
  - Evitar first/last hour del día
  - Buffer time si entrevistadores tienen meetings antes/después
- **Email invitations:** Usar formato iCal (.ics) para compatibilidad universal con clients de email
- **Reminder scheduling:** Job scheduler (node-cron o AWS EventBridge) para enviar emails/notificaciones en tiempos específicos
- **Conflict detection:** Webhook de Google/Outlook para recibir notificaciones de cambios en calendarios, polling cada hora como fallback
- **Retry logic:** Si un slot se llena antes de confirmar, ofrecer inmediatamente siguientes opciones
- **Permisos:** Usuarios deben authorizar acceso a calendario en su perfil (OAuth flow)
- **Multi-timezone:** Si participantes en diferentes zonas horarias, mostrar en timezone de cada uno

### Definición de Hecho (DoD)

- [ ] Integración con Google Calendar funcionando: lectura de disponibilidad + creación de eventos
- [ ] Integración con Microsoft Outlook funcionando: lectura de disponibilidad + creación de eventos
- [ ] Integración con Zoom o Teams para generación de video links
- [ ] API endpoints: POST /interviews/schedule, GET /interviews/{id}, PUT /interviews/{id}/reschedule, DELETE /interviews/{id}
- [ ] Tests unitarios para algoritmo de slot matching con >80% cobertura
- [ ] Tests de integración con calendarios mock y reales en sandbox
- [ ] Tests de manejo de timezones con participantes en diferentes zonas
- [ ] UI implementada: modal de scheduling con selección de tipo y participantes, visualización de slots sugeridos en formato calendario, confirmación
- [ ] Email templates para invitaciones y recordatorios diseñados y testeados
- [ ] Job scheduler configurado para recordatorios automáticos
- [ ] Documentación de OAuth setup y permisos necesarios
- [ ] Logs detallados de todas las operaciones de calendario para debugging
- [ ] Performance testing: scheduling de entrevista completa en <15 segundos
- [ ] Demo exitosa coordinando entrevista real con múltiples participantes
- [ ] Aprobado por Product Owner

### Prioridad: Alta
### Estimación Inicial: XL (3-4 semanas)
### Dependencias:
- Modelo de datos INTERVIEW, INTERVIEW_PARTICIPANT implementado
- Cuentas de desarrollo/sandbox para Google Calendar, Outlook, Zoom/Teams
- OAuth infrastructure para manejo de tokens de usuarios
- Email service operacional
- Scheduler service configurado

---

## US-008: Evaluación Estructurada con Scorecards

**Como** Interviewer
**Quiero** completar un scorecard estructurado después de cada entrevista con ratings por dimensión y comentarios cualitativos
**Para** proporcionar feedback consistente y comparable que ayude al equipo a tomar decisiones de contratación basadas en datos

### Criterios de Aceptación

- [ ] Después de que una entrevista finaliza (status cambia a "Completed"), cada Interviewer recibe una notificación para completar su scorecard
- [ ] El scorecard presenta las dimensiones de evaluación predefinidas según el tipo de entrevista (ej: Technical → "Problem Solving", "Code Quality", "Communication"; Manager → "Culture Fit", "Motivation", "Leadership Potential")
- [ ] Para cada dimensión, el Interviewer asigna un rating en escala de 1-5 estrellas o Likert (Poor/Fair/Good/Very Good/Excellent)
- [ ] El scorecard incluye secciones obligatorias para: comentarios cualitativos generales (min 50 caracteres), concerns específicos (optional), highlights del candidato (optional), y recomendación final (Strong Yes / Yes / Maybe / No / Strong No)
- [ ] El sistema valida que todos los campos obligatorios estén completos antes de permitir submit
- [ ] Al guardar el scorecard como "Draft", el Interviewer puede volver más tarde para completarlo
- [ ] Al submitir el scorecard (status "Submitted"), este se registra en tabla SCORECARD con timestamp y queda visible para Hiring Manager y Recruiters
- [ ] El sistema calcula automáticamente un score promedio ponderado por dimensión para el candidato agregando todos los scorecards
- [ ] Si el scorecard no se completa en 24 horas, el sistema envía recordatorio al Interviewer

### Notas Técnicas

- **Templates de scorecard:** Tabla INTERVIEW_TEMPLATE con campo scorecard_template (JSON) definiendo dimensiones por tipo:
  ```json
  {
    "dimensions": [
      {"name": "Problem Solving", "weight": 0.3, "scale": "1-5"},
      {"name": "Code Quality", "weight": 0.3, "scale": "1-5"},
      {"name": "Communication", "weight": 0.2, "scale": "1-5"},
      {"name": "Culture Fit", "weight": 0.2, "scale": "1-5"}
    ],
    "required_fields": ["qualitative_feedback", "recommendation"]
  }
  ```
- **Modelo de datos:** Tabla SCORECARD con campos interviewer_id, interview_id, ratings (JSON), qualitative_feedback (text), concerns (text), highlights (text), recommendation (enum), status (draft/submitted), submitted_at
- **Validación:** Backend validation que todos los campos required del template estén presentes
- **Peso de dimensiones:** Usar weight para calcular score ponderado agregado
- **Permisos:** Solo participantes del interview pueden crear scorecard para ese interview
- **Notifications:** Trigger automático post-interview para enviar email/in-app notification con link directo al scorecard
- **Reminders:** Job scheduler que chequea scorecards pendientes y envía recordatorios
- **Draft auto-save:** Auto-save cada 30 segundos mientras el usuario escribe (localStorage o API call)
- **Anonymization (opcional):** Opción de anonimizar scorecards durante evaluación para reducir bias (mostrar solo ratings sin nombres)
- **Audit trail:** Registrar created_at, updated_at, submitted_at para tracking de tiempos de respuesta
- **Metrics:** Trackear completion rate y tiempo promedio para completar scorecards

### Definición de Hecho (DoD)

- [ ] Modelo de datos SCORECARD y INTERVIEW_TEMPLATE implementado
- [ ] API endpoints: POST /scorecards (create draft), PUT /scorecards/{id} (update), POST /scorecards/{id}/submit
- [ ] Tests unitarios para validación de campos y cálculo de scores ponderados con >80% cobertura
- [ ] Tests de integración: flujo completo desde interview completed → notification → scorecard submission
- [ ] UI implementada: formulario de scorecard con ratings interactivos (estrellas/slider), text areas, dropdown de recomendación, indicador de progreso, botones save draft/submit
- [ ] Templates predefinidos para mínimo 3 tipos de entrevista (Technical, Manager, Phone Screen)
- [ ] Notifications configuradas: post-interview + reminder 24h
- [ ] Auto-save functionality implementada y testeada
- [ ] Documentación de estructura de templates y guía para crear custom templates
- [ ] Performance testing: rendering de scorecard y submit en <2 segundos
- [ ] Demo con diferentes tipos de scorecards
- [ ] Aprobado por Product Owner

### Prioridad: Alta
### Estimación Inicial: M (1-2 semanas)
### Dependencias:
- Modelo de datos INTERVIEW completo
- Notification Service operacional
- Sistema de permisos validando participantes de interview

---

## US-009: Evaluación Colaborativa en Tiempo Real

**Como** Hiring Manager
**Quiero** discutir candidatos con mi equipo usando comentarios en tiempo real, menciones, y threads de discusión dentro de la plataforma
**Para** centralizar toda la conversación sobre candidatos en un solo lugar y tomar decisiones más rápidas sin depender de emails o Slack

### Criterios de Aceptación

- [ ] Desde la vista de detalle de cualquier candidato (APPLICATION), los usuarios autorizados (Recruiter, HM, Interviewers) pueden agregar comentarios en un thread de discusión
- [ ] El sistema de comentarios soporta @menciones de otros usuarios (ej: "@john ¿qué opinas de las habilidades de React de este candidato?")
- [ ] Cuando un usuario es @mencionado, recibe una notificación inmediata (in-app + email opcional) con link directo al comentario
- [ ] Los comentarios se pueden marcar como "Internal Only" (solo visibles para el equipo) o "Shareable" (pueden incluirse en reports)
- [ ] Los comentarios se muestran en orden cronológico con timestamp, avatar del autor, y nombre
- [ ] Los usuarios pueden editar sus propios comentarios dentro de 15 minutos de publicados (indicador "edited" visible)
- [ ] Los usuarios pueden responder a comentarios específicos creando threads anidados
- [ ] El sistema muestra indicador de "X está escribiendo..." cuando otro usuario está componiendo un comentario en tiempo real
- [ ] Los nuevos comentarios aparecen automáticamente sin necesidad de refresh (WebSockets o polling)
- [ ] El Hiring Manager puede "resolver" threads de discusión cuando la pregunta/issue está cerrado
- [ ] En el dashboard del candidato, se muestra un badge con el número de comentarios no leídos

### Notas Técnicas

- **Real-time communication:** WebSockets (Socket.io o similar) para notificaciones en tiempo real de nuevos comentarios y typing indicators
- **Modelo de datos:** Tabla COMMENT con campos application_id, user_id, content (text), mentions (JSON array de user_ids), is_internal (boolean), parent_comment_id (para threads), is_resolved (boolean), created_at, updated_at, edited (boolean)
- **Menciones parsing:** Regex para detectar @username en texto, reemplazar con links y almacenar IDs en campo mentions
- **Notifications:** Trigger automático cuando se crea comment con mentions → enviar notificación a usuarios mencionados
- **Permisos:** Solo usuarios con acceso al APPLICATION (misma company, asignados al job) pueden ver/crear comentarios
- **WebSocket rooms:** Crear room por application_id, usuarios join al abrir vista de candidato
- **Typing indicator:** Emit evento "typing" via WebSocket con debounce de 2 segundos
- **Polling fallback:** Si WebSocket falla, polling cada 10 segundos para fetch nuevos comentarios
- **Edición:** Permitir UPDATE solo si current_time - created_at < 15min y user_id = comment.user_id
- **Threads anidados:** Usar parent_comment_id para estructura de árbol, limitar a 2 niveles de profundidad
- **Markdown support (opcional):** Permitir formatting básico (bold, italic, links) con librería markdown-it
- **Unread tracking:** Tabla USER_COMMENT_READ (user_id, comment_id, read_at) para tracking de leídos
- **Performance:** Paginar comentarios si >100 para un candidato (cargar los 20 más recientes inicialmente)

### Definición de Hecho (DoD)

- [ ] Modelo de datos COMMENT y USER_COMMENT_READ implementado
- [ ] WebSocket server configurado con rooms por application
- [ ] API endpoints: GET /applications/{id}/comments, POST /applications/{id}/comments, PUT /comments/{id}, POST /comments/{id}/resolve
- [ ] Tests unitarios para parsing de menciones y validaciones con >80% cobertura
- [ ] Tests de integración: flujo completo de crear comment → mention → notification
- [ ] Tests de real-time: verificar que múltiples clientes reciben updates inmediatos
- [ ] UI implementada: thread de comentarios con avatars, timestamps, botón reply, indicador edited, typing indicator, badges de unread
- [ ] Funcionalidad de @mention con autocomplete dropdown de usuarios
- [ ] Notifications in-app y email configuradas para mentions
- [ ] Documentación de arquitectura WebSocket y manejo de reconexión
- [ ] Performance testing: 100 usuarios concurrentes en un application sin latencia >500ms
- [ ] Demo con múltiples usuarios comentando en tiempo real
- [ ] Aprobado por Product Owner

### Prioridad: Media
### Estimación Inicial: L (2-3 semanas)
### Dependencias:
- Modelo de datos APPLICATION base
- WebSocket infrastructure configurada (Socket.io, Redis para pub/sub si multi-server)
- Notification Service operacional
- Sistema de permisos RBAC

---

## US-010: Vista Consolidada de Feedback de Entrevistas

**Como** Hiring Manager
**Quiero** ver todos los scorecards de entrevistas de un candidato agregados en una vista consolidada con ratings promedio, distribución de recomendaciones y comentarios destacados
**Para** tener una visión holística rápida de la evaluación del candidato sin tener que leer cada scorecard individualmente

### Criterios de Aceptación

- [ ] Desde la vista de detalle del candidato, existe una sección "Interview Feedback" que muestra todos los scorecards completados
- [ ] La vista consolidada muestra para cada dimensión evaluada: rating promedio (ej: "Problem Solving: 4.2/5"), ratings individuales por entrevistador con nombres, y gráfico de barras visual
- [ ] La distribución de recomendaciones finales se muestra con un gráfico de torta o barras (ej: "2 Strong Yes, 1 Yes, 0 Maybe, 0 No, 0 Strong No")
- [ ] Los comentarios cualitativos de todos los scorecards se agregan en una sección con filtros para ver "All", "Highlights Only", o "Concerns Only"
- [ ] Los highlights y concerns se muestran agrupados en bullet points con el nombre del entrevistador al lado
- [ ] El sistema detecta y alerta visualmente si hay divergencias significativas en evaluaciones (ej: un "Strong Yes" y un "No" para el mismo candidato)
- [ ] El HM puede comparar visualmente al candidato actual vs otros candidatos del mismo Job mostrando score promedio general
- [ ] La vista incluye un timeline de todas las entrevistas con fechas, tipos, participantes y status de scorecards (completado/pendiente)
- [ ] El HM puede exportar el consolidated feedback a PDF para compartir con stakeholders

### Notas Técnicas

- **Agregación de datos:** Query que join INTERVIEW → SCORECARD para obtener todos los scorecards de un APPLICATION
- **Cálculo de promedios:** Agregar ratings por dimensión usando AVG, considerar weights de dimensions si están definidos
- **Detección de divergencias:** Lógica que compara recommendations: si contiene "Strong Yes" Y ("No" O "Strong No") → flag de divergencia
- **Gráficos:** Usar biblioteca de charting (Chart.js, Recharts, o D3.js) para visualización de barras y tortas
- **Comparación de candidatos:** Query adicional para obtener score promedio de todos los APPLICATION del mismo Job, ordenar, indicar posición del candidato actual
- **Export to PDF:** Usar biblioteca de generación de PDF (Puppeteer para headless Chrome rendering, o jsPDF/PDFKit para server-side)
- **Performance:** Cachear cálculos agregados si el número de scorecards es alto (Redis cache con invalidación al agregar nuevo scorecard)
- **Responsive design:** Vista debe funcionar en desktop y tablet
- **Permisos:** Solo HM, Recruiter y Admin pueden ver vista consolidada
- **Audit trail:** Registrar cada vez que se exporta PDF (quién, cuándo, qué candidato)

### Definición de Hecho (DoD)

- [ ] Query de agregación optimizada con índices apropiados (application_id, interview_id)
- [ ] API endpoint: GET /applications/{id}/consolidated-feedback
- [ ] Tests unitarios para lógica de agregación y detección de divergencias con >80% cobertura
- [ ] Tests de integración con dataset de múltiples scorecards
- [ ] UI implementada: sección de consolidated feedback con gráficos de ratings, distribución de recommendations, highlights/concerns agrupados, timeline de entrevistas, alert visual de divergencias
- [ ] Funcionalidad de comparación con otros candidatos del Job
- [ ] Export to PDF funcional con formato profesional
- [ ] Documentación de estructura de datos y cálculos
- [ ] Performance testing: rendering de vista con 10+ scorecards en <3 segundos
- [ ] Demo mostrando vista consolidada de candidato con múltiples entrevistas
- [ ] Aprobado por Product Owner

### Prioridad: Media
### Estimación Inicial: M (1-2 semanas)
### Dependencias:
- US-008 (Scorecards) - necesita scorecards completos para agregar
- Modelo de datos SCORECARD, INTERVIEW
- Biblioteca de charting seleccionada e integrada
- Biblioteca de PDF generation configurada

---

## US-011: Pipeline Visual Kanban para Gestión de Candidatos

**Como** Recruiter
**Quiero** ver todos los candidatos de un Job organizados en un tablero Kanban visual por etapa del proceso (Applied → Screening → Interview → Offer → Hired)
**Para** tener visibilidad instantánea del estado del pipeline, identificar cuellos de botella, y mover candidatos entre etapas con drag-and-drop

### Criterios de Aceptación

- [ ] Al abrir la vista de un Job, se muestra un tablero Kanban con columnas representando cada etapa del proceso: "Applied", "Screening", "Interview", "Offer", "Hired", "Rejected"
- [ ] Cada columna muestra tarjetas de candidatos (APPLICATION) con información resumida: nombre, foto, Match Score, top 3 skills, tags principales, y días en etapa actual
- [ ] El número total de candidatos en cada columna se muestra en el header de la columna
- [ ] El Recruiter puede arrastrar y soltar tarjetas entre columnas para cambiar el current_stage de un APPLICATION
- [ ] Al mover una tarjeta a nueva etapa, el sistema muestra un modal pidiendo razón opcional del cambio (especialmente para moves a "Rejected")
- [ ] El cambio de etapa se registra automáticamente en STATUS_HISTORY con timestamp, usuario que hizo el cambio, y razón
- [ ] Las tarjetas se pueden filtrar por tags, Match Score range, o búsqueda por nombre
- [ ] Las columnas se pueden colapsar para ahorrar espacio si contienen muchos candidatos
- [ ] El sistema muestra indicadores visuales de alertas: candidatos estancados >7 días en una etapa (badge amarillo), interviews próximos en 24h (badge azul), high-priority candidates (badge verde)
- [ ] El pipeline se actualiza en tiempo real si otro usuario hace cambios (WebSocket o polling)

### Notas Técnicas

- **Librería UI:** Usar biblioteca de drag-and-drop (react-beautiful-dnd, dnd-kit, o SortableJS)
- **Modelo de datos:** Usar campo current_stage de APPLICATION como fuente de verdad, etapas como enum: Applied/Screening/Interview/Offer/Hired/Rejected
- **Queries optimizadas:** Index en (job_id, current_stage) para queries rápidas, eager loading de relationships necesarios (candidate, skills, tags)
- **Real-time updates:** WebSocket room por job_id, emit evento "application_moved" cuando cambia current_stage
- **Responsive design:** Kanban horizontal en desktop, puede cambiar a lista vertical con filtros en mobile
- **Virtualization:** Si columnas tienen >50 candidatos, usar virtual scrolling (react-window) para performance
- **Drag validation:** Backend debe validar que el move es permitido (ej: no se puede mover directo de Applied a Hired sin pasar por etapas intermedias, esto depende de business rules)
- **Status history:** Trigger automático en UPDATE de current_stage para insertar registro en STATUS_HISTORY
- **Modal de razón:** Obligatorio solo para moves a "Rejected", opcional para otros
- **Cálculo de "días en etapa":** current_date - MAX(changed_at) de STATUS_HISTORY para current_stage
- **Alerts lógica:** Query que calcula días en etapa, si >7 → stale_alert, si tiene interview en próximas 24h → upcoming_interview_alert
- **Performance:** Rendering inicial del board completo debe ser <2 segundos para jobs con 200+ candidatos

### Definición de Hecho (DoD)

- [ ] UI Kanban implementada con drag-and-drop funcional
- [ ] API endpoints: GET /jobs/{id}/pipeline, PUT /applications/{id}/stage (con validación y status history)
- [ ] Tests unitarios para lógica de validación de moves y cálculo de días en etapa con >80% cobertura
- [ ] Tests de integración: flujo completo de drag candidato → update stage → registro en history
- [ ] Tests de performance: rendering de board con 200 candidatos en <2s
- [ ] WebSocket configurado para updates en tiempo real
- [ ] Modal de razón implementado con UX fluida
- [ ] Filtros laterales funcionales (tags, score range, name search)
- [ ] Badges de alerts implementados con colores apropiados
- [ ] Documentación de business rules para validación de moves
- [ ] Logs de todos los cambios de etapa para auditoría
- [ ] Demo con Job real mostrando drag-and-drop y real-time updates
- [ ] Aprobado por Product Owner

### Prioridad: Alta
### Estimación Inicial: L (2-3 semanas)
### Dependencias:
- Modelo de datos APPLICATION con campo current_stage y tabla STATUS_HISTORY
- WebSocket infrastructure (si se quiere real-time)
- Librería de drag-and-drop integrada en frontend
- UI components base (cards, badges, modals)

---

## US-012: Dashboard de Métricas de Reclutamiento en Tiempo Real

**Como** Hiring Manager o Admin
**Quiero** ver un dashboard ejecutivo con KPIs clave del proceso de reclutamiento (Time-to-Hire, Cost-per-Hire, funnel conversion, source effectiveness)
**Para** tomar decisiones basadas en datos, identificar cuellos de botella, y medir la efectividad de nuestras estrategias de reclutamiento

### Criterios de Aceptación

- [ ] El dashboard muestra los siguientes KPIs principales con visualizaciones:
  - **Time-to-Hire:** Promedio de días desde Applied hasta Hired (tendencia últimos 3/6/12 meses)
  - **Cost-per-Hire:** Costo total de reclutamiento / número de hires (si se trackean costos)
  - **Offer Acceptance Rate:** % de ofertas aceptadas vs extendidas
  - **Funnel Conversion Rates:** % conversión en cada etapa (Applied→Screening, Screening→Interview, Interview→Offer, Offer→Hired)
  - **Source Effectiveness:** Número de aplicaciones, hires, y quality score por fuente (LinkedIn, Indeed, Referral, etc.)
  - **Pipeline Health:** Número de candidatos activos por etapa, roles con pipeline débil (<5 candidatos activos)
- [ ] Todos los KPIs se pueden filtrar por: rango de fechas, departamento, ubicación, nivel de seniority, y job específico
- [ ] Los gráficos son interactivos: click en barra/segmento muestra drill-down con detalles (ej: click en "LinkedIn" muestra jobs con más applies de LinkedIn)
- [ ] El dashboard se actualiza automáticamente cada 5 minutos o tiene botón de "Refresh Now"
- [ ] Los KPIs muestran comparación vs período anterior (ej: "Time-to-Hire: 18 días, ↓12% vs mes anterior")
- [ ] El usuario puede exportar el dashboard completo a PDF o cada gráfico individual a PNG/CSV
- [ ] Las métricas de diversity (opcional) muestran distribución de género, etnicidad si está disponible (cumpliendo con GDPR)
- [ ] El sistema alerta visualmente si algún KPI está fuera de rango esperado (ej: Time-to-Hire >30 días, Offer Acceptance <70%)

### Notas Técnicas

- **Arquitectura:** Analytics Service separado (Go para high performance) con base de datos optimizada para agregaciones
- **Data pipeline:** ETL job (Airflow, Luigi, o custom) que corre cada hora para pre-calcular métricas y almacenar en tablas agregadas (ANALYTICS_METRICS)
- **Queries optimizadas:**
  - Time-to-Hire: AVG(hired_date - applied_date) WHERE current_stage = 'Hired' GROUP BY month
  - Funnel conversion: COUNT por stage / COUNT stage anterior
  - Source effectiveness: JOIN APPLICATION → JOB_POSTING, GROUP BY platform
- **Visualizaciones:** Biblioteca de charting moderna (Recharts, Chart.js, Apache ECharts)
- **Tipos de gráficos:** Line chart (tendencias), Bar chart (comparaciones), Pie/Donut (distribuciones), Funnel chart (conversión)
- **Real-time updates:** WebSocket para push de nuevas métricas o polling cada 5min
- **Caching:** Redis cache para métricas pre-calculadas (TTL 5min)
- **Export:** PDF usando Puppeteer/headless Chrome, CSV usando bibliotecas de data export
- **Drill-down:** Click handlers en gráficos que hacen queries específicos con filtros aplicados
- **Alerting:** Definir thresholds en config, evaluar en ETL job, emitir notificaciones si se exceden
- **Performance:** Dashboard completo debe cargar en <3 segundos, queries pre-computadas en tablas OLAP
- **Data retention:** Mantener datos históricos mínimo 24 meses para análisis de tendencias
- **Permisos:** Solo roles Admin, Hiring Manager, y Recruiter con permisos especiales pueden ver analytics completos

### Definición de Hecho (DoD)

- [ ] Analytics Service implementado con endpoints para cada KPI
- [ ] ETL pipeline configurado para pre-cálculo de métricas con job scheduler
- [ ] Tablas de agregación (ANALYTICS_METRICS) con índices apropiados
- [ ] API endpoints: GET /analytics/time-to-hire, GET /analytics/funnel, GET /analytics/source-effectiveness, etc.
- [ ] Tests unitarios para lógica de cálculo de métricas con >80% cobertura
- [ ] Tests de integración con dataset histórico
- [ ] UI dashboard implementada con todos los gráficos, filtros interactivos, drill-down
- [ ] Funcionalidad de export a PDF y CSV testeada
- [ ] Sistema de alertas configurado con thresholds sensatos
- [ ] Documentación de fórmulas de cálculo de cada KPI
- [ ] Performance testing: dashboard completo carga en <3s con 10k+ applications
- [ ] Caching configurado y validado
- [ ] Demo mostrando dashboard completo con datos reales y drill-downs
- [ ] Aprobado por Product Owner

### Prioridad: Media
### Estimación Inicial: XL (3-4 semanas)
### Dependencias:
- Modelo de datos completo (JOB, APPLICATION, STATUS_HISTORY, JOB_POSTING)
- Dataset histórico suficiente para métricas significativas (mínimo 3 meses de datos)
- Analytics Service infrastructure
- ETL/pipeline infrastructure (Airflow o similar)
- Biblioteca de charting integrada en frontend

---

## Resumen de Priorización

### Fase 1 - MVP (Alta Prioridad)
1. **US-001** - Parseo de CV (foundational)
2. **US-005** - Screening Automatizado (core value prop)
3. **US-011** - Pipeline Kanban (core UX)
4. **US-003** - Publicación Multi-Canal (acquisition)
5. **US-007** - Scheduling Automático (time-saving)
6. **US-008** - Scorecards (evaluation)
7. **US-002** - Búsqueda Semántica (talent reuse)

### Fase 2 - Maduración (Media Prioridad)
8. **US-006** - Auto-Tagging (efficiency)
9. **US-004** - Aprobación de Jobs (process)
10. **US-009** - Colaboración Real-Time (teamwork)
11. **US-010** - Feedback Consolidado (decision quality)
12. **US-012** - Dashboard Analytics (insights)

---

## Notas Finales

**Convenciones INVEST aplicadas:**
- ✅ **Independent:** Cada US puede desarrollarse en paralelo (con dependencias documentadas)
- ✅ **Negotiable:** Criterios de aceptación específicos pero implementación flexible
- ✅ **Valuable:** Cada US entrega valor claro a un rol de usuario específico
- ✅ **Estimable:** T-shirt sizing basado en complejidad técnica
- ✅ **Small:** La mayoría es M o L (1-3 semanas), XL solo para features muy complejas
- ✅ **Testeable:** Criterios de aceptación medibles y testables

**Próximos Pasos:**
1. Validar priorización con stakeholders
2. Refinar estimaciones con equipo técnico
3. Crear tickets en Jira/Linear con estas User Stories
4. Planificar sprints según capacidad del equipo
5. Establecer métricas de éxito para cada US

---

**Documento generado:** 2025-01-09
**Versión:** 1.0
**Aprobación pendiente:** Product Owner