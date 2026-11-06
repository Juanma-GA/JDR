# CLAUDE.md — JDR (Mockup Builder)

## Lectura obligatoria

Antes de escribir nada en este repo, lee:

1. [`AGENTS.md`](AGENTS.md): punto de entrada de AACF v2.0.0 (copia en [`aacf/`](aacf/)).
2. [`aacf/rules/atexis-hard-rules.md`](aacf/rules/atexis-hard-rules.md): Reglas Estrictas HR0–HR21. No son negociables; autocomprueba cada cambio contra ellas.

Si una regla de este fichero choca con AACF, gana AACF ([ADR-002](docs/decisions/ADR-002-prioridad-de-reglas.md)).

## Producto

- JDR es la versión real del **Mockup Builder v1.1**: una herramienta web para especificar y generar mockups HTML interactivos con ayuda de IA.
- Pantalla principal: la lista de **Mis mockups** a la izquierda, un **asistente de 7 pasos** en el centro y un **chat IA** a la derecha.
- Los 7 pasos son: información básica, objetivo y usuarios, pantalla y layout, estilo visual, componentes e interacción, contenido y datos, y revisión y generación.
- El usuario puede subir referencias (capturas, HTML, PDF, guías de marca) para extraer estilo y layout. El resultado es un único HTML autocontenido que se comparte con el cliente.
- La interfaz está en zh, kk y es. El dominio es la publicación técnica (S1000D, ATA iSpec 2200, DITA, ASD-STE100).

Referencia visual y funcional: [`docs/reference/mockup-builder-v1.1.html`](docs/reference/mockup-builder-v1.1.html). En el mockup, la IA es simulada (respuestas fijas con `setTimeout`), los mockups viven en memoria y el idioma se guarda en `localStorage`. Nada de eso se traslada a JDR (HR1).

## Capa IA

Detalle y justificación en [ADR-003](docs/decisions/ADR-003-capa-ia.md).

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

Además (ai-output-safety): el HTML generado se previsualiza en un iframe sandbox con CSP; las referencias subidas son datos delimitados, nunca instrucciones; toda dependencia propuesta por IA se verifica y se fija.

## Cómo trabajamos

- Un encargo = una sesión, una rama enc/EXX-nombre y un PR.
- Ciclo: checklist-creator → aprobación mía → checklist-validator → checklist-implementer → verificación ejecutable → code-reviewer (+ security-reviewer si toca auth/datos/dependencias/LLM; + senior-prompt-engineer si toca prompts) → actualizar CLAUDE.md y CHANGELOG → PR.
- Hecho = checklist al 100 %, autocomprobado contra HR0–HR21, CI en verde, textos de UI localizados y humanizados, revisiones sin hallazgos abiertos, CLAUDE.md y CHANGELOG al día.

«Mía» se refiere al responsable del proyecto: el checklist no se implementa hasta que lo aprueba.

- Plan y dependencias: [`docs/plan/encargos.md`](docs/plan/encargos.md).
- Checklists de cada encargo: [`docs/plan/checklists/`](docs/plan/checklists/).
- Decisiones: [`docs/decisions/`](docs/decisions/README.md).
- Subagentes: [`.claude/agents/`](.claude/agents/), generados desde [`aacf/agents/`](aacf/agents/README.md).

## Adaptaciones de AACF a JDR

- **Investigación:** donde AACF dice `research_quick` / `research_submit`, usa WebSearch y WebFetch.
- **Despliegue:** donde AACF dice `deploy.ps1`, usa el procedimiento de despliegue que se defina en E01 (se documentará en «Arquitectura»).
- **`aacf_fetch` no es accesible desde esta red.** Se trabaja con la copia de `aacf/`, sacada de `Juanma-GA/aacf`, rama `claude/crear-carpetas-en-sp-e57wrh`, commit `d0ab40c` (`aacf/VERSION` = 2.0.0). Avisa al usuario una vez por sesión, como pide AACF, y continúa.
- **`aacf/` no se edita.** Para actualizarlo se sustituye entero por una versión nueva de AACF, con su propio encargo.
- **Defecto conocido del origen:** en `d0ab40c`, los 37 ficheros de AACF terminan con una línea suelta `</content>`. Lo mismo ocurre en `AGENTS.md` y en `.claude/agents/`, que se derivan de ellos. Se conserva por la regla de copia literal. La versión es la primera línea de `aacf/VERSION` (`2.0.0`). Ignora esa línea al leer los ficheros y avisa al usuario si da problemas.
- **Revisores:** los agentes revisores usan Bash solo para leer y ejecutar (tests, git diff/log, lint); nunca modifican ficheros ni hacen commits.

## Arquitectura

_Pendiente hasta E01 (andamiaje)._ La pila propuesta está en [ADR-001](docs/decisions/ADR-001-pila.md), todavía sin aprobar.
