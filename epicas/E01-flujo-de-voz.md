# 🎯 E01 — Flujo de voz para trabajar desde la bici

> **Estado:** 🔄 en curso · **Semana:** 01 · **Días:** lun 27 (A–C ✅) · mar 28 (D) · **Proyecto:** infraestructura personal
> **Estimado:** 3.6 pom (~90 min: 30 de setup + 60 de validación en rodillo) · **Real:** 1.0 pom (A–C) + ___ (D)
> **Parcial A–C:** estimado 1.2 → real **1.0** · desviación **−17%** · causa: el setup fue más limpio de lo previsto una vez corregido el cask
> **Depende de:** nada · **Desbloquea:** ~3 h/semana de capacidad nueva + el dictado del ADR-001 (E03 fase C)

---

## 🎯 Objetivo
Poder pensar y dictar con Claude desde el rodillo, con calidad suficiente para producir entregables reales — no solo notas sueltas.

## 💡 Por qué existe
Es la única fuente de horas nuevas del programa que no requiere agregar tiempo al día. Ruedas Z2 varias veces por semana y esas horas hoy son tiempo muerto cognitivo. Si el flujo funciona, el presupuesto semanal sube de ~18 h a ~22 h sin quitarle nada a nada. Si no funciona, se sabe el lunes y no se vuelve a intentar — y el presupuesto de la semana se recalcula a 18 h en vez de arrastrar horas fantasma.

**Riesgo estructural:** 9.6 pom de la semana (18% de la capacidad total) están planeados sobre la bici. Si E01 falla, el ADR-001, el digest, la metodología P→I→V y el diseño del parser se quedan sin slot y hay que recortar en escritorio.

## 🔑 Prerrequisitos
○ macOS 14+ (requisito de la app)
○ AirPods cargados
○ Sesión de rodillo el martes (fase D)

---

## ✅ DEFINITION OF DONE

○ Dicto un párrafo técnico bilingüe y sale legible **corrigiendo ≤2 palabras**
○ **Ninguna frase inventada** en las pausas de un dictado de 60 min *(criterio añadido el 27 jul: con toggle + Whisper, la alucinación produce oraciones enteras que el conteo de palabras corregidas no detecta)*
○ Activo el dictado **sin bajarme de la bici** ni soltar el manubrio más de 1 segundo
○ Produje un entregable real dictado pedaleando: **la postura de stack completa** (ORM, hosting, auth, testing — cada una con opciones, trade-off, elección y qué pierdo)
○ Puedo decir qué motor, qué micrófono y qué trigger uso, **y por qué cada uno** — con Parakeet efectivamente probado, no descartado por default

---

## 📋 Checklist de ejecución

### Fase A — Instalación · 0.4 pom (10 min) · **LUN 27** · 🟢
🟢 `brew install --cask my-monkeys/tap/opensuperwhisper` *(⚠️ corregido: el cask vive en el tap de **my-monkeys**, el fork mantenido. `brew install --cask opensuperwhisper` a secas falla)*
🟢 Descargar motor desde la app: **Parakeet** (streaming, ~200 ms de latencia) y/o **Whisper** (más preciso, mejor en code-switching)
🟢 Conceder permisos de micrófono **y accesibilidad** en Ajustes del Sistema
🟢 Dictar una frase cualquiera en un editor para confirmar que escribe

### Fase B — Configuración para bici · 0.6 pom (15 min) · **LUN 27** · 🟢
🟢 AirPods emparejados y **seleccionados como input** en la app *(el mic de la Mac no sobrevive al ruido del rodillo y el ventilador)*
🟢 Probar los tres triggers: combinación de teclas · **modificador único** (⌥ derecho, Fn) · **botón del mouse** (medio o de pulgar)
🟢 Elegir el que alcanzas sin soltar el manubrio → **⌥ Opción (modificador único)** · descartados: combinación de teclas (no se encuentra a ciegas) y botón de mouse
🟢 Probar *hold-to-record* vs *toggle* → **TOGGLE**. Manos libres mientras hablas; el costo es que si se te olvida parar, sigue grabando
🟢 Colocar la Mac donde la pantalla se lea desde la posición de la bici

### Fase C — Calibración de idioma · 0.2 pom (5 min) · **LUN 27** · 🟢
🟢 Dictar la frase de prueba con **español fijo**
🟢 Dictar la misma con **autodetección**
🟢 Comparar errores y fijar la configuración ganadora → **Whisper, 0 errores en escritorio.** ⚠️ Parakeet no se probó; la comparación queda abierta
```
FRASE DE PRUEBA (vocabulario bilingüe, que es donde estas herramientas fallan):
"El ORM maneja las migraciones con Drizzle y el schema vive en Postgres,
 pero el deploy del frontend va por separado."
```

### Fase D — Validación en rodillo · 2.4 pom (60 min, Z2) · **MAR 28**
*Rediseñada el 27 jul según la config elegida. **Toggle + Whisper** concentra el riesgo en un modo de
falla que el escritorio no puede mostrar: alucinación sobre respiración y ruido de ventilador.*

**Bloque 0 — Ergonomía del trigger · 5 min**
○ Arrancar Z2 *(solo Z2 indoor · nunca en calle · nunca en sesión de calidad)*
○ Desde la posición de pedaleo: alcanzar ⌥ y volver al manubrio. **¿Se puede en ≤1 segundo sin desestabilizar?**
○ ¿Encuentras ⌥ a ciegas, sin mirar el teclado? *(⌥ no está en una esquina — Fn sí. Si fallas por tacto, Fn es el plan B)*
○ ¿Colisiona? Verifica que un tap de ⌥ no dispare atajos del editor (⌥+flecha para saltar palabra es el sospechoso)
○ **Colocar el indicador donde lo veas de reojo.** Con toggle, ese indicador es tu única señal de que sigue grabando

**Bloque 1 — Whisper contra las condiciones reales · 15 min**
○ Dictar notas del modelo de datos *(incubación — sin Claude, es tu trabajo solo)*
○ Hablar en párrafos: tap de inicio, párrafo, tap de fin. **No dejes el toggle abierto entre párrafos.**
○ Al menos un párrafo con una pausa larga a propósito, respirando fuerte → es la prueba de alucinación

**Bloque 2 — A/B con Parakeet · 5 min** *(cierra el hueco del gate)*
○ Misma frase de prueba bilingüe, ahora con Parakeet, con ventilador y respiración
○ **Con el Dictionary cargado.** Si Parakeet también acierta `Drizzle` y `ORM` → el diccionario es reemplazo posterior al texto, agnóstico del motor. Si falla → es condicionamiento del decoder, y entonces solo funciona con Whisper *(es la respuesta que sostiene tu argumento del gate)*
○ Un párrafo técnico real con cada motor
○ Anotar **dos cosas distintas**: palabras mal (precisión) y cómo se siente el streaming vs el "hablo-espero-aparece"

**Bloque 3 — El entregable · 30 min**
○ **Dictar el ADR-001 de stack completo** — ORM · hosting · auth · testing, cada uno con opciones → trade-off → elección → qué pierdo *(esto es E03 fase C; se contabiliza allá)*

**Bloque 4 — Al bajar · 5 min**
○ Contar **palabras corregidas** por párrafo
○ Buscar **texto inventado**: frases que no dijiste, en las pausas. Es un modo de falla distinto y no lo captura el conteo de palabras
○ Veredicto: ¿el modo bici entra al presupuesto permanente?

---

## 🚫 Fuera de scope (explícito)
```
WezTerm · tmux · Neovim · dotfiles de Kun Chen  → parking: se revisa cuando corras 2-3 agentes
                                                   en paralelo (realista ~S9-S11)
Lavish · Treehouse · No Mistakes                → cuando shippees volumen de PRs
Escribir CÓDIGO por voz                          → nunca. El flujo es para pensar y dictar prosa.
Segundo monitor / rediseño del setup del rodillo → solo si la validación falla POR eso
```

## ⚠️ Trampa conocida
Expandir esto al "setup completo del video". Es migración de entorno disfrazada de productividad, cuesta semanas y no shippea nada. **Timebox duro: 30 minutos para las fases A–C.** Si a los 30 no dictas legible, se congela como esté y se retoma otro día — no se sacrifican los requerimientos ni el modelo de datos por configurar herramientas.

**Trampa de la fase D (añadida 27 jul):** creer que 0 errores en el escritorio predicen 0 errores en el rodillo. No lo predicen, porque el escritorio no tiene ventilador ni respiración a 140 ppm. Y con **toggle + Whisper** el modo de falla no es "palabra mal": es que Whisper, siendo generativo, **inventa texto plausible** sobre silencio y ruido. Un toggle olvidado durante 90 segundos de respiración puede producir un párrafo entero que nunca dijiste — y va a sonar bien, que es lo peligroso. Por eso el bloque 4 revisa por texto inventado, no solo por palabras corregidas.

## 🚪 Comprehension gate
○ Explico por qué elegí Parakeet o Whisper, y qué pierdo con el otro
○ Explico por qué el trigger que elegí es el correcto para pedalear, y cuál descarté

---

## 📝 Log de ejecución
| Fecha | Fase | Pom | Qué pasó |
|---|---|---:|---|
| 27 jul | A, B, C | **1.0** | Instalado y dictando dentro del timebox. El cask de la épica estaba mal (`opensuperwhisper` a secas); el correcto es `my-monkeys/tap/opensuperwhisper` — el fork mantenido, v0.9.6. **Config final y tabla de errores sin registrar.** |
| 28 jul | D | | |

---

## ⚙️ CONFIGURACIÓN VIGENTE (al 27 jul, escritorio)
```
Motor:       Whisper
Idioma:      español fijo
Trigger:     ⌥ Opción, modificador único · descartados: combinación de teclas · botón de mouse
Modo:        TOGGLE (tap para arrancar, tap para parar)
Dictionary:  CARGADO — Whisper, ORM, Drizzle, Postgres, etc.
Errores:     Whisper 0 · Parakeet ⛔ no probado
```

> ⚠️ **Los 0 errores son asistidos, no crudos.** Salieron con el Dictionary ya poblado con la jerga.
> Eso no los invalida — es exactamente el arreglo correcto al modo de falla de la jerga técnica — pero
> cambia la afirmación: *"Whisper acierta mi vocabulario"* no está demostrado; lo demostrado es
> *"Whisper + diccionario acierta mi vocabulario"*.

> ⚠️ **Hueco del comprehension gate.** El gate dice *"explico por qué elegí Whisper **y qué pierdo
> con el otro**"*. Whisper ganó por default: nunca corrió Parakeet. Se cierra en la fase D con un
> A/B de 5 min, en las condiciones que importan (ventilador, respiración) y no en el escritorio.

---

## 🏁 Cierre
- **Estimado vs real:** 3.6 → ___ pom · desviación ___% · **causa:** {…}
- **DoD cumplido:** {sí / parcial — cuáles faltaron}
- **Config final:** motor ___ · mic ___ · trigger ___ · idioma ___
- **Tasa de error del dictado:** ___ palabras corregidas por párrafo
- **Aprendizaje:** {…}
- **Veredicto:** ¿el modo bici entra al presupuesto semanal permanente? {sí / no / con ajustes}
- **Si el veredicto es NO:** qué se recorta de la semana para recuperar los 9.6 pom perdidos: {…}
