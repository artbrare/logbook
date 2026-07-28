# 📊 Semana 01 — S1 · Núcleo de datos de Loop (Jul 27 – Ago 2) · **v4**

> **De dónde venimos:** Sprint 0 fue ensayo del sistema documental — sirvió para validar el formato,
> no para producir software. **Aquí empieza el programa de verdad.**
> **Deuda que entra:** ninguna. Es la semana 1.
> **Fase:** S1 de S1–S3 · Loop debe estar en producción el **15 de agosto**.
> **Estado del proyecto:** `loop` no existe todavía. Al domingo debe guardar y consultar datos reales.

---

# 🎯 GOAL DE LA SEMANA

> **Al domingo 2 de agosto, Loop guarda y consulta datos reales — modelo diseñado por mí en
> PostgreSQL, mis tres semanas de Apple Notes importadas, y una query mía de score ponderado con su
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
| **E02** | Claude Code experto: primitivas y herramientas propias | 7.1 pom | | 🔲 | `epicas/E02-claude-code.md` |
| **E03** | Diseño de Loop v1: spec → modelo → ADR de stack | 7.8 pom | | 🔲 | `epicas/E03-diseno-loop-v1.md` |
| **E04** | Fundación del repo: Project · scaffold · plan mode | 2.8 pom | | 🔲 | *se genera al cerrar el ADR-001 (mar noche)* |
| **E05** | Núcleo de datos: schema · migraciones · import · query | 10.8 pom | | 🔲 | *se genera al cerrar E03 (jue)* |
| **E06** | Arranque de Tableau: fundamentos + primer dashboard propio | 3.6 pom | | 🔲 | `epicas/E06-tableau-arranque.md` |

> **Total épicas: 35.7 pom** de los 53.9 de capacidad. El resto es lectura (8.4), cierres (2.8),
> domingo (4.6: LeetCode, gates, retro, S02) y bici no-épica (2.4: digest y video).
> E04 y E05 se generan al desbloquearse: su checklist depende de decisiones que aún no existen.

## 🎯 Targets no negociables
- [ ] **T1 (w5):** modelo de datos propio, defendido bajo interrogatorio, convertido en schema corriendo contra Postgres local
- [ ] **T2 (w5):** las 3 semanas de Apple Notes importadas y consultables en la base
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
| 🛠️ Artefactos de Claude Code | 3 | 5 | | CLAUDE.md ×2 · skill · slash command · subagente |
| 🎬 Videos con takeaways | 1 | 2 | | Modo bici |
| 📊 Sesiones de Tableau | 3 | 3 | | **Lun · Mié · Vie** |
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
**Foco:** que las herramientas existan antes de construir con ellas. · **Plan 6.5 pom** · real: ___

### 🟢 Bloque 1 · E01 fases A–C · 1.2 pom → **real 1.0** ✅
> ⏱️ Timebox duro 30 min · cerrado en 25

- [x] `brew install --cask my-monkeys/tap/opensuperwhisper`
- [x] Descargar motores Whisper y Parakeet desde la app
- [x] Permisos de micrófono **y accesibilidad** en Ajustes del Sistema
- [x] AirPods como input · trigger **⌥** · modo **toggle**
- [x] Calibración: español fijo + Dictionary cargado con la jerga → 0 errores
- [ ] ⚠️ Deuda: A/B contra Parakeet → se paga mañana en la bici (bloque 2 de fase D)

### 🔄 Bloque 2 · E04 fase A — Fundación: dos repos · 1.0 pom
> Mecánico. Es el desbloqueo del resto del día.
> **Corregido 27 jul:** son **dos** repos, no uno. Ciclos de vida distintos — el programa dura 26
> semanas y Loop vive S1–S3; en septiembre los docs semanales quedarían enterrados en un repo
> archivado. Además `loop` puede volverse portafolio y los docs del programa traen autoevaluación
> y leaks. Ver `tech-lead-path/adr/adr-001-arquitectura-docs.md`.

**2a · `tech-lead-path` — el programa (privado)**
- [ ] `mkdir tech-lead-path && cd tech-lead-path && git init`
- [ ] `mkdir -p semanas epicas adr templates .claude`
- [ ] Mover `semana-01.md` → `semanas/` · las 5 épicas → `epicas/` · los 2 templates → `templates/`
- [ ] `README.md` — qué es el programa, cómo se navega, dónde vive cada cosa
- [ ] `git add . && git commit -m "chore: bootstrap tech-lead-path"`

**2b · `loop` — el producto**
- [ ] `mkdir loop && cd loop && git init`
- [ ] `README.md` — qué es Loop en 3 líneas + *"estado: en diseño, sin código aún"*
- [ ] `.gitignore` de Node
- [ ] `mkdir -p docs/adr` *(aquí van adr-001-stack y adr-002-modelo — se van con el código)*
- [ ] `git add . && git commit -m "chore: bootstrap loop repo"`

> 📌 **Regla de reparto:** ADR de producto (ORM, hosting, modelo) → `loop/docs/adr/`.
> ADR del programa (rituales, arquitectura de docs, alcance) → `tech-lead-path/adr/`.

### 🔲 Bloque 3 · E02 fase A — CLAUDE.md · 1.5 pom
> El bloque de mayor apalancamiento del día. Aquí sí hay que pensar.

**3a · `~/.claude/CLAUDE.md` global — completo hoy**
- [ ] Leer la doc oficial de memoria de Claude Code: jerarquía y precedencia
- [ ] Bloque 1 — cómo respondo contigo: idioma, longitud, preámbulo o no
- [ ] Bloque 2 — convenciones que cargas: estilo de commits, branches, naming
- [ ] Bloque 3 — **qué NUNCA hacer** *(el de mayor retorno: repasa tus sesiones desde junio y anota qué te hizo decir "no, eso no")*
- [ ] Bloque 4 — contexto de trabajo: stack diario, nivel al que te hablo

**3b · `tech-lead-path/CLAUDE.md` — el programa**
- [ ] Qué es el programa y qué NO se hace aquí (no se escribe código de producción)
- [ ] Convenciones de los docs: numeración ISO, umbral de épica, formato de bolitas
- [ ] Dónde vive cada cosa: `semanas/` `epicas/` `adr/` `templates/`
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

### 🔲 Bloque 4 · E06 fase A — Tableau sesión 1 · 1.2 pom
> ⏱️ 35 min. Desktop **Free Edition**. 🔒 Nada se publica.

- [ ] Instalar Tableau Desktop Free Edition
- [ ] Conectar Greenstone con tu cadena de conexión
- [ ] Identificar en el panel: dimensiones vs medidas, filas vs columnas, marcas
- [ ] Construir una vista simple: una serie de tiempo o un ranking
- [ ] **Anotar la pregunta concreta** que el dashboard responderá el viernes
- [ ] Elegir qué dashboard existente de Greenstone vas a replicar (tu ground truth)

### 🔲 Bloque 5 · Personal · 1.2 pom
- [ ] Lectura 30 min — *A Philosophy of Software Design*, caps 1–2
- [ ] Registro diario · hábitos · suplementos
- [ ] Entrenos del día

### 🔲 Bloque 6 · Cierre · 0.4 pom
- [ ] Llenar pom reales por bloque y el análisis del día
- [ ] Marcar checkboxes
- [ ] `git commit` del día

> 🔍 **Análisis del día:** {qué se shippeó · qué salió distinto al plan y por qué · corrección para mañana}

---

## Día 2 — MAR 28 JUL · Requerimientos y primeras herramientas
**Foco:** saber qué es Loop, y construir la primera herramienta reutilizable. · **Plan 6.0 escritorio + 4.8 bici** · real: ___

### 🔲 Bloque 1 · E03 fase A — Requerimientos · 2.0 pom · 🤝 sesión conmigo
- [ ] Inventario honesto de tu sistema de Apple Notes: qué funciona, qué falla, con evidencia de las semanas 26/29/30
- [ ] Los 4 tipos de task: hábito · one-off · contador semanal · meta
- [ ] Categorías anidadas, pesos (w5/w3/w2/w1) y fórmula de scoring
- [ ] Detección de leaks (≥3 apariciones sin ✓) y zombies
- [ ] Import del formato Apple Notes → **trae una nota exportada de muestra** (D5)
- [ ] 🚫 Lista explícita de NO-features de v1
- [ ] Escribir `spec-v1.md` y commitear

### 🔲 Bloque 2 · E02 fase C — Skill + slash command · 2.4 pom
- [ ] Leer la doc de Skills: estructura de carpeta, `SKILL.md`, cuándo se dispara
- [ ] Elegir un procedimiento que repitas **de verdad** *(la mejor candidata: generar el doc semanal desde el template — la vas a correr 25 veces más)*
- [ ] Escribir la skill, cuidando la descripción de disparo *(es lo que determina si se activa; es la parte difícil)*
- [ ] Crear un slash command en `.claude/commands/`
- [ ] **Usar ambos al menos una vez el mismo día**

### 🔲 Bloque 3 · Personal · 1.2 pom
- [ ] Lectura 30 min — APoSD caps 3–4
- [ ] Registro · hábitos · entrenos

### 🔲 Bloque 4 · 🚴 BICI · 2 h · 4.8 pom *(sesión fija)*
**4a · E01 fase D — validación del flujo · 2.4 pom**
- [ ] Ergonomía del trigger: ¿alcanzas ⌥ en ≤1 s sin desestabilizar? ¿lo encuentras a ciegas?
- [ ] Indicador colocado donde lo veas de reojo *(única señal de que el toggle sigue grabando)*
- [ ] Dictar notas del modelo de datos — **sin Claude**, es tu trabajo solo
- [ ] Un párrafo con pausa larga respirando fuerte → prueba de alucinación
- [ ] **A/B con Parakeet, con el Dictionary cargado** → cierra el hueco del gate

**4b · E03 fase C — dictar ADR-001 · 1.2 pom**
- [ ] ORM: Prisma vs Drizzle
- [ ] Hosting v1
- [ ] Auth: ¿un solo usuario necesita auth completo?
- [ ] Testing: setup y alcance
- [ ] Formato por decisión: opciones → trade-off → elección → **qué pierdo con la descartada**

**4c · E02 fase E — metodología · 1.2 pom**
- [ ] Video o lectura sobre Plan → Implement → Validate
- [ ] 3 takeaways dictados *(sin takeaways no cuenta)*

### 🔲 Bloque 5 · Cierre · 0.4 pom
- [ ] Log + commit + checkboxes
- [ ] ⚠️ **Generar el doc de E04** — el ADR-001 lo desbloquea
- [ ] Revisar la transcripción: contar palabras corregidas **y buscar frases inventadas**

> 🔍 **Análisis del día:** {…}

---

## Día 3 — MIÉ 29 JUL · El modelo y el plan
**Foco:** el entregable más importante de la semana, hecho solo. · **Plan 7.3 pom — día más cargado** · real: ___

### 🔲 Bloque 1 · E03 fase B — Modelo de datos · 1.6 pom · 🚫 **SIN CLAUDE**
> Lo primero del día, con la cabeza fresca. Es la línea que separa esto del vibe-coding.

- [ ] Entidades, campos, relaciones — papel o markdown
- [ ] **Problema duro 1:** ¿cómo se alimenta `0/15 pomodoros` desde tasks diarias sin duplicar estado?
- [ ] **Problema duro 2:** ¿recurrencia sin generar 365 filas al año? ¿y si cambia a mitad de mes?
- [ ] **Problema duro 3:** ¿dónde vive el peso — task, categoría, o ambas? ¿por qué?
- [ ] Anotar las 2-3 decisiones de las que menos seguro estás *(son las que voy a atacar primero)*

### 🔲 Bloque 2 · E04 fase B — Scaffold en plan mode · 2.0 pom
- [ ] Ejecutar el scaffold de `loop` **en plan mode**
- [ ] **Leer el plan completo antes de aprobarlo.** Anotar qué habrías hecho distinto
- [ ] Completar `loop/CLAUDE.md` con la estructura y comandos que ya existen

### 🔲 Bloque 3 · E02 fase B — El dato de plan mode · 0.5 pom
- [ ] Comparar: ¿hasta dónde llegó solo con plan detallado vs con un prompt de una línea?
- [ ] Anotar el número en el log — es el dato que sostiene toda la metodología

### 🔲 Bloque 4 · E03 — ADR-001 a limpio · 0.4 pom
- [ ] Corregir la transcripción del dictado y commitear `docs/adr/adr-001-stack.md`

### 🔲 Bloque 5 · E06 fase B — Tableau sesión 2 · 1.2 pom
> ⚠️ **El bloque que se sacrifica si el día se aprieta.** Si cae, se marca slip — no se arrastra en silencio.

- [ ] Workout Wednesday **2020 W13** — dashboard de ventas simple
- [ ] Reproducirlo **contra Greenstone**, no contra el dataset del reto
- [ ] Comparar contra el video de solución

### 🔲 Bloque 6 · Personal + cierre · 1.6 pom
- [ ] Lectura 30 min — APoSD caps 5–6
- [ ] Registro · hábitos · entrenos
- [ ] Cierre: log + commit + checkboxes

> 🔍 **Análisis del día:** {…}

---

## Día 4 — JUE 30 JUL · Interrogatorio y subagente
**Foco:** que el modelo sobreviva o se corrija con argumento. · **Plan 5.7 escritorio + 4.8 bici** · real: ___

### 🔲 Bloque 1 · E03 fase D — Interrogatorio · 2.2 pom · 🤝 sesión conmigo
- [ ] Tu modelo junto al mío, diferencia por diferencia
- [ ] Preguntas de sinodal: ¿qué pasa con 10,000 tasks? ¿qué query se degrada primero? ¿si cambias el peso de una categoría a media semana, se recalcula el histórico?
- [ ] ⚠️ Trampa: ceder por cansancio en vez de por argumento. Si cedes, anota por qué

### 🔲 Bloque 2 · E03 fase E — ADR-002 y cierre · 0.4 pom
- [ ] Escribir `adr-002-modelo.md`: qué cambió, qué defendiste, por qué
- [ ] **Cerrar E03** → generar el doc de **E05**

### 🔲 Bloque 3 · E02 fase D — Subagente revisor · 1.5 pom
- [ ] Leer la doc de subagentes: contexto separado, cuándo conviene
- [ ] Crear en `.claude/agents/` uno que **interrogue tu código** contra los gates: que pregunte por qué, no que apruebe
- [ ] Correrlo contra `loop` y anotar qué encontró
- [ ] Documentar: ¿cuándo subagente, cuándo skill, cuándo slash command?

### 🔲 Bloque 4 · Personal · 1.2 pom
- [ ] Lectura 30 min — APoSD
- [ ] Registro · hábitos · entrenos

### 🔲 Bloque 5 · 🚴 BICI · 2 h · 4.8 pom *(sesión fija)*
- [ ] 📰 Digest de vanguardia + 3 takeaways dictados *(1.2)*
- [ ] 🎬 Video de Claude Code + 3 takeaways *(1.2)*
- [ ] **E05 fase 0 — diseñar en voz alta el parser de Apple Notes y la query de agregación** *(2.4)* → llegar el viernes a escribir, no a descubrir

### 🔲 Bloque 6 · Cierre · 0.4 pom
- [ ] Log + commit + checkboxes

> 🔍 **Análisis del día:** {…}

---

## Día 5 — VIE 31 JUL · El modelo deja de ser papel
**Foco:** schema corriendo contra Postgres. · **Plan 6.2 pom** · real: ___
*Subtasks finas se llenan el jueves: dependen del modelo y del ORM, que aún no existen.*

### 🔲 Bloque 1 · E05 fase A — Schema v0 · 2.0 pom
- [ ] Traducir el modelo defendido a schema (SQL o el DSL del ORM elegido)

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

### 🔲 Bloque 1 · E05 fase B — Parser e import · 2.4 pom
- [ ] Parser del formato Apple Notes
- [ ] Seed con las semanas 26, 29 y 30
- [ ] Verificar: contar filas y comparar contra las notas originales

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
- [ ] Precisión de estimación: 35.7 pom estimados vs reales
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
Stack base → Next.js + TypeScript + PostgreSQL
Orden de diseño → requerimientos → modelo de datos → stack
Arquitectura de docs → 2 repos: tech-lead-path (programa, privado) · loop (producto)
Reparto de ADRs → producto en loop/docs/adr · programa en tech-lead-path/adr
Hosting v1 → fuera de Azure (tentativo Vercel + Neon); migración a infra propia en S9-S11
Pendiente de decidir → ORM · hosting definitivo · auth · testing (ADR-001, martes en la bici)
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
S2 (3-9 ago): API tipada + tests · S3 (10-15 ago): UI + deploy a producción
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
5. **Ejecutar no es registrar.** A–C corrieron en 1.0 pom y el log salió vacío: motor, trigger y
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
| D6 | ¿La empresa tiene licencias de Tableau? Creator son $900/año, solo anual, y solo compran publicar a Cloud — no capacidad de análisis. Si la empresa tiene, es además donde deben vivir los datos de Greenstone | E06 a partir de S2 | vie 31 | ___ |
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
| **v5.1** | **27 jul 2026** | **Dos repos, no uno.** Los docs del programa se separan de `loop`: ciclos de vida distintos, audiencias distintas, y el CLAUDE.md de `loop` debe estar scoped a su código. Los ADR de producto se quedan en `loop/docs/adr/`; los del programa van a `tech-lead-path/adr/`. Bloque 2 sube de 0.8 a 1.0 pom. Aparece un tercer `CLAUDE.md`. Decisión de Arturo. |
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
- **Precisión de estimación:** 35.7 pom de épicas estimados vs ___ reales · desviación ___% · causa: {…}
- **Score ponderado:** ___% — Ingeniería ___% · Personal ___%
- **Leaks** (≥3 apariciones sin 🟢): {…} → {reestructurar / delegar / matar}
- **Slips → causa raíz:** {sin excusas}
- **Aprendizaje técnico top:** {…}
- **Score global (0–10):** ___
- **Deuda que pasa a Semana 02:** {épicas abiertas + slips}
