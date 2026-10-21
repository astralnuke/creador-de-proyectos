# creador-de-proyectos

Mi stack de skills para Claude Code en un solo archivo: [mi-stack-de-skills.md](mi-stack-de-skills.md). Sirve como guía de referencia y como instalador.

## Cómo se usa

1. Descarga `mi-stack-de-skills.md` y ponlo en la raíz del proyecto, nuevo o ya creado.
2. Abre Claude Code en esa carpeta y escribe:
   ```
   corre mi stack de skills (mi-stack-de-skills.md) para este proyecto
   ```
3. Claude instala las skills a nivel de proyecto, crea `CLAUDE.md` y `docs/`, bloquea la atribución a la IA en commits y PRs, y comitea cada paso por separado. No hace push.
4. Abre una sesión nueva y pide los pasos:
   - `tengo un proyecto nuevo`
   - `tengo un feature`

Para descargarlo desde la terminal (el repo es privado, así que necesitas `gh` con sesión iniciada):

```bash
gh api repos/astralnuke/creador-de-proyectos/contents/mi-stack-de-skills.md -H "Accept: application/vnd.github.raw" > mi-stack-de-skills.md
```

## Qué incluye

| Etapa | Skills |
|---|---|
| Requerimientos | `interview-me`, `grill-with-docs`, `to-questionnaire`, `spec-driven-development` |
| Front | `impeccable`, 4 de Emil Kowalski, `frontend-ui-engineering` |
| Back | `security-and-hardening` |
| BD | `supabase-postgres-best-practices` |
| Código general | `test-driven-development` |
| Utilidades | `graphify` |

El detalle de cada una, los flujos para proyecto nuevo y para feature, y los pasos de instalación están en [mi-stack-de-skills.md](mi-stack-de-skills.md).
