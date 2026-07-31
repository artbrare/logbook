# 🗄️ VIERNES 31 JUL — Plan de batalla

> **Día 5 de 7 · S1** · Loop en producción el 15 de agosto
> De dónde venimos: lun 2.0 · mar 9.4 · mié 10.0 · jue ___ pom

---

# 🎯 El día en una frase

> **Hoy el modelo deja de ser markdown y se vuelve tablas corriendo en Postgres.**

Y por qué importa tanto: **el sábado es el día que decide el goal de la semana** — el seed, la query de score y el `EXPLAIN`. Si hoy no hay schema, mañana arranca construyendo en vez de consultando, y el goal se cae.

```
Un modelo en markdown NO revela sus errores.
Un schema SÍ.

La columna que no supiste tipar, el enum que no existe en Postgres,
la vista que Alembic no genera sola — todo eso aparece hoy, o no aparece nunca.
```

---

# ⚖️ Prioridad, decidida antes de arrancar

```
🔴 INTOCABLE     Bloques 1 y 2 — schema y migración
                 Sin esto, el sábado no existe.

🟠 IMPORTANTE    Bloque 3 — consolidación (deuda del jueves)
                 Es la corrección al análisis del miércoles.

🟡 SACRIFICABLE  Bloques 4 y 5 — Tableau y digest
                 Si el día aprieta, caen en ese orden.
```

**Si algo se cae, que caiga el digest primero, después Tableau. Nunca el bloque 1.**

---

# 🕗 ORDEN DEL DÍA

```
🔴 1 · E05 fase A · Schema v0 ············ 2.0 pom  ← PRIMERO, sin excepción
🔴 2 · E05 fase A2 · Migración ··········· 1.4 pom
🟠 3 · Consolidación (deuda del jueves) ·· 1.2 pom
🟡 4 · Tableau sesión 2 ·················· 1.2 pom
🟡 5 · Digest de vanguardia ·············· 1.2 pom
⚪ 6 · Personal + cierre ················· 1.6 pom
🚴 opcional · Bici — estudio temas 4-7

Escritorio 8.6 pom
```

---

## 🔴 BLOQUE 1 · Schema v0 · 2.0 pom (50 min)

> **Lo primero del día, con la cabeza fresca.** Traducir el modelo v3 a modelos de SQLAlchemy.
> `docs/modelo-arturo.md` es la fuente de verdad del diseño; los nombres van **en inglés**.

### 1a · La tabla de equivalencias · 5 min

Escríbela antes de codear. Va en el ADR o en el propio modelo:

```
usuarios              →  users
categorias            →  categories
tareas_programadas    →  scheduled_tasks
registros_diarios     →  daily_records
sesiones_pomodoro     →  pomodoro_sessions
reportes_diarios      →  daily_reports
reportes_semanales    →  weekly_reports
```

### 1b · Los modelos · 30 min

- [ ] Un archivo por modelo en `api/app/models/`
- [ ] ⚠️ **Importarlos TODOS en `api/app/models/__init__.py`**

> **Esta es la trampa grande del día y es silenciosa.** `autogenerate` **solo ve los modelos
> importados ahí**. Si falta uno, la migración **no falla: sale incompleta**, `make migrate`
> responde OK, y lo descubres mañana con el seed encima.

**Los tipos que no son obvios:**

```
id                  uuid, default gen_random_uuid()
status              ENUM de Postgres: pendiente|completado|no_completado
tipo                ENUM: pomodoros|puntual
repeat_days         smallint[]        ← array, 0=lunes … 6=domingo
date                date              ← fecha CIVIL local (H3)
start / end         timestamptz       ← instantes (H3)
tz                  text              ← IANA, NO offset (H3)
percentage          numeric(5,2)      ← exacto, no float
```

**Las restricciones, no solo las columnas:**

```
CHECK (level BETWEEN 1 AND 3)         categories
CHECK (weight BETWEEN 1 AND 5)        scheduled_tasks y daily_records
UNIQUE (user_id, date)                daily_reports
UNIQUE (user_id, year, week)          weekly_reports
NOT NULL en cada user_id              todas las tablas de dominio
```

**Los 5 índices, declarados en los modelos** — no a mano en la migración:

```
(user_id, date)                       daily_records   ← igualdad antes que rango
(user_id, scheduled_task_id, date)    daily_records   ← la query de leaks
(daily_record_id)                     pomodoro_sessions
(user_id, parent_category_id)         categories
(user_id, active)                     scheduled_tasks
```

### 1c · Lo que NO entra hoy · 2 min

```
⛔ embedding y pgvector en Python  → S2, con su propia migración
   Sin generador de embeddings, una columna NOT NULL bloquea el seed de mañana.
   Se anota como DEUDA en el log, no como olvido.
```

---

## 🔴 BLOQUE 2 · Primera migración · 1.4 pom (35 min)

```bash
make revision m="initial schema"
```

### ⚠️ Lee la migración completa ANTES de aplicarla

**Es autogenerada: se revisa como se revisa un PR ajeno.** Y hay tres cosas que `autogenerate` **no sabe generar** y van a mano:

**1 · La extensión — primera operación del `upgrade`**

```python
op.execute("CREATE EXTENSION IF NOT EXISTS vector")
```

> **Por qué (paridad dev/prod):** el scaffold del miércoles probó pgvector y **borró la extensión
> a propósito**. Si tu base de dev tiene estado que ninguna migración creó, la migración funciona
> en tu máquina y **falla en una base limpia**. Es de las cosas que tumban deploys.

**2 · Los `CREATE TYPE` de los enums**, si no salieron solos.

**3 · La vista `records` — la mitigación de H1**

```python
op.execute("""
CREATE VIEW records AS
SELECT r.*,
       CASE WHEN r.status = 'pendiente'
             AND r.date < (now() AT TIME ZONE u.timezone)::date
            THEN 'no_completado' ELSE r.status END AS effective_status
FROM daily_records r JOIN users u ON u.id = r.user_id
""")
```

**Y su `DROP VIEW` en el `downgrade`.**

> ⚠️ **Si la vista no queda en esta migración**, cualquier query que se escriba después lee de la
> tabla directa y **subestima los leaks sin que nadie se entere**. Es exactamente el modo de falla
> que la vista existe para prevenir.

### Verificar

```bash
make migrate
make psql
```

```sql
\dt                  -- las 7 tablas
\d daily_records     -- índices y CHECK
\dv                  -- la vista records
```

- [ ] **Probar el `downgrade` una vez.** Si no baja, la migración no es reversible — y eso se descubre hoy, no en S2
- [ ] Commit: **modelos y migración juntos, nunca separados**

---

## 🟠 BLOQUE 3 · Consolidación · 1.2 pom (30 min) · **deuda del jueves**

> Es la corrección al análisis del miércoles: *"productivo pero sin aprendizaje, ~17 conceptos
> nuevos en un día"*. **No es repaso pasivo.**

**El método:** doc de estudio en una ventana, **el código en otra**, y por cada concepto **buscar la línea real en el repo**.

- [ ] **Tema 4 · Docker** → `api/Dockerfile` y `compose.yml`
  - [ ] La línea donde las dependencias van **antes** del código
  - [ ] El `--bind 0.0.0.0`
  - [ ] El volumen nombrado · `docker volume ls` para verlo existir
- [ ] **Tema 6 · Servidor web** → `api/app/__init__.py`
  - [ ] El factory, el `register_blueprint`, los dos hooks
- [ ] **Tema 7 · Transacciones** → `api/app/db.py`
  - [ ] Dónde se crea el engine (**una vez**) y dónde la sesión (**por request**)
  - [ ] El `close()` — y confirmar que **no hay `commit()`**
- [ ] Anotar **qué concepto no encontraste** en el código

---

## 🟡 BLOQUE 4 · Tableau sesión 2 · 1.2 pom (35 min)

> 🔄 El ejercicio cambió: **Load Times 2.0 vive en Luzmo**, no en Tableau. Replicar el diseño sería
> copiar una interfaz. **Se reproducen las CIFRAS y se comparan las herramientas.**

- [ ] Abrir **Load Times 2.0 en Luzmo** y anotar **3-5 números concretos**
      *("promedio 14.2 min" · "3,412 tickets over 60" · "top location: X")*
- [ ] Reproducir esos números en Tableau con el `.twb` de ayer
- [ ] ⭐ **La pregunta clave:** ¿cómo trata Luzmo los **82,737 "Not timed"**? ¿Los muestra, los excluye, o los ignora en silencio?

> Si Load Times 2.0 reporta un promedio sin decir que excluye el 27% de los tickets, **ese es un
> hallazgo** — y lo encontraste tú, con tu propia query, en tu primera semana de Tableau.
> Eso ya no es aprender una herramienta: es **leer críticamente un entregable de producción**,
> que es justo el rep de liderazgo que el programa pide para agosto.

- [ ] Anotar la comparación: qué fue más fácil en cada herramienta · qué número no coincidió y **por qué**
- [ ] 🔒 Guardado local

---

## 🟡 BLOQUE 5 · Digest de vanguardia · 1.2 pom (30 min)

> Ritual del viernes. Yo busco, tú anotas.

- [ ] Pedirme el digest: AI engineering · ecosistema Claude · backend, infra y data
- [ ] **Escribir 3 takeaways** *(sin takeaways escritos no cuenta — regla de cuotas #2)*

---

## ⚪ BLOQUE 6 · Personal + cierre · 1.6 pom

**Personal**
- [ ] Lectura 30 min — APoSD
- [ ] Registro · hábitos · entrenos

**Pendientes administrativos** *(los tres llevan días abiertos)*
- [ ] 📞 **D6** — ¿la empresa tiene licencias de Tableau? *(ya sabes que la edición gratuita conecta; la pregunta ahora es dónde deben vivir los datos de Greenstone)*
- [ ] 📞 **D7** — ¿hay acceso a las keys del eLearning? *(Sarvani)*
- [ ] 🔴 **Rotar la credencial** `fronagbb1282b1_temp_ro` — se expuso en texto plano
- [ ] 🔴 Limpiar el historial: `grep -n "fronagbb" ~/.zsh_history`
- [ ] 🔍 Ubicar el `.hyper` del extract *(normalmente `Documentos/Mi repositorio de Tableau/Fuentes de datos/`)* y confirmar que **no** está en iCloud

**Cierre**
- [ ] Poms reales por bloque
- [ ] Análisis del día
- [ ] Commit y push en los dos repos

---

## 🚴 Opcional · Bici — estudio temas 4-7

```
Tema 4  · Docker
Tema 5  · Puertos y redes
Tema 6  · Servidores web
Tema 7  · Transacciones y sesiones
```

**El ciclo:** leer el tema → **cerrar el doc** → contestar dictando → comparar → anotar lo que falló.

📌 **Y el ejercicio nuevo, que sale de la revisión de ayer:** por cada decisión que expliques, **di también qué cuesta**, aunque la pregunta no lo pida. El patrón detectado fue que los mecanismos están pero faltan los nombres y los costos — y en el gate, *"lo elegí porque X"* sin *"y me cuesta Y"* cuenta como media respuesta.

> Temas 8-10 el sábado. **El examen de 35 preguntas: domingo.**

---

# 🚨 Las cuatro trampas de hoy

**1 · Olvidar un import en `models/__init__.py`.** Es la más cara y la más silenciosa: la migración no falla, sale incompleta, y lo descubres mañana con el seed encima. **Por eso se lee la migración generada completa antes de aplicarla.**

**2 · Aplicar la migración sin leerla.** Es autogenerada. Se revisa como un PR ajeno.

**3 · Que se olvide la vista `records`.** Sin ella, toda query futura lee la tabla directa y subestima los leaks en silencio.

**4 · Empezar por Tableau o el digest.** Son los bloques cómodos. El schema es el incómodo, y es el que desbloquea mañana.

---

# ✅ Cómo sabes que el día salió bien

```
□ make db-nuke && make up && make migrate corre sin error desde cero
□ \dt lista las 7 tablas
□ \dv muestra la vista records
□ El downgrade probado y funcionando
□ Modelos y migración commiteados juntos
□ Las líneas de Docker, Flask y SQLAlchemy señaladas en el código real
```

**Si los primeros cinco están, el sábado arranca en la línea de salida** — que es exactamente para lo que E04 existió.

---

# 📅 Lo que espera el sábado

```
E05 fase B   Transcribir la SEMANA 15 (no la 29) y cargarla · 2.4 pom
             S15 tiene "- [ ]"/"- [x]" · S29-30 usan "*" sin estado
             Leaks reales: Apex Lab ×5 · Insights ×6

E05 fase C   La query de score — LA ESCRIBES TÚ, sale de la vista, filtra por user_id
E05 fase D   EXPLAIN + índice justificado + H4 (query de subárbol) · 🤝 sesión conmigo

Estudio      Temas 8, 9 y 10
```

**El sábado es el día que decide el goal.** Hoy existe para que mañana no tenga que improvisar.
