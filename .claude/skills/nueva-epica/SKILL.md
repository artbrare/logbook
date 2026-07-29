---
name: nueva-epica
description: Usar cuando el usuario pida crear, generar o abrir una épica nueva del programa Tech Lead Path, o mencione un ID de épica (E04, E05...) que aún no tiene documento en epicas/.
model: best
---

# Generar un doc de épica

## Antes de escribir nada
Verifica que tienes estos datos. Si falta alguno, PREGUNTA — no lo inventes:
- ID y nombre de la épica
- Estimado en pomodoros (1 pom = 25 min) y reparto por fases
- De qué depende y qué desbloquea al cerrarse
- En qué día o días de la semana cae

## Reglas que decides ANTES de generar
1. **Umbral:** solo merece épica si es ≥3 pom, O cruza más de un día, O produce
   un artefacto con nombre propio. Si no llega, dilo: es un checkbox del doc
   semanal, no una épica. No la generes.
2. **Dependencias:** si el checklist depende de un artefacto que todavía no
   existe (un modelo sin diseñar, un ADR sin cerrar), NO generes la épica.
   Explica qué falta y cuándo se desbloquea. Un checklist sobre algo inexistente
   es ficción.

## Cómo generar
1. Lee `templates/template-epica.md` desde la raíz del repo y respeta su estructura.
2. Escribe el archivo en `epicas/E{NN}-{slug}.md`, con NN de dos dígitos y slug
   en kebab-case.
3. Estado inicial: 🔲 no empezada.

## Calidad — la épica está mal escrita si falla alguna
- El **DoD** debe poder palomearse sin discutir. Si necesita interpretación, reescríbelo.
- El **estimado** va antes de arrancar; el real se llena al cerrar.
- El **fuera de scope** es obligatorio y explícito.
- El **objetivo** es una sola frase. Si necesita dos, la épica es muy grande: propón partirla.