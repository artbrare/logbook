# 🎯 E05 — Núcleo de datos: schema · migración · seed · query

> **Estado:** 🔲 no empezada · **Semana:** 01 · **Días:** jue 30 (🚴) · vie 31 · sáb 1 · **Proyecto:** `loop`
> **Estimado:** 10.8 pom (~270 min: 8.4 de escritorio + 2.4 de bici) · **Real:** ___ pom
> **Depende de:** E03 — modelo v3 defendido y `adr-002-modelo.md` aceptado (30 jul) · E04 fases B–C — Compose levantando API y Postgres
> **Desbloquea:** el goal de la semana. Y S2: sin tablas ni query no hay API que tipar ni endpoints que testear

> ⚠️ **Cambio de alcance respecto al doc semanal (30 jul).** La fase B decía *"parser del formato Apple Notes"*.
> La `spec-v1.md` §10 —escrita el martes, después del plan— dice literal: *"Loop arranca de cero. No hay
> importador de ninguna herramienta externa"*, y define en su lugar un **seed transcrito** de semanas reales.
> Gana la spec. **No se escribe parser.** Lo que el goal necesita no es el parser: son datos reales con
> patrones de repetición verdaderos contra los que correr la query. La transcripción los da igual, y sin
> depender de D5 —que sigue sin respuesta— ni del formato en que Apple Notes decida exportar.

> ⚠️ **Corrección de datos, no cambio de scope (30 jul).** La primera versión de esta épica mandaba
> transcribir las semanas 26, 29 y 30, con la 29 como prioridad. Estaba invertido: **las semanas 29 y 30
> usan `*` sin corchetes y no registran estado de completado.** Transcritas, ningún registro sale
> `completado` y **todo se ve como leak** — el detector devuelve falsos positivos masivos y el DoD 3 deja
> de ser verificable, porque no hay señal contra ruido. La 26 nunca se revisó y su formato se desconoce.
> Las semanas **12 y 15 usan `- [ ]` / `- [x]`**: sí tienen estado. Se transcribe la **15**, que además
> trae los dos leaks documentados *y* completados alrededor para contrastar. **No es scope creep: la
> muestra anterior no podía probar lo que el DoD pide.**

---

## 🎯 Objetivo

> Que el modelo defendido el jueves exista como tablas corriendo en Postgres, con tus semanas reales adentro y una query de score que tú escribiste y cuyo plan de ejecución sabes leer.

## 💡 Por qué existe

Es la mitad del goal de la semana, y la mitad que no se puede simular: un modelo en markdown no revela sus errores, un schema sí. La columna que no supiste tipar, el enum que no existía en Postgres, la vista que Alembic no genera sola — todo eso aparece el viernes o no aparece nunca.

Y el sábado es donde la semana deja de ser CRUD. Una query de agregación ponderada contra datos reales, con su `EXPLAIN` leído y su índice justificado, es el entregable que más te acerca a la capacidad que persigue la semana: **SQL que entiendes línea por línea, no que te generaron.** Si esta épica se cae, la semana produjo documentos y ningún software.

## 🔑 Prerrequisitos

- [x] Modelo v3 cerrado y `adr-002-modelo.md` aceptado — H1, H2 y H3 resueltas (30 jul)
- [x] `api/` con SQLAlchemy, Alembic y `migrations/versions/` vacío (E04 fase B)
- [x] Postgres corriendo con imagen `pgvector/pgvector:pg16` y volumen nombrado
- [ ] E04 fase C cerrada: `docker compose up` levanta, `/health` responde y la fila sobrevive un `down && up`. **Si el Compose quedó como deuda del miércoles, se paga antes de fase A** — sin base que persista no hay contra qué migrar
- [ ] La semana 15 de Apple Notes a la mano, en pantalla, para transcribir *(y la 12 si sobra tiempo — las dos usan `- [ ]` / `- [x]`, que es lo que hace transcribible el estado)*

---

## ✅ DEFINITION OF DONE

- [ ] Desde cero —`make db-nuke && make up && make migrate`— la migración corre sin error y `make sql q="\dt"` lista **las 7 tablas del modelo v3** más la vista `records`
- [ ] `make seed` carga la **semana 15 completa** y es **idempotente**: correrlo dos veces no cambia el conteo de filas. El conteo coincide con el número que anotaste **antes** de correrlo, contando a mano sobre las notas
- [ ] Tu query de score ponderado devuelve porcentaje por semana, por día y por categoría contra esos datos, y **los dos leaks documentados de la semana 15 aparecen solos** — Apex Lab (5 apariciones, 0 completadas) e Insights (6 apariciones, 0 completadas) — **y nada más aparece con ellos**
- [ ] El `EXPLAIN` está pegado en el log de esta épica, con el índice elegido y su razón escrita, y **las dos condiciones aparecen en `Index Cond`, ninguna en `Filter`**

---

## 📋 Checklist de ejecución

### Fase 0 — Diseño en voz alta · 2.4 pom (60 min) · **JUE 30** · 🚴 *bici*
> Llegar el viernes a escribir, no a descubrir. Nada de esto necesita teclado.

- [ ] **Mapeo nota → filas.** Toma una línea real de la semana 15 y dicta qué fila produce y en qué tabla: qué es `scheduled_task` (la regla) y qué es `daily_record` (el hecho). Los casos sucios primero: `Dedicar 2 pomodoros al plan` · `0/2` · `2/2` — la misma tarea escrita de tres formas (F7 de la spec)
- [ ] **El estado sale del corchete, y de nada más.** `- [x]` → `completado`, `- [ ]` → `pendiente`. Dicta cómo se decide qué `- [ ]` de una semana ya vivida queda `no_completado`: **no se escribe, se deriva de la vista** (H1). El seed guarda `pendiente`; la vista hace el resto
- [ ] **La query de score, en voz alta antes que en SQL.** Qué se suma en el numerador, qué en el denominador, de qué tabla sale el peso *(de `daily_records`, no de `scheduled_tasks` — D3)*, y por qué se lee de la **vista** y no de la tabla *(H1: filtrar sobre la tabla subestima los leaks en silencio)*
- [ ] **Los tres cortes.** Semana, día y categoría, ¿son tres queries o una con `GROUP BY` distinto? Dicta tu respuesta y el porqué
- [ ] **Predicción de índice.** Antes de ver un solo `EXPLAIN`: ¿qué índice espera usar esta query y cuál es tu apuesta de qué va a aparecer en el plan? Anótala — el sábado se compara contra la realidad y esa comparación **es el aprendizaje**, más que el resultado

### Fase A — Schema v0 · 2.0 pom (50 min) · **VIE 31**
> Traducir el modelo a modelos de SQLAlchemy. Nombres **en inglés** (ADR-002 §2); `modelo-arturo.md` sigue siendo la fuente de verdad del diseño.

- [ ] Fijar el nombre en inglés de cada tabla y columna, en una tabla de equivalencias en el ADR o en el propio modelo: `users` · `categories` · `scheduled_tasks` · `daily_records` · `pomodoro_sessions` · `daily_reports` · `weekly_reports`
- [ ] Un archivo por modelo en `api/app/models/`, e **importarlos todos en `app/models/__init__.py`** *(ver la trampa: si no se importan, autogenerate no falla — genera una migración vacía)*
- [ ] Los tipos que no son obvios: `uuid` con `gen_random_uuid()` · los dos `enum` de Postgres · `smallint[]` de `repeat_days` · `timestamptz` vs `date` según H3 · `numeric(5,2)` de los porcentajes
- [ ] Las restricciones que el modelo declara, no solo las columnas: `CHECK` de `level BETWEEN 1 AND 3`, `CHECK` de `weight BETWEEN 1 AND 5`, los dos `UNIQUE` de reportes, `NOT NULL` en cada `user_id`
- [ ] Los 5 índices del modelo, declarados en los modelos — no a mano en la migración
- [ ] ⛔ **`embedding` NO entra.** Decisión del 30 jul: sin generador de embeddings, una columna `NOT NULL` bloquea el seed del sábado, y no le sirve a ningún DoD de esta semana. Entra en S2 con su propia migración. Se anota en el log como deuda, no como olvido

### Fase A2 — Primera migración · 1.4 pom (35 min) · **VIE 31**
- [ ] `make revision m="initial schema"` y **leer la migración generada completa antes de aplicarla.** Es autogenerada: se revisa como se revisa un PR ajeno
- [ ] Agregar a mano lo que autogenerate no ve: `CREATE EXTENSION IF NOT EXISTS vector` como primera operación *(ADR-002 §1)* y los dos `CREATE TYPE` de los enums si no salieron solos
- [ ] Agregar a mano la **vista `records`** con `effective_status`, con `op.execute()` — con su `DROP VIEW` en el `downgrade`
- [ ] `make migrate` y verificar en `make psql`: `\dt` lista las tablas, `\d daily_records` muestra los índices y los CHECK, `\dv` muestra la vista
- [ ] **Probar el `downgrade` una vez** — si no baja, la migración no es reversible y eso se descubre ahora, no en S2
- [ ] Commit: los modelos y la migración juntos, nunca separados

### Fase B — Seed con datos reales · 2.4 pom (60 min) · **SÁB 1**
> ⏱️ **Orden obligatorio: primero la semana 15 completa.** Es la única que trae las dos cosas que el DoD 3 necesita — los leaks *y* los completados alrededor para contrastar. Si el bloque se acaba, la 12 pasa a deuda; la 15 no.

- [ ] Transcribir a un archivo estructurado en `docs/seed/` —un archivo por semana— siguiendo el mapeo dictado en fase 0. **Es transcripción, no interpretación:** lo que la nota no dice, el seed no lo inventa
- [ ] **Contar a mano, antes de cargar nada**, cuántos registros deberían salir por semana. Ese número es el DoD 2 y no vale contarlo después
- [ ] `api/scripts/seed.py` — lee los archivos e inserta. **Idempotente**: correrlo dos veces deja la base igual. Usa el ORM, es CRUD *(la regla de `loop/CLAUDE.md`)*
- [ ] Un `scheduled_task` por cada tarea que se repite, y su `scheduled_task_id` en cada `daily_record` que generó — **sin esa FK la detección de leaks compara por nombre y los nombres cambian** (ADR-002 C1)
- [ ] Target `seed` en el `Makefile`
- [ ] Verificar: `make sql q="select count(*) from daily_records"` contra tu conteo a mano. Si no coincide, se investiga la diferencia — **no se ajusta el número**

### Fase C — Query de score · 1.4 pom (35 min) · **SÁB 1** · 🚫 **la escribes tú**
> ⚠️ Si sale de un prompt, la capacidad de la semana no se adquirió aunque el checkbox esté verde (Riesgo #4).

- [ ] Escribir la query en `api/app/queries/` — Core, no ORM *(la regla de `loop/CLAUDE.md`: si una agregación aparece escrita con el ORM, está mal aunque funcione)*
- [ ] Lee de la **vista `records`**, filtra por `user_id`, agrega por semana / día / categoría
- [ ] Correrla contra los datos del seed y **contrastar el porcentaje contra lo que dicen tus notas de esa semana**. Un número que no puedes verificar contra la realidad no prueba nada
- [ ] La query de leaks: tareas programadas con ≥3 apariciones y ninguna completada. **Apex Lab e Insights tienen que salir los dos, y nada más con ellos** — si sale media semana, el problema no es la query: es que el seed perdió los `- [x]`
- [ ] Guardar el SQL en el repo, no en el historial de la terminal

### Fase D — Sesión de diseño SQL · 1.2 pom (30 min) · **SÁB 1** · 🤝 *sesión conmigo*
- [ ] `EXPLAIN ANALYZE` de la query de score. Leerlo de adentro hacia afuera y **compararlo contra tu predicción de fase 0**
- [ ] Verificar la regla del modelo: `user_id` y `date` deben aparecer los dos en `Index Cond`. **Si `date` cayó en `Filter`, el índice está en el orden equivocado** y ahí está el aprendizaje
- [ ] Con ~200 filas Postgres puede elegir `Seq Scan` y tener razón. Forzar el escenario: `SET enable_seqscan = off` para ver el plan con índice, o razonar el punto de cruce
- [ ] **H4, que ADR-002 dejó para hoy:** escribir la query de subárbol *("todo lo que cuelga de Personal")* con los 3 niveles fijos y confirmar si `(user_id, parent_category_id)` alcanza
- [ ] ¿Qué se degrada primero con volumen — el score, los leaks o el subárbol? Anotar la respuesta con su razón
- [ ] Pegar el `EXPLAIN` y la justificación del índice en el log de esta épica

---

## 🚫 Fuera de scope (explícito)

```
Parser de Apple Notes                   → no se escribe. spec-v1 §10. La transcripción lo sustituye
Columna embedding y pgvector en Python  → S2, con su propia migración, cuando exista el generador
Endpoints CRUD sobre las tablas         → S2. Aquí las tablas se llenan por seed, no por HTTP
Repositories en api/app/repositories/   → S2. El seed usa el ORM directo; no se abstrae lo que no se repite
Auth con Clerk                          → S2. users se llena con una fila tuya y clerk_user_id de mentira
Reportes generados con IA               → S2+. Las tablas existen; nadie las llena esta semana
H5, el día que no existe                → sin mitigación por decisión (ADR-002). No se resuelve aquí
Optimizar la query                      → primero se lee el plan. Optimizar antes de medir es adivinar
Tests de la query                       → ADR-001 §5 pide unit de lógica pura; esta semana la verificación
                                          es contrastar contra tus notas, y está en el DoD
Semanas 26, 29 y 30 en el seed          → fuera. 29 y 30 usan `*` sin corchetes: no traen estado, y sin
                                          estado todo se ve como leak. La 26 nunca se revisó. Se revisan
                                          en S2, si aparece la necesidad de más historia
Mapear el `*` sin corchetes a un status → no se hace. Cualquier mapeo inventa un dato que la nota no dio
```

## ⚠️ Trampa conocida

**La grande es Alembic, y es silenciosa.** `autogenerate` solo ve los modelos importados en `app/models/__init__.py` — el propio archivo lo advierte. Si falta un import, la migración **no falla: sale vacía o incompleta**, `make migrate` responde OK y descubres el hueco el sábado con el seed encima. Por eso el checklist dice *leer la migración generada completa antes de aplicarla*.

Lo mismo con lo que autogenerate no sabe generar: **la extensión, los enums de Postgres y la vista**. La vista `records` es la mitigación obligatoria de H1 — si no queda en esta migración, cualquier query que se escriba después lee de la tabla y **subestima los leaks sin que nadie se entere**. Es exactamente el modo de falla que la vista existe para prevenir.

**El timebox real es el sábado, y es la transcripción.** Una semana de notas transcrita a mano se come 30 minutos sin esfuerzo, y el bloque de la query viene detrás. **Semana 15 primero, completa.** A los 30 minutos de fase B, lo que no esté transcrito pasa a deuda y se sigue con el seed de lo que haya — la 15 sola alcanza para el DoD; dos semanas y ninguna query, no.

**Y la trampa que ya cobró una vez:** transcribir una semana sin verificar antes su formato. Las semanas 29 y 30 se planearon como fuente del seed durante días y no sirven — `*` sin corchetes, sin estado de completado. **Antes de transcribir la 12, ábrela y confirma que usa `- [x]`.** El costo de verificar es un minuto; el de no verificar es descubrir a media fase B que el seed no puede probar nada.

**Y la de siempre, la del Riesgo #4:** que la query salga de un prompt. Es la única tarea de la semana donde pedir ayuda se siente gratis y cuesta la capacidad completa. Si te trabas, el movimiento es pedir el mecanismo —*cómo funciona un `GROUP BY` con `FILTER`*— nunca la query.

## 🚪 Comprehension gate

- [ ] Explico por qué el peso se lee de `daily_records` y no de `scheduled_tasks`, y qué le pasaría al 78% de la semana 12 si fuera al revés
- [ ] Explico por qué `no_completado` se deriva en una vista en vez de escribirse con un job, y qué se rompe si una query nueva lee la tabla directo
- [ ] Explico qué índice necesita mi query de score y **por qué**, leyendo mi propio `EXPLAIN` — igualdad antes que rango, y qué significa que una condición caiga en `Filter`
- [ ] Explico qué ve `autogenerate` y qué no, y por qué la vista y la extensión van escritas a mano

---

## 📝 Log de ejecución
| Fecha | Fase | Pom | Qué pasó |
|---|---|---:|---|
| 30 jul | 0 | | |
| 31 jul | A, A2 | | |
| 1 ago | B, C, D | | |

---

## 🏁 Cierre
- **Estimado vs real:** 10.8 → ___ pom · desviación ___% · **causa:** {…}
- **DoD cumplido:** {sí / parcial — cuáles faltaron}
- **`EXPLAIN` y el índice:** {pegar el plan y la justificación · ¿coincidió con la predicción de fase 0?}
- **Conteo de filas:** esperado ___ / real ___ · {si no coincidió, por qué}
- **Aprendizaje:** {…}
- **Deuda o seguimiento:** {embedding a S2 · semana 12 si no entró · lo que salga de H4}
- **Nota para la retro:** el cambio de semanas 26/29/30 → 15 (30 jul) fue **corrección de datos, no scope creep**: la muestra planeada no registraba estado de completado y hacía inverificable el DoD 3. Cuenta como lección de *verificar la fuente antes de agendarla*, hermana de la lección 4 de S01
