# 🎯 E02 — Claude Code a nivel experto: primitivas y herramientas propias

> **Estado:** 🔲 no empezada · **Semana:** 01 · **Días:** lun–jue · **Proyecto:** `loop` + `~/.claude`
> **Estimado:** 7.1 pom (~178 min: 5.9 de escritorio + 1.2 de bici) · **Real:** ___ pom
> **Depende de:** E01 (solo la fase E) · **Desbloquea:** todo el resto del semestre — es la épica que multiplica

---

## 🎯 Objetivo
Operar Claude Code como un tech lead dirige a su equipo — planear, delegar con contexto, validar — y salir de la semana con herramientas propias que uses de verdad, no con tutoriales vistos.

## 💡 Por qué existe
Es la épica de mayor apalancamiento del programa completo. Cada primitiva que domines se paga en las 25 semanas restantes: un CLAUDE.md bien escrito mejora cada sesión; una skill propia se reusa indefinidamente; plan mode es la diferencia entre un agente que corre 5 minutos y uno que corre horas. Si esto sale bien, el resto del semestre se acelera. Si se pospone, cada semana futura rinde menos de lo que podía.

## 🔑 Prerrequisitos
○ Claude Code instalado y funcionando
○ Repo `loop` creado (E04 fase A, lunes)

---

## ✅ DEFINITION OF DONE

○ Existe `~/.claude/CLAUDE.md` global **escrito por mí**, y `loop/CLAUDE.md` del proyecto — y puedo explicar qué va en cada nivel y por qué
○ El scaffold de `loop` se construyó en **plan mode**, con el plan revisado y anotado ANTES de que escribiera nada
○ Existe **una skill propia** (`SKILL.md`) que ya usé al menos dos veces
○ Existe **un slash command propio** y **un subagente** con caso de uso real
○ Puedo explicar la diferencia entre skill, slash command y subagente — y cuándo usar cada uno
○ Tengo **el dato propio** de plan mode: cuánto corrió el agente con plan detallado vs con prompt corto

---

## 📋 Checklist de ejecución

### Fase A — CLAUDE.md · 1.5 pom (37 min) · **LUN 27**
○ Leer la doc oficial de memoria de Claude Code — jerarquía y precedencia
○ Escribir `~/.claude/CLAUDE.md` **global**: tus preferencias transversales (idioma, estilo de commits, cómo quieres que se te responda, qué nunca hacer)
○ Escribir `loop/CLAUDE.md` **del proyecto**: stack, estructura de carpetas, comandos (`dev`, `test`, `migrate`), convenciones de naming, trampas conocidas
○ **Regla de contenido:** va lo que el código NO dice (por qué, convenciones, gotchas). No va lo que el código ya dice.
○ Probar: abrir sesión nueva y verificar que el contexto se aplicó sin repetirlo

### Fase C — Primera skill + slash command · 2.4 pom (60 min) · **MAR 28**
○ Leer la doc de Skills: estructura de carpeta, `SKILL.md`, cuándo se dispara
○ **Elegir un procedimiento que repitas de verdad.** Candidatos: generar el doc semanal desde el template · crear una épica nueva · revisar un PR contra tus convenciones
○ Escribir la skill con su descripción de disparo bien redactada *(la descripción es lo que determina si se activa — es la parte difícil)*
○ Crear un slash command en `.claude/commands/` para algo que tecleas seguido
○ Usar ambos al menos una vez el mismo día

### Fase E — Metodología P→I→V · 1.2 pom (30 min) · **MAR 28** · 🚴 *bici*
○ Video o lectura sobre Plan → Implement → Validate
○ **3 takeaways dictados** *(sin takeaways escritos no cuenta — regla de cuotas #2)*
○ Formular la hipótesis que vas a medir mañana en la fase B

### Fase B — Plan mode aplicado · 0.5 pom (12 min) · **MIÉ 29**
○ Ejecutar el scaffold de `loop` (E04 fase B) **en plan mode**
○ **Leer el plan completo antes de aprobarlo.** Anotar qué habrías hecho distinto
○ Comparar: ¿qué tan lejos llegó solo con este plan vs con un prompt de una línea?
○ Anotar la conclusión en el log — es el dato que sostiene toda la metodología

### Fase D — Subagente revisor · 1.5 pom (37 min) · **JUE 30**
○ Leer la doc de subagentes: contexto separado, cuándo conviene
○ Crear un subagente en `.claude/agents/` que **interrogue tu código** contra los comprehension gates: que pregunte por qué, no que apruebe
○ Correrlo contra lo que llevas de `loop` y anotar qué encontró
○ Documentar en qué caso usarías subagente vs skill vs slash command

---

## 📚 Metodología (se lee/escucha en modo bici)
```
PLAN → IMPLEMENT → VALIDATE
· La calidad del plan determina cuánto corre el agente solo: prompt de una línea = minutos;
  plan detallado = horas.
· Tratar a los agentes como ingenieros junior: delegar con contexto y RAZONES, no con
  micro-instrucciones.
· Todo lo que regresa se valida.
⚠️ LÍMITE DEL PROGRAMA: quien deja de revisar código lo hace con 15 años de criterio construido.
   Tú estás construyendo ese criterio. Hasta enero, todo se revisa. Los comprehension gates
   existen exactamente por eso.
```

---

## 🚫 Fuera de scope (explícito)
```
Hooks y MCP                       → S2-S3, cuando haya flujo que automatizar
WezTerm · tmux · Neovim           → parking hasta ~S9-S11 (orquestación de múltiples agentes)
Configurar por configurar         → cada primitiva sale con artefacto de uso real o no cuenta
Gestión de contexto (primitiva 8) → S2, cuando las sesiones sean largas de verdad
```

## ⚠️ Trampa conocida
Leer documentación en vez de construir herramientas. El DoD no dice "entendí las skills", dice "existe una skill que ya usé dos veces". Si al final de la semana tienes notas y no artefactos, la épica falló aunque hayas aprendido cosas.

**Segunda trampa:** que la skill sea de juguete. Si eliges un procedimiento que no repites de verdad, nunca la usarás dos veces y el DoD se cumple en el papel. La mejor candidata es la generación del doc semanal — la vas a correr 25 veces más.

## 🚪 Comprehension gate
○ Explico la jerarquía de CLAUDE.md y qué va en cada nivel
○ Explico cuándo uso skill, cuándo slash command y cuándo subagente — con un ejemplo de cada uno
○ Explico con mi propio dato por qué un plan detallado mantiene al agente trabajando más tiempo

---

## 📝 Log de ejecución
| Fecha | Fase | Pom | Qué pasó |
|---|---|---:|---|
| 27 jul | A | | |
| 28 jul | C, E | | |
| 29 jul | B | | |
| 30 jul | D | | |

---

## 🏁 Cierre
- **Estimado vs real:** 7.1 → ___ pom · desviación ___% · **causa:** {…}
- **DoD cumplido:** {sí / parcial — cuáles faltaron}
- **Artefactos producidos:** CLAUDE.md global ___ · CLAUDE.md proyecto ___ · skill ___ · comando ___ · subagente ___
- **Dato de plan mode:** con plan detallado el agente corrió ___ vs ___ con prompt corto
- **Veces que usé la skill:** ___
- **Aprendizaje:** {…}
- **Qué se adopta en S2:** {hooks · MCP · más skills · gestión de contexto}
