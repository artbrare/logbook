# 📊 Semana 01 — S1 · Núcleo de datos de Loop (Jul 27 – Ago 2) · **v4**

> **De dónde venimos:** Sprint 0 fue ensayo del sistema documental — sirvió para validar el formato,
> no para producir software. **Aquí empieza el programa de verdad.**
> **Deuda que entra:** ninguna. Es la semana 1.
> **Fase:** S1 de S1–S3 · Loop debe estar en producción el **15 de agosto**.
> **Estado del proyecto:** `loop` no existe todavía. Al domingo debe guardar y consultar datos reales.

---

# 🎯 GOAL DE LA SEMANA

> **Al domingo 2 de agosto, Loop guarda y consulta datos reales — modelo diseñado por mí en
> PostgreSQL, una semana real de mis datos adentro, y una query mía de score ponderado con su
> `EXPLAIN` leído — y todo se construyó con un flujo de Claude Code que yo armé: CLAUDE.md propio,
> plan mode, una skill y un subagente en uso real.**
>
> **Capacidad 1 — qué construyo:** sé diseñar un esquema relacional para un dominio real, con sus
> recurrencias y agregaciones, y consultarlo con SQL que entiendo línea por línea, no que me generaron.
>
> **Capacidad 2 — cómo lo construyo:** opero Claude Code como un tech lead opera a su equipo —
> planeo con detalle, delego con contexto y razones, y valido todo lo que regresa.

*El goal tiene dos mitades a propósito: el artefacto y el método. Un Loop funcionando construido a
prompts sueltos reprobaría la semana igual que un Loop sin terminar. La retro no pregunta si hiciste
las tareas: pregunta si adquiriste las dos capacidades.*

*SQL es la primera pieza del track de lenguajes — la habilidad más subestimada del stack. Claude Code
es la de mayor apalancamiento: cada primitiva que domines se paga en las 25 semanas restantes.*

---

# 🏆 RESUMEN EJECUTIVO

## 💎 Lo más importante (orden de impacto)

### 1. ⭐⭐⭐⭐ 🧠 **EL MODELO DE DATOS, DISEÑADO POR TI**
El corazón del goal y de la semana. Si el modelo sale mío, todo lo que se construya encima es mío
también y la capacidad no se adquiere. Los tres problemas duros: contador semanal alimentado por
tasks diarias · recurrencia sin duplicar filas · dónde vive el peso. **Es del miércoles y no se mueve.**

### 2. ⭐⭐⭐⭐ 🗄️ **TUS DATOS REALES ADENTRO**
Importar las semanas 26, 29 y 30 de Apple Notes no es un ejercicio: es lo que convierte a Loop en
algo que puedes usar y lo que hace que la query de score tenga sentido contra datos verdaderos.

### 3. ⭐⭐⭐ 🔍 **LA QUERY DE SCORE, CON SU `EXPLAIN` LEÍDO**
Aquí es donde la semana deja de ser CRUD y se vuelve ingeniería: agregación ponderada, índices,
plan de ejecución. Es el entregable que más te acerca al goal.

### 4. ⭐⭐⭐⭐ 🛠️ **CLAUDE CODE COMO HERRAMIENTA PROPIA (E02)**
La épica de mayor apalancamiento del semestre completo: cada primitiva que domines se paga en las
25 semanas restantes. No es "practicar Claude Code" — es salir con CLAUDE.md, una skill, un comando
y un subagente que uses de verdad. Si esto se pospone, cada semana futura rinde menos de lo que podía.

### 5. ⭐⭐ 🎙️ **EL FLUJO DE VOZ (E01)**
Va el lunes porque desbloquea ~3 h semanales para el resto del programa. Timebox duro: 30 min.

## 📊 Números clave
```
Presupuesto: 44.3 pom escritorio (~18.5 h) + 9.6 pom de bici fija (mar y jue, 2 h c/u)
  Total 53.9 pom ≈ 22.5 h · sin colchón
Libro activo: A Philosophy of Software Design — pp. 0/190 · Libros terminados: 0/6
Épicas: 6 · 35.7 pom · Proyectos shippeados: 0/5 · Semanas completadas: 0/26
🎯 PRÓXIMOS: Loop en producción 15 ago · Tokio 16-28 ago · 1ª evaluación oral dom 30 ago
```

## 🎯 Épicas de la semana
| ID | Épica | Estimado | Real | Estado | Doc |
|---|---|---:|---:|:---:|---|
| **E01** | Flujo de voz para trabajar desde la bici | 3.6 pom | | 🔲 | `epicas/E01-flujo-de-voz.md` |
| **E02** | Claude Code experto (parcial — fase D a S2) | 5.6 pom | | 🔲 | `epicas/E02-claude-code.md` |
| **E03** | Diseño de Loop v1: spec → modelo → ADR de stack | 7.8 pom | ~7.0 | 🔄 | `epicas/E03-diseno-loop-v1.md` |
| **E04** | Fundación del repo: repos · scaffold · plan mode | 4.5 pom | | ✅ | `epicas/E04-fundacion-repo.md` |
| **E05** | Núcleo de datos: schema · migración · seed · query | 10.8 pom | | 🔲 | `epicas/E05-nucleo-de-datos.md` |
| **E06** | Arranque de Tableau (recortada a 2 sesiones) | 2.4 pom | | 🔲 | `epicas/E06-tableau-arranque.md` |

> **Total épicas: 34.2 pom** *(E02 baja de 7.1 a 5.6: fase D movida a S2)* de los 53.9 de capacidad. El resto es lectura (8.4), cierres (2.8),
> domingo (4.6: LeetCode, gates, retro, S02) y bici no-épica (2.4: digest y video).
> E04 y E05 se generan al desbloquearse: su checklist depende de decisiones que aún no existen.

## 🎯 Targets no negociables
- [ ] **T1 (w5):** modelo de datos propio, defendido bajo interrogatorio, convertido en schema corriendo contra Postgres local
- [ ] **T2 (w5):** datos reales adentro y consultables — **semana 15 transcrita** *(no hay parser: `spec-v1.md` §10)*
- [ ] **T3 (w5):** query de score ponderado escrita por mí, con su `EXPLAIN` revisado y el índice justificado
- [ ] **T4 (w5):** herramientas propias de Claude Code en uso — CLAUDE.md, una skill, un slash command, un subagente

## 🎯 Cuotas
| Cuota | Piso | Plan | Real | Se alimenta de |
|---|---:|---:|---:|---|
| 🍅 Pom ingeniería (sin Tableau) | 26 | 29.5 | | Días 1–7, primer bloque del día |
| 🍅 Pom totales escritorio | 41 | 44.3 | | Ingeniería + Tableau + lectura + cierres |
| 🚴 Pom modo bici | 8 | 9.6 | | **Mar y jue fijos, 2 h c/u** |
| 📖 Sesiones de lectura | 5 | 7 | | Slot de 30 min |
| 📄 Páginas (APoSD) | 60 | 80 | | Mismo slot |
| 🤝 Sesiones formales con Claude | 3 | 3 | | Requerimientos (mar) · interrogatorio (jue) · diseño SQL (sáb) |
| 🛠️ Artefactos de Claude Code | 3 | 4 | | CLAUDE.md ×2 · skill · slash command |
| 🎬 Videos con takeaways | 1 | 2 | | Modo bici |
| 📊 Sesiones de Tableau | 2 | 2 | **1** | **Jue ✅ · Vie** *(las de lun y mié se perdieron)* |
| 🧮 LeetCode | 3 | 3 | | Domingo |
| 💾 Commits en `loop` | 10 | 15 | | Cierre diario |
| 📰 Digest + 3 takeaways | 1 | 1 | | Jueves (bici) |

---

# 📋 PARTE 1 — OVERVIEW

## 🗓️ Calendario

| Día | Escritorio | 🚴 Bici (fija mar/jue) | Pom |
|-----|---|---|----:|
| **LUN 27** | 🎙️ Voz + 🏗️ repo + 🛠️ **CLAUDE.md** + 📊 Tableau 1 | — | 6.3 |
| **MAR 28** | 🤝 **Requerimientos de Loop** + 🛠️ **skill + slash command** | ✅ **E01 validación · dictar ADR-001 · metodología P→I→V** | 6.0 + 4.8 |
| **MIÉ 29** | 🧠 **Modelo (solo)** + 🏗️ scaffold en **plan mode** + 📊 Tableau 2 | — | 7.3 |
| **JUE 30** | 🤝 **Interrogatorio** + 🛠️ **subagente revisor** | ✅ **digest · video CC · diseñar parser y query** | 5.7 + 4.8 |
| **VIE 31** | 🗄️ Schema v0 + migración + 📊 Tableau 3 | opcional | 6.2 |
| **SÁB 1** | 📥 Import real + 🔍 **query + sesión de diseño SQL** | opcional | 6.6 |
| **DOM 2** | 🧮 LeetCode + 🚪 gates + 🔍 retro + 📅 generar S02 | opcional | 6.2 |

> **Escritorio 44.3 pom (~18.5 h ≈ 2.6 h/día) + bici 9.6 pom fijos.** Nada se recortó: Claude Code
> entró completo y Tableau conservó sus 3 sesiones. El ADR, el digest, la metodología y el diseño
> del parser viven en la bici — trabajo nativo de voz que ya no compite por teclado.
> **El miércoles es el día más cargado (7.3). Si algo se cae, se cae Tableau 2 — nunca el modelo.**

## ⚠️ Compromisos inamovibles
```
1. 🧠 EL MODELO DE DATOS LO HACES TÚ, SIN MÍ. Es la línea que separa esto del vibe-coding.
2. ⏰ LO SAGRADO VA PRIMERO. El bloque cognitivo duro al arrancar el día, nunca después de la chamba.
3. ⏱️ EL TIMEBOX ES EL TIMEBOX. El rabbit hole de hoy es el bloque robado de mañana.
   E01 tiene timebox especial: 30 min para fases A–C, pase lo que pase.
4. 🚪 NADA SE MERGEA SIN PODER EXPLICARLO en voz alta, sin ver el código.
5. 📅 EL DOMINGO SE GENERA LA SEMANA 02. Pase lo que pase con el resto.
6. 🔒 SCOPE CONGELADO. Idea nueva → se anota, se decide en la retro. Nunca a media semana.
```

---

# 📅 PARTE 2 — DÍA POR DÍA
*Cada día es una secuencia de bloques en orden de ejecución. `- [ ]` pendiente → `- [x]` hecho.
Sin estados intermedios. El orden importa: lo cognitivo duro va arriba.*

---

## Día 1 — LUN 27 JUL · Voz y memoria del proyecto
**Foco:** que las herramientas existan antes de construir con ellas. · **Plan 6.5 pom** · **real 2.0** ❌

### 🟢 Bloque 1 · E01 fases A–C · 1.2 pom → **real 1.0** ✅
> ⏱️ Timebox duro 30 min · cerrado en 25

- [x] `brew install --cask my-monkeys/tap/opensuperwhisper`
- [x] Descargar motores Whisper y Parakeet desde la app
- [x] Permisos de micrófono **y accesibilidad** en Ajustes del Sistema
- [x] AirPods como input · trigger **⌥** · modo **toggle**
- [x] Calibración: español fijo + Dictionary cargado con la jerga → 0 errores
- [ ] ⚠️ Deuda: A/B contra Parakeet → se paga mañana en la bici (bloque 2 de fase D)

### 🟢 Bloque 2 · E04 fase A — Fundación: dos repos · 1.0 pom → **real ___**
> **Corregido 27 jul:** son **dos** repos, no uno. Ciclos de vida distintos — el programa dura 26
> semanas y Loop vive S1–S3; en septiembre los docs semanales quedarían enterrados en un repo
> archivado. Además `loop` puede volverse portafolio y los docs del programa traen autoevaluación
> y leaks. Decisión de Arturo → se registra como `logbook/adr/adr-001-arquitectura-docs.md` (dictado mañana).

```
~/dev/tech-lead-path/        ← carpeta contenedora, no es repo
├── logbook/                 ← repo 1 · privado · el programa
└── loop/                    ← repo 2 · el producto
```

**2a · `logbook` — el programa (privado)**
- [x] `git init -b main` + `mkdir -p semanas epicas adr templates .claude/skills .claude/commands`
- [x] Mover `semana-01.md` → `semanas/` · las 4 épicas → `epicas/` · los 2 templates → `templates/`
- [x] `README.md` + `.gitignore`
- [ ] `git add . && git commit -m "chore: bootstrap logbook"` ← **verificar con `git log --oneline`**

**2b · `loop` — el producto**
- [ ] `git init -b main` + `mkdir -p docs/adr`
- [ ] `.gitignore` de Node + `.env`
- [ ] `README.md` — qué es Loop, estado "en diseño, sin código aún", stack congelado y pendientes
- [ ] `docs/adr/README.md` *(git no versiona carpetas vacías — sin esto la carpeta no entra al commit)*
- [ ] `git add . && git commit -m "chore: bootstrap loop repo"`

> 📌 **Regla de reparto:** ADR de producto (ORM, hosting, modelo) → `loop/docs/adr/`.
> ADR del programa (rituales, arquitectura de docs, alcance) → `logbook/adr/`.

### 🔲 Bloque 3 · E02 fase A — CLAUDE.md · 1.5 pom
> El bloque de mayor apalancamiento del día. Aquí sí hay que pensar.

**3a · `~/.claude/CLAUDE.md` global — completo hoy**
- [ ] Leer la doc oficial de memoria de Claude Code: jerarquía y precedencia
- [ ] Bloque 1 — cómo respondo contigo: idioma, longitud, preámbulo o no
- [ ] Bloque 2 — convenciones que cargas: estilo de commits, branches, naming
- [ ] Bloque 3 — **qué NUNCA hacer** *(el de mayor retorno: repasa tus sesiones desde junio y anota qué te hizo decir "no, eso no")*
- [ ] Bloque 4 — contexto de trabajo: stack diario, nivel al que te hablo

**3b · `logbook/CLAUDE.md` — el programa**
- [ ] Qué es el programa y qué NO se hace aquí (no se escribe código de producción)
- [ ] Convenciones de los docs: numeración ISO, umbral de épica, formato de bolitas
- [ ] Dónde vive cada cosa: `semanas/` `epicas/` `adr/` `templates/`
- [ ] Que las épicas se generan al desbloquearse, nunca antes
- [ ] Regla de reparto de ADRs entre este repo y `loop`

**3c · `loop/CLAUDE.md` de proyecto — v0 esqueleto**
- [ ] Qué es Loop, dos líneas
- [ ] Stack congelado: Next.js + TypeScript + PostgreSQL · web desktop-first, keyboard-driven
- [ ] Orden de diseño: requerimientos → modelo → stack
- [ ] Marcar explícito: ⚠️ PENDIENTE ADR-001 → ORM, hosting, auth, testing
- [ ] Marcar explícito: ⚠️ PENDIENTE scaffold → estructura de carpetas, comandos

**3d · Validación (es el DoD, no te la saltes)**
- [ ] Abrir sesión nueva de Claude Code en `loop` y verificar que el contexto se aplicó **sin repetirlo**
- [ ] Filtro final: pasar cada línea por *"¿un dev nuevo lo deduciría del repo en 5 min?"* Si sí → se borra

### 🟢 Bloque 4 · E06 fase A — Tableau sesión 1 · 1.2 pom → **real 2.0** ✅ *(+43%)*
- [x] Conectado a Greenstone — **plan A, JDBC** · Custom SQL con la query de producción
- [x] Extracto, no conexión viva · 33 campos · 305,141 filas
- [x] Vista `Timing status` construida · guardada en `Documentos/Greenstone/Tableau/` · 🔒 nada se publicó
- [x] **Hallazgos:** 27% de tickets no medibles (entrada manual, no error) · `Out of guard` ≈ 0.2% → datos limpios · piso físico en 6-10 min
- [x] Pregunta escrita · referencia: **Load Times 2.0 (Luzmo)** → el viernes se reproducen **cifras**, no diseño
- [ ] ⚠️ Buscar el `.hyper` del extract y confirmar que está fuera de iCloud — **ese sí tiene datos de la empresa**

### 🔲 Bloque 5 · Personal · 1.2 pom
- [ ] Lectura 30 min — *A Philosophy of Software Design*, caps 1–2
- [ ] Registro diario · hábitos · suplementos
- [ ] Entrenos del día

### 🔲 Bloque 6 · Cierre · 0.4 pom
- [ ] Llenar pom reales por bloque y el análisis del día
- [ ] Marcar checkboxes
- [ ] `git commit` del día

> 🔍 **Análisis del día:** E01 cerrada bajo timebox (1.0 vs 1.2) y dos repos en pie con commits limpios.
> **El bloque duro arrancó a las 23:30** — violación directa del compromiso #2 ("lo sagrado va primero").
> Deriva de tiempo: 5 intercambios nombrando un repo privado, trabajo cómodo desplazando al incómodo.
> Es el leak documentado operando en día 1. **Corrección: el bloque 1 del martes es CLAUDE.md, antes de todo.**
> Slip: 4.5 pom — CLAUDE.md ×3, Tableau sesión 1, lectura.

---

## Día 2 — MAR 28 JUL · Requerimientos y primeras herramientas
**Foco:** saber qué es Loop, y construir la primera herramienta reutilizable.
**Plan 6.6 escritorio + 2.4 bici** · **real ~9.4** ✅ *(confirmar minutos exactos en el cierre)*
> ⚠️ **Bici recortada a 1 h** (2.4 pom, no 4.8). Cae el dictado de notas del modelo — se recupera
> el miércoles, que es su bloque natural. **Prioridad si el día se aprieta: bloques 1, 2 y 5.**
> El bloque 3 es el primero que se mueve al miércoles.

### 🟢 Bloque 1 · CLAUDE.md global · 0.6 pom → **real ___** ✅
> Deuda de ayer, pagada primero. **Hallazgo:** Arturo nunca había usado Claude Code en serio,
> así que no hay fricciones documentadas. El archivo sale como **v0 con defaults marcados a prueba**,
> no como reglas inventadas. La sección *Fricciones observadas* queda vacía a propósito y se
> alimenta en el momento en que algo moleste — no al final del día.

- [x] `~/.claude/CLAUDE.md` creado *(la carpeta ya existía; solo faltaba el archivo)*
- [x] Contexto · Cómo respondes · Nunca · Porque estoy aprendiendo · Fricciones (vacía)
- [x] **Validado en sesión nueva:** recitó las reglas sin que se las repitiera
- [ ] 📌 Revisar en 2 semanas: borrar los defaults 🔵 que no hayan servido

### 🟢 Bloque 2 · E03 fase A — Requerimientos · 2.0 pom ✅
- [x] **D5 respondida:** 4 semanas reales (12, 15, 29, 30). Dos formatos distintos; S29-S30 sin estado de completado — bloquea el parser del sábado
- [x] Inventario honesto del sistema actual: qué funciona, qué falla, con evidencia de las semanas 26/29/30
- [x] Los 4 tipos de task: hábito · one-off · contador semanal · meta
- [x] Categorías anidadas (**3 niveles, no 2**), pesos y scoring
- [x] Leaks con **4 salidas** · la 4ª es reagendar al primer bloque
- [x] 🚫 NO-features · **el importador sale sin fecha de reingreso**
- [x] `spec-v1.md` commiteada (a58d8fd)

### 🟢 Bloque 3 · E02 fase C — Skill · ~2.0 pom ✅
- [x] Leída la doc de Skills
- [x] **Skill `nueva-epica`** en `logbook/.claude/skills/` — se activó sola y generó **E04**
- [x] Usada 1 de las 2 veces que pide el DoD · la 2ª será E05 el jueves
- [x] 📌 **Hallazgo:** los slash commands se fusionaron con skills. `commands/x.md` y `skills/x/SKILL.md` producen el mismo `/x`; el frontmatter controla quién invoca. **Ya no son dos primitivas** — el gate del domingo cambia de pregunta.

### 🟢 Bloque 3b · Infraestructura · ~1.2 pom ✅
- [x] **GitHub:** `logbook` y `loop` creados privados y pusheados
- [x] `logbook/CLAUDE.md` — convenciones, umbral de épica, reparto de ADRs
- [x] `loop/CLAUDE.md` — stack de ADR-001, principio multiusuario, frontera ORM/Core
- [x] `.gitignore` de `loop` extendido a Python + `.env`

### 🟢 Bloque 4 · Personal · 1.2 pom ✅
- [x] Lectura 30 min — APoSD caps 1–2
- [ ] Registro · hábitos · entrenos

### 🟢 Bloque 5 · 🚴 BICI · 2.0 pom reales ✅
**5a · E01 fase D — validación · ~0.3 pom**
- [x] ⌥ alcanzable pedaleando y encontrado a ciegas
- [x] **A/B Whisper vs Parakeet:** 4 errores cada uno. **Whisper gana por puntuación** — Parakeet no puso ninguna
- [x] Alucinación **reproducida**: 30 s de silencio → repitió la última frase 3 veces
- [x] **Con disciplina de tap-off, cero frases inventadas en el dictado real** → la mitigación funciona

**5b · E03 fase C — ADR-001 · ~1.7 pom**
- [x] Dictado completo. **Creció de 4 a 6 decisiones** durante la sesión
- [x] Whisper falla solo en sustantivos técnicos; la prosa en español salió limpia

⬛ **5c · Metodología P→I→V — no se hizo.** El ADR ocupó la hora completa. Reasignación consciente al entregable que bloquea el miércoles, no slip.

### 🔲 Bloque 6 · Cierre · 0.4 pom
- [x] Transcripción revisada: 4 palabras por párrafo (solo jerga técnica), **cero frases inventadas** en dictado real
- [x] **E04 generada** por la skill
- [ ] Confirmar poms reales por bloque
- [ ] Commit y push en los dos repos

> 🔍 **Análisis del día:** El bloque 1 (deuda de ayer) arrancó primero — corrección aplicada, y el
> día pasó de 2.0 a 4.6 pom. **La diferencia no fue esfuerzo: fue el orden.**
> **Tres decisiones de arquitectura en un día:** fuera Next.js → API separada → backend en Python.
> Las tres defendibles, pero el ADR-001 pasó de 4 a 6 decisiones y consumió la bici entera.
> **El stack ya no se toca esta semana.**
> Hallazgo que corrige el programa: *Python de servidores* (Flask, nginx, deploy) no estaba cubierto
> en ninguna fase — S12-S15 es Python de datos. Era un hueco real, no un adelanto.

---

## Día 3 — MIÉ 29 JUL · El modelo y el plan
**Foco:** el entregable más importante de la semana, hecho solo. · **Plan 7.2 pom** · **real 10.0** ✅ *(+39%)*
> ✂️ **Corte aplicado al arrancar:** Tableau sesión 2 salió (8.8 → 7.2) porque E04 se re-estimó
> de 2.8 a 4.5 pom. Se decidió antes de empezar, no a media tarde.

### 🟢 Bloque 1 · E03 fase B — Modelo de datos · 1.6 pom · 🚫 SIN CLAUDE ✅
- [x] Entidades, campos y relaciones — 7 tablas
- [x] **P1 contador semanal** → resuelto quitando objetivos semanales (D5)
- [x] **P2 recurrencia** → `tareas_programadas` (la regla, sin fecha) vs `registros_diarios` (el hecho)
- [x] **P3 dónde vive el peso** → en la tarea. *"La dificultad no es propiedad de la tarea, es propiedad de la tarea en un momento"*
- [x] **P4 recálculo del histórico** → el peso se copia al registro; el pasado no se reescribe
- [x] **P5 duración y hora fija** → no se agregan. Insights por IA, no planificador rígido
- [x] **P6 no se hizo vs no había dato** → `status` enum de 3 estados
- [x] **P7 `user_id` e índices** → `(usuario_id, fecha)`, igualdad antes que rango
- [x] Anidamiento de 3 niveles con CHECK · contadores >100% permitidos
- [x] `docs/modelo-arturo.md` escrito

### 🟢 Bloque 2 · E03 fase D — Interrogatorio · 1.0 pom · 🤝 adelantado del jueves ✅
- [x] 8 decisiones difíciles con formato completo (problema · opciones · decisión · qué pierdo · qué lo revertiría)
- [x] **Tres cambios por argumento:** FK `tarea_programada_id` nullable · `boolean` → `status` enum · objetivos semanales fuera
- [x] 4 huecos nombrados: H1 derivar vs escribir · H2 validar `sesiones_pomodoro` · H3 zonas horarias · H4 query de subárbol
> La fase D de mañana baja de 2.2 a 1.2 pom.

### 🟢 Bloque 3 · E04 fase B — Scaffold en plan mode · 2.5 pom ✅
- [x] `docs/plan-scaffold-arturo.md` escrito ANTES de abrir plan mode — 8 decisiones + **14 preguntas abiertas**
- [x] **Vuelta 1:** prompt de 4 palabras → plan de nivel senior *(el dato, ver bloque 5)*
- [x] **Vuelta 2:** con el plan propio como contexto → 4 correcciones aplicadas
- [x] `web/` Vite + React + TS · `api/` Flask con factory, blueprint, `/health` y `/health/ready`
- [x] SQLAlchemy: engine global, sesión por request, cierre en `teardown_appcontext`
- [x] CORS a mano en `after_request` — sin librería
- [x] Alembic cableado a `app.config`, cero migraciones
- [x] Frontend pinta `/health` → **`api: ok`** · consola sin errores de CORS
- [x] 6 tests pasan · marker separa unit de integración

**Las 4 correcciones que salieron de tu criterio, no del repo:**
```
D3  requirements.txt → pyproject.toml + uv.lock
D5  una clase Config → clases por ambiente
D4  /health/db → /health/ready (convención liveness/readiness de k8s)
CORS  flask-cors → headers a mano en after_request
```

### 🟢 Bloque 4 · E04 fase C — Contenedores y persistencia · 1.0 pom ✅
- [x] `Dockerfile` a mano: `COPY pyproject.toml uv.lock` **antes** del código · usuario no-root · gunicorn en `0.0.0.0:8000`
- [x] `compose.yml` con **volumen nombrado** `loop_pgdata` y `healthcheck` + `depends_on: service_healthy`
- [x] `/health` responde 200 desde el contenedor
- [x] 🔒 **Persistencia verificada:** fila insertada → `make down && make up` → la fila sigue ahí
- [x] Liveness vs readiness probado apagando Postgres: `/health` 200, `/health/ready` 503, vuelve a 200
- [x] `loop/CLAUDE.md` con estructura real, tabla de comandos y hueco de auth

### 🟢 Bloque 5 · E02 fase B — El dato de plan mode · 0.5 pom ✅
- [x] **EL DATO:** un prompt de **4 palabras** ("haz el scaffold de loop") produjo un plan de nivel senior — citando `CLAUDE.md`, `spec-v1.md`, `adr-001-stack.md` y `modelo-arturo.md` por sección, verificando que el puerto 5000 está ocupado en esta Mac, y proponiendo `repositories/` vs `queries/` para volver física la regla ORM/Core.
- [x] **Conclusión que sostiene la metodología del semestre:** el apalancamiento no está en escribir prompts largos, está en **el contexto persistente del repo**. El trabajo de lunes y martes se pagó el miércoles, medido.
- [x] Lo que el contexto NO podía saber salió del plan propio: las 4 correcciones de arriba
- [ ] ⏭️ **ADR-003 (librería de fechas)** pasa al jueves — depende de H3 (zonas horarias), sin resolver

### ⬛ Bloque 6 · Tableau sesión 2 — CORTADO
> Salió al arrancar el día por la re-estimación de E04. **La semana cierra en 2/3 de Tableau.**

### 🟢 Bloque 7 · Personal + cierre · 1.6 pom ✅
- [x] Lectura 30 min — APoSD caps 3–4
- [x] Registro · hábitos · entrenos
- [x] 10 commits por pieza + push en los dos repos
- [x] CORS verificado: `CORS_ORIGINS` es lista, no string. Sin bug de substring
- [x] Poms reales: **10.0**

> 🔍 **ANÁLISIS DEL DÍA**
> **Productivo, pero sin consolidación.** Arturo lo reporta así: *"bastante productivo, solo creo
> no hubo mucho aprendizaje ya que los conceptos son muy complejos para mí"*. El diagnóstico es
> correcto y hay que separarlo en dos: **el modelo de datos sí se aprendió** (se diseñó solo,
> se defendió, se cambiaron tres decisiones por argumento). **La infraestructura no** — se aprobó,
> no se construyó, y son ~9 conceptos nuevos en un día: contenedores, capas, volúmenes, app factory,
> blueprints, WSGI, engine/sesión, CORS, migraciones.
>
> **Causa raíz: la culpa es del plan, no de la comprensión.** El miércoles metía el modelo de datos
> Y el scaffold completo Y los contenedores. Nueve conceptos nuevos no se consolidan el mismo día
> que se ven, sin importar quién los vea.
>
> **Desviación de estimado: +39%** (7.2 → 10.0). Segunda medición del programa.
>
> **Corrección concreta:** se generó `logbook/estudio/semana-01.md` con los 9 temas explicados
> desde cero y ~60 preguntas de recall. **El jueves en la bici se repasa, no se agrega material
> nuevo.** Los gates del domingo miden si funcionó.

---

## Día 4 — JUE 30 JUL · Cerrar el diseño y consolidar
**Foco:** cerrar E03 con las decisiones abiertas resueltas, para que el viernes se escriba schema
y no se decida schema. · **Plan 6.0 escritorio + 2.4 bici** · real: ___
> 📄 Detalle completo en `logbook/dias/jueves-30-jul.md`
> 🔄 **Replanificado:** el interrogatorio del modelo se adelantó al miércoles, así que la fase D baja
> de 2.2 a 1.0. Entra un bloque nuevo de **consolidación** como corrección al análisis del miércoles
> (~17 conceptos nuevos en un día, sin tiempo de fijarlos). Tableau entra: es deuda del lunes.

### 🔲 Bloque 1 · Decisiones abiertas · 1.0 pom · 🤝 conmigo
> Tres de las cuatro **bloquean el viernes**. Sin ellas el schema se escribe a ciegas.

- [x] **H1 · `no_completado` se DERIVA, no se escribe** → `pendiente` + `fecha < hoy`
  > **Razón de Arturo:** es una línea, no cuesta nada, y se autocorrige — una tarea que se olvidó
  > de actualizar queda resuelta sola sin importar cuánto tiempo pase. Sin job, sin cron, sin infra.
  > **Qué cuesta:** toda query que filtre por estado tiene que incluir la derivación. `WHERE status
  > = 'no_completado'` no funciona solo. **Mitigación: la derivación se escribe UNA vez en una vista**,
  > y todas las queries leen de la vista, no de la tabla.
  > ⚠️ **Depende de H3:** "fecha < hoy" — ¿hoy según qué zona horaria?
- [x] **H2 · `sesiones_pomodoro` ENTRA al schema**
  > **Razón de Arturo:** se necesita trackear a qué hora se trabajó, y **hacerlo al principio es
  > mejor que hacerlo al final.** El argumento fuerte detrás de eso: el schema se puede migrar
  > después, pero **los datos que no capturas hoy no se pueden reconstruir.** Si arrancas con un
  > contador, los meses de historia sin horas se pierden para siempre.
  > **Qué cuesta:** una escritura por pomodoro en vez de un incremento · será la tabla más grande
  > de la base · obliga a que la UI tenga "empezar pomodoro", no solo "marcar hecho".
  > **Consecuencia:** `num_pomodoros_terminados` no existe — se cuenta desde esta tabla.
- [x] **H3 · instante + zona IANA + fecha civil local**
  > `usuarios.timezone` (IANA) · `registros_diarios.fecha` = fecha **local** del usuario ·
  > `sesiones_pomodoro.inicio` (timestamptz) + `tz` (IANA al momento).
  > **Por qué:** `timestamptz` guarda el instante pero **no recuerda dónde ocurrió**, y lo que
  > importa es *"a las 5am aquí o en China es el mismo hábito"*. Zona IANA y no offset, porque
  > `dias_repeticion` son reglas a futuro y los horarios de verano mueven el offset.
  > **La zona se copia al evento**, mismo patrón que el peso: cambiar el perfil no reinterpreta el pasado.
  > ⚠️ **Corrige H1:** `fecha < (now() AT TIME ZONE u.timezone)::date`, no `CURRENT_DATE`.
- [ ] **H4 · query de subárbol** → **movida al viernes.** Afecta un índice, no el schema
- [ ] **A1 · el engine global** *(no bloquea — es material del gate del domingo)*

### 🟢 Bloque 2 · E03 fase E — Cerrar E03 · 1.0 pom ✅
- [x] `adr-002-modelo.md` — lo defendido sin cambios, los 3 cambios del interrogatorio, y H1/H2/H3
- [x] `adr-003-fechas.md` — **Temporal** en el frontend, `zoneinfo` en el backend, `TZ=UTC` en el proceso
- [x] `modelo-arturo.md` v3 — vista `records`, `usuarios.timezone`, `sesiones_pomodoro.tz`
- [x] **E05 generada con la skill** → segundo uso · **cierra el DoD de E02 fase C**
- [x] La skill detectó sola la contradicción entre el doc semanal y la spec sobre el parser
- [ ] ⚠️ **Commit y push pendientes** en `loop` y `logbook`

> 🔍 **Hallazgo del segundo uso de la skill:** cachó que el doc semanal decía "parser" y la spec §10
> decía "no hay importador", y preguntó cuál gana en vez de elegir sola. También encontró que el DoD 3
> de E05 estaba mal escrito — *"el leak aparece"* pasaba igual con falsos positivos; ahora dice
> *"y nada más aparece con ellos"*. **Ese error era mío, no de los datos.**

### 🔲 Bloque 3 · Consolidación · 1.2 pom · **bloque nuevo**
> Corrección al análisis del miércoles. **No es repaso pasivo:** doc de estudio en una ventana,
> código en otra, y por cada concepto se busca **la línea real en el repo**.

- [ ] **Tema 4 · Docker** → `api/Dockerfile` y `compose.yml`: capas, `--bind 0.0.0.0`, volumen nombrado
- [ ] **Tema 5 · Servidor web** → `api/app/__init__.py`: factory, blueprint, los dos hooks
- [ ] **Tema 6 · SQLAlchemy** → `api/app/db.py`: engine (una vez) vs sesión (por request), `close()` sin `commit()`
- [ ] Anotar **qué concepto no se encontró en el código** — ese es el que no se entendió

### 🔲 Bloque 4 · E06 fase A — Tableau sesión 1 · 1.2 pom · **deuda del lunes**
> Desktop **Free Edition** · 🔒 guardado local, nunca se publica

- [ ] Conectar **Greenstone** con la cadena de conexión
- [ ] Identificar dimensiones vs medidas, filas vs columnas, marcas
- [ ] Construir una vista simple: serie de tiempo o ranking
- [ ] **Anotar la pregunta concreta** que el dashboard responderá
- [ ] Elegir qué dashboard de Greenstone replicar — tu ground truth
- [ ] Anotar qué se aprendió *(sesión sin nota no cuenta)*

### 🔲 Bloque 5 · Personal · 1.2 pom
- [ ] Lectura 30 min — APoSD
- [ ] Registro · hábitos · entrenos

### 🔲 Bloque 6 · 🚴 Estudio completo + examen
> **Los nueve temas con sus preguntas, y el examen de 35 el mismo día.** Decisión de Arturo.
> Ciclo por tema: leer → **cerrar el doc** → contestar dictando → comparar → anotar lo que falló.

- [ ] Temas 1-9 leídos, cada uno con sus preguntas contestadas a ciegas
- [ ] **Examen de 35 preguntas** de corrido, sin abrir la rúbrica hasta terminar
- [ ] Comparar contra la rúbrica y contar aciertos
- [ ] Lectura APoSD
- [ ] Mandar las respuestas → dirigen el bloque de consolidación

> ⏱️ ~2h15 de material contra 1h de bici. Lo que no quepa pedaleando se termina en escritorio.

### 🔲 Bloque 7 · Cierre · 0.4 pom
- [ ] Poms reales + análisis del día
- [ ] Commit y push en los dos repos
- [ ] Verificar que **E05 existe** — el viernes arranca de ahí

> 🔍 **Análisis del día:** {…}

---

## Día 5 — VIE 31 JUL · El modelo deja de ser papel
**Foco:** schema corriendo contra Postgres. · **Plan 6.2 pom** · real: ___
*Subtasks finas se llenan el jueves: dependen del modelo y del ORM, que aún no existen.*

### 🔲 Bloque 1 · E05 fase A — Schema v0 · 2.0 pom
- [ ] 🚨 **La PRIMERA línea de la migración: `CREATE EXTENSION IF NOT EXISTS vector`**
  > **Por qué, y no se puede olvidar.** El scaffold del miércoles probó que `pgvector` está en la
  > imagen y **luego borró la extensión a propósito**. Si la base de desarrollo tiene estado que
  > ninguna migración produjo, la migración funciona local y **falla en una base limpia** — o en
  > producción, o en el clon de otra persona. Paridad dev/prod: la migración crea la extensión.
- [ ] Traducir el modelo defendido a schema — `docs/modelo-arturo.md` es la fuente
- [ ] Nombres de tablas y columnas en **inglés** (`users`, `scheduled_tasks`, `completed_at`)
- [ ] Decidir **H2**: ¿entra `sesiones_pomodoro` al schema? Es propuesta de Claude, no diseño propio
- [ ] Decidir **H1**: `no_completado` se escribe con un job, o se deriva (`pendiente` + `fecha < hoy`)

### 🔲 Bloque 2 · E05 fase A2 — Migración · 1.4 pom
- [ ] Primera migración corriendo contra Postgres local
- [ ] Verificar que las tablas existen y commitear

### 🔲 Bloque 3 · E06 fase C — Tableau sesión 3 · 1.2 pom
- [ ] Layout con contenedores — ref. WW **2020 W53**
- [ ] Componer 2-3 vistas + un filtro o acción
- [ ] **Guardar local (.twbx)** y anotar la ruta
- [ ] 3 líneas: qué pregunta responde y qué te sorprendió del dato

### 🔲 Bloque 4 · Personal + cierre · 1.6 pom
- [ ] Lectura 30 min · registro · entrenos
- [ ] Cierre: log + commit + checkboxes
- [ ] 📞 Preguntar D6 (licencias Tableau) y D7 (keys eLearning)

> 🔍 **Análisis del día:** {…}

---

## Día 6 — SÁB 1 AGO · Tus datos y la query
**Foco:** el día que define si el goal se cumple. · **Plan 6.6 pom** · real: ___
*Subtasks finas se llenan el viernes.*

### 🔲 Bloque 1 · E05 fase B — Seed con datos reales · 2.4 pom
> ⚠️ **NO se escribe parser** (`spec-v1.md` §10). Es transcripción a un archivo estructurado.
> 🔴 **Corregido 30 jul: la semana es la 15, no la 29.**
> S12 y S15 tienen `- [ ]`/`- [x]`. **S29 y S30 usan `*` sin corchetes — no tienen estado de
> completado.** Transcribir la 29 haría que nada apareciera como completado y **todo se vería
> como leak**: cientos de falsos positivos, y el DoD 3 sin forma de verificarse.
> La S15 además trae los leaks reales: **Apex Lab ×5 y los 4 tasks de Insights ×6**, con
> completados alrededor para contrastar.

- [ ] Transcribir la **semana 15** a `docs/seed/` — transcripción, no interpretación
- [ ] **Contar a mano** cuántos registros deberían salir, ANTES de cargar
- [ ] `seed.py` idempotente: correrlo dos veces deja la base igual
- [ ] Un `scheduled_task` por tarea repetida, con su FK en cada `daily_record`
- [ ] Verificar el conteo. Si no coincide, se investiga — **no se ajusta el número**
- [ ] Semana 12 solo si sobra tiempo. S26/29/30 fuera

### 🔲 Bloque 2 · E05 fase C — Query de score · 1.4 pom · 🚫 **la escribes tú**
- [ ] Agregación ponderada por semana, día y categoría
- [ ] ⚠️ Si sale de un prompt, la capacidad de la semana no se adquirió aunque el checkbox esté verde

### 🔲 Bloque 3 · E05 fase D — Diseño SQL · 1.2 pom · 🤝 sesión conmigo
- [ ] Leer el `EXPLAIN` de tu query
- [ ] Decidir el índice y **justificarlo**
- [ ] ¿Qué se degrada primero con volumen?

### 🔲 Bloque 4 · Personal + cierre · 1.6 pom
- [ ] Lectura 30 min · registro · entrenos
- [ ] Cierre: log + commit + checkboxes

> 🔍 **Análisis del día:** {…}

---

## Día 7 — DOM 2 AGO · Cierre y siguiente semana
**Foco:** verificar las capacidades, no las tareas. · **Plan 6.2 pom** · real: ___

### 🔲 Bloque 1 · LeetCode · 1.6 pom
- [ ] 3 problemas — tema sugerido: hash maps y agregación

### 🔲 Bloque 2 · Comprehension gates · 1.2 pom · 🤝 interrogatorio
- [ ] Los 8 gates de abajo, en voz alta, sin ver notas

### 🔲 Bloque 3 · Retro · 0.8 pom
- [ ] ¿Se cumplió el goal? ✅/❌ con evidencia
- [ ] ¿Se adquirieron las dos capacidades?
- [ ] Precisión de estimación: 34.2 pom estimados vs reales
- [ ] Leaks, slips y causa raíz
- [ ] **Autoevaluación 1-5 por dominio** (línea base para enero)

### 🔲 Bloque 4 · Generar Semana 02 · 1.0 pom
- [ ] Doc semanal + docs de épica
- [ ] Deuda que pasa de S01

### 🔲 Bloque 5 · Personal + cierre · 1.6 pom
- [ ] Lectura 30 min · registro · entrenos
- [ ] Cierre: log + commit + checkboxes

> 🔍 **Análisis del día:** {…}

---

# 🚪 COMPREHENSION GATES
*Se interrogan el domingo. Titubeo = no se marca y el tema pasa como deuda a Semana 02.*

○ Explico mi modelo de datos de memoria, entidad por entidad, y por qué cada relación es como es
○ Explico cómo un contador semanal (`0/15 pomodoros`) se alimenta de tasks diarias **sin duplicar estado**
○ Explico cómo modelo una recurrencia sin generar 365 filas al año, y qué pasa si cambia a mitad de mes
○ Explico qué índice necesita mi query de score y **por qué**, leyendo el `EXPLAIN`
○ Explico por qué elegí ese ORM y qué pierdo exactamente con la alternativa
○ Explico la jerarquía de CLAUDE.md y qué va en cada nivel
○ Explico cuándo uso skill, cuándo slash command y cuándo subagente, con un ejemplo de cada uno
○ Explico con mi propio dato por qué un plan detallado mantiene al agente trabajando más tiempo

---

# 📋 PARTE 3 — REFERENCIA

## ⚙️ Calibración vigente (nivel por dominio, 1–5)
```
Backend _ · Frontend _ · System Design _ · DevOps _ · Data Eng _ · Lenguajes/paradigmas _
Pendiente: autoevaluación honesta esta semana (línea base para comparar en enero).
Primera evaluación oral: dom 30 ago.
```

## 🧱 Decisiones congeladas
```
App → Loop · Plataforma v1 → web desktop-first, keyboard-driven
PRINCIPIO → diseñar para multiusuario, desplegar para uno
Stack (ADR-001) → Vite+React+TS · Flask/Python · SQLAlchemy · PostgreSQL
  Hosting → Cloudflare Pages (front) · contenedores propios (API + BD)
  Auth → Clerk · Testing → E2E como objetivo, comprometido post-Tokio
Orden de diseño → requerimientos → modelo de datos → stack
Arquitectura de docs → 2 repos hermanos bajo ~/dev/tech-lead-path/: logbook (programa, privado) · loop (producto)
Reparto de ADRs → producto en loop/docs/adr · programa en logbook/adr
Hosting v1 → fuera de Azure (tentativo Vercel + Neon); migración a infra propia en S9-S11
Pendiente → librería de fechas (ADR-003, miércoles: depende del modelo de recurrencia)
```

## 🅿️ Parking
```
WezTerm · tmux · Neovim · dotfiles → cuando corras 2-3 agentes en paralelo (~S9-S11)
Hooks · MCP → S2-S3, cuando haya flujo que automatizar
Escribir código por voz → nunca. El flujo de voz es para pensar y dictar prosa.
Proyectos 2-5 → cada uno se define la semana anterior a su arranque
```

## 📅 Horizonte
```
S2 (3-9 ago): API + integración · **E07 pipeline propio de validación (~8 pom)**
S3 (10-15 ago): UI + deploy · E2E por debajo del deploy en prioridad
S4 (post-Tokio): **E08 comparar contra No Mistakes y decidir (~4 pom)**
16-28 ago: Tokio (modo viaje) · 22 ago: se define el Proyecto 2 · 30 ago: 1ª evaluación oral
```

---

# 🎓 PARTE 4 — APRENDIZAJES & PENDIENTES

## ⭐ Lecciones vigentes
```
1. Diseñar el sistema se siente productivo — por eso es peligroso. El scope queda congelado
   hasta la retro del domingo.
2. El leak no es disciplina, es estructura externa. Lo que tiene deadline o cita se ejecuta al
   85-100%; lo autodirigido sin estructura tiende a 0. El programa le da al estudio la misma
   estructura que ya tiene el entrenamiento.
3. Una cuota que se alcanza vale más que una que se abandona.
4. **Verificar la herramienta antes de agendarla.** El cask de E01 estaba mal en el doc y habría
   costado ~10 min del timebox de 30. Toda épica que dependa de software externo se verifica al
   generarse, no al ejecutarse. (27 jul)
5. **El mensaje de error dice qué tecnología falló — hay que leerlo completo.** Tableau decía
   *"No suitable driver installed, **or the URL is incorrect**"*. "URL" es vocabulario de JDBC;
   ODBC no habla de URLs. Se asumió ODBC y se instaló el driver equivocado. **Tableau en Mac usa
   JDBC**, con el `.jar` en `~/Library/Tableau/Drivers`. (30 jul)
6. **Aislar antes de arreglar.** El rodeo del driver no fue tiempo perdido porque `sqlcmd` descartó
   red, credenciales y permisos de un golpe, dejando una sola variable viva. *Cuando algo dice "no
   encuentro X" y sabes que X existe, la pregunta no es "¿está instalado?" sino "¿qué copia usa y
   dónde la busca?"*. (30 jul)
7. **Ejecutar no es registrar.** A–C corrieron en 1.0 pom y el log salió vacío: motor, trigger y
   tabla de errores sin anotar. La evidencia se escribe en el momento o no existe. (27 jul)
```

## ❓ Decisiones abiertas
*Cada una bloquea un día concreto. Sin respuesta al día indicado, el bloque se ejecuta a ciegas o se cae.*

| # | Decisión | Bloquea | Deadline | Respuesta |
|---|---|---|---|---|
| D1 | ¿Doc de E04 completo ahora, o checkbox hoy y doc el martes? | E04 fase A | lun 27 | ✅ **Checkbox hoy; doc el martes al cerrar el ADR-001** |
| D2 | Dataset de Tableau | E06 fase A | lun 27 | ✅ **Greenstone** · Desktop Free Edition · 🔒 guardado local, nunca se publica |
| D3 | ¿Claude Code instalado? | E02 fase A | lun 27 | ✅ **Sí, desde el 21 jun.** Sin `CLAUDE.md`, sin `commands/`, sin `agents/`, sin `skills/` — se escribe todo de cero |
| D4 | Autoevaluación 1-5 por dominio: ¿conmigo o solo? | Retro dom 2 | mié 29 | ___ |
| D5 | Semanas 26/29/30 de Apple Notes: ¿exportables o a mano? ¿en qué formato salen? | E05 fase B (parser, sáb) | **mar 28** (sesión de requerimientos) | ___ |
| D6 | ¿La empresa tiene licencias de Tableau? | E06 a partir de S2 | vie 31 | 🔶 **Parcial (30 jul): la edición gratuita SÍ conecta a Azure SQL.** La limitante era el driver JDBC, no la licencia. Sigue abierta la parte de dónde deben vivir los datos de Greenstone |
| D7 | ¿Hay acceso a las keys del eLearning de Tableau? (las comparte Sarvani en el learning path) | nada — es opcional | vie 31 | ___ |

> D5 es la que más riesgo esconde: el parser del sábado se diseña el jueves en la bici. Si el formato
> real trae sorpresas y se descubren el sábado, la fase B de E05 (2.4 pom) se desborda y el goal se cae.
> **Trae una nota exportada de muestra a la sesión del martes.**

## 📌 Pendientes
```
🔲 Averiguar si hay acceso a keys del eLearning de Tableau (no bloquea nada)
🔲 Autoevaluación honesta 1-5 por dominio (línea base para comparar en enero) → ver D4
🔲 Confirmar dataset propio para el primer dashboard de Tableau → ver D2
```

## 🧰 Deuda técnica registrada (29 jul)
```
🔲 Dockerfile: la imagen de uv está pineada a `:latest` → no es reproducible.
   Pin explícito cuando se decida la versión. No urgente, sí real.
🔲 `oxlint` viene por default en la plantilla de Vite (ya no ESLint). Se dejó.
   Si estorba, son dos líneas. Decisión de estilo, no de hoy.
🔲 El grupo `dev` entra a la imagen de producción. Se separa con `--no-dev`
   el día del deploy (S3). Deuda consciente, documentada por el agente.
🔲 `logbook/adr/` sigue vacío en git → se cierra con el primer ADR del programa.
🔲 ODBC instalado sin necesidad para Tableau (msodbcsql18, mssql-tools18, unixodbc).
   No estorba y `sqlcmd` es útil. No se desinstala. (30 jul)
🔲 Anaconda y Homebrew con unixODBC compitiendo en el PATH. No afecta a Tableau
   (usa JDBC), pero puede morder si algo de Python usa ODBC. (30 jul)
🔴 SEGURIDAD: rotar la credencial `fronagbb1282b1_temp_ro` — se expuso en texto plano.
   Y limpiar `~/.zsh_history`. (30 jul)
```

## 🚨 Banderas
```
- Riesgo #1: que E01 se expanda al "setup completo del video". Timebox 30 min, sin negociación.
- Riesgo #2: llegar al JUEVES sin el modelo hecho → el interrogatorio no existe y el goal de la
  semana se cae completo. El modelo es del MIÉRCOLES y no se mueve.
- Riesgo #3: que E02 se convierta en leer documentación. El DoD no dice "entendí skills",
  dice "existe una skill que ya usé dos veces". Notas sin artefactos = épica fallida.
- Riesgo #4: que la query de score se genere en vez de escribirse. Si sale de un prompt, la
  capacidad de la semana no se adquirió aunque el checkbox esté verde.
- Riesgo #5 (NUEVO): la semana no tiene colchón (53.9 pom de 53.9 de capacidad). Un día perdido
  no se recupera: se paga recortando Tableau, nunca el modelo ni la query.
```

---

# 📝 CONTROL DE VERSIONES
| Versión | Fecha | Cambio |
|---|---|---|
| v1 | 25 jul 2026 | Creada. Primera semana real del programa. |
| v2 | 25 jul 2026 | **Claude Code entra como E02** (6 pom) y las demás se recorren; **nada se recortó** — Tableau conserva sus 3 sesiones. El goal pasa a tener dos mitades: el artefacto y el método. La semana sube de ~38 a ~44 pom de escritorio y queda sin colchón. |
| v3 | 25 jul 2026 | **Bici fija: martes y jueves, 2 h c/u = 9.6 pom seguros.** El ADR, el digest, la metodología y el diseño del parser se mueven ahí. Capacidad total ~53.6 pom. |
| **v12.3** | **30 jul 2026** | Bloque 2 marcado: **E03 cerrada** con `adr-002`, `adr-003`, modelo v3 y E05 generada. Alineada la numeración de bloques entre `semana-01.md` y `dias/jueves-30-jul.md`, que habían divergido. **Bloque 3 (consolidación) sigue pendiente** y no estaba en la lista de faltantes. |
| **v12.2** | **30 jul 2026** | **E06 fase A cerrada: 50 min reales contra 35 estimados (+43%).** Causa: el driver JDBC, prerrequisito no verificado — **segunda vez esta semana** (la primera fue el cask de OpenSuperWhisper el lunes). Va a la retro. Tres hallazgos reales sobre Greenstone. **El ejercicio del viernes cambió:** Load Times 2.0 vive en Luzmo, así que se reproducen cifras y se comparan herramientas, no se replica un diseño. El drill de Workout Wednesday se mueve a S2. |
| **v12.1** | **30 jul 2026** | **Tableau conectado a Greenstone (plan A).** Hallazgo: **Tableau Desktop en Mac usa JDBC, no ODBC** — el `.jar` va en `~/Library/Tableau/Drivers`. El mensaje de error lo decía desde el principio ("or the URL is incorrect" es vocabulario de Java) y se leyó mal. **D6 parcialmente resuelta:** la edición gratuita sí conecta; la limitante era el driver, no la licencia. Dos lecciones nuevas y deuda técnica registrada, incluida una credencial de producción que hay que rotar. |
| **v12** | **30 jul 2026** | **E03 cerrada.** `adr-002-modelo.md` y `adr-003-fechas.md` escritos, modelo v3 con H1-H3, **E05 generada con la skill** (segundo uso → cierra el DoD de E02 fase C). 🔴 **Corrección importante en E05 fase B: el seed usa la semana 15, no la 29.** S29 y S30 usan `*` sin corchetes y no tienen estado de completado — transcribirlas haría que todo se viera como leak. La S15 trae los leaks reales (Apex Lab ×5, Insights ×6) con completados alrededor. Eliminadas las menciones a "parser" e "importar" del goal, T2 y día 6: `spec-v1.md` §10 las supersede. |
| **v11.4** | **30 jul 2026** | Revertido: **los nueve temas y el examen de 35 se hacen hoy**, no tres temas y el examen el domingo. Decisión de Arturo. El material son ~2h15 contra 1h de bici; lo que no quepa se termina en escritorio. |
| **v11.3** | **30 jul 2026** | Bici del jueves ajustada: **estudio tema por tema** (leer → cerrar el doc → contestar a ciegas) en vez del examen completo. Nueve temas no caben en una hora, y un examen con 25 blancos no enseña. Se cubren los tres que solo se aprobaron el miércoles: Docker, servidor web y SQLAlchemy. **El examen de 35 se reserva para el domingo** como preparación de los gates. |
| **v11.2** | **30 jul 2026** | **H3 decidida** y con ella cierra el bloque de decisiones: instante + zona IANA + fecha civil local. Corrige la derivación de H1. H4 se mueve al viernes. **Día 4 reordenado:** el examen del doc de estudio se va a la bici y la consolidación queda **dirigida por lo que falle** en el examen, no genérica. |
| **v11.1** | **30 jul 2026** | **H2 decidida: `sesiones_pomodoro` entra al schema.** Razón: los datos que no se capturan hoy no se pueden reconstruir después. Obliga a "empezar pomodoro" en la UI y elimina `num_pomodoros_terminados` como columna. |
| **v11** | **30 jul 2026** | **Día 4 replanificado.** La fase D del interrogatorio baja de 2.2 a 1.0 (se adelantó al miércoles) y el subagente ya estaba en S2, así que el día se reorganiza alrededor de **cerrar E03 con H1–H4 resueltas**. Entra un **bloque de consolidación** — código abierto junto al doc de estudio, buscando la línea real de cada concepto — como corrección directa al análisis del miércoles. **Tableau entra** (deuda del lunes). La bici es repaso, no material nuevo; el digest se mueve al viernes. **H1 decidida: `no_completado` se deriva.** |
| **v10** | **29 jul 2026** | **Día 3 cerrado en 10.0 pom** de 7.2 planeados (+39%). E04 4/4, modelo de datos diseñado e interrogado, 10 commits pusheados, CORS verificado sin bug. **Señal importante en el análisis:** volumen de conceptos nuevos por encima de lo que se consolida en un día. Se crea `estudio/semana-01.md` (9 temas desde cero, ~60 preguntas de recall) y **el jueves en la bici se repasa en vez de agregar material nuevo.** |
| **v9.1** | **29 jul 2026** | Día 3 marcado completo bloque por bloque, con las 4 correcciones al plan del agente y el dato de E02 registrados. Tableau baja a 2 sesiones. ADR-003 pasa al jueves: depende de H3. |
| **v9** | **29 jul 2026** | **E04 cerrada 4/4.** `docker compose up` levanta API y Postgres con healthcheck; el volumen nombrado sobrevivió `down && up`; CORS a mano probado con dos orígenes distintos; `CLAUDE.md` con estructura y comandos reales. Liveness vs readiness verificado apagando Postgres. 6 tests pasan. **Modelo de datos diseñado solo e interrogado el mismo día** (8 decisiones difíciles). **Dato de E02:** un prompt de 4 palabras produjo un plan de nivel senior — el apalancamiento está en el contexto del repo, no en el prompt. Añadida deuda técnica y la regla de `CREATE EXTENSION` para el viernes. |
| **v8** | **28 jul 2026** | **Día 2 cerrado en ~9.4 pom** — el mejor del programa y 4.7× el día 1. Artefactos: CLAUDE.md ×3, `spec-v1.md`, `adr-001-stack.md` con 6 decisiones, skill `nueva-epica`, E04 generada, ambos repos en GitHub. Hallazgo: **slash commands y skills se fusionaron**; el gate del domingo cambia de pregunta. ⚠️ **Alerta de capacidad para el miércoles:** E04 re-estimada 2.8→4.5 pom deja el día en 8.8 — hay que recortar antes de arrancar. |
| **v7** | **28 jul 2026** | **Día 2 cerrado en 4.6 pom.** Spec commiteada y **ADR-001 completo** — creció de 4 a 6 decisiones: frontend y API separados, backend en **Python/Flask**, SQLAlchemy con frontera ORM/Core, Cloudflare Pages + contenedores propios, **Clerk**, E2E como objetivo. Principio nuevo que reescribe la spec: **diseñar para multiusuario, desplegar para uno**. Agendadas E07 (S2) y E08 (S4). ⚠️ *Este archivo se corrompió al generarse (28 MB por un `replace` con cadena vacía) y se reparó — si algo se ve raro, avisar.* |
| **v6.1** | **28 jul 2026** | **CLAUDE.md global cerrado y validado.** Hallazgo que cambia el plan: Arturo nunca había usado Claude Code en serio, así que no hay fricciones que documentar — el archivo sale como v0 con defaults marcados a prueba y una sección vacía que se alimenta con evidencia real. **E02 fase D (subagente) se mueve a S2**: la primitiva más avanzada no se diseña sin haber sufrido el flujo. E02 baja a 5.6 pom y cierra parcial, con deuda documentada. |
| **v6** | **28 jul 2026** | Día 1 cerrado en **2.0/6.5** con análisis de causa raíz. Día 2 replanificado: **bici recortada a 1 h** (2.4 pom), CLAUDE.md global entra como bloque 1 en deuda, y se detalla paso a paso. Tableau sesión 1 **no se apila** al martes: se mueve al miércoles y la semana cierra en **2/3 de Tableau** — se marca ahora, no se finge. |\n| **v5.2** | **27 jul 2026** | Repo del programa nombrado **`logbook`**; los dos repos cuelgan como hermanos de `~/dev/tech-lead-path/`. Bloque 2 detallado con los comandos reales y marcado 2a como hecho. Añadido `docs/adr/README.md` en loop porque git no versiona carpetas vacías. Referencias actualizadas en bloque 3 y en decisiones congeladas. |\n| **v5.1** | **27 jul 2026** | **Dos repos, no uno.** Los docs del programa se separan de `loop`: ciclos de vida distintos, audiencias distintas, y el CLAUDE.md de `loop` debe estar scoped a su código. Los ADR de producto se quedan en `loop/docs/adr/`; los del programa van a `tech-lead-path/adr/`. Bloque 2 sube de 0.8 a 1.0 pom. Aparece un tercer `CLAUDE.md`. Decisión de Arturo. |
| **v5** | **27 jul 2026** | **Arreglado el día por día.** El marcador `○` no es sintaxis de lista de markdown: el renderer unía las tareas en un solo párrafo ilegible. Migrado a `- [ ]` / `- [x]`, que renderiza como checkbox real y sigue siendo binario. Cada día pasa de lista plana de épicas a **bloques numerados en orden de ejecución con subtasks**. Días 5 y 6 quedan con subtasks gruesas a propósito: dependen del modelo y del ORM, que aún no existen. |
| **v4.1** | **27 jul 2026** | Añadida la tabla de **decisiones abiertas (D1–D5)** con el día que bloquea cada una. Corregido el comando de instalación de E01: el cask es `my-monkeys/tap/opensuperwhisper`, no `opensuperwhisper` a secas. |
| **v4** | **27 jul 2026** | **Auditoría aritmética.** Corregidos los subtotales de mié (5.3→5.7 ing, 6.9→7.3 total) y jue (5.3→4.1 ing). Escritorio real: 44.3, no ~44. **Las épicas ahora absorben el trabajo de bici que no se les estaba atribuyendo**: total de épicas 28.7→35.7 pom (E02 5.9→7.1, E03 7.0→7.8, E05 6.0→10.8). Corregida la contradicción del Riesgo #2 (el modelo es del miércoles, no del martes) y de la cuota de Tableau (Lun/Mié/**Vie**, no Jue). **E06 se genera hoy** (no dependía de nada). Añadidos: fila de calibración, riesgo #5 (cero colchón) y regla de sacrificio (cae Tableau 2, nunca el modelo). |

---

# 🏁 CIERRE SEMANA 01
*(se llena el domingo 2, antes de generar Semana 02)*

## 📊 Números finales
```
POMODOROS: ___ de 53.9 · escritorio ___/44.3 · bici ___/9.6
LECTURA: ___ sesiones · pp. ___ · COMMITS: ___
```

## ✅ Inventario real vs plan
| Área | Real/Plan | Detalle |
|---|---|---|
| 🎓 Bloques de ingeniería | _/7 | |
| 🎯 Épicas cerradas | _/6 | |
| 📖 Lectura | _/7 | |
| 📊 Tableau | _/3 | |
| 🤝 Sesiones con Claude | _/3 | |
| 🧮 LeetCode | _/3 | |
| 🚪 Gates superados | _/8 | |

## 🎯 Retro
- **¿SE CUMPLIÓ EL GOAL?** ✅ / ❌ — {evidencia: URL, query, output real}
- **¿SE ADQUIRIERON LAS DOS CAPACIDADES?** (1) esquema + SQL: {…} · (2) operar Claude Code: {…}
- **Precisión de estimación:** 34.2 pom de épicas estimados vs ___ reales · desviación ___% · causa: {…}
- **Score ponderado:** ___% — Ingeniería ___% · Personal ___%
- **Leaks** (≥3 apariciones sin 🟢): {…} → {reestructurar / delegar / matar}
- **Slips → causa raíz:** {sin excusas}
- **Aprendizaje técnico top:** {…}
- **Score global (0–10):** ___
- **Deuda que pasa a Semana 02:** {épicas abiertas + slips}
