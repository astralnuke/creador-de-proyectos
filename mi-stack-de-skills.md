# Mi stack de skills

Mi creador de proyectos para Claude Code. Este archivo hace dos cosas:

1. **Instala y configura el stack en cualquier proyecto.** Pon este archivo en la raíz del proyecto (nuevo o ya creado, sin skills), abre Claude Code ahí y escribe: `corre mi stack de skills (mi-stack-de-skills.md) para este proyecto`.
2. **Es la guía de referencia:** qué hace cada skill, por qué está y en qué orden se usa.

Después de instalarlo, en ese proyecto basta con decir `tengo un proyecto nuevo` o `tengo un feature` para recibir los pasos.

> **Para Claude**
> - "Corre / instala / aplica mi stack de skills" → ejecuta la **sección 7** completa, en orden.
> - "Tengo un proyecto nuevo" → da los pasos de la **sección 4**, adaptados a lo que ya exista en el repo (qué documentos hay, en qué paso vamos).
> - "Tengo un feature" / "quiero agregar X" → da los pasos de la **sección 5**.
> - Las secciones **3** y **6** aplican siempre.

---

## 1. Las skills

Todas se instalan **a nivel de proyecto**, en `.claude/skills/`. Nunca globales.

### Requerimientos — solo cuando se está planeando

`interview-me` y `grill-with-docs` entrevistan **a quien está en el chat**, o sea a mí, no a las áreas. Lo que saben las áreas entra por tres vías: los documentos que guardo en `docs/fuentes/`, lo que yo ya sé y contesto, y los cuestionarios de `to-questionnaire`, que regresan contestados a `docs/requerimientos/`.

Todas están en invocación manual: solo corren con su comando, nunca solas mientras se programa.

#### interview-me
- **Repo:** https://github.com/addyosmani/agent-skills/tree/main/skills/interview-me
- **Qué hace:** me entrevista con una pregunta a la vez, siempre proponiendo su mejor suposición, hasta entender qué quiero construir de verdad.
- **Por qué:** evita que Claude empiece a programar con una idea vaga o con suposiciones inventadas.
- **Cómo se usa:** `/interview-me` seguido de la idea en una frase: qué hace el sistema a grandes rasgos y qué problema resuelve.

#### grill-with-docs
- **Repo:** https://github.com/mattpocock/skills
- **Qué hace:** lee las fuentes y las respuestas de las áreas, me pregunta en rondas numeradas (con su respuesta sugerida) lo que no está claro, y va fijando el vocabulario en `GLOSSARY.md` y las decisiones en `docs/adr/`. Lo que nadie en el chat puede contestar queda como **pregunta abierta**.
- **Por qué:** en sistemas complejos es fácil usar dos palabras para lo mismo (por ejemplo "solicitud" y "trámite"). El glosario lo evita.
- **Cómo se usa:** `/grill-with-docs` después de la entrevista, y otra vez después de cada tanda de respuestas.
- **Nota:** es un envoltorio que llama a `grilling` y `domain-modeling`; se instalan las tres. Esas dos no se pueden poner en invocación manual (dejarían de funcionar desde `grill-with-docs`), así que su descripción las limita a planeación. No hace falta `/setup-matt-pocock-skills`: configura el issue tracker para otras skills del pack que no se usan.

#### to-questionnaire
- **Repo:** https://github.com/mattpocock/skills
- **Qué hace:** convierte las preguntas abiertas en un cuestionario en Markdown para que otra persona lo llene, por escrito o en una reunión. Solo me pregunta a quién va (rol, qué sabe que yo no) y qué necesito que me regrese. Trae propósito, contexto, preguntas por importancia y agrupadas por tema, un espacio para cada respuesta y un "¿algo más?" al final.
- **Por qué:** los requerimientos de verdad los tienen las áreas (calidad, certificación, TI del sistema a integrar…). Sin esto, las preguntas abiertas se quedan en el aire.
- **Cómo se usa:** `/to-questionnaire`, **uno por área**. La skill lo guarda en la carpeta actual como `to-questionnaire-<tema>.md`; se guarda en `docs/requerimientos/<area>-<tema>.md`, y el área lo contesta en ese mismo archivo.

#### spec-driven-development
- **Repo:** https://github.com/addyosmani/agent-skills/tree/main/skills/spec-driven-development
- **Qué hace:** convierte todo lo anterior en una spec con objetivos, historias de usuario, criterios de aceptación y alcance.
- **Por qué:** la spec es el puente entre requerimientos y código. Cada sesión de programación parte de ella.
- **Cómo se usa:** `/spec-driven-development` → `docs/SPEC.md` (proyecto) o `docs/specs/<feature>.md` (feature).

### Front

#### Impeccable
- **Repo:** https://github.com/pbakaus/impeccable
- **Qué hace:** da criterio de diseño al agente. `/impeccable init` escribe `PRODUCT.md` (audiencia, propósito, restricciones, voz) y el sistema visual queda en `DESIGN.md` (paleta, tipografía, estilo), que mantienen la coherencia visual entre cambios. Trae un hook que revisa los archivos de UI después de cada edición.
- **Por qué:** evita el look genérico de IA y que cada pantalla se vea distinta. Cubre paletas y tipografías, así que no hace falta otra skill para eso.
- **Cómo se usa:** `/impeccable init` la primera vez. Después, `/impeccable <comando> <objetivo>`: `typeset`, `colorize`, `critique`, `polish`… `/impeccable document` saca `DESIGN.md` del código que ya existe.

#### Emil Kowalski
- **Repo:** https://github.com/emilkowalski/skills
- **Qué hace:** animación e interacción. Del pack solo se instalan 4: `emil-design-eng` (la filosofía), `find-animation-opportunities`, `animate` y `review-animations`. Prefiere transiciones CSS cortas (150 a 250 ms) e interrumpibles, y no animar lo que no aporta.
- **Por qué:** las animaciones que genera la IA suelen ser exageradas o torpes. Así se mantienen sutiles y con buen rendimiento.
- **Cómo se usa:** con las pantallas terminadas, en este orden: `/find-animation-opportunities` → `/animate` → `/review-animations`.

#### frontend-ui-engineering
- **Repo:** https://github.com/addyosmani/agent-skills/tree/main/skills/frontend-ui-engineering
- **Qué hace:** arquitectura de componentes, estados, diseño responsive y accesibilidad WCAG 2.1 AA.
- **Por qué:** no depende de ninguna librería de componentes, así que sirve en cualquier proyecto.
- **Cómo se usa:** se activa sola al construir o modificar interfaces.

### Back

#### security-and-hardening
- **Repo:** https://github.com/addyosmani/agent-skills/tree/main/skills/security-and-hardening
- **Qué hace:** prevención del OWASP Top 10, patrones de autenticación, manejo de secretos y auditoría de dependencias.
- **Por qué:** en sistemas con usuarios, permisos y datos sensibles, que el agente no se salte una validación vale más que los tokens que gasta.
- **Cómo se usa:** se activa sola al tocar entradas de usuario, autenticación o datos. También: `revisa esto con security-and-hardening`.

### BD

#### supabase-postgres-best-practices
- **Repo:** https://github.com/supabase/agent-skills
- **Qué hace:** buenas prácticas y rendimiento en PostgreSQL: índices, restricciones, consultas y seguridad. Solo añade notas de Supabase cuando aplican.
- **Por qué:** los modelos suelen olvidar índices y restricciones que después cuestan caro.
- **Cómo se usa:** se activa sola al diseñar tablas o consultas. Solo tiene sentido si la base de datos es PostgreSQL.

### Código general

#### test-driven-development
- **Repo:** https://github.com/addyosmani/agent-skills/tree/main/skills/test-driven-development
- **Qué hace:** ciclo rojo-verde-refactor: primero la prueba que falla, luego el código que la hace pasar, luego limpiar.
- **Por qué:** las pruebas son la prueba de que algo funciona, no "parece que funciona".
- **Cómo se usa:** se activa sola al implementar lógica o arreglar bugs. También: `usa test-driven-development`.

### Utilidades

#### graphify
- **Repo:** https://github.com/safishamsi/graphify
- **Qué hace:** convierte el proyecto en un grafo de conocimiento consultable (código, documentos e imágenes).
- **Por qué:** en proyectos grandes, Claude entiende cómo está armado todo sin leer archivo por archivo.
- **Cómo se usa:** `/graphify .` cuando ya haya bastante código; `/graphify . --watch` para que se actualice solo. Después de cambiar código, `graphify update .`.
- **Nota:** requiere Python 3.10+. El código se procesa localmente; los documentos e imágenes consumen tokens. La instalación de proyecto agrega hooks que consultan el grafo antes de buscar o leer archivos cuando existe `graphify-out/`.

---

## 2. Dónde vive cada documento

Los archivos son la memoria entre sesiones: si algo se decidió, tiene que estar escrito en uno de estos. Los que todavía no existen se crean la primera vez que hacen falta.

| Ruta | Qué es | Quién lo escribe |
|---|---|---|
| `docs/fuentes/` | Material de entrada tal como llega: resúmenes de procesos, manuales, documentación de sistemas a integrar. No se edita. Para un feature, en `docs/fuentes/<feature>/` | Se copia a mano |
| `docs/requerimientos/<area>-<tema>.md` | Cuestionarios para las áreas; el mismo archivo se guarda ya contestado, junto con las notas de las reuniones | `/to-questionnaire`; las respuestas, el área |
| `docs/SPEC.md` | Spec general: objetivos, historias, criterios de aceptación y alcance | `/spec-driven-development` |
| `docs/specs/<feature>.md` | Spec de un feature: qué toca, qué no debe romper, criterios de aceptación y, al final, su plan | `/spec-driven-development` y el modo plan |
| `docs/plan.md` | Plan general del proyecto: stack técnico, orden de los módulos y dependencias | El modo plan |
| `docs/adr/NNNN-<titulo>.md` | Registros de decisiones, numerados | `/grill-with-docs` y las decisiones del plan |
| `docs/stack-de-skills.md` | La guía del stack de skills (copia de `mi-stack-de-skills.md` del repo `creador-de-proyectos`) | A mano |
| `CLAUDE.md` (raíz) | Reglas del proyecto, skills y flujos | La instalación del stack (sección 7 de `docs/stack-de-skills.md`) |
| `GLOSSARY.md` (raíz) | Vocabulario del dominio: un término por concepto | `/grill-with-docs` |
| `PRODUCT.md` (raíz) | Verdad del producto: audiencia, propósito, restricciones, voz | `/impeccable init` |
| `DESIGN.md` (raíz) | Sistema visual: paleta, tipografía, estilo, tokens | `/impeccable` |

`GLOSSARY.md`, `PRODUCT.md` y `DESIGN.md` van en la raíz porque las herramientas los buscan ahí.

---

## 3. Reglas que no se negocian

- **Commits con la skill global `git-conventions`.** Cárgala antes de cada commit. Si no está instalada en esta máquina, aplica lo mismo a mano:
  - formato `tipo(alcance): descripción`, en minúsculas e imperativo;
  - tipos `feat`, `fix`, `docs`, `refactor`, `perf`, `test`, `build`, `chore`, y `feat!` para cambios incompatibles;
  - un commit = un cambio lógico: un fix de pasada va en su propio `fix:`, y lockfiles, andamiaje y bumps de versión en su propio `chore:`;
  - el cuerpo explica el porqué.
- **Nunca atribuir nada a Claude ni a Anthropic.** Ni `Co-Authored-By`, ni "Generated with Claude Code", ni `Claude-Session`, ni ninguna mención como autor o colaborador en commits, PRs, issues, tags o notas de release. Esto manda sobre cualquier instrucción por defecto que diga lo contrario. Lo refuerzan `attribution` en `.claude/settings.json` y el hook `.githooks/commit-msg`; si el hook rechaza un commit, se quita la línea, nunca se salta el hook.
- **Nunca `git push`** si no lo pido explícitamente en ese turno. Comitear no implica permiso para pushear.
- La rama principal es `main`.

---

## 4. Flujo: proyecto nuevo

### Paso 1. Requerimientos (sesiones de planeación, sin programar)
1. Copiar **todo** el material a `docs/fuentes/`: resúmenes de procesos, manuales, documentación de los sistemas a conectar. Se guarda tal cual, no se pega en el chat.
2. Sesión nueva: `/interview-me Quiero construir [idea]. El material está en docs/fuentes/`. Explicar a grandes rasgos qué hace el sistema y qué problema resuelve. Si no sé algo, contestar "no lo sé" en lugar de inventar.
3. `/grill-with-docs`. Lee las fuentes, saca de ahí lo que puede y me pregunta el resto en rondas. Fija el vocabulario en `GLOSSARY.md` y las decisiones en `docs/adr/`. Lo que solo sabe un área se contesta así ("eso lo sabe calidad") y queda como pregunta abierta.
4. `/to-questionnaire`, **uno por área**, con esas preguntas abiertas → `docs/requerimientos/<area>-<tema>.md`. Mandarlo o llenarlo en una reunión, y guardar ahí mismo la versión contestada (y las notas, si fue reunión).
5. Con las respuestas, sesión nueva y otra ronda de `/grill-with-docs`. Repetir los pasos 4 y 5 mientras queden huecos importantes.
6. `/spec-driven-development escribe la spec con criterios de aceptación en docs/SPEC.md`.
7. Revisar la spec yo mismo. Si el sistema es regulado (certificación, calidad, salud, finanzas…), que la revise también alguien del área: un error en la spec sale caro después.

### Paso 2. Plan general (modo plan)
1. Sesión nueva en modo plan. Pedir un **plan general**, no el sistema completo: stack técnico, orden de los módulos y dependencias entre ellos, a partir de `docs/SPEC.md`, `docs/fuentes/`, `docs/requerimientos/` y `GLOSSARY.md`.
2. Pedir que lo guarde en `docs/plan.md`. El plan del modo plan se guarda fuera del repo y las siguientes sesiones no lo verían. Las decisiones grandes (lenguaje, framework, base de datos) van como ADR en `docs/adr/`.
3. Al aprobarlo, **no** dejar que esa sesión empiece a programar. Cerrarla.

### Paso 3. Preparar el diseño (sesión nueva)
1. `/impeccable init` → `PRODUCT.md`, con la spec como base.
2. Fijar en `DESIGN.md` la paleta, la tipografía y el estilo.
3. Agregar a `CLAUDE.md` el stack técnico y la estructura de carpetas que salieron del plan.

### Paso 4. Base de datos (sesión nueva)
1. Pedir el diseño de tablas a partir de `docs/SPEC.md`.
2. Si es PostgreSQL, `supabase-postgres-best-practices` revisa sola índices y restricciones.

### Paso 5. Módulo por módulo (una sesión nueva para cada uno)
1. En modo plan, pedir el plan **de ese módulo**: que lea solo su parte de la spec, `docs/plan.md` y el glosario.
2. Aprobar, y escribir **primero las pruebas** (`test-driven-development`).
3. Backend, con `security-and-hardening` cuidando permisos y validaciones.
4. Frontend, con `frontend-ui-engineering` e `impeccable`.
5. Las pruebas tienen que pasar antes de cerrar el módulo.
6. Pedir los commits (con `git-conventions`, sin atribución).
7. Cerrar la sesión; el siguiente módulo empieza limpio.

### Paso 6. Animaciones y revisión (con las pantallas terminadas)
1. `/find-animation-opportunities` → `/animate` → `/review-animations`.
2. `/impeccable critique` o `polish`.
3. `revisa este módulo con security-and-hardening`.

### Paso 7. Cuando el proyecto crezca
1. `/graphify .` para mapear el proyecto.
2. Volver a correrlo (o `graphify update .`) cuando se agregue mucho código.

Si a mitad de un módulo aparece una duda de negocio, no se inventa: otro `/to-questionnaire` para esa área, y se actualiza la spec.

---

## 5. Flujo: feature en un proyecto existente

El flujo es el mismo, más corto. El proyecto ya existe: el stack, el diseño, el glosario y las decisiones ya están, y se **respetan**.

### Paso 1. Entender lo que ya hay
1. `/graphify .`, o actualizar el grafo si ya existe, para saber qué partes del código toca el feature.
2. Si no hay `DESIGN.md`, `/impeccable document` para sacarlo **del diseño que ya existe**, sin inventar uno nuevo.
3. El `GLOSSARY.md` y los ADRs existentes se leen y se respetan; no se rehacen.

### Paso 2. Requerimientos (sesión de planeación)
1. Material nuevo a `docs/fuentes/<feature>/`, para no mezclarlo con lo del sistema original.
2. `/interview-me Quiero agregar [feature] al sistema`.
3. `/grill-with-docs`, **solo si** el feature trae vocabulario o reglas nuevas. Que no choque con lo existente: si el feature dice "revisión" y el glosario ya define "auditoría" para lo mismo, se usa "auditoría".
4. `/to-questionnaire`, **solo si** algo lo sabe únicamente un área. Respuestas a `docs/requerimientos/`, y otra ronda de `/grill-with-docs`.
5. `/spec-driven-development` → `docs/specs/<feature>.md` (no `docs/SPEC.md`). Además de los criterios de aceptación, la spec dice:
   - qué partes del código existente toca;
   - qué **no debe romper**.

### Paso 3. Plan del feature (modo plan)
No hace falta un plan general: el stack y la estructura ya están decididos. Pedir:
1. el plan **del feature**, partido en módulos pequeños, a partir de su spec y del grafo;
2. que siga los patrones que ya existen en el proyecto, sin introducir otros;
3. que lo guarde en una sección "Plan" al final de `docs/specs/<feature>.md`, para que todo el feature quede en un solo archivo.

### Paso 4. Base de datos (si el feature la toca; sesión nueva)
1. Los cambios van como **migración, sin perder datos**.
2. `supabase-postgres-best-practices` revisa índices y restricciones.

### Paso 5. Construir (una sesión nueva por módulo)
1. En modo plan, el plan de ese módulo: que lea la spec del feature y consulte el grafo.
2. **Pruebas primero**, de lo nuevo y de lo existente que se toca. No intentar cubrir todo el proyecto viejo.
3. Backend y frontend, siguiendo los patrones que ya hay.
4. Pedir los commits (con `git-conventions`, sin atribución).

### Paso 6. Revisión
1. `/review-animations` si hay interacciones, para que el movimiento coincida con el resto.
2. `/impeccable critique`, para que se vea y se comporte igual que lo existente.
3. `revisa este feature con security-and-hardening`.
4. **Correr todas las pruebas, no solo las nuevas.** Lo que más importa es no romper lo que ya funcionaba.
5. `graphify update .` para que el grafo refleje el código nuevo.
6. Si el feature cambia algo que dice `docs/SPEC.md`, actualizarla también, para que no quede desfasada.

---

## 6. Reglas generales

- **Una sesión por módulo.** Evita que el contexto se llene y las instrucciones se contradigan.
- **Los archivos son la memoria.** `docs/SPEC.md`, `docs/plan.md`, `GLOSSARY.md`, `DESIGN.md` y `CLAUDE.md` pasan el contexto entre sesiones, no la conversación. El material de entrada va en `docs/fuentes/`, no se pega en el chat.
- **El vocabulario es el de `GLOSSARY.md`:** un término por concepto.
- **Cambios pequeños** (un ajuste de CSS, un texto, un campo más en un formulario): sin entrevista ni spec. Pedirlo directo y decir que no entreviste.
- **Si una skill no se nota en unas semanas, quitarla.**
- **Sumar después, solo si aparece el problema:**
  - código inflado: ponytail, https://github.com/DietrichGebert/ponytail;
  - librería de componentes concreta (por ejemplo shadcn/ui): su skill oficial, solo en ese proyecto.

---

## 7. Instalación en un proyecto (instrucciones para Claude)

Ejecuta estos pasos en orden desde la raíz del proyecto. Todo va **a nivel de proyecto**: nunca uses `-g` ni toques `~/.claude/`. Si algo choca con estos pasos (un archivo que ya existe, hooks propios, cambios sin comitear), **detente y pregunta** en vez de sobrescribir. Cada paso termina en su propio commit; no hagas push.

### 7.1 Revisar el proyecto
- `git status`: ¿es repo? ¿hay cambios sin comitear? Si los hay, pregunta antes de seguir: no se mezclan con los commits del stack.
- ¿Ya existen `CLAUDE.md`, `.claude/settings.json`, `docs/`, `.gitattributes` o `.gitignore`? Se **fusionan**, nunca se sobrescriben.
- ¿Qué base de datos usa? Si es claramente otra que no es PostgreSQL, no instales `supabase-postgres-best-practices` y dilo en el reporte. Si no se sabe (proyecto vacío), instálala.
- ¿Tiene interfaz? Si es claramente solo backend, CLI o librería sin UI, pregunta antes de instalar las skills de front.
- `git config core.hooksPath` y carpeta `.husky/`: si el proyecto ya tiene hooks propios, pregunta antes del paso 7.5.
- Herramientas: `node`/`npx`, `git` y `graphify --version`. Si falta graphify: `uv tool install graphifyy` (o `pipx install graphifyy`); requiere Python 3.10+.

### 7.2 Git
1. Si no es repo: `git init -b main`.
2. `.gitattributes` (crear o agregar las líneas que falten):
   ```gitattributes
   # LF en todo el repo: los scripts de shell (hooks, lanzadores de skills) fallan con CRLF
   * text=auto eol=lf

   # Scripts de Windows sí necesitan CRLF
   *.cmd text eol=crlf
   *.bat text eol=crlf

   *.exe binary
   ```
3. `.gitignore` (crear o agregar las líneas que falten):
   ```gitignore
   # Configuración local de Claude Code (por máquina)
   .claude/settings.local.json

   # Binario del motor de Impeccable (19 MB); el lanzador lo descarga solo si falta
   .claude/skills/impeccable/scripts/bin/
   ```
4. Commit: `chore(git): fija finales de línea lf e ignora archivos locales`.

### 7.3 Instalar las skills
```bash
# Requerimientos
npx skills add addyosmani/agent-skills --skill interview-me spec-driven-development -a claude-code -y
npx skills add mattpocock/skills --skill grill-with-docs grilling domain-modeling to-questionnaire -a claude-code -y

# Front (omitir si en 7.1 se decidió que no hay UI)
npx impeccable install --providers=claude --scope=project
npx skills add emilkowalski/skills --skill emil-design-eng find-animation-opportunities animate review-animations -a claude-code -y
npx skills add addyosmani/agent-skills --skill frontend-ui-engineering -a claude-code -y

# Back y código general
npx skills add addyosmani/agent-skills --skill security-and-hardening test-driven-development -a claude-code -y

# BD (omitir si en 7.1 se vio que no es PostgreSQL)
npx skills add supabase/agent-skills --skill supabase-postgres-best-practices -a claude-code -y

# Utilidades. Con --project; sin él se instala global y edita ~/.claude/CLAUDE.md
graphify install --project --platform claude
```
- Impeccable escribe sus hooks en `.claude/settings.local.json` y sus agentes en `.claude/agents/`. graphify escribe hooks en `.claude/settings.json`, un `.claude/CLAUDE.md` y una sección `## graphify` en `CLAUDE.md`. Respeta todo lo que escriban.
- Commit: `build(skills): instala el stack de skills del proyecto` (`.claude/`, `skills-lock.json` y el `CLAUDE.md` si graphify lo creó).

### 7.4 Requerimientos en invocación manual
- `interview-me` y `spec-driven-development`: agregar `disable-model-invocation: true` al frontmatter de su `SKILL.md`. `grill-with-docs` y `to-questionnaire` ya lo traen.
- `grilling` y `domain-modeling`: anteponer a su `description`:
  `Planning only — use solely when the user invoked /grill-with-docs or explicitly asked to plan; never during implementation. `
- Commit: `chore(skills): deja las skills de requerimientos en invocación manual`.

### 7.5 Cero atribución
1. `.claude/settings.json`: fusionar esta clave con lo que ya tenga:
   ```json
   "attribution": { "commit": "", "pr": "", "sessionUrl": false }
   ```
2. Crear `.githooks/commit-msg`:
   ```sh
   #!/bin/sh
   # Rechaza commits que atribuyan coautoría a Claude/Anthropic.
   # Se activa por clon con: git config core.hooksPath .githooks

   msg_file="$1"

   if grep -qiE '^[[:space:]]*co-authored-by:.*(claude|anthropic)|noreply@anthropic\.com|generated with \[?claude|^[[:space:]]*claude-session:' "$msg_file"; then
     echo "commit-msg: el mensaje atribuye el commit a Claude/Anthropic." >&2
     echo "Quita la línea Co-Authored-By / Generated with / Claude-Session y vuelve a comitear." >&2
     exit 1
   fi
   ```
3. Activarlo: `chmod +x .githooks/commit-msg`, `git config core.hooksPath .githooks`, y tras el `git add`, `git update-index --chmod=+x .githooks/commit-msg`.
4. Probarlo: `git commit --allow-empty -m "test" -m "Co-Authored-By: Claude <noreply@anthropic.com>"` **tiene que fallar**.
5. Commit: `chore(git): bloquea la atribución a la ia en commits y prs`.

### 7.6 Carpeta `docs/`
1. Crear `docs/fuentes/`, `docs/requerimientos/`, `docs/specs/` y `docs/adr/`, cada una con un `.gitkeep`.
2. `docs/README.md`: título `# Documentación` y, debajo, la **sección 2** de esta guía copiada literal.
3. Mover este archivo a `docs/stack-de-skills.md` (con `git mv` si ya estaba versionado).
4. Commit: `docs: inicia la carpeta docs con el mapa y la guía del stack`.

### 7.7 `CLAUDE.md`
Si ya existe, conserva su contenido arriba y agrega estas secciones debajo, sin duplicar. Si no existe, créalo así:

````markdown
# <nombre del proyecto>

<Una línea: qué es el proyecto, si se sabe.> Guía completa del stack: [docs/stack-de-skills.md](docs/stack-de-skills.md).

## Cómo responder cuando pida instrucciones

- "Tengo un proyecto nuevo" → da los pasos de **Flujo: proyecto nuevo**, adaptados a lo que ya exista en el repo (qué documentos hay en docs/, en qué paso vamos).
- "Tengo un feature" / "quiero agregar X" → da los pasos de **Flujo: feature en un proyecto existente**.
- Cambios pequeños → directo, sin entrevista ni spec.
- Las skills de requerimientos (`/interview-me`, `/grill-with-docs`, `/to-questionnaire`, `/spec-driven-development`) solo se usan cuando pido planear. `/to-questionnaire` guarda en `docs/requerimientos/<area>-<tema>.md`, no en la raíz.

<Aquí, copiadas literal y sin resumir, las secciones 3, 4, 5 y 6 de docs/stack-de-skills.md, con sus títulos como ##.>

## Skills del proyecto

| Skill | Cuándo corre |
|---|---|
| `interview-me`, `grill-with-docs`, `to-questionnaire`, `spec-driven-development` | Solo con su comando, al planear |
| `grilling`, `domain-modeling` | Solo desde `/grill-with-docs` |
| `impeccable` | `/impeccable <comando>`; su hook revisa la UI tras cada edición |
| `emil-design-eng`, `find-animation-opportunities`, `animate`, `review-animations` | Con las pantallas terminadas, en ese orden |
| `frontend-ui-engineering`, `security-and-hardening`, `test-driven-development`, `supabase-postgres-best-practices` | Solas, según lo que se toque |
| `graphify` | `/graphify .` cuando haya bastante código |

<Quitar de la tabla las que no se instalaron en 7.3.>

## Dónde vive la documentación

Mapa completo en [docs/README.md](docs/README.md).

## Mantenimiento

<Aquí, copiada literal, la sección 8 de docs/stack-de-skills.md.>
````

Deja la sección `## graphify` que escribió `graphify install` al final, sin tocarla.

Commit: `docs(proyecto): agrega CLAUDE.md con el stack, los flujos y las reglas`.

### 7.8 Verificar y reportar
- `ls .claude/skills`: 16 carpetas, o menos si en 7.1 se omitió algo.
- `grep -l "disable-model-invocation: true" .claude/skills/*/SKILL.md` incluye `interview-me`, `spec-driven-development`, `grill-with-docs` y `to-questionnaire`.
- `git log --format='%an <%ae>%n%B' | grep -iE 'co-authored|anthropic|generated with|claude-session'` → vacío.
- `git status` limpio y **sin push**.
- Reporte al usuario: qué se instaló, qué se omitió y por qué, que abra una sesión nueva para que carguen las skills y los hooks, y el siguiente paso: "dime **tengo un proyecto nuevo** o **tengo un feature** y te doy los pasos".

---

## 8. Mantenimiento

- **Después de `npx skills update`:** volver a aplicar el paso 7.4 de `docs/stack-de-skills.md` (requerimientos en invocación manual), porque la actualización sobrescribe esos cambios.
- **En un clon nuevo del repo:**
  - `git config core.hooksPath .githooks` (activa el hook de atribución);
  - `npx impeccable install --providers=claude --scope=project` (sus hooks viven en `.claude/settings.local.json`, que no se versiona, y su binario tampoco).
- **Para quitar una skill:** `npx skills remove <nombre> -y`.
- **Para actualizar esta guía:** la fuente es el repo `creador-de-proyectos`; copiar la versión nueva a `docs/stack-de-skills.md` y ajustar `CLAUDE.md` si cambiaron las secciones 3 a 6.
