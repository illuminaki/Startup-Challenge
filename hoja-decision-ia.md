# Hoja de decisión: ¿Modelo base, Fine-Tuning o RAG?

## Objetivo
Guiar, a nivel teórico y de forma simple, la selección entre usar un modelo base, aplicar RAG (Retrieval-Augmented Generation) o hacer fine-tuning, considerando datos, costos, tiempo, mantenimiento, riesgos y calidad esperada.

---

## Resumen
- Modelo base: adecuado cuando el tema es general, la exactitud “suficiente” alcanza y se busca rapidez y bajo costo.
- RAG: útil si tienes documentos propios (manuales, políticas, FAQs) y necesitas respuestas con base en esas fuentes, actualizables y con posibilidad de citar.
- Fine-Tuning: útil si se requiere un estilo muy específico o desempeño consistente en tareas repetitivas, y cuentas con buenos ejemplos etiquetados.

Regla simple: empieza con modelo base. Si falta conocimiento propio, añade RAG. Si aún necesitas estilo/estructura muy específica o clasificación a medida, considera fine-tuning.

---

## Criterios de decisión

### 1) Naturaleza del conocimiento requerido
- General/público → Modelo base
- Propietario y cambiante (manuales, políticas, FAQs, tickets) → RAG
- Propietario, estable, y necesitas “hablar como la empresa” → Fine-Tuning

### 2) Tipo de tarea
- Búsqueda-respuesta con citas, QA sobre documentos → RAG
- Clasificación/etiquetado, extracción con formatos fijos → Fine-Tuning
- Redacción general, brainstorming, traducciones no críticas → Modelo base

### 3) Datos disponibles
- Pocos/no curados → Modelo base o RAG (usando tus fuentes)
- Muchos ejemplos etiquetados de alta calidad (≥500–1,000, idealmente más) → Fine-Tuning
- Corpus de documentos grandes y actualizables → RAG

### 4) Exactitud y verificabilidad
- Necesitas trazabilidad y citas a fuentes → RAG
- Necesitas consistencia estilística/estructura estricta → Fine-Tuning
- Tolerancia a pequeñas imprecisiones → Modelo base

### 5) Mantenimiento y actualización
- Contenido cambia con frecuencia → RAG (actualizas el índice, no el modelo)
- Contenido estable a largo plazo → Fine-Tuning posible
- Sin mantenimiento significativo → Modelo base

### 6) Costos y tiempo
- Implementación más rápida/low-cost → Modelo base
- Coste moderado con buen retorno en soporte/knowledge → RAG
- Coste más alto (dataset, entrenamiento, validación) → Fine-Tuning

### 7) Riesgos
- Riesgo de alucinaciones inaceptable → RAG con grounding y citas
- Riesgo de sobreajuste o fuga de datos → Cuidar Fine-Tuning y gobernanza
- Riesgo bajo/POC → Modelo base

---

## Árbol de decisión rápido
1. ¿Tu caso depende de información propia/documentos? 
   - Sí → ¿Esa info cambia seguido? 
     - Sí → RAG
     - No → ¿Requieres estilo/formatos muy específicos? 
       - Sí → Fine-Tuning (+ opcional RAG)
       - No → RAG
   - No → ¿Exactitud “buena” es suficiente sin trazabilidad? 
     - Sí → Modelo base
     - No → ¿Tienes dataset etiquetado amplio y de calidad? 
       - Sí → Fine-Tuning
       - No → Primero RAG (si aplica) o mejora prompting/recopila datos

---

## Guía rápida de puntuación (simple)
Para cada criterio clave (conocimiento propio, necesidad de citas, estilo específico, datos etiquetados, rapidez), puntúa 1–3 por enfoque (1=bajo, 3=alto). Suma y compara: el mayor puntaje indica la estrategia inicial. Si hay empate, empieza por modelo base o RAG según si hay documentos propios.

---

## Métricas y evaluación (teóricas)
- RAG: ¿La respuesta usa información correcta de tus documentos? ¿Incluye referencia/cita?
- Fine-Tuning: ¿Clasifica o genera con el formato/estilo esperado de forma consistente?
- Modelo base: ¿La respuesta es útil y comprensible para el usuario?

Sugerencias: define ejemplos de prueba sencillos, revisa con un compañero y toma nota de errores frecuentes.

---

## Orientación teórica (sin implementación técnica)

Modelo base:
- Empieza con instrucciones claras y ejemplos breves.
- Asegúrate de revisar y corregir salidas manualmente al principio.

RAG:
- Reúne tus documentos clave y asegúrate de que estén actualizados y organizados.
- Verifica que las respuestas puedan señalar de qué documento proviene la información.

Fine-Tuning:
- Define la tarea con claridad (por ejemplo: clasificar en 5 categorías, o generar un correo con un formato específico).
- Junta ejemplos de calidad que muestren exactamente lo que esperas.

---

## Ejemplos rápidos
- Soporte interno con base de conocimiento viva → RAG
- Generación de emails con voz de marca muy consistente → Fine-Tuning
- Asistente general para ideación y borradores → Modelo base
- Clasificador de tickets por categorías propias → Fine-Tuning
- Buscador semántico de políticas y normativas → RAG

---

## Checklist antes de decidir
- ¿Cuánta información propia necesito y con qué frecuencia cambia?
- ¿Necesito citas y trazabilidad?
- ¿Dispongo de dataset etiquetado suficiente y de calidad para FT?
- ¿Cuál es el presupuesto y el time-to-value esperado?
- ¿Qué métricas de éxito usaré y cómo monitorearé en producción?
- ¿Hay riesgos de privacidad, compliance o fuga de datos?

---

## Riesgos y mitigaciones
- Alucinaciones: RAG con grounding y verificación post-gen; validación de salidas.
- Privacidad/PII: anonimización, política de acceso, cifrado en reposo y tránsito.
- Sesgo: auditorías de dataset (FT), evaluación por subgrupos, revisiones humanas.
- Vendor lock-in: abstraer proveedor, formatos portables para índices y datasets.

---

## Cómo usar esta hoja en el challenge
1) Define tu caso de uso en 3–5 líneas.
2) Recorre el árbol de decisión y la matriz.
3) Elige estrategia inicial (modelo base → RAG → fine-tuning si hace falta).
4) Documenta métricas y un plan mínimo de implementación.

---

## Glosario breve
- RAG: Recuperar documentos relevantes y usarlos para guiar la generación.
- Fine-Tuning: Ajustar los parámetros del modelo con ejemplos de tu tarea.
- Embeddings: Representaciones vectoriales para búsqueda semántica.
- Reranking: Reordenar resultados recuperados con un modelo más preciso.
