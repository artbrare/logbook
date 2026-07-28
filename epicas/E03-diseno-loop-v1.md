# 🎯 E03 — Diseño de Loop v1: spec → modelo → ADR de stack

> **Estado:** 🔲 no empezada · **Semana:** 01 · **Días:** mar 28 – jue 30 · **Proyecto:** `loop`
> **Estimado:** 7.8 pom (~195 min: 6.6 de escritorio + 1.2 de bici) · **Real:** ___ pom
> **Depende de:** E01 (fase C se dicta) · **Desbloquea:** E04 fase B (scaffold, necesita el ADR-001) y E05 (núcleo de datos, necesita el modelo)

---

## 🎯 Objetivo
Definir qué es Loop v1, diseñar su modelo de datos con tus propias manos, y elegir el stack con argumentos defendibles.

## 💡 Por qué existe
Es la épica de la que cuelga todo lo demás de la semana y buena parte de las tres siguientes. Si el modelo sale mal, se paga en cada feature. Si sale mío en vez de tuyo, la capacidad del semestre no se construye: acabas con una app que funciona y que no entiendes.

## 🔑 Prerrequisitos
○ Project `Tech Lead Path` montado y repo `loop` creado (E04 fase A, lunes)
○ Tus semanas 26, 29 y 30 de Apple Notes a la mano como evidencia
○ Flujo de voz funcionando (E01) — si falló, la fase C se mueve a escritorio y hay que recortar en otro lado

---

## ✅ DEFINITION OF DONE

○ `spec-v1.md` existe y **alguien más podría construir Loop leyéndola**, sin preguntarte nada
○ La spec incluye una **lista explícita de NO-features de v1**
○ `modelo-arturo.md` existe, lo diseñaste **sin Claude**, y resuelve los tres problemas duros
○ Sobreviviste el interrogatorio: cada diferencia contra mi modelo la defendiste o la cediste **con razón**, nunca por cansancio
○ `adr-001-stack.md` con las 4 decisiones, y **ninguna dice "porque es lo que se usa"**
○ `adr-002-modelo.md` registra lo que cambió tras el interrogatorio y por qué

---

## 📋 Checklist de ejecución

### Fase A — Requerimientos · 2.0 pom (50 min) · **MAR 28**
○ Inventario honesto: qué hace bien tu sistema de Apple Notes y qué falla, con evidencia de las semanas 26/29/30
○ Los 4 tipos de task: hábito · one-off · contador semanal · meta
○ Categorías anidadas, pesos (w5/w3/w2/w1) y fórmula de scoring
○ Detección de leaks (≥3 apariciones sin ✓) y de zombies (heredadas sin avance)
○ Import del formato Apple Notes → analytics desde el día 1
○ 🚫 **Lista explícita de NO-features v1**
○ Escribir `spec-v1.md` y commitear

### Fase C — ADR-001 de stack · 1.2 pom (30 min) · **MAR 28** · 🚴 *dictado en la bici*
○ **ORM:** Prisma vs Drizzle — DX y mercado vs control de SQL y aprendizaje de Postgres
○ **Hosting v1:** llegar rápido al 15 de agosto vs infra propia desde el día 1
○ **Auth:** ¿Loop v1 de un solo usuario necesita auth completo?
○ **Testing:** setup y alcance
○ Formato por decisión: opciones → trade-off → elección → **qué pierdo con la descartada**

> ⚠️ Al cerrar esta fase se desbloquea **E04**: se genera su doc antes de dormir el martes.

### Fase B — Modelo de datos · 1.6 pom (40 min) · **MIÉ 29** · **SIN CLAUDE**
○ Entidades, campos, relaciones — en papel o markdown, como prefieras
○ **Problema duro 1:** ¿cómo se alimenta `0/15 pomodoros` desde tasks diarias sin duplicar estado?
○ **Problema duro 2:** ¿cómo modelas una recurrencia sin generar 365 filas al año? ¿Y si cambia a mitad de mes?
○ **Problema duro 3:** ¿dónde vive el peso — en la task, en la categoría, o en ambas? ¿Por qué?
○ Anotar las 2-3 decisiones de las que menos seguro estás *(son las que voy a atacar primero)*

### Fase C2 — ADR-001 a limpio · 0.4 pom (10 min) · **MIÉ 29**
○ Pasar a limpio la transcripción del dictado, corregir vocabulario técnico y commitear `adr-001-stack.md`

### Fase D — Interrogatorio · 2.2 pom (55 min) · **JUE 30**
○ Tu modelo junto al mío, diferencia por diferencia
○ Preguntas de sinodal: ¿qué pasa con 10,000 tasks? ¿qué query se degrada primero? ¿si cambias el peso de una categoría a media semana, se recalcula el histórico o no — y por qué?
○ Anotar los cambios acordados

### Fase E — ADR-002 y cierre · 0.4 pom (10 min) · **JUE 30**
○ Escribir `adr-002-modelo.md`: qué cambió tras el interrogatorio, qué defendiste, por qué
○ Commitear → **cerrar E03** → generar el doc de **E05**

---

## 🚫 Fuera de scope (explícito)
```
Escribir schema o migraciones          → es E05, viernes
Escribir código de features            → S2
Diseñar la UI                          → S3
Features de v1.5 (voz, push, mobile)   → ya están en la spec como NO-features; no se rediscuten
Multiusuario / equipos                 → nunca en v1; se revisa si Loop sale de tu máquina
```

## ⚠️ Trampa conocida
**Fase A:** diseñar lo que suena padre en vez de lo que ataca tu leak documentado. La spec se juzga contra tus datos reales, no contra tu entusiasmo.
**Fase B:** preguntarme. No lo hagas. Un modelo tuyo imperfecto vale infinitamente más que uno mío perfecto — el imperfecto se puede defender y corregir; el perfecto solo se puede copiar.
**Fase D:** ceder por cansancio en vez de por argumento. Si cedes, que sea porque te convencí, y anota por qué.

## 🚪 Comprehension gate
○ Explico mi modelo de memoria, entidad por entidad, y por qué cada relación es como es
○ Explico el contador semanal sin duplicar estado
○ Explico la recurrencia sin 365 filas
○ Explico por qué elegí ese ORM y qué pierdo con el otro

---

## 📝 Log de ejecución
| Fecha | Fase | Pom | Qué pasó |
|---|---|---:|---|
| 28 jul | A, C | | |
| 29 jul | B, C2 | | |
| 30 jul | D, E | | |

---

## 🏁 Cierre
- **Estimado vs real:** 7.8 → ___ pom · desviación ___% · **causa:** {…}
- **DoD cumplido:** {sí / parcial — cuáles faltaron}
- **Decisiones que cambiaron tras el interrogatorio:** {…}
- **Decisiones que defendiste con éxito:** {…}
- **Aprendizaje:** {…}
- **Lo que pasa a E05:** {el modelo final que se vuelve schema}
