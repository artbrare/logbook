# 📚 Estudio — Semana 01
### Todo desde cero, sin dar nada por sabido

> **Cómo usar este documento.** Cada tema empieza desde lo más básico y va subiendo.
> Primero se explica todo; **al final de cada tema hay preguntas.**
>
> Lee la explicación completa. Después contesta las preguntas **en voz alta, sin ver arriba**.
> Si titubeas en una, vuelve a leer solo esa parte.
>
> 🚪 = pregunta del comprehension gate del domingo

---

## Índice

1. [Bases de datos: lo mínimo](#tema-1--bases-de-datos-lo-mínimo)
2. [Modelar bien: reglas vs hechos](#tema-2--modelar-bien-reglas-vs-hechos)
3. [Índices](#tema-3--índices)
4. [Contenedores y Docker](#tema-4--contenedores-y-docker)
5. [Cómo funciona un servidor web](#tema-5--cómo-funciona-un-servidor-web)
6. [Hablar con la base desde el código](#tema-6--hablar-con-la-base-desde-el-código)
7. [CORS](#tema-7--cors)
8. [Migraciones](#tema-8--migraciones)
9. [Trabajar con agentes](#tema-9--trabajar-con-agentes)

---

# TEMA 1 · Bases de datos: lo mínimo

## Qué es una base de datos relacional

Un programa que guarda información en **tablas**. Piensa en una hoja de Excel:

```
        ┌─────────┬──────────┬──────────────┐
        │   id    │  nombre  │    email     │  ← COLUMNAS (los campos)
        ├─────────┼──────────┼──────────────┤
FILA →  │  a1b2   │  Arturo  │ art@mail.com │
FILA →  │  c3d4   │  Sarvani │ sar@mail.com │
        └─────────┴──────────┴──────────────┘
```

- **Columna:** un tipo de dato. Todas las filas tienen las mismas.
- **Fila:** un registro. Una persona, una tarea, un día.
- **Tabla:** un conjunto de filas del mismo tipo.

Postgres es la base que estás usando. **"Relacional"** significa que las tablas se conectan entre sí.

## Tipos de dato

Cada columna tiene un tipo, y la base **te impide** guardar algo del tipo equivocado. Eso es una ventaja: los errores salen al escribir, no meses después al leer.

```
text           texto de cualquier largo
int            número entero
numeric(5,2)   número con decimales EXACTOS (dinero, porcentajes)
boolean        true o false
date           una fecha: 2026-07-29
timestamptz    fecha + hora + zona horaria
uuid           identificador único largo: a1b2c3d4-e5f6-...
enum           una lista cerrada de valores permitidos
vector         una lista de números (para IA — ver abajo)
```

**Por qué `numeric` y no decimales normales:** los decimales de punto flotante tienen errores de redondeo. `0.1 + 0.2` no da exactamente `0.3`. Para porcentajes y dinero se usa `numeric`, que es exacto.

## Primary Key (PK) — la llave primaria

La columna que **identifica una fila de forma única**. No se repite y no puede estar vacía.

```
Autoincremental    1, 2, 3, 4...        corto, pero adivinable
UUID               a1b2c3d4-e5f6-...    largo, imposible de adivinar
```

**Loop usa UUID.** Con IDs autoincrementales alguien podría entrar a `/tarea/1`, `/tarea/2`, `/tarea/3` y descubrir cuántas tareas existen, o ver las de otro usuario. Con UUID no hay "siguiente número" que probar.

## Foreign Key (FK) — la llave foránea

Una columna que **apunta a la PK de otra tabla**. Así se conectan.

```
usuarios                    registros_diarios
┌──────┬─────────┐         ┌──────┬─────────────┬────────────┐
│ id   │ nombre  │         │ id   │ usuario_id  │ nombre     │
├──────┼─────────┤         ├──────┼─────────────┼────────────┤
│ a1b2 │ Arturo  │◄────────│ x9y8 │ a1b2        │ Leer       │
└──────┴─────────┘   FK    │ z7w6 │ a1b2        │ Alopurinol │
                           └──────┴─────────────┴────────────┘
```

**Lo que te da gratis:** la base **rechaza** guardar un registro cuyo `usuario_id` no existe. Sin FK acabarías con registros huérfanos que apuntan a nada.

## `NOT NULL` y `NULLABLE`

- **`NOT NULL`** = siempre tiene valor. La base rechaza filas sin él.
- **`NULLABLE`** = puede estar vacía.

**`NULL` no es cero ni cadena vacía. Es "no hay valor".** Y eso importa: `WHERE peso = NULL` **nunca** encuentra nada, porque `NULL` no es igual a nada, ni a sí mismo. Hay que escribir `WHERE peso IS NULL`.

```
usuario_id            NOT NULL   ← todo registro tiene dueño
tarea_programada_id   NULLABLE   ← puede no venir de una regla
```

## `UNIQUE` y `CHECK`

```
UNIQUE (usuario_id, fecha)        no hay dos reportes del mismo día
CHECK (peso BETWEEN 1 AND 5)      el peso solo puede ser 1..5
```

Son **restricciones**. La base las hace cumplir **siempre**, aunque tu código tenga un bug. Es la última línea de defensa: la validación en el código se puede saltar, la de la base no.

## Los tipos de relación

```
UNO A MUCHOS      un usuario tiene muchas tareas
                  la FK vive en el lado "muchos"

MUCHOS A MUCHOS   un alumno tiene muchos cursos y un curso muchos alumnos
                  necesita una tabla intermedia

AUTO-REFERENCIA   una categoría tiene una categoría padre
                  la FK apunta a la MISMA tabla
```

## Categorías anidadas — la auto-referencia

Loop necesita `Personal › Medicinas › Alopurinol`. Se hace con una FK que apunta a la propia tabla:

```
┌──────┬──────────────┬─────────────────────┐
│ id   │ nombre       │ categoria_padre_id  │
├──────┼──────────────┼─────────────────────┤
│ 001  │ Personal     │ NULL         ← raíz │
│ 002  │ Medicinas    │ 001                 │
│ 003  │ Entrenos     │ 001                 │
└──────┴──────────────┴─────────────────────┘
```

Se llama **lista de adyacencia**: cada fila sabe quién es su papá.

**El problema que trae:** *"dame todo lo que cuelga de Personal, incluyendo subcategorías"* deja de ser una consulta simple. Con profundidad ilimitada haría falta una **CTE recursiva** — una consulta que se llama a sí misma hasta llegar al fondo del árbol.

**Por eso Loop limita a 3 niveles:** con 3 fijos se resuelve con dos JOINs normales, sin recursión.

## `vector` y embeddings — para qué

Un **embedding** es una lista de números que representa el *significado* de un texto. Textos con significado parecido dan números parecidos:

```
"Tomar Alopurinol"   → [ 0.21, -0.44,  0.87, ...]
"Tomar vitamina C"   → [ 0.19, -0.41,  0.85, ...]   ← muy parecido
"Hacer deploy"       → [-0.77,  0.12, -0.33, ...]   ← muy distinto
```

**Para qué en Loop:** escribes *"tomar omega 3"*, el sistema busca cuál categoría existente tiene el embedding más cercano, y la asigna sola. Capturar sin fricción.

**`pgvector`** es la extensión de Postgres que permite guardar esos vectores y comparar cercanía.

---

## ❓ Preguntas del tema 1

1. ¿Qué es una primary key y por qué Loop usa UUID en vez de números?
2. ¿Qué te da gratis una foreign key que no tendrías sin ella?
3. ¿Cuál es la diferencia entre `NULL`, `0` y `""`?
4. ¿Por qué `WHERE peso = NULL` nunca encuentra nada?
5. ¿Para qué sirve un `CHECK` si tu código ya valida?
6. ¿Cómo se modela una categoría que tiene categoría padre? ¿Cómo se llama ese patrón?
7. ¿Por qué limitar el anidamiento a 3 niveles?
8. ¿Qué es un embedding y para qué lo usa Loop?

---

# TEMA 2 · Modelar bien: reglas vs hechos

## El problema

Quieres guardar *"tomar Alopurinol todos los días"*.

**Intento 1 — una fila por día del año**

```
2026-01-01  Alopurinol  ☐
2026-01-02  Alopurinol  ☐
...  365 filas
```

Problemas: llenas la base de cosas que no pasaron. ¿Y el año que entra? Y si en marzo lo cambias a solo entre semana, ¿borras las futuras? ¿las pasadas ya palomeadas se quedan?

**Intento 2 — una sola fila**

```
Alopurinol · todos los días
```

Problema: ¿cómo marcas que **hoy** sí y **ayer** no? Una fila solo tiene un estado.

## La solución: dos tablas

```
┌────────────────────────────────────────────────────────┐
│  tareas_programadas  →  LA REGLA                       │
│  "Alopurinol, días [0,1,2,3,4,5,6]"                    │
│  NO tiene fecha. Una sola fila, para siempre.          │
└────────────────────────────────────────────────────────┘
                          │
                          │  genera, día por día
                          ▼
┌────────────────────────────────────────────────────────┐
│  registros_diarios   →  EL HECHO                       │
│  "29 jul · Alopurinol · completado · 8:03am"           │
│  SÍ tiene fecha. Una fila por día.                     │
└────────────────────────────────────────────────────────┘
```

**El concepto:** una tarea recurrente **es una regla**, y las reglas no tienen fecha. Completarla **es un evento**, y los eventos sí.

**Qué cuesta:** hay que crear los registros del día al arrancar la app. Es trabajo real y es un punto de fallo — si eso no corre, el día aparece vacío.

**Qué resuelve:** si la regla cambia en marzo, editas la regla. Los registros ya generados son historia y no se tocan.

## Segundo concepto: guardar vs calcular

El "peso" de una tarea (qué tan difícil es, 1 a 5) vive en `tareas_programadas`. Cuando calculas el score de un día, ¿lo lees de ahí, o lo copias al registro?

**Si lo lees de la tarea programada:**

```
Enero:  leer vale 2  →  tu score de enero fue 78%
Marzo:  decides que leer vale 3
        →  tu score de enero AHORA dice 81%
```

**Tu pasado cambió solo.** Y un sistema que reescribe su propio pasado no sirve para medir progreso, que es todo el punto de Loop.

**Si lo copias al registro diario:** cada registro guarda el peso vigente ese día. Enero sigue diciendo 78% para siempre.

> **La regla general:** los datos que forman parte de un **acuerdo** se copian en el momento del acuerdo. Los que se **derivan** se calculan.

El mismo patrón en otro dominio: una biblioteca cobra $10/día de multa. Si subes la tarifa a $15 en marzo, las multas de enero **no** deben recalcularse. Se copia la tarifa al préstamo.

**Y el revés también es un error.** El número de pomodoros completados **no** se guarda: se cuenta. Si guardaras `terminados = 3` **y además** las tres sesiones, tendrías el mismo hecho en dos lugares — y se van a desincronizar.

```
COPIAR    lo que era cierto en un momento y debe congelarse
CALCULAR  lo que se deriva de otros datos que ya tienes
```

## Tercer concepto: un booleano casi nunca alcanza

`completado: true/false`. Son las 10 de la mañana y una tarea de hoy está en `false`. ¿Qué significa?

```
"no la hice"      ← se acabó el día y no pasó
"todavía no"      ← son las 10am, apenas empieza
"no había dato"   ← la nota original nunca registró estado
```

**Tres significados, un solo valor.** Y la detección de leaks depende de distinguirlos: si todo es `false`, no puedes saber qué llevas cinco días arrastrando.

Solución: `enum('pendiente','completado','no_completado')`.

**El caso real:** las semanas 29 y 30 de tus notas usaban `*` sin corchetes, sin estado. Importarlas como `false` haría creer al sistema que no hiciste nada en dos semanas — cientos de falsos positivos.

## Cuarto: por qué las FK importan para los leaks

La feature central de Loop es detectar que algo lleva cinco días sin hacerse. ¿Cómo sabes que el registro del lunes y el del viernes son **la misma tarea**?

**Comparar el nombre es frágil.** En tus datos reales la misma tarea aparece como:

```
"Dedicar 2 pomodoros al plan"
"Dedicar 0/2 pomodoros"
"2/2 pomodoros"
```

Por eso existe `tarea_programada_id`: todos los registros que vienen de la misma regla apuntan al mismo ID, sin importar cómo se escribió el nombre.

---

## ❓ Preguntas del tema 2

1. 🚪 ¿Cómo modelas una tarea que se repite a diario sin crear 365 filas? ¿Qué cuesta tu solución?
2. 🚪 Si cambias el peso de una tarea hoy, ¿qué le pasa al score de hace tres semanas? ¿Por qué?
3. ¿Cuándo se copia un dato y cuándo se calcula? Un ejemplo de cada uno.
4. ¿Por qué el número de pomodoros terminados se cuenta en vez de guardarse?
5. ¿Por qué un booleano no alcanza para "¿lo hice?"
6. ¿Por qué comparar por nombre no sirve para detectar leaks?

---

# TEMA 3 · Índices

## Qué es un índice

Imagina un libro de 1,000 páginas sin índice. Para encontrar "fotosíntesis" tienes que leer página por página. Eso es un **table scan**: la base revisa todas las filas.

Un índice es una estructura **ordenada** que vive aparte y dice dónde está cada cosa. Con índice, la base salta directo.

```
SIN índice, 1 millón de filas   →  revisa 1,000,000
CON índice                      →  revisa ~20
```

**Qué cuesta:** ocupa espacio en disco, y **cada `INSERT` tiene que actualizarlo**. Por eso no se indexa todo: se indexan las columnas por las que **buscas**.

## Índice compuesto

Un índice sobre **dos o más columnas**. Y aquí lo importante: **el orden de las columnas cambia todo.**

Piensa en un directorio telefónico ordenado por **apellido, luego nombre**:

```
García, Ana
García, Luis
García, Pedro
López, Ana
López, Beto
```

- *"Todos los García"* → fácil, están juntos.
- *"Todos los que se llaman Ana"* → imposible sin leer todo. Las Anas están dispersas.

**Un índice compuesto solo agrupa por la primera columna.**

## Tu caso concreto

```sql
WHERE usuario_id = 'a1b2' AND fecha BETWEEN '2026-07-01' AND '2026-07-31'
```

```
(usuario_id, fecha)          (fecha, usuario_id)
─────────────────────        ─────────────────────
A · 2026-01-01               2026-01-01 · A
A · 2026-01-02               2026-01-01 · B
A · 2026-01-03               2026-01-02 · A
B · 2026-01-01               2026-01-02 · B
```

- **`(usuario_id, fecha)`** → salta a `(A, julio-1)` y **lee seguido** hasta `(A, julio-31)`. Un bloque contiguo.
- **`(fecha, usuario_id)`** → dentro de cada fecha, las filas de A están mezcladas con B y C. Entra y sale en cada día.

## La regla

> **Columnas de igualdad primero. Columnas de rango al final.**

- `usuario_id = ?` es **igualdad** — un valor exacto
- `fecha BETWEEN x AND y` es **rango** — muchos valores

**Por qué:** en cuanto el índice llega a una columna de rango, **ya no puede usar las siguientes para buscar** — solo para descartar lo que ya leyó. Poner el rango primero desperdicia todo lo que viene después.

## `EXPLAIN` — cómo ver qué hace Postgres

`EXPLAIN` le pregunta a Postgres *"¿cómo piensas resolver esto?"* sin ejecutarlo.

```
Seq Scan     ← leyó TODA la tabla. Sin índice, o el índice no sirvió.
Index Scan   ← usó un índice. Bien.

Index Cond:  ← las condiciones que usó para BUSCAR dentro del índice
Filter:      ← las que aplicó DESPUÉS, descartando filas ya leídas
```

**La señal de que el orden está bien:** las **dos** condiciones dentro de `Index Cond`.

```
✅ Index Cond: (usuario_id = ... AND fecha >= ... AND fecha <= ...)

❌ Index Cond: (fecha >= ... AND fecha <= ...)
   Filter: (usuario_id = ...)      ← leyó de más y descartó
```

---

## ❓ Preguntas del tema 3

1. ¿Qué es un índice y qué cuesta tenerlo?
2. 🚪 `(usuario_id, fecha)` o `(fecha, usuario_id)`? Explica el **mecanismo**, no solo la respuesta.
3. ¿Por qué el orden de las columnas importa? Usa la analogía del directorio.
4. ¿Qué diferencia hay entre `Index Cond` y `Filter`?
5. ¿Qué significa `Seq Scan` y por qué es mala señal en una tabla grande?

---

# TEMA 4 · Contenedores y Docker

## El problema que resuelve

Tu app necesita Python 3.12, Postgres 16, y ciertas librerías con versiones específicas. Instalar todo eso a mano en cada máquina es lento y frágil — *"en mi máquina sí funciona"* viene de ahí.

Un **contenedor** empaqueta la app **y todo lo que necesita**. En cualquier máquina con Docker, corre igual.

## Imagen vs contenedor

```
IMAGEN       la receta congelada. Un archivo. No corre.
CONTENEDOR   la imagen ejecutándose. Se crea y se borra muchas veces.
```

Analogía: la imagen es la clase, el contenedor es la instancia.

## El Dockerfile y las capas

El `Dockerfile` son las instrucciones para construir la imagen. **Cada instrucción crea una capa.**

```dockerfile
FROM python:3.12-slim         ← capa 1: el sistema base
COPY pyproject.toml uv.lock   ← capa 2: la lista de dependencias
RUN uv sync                    ← capa 3: instalarlas
COPY . .                       ← capa 4: tu código
CMD ["gunicorn", ...]          ← qué corre al arrancar
```

**Docker cachea las capas.** Si una capa no cambió, la reutiliza. **Pero invalida todas las capas posteriores a la primera que cambió.**

Por eso el orden importa:

```
✅ dependencias ANTES del código
   Cambias una línea de código → solo se rehace la capa 4.
   La instalación (lenta) se reutiliza.

❌ código ANTES de las dependencias
   Cambias una línea → se invalida la instalación →
   reinstalas 23 paquetes en CADA build.
```

**`slim`** = imagen base recortada, sin herramientas que no necesitas. Menos peso y menos superficie de ataque.

**`USER appuser`** hace que el proceso corra sin ser root. Si alguien logra ejecutar código dentro del contenedor, no tiene permisos de administrador. **Va después de instalar**, porque instalar necesita permisos de escritura.

## Volúmenes — lo que sobrevive

Un contenedor tiene una **capa de escritura efímera**. Todo lo que escribe adentro **muere con el contenedor**.

Postgres guarda su base en `/var/lib/postgresql/data`. Sin volumen, esa carpeta es parte de la capa efímera:

```
docker compose down  →  contenedor eliminado  →  base perdida
```

Un **volumen** es almacenamiento que Docker administra **por separado**:

```yaml
volumes:
  - loop_pgdata:/var/lib/postgresql/data
```

*"Lo que se escriba en esa ruta no va a la capa del contenedor, va al volumen `loop_pgdata`."*

```
down  →  contenedor muere  →  el volumen SIGUE AHÍ
up    →  contenedor NUEVO  →  monta el MISMO volumen  →  datos intactos
```

**El contenedor es desechable. El volumen no.**

**Nombrado vs anónimo:**

```
loop_pgdata:/var/lib/...   ← NOMBRADO: identidad estable, respaldable
/var/lib/...               ← ANÓNIMO: ID aleatorio, se vuelve basura huérfana
```

**⚠️ Lo que sí lo destruye:** `docker compose down -v`. Por eso ese comando tiene nombre propio y advertencia en el Makefile (`make db-nuke`).

**Verlo:** `docker volume ls` y `docker volume inspect`. El `Mountpoint` existe incluso con los contenedores apagados.

## Puertos y `0.0.0.0`

Cada contenedor tiene su propia red. Para llegar desde tu máquina hay que **publicar** un puerto:

```yaml
ports: ["8000:8000"]    # puerto de tu máquina : puerto del contenedor
```

Pero eso no basta. El proceso adentro tiene que escuchar en la interfaz correcta:

```
--bind 127.0.0.1:8000   ❌ loopback DEL CONTENEDOR.
                           Solo acepta conexiones nacidas adentro.
--bind 0.0.0.0:8000     ✅ todas las interfaces.
```

**El síntoma cuando está mal es engañoso:** el contenedor "corre", los logs se ven bien, y `curl` se queda colgado sin error.

## Docker Compose

Un archivo (`compose.yml`) que describe **varios contenedores juntos** y cómo se hablan.

**Red interna:** Compose crea una red donde cada servicio es alcanzable **por su nombre**. Por eso la API se conecta a `postgres:5432`, no a `localhost:5432` — dentro de esa red, `postgres` es un nombre de host real.

## Healthcheck — por qué no basta `depends_on`

```yaml
depends_on:
  postgres:
    condition: service_healthy
```

`depends_on` sin condición solo espera a que el contenedor **arranque**, no a que el servicio esté **listo**. Postgres, en su primer arranque, crea el cluster antes de empezar a escuchar.

**Sin la condición: la primera vez falla y la segunda funciona.** Ese es el peor tipo de bug — el que se arregla reintentando, así que nunca se diagnostica.

`healthcheck` con `pg_isready` le da a Docker una forma de saber cuándo Postgres realmente acepta conexiones.

**Por qué no un `sleep`:** un sleep adivina. En una máquina lenta no alcanza.

---

## ❓ Preguntas del tema 4

1. ¿Diferencia entre imagen y contenedor?
2. 🚪 ¿Por qué las dependencias van antes que el código en el Dockerfile?
3. 🚪 ¿Por qué el volumen tiene que ser nombrado, y qué pasa en un `down` si no lo es?
4. ¿Qué comando sí destruye el volumen?
5. ¿Por qué `0.0.0.0` y no `127.0.0.1`? ¿Cuál es el síntoma cuando está mal?
6. ¿Por qué la API se conecta a `postgres:5432` y no a `localhost:5432`?
7. ¿Por qué `healthcheck` y no un `sleep`?
8. ¿Por qué el `USER appuser` va después de instalar?

---

# TEMA 5 · Cómo funciona un servidor web

## El ciclo request/response

```
1. El navegador manda un REQUEST:   GET /health
2. El servidor lo recibe y ejecuta código
3. El servidor manda un RESPONSE:   200 {"status":"ok"}
```

**Códigos de estado:**

```
200  OK
201  creado
400  el cliente mandó algo mal
401  no autenticado (no sé quién eres)
403  autenticado pero sin permiso
404  no existe
500  el servidor tronó
503  el servidor vive pero no puede atender ahora
```

## Qué es Flask

Un **framework web**: una librería que recibe requests, decide qué código correr según la URL, y arma la respuesta.

```python
@app.route("/health")
def health():
    return {"status": "ok"}
```

Ese decorador dice *"cuando llegue un request a `/health`, corre esta función"*. Eso es una **ruta**.

## Blueprints

Con 40 rutas, ponerlas todas en un archivo es inmanejable. Un **blueprint** es un grupo de rutas en un módulo, que después se **registra** en la app.

```python
health_bp = Blueprint("health", __name__)   # el grupo

@health_bp.route("/health")                  # rutas del grupo
def health(): ...

app.register_blueprint(health_bp)            # se conecta a la app
```

Es el equivalente de un *router* en Express.

## El app factory

Lo normal en tutoriales de Flask:

```python
app = Flask(__name__)      # ← esto corre al IMPORTAR el archivo
```

La app existe **como efecto secundario de importar**. Tres problemas:

**1. Tests.** Una sola app con una sola config. No puedes tener una app apuntando a una base de prueba y otra a la real.

**2. Importaciones circulares.** Tus rutas necesitan `app` para el decorador, y `app` necesita las rutas para registrarlas. Se importan mutuamente y Python truena. Es *el* dolor clásico de Flask.

**3. Estado global.** Si abres Postgres ahí, eso pasa al **importar**. Si falla, falla en un `import`, no al arrancar.

**La solución — una función que devuelve la app:**

```python
def create_app(config_class=ProductionConfig) -> Flask:
    app = Flask(__name__)
    app.config.from_object(config_class)
    ...
    return app
```

Nada existe hasta que alguien la llama. Puedes crear dos con configs distintas. **La config entra como argumento.**

**Detalle de diseño:** el default es `ProductionConfig`, la más estricta. Si alguien olvida especificar, cae en lo seguro, no en lo permisivo.

`app.config.from_object()` lee los atributos en MAYÚSCULAS de la clase y los mete a `app.config`, para que cualquier ruta los lea sin importar el módulo de config.

## Hooks: `after_request` y `teardown_appcontext`

Flask te deja registrar funciones que corren **automáticamente** en momentos del ciclo:

```python
@app.after_request           # después de cada respuesta, puede modificarla
def add_cors_headers(response): ...

app.teardown_appcontext(close_session)   # al terminar CADA request
```

**Lo importante de `teardown`:** corre aunque la ruta lance excepción, aunque devuelva 500. **Eso hace imposible olvidar cerrar la sesión** — no depende de que cada ruta se acuerde de un `try/finally`.

## `g` — el objeto por request

`g` es un objeto que Flask crea y destruye con cada request.

```python
g.db_session = SessionFactory()
```

**Dos requests simultáneos tienen dos `g` distintos**, sin importar cuántos hilos haya. Por eso guardar la sesión ahí no se pisa entre usuarios.

## WSGI y gunicorn

**WSGI** es el estándar de cómo un servidor web habla con una app de Python. Flask **habla** WSGI; Flask no **es** un servidor.

Flask trae un servidor de desarrollo (`flask run`) que **no sirve para producción**: un solo proceso, lento, sin manejo de carga.

**gunicorn** es un servidor WSGI de producción: corre varios *workers* (procesos) de tu app y reparte los requests entre ellos.

```python
# wsgi.py
app = create_app()      # ← gunicorn busca este objeto
```

**Decisión que se tomó:** usar gunicorn también en desarrollo, con `--reload`. Así el servidor que corres es el mismo que corre en producción. Se pierde el debugger bonito de Flask, se gana no tener sorpresas el día del deploy.

## Liveness vs readiness — los dos health checks

```
GET /health          ¿el proceso vive?   NO toca la base.
GET /health/ready    ¿puede atender?     Hace SELECT 1 a Postgres.
```

**Por qué separados, y esto es lo importante:**

Si tu único health check consulta la base y Postgres se cae, el orquestador cree que **tu API** está mal y **la reinicia en bucle** — cuando la API está perfecta y el problema es otro servicio.

```
liveness falla   →  "reiníciame"
readiness falla  →  "sácame del balanceador, pero NO me reinicies"
```

Es el patrón que Kubernetes usa, y es lo que vas a configurar en S9-S11.

---

## ❓ Preguntas del tema 5

1. ¿Qué es un request y un response? ¿Qué significan 200, 401, 403, 404, 500, 503?
2. ¿Qué es un blueprint y qué problema resuelve?
3. 🚪 ¿Por qué `create_app()` en vez de `app = Flask(__name__)`? Da los tres problemas.
4. ¿Qué es `g` y por qué sirve para tener algo por request?
5. ¿Qué hace `teardown_appcontext` y por qué se **registra** en vez de llamarse?
6. ¿Qué es WSGI? ¿Por qué gunicorn y no `flask run` en producción?
7. ¿Por qué dos endpoints de salud y no uno? Da el escenario de falla.

---

# TEMA 6 · Hablar con la base desde el código

## Las tres formas

```
SQL A PELO   escribes el SQL y mapeas los resultados a mano
CORE         un constructor de SQL en Python: select(), where(), join()
ORM          las filas se vuelven objetos de Python
```

Un **ORM** (Object-Relational Mapper) convierte filas en objetos:

```python
# ORM
task = session.get(Task, task_id)
task.nombre = "nuevo"
session.commit()

# Core (SQL casi literal)
session.execute(select(tasks).where(tasks.c.id == task_id))
```

**SQLAlchemy tiene las dos capas** y puedes mezclarlas. Esa es su ventaja sobre las alternativas.

## Engine: uno por proceso

El **engine** no es una conexión. Es un **pool** de conexiones más la lógica para hablar con Postgres.

Abrir una conexión cuesta: handshake TCP + autenticación, unos milisegundos. Con 100 requests por segundo eso te mata. Así que se abren unas cuantas al arrancar y **se reutilizan**.

Analogía: el engine es una centralita con 10 líneas telefónicas ya abiertas.

```python
engine = create_engine(url, pool_pre_ping=True)   # UNA VEZ al arrancar
```

**`pool_pre_ping=True`:** antes de entregar una conexión del pool, manda un `SELECT 1` para confirmar que vive. Si está muerta, la descarta y abre otra.

**Sin eso:** reinicias Postgres, el pool sigue guardando conexiones muertas, y el siguiente request truena con un error confuso.

**Ya lo comprobaste:** apagaste Postgres (`/health/ready` → 503), lo prendiste, y volvió a 200 **solo**. Eso fue `pool_pre_ping`. Sin él, seguirías en 503 hasta reiniciar la API.

## Sesión: una por request

Una **sesión** es una **unidad de trabajo**. Toma una conexión del pool, hace lo suyo, hace commit o rollback, y la devuelve.

Analogía: la sesión es una llamada telefónica.

### Qué es una transacción

Un grupo de operaciones que se aplican **todas o ninguna**.

```
BEGIN
  INSERT tarea A
  INSERT tarea B
COMMIT      ← ahora las dos existen
```

Si algo falla antes del commit, `ROLLBACK` deshace todo. **No quedan estados a medias.**

### Por qué UNA sesión POR REQUEST

Una sesión mantiene **una transacción abierta**. Si dos requests comparten sesión:

```
Request A   INSERT tarea
Request B   algo falla → ROLLBACK
            ↑ acaba de borrar el INSERT de A
```

**Segunda razón, más sutil:** la sesión tiene un **identity map** — un caché de los objetos que ya cargó. Con sesión compartida, A puede leer objetos de B que **todavía no se commitearon**. Estás leyendo datos que quizá nunca existan.

### El engine sí es global, la sesión no

```
ENGINE    pool de conexiones · caro de crear · seguro de compartir
SESSION   transacción abierta · barato de crear · NO se comparte
```

## El cierre — y qué pasa si lo olvidas

```python
def close_session(exc=None):
    session = g.pop("db_session", None)
    if session is not None:
        session.close()
```

`close()` hace dos cosas: **devuelve la conexión al pool** y **descarta cualquier transacción sin commitear**.

### Por qué NO hay `commit()` aquí

Las rutas commitean explícitamente. Si un request escribió tres filas y falló en la cuarta, `close()` descarta las tres.

**Un commit automático en el teardown guardaría trabajo a medias**, y eso es peor que perderlo: deja una base inconsistente que nadie sabe que existe.

### Si no cierras

La conexión no vuelve al pool y queda ocupada. Con pool de 10 y 10 fugas, **el request 11 espera para siempre.**

**El síntoma es lo peligroso: la app no truena, se cuelga.** Funciona horas y de pronto deja de responder, sin error en los logs. Es una de las fallas de producción más difíciles de diagnosticar.

## Carga diferida, N+1, y `expire_on_commit`

**Carga diferida (lazy loading):** con el ORM, `task.categoria` puede disparar una query **cuando accedes al atributo**, no antes. Si haces eso dentro de un bucle de 200 tareas, son 200 queries extra. Eso es el **problema N+1**: 1 query para la lista, N para los detalles.

**`expire_on_commit`:** el default (`True`) marca todos los objetos como caducados después de un `commit()`. El siguiente acceso a cualquier atributo dispara un `SELECT` nuevo.

```python
task = create_task(...)
session.commit()
return {"id": task.id}   # ← con True, esto hace OTRA query
```

Con `False`, los objetos conservan sus valores y no hay query extra.

**Qué pierdes:** si Postgres calcula algo al insertar (un default, un trigger), no lo vas a ver sin refrescar explícitamente.

## La frontera ORM/Core de Loop

```
ORM   → CRUD: tasks, categorías, semanas. Código repetitivo.
CORE  → toda agregación: score ponderado, detección de leaks.
```

**Por qué.** Con un agente escribiendo el código, el ahorro de teclas del ORM se vuelve irrelevante — **pero su opacidad no.** Carga diferida, N+1, identity map: eso hay que entenderlo aunque no hayas escrito una línea. Y si no lo escribiste, **es menos probable que notes el N+1.**

Con Core, la query que ves en el archivo es la que va a la base.

**Cómo se defiende la frontera:** dos carpetas, `repositories/` (ORM) y `queries/` (Core). La regla pasa de disciplina a **ubicación** — una agregación mal puesta se ve en el diff sin leer una línea de SQL.

---

## ❓ Preguntas del tema 6

1. ¿Qué es un ORM? ¿Y qué es Core?
2. ¿Qué es el engine y por qué es global?
3. ¿Qué es una transacción? ¿Qué hacen `commit` y `rollback`?
4. 🚪 ¿Por qué una sesión por request y no una global? Da el escenario de falla.
5. 🚪 ¿Por qué `close()` y no `commit()` en el teardown?
6. ¿Qué pasa si no cierras la sesión? Describe el síntoma exacto.
7. ¿Qué hace `pool_pre_ping` y cómo lo comprobaste hoy?
8. ¿Qué es el problema N+1?
9. ¿Qué hace `expire_on_commit=False` y qué pierdes?
10. ¿Cuál es la frontera ORM/Core y cómo se defiende físicamente?

---

# TEMA 7 · CORS

## Qué es un "origen"

```
https://loop.app:443
└─┬──┘  └───┬───┘ └┬┘
esquema    host   puerto
```

**Si cambia cualquiera de los tres, es otro origen.**

```
http://localhost:5173   ← tu frontend
http://localhost:8000   ← tu API
        ↑ mismo host, PUERTO distinto → ORÍGENES DISTINTOS
```

## La política de mismo origen

Por default, **el navegador no deja que el JavaScript de un origen lea respuestas de otro.**

Existe para protegerte: sin ella, cualquier página que abras podría hacer requests a tu banco usando tus cookies y **leer** las respuestas.

## Qué es CORS

**Cross-Origin Resource Sharing.** El mecanismo con el que un servidor dice *"permito que este origen lea mis respuestas"*, mediante headers:

```
Access-Control-Allow-Origin: http://localhost:5173
```

## 🔑 El concepto que casi todos entienden mal

> **CORS no lo aplica tu servidor. Lo aplica el NAVEGADOR.**

Tu API **siempre** contesta, con o sin headers. Lo que cambia es si el JavaScript del navegador tiene **permiso de leer** la respuesta.

Por eso:

```bash
curl -H "Origin: http://evil.example" localhost:8000/health
# ← te devuelve el JSON completo
```

`curl` no es un navegador y no obedece CORS.

**Corolario: CORS no es seguridad del servidor.** Es una protección del navegador **para el usuario**. Si necesitas seguridad real, va en el servidor: autenticación y autorización. **CORS no protege tu API de nadie con `curl`.**

## `Vary: Origin`

La respuesta **cambia** según el header `Origin` que llegó.

Sin `Vary: Origin`, un caché intermedio podría guardar la respuesta generada para `localhost:5173` y servírsela a otro origen — con el header de permiso equivocado adentro.

`Vary` le dice al caché: *"esta respuesta depende de ese header, guarda una versión por valor"*.

## Preflight

Para algunas requests, el navegador manda **primero** un `OPTIONS` preguntando si tiene permiso. Eso es el **preflight**.

**No siempre pasa.** Un `GET` sin headers custom es una *simple request* y va directo. **Por eso hoy bastaron 4 líneas.**

**Cuándo aparece:** métodos distintos de GET/POST/HEAD, o headers custom.

**En S2 va a pasar:** cuando Clerk mande `Authorization: Bearer ...`, la request deja de ser simple. Ahí hacen falta `Access-Control-Allow-Methods`, `Allow-Headers` y `Max-Age`.

**Lo que nunca hará falta aquí:** `Access-Control-Allow-Credentials`, porque Clerk va por header y no por cookies. Ese es donde CORS se vuelve realmente peludo.

## ⚠️ La trampa de `in`

```python
if origin in app.config["CORS_ORIGINS"]:
```

Si `CORS_ORIGINS` es una **lista**, `in` compara elementos completos. Bien.

Si es un **string**, `in` compara **substrings**:

```python
"http://localhost:517" in "http://localhost:5173"   # → True 😬
```

Un origen parcial pasaría la validación.

**Prueba de 10 segundos:**
```bash
curl -is -H "Origin: http://localhost:517" localhost:8000/health | grep -i access-control
```
Sin salida = bien. Con header = bug.

---

## ❓ Preguntas del tema 7

1. ¿Qué es un "origen"? ¿`localhost:5173` y `localhost:8000` son el mismo?
2. ¿Qué es la política de mismo origen y por qué existe?
3. 🚪 ¿Qué es CORS, **quién lo aplica**, y por qué `curl` recibe la respuesta igual?
4. ¿Para qué sirve `Vary: Origin`?
5. ¿Qué es un preflight y cuándo se dispara? ¿Por qué hoy no apareció?
6. ¿Por qué `origin in CORS_ORIGINS` es peligroso si es un string?

---

# TEMA 8 · Migraciones

## Qué es una migración

Un archivo que describe **un cambio al esquema** de la base: crear una tabla, agregar una columna, crear un índice.

**Por qué no ejecutar SQL a mano:** porque necesitas que la base de tu compañero, la de tests y la de producción terminen **exactamente igual** que la tuya. Las migraciones son un historial versionado y ordenado que cualquiera puede aplicar.

```
001_crear_usuarios.py
002_crear_tareas.py
003_agregar_indice.py
```

Cada base guarda **qué migraciones ya aplicó** (en una tabla `alembic_version`), así que aplicar solo corre las que faltan.

## Alembic

La herramienta de migraciones de SQLAlchemy.

```
alembic upgrade head              aplica las que faltan
alembic revision --autogenerate   compara tus modelos vs la base y genera el archivo
alembic current                   qué versión tiene esta base
```

**`--autogenerate` no es magia:** compara los modelos que **están importados** contra el estado real de la base. Si un modelo no está importado donde Alembic mira, **no lo ve** y la migración sale incompleta. Es la trampa clásica.

## Paridad dev/prod

**Este concepto tumba deploys**, y hoy lo viste en vivo.

El agente creó la extensión `vector`, comprobó que funciona, **y la borró a propósito**.

Por qué: si tu base de desarrollo tiene algo que **ninguna migración creó**, tu migración va a funcionar **en tu máquina** y fallar en una base limpia — producción, o el clon de otra persona.

> **Si tu base de desarrollo tiene estado que ninguna migración produjo, tus migraciones mienten.**

Por eso la migración del viernes debe empezar con:

```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

## `naming_convention`

Cuando creas un `CHECK` o un `UNIQUE` sin nombre, **Postgres le inventa uno**. Cuando una migración futura quiera borrarlo, Alembic no sabe cómo se llama.

`naming_convention` le da a SQLAlchemy un patrón para nombrarlos siempre igual.

**Por qué hoy y no después — costo asimétrico:** son 5 líneas ahora. Agregarlo después **no renombra lo ya creado**, y quedas con dos convenciones en la misma base.

## `.gitkeep`

**Git no versiona directorios vacíos.** Sin un archivo adentro, `migrations/versions/` no existe en el commit, y en un clon limpio Alembic no encuentra dónde escribir.

Es la misma deuda que ya tienes anotada con `logbook/adr/`.

---

## ❓ Preguntas del tema 8

1. ¿Qué es una migración y por qué no ejecutar SQL a mano?
2. ¿Cómo sabe una base qué migraciones ya aplicó?
3. ¿Por qué `--autogenerate` puede generar una migración incompleta?
4. ¿Por qué el agente borró la extensión `vector` después de probarla?
5. ¿Qué es `naming_convention` y por qué se agrega hoy y no después?
6. ¿Por qué `versions/.gitkeep`?

---

# TEMA 9 · Trabajar con agentes

## Plan mode

Un modo donde el agente **lee el proyecto y propone un plan, sin escribir nada**. Nada se toca hasta que apruebas.

**Para qué sirve:** te deja revisar el enfoque antes de que existan 30 archivos. Corregir un plan cuesta minutos; corregir 30 archivos cuesta una tarde.

## `CLAUDE.md` — la memoria del proyecto

Un archivo de texto que el agente lee **automáticamente** al abrir sesión. Tres niveles:

```
~/.claude/CLAUDE.md        GLOBAL    · todos tus proyectos
<repo>/CLAUDE.md           PROYECTO  · se commitea, lo ve el equipo
<repo>/.claude/CLAUDE.md   PERSONAL  · tuyo dentro de ese proyecto
```

Precedencia: **lo más específico gana.**

**Qué va:** convenciones, comandos, arquitectura, trampas conocidas, **el porqué de las decisiones**.
**Qué NO va:** lo que el código ya dice.

**La prueba para cada línea:** *¿un dev nuevo lo deduciría del repo en 5 minutos?* Si sí, se borra.

**Y la restricción que gobierna todo:** cada token del CLAUDE.md está en el contexto **antes** de que escribas la primera palabra, en **cada** sesión. No es un wiki, es un presupuesto. La pregunta al escribir no es *"¿es cierto?"* sino **"¿esto cambia lo que el agente va a hacer?"**

## 🔑 Tu dato del 29 de julio

Un prompt de **cuatro palabras** —*"haz el scaffold de loop"*— produjo un plan que:

- Citó `CLAUDE.md`, `spec-v1.md`, `adr-001-stack.md` y `modelo-arturo.md` **por sección**
- Verificó que el puerto 5000 está ocupado en tu Mac (AirPlay)
- Propuso separar `repositories/` de `queries/` para volver **física** tu regla ORM/Core
- Cachó contradicciones entre tu spec y tu modelo

**Nada de eso vino del prompt. Vino del repo.**

> **El apalancamiento no está en escribir prompts largos. Está en el contexto persistente.**
> Invertir en `CLAUDE.md` y en docs de decisiones rinde en **cada** sesión futura.
> Invertir en un prompt rinde **una** vez.

**Y el complemento, igual de importante:** lo que el contexto **no podía saber** salió de tu criterio. Cuatro correcciones: `pyproject.toml` en vez de `requirements.txt`, config por clases, `/health/ready` en vez de `/health/db`, y CORS a mano en vez de librería.

```
El repo aporta   →  contexto
Tú aportas       →  criterio
```

## Por qué escribir tu plan antes de ver el del agente

Sin plan propio, cuando el agente proponga el suyo solo puedes decir *"suena bien"*. **Eso no es revisar, es aprobar.**

**Ajuste que se descubrió hoy:** cuando no dominas la tecnología, tu ground truth no son tus **respuestas** — son tus **preguntas**. Una lista de 14 preguntas concretas mide algo mejor: *¿el plan resolvió lo que no entendía, y levantó algo que no sabía ni preguntar?*

Saber qué no sabes es habilidad de nivel senior y casi nadie la practica.

## Skills y slash commands

**Se fusionaron.** `.claude/commands/x.md` y `.claude/skills/x/SKILL.md` producen el mismo `/x`.

Lo que cambia es **quién invoca**, y se controla en el frontmatter:
- el agente la carga **solo** cuando el contexto aplica
- o tú la invocas escribiendo `/nombre`

**El `description` es lo único que el agente ve para decidir si activarla.** El cuerpo solo se carga si el description ganó. Una skill perfecta con description vago **nunca se dispara**.

**Un subagente sí es distinto:** contexto **separado**, para aislar trabajo pesado.

## El límite del programa

Kun Chen dejó de revisar el código de sus agentes porque tiene 15 años de criterio construido. **Tú estás construyendo ese criterio.**

Hasta enero, **todo se revisa.** Y aprobar no es aprender: el aprendizaje está en poder explicar cada pieza después. Por eso existen los comprehension gates.

---

## ❓ Preguntas del tema 9

1. ¿Qué es plan mode y por qué conviene usarlo?
2. ¿Cuáles son los tres niveles de `CLAUDE.md` y cuál gana?
3. ¿Qué va y qué no va en un `CLAUDE.md`? ¿Cuál es la prueba de cada línea?
4. 🚪 Con tu propio dato: ¿qué hace que un agente produzca un plan de nivel senior?
5. ¿Por qué escribir tu plan antes de ver el del agente?
6. ¿Cuál es la diferencia entre skill, slash command y subagente hoy?
7. ¿Por qué tú sí revisas todo y Kun Chen no?

---

# 🎯 Los 10 gates del domingo

*Se interrogan en voz alta, sin ver notas. Titubeo → pasa como deuda a S2.*

```
□  1. Regla vs evento: recurrencia sin 365 filas
□  2. Por qué el peso se copia al registro diario
□  3. Orden de columnas en un índice compuesto, con el mecanismo
□  4. Volumen nombrado: qué pasa en un down si no lo es
□  5. Orden de capas del Dockerfile
□  6. Por qué app factory: los tres problemas
□  7. Sesión por request: el escenario de falla
□  8. Por qué close() y no commit()
□  9. Qué es CORS y quién lo aplica
□ 10. El dato de plan mode, con tu número
```

---

# ❓ Lo que sigue sin resolver

```
A1  El engine es global de módulo. ¿Anula eso la razón por la que
    elegiste el app factory?

A2  H1 · no_completado: ¿se escribe con un job, o se deriva?
    → DECIDIR ANTES DEL VIERNES

A3  H2 · ¿entra sesiones_pomodoro al schema? Es propuesta de Claude,
    no diseño tuyo.  → DECIDIR ANTES DEL VIERNES

A4  H3 · zonas horarias. Un pomodoro a las 11pm en Tokio, ¿a qué día
    pertenece? Viaje del 16 al 28 de agosto. Bloquea el ADR-003.

A5  H4 · la query de subárbol de categorías, sin escribir.
```
