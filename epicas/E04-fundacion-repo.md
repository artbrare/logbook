# 🎯 E04 — Fundación del repo: repos · scaffold · plan mode

> **Estado:** 🔄 en curso (fase A cerrada el 27 jul) · **Semana:** 01 · **Días:** lun 27 · mié 29 · **Proyecto:** `loop`
> **Estimado:** 4.5 pom (~113 min) · **Real:** ___ pom
> **Depende de:** ADR-001 de stack (aceptado 28 jul) · **Desbloquea:** E05 — sin API con SQLAlchemy y sin Postgres que persista, el viernes no hay contra qué migrar

> ⚠️ **Re-estimada el 29 jul: 3.0 → 4.5 pom.** El plan del lunes asumía monolito Next.js (fase B, 2.0 pom).
> ADR-001 decidió frontend y API **separados**, Flask en vez de Node, Dockerfile a mano y Postgres en Compose.
> Son dos proyectos y una capa de infra, no un `create-next-app`. El plan original queda registrado para la retro.

---

## 🎯 Objetivo

> Que `loop` levante con un comando los dos proyectos que decidió ADR-001 — frontend Vite+React+TS, API Flask y Postgres persistente — sin una sola línea de dominio escrita.

## 💡 Por qué existe
El viernes E05 traduce el modelo a schema y corre la primera migración. Si ese día arranca instalando Flask y peleándose con un `compose.yml`, el schema no llega y con él se cae la mitad del goal de la semana. Esta épica existe para que el viernes empiece en la línea de salida, no en la caseta.

Y es el vehículo de la prueba de plan mode (E02 fase B): es la única tarea de la semana lo bastante mecánica y lo bastante grande para medir hasta dónde llega el agente con un plan detallado. Ese número sostiene toda la metodología del semestre.

## 🔑 Prerrequisitos
- [x] ADR-001 aceptado — ORM, hosting, auth y testing decididos (28 jul)
- [x] Repos `logbook` y `loop` creados con commit inicial (fase A, 27 jul)
- [ ] `loop/CLAUDE.md` existe — lo crea **E02 fase A 3c**, no esta épica. Fase C solo lo completa. Si el lunes no se escribió, se escribe antes de fase C o el DoD 4 no se puede palomear

---

## ✅ DEFINITION OF DONE

- [ ] `docker compose up` levanta API y Postgres, y `curl localhost:<puerto>/health` devuelve **200**
- [ ] Insertas una fila a mano, corres `docker compose down && docker compose up`, y **la fila sigue ahí** *(volumen nombrado — mitigación obligatoria de ADR-001 §3)*
- [ ] El frontend corre en dev y pinta en pantalla la respuesta de `/health` de la API, **sin error de CORS en consola**
- [ ] `loop/CLAUDE.md` tiene la estructura de carpetas real, los comandos que de verdad existen, y la frontera **ORM para CRUD / Core para agregación** escrita como regla *(ADR-001 §2)*

---

## 📋 Checklist de ejecución

### Fase A — Los dos repos · 1.0 pom (25 min) · **LUN 27** · ✅ cerrada
- [x] `logbook`: `git init`, carpetas `semanas/ epicas/ adr/ templates/`, README, commit inicial
- [x] `loop`: `git init`, README con el estado "en diseño, sin código aún", `.gitignore`, `docs/adr/`, commit inicial
- [ ] ⚠️ Deuda: `logbook/adr/` existe en disco pero está vacío, y git no versiona directorios vacíos — hoy el repo clonado no lo trae. Se cierra solo cuando aterrice el primer ADR del programa

### Fase B — Scaffold en plan mode · 2.5 pom (63 min) · **MIÉ 29**
> El orden importa: **tu plan primero, el del agente después.** Al revés no hay comparación, hay aprobación.

- [ ] Escribir tu plan a mano antes de abrir plan mode: qué carpetas, qué archivos, qué comandos. Es el ground truth contra el que se juzga lo que proponga el agente
- [ ] Correr el scaffold **en plan mode**. Leer el plan completo antes de aprobar y anotar **qué habrías hecho distinto** *(ese dato es de E02 fase B)*
- [ ] `frontend/` — Vite + React + TypeScript, arranca en dev y compila
- [ ] `api/` — Flask con app factory, un blueprint y el endpoint `/health`; dependencias declaradas en archivo, no instaladas a mano
- [ ] La API conecta a Postgres con SQLAlchemy y `/health` reporta si la conexión vive
- [ ] El frontend llama a `/health` y pinta la respuesta *(aquí aparece CORS por primera vez)*
- [ ] Extender `.gitignore` a Python — hoy solo cubre Node: `__pycache__/`, `.venv/`, `*.pyc`, `.env`
- [ ] Un commit por pieza. Un commit de 40 archivos no se puede revisar ni revertir

### Fase C — Contenedores y persistencia · 1.0 pom (25 min) · **MIÉ 29**
> ⏱️ Timebox duro 25 min. Ver la trampa antes de arrancar.

- [ ] `Dockerfile` de la API escrito a mano: base slim, **capa de dependencias separada de la de código**, usuario no-root, gunicorn como entrypoint *(ADR-001 §3 — el punto es ver la plomería)*
- [ ] `compose.yml`: servicio `api` + servicio `postgres`, **volumen nombrado**, credenciales por `.env` ignorado
- [ ] `docker compose up` y `/health` responde desde el contenedor, no desde tu máquina
- [ ] **Prueba de persistencia:** insertar una fila, `down && up`, verificar que sobrevivió → es el DoD 2, y se hace **antes** de que existan datos que importen
- [ ] Completar `loop/CLAUDE.md`: estructura real, comandos reales, frontera ORM/Core

---

## 🚫 Fuera de scope (explícito)
```
Auth con Clerk                          → S2. El scaffold deja el hueco, no lo llena
Tablas de dominio (tasks, categorías)   → E05, viernes. Aquí solo se prueba que la conexión vive
Deploy a Cloudflare Pages               → S3 (15 ago). Dónde corren los contenedores en prod → S2
Tests                                   → ADR-001 §5 pide unit de lógica pura en S1, y todavía no hay lógica
Generar tipos compartidos front↔back    → ADR-001 §1 los deja a mano a propósito. No se busca generador
Rediseñar el .gitignore como monorepo   → se extiende con Python, no se reescribe
```

## ⚠️ Trampa conocida
**El Dockerfile.** Escribirlo a mano es el punto de ADR-001 §3, y también donde se van 40 minutos: un `COPY` en el orden equivocado que invalida la caché en cada build, o un gunicorn que bindea a `127.0.0.1` y desde fuera del contenedor no responde nunca.

**Timebox duro de fase C: 25 min.** Si al minuto 25 `/health` no responde desde el contenedor, se congela ahí: frontend y API corriendo en local ya es lo que E05 necesita el viernes. El Compose pasa como deuda al jueves y **se anota como slip** — no se arrastra en silencio.

**El segundo agujero es CORS.** ADR-001 §0 lo aceptó como costo permanente de separar los proyectos. La primera vez duele; que duela hoy con un `/health` y no el viernes con la migración encima.

**Y la de fase B:** aprobar el plan del agente sin haber escrito el tuyo. Si no hay contra qué comparar, el bloque produce un scaffold pero no produce el dato, y el dato es la mitad de por qué esta épica existe.

## 🚪 Comprehension gate
- [ ] Explico por qué el volumen tiene que ser nombrado y qué pasa exactamente en un `down` si no lo es
- [ ] Explico qué hace cada capa de mi Dockerfile y por qué las dependencias van antes que el código
- [ ] Explico qué es CORS, por qué aparece aquí y no aparecía en un monolito
- [ ] Explico con **mi propio número** hasta dónde llegó el agente con plan detallado vs con un prompt de una línea

---

## 📝 Log de ejecución
| Fecha | Fase | Pom | Qué pasó |
|---|---|---:|---|
| 27 jul | A | 1.0 | Dos repos creados y commiteados. Corrección sobre el plan: son dos repos, no uno — ciclos de vida y audiencias distintas (v5.1 del doc semanal) |
| 29 jul | B, C | | |

---

## 🏁 Cierre
- **Estimado vs real:** 4.5 → ___ pom · desviación ___% · **causa:** {…}
  *(plan original 3.0 pom; re-estimada a 4.5 el 29 jul porque ADR-001 cambió el scaffold. La desviación se mide contra 4.5, pero la retro discute las dos)*
- **DoD cumplido:** {sí / parcial — cuáles faltaron}
- **Dato de plan mode:** {hasta dónde llegó con plan detallado vs prompt de una línea} → alimenta E02 fase B
- **Aprendizaje:** {…}
- **Deuda o seguimiento:** {qué queda pendiente y a dónde se va}
