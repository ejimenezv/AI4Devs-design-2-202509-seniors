# Prompts Utilizados para el Proyecto LTI-EJV

## Metaprompting

Sigo asiendo uso de metaprompting ya que me ha dado buenos resultados. Usé el siguiente prompt cen Claude.ai, adjuntando los documentos 🎥 Primeros pasos en un proyecto de software.docx y ReadMe.md.

Claude me proporcionó el Prompt principal que usé para desarrollar la actividad.

Es importante mencionar que tanto en Claude.ai como en Claude Code usé el modo de pensamiento extendido, y en Claude Code me asuguré de usar el modo planeación.

```

Estoy en un curso de AI4Devs. Acabo de estudiar un módulo sobre planificación y documentación efectivas con IA. Te adjunto el documento con la información del módulo. 

Ahora debo realizar una actividad, la cual adjunto en el archivo ReadMe.md

Como experto en Inteligencia artificial con más de 10 años de experiencia, me apoyaras a desarrollar un prompt o una serie de prompts que introduciré en Cluade Code. Considera que estará corriendo en Cursor con el proyecto clonado y las instrucciones en ReadMe.md.

Detalles:
* Puedes desarrollar un solo prompt o dividir la tarea en múltiples prompts.
* Estructura bien los prompts con Rol, Tarea, Conteto, Detalles y cualquier otra sección que sea importante para entregar el mejor resultado
* Los diagramas deben ser desarrollados en algún lenguaje de diagramación como PlantUML o mermaid. Claude Code debe proporcionar instrucciones para exportar los diagramas e incluirlos en el markdown final.
* El prompt debe general el archivo .md solicitado, así como una carpeta con los diagramas.
* Se debe cubrir todos los requisitos de ReadMe.md
* Asegúrate que los prompts estén optimizados para Claude Code corriendo en Cursor.

```


## Prompt Principal

```
# ROL
Eres un Product Manager y Arquitecto de Software senior con más de 15 años de experiencia diseñando sistemas empresariales, especialmente en el sector de HR Tech y ATS (Applicant-Tracking Systems). Tienes experiencia profunda en:
- Análisis de mercado y definición de productos
- Diseño de sistemas escalables
- Modelado de datos y arquitectura de software
- Metodologías Lean y Canvas Business Model
- Diagramación con Mermaid y PlantUML
- Documentación técnica de alto nivel

# CONTEXTO
LTI es una startup que quiere desarrollar el ATS (Applicant-Tracking System) del futuro. Actualmente no existe nada creado, y necesitamos diseñar desde cero un sistema que destaque por:
- Aumentar la eficiencia para departamentos de HR
- Mejorar la colaboración en tiempo real entre reclutadores y managers
- Automatizaciones inteligentes
- Asistencia de IA en diversas tareas

Este es un proyecto académico para el curso AI4Devs, módulo de planificación y documentación con IA.

# TAREA PRINCIPAL
Debes crear una documentación completa del sistema LTI que incluya todos los artefactos solicitados. La entrega debe cumplir con estos requisitos específicos:

## Estructura de Archivos a Crear:
```
LTI-{iniciales}/
├── LTI-{iniciales}.md          # Documento principal
├── prompts.md                  # Este prompt y cualquier refinamiento usado
└── diagrams/                   # Carpeta con todos los diagramas
    ├── lean-canvas.mmd
    ├── use-case-1.mmd
    ├── use-case-2.mmd
    ├── use-case-3.mmd
    ├── data-model.mmd
    ├── high-level-architecture.mmd
    └── c4-component.mmd
```

## Contenido del Documento Principal (LTI-{iniciales}.md):

### 1. DESCRIPCIÓN DEL SOFTWARE LTI
- Descripción breve y compelling del producto (2-3 párrafos)
- Propuesta de valor única
- Ventajas competitivas frente a ATS tradicionales (Greenhouse, Lever, Workday)
- Diferenciadores clave enfocados en IA y colaboración

### 2. FUNCIONES PRINCIPALES
Describe 8-10 funcionalidades core del sistema, organizadas en categorías:
- Gestión de candidatos
- Colaboración y comunicación
- Automatización con IA
- Analytics e informes
- Integraciones

Para cada función, incluye: nombre, descripción (2-3 líneas), beneficio principal

### 3. LEAN CANVAS
- Crear un diagrama Lean Canvas completo usando Mermaid
- Incluir los 9 bloques: Problema, Segmentos de clientes, Propuesta única de valor, Solución, Canales, Flujos de ingresos, Estructura de costes, Métricas clave, Ventaja especial
- El diagrama debe ser visualmente claro y profesional
- Incluir explicación de cada bloque (1-2 párrafos por bloque)

### 4. CASOS DE USO PRINCIPALES
Debes definir exactamente 3 casos de uso principales que representen el valor core del sistema:

**Para cada caso de uso proporciona:**
- Nombre descriptivo
- Actor(es) involucrado(s)
- Precondiciones
- Flujo principal (paso a paso, 5-8 pasos)
- Flujos alternativos (2-3 escenarios)
- Postcondiciones
- Diagrama de caso de uso en Mermaid (usando sintaxis de UML)

**Sugerencia de casos de uso (puedes ajustar):**
1. Publicación y distribución automática de ofertas
2. Screening de candidatos con IA
3. Proceso colaborativo de entrevistas

### 5. MODELO DE DATOS
- Crear un diagrama de entidad-relación completo en Mermaid
- Incluir mínimo 8-10 entidades principales
- Para cada entidad especificar:
  - Nombre de la entidad
  - Atributos con nombre y tipo de dato (ej: nombre: VARCHAR, fecha_creacion: TIMESTAMP)
  - Claves primarias y foráneas
- Mostrar todas las relaciones con cardinalidad (1:1, 1:N, N:M)
- Incluir entidades como: Job, Candidate, Application, Interview, User, Company, etc.

Después del diagrama, incluir una tabla explicativa de cada entidad:
```
| Entidad | Descripción | Atributos Clave |
```

### 6. DISEÑO DE ALTO NIVEL
- Descripción textual de la arquitectura (3-4 párrafos)
- Explicar el estilo arquitectónico elegido (ej: microservicios, monolito modular)
- Justificar decisiones de diseño
- Describir capas/componentes principales:
  - Frontend (tecnología sugerida)
  - Backend/API (tecnología sugerida)
  - Base de datos (tecnología sugerida)
  - Servicios de IA
  - Integraciones externas

- Diagrama de arquitectura de alto nivel en Mermaid mostrando:
  - Componentes principales
  - Flujo de datos
  - Integraciones externas
  - Usuarios/actores

### 7. DIAGRAMA C4 - COMPONENTE ESPECÍFICO
Elige UNO de estos componentes para profundizar con modelo C4:
- Sistema de IA para screening
- Motor de notificaciones y comunicaciones
- Sistema de gestión de entrevistas

Crear un diagrama C4 a nivel de Componentes que muestre:
- Contenedores del sistema
- Componentes internos del contenedor elegido
- Relaciones entre componentes
- Responsabilidades de cada componente
- Usar sintaxis C4 en Mermaid

Incluir descripción textual explicando:
- Por qué elegiste ese componente
- Responsabilidades de cada subcomponente
- Flujos de datos principales
- Tecnologías propuestas

# REQUISITOS TÉCNICOS DE LOS DIAGRAMAS

## Para Mermaid:
- Usa sintaxis actualizada de Mermaid
- Todos los diagramas deben ser funcionales y renderizables
- Incluye títulos claros
- Usa colores cuando sea apropiado para mejorar la comprensión
- Asegúrate que el texto sea legible

## Tipos de diagramas a usar:
- **Lean Canvas**: Usa `graph` o `mindmap`
- **Casos de uso**: Usa `graph TD` con formas UML
- **Modelo de datos**: Usa `erDiagram`
- **Arquitectura alto nivel**: Usa `graph TB` o `C4Context`
- **C4 Componentes**: Usa sintaxis C4 de Mermaid

## Instrucciones de exportación:
Después de crear cada diagrama, incluye en el documento markdown:
```
Para visualizar este diagrama:
1. Copia el código del bloque mermaid
2. Pégalo en https://mermaid.live
3. Exporta como PNG o SVG
4. Alternativamente, usa una extensión de Mermaid en VS Code
```

# FORMATO DEL DOCUMENTO

El documento LTI-{iniciales}.md debe seguir esta estructura:
```markdown
# LTI - Applicant-Tracking System del Futuro

> Sistema ATS de nueva generación potenciado por IA

## Tabla de Contenidos
[Genera tabla de contenidos automática]

## 1. Descripción del Producto
[Contenido aquí]

## 2. Funcionalidades Principales
[Contenido aquí]

## 3. Lean Canvas
[Diagrama + explicaciones]

## 4. Casos de Uso Principales

### 4.1 [Caso de uso 1]
[Contenido + diagrama]

### 4.2 [Caso de uso 2]
[Contenido + diagrama]

### 4.3 [Caso de uso 3]
[Contenido + diagrama]

## 5. Modelo de Datos
[Diagrama + tabla explicativa]

## 6. Diseño de Alto Nivel
[Descripción + diagrama]

## 7. Diagrama C4 - [Componente elegido]
[Descripción + diagrama C4]

## 8. Conclusiones y Próximos Pasos
[Resumen ejecutivo y roadmap sugerido]
```

# CRITERIOS DE CALIDAD

El documento debe:
✅ Ser profesional y listo para presentar a stakeholders
✅ Tener coherencia entre todas las secciones
✅ Mostrar pensamiento estratégico en decisiones de producto
✅ Incluir justificaciones de diseño
✅ Tener todos los diagramas funcionales y renderizables
✅ Usar terminología técnica apropiada
✅ Ser comprehensivo pero conciso
✅ Estar en español profesional

# INSTRUCCIONES DE EJECUCIÓN

1. **Primero**: Crea la estructura de carpetas especificada
2. **Segundo**: Genera el documento markdown completo con todo el contenido
3. **Tercero**: Crea cada archivo de diagrama en la carpeta `diagrams/`
4. **Cuarto**: Verifica que todos los diagramas sean sintácticamente correctos
5. **Quinto**: Crea el archivo `prompts.md` con este prompt
6. **Sexto**: Muéstrame un resumen de los archivos creados

**IMPORTANTE**:
- Reemplaza {iniciales} con las iniciales que el usuario te proporcione
- Si no se proporcionan iniciales, usa "XXX" como placeholder
- Asegúrate de que todos los archivos se creen en la carpeta correcta
- Todos los diagramas deben estar en archivos separados .mmd
- El documento principal debe referenciar los diagramas con includes de markdown

# PREGUNTA INICIAL
Antes de empezar, dime tus iniciales para personalizar los nombres de archivos (ej: ARM, JLS, etc.)
```
