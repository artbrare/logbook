# Logbook — Tech Lead Path

Registro de ejecución de un programa de ingeniería de 26 semanas
(27 jul 2026 → 24 ene 2027).

## Qué NO se hace aquí
Aquí no se escribe código de producción. Este repo es documentación.
El código vive en el repo de cada proyecto (activo: `loop`).

## Dónde vive cada cosa
- `semanas/`    doc semanal, numeración ISO. Fuente de verdad de la ejecución.
- `epicas/`     un doc por tarea grande. Ver umbral abajo.
- `adr/`        decisiones sobre el PROGRAMA (rituales, alcance, arquitectura de docs).
- `templates/`  plantillas de doc semanal y de épica.

## Reparto de ADRs
- Decisión sobre el producto (stack, modelo, hosting) → `loop/docs/adr/`
- Decisión sobre el programa (rituales, alcance) → aquí, en `adr/`

## Convenciones
- Marcado diario: `- [ ]` pendiente → `- [x]` hecho. Sin estados intermedios.
- Moneda del programa: el pomodoro. 1 pom = 25 min.
- Estados de épica: 🔲 no empezada · 🔄 en curso · ✅ cerrada (DoD completo, sin parciales).
- Los ADR son inmutables: no se editan, se supersede con uno nuevo.

## Umbral de épica
Merece doc propio toda tarea de ≥3 pom, O que cruce más de un día, O que
produzca un artefacto con nombre propio. Lo que no llegue vive como checkbox
del doc semanal.

## Regla que más se viola
No generes una épica cuyo checklist dependa de un artefacto que aún no existe.
Se genera cuando se desbloquea. Un checklist sobre algo inexistente es ficción.