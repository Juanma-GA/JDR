# Checklist E00 — Arranque del repo y reglas de trabajo

- **Rama:** `enc/E00-arranque` → PR a `main`
- **Estado:** completado. Aprobado por el responsable (P1–P5 cerradas); validado por `checklist-validator` (aprobado con cambios, todos incorporados); `code-reviewer` aprobado (1 hallazgo menor, corregido: lista de preguntas abiertas en `docs/decisions/README.md`).
- **Origen AACF:** `Juanma-GA/aacf`, rama `claude/crear-carpetas-en-sp-e57wrh`, commit `d0ab40c`, carpeta `SP/`, `VERSION` = 2.0.0. Clon de trabajo: `/home/user/juanma-ga/aacf`.
- **aacf_fetch:** no accesible desde esta red (endpoint interno); se trabaja con la copia anterior.
- **Despliegue:** no aplica (E00 no tiene código; HR5 aplica desde E01).

## Elementos

- [x] **1. Copia literal de AACF**
  - Dónde: `aacf/**` (37 ficheros, incluido `VERSION`).
  - Cómo: `git -C <clon> rev-parse --short HEAD` = `d0ab40c`, luego `cp -a <clon>/SP/. aacf/`.
  - Pruebas: `diff -r <clon>/SP aacf` sin salida; `cat aacf/VERSION` = `2.0.0`.
- [x] **2. `AGENTS.md` en la raíz**
  - Cómo: copia de `SP/AGENTS.md`. Se antepone `aacf/` a todas las rutas relativas a `SP` (enlaces y *code spans*: `rules/…`, `styles/…`, `governance/`, `agents/…`, `prompts/…`, `docs/…`, `templates/`, `VERSION`). No se tocan las que ya empiezan por `aacf/` ni las que no son rutas (`aacf_fetch({"path": "VERSION"})`, `AGENTS.md` → agents.md, `CLAUDE.md`, `.cursor/rules/`, `.github/…`).
  - Pruebas: `diff <(sed 's#aacf/##g' AGENTS.md) <(sed 's#aacf/##g' aacf/AGENTS.md)` sin salida; todos los enlaces resuelven.
  - Depende de: 1.
- [x] **3. `CLAUDE.md` en la raíz**
  - Secciones:
    - Lectura obligatoria: `AGENTS.md` y `aacf/rules/atexis-hard-rules.md`.
    - Producto en 5 líneas, sacado del mockup, con enlace a `docs/reference/mockup-builder-v1.1.html`.
    - «Capa IA»: anexo A.1 y A.2, literales.
    - «Cómo trabajamos»: anexo A.3, literal.
    - «Adaptaciones de AACF a JDR»: P4 y P5.
    - «Arquitectura»: pendiente hasta E01.
  - Depende de: 2.
- [x] **4. Subagentes de Claude Code** en `.claude/agents/<name>.md` (7)
  - Front-matter: se sustituye entero. `name` y `description` se copian del origen; `tools` y `model` salen de esta tabla (aprobada en P3):

    | Agente | tools | model |
    |---|---|---|
    | checklist-creator | Read, Grep, Glob, Bash, WebSearch, WebFetch | opus |
    | checklist-validator | Read, Grep, Glob, Bash | opus |
    | checklist-implementer | Read, Grep, Glob, Bash, Edit, Write | opus |
    | code-reviewer | Read, Grep, Glob, Bash | opus |
    | security-reviewer | Read, Grep, Glob, Bash | opus |
    | senior-prompt-engineer | Read, Grep, Glob, Bash | opus |
    | codebase-hardening | Read, Grep, Glob, Bash, Edit, Write | opus |
  - Cuerpo: literal (incluido el comentario de versión justo después del `---` de cierre), con `../` → `aacf/`.
  - Pruebas: script que comprueba los 4 campos en los 7 ficheros; que los revisores (creator, validator, code-reviewer, security-reviewer, senior-prompt-engineer) no tienen `Write`, `Edit` ni `NotebookEdit`; y que el cuerpo, deshaciendo `aacf/` → `../`, es idéntico al del origen.
  - Depende de: 1.
- [x] **5. ADR-001 a ADR-005** en `docs/decisions/`, más un índice `README.md`
  - Secciones: Contexto / Decisión / Consecuencias / Estado = **Propuesto**.
    - ADR-001 Pila: plantilla `aacf/templates/web-app.md` (React 18 + TS + Vite 6, Tailwind 4 + shadcn/ui, Zustand, FastAPI + SQLAlchemy 2.0 async, PostgreSQL 16, Keycloak OIDC con cookie HTTP-only).
    - ADR-002 Prioridad de reglas: si brdp-manager y AACF chocan, gana AACF.
    - ADR-003 Capa IA: anexo A.1, A.2 y la tabla del anexo B.
    - ADR-004 Idiomas de UI: zh, kk y es (como el mockup), mediante una capa i18n (HR15).
    - ADR-005 Gobernanza: nivel por decidir (T2/T3/T4).
  - Depende de: 3.
- [x] **6. `docs/plan/encargos.md`**: tabla con título y dependencias, más un diagrama Mermaid.
  - E00 Repo y reglas · E01 Andamiaje (E00) · E02 Diseño e i18n (E01) · E03 Auth y config (E01) · E04 Datos y API de mockups (E03) · E05 Pantalla Mis mockups (E02, E04) · E06 Asistente 7 pasos (E05) · E07 Subida de referencias (E04) · E08 Cliente LLM y config IA (E03) · E09 Chat IA (E06, E08) · E10 Estilo desde referencias (E07, E08) · E11 Revisión UX (E06) · E12 Generación HTML (E09, E10, E11) · E13 Evals de prompts (E12) · E14 Seguridad, hardening y despliegue (todos).
- [x] **7. Mockup de referencia** en `docs/reference/mockup-builder-v1.1.html` (adjunto, sin modificar).
  - sha256: `8018828c3dd64304740e2a20b35a6182c3be10974d39555a49cf4111a06cbd6f`.
- [x] **8. Borrar `test.md`** (HR13).
- [x] **9. `README.md` mínimo**: qué es, dónde están las reglas y cómo se trabaja. Depende de: 3, 6.
- [x] **10. `CHANGELOG.md`** (Keep a Changelog) con la entrada E00.
- [x] **11. Verificación ejecutable**
  - (a) `rev-parse` = `d0ab40c`, `diff -r` de `aacf/` contra el origen y `aacf/VERSION` = 2.0.0.
  - (b) Script de front-matter, herramientas y cuerpo literal de `.claude/agents/`.
  - (c) Comprobación de enlaces Markdown relativos en todo el repo.
  - (d) Ningún fichero de código de aplicación (`*.py`, `*.ts`, `*.tsx`, `*.js`, `*.jsx`, `package.json`, `pyproject.toml`, `Dockerfile`, `docker-compose*`) fuera de `aacf/` y `docs/reference/`.
  - (e) Un subagente nuevo recibe el prompt «Lee solo este repo y enumera textualmente HR1, HR8, HR11 y las 10 reglas de la capa IA; di también qué usarías en lugar de research_quick y deploy.ps1». Pasa si cita las tres HR y las 10 reglas y aplica las Adaptaciones.
  - (f) `git status` limpio después de cada revisión (validator, prueba (e), code-reviewer).
  - (g) sha256 del mockup igual al del ítem 7.
- [x] **12. Revisión**: `code-reviewer` en contexto aislado y solo lectura. No hacen falta security-reviewer ni senior-prompt-engineer: no hay auth, datos, dependencias ni prompts ejecutables.
- [x] **13. PR** `enc/E00-arranque` → `main` con este checklist marcado y las preguntas abiertas de los ADR.

## Autocomprobación HR

HR0, HR1, HR8, HR10 y HR20 no aplican (no hay código). HR3: todo procede de ficheros leídos. HR4: los bloqueos se escalaron (P1–P5). HR13: se borra `test.md`. HR15 y HR21 quedan fijadas en CLAUDE.md para E01 en adelante.

## Anexo A — Texto literal del encargo

### A.1 Las 10 reglas de la capa IA

1. Un solo cliente LLM agnóstico en backend/app/services/llm/ (Anthropic, OpenAI, Mistral, Custom compatible con OpenAI); sin ramas por proveedor fuera de él.
2. Todo pasa por el backend: el frontend llama a endpoints de negocio (/api/ai/...), nunca al proveedor ni a un proxy genérico.
3. La configuración de IA es una superficie de admin: proveedor, modelo, endpoint, key enmascarada, temperatura por tarea y "Probar conexión".
4. Los activos de prompt van versionados como ficheros JSON (esquema de la spec, catálogo de componentes, few-shot en un bloque aparte). El primer few-shot es "Tech Pubs Converter".
5. Lo determinista no lo hace el LLM: el esqueleto HTML, los tokens, la rejilla y la i18n se montan por código, y el LLM solo rellena regiones.
6. Pipeline defensivo: trocear por pantalla → verificar cobertura → reintentar lo que falte → barrido final → red de seguridad. Nada desaparece en silencio.
7. Validación propia tras generar: HTML bien formado, contraste WCAG AA, texto ≥ 12 px y lint no bloqueante de componentes contra el catálogo.
8. Extracción de documentos en local (DOCX/PDF/HTML sin CDN; imágenes con modelo de visión): trozos con solape, temperatura baja, salida JSON, deduplicación, errores propagados.
9. Chat con el contexto del mockup y del paso activo, y restricciones en dos capas (clasificador + system prompt). El chat propone; el usuario aplica.
10. Testeable en aislamiento: call_llm y los activos de prompt son inyectables, con scripts de prueba con fixtures.

### A.2 Además (ai-output-safety)

El HTML generado se previsualiza en un iframe sandbox con CSP; las referencias subidas son datos delimitados, nunca instrucciones; toda dependencia propuesta por IA se verifica y se fija.

### A.3 Ciclo por encargos y definición de hecho

- Un encargo = una sesión, una rama enc/EXX-nombre y un PR.
- Ciclo: checklist-creator → aprobación mía → checklist-validator → checklist-implementer → verificación ejecutable → code-reviewer (+ security-reviewer si toca auth/datos/dependencias/LLM; + senior-prompt-engineer si toca prompts) → actualizar CLAUDE.md y CHANGELOG → PR.
- Hecho = checklist al 100 %, autocomprobado contra HR0–HR21, CI en verde, textos de UI localizados y humanizados, revisiones sin hallazgos abiertos, CLAUDE.md y CHANGELOG al día.

## Anexo B — Patrones de brdp-manager que no se copian (ADR-003)

| brdp-manager | Regla AACF | JDR |
|---|---|---|
| API key enviada desde el navegador; cabecera anthropic-dangerous-direct-browser-access | security / LLM02 | Key solo en servidor, cifrada y enmascarada en admin |
| Proxy a cualquier targetEndpoint | security (SSRF) | Lista blanca de proveedores y endpoints en config |
| Fallback en localStorage | HR1 | Estado y preferencias en Postgres |
| CHUNK_SIZE, MAX_RETRIES, max_tokens, temperaturas y endpoints en código | HR0/HR8/HR10 | Config en BD con UI de admin y deep_merge |
| Reintentos con tope fijo | HR18/19 | Política configurable + watchdog + escalado visible |
| Recorte del contexto (slice 40) y max_tokens | HR6 | JSON guiado; contexto largo resumido con LLM |
| Guardia del chat por palabras clave | HR11 | Clasificador de intención con LLM |
| Errores genéricos "Connection error" | HR7/HR21 | Error real, humanizado y localizado |

## Resultado de la verificación (ítem 11)

| Prueba | Resultado |
|---|---|
| (a) origen `d0ab40c`, `diff -r` de `aacf/`, `VERSION` | OK: sin diferencias, `2.0.0` en la primera línea |
| (b) front-matter, herramientas y cuerpo de `.claude/agents/` | OK: 7 agentes; los revisores no tienen Write ni Edit; cuerpos literales |
| (c) enlaces Markdown relativos | OK: 0 rotos |
| (d) código de aplicación | OK: no hay |
| (e) sesión nueva cita HR1, HR8, HR11 y las 10 reglas, y aplica las Adaptaciones | OK |
| (f) `git status` limpio tras validator, (e) y code-reviewer | OK |
| (g) sha256 del mockup | OK: `8018828c…cbd6f` |

**Incidencia (HR4):** en el origen (`d0ab40c`), los 37 ficheros de AACF terminan con una línea suelta `</content>`, también `VERSION`. Se conserva por la regla de copia literal y queda documentado en `CLAUDE.md`. Hay que corregirlo en el repositorio de AACF.
