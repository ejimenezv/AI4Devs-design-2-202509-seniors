# Metaprompting

En preparación para desarrollar la actividad, usé Claude.ai para generar los prompts a usar dentro en Claude Code dentro de Cursor.

PROMPT:

---

Como Project Manager Sr con más de 10 años de experiencia, me apoyarás a desarrollar la siguiente actividad como parte del programa AI4Devs del que estoy participando.

Requiero me generes un prompt para completar cada paso solicitado: 1) Generar User Stories, 2) Armar el Backlog de producto, 3) Elegir una user story (dame la capacidad de elegir) y generar los tickets de trabajo y 4) Estimar el esfuerzo de los tickets.

Considera que los prompts serán ejecutados pro Claude Code dentro de Cursor. En el proyecto se tiene una carpeta LTI-EJV dentro de la cual se ejecutará el código. Existe una carpeta PRD que tiene el entregable de la activdad anterior, donde se desarrolló un PRD básico para el proyecto a desarrollar. Los archivos relevantes dentro de la carpeta PRD son LTI-EJV.md (archivo del PRD) y la carpeta diagrams, que contiene diagramas adjuntos al PRD. Ignora los archivos prompts.md y promts-CLAUDE.md. 

Actividad:

En este ejercicio vas a actuar como un Product Manager y Business Analyst. 

Usando los documentos que generaste en la sección anterior y que conforman un PRD básico (funcionalidades clave, casos de uso, modelo de datos...), tu misión es preparar la documentación necesaria para empezar a implementar LTI:

1. Generar las User Stories. Puedes implementar tantas como quieras y puedas, el mínimo son 2. Utiliza lo aprendido sobre buenas prácticas de este capítulo para que contenga toda la información necesaria, y como consejo, usa una plantilla común para todas ellas (recuerda que dejamos un ejemplo de plantilla en la sección de User Stories).
2. Arma el Backlog de producto con las User Stories, priorizándolas como consideres conveniente acorde a alguna metodología concreta. experimenta con diferentes formas de generar un prompt que te pueda genera tu back log basado en la documentación que has generado previamente. Entrega los diferentes prompts que usaste e indica cual prompt te dio mejores resultados. Entrega junto a los prompts tus conclusiones, por qué crees este prompt fue efectivo. 
3. Elige la User Story que prefieras, y genera los Tickets de trabajo. Aterrízalos técnicamente, tal y como se hace en las reuniones de planificación
4. (Extra 🎁) Estima el esfuerzo de los tickets de trabajo usando la metodología (fibonacci, poker, tallas de camiseta) y unidades (horas, puntos de historia) que prefieras.
Utiliza el asistente que prefieras: ChatGPT, Google Gemini, Microsoft Copilot, Claude...

No olvides revisar lo que te devuelve el asistente y retocarlo para adaptarlo a tus necesidades, corrigiendo o incluso borrando lo que consideres adecuado. 

Documenta todo en un único documento markdown (.md) con el nombre UserStories-iniciales, y déjalo en la carpeta LTI-iniciales (ya la creaste en el ejercicio del módulo anterior si realizaste la entrega), en el repositorio Github de este tema:

AI4Devs-design-2-202509-seniors

El repositorio será colaborativo, iremos aceptando las pull requests para generar una base común con todas las carpetas. 

Recuerda actualizar a la última versión del repositorio antes de lanzar tus cambios para no tener conflictos.

Si no sabes cómo mantenerte actualizado antes de publicar tu contenido y encontrarte con conflictos, pregunta en el grupo de Whatsapp o revisa documentación sobre git.

Por último, no olvides añadir tus prompts en prompts.md dentro de tu carpeta. 


¡A por ello! 

---

# PROMPT 1: Generar User Stories

---

Actúa como un Product Manager experimentado y Business Analyst especializado en sistemas ATS.

CONTEXTO:
- Lee y analiza el archivo PRD/LTI-EJV.md y todos los diagramas en PRD/diagrams/
- El proyecto es un Applicant Tracking System (ATS) llamado LTI
- Necesito generar User Stories completas y bien estructuradas

TAREA:
1. Analiza el PRD completo incluyendo funcionalidades clave, casos de uso y modelo de datos
2. Genera un MÍNIMO de 8 User Stories que cubran las funcionalidades principales del ATS
3. Cada User Story debe seguir esta plantilla estricta:

---
## US-[ID]: [Título descriptivo]

**Como** [tipo de usuario]  
**Quiero** [acción/funcionalidad]  
**Para** [beneficio/valor]

### Criterios de Aceptación
- [ ] [Criterio específico y medible]
- [ ] [Criterio específico y medible]
- [ ] [Criterio específico y medible]

### Notas Técnicas
- [Consideraciones de implementación]
- [Dependencias técnicas]
- [Requisitos de rendimiento si aplica]

### Definición de Hecho (DoD)
- [ ] Código implementado y revisado
- [ ] Tests unitarios y de integración pasando
- [ ] Documentación técnica actualizada
- [ ] Aprobado por el Product Owner

### Prioridad: [Alta/Media/Baja]
### Estimación Inicial: [T-Shirt Size: XS/S/M/L/XL]
### Dependencias: [Lista de US de las que depende]
---

PRIORIZA User Stories que cubran:
- Gestión de candidatos (CRUD básico)
- Proceso de aplicación
- Gestión de vacantes
- Panel de reclutador/hiring manager
- Flujo de entrevistas
- Sistema de permisos y roles

ENTREGABLE:
Crea el archivo LTI-EJV/UserStories-EJV.md con todas las User Stories generadas, organizadas por área funcional. Incluye una sección inicial con:
- Índice de User Stories
- Leyenda de prioridades
- Convenciones utilizadas

Asegúrate de que cada User Story sea independiente, valiosa, estimable, pequeña y testeable (INVEST).

---

# PROMPT 2: Armar el Backlog de Producto

---

Actúa como un Product Manager Senior especializado en priorización de backlog usando metodologías ágiles.

CONTEXTO:
- Ya generaste User Stories en el archivo LTI-EJV/UserStories-EJV.md
- Necesito crear un Product Backlog priorizado usando el framework MoSCoW y Value vs Effort

TAREA:
1. Lee todas las User Stories del archivo UserStories-EJV.md
2. Analiza cada US considerando:
   - Valor de negocio (impacto en usuarios, ROI, diferenciación competitiva)
   - Esfuerzo técnico (complejidad, dependencias, riesgos)
   - Dependencias entre User Stories
   - Viabilidad técnica

3. Crea una matriz de priorización con 3 enfoques diferentes:

**ENFOQUE 1: MoSCoW**
- Must Have (Crítico para MVP)
- Should Have (Importante pero no bloqueante)
- Could Have (Deseable si hay tiempo)
- Won't Have (Fuera de scope actual)

**ENFOQUE 2: Value vs Effort Matrix**
- Quick Wins (Alto valor, bajo esfuerzo) → Prioridad 1
- Big Bets (Alto valor, alto esfuerzo) → Prioridad 2
- Fill-Ins (Bajo valor, bajo esfuerzo) → Prioridad 3
- Time Sinks (Bajo valor, alto esfuerzo) → Prioridad 4

**ENFOQUE 3: Weighted Shortest Job First (WSJF)**
Calcula: (Business Value + Time Criticality + Risk Reduction) / Job Size
Para cada US asigna valores 1-10 a cada factor

4. Genera el backlog final recomendado combinando los 3 enfoques, justificando la priorización

ENTREGABLE:
Añade al archivo LTI-EJV/UserStories-EJV.md una nueva sección al final del documento:

# Product Backlog - LTI ATS

## Metodología de Priorización
[Explicación breve de cómo priorizaste]

## Análisis de Priorización

### Enfoque 1: MoSCoW
[Tabla con todas las US clasificadas]

### Enfoque 2: Value vs Effort Matrix
[Tabla con todas las US clasificadas]

### Enfoque 3: WSJF
[Tabla con scores y ranking]

## Backlog Priorizado Final
[Tabla consolidada con orden final, justificación, sprint sugerido]

## Roadmap de Sprints Sugerido
- Sprint 1 (MVP Core): [Lista de US]
- Sprint 2 (Essential Features): [Lista de US]
- Sprint 3 (Enhanced UX): [Lista de US]
- Backlog Futuro: [Lista de US]

Incluye una tabla comparativa mostrando cómo cada metodología impactó la priorización final.

---

# PROMPT 3: Generar Tickets de Trabajo para una User Story

Actúa como un Tech Lead y Scrum Master experimentado en descomposición técnica de User Stories.

CONTEXTO:
- Tienes User Stories priorizadas en LTI-EJV/UserStories-EJV.md
- Necesito descomponer UNA User Story específica en tickets técnicos detallados
- Stack técnico: React 19.2, Node.js con TypeScript, MySQL, Docker

MODO INTERACTIVO:
1. Lee el archivo UserStories-EJV.md
2. Muéstrame SOLO la lista numerada de User Stories con su título y prioridad
3. ESPERA a que yo elija un número
4. Una vez que elija, procede con la descomposición técnica

TAREA (después de mi elección):
Descompone la User Story seleccionada en tickets técnicos siguiendo esta estructura:

Para cada ticket genera:

---
### TICKET [US-ID]-[Número]: [Título técnico]

**Tipo**: [Feature/Bug/Technical/Refactor]  
**Componente**: [Frontend/Backend/Database/DevOps]  
**Asignado a**: [Frontend Dev/Backend Dev/FullStack Dev/DevOps]

#### Descripción
[Descripción técnica detallada de qué hay que implementar]

#### Tareas Técnicas
- [ ] [Tarea específica 1]
- [ ] [Tarea específica 2]
- [ ] [Tarea específica 3]

#### Criterios de Aceptación Técnicos
- [ ] [Criterio técnico verificable]
- [ ] [Tests específicos que deben pasar]
- [ ] [Métricas de rendimiento si aplica]

#### Dependencias
- Depende de: [Tickets previos necesarios]
- Bloquea a: [Tickets que dependen de este]

#### Definición Técnica de Hecho
- [ ] Código cumple con guías de estilo (ESLint/Prettier)
- [ ] Code review aprobado
- [ ] Tests unitarios con cobertura >80%
- [ ] Tests de integración pasando
- [ ] Documentación de API/componentes
- [ ] Sin vulnerabilidades de seguridad
- [ ] Deploy en ambiente de desarrollo exitoso

#### Notas de Implementación
- [Consideraciones técnicas específicas]
- [Patterns o bibliotecas recomendadas]
- [Edge cases a considerar]

#### Artefactos
- [ ] [Archivos/componentes a crear o modificar]
---

CONSIDERACIONES:
- Separa frontend, backend, database schema, tests, y devops en tickets diferentes
- Identifica dependencias entre tickets
- Incluye tickets para tests E2E si es necesario
- Considera tickets de configuración (Docker, ENV vars, CI/CD)
- Ordena los tickets en secuencia lógica de implementación

ENTREGABLE:
Crea un archivo nuevo: LTI-EJV/tickets/US-[ID]-tickets.md con:
1. Resumen de la User Story original
2. Arquitectura técnica de la solución (diagrama en Mermaid)
3. Todos los tickets descompuestos
4. Diagrama de dependencias entre tickets
5. Orden de implementación sugerido
6. Riesgos técnicos identificados

# PROMPT 4: Estimar Esfuerzo de los Tickets

---

Actúa como un Scrum Master y Tech Lead experto en estimación ágil con más de 10 años de experiencia.

CONTEXTO:
- Tienes tickets técnicos generados en LTI-EJV/tickets/US-[ID]-tickets.md
- Stack: React 19.2, Node.js TypeScript, MySQL, Docker
- Team: 3 desarrolladores full-stack, 1 senior, 2 mid-level
- Velocidad de sprint: ~40 story points (2 semanas)

TAREA:
1. Lee todos los tickets del archivo de tickets generado
2. Estima cada ticket usando 3 metodologías diferentes:

**METODOLOGÍA 1: Story Points (Fibonacci)**
Escala: 1, 2, 3, 5, 8, 13, 21
Referencia: 
- 1 punto = ~2-4 horas (cambio trivial, sin incertidumbre)
- 3 puntos = ~1 día (implementación estándar)
- 5 puntos = ~2-3 días (complejidad media, algunos unknowns)
- 8 puntos = ~1 semana (alta complejidad o muchas dependencias)
- 13+ puntos = Considerar subdividir

Considera:
- Complejidad técnica
- Incertidumbre/riesgos
- Esfuerzo de testing
- Deuda técnica generada

**METODOLOGÍA 2: T-Shirt Sizing**
- XS: < 4 horas (trivial)
- S: 4-8 horas (simple)
- M: 1-2 días (estándar)
- L: 3-5 días (complejo)
- XL: 1-2 semanas (muy complejo, considerar subdividir)

**METODOLOGÍA 3: Horas Ideales**
Estima en horas de trabajo real sin interrupciones
Luego aplica factor de realidad x1.5-2 (meetings, interrupciones, code review)

3. Para cada ticket, proporciona:
   - Estimación en las 3 metodologías
   - Justificación de la estimación
   - Factores de riesgo que afectan la estimación
   - Nivel de confianza (Alto/Medio/Bajo)

4. Genera un análisis de capacidad de sprint:
   - Total de story points estimado
   - Distribución por tipo de ticket
   - Identificación de cuello de botella
   - Sugerencia de tickets para Sprint 1

ENTREGABLE:
Actualiza el archivo LTI-EJV/tickets/US-[ID]-tickets.md añadiendo:

## Estimaciones de Esfuerzo

### Metodología y Referencias
[Explicación de las escalas usadas y referencias de calibración]

### Tabla de Estimaciones Consolidada
| Ticket ID | Título | Story Points | T-Shirt | Horas Ideales | Horas Reales | Confianza | Riesgos |
|-----------|--------|--------------|---------|---------------|--------------|-----------|---------|
| [ID] | [Título] | [Fibonacci] | [XS-XL] | [horas] | [horas x factor] | [Alto/Medio/Bajo] | [Lista] |

### Análisis por Ticket
Para cada ticket incluye:
#### [Ticket ID]: [Título]
- **Story Points**: X (Justificación)
- **T-Shirt Size**: X (Justificación)
- **Horas Estimadas**: X ideales → Y reales
- **Factores de Complejidad**: [Lista]
- **Riesgos de Estimación**: [Lista]
- **Supuestos**: [Lista]
- **Nivel de Confianza**: [Alto/Medio/Bajo] porque [razón]

### Resumen de Capacidad
- **Total Story Points**: X
- **Total Horas Estimadas**: Y horas reales
- **Duración Estimada**: Z días/sprints con team de 3 devs
- **Critical Path**: [Secuencia de tickets en ruta crítica]
- **Recomendación**: [Sprint planning basado en velocidad del team]

### Análisis de Riesgos de Estimación
[Identifica tickets con alta incertidumbre y propón estrategias de mitigación]

### Planning Poker Simulation
[Si aplica, simula una sesión mostrando diferentes perspectivas: junior vs senior developer]

Incluye recomendaciones sobre cuándo re-estimar si hay mucha incertidumbre.

---