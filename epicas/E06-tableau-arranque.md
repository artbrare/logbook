# 🎯 E06 — Arranque de Tableau: fundamentos + primer dashboard propio

> **Estado:** 🔲 no empezada · **Semana:** 01 · **Días:** lun 27 · mié 29 · vie 31 · **Proyecto:** track transversal
> **Estimado:** 3.6 pom (3 sesiones × 35 min) · **Real:** ___ pom
> **Depende de:** nada · **Desbloquea:** el track de visualización que converge con data engineering en S12–S15

---

## 🎯 Objetivo
Salir de la semana con un dashboard propio publicado sobre datos tuyos — no con tres tutoriales vistos.

## 💡 Por qué existe
Tableau es el track transversal más fácil de convertir en zombie: se avanza en tutoriales que se sienten productivos y no dejan artefacto. La regla del programa es explícita — curso sin entregable no puntúa. Por eso las tres sesiones de la semana terminan en un dashboard, no en un módulo terminado.

Es también la única cuota de la semana que se puede sacrificar sin tumbar el goal. Eso la hace la primera candidata a recorte el miércoles y la más propensa a convertirse en leak. Se cuida por eso, no a pesar de eso.

## 🔑 Prerrequisitos
○ **Tableau Desktop Free Edition** *(decisión 27 jul: autoría completa contra bases de datos, guardado local, $0. La licencia Creator de $900/año solo compra publicar a Cloud — no compra ninguna capacidad de análisis)*
○ **Dataset: Greenstone** (proyecto de Insights) vía cadena de conexión propia
○ 🔒 **REGLA DURA: el workbook NUNCA se publica.** Ni Tableau Public ni Cloud personal. Guardado local (.twbx) y punto. Son datos de la empresa.

---

## ✅ DEFINITION OF DONE

○ Existe **un dashboard propio guardado localmente** sobre datos de Greenstone
○ El dashboard responde **una pregunta concreta que yo tenía**, no una genérica del tutorial
○ Puedo explicar la diferencia entre **dimensión y medida**, y entre **conexión live y extract**
○ Las 3 sesiones están registradas con lo que se aprendió en cada una — no solo palomeadas
○ Nada salió de mi máquina

---

## 📋 Checklist de ejecución

### Fase A — Fundamentos y conexión · 1.2 pom (35 min) · **LUN 27**
○ Conectar Greenstone con la cadena de conexión
○ Entender el panel: dimensiones vs medidas, filas vs columnas, marcas
○ Construir la primera vista simple (una serie de tiempo o un ranking)
○ Anotar **la pregunta concreta** que el dashboard va a responder al final de la semana
○ Elegir **qué dashboard existente de Greenstone vas a replicar** — es tu ground truth

### Fase B — Drill de Workout Wednesday · 1.2 pom (35 min) · **MIÉ 29**
○ **2020 Week 13 — Merra Umasankar, "Do you want to build a simple Sales dashboard?"**
  `workout-wednesday.com/2020w13/` *(elegido del learning path de Sarvani: es el drill del nivel beginner que más se parece a replicar un dashboard de negocio)*
○ Reproducirlo **contra Greenstone**, no contra el dataset del reto
○ Comparar contra el video de solución
○ Anotar qué técnica nueva salió de aquí

> ⚠️ Esta es la sesión que se sacrifica si el miércoles (7.3 pom) se aprieta. Si se cae, se recupera el sábado o se marca como slip — **no se arrastra en silencio**.

### Fase C — Dashboard y guardado · 1.2 pom (35 min) · **VIE 31**
○ Layout con contenedores — referencia: **2020 Week 53, Brad Werner, "Can you build with containers"** `workout-wednesday.com/2020w53/`
○ Componer 2-3 vistas en un dashboard
○ Agregar un filtro o una acción de dashboard
○ **Guardar local (.twbx)** y anotar la ruta
○ Escribir 3 líneas: qué pregunta responde y qué me sorprendió del dato

---

## 📚 Checklist de competencias — Desktop I
*Tomado del learning path que compartió Sarvani. **No es el temario a cursar: es la lista contra la
que se mide si sé Tableau.** Se palomea construyendo, no viendo videos.*

```
○ Conectar a datos · editar y guardar una fuente
○ Terminología de Tableau
○ Cálculos básicos: aritmética, agregaciones y ratios propios, fecha, table calcs rápidos
○ Dashboards para compartir visualizaciones
Tipos de viz que debo poder construir:
○ Crosstab/tabla   ○ Pie y barras      ○ Mapa geográfico   ○ Doble eje / combo
○ Highlight table  ○ Treemap           ○ Scatter plot
```

## 🅿️ Cola de drills (del learning path, nivel beginner)
```
2020 W13  dashboard de ventas simple      → fase B, esta semana
2020 W53  contenedores                    → fase C, esta semana
2020 W05  ranking mes a mes (L. Stanke)   → S2
2021 W06  fancy text table (A. Jackson)   → S2
2020 W25  sets (L. Brown)                 → S3
```

---

## 🚫 Fuera de scope (explícito)
```
Cert Desktop Specialist        → oct, y solo si se concreta la oportunidad en el trabajo
Tableau Prep / Server / Cloud  → S12-S15, fase de data engineering
LOD expressions y cálculos avanzados → S4-S5 (Tableau intermedio)
Diseño visual bonito           → v1 responde la pregunta; el polish no puntúa esta semana
```

## ⚠️ Trampa conocida
Ver el curso completo en vez de construir. El formato pasivo sin entregable **no cuenta** (regla de cuotas #2). Si terminas la semana con tres módulos vistos y sin dashboard, la épica falló.

**Segunda trampa:** buscar el dataset perfecto. El dataset se elige ANTES de la sesión 1, en 5 minutos, y no se rediscute. Cualquiera de los tres candidatos sirve.

## 🚪 Comprehension gate
○ Explico la diferencia entre dimensión y medida con un ejemplo de mi propio dataset
○ Explico cuándo conviene una conexión live y cuándo un extract

---

## 📝 Log de ejecución
| Fecha | Fase | Pom | Qué pasó |
|---|---|---:|---|
| 27 jul | A | | |
| 29 jul | B | | |
| 31 jul | C | | |

---

## 🏁 Cierre
- **Estimado vs real:** 3.6 → ___ pom · desviación ___% · **causa:** {…}
- **DoD cumplido:** {sí / parcial — cuáles faltaron}
- **Dataset usado:** ___ · **Pregunta que responde:** ___
- **URL o ruta del dashboard:** ___
- **Aprendizaje:** {…}
- **Qué se hace en S2:** {…}
