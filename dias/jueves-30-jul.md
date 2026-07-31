# 🎯 JUEVES 30 JUL — Plan de batalla

> **Día 4 de 7 · S1 · Loop en producción el 15 de agosto.**
> De dónde venimos: lun 2.0 · mar 9.4 · mié 10.0 pom.

---

## 🎯 El trabajo de hoy, en una frase

> **Cerrar E03 con las cuatro decisiones abiertas resueltas, para que el viernes el schema
> se escriba sin descubrir nada.**

El viernes es el día en que el modelo deja de ser papel. Si llega con H1, H2 y H3 sin decidir, ese día se va en decidir en vez de en construir — y el sábado, que define el goal, arranca tarde.

## ⚖️ Y una corrección al método

El análisis de ayer dice: *"bastante productivo, pero sin mucho aprendizaje: los conceptos son muy complejos"*.

**El diagnóstico es correcto y la causa es el plan, no la comprensión.** Ayer entraron ~17 conceptos nuevos en un día. Nadie consolida eso.

Por eso hoy hay un bloque que ayer no existía: **consolidación**. Leer el código que se aprobó, con el doc de estudio al lado. No es repaso pasivo — es abrir el archivo real y encontrar la línea de la que habla cada concepto.

---

# 🕗 ORDEN DEL DÍA

> 🔢 **Numeración alineada con `semanas/semana-01.md`.** Los dos docs habían divergido
> y era confuso. Este es el orden de ejecución real, que cambió sobre la marcha.

```
✅ 1 · Decisiones abiertas ······ 1.0 pom · H1, H2, H3 cerradas
✅ 2 · Cerrar E03 ··············· 1.0 pom · ADR-002 · ADR-003 · E05 generada
🔲 3 · Consolidación ············ 1.2 pom · ← PENDIENTE
✅ 4 · Tableau sesión 1 ········· real 2.0 pom (+43%)
🔲 5 · Personal · lectura ······· 1.2 pom
🔲 6 · Bici: estudio ············ 2.4 pom
🔲 7 · Cierre ·················· 0.4 pom
```

> ⚠️ **El bloque 3 se saltó.** Estaba planeado antes de Tableau y se pasó por alto.
> Sigue vivo y es la corrección directa al análisis del miércoles.

## ✅ BLOQUE 1 · Decisiones abiertas · CERRADO

**H1 · `no_completado` se DERIVA** — `pendiente` + fecha vencida, sin job.
Es una expresión, no infraestructura, y se autocorrige. Se escribe **una vez en una vista** para que ninguna query la olvide.

**H2 · `sesiones_pomodoro` ENTRA** — el schema se puede migrar después; **los datos que no capturas hoy no se pueden reconstruir.** Elimina `num_pomodoros_terminados` (se cuenta).

**H3 · Instante + zona IANA, fecha civil local**

```
usuarios.timezone            text   IANA · "America/Mexico_City"
registros_diarios.fecha      date   la fecha LOCAL del usuario
sesiones_pomodoro.inicio     timestamptz   el instante
sesiones_pomodoro.tz         text   IANA al momento del pomodoro
```

- `timestamptz` guarda el **instante** (en UTC por dentro) pero **no recuerda dónde ocurrió**. Por eso la zona se guarda aparte.
- **Zona IANA, no offset.** `-06:00` es un número muerto; `America/Mexico_City` es un conjunto de reglas que sabe de horarios de verano. Importa porque `dias_repeticion` son reglas a futuro y "lunes" solo existe dentro de una zona.
- **La zona se copia al evento**, igual que el peso. Si en un año cambias tu perfil a Tokio, tus sesiones de julio en CDMX no se reinterpretan.
- **Quién decide:** el cliente manda su zona, el servidor calcula y guarda. Nunca el reloj del servidor solo.
- ⚠️ **Corrige la derivación de H1:**
  ```sql
  ❌ fecha < CURRENT_DATE
  ✅ fecha < (now() AT TIME ZONE u.timezone)::date
  ```
  Sin eso, entre las 6pm y medianoche en CDMX tus tareas de hoy se marcarían como no completadas mientras sigues trabajando.

**H4 · query de subárbol → VIERNES.** Afecta un índice, no el schema.

**A1 · el engine global → gate del domingo.** No bloquea nada.

---

## 🚴 BLOQUE 2 · Estudio completo + examen

> **Los nueve temas con sus preguntas, y después el examen de 35.** Decisión de Arturo.

**2a · Los nueve temas**
- [ ] Tema 1 · Bases de datos + sus 8 preguntas
- [ ] Tema 2 · Modelado + sus 7 preguntas
- [ ] Tema 3 · Índices + sus 6 preguntas
- [ ] Tema 4 · Docker + sus 8 preguntas
- [ ] Tema 5 · Servidor web + sus 8 preguntas
- [ ] Tema 6 · Transacciones y SQLAlchemy + sus 10 preguntas
- [ ] Tema 7 · CORS + sus 7 preguntas
- [ ] Tema 8 · Migraciones + sus 7 preguntas
- [ ] Tema 9 · Agentes + sus 7 preguntas

**Ciclo por tema:** leer completo → **cerrar el doc** → contestar dictando → comparar → anotar lo que no salió.
Contestar con el texto a la vista se siente igual de productivo y no fija nada.

**2b · El examen · 35 preguntas**
- [ ] Contestar de corrido, en voz alta
- [ ] 🚫 **Sin abrir la rúbrica** hasta terminar las 35
- [ ] Comparar contra la rúbrica y contar aciertos
- [ ] Mandarme las respuestas

**2c · Lectura**
- [ ] APoSD

> ⏱️ **Nota de tiempo, sin decidir por ti:** los nueve temas son ~60-75 min de lectura,
> sus preguntas ~30 min, y el examen ~40 min. Son ~2h15 contra 1h de bici.
> Lo que no quepa pedaleando se termina en escritorio — tú decides el corte.

---

## 🟠 BLOQUE 3 · Cerrar E03 · 1.0 pom (25 min)

- [ ] `adr-002-modelo.md` — los tres cambios del interrogatorio (FK nullable · `status` enum · fuera objetivos semanales) + H1, H2 y H3
- [ ] `adr-003-fechas.md` — librería de fechas, ahora sí: H3 ya está resuelta
- [ ] Actualizar `modelo-arturo.md` con H1–H3 y los campos de zona
- [ ] Commit y push
- [ ] ✅ **Cerrar E03** → generar **E05** con la skill `nueva-epica`

> 📌 Segundo uso de la skill → **cierra el DoD de E02 fase C**.

---

## 🟡 BLOQUE 3 · Consolidación · 1.2 pom (30 min) · 🔲 **PENDIENTE**

> **La corrección al análisis del miércoles** (*"productivo pero sin aprendizaje: ~17 conceptos
> nuevos en un día"*). **No es repaso pasivo:** doc de estudio en una ventana, **código en otra**,
> y por cada concepto se busca **la línea real en el repo**.

- [ ] **Tema 4 · Docker** → abrir `api/Dockerfile` y `compose.yml`
  - [ ] Señalar la línea donde las dependencias van **antes** del código
  - [ ] Señalar el `--bind 0.0.0.0`
  - [ ] Señalar el volumen nombrado · correr `docker volume ls` para verlo existir
- [ ] **Tema 6 · Servidor web** → abrir `api/app/__init__.py`
  - [ ] Señalar el factory, el `register_blueprint`, y los dos hooks
- [ ] **Tema 7 · Transacciones** → abrir `api/app/db.py`
  - [ ] Señalar dónde se crea el engine (**una vez**) y dónde la sesión (**por request**)
  - [ ] Señalar el `close()` y confirmar que **no hay `commit()`**
- [ ] Anotar **qué concepto no encontraste en el código** — ese es el que no se entendió

> 📌 **Si el bloque 6 (estudio) va primero, este llega dirigido:** las preguntas que falles
> marcan exactamente qué buscar. Si no, se hace genérico y rinde menos.

---

## ✅ BLOQUE 4 · E06 fase A — Tableau sesión 1 · 1.2 pom → real **2.0** (+43%)

> Deuda del lunes · Desktop **Free Edition** · 🔒 guardado local, nunca se publica

- [ ] Conectar **Greenstone** con la cadena de conexión
- [ ] Identificar dimensiones vs medidas, filas vs columnas, marcas
- [ ] Construir una vista simple: serie de tiempo o ranking
- [ ] **Anotar la pregunta concreta** que el dashboard responderá
- [ ] Elegir qué dashboard de Greenstone replicar — tu ground truth
- [ ] Anotar qué se aprendió *(sesión sin nota no cuenta)*

---

## ✅ BLOQUE 7 · Cierre · 0.4 pom

- [ ] Lectura registrada · hábitos · entrenos
- [ ] Poms reales + análisis del día
- [ ] Commit y push en los dos repos
- [ ] Verificar que **E05 existe** — el viernes arranca de ahí

---

# 🚨 Las tres trampas de hoy

**1. Contestar con el documento abierto.** Es la más cara del día. Leer la respuesta mientras "contestas" **elimina el aprendizaje** y te deja con la sensación falsa de que sí sabías. Cierra el doc antes de contestar, siempre.

**2. Que la consolidación se convierta en lectura pasiva.** Si solo relees el doc sin abrir el código, no sirve. **La instrucción es señalar líneas reales en tu repo.**

**3. Que Tableau se vuelva ver tutoriales.** La sesión termina con una vista construida sobre Greenstone, o no cuenta.

---

# ✅ Cómo sabes que el día salió bien

```
□ H1, H2 y H3 decididas y escritas          ✅ hecho
□ Temas 4, 5 y 6 leídos y sus preguntas contestadas a ciegas
□ adr-002 y adr-003 commiteados
□ E03 cerrada · E05 generada con la skill
□ Las preguntas falladas rastreadas hasta una línea real de código
□ Una vista de Tableau construida sobre Greenstone
```

Si eso está, **el viernes arranca escribiendo** — que es exactamente lo que E04 existía para lograr.

---

# 📌 Lo que queda para el viernes

```
🚨 La PRIMERA línea de la migración: CREATE EXTENSION IF NOT EXISTS vector
   El scaffold probó pgvector y luego borró la extensión a propósito.
   Si la base de dev tiene estado que ninguna migración creó, la migración miente.

· Schema v0 traducido del modelo · nombres en inglés
· Primera migración corriendo contra Postgres
· Tableau sesión 2
· Digest de vanguardia + 3 takeaways
· Preguntar D6 (licencias Tableau) y D7 (keys eLearning)
```
