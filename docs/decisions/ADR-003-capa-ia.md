# ADR-003 — Capa IA

## Contexto

En el mockup de referencia la IA es simulada: las respuestas son fijas y salen con `setTimeout`. JDR necesita una capa IA real para el chat del asistente, la extracción de estilo desde referencias, la revisión UX y la generación del HTML. Como referencia de diseño se usa `Juanma-GA/brdp-manager` (`CLAUDE.md` y `src/api/`), pero varios de sus patrones incumplen AACF; por [ADR-002](ADR-002-prioridad-de-reglas.md), en ese caso gana AACF. Aplican en especial [`aacf/rules/ai-output-safety.mdc`](../../aacf/rules/ai-output-safety.mdc), [`aacf/rules/security.mdc`](../../aacf/rules/security.mdc) y HR0, HR1, HR6, HR7, HR8, HR10, HR11, HR18/19 y HR21.

## Decisión

La capa IA cumple estas 10 reglas:

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

### Patrones de brdp-manager que no se copian

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

## Consecuencias

- **E08** crea el cliente LLM, la configuración de IA en admin y la lista blanca de proveedores y endpoints. Depende de qué proveedores estén permitidos ([pregunta abierta 3](README.md#preguntas-abiertas)).
- **E09–E12** consumen el cliente solo mediante endpoints de negocio `/api/ai/...`.
- **E13** añade evals de los activos de prompt con fixtures, apoyándose en la regla 10.
- Todo encargo que toque prompts pasa por `senior-prompt-engineer`, y todo el que toque LLM, datos o dependencias pasa por `security-reviewer`.
- El coste y la latencia aumentan respecto a brdp-manager (clasificador de intención, resumen en lugar de recorte, validación posterior). Es un coste aceptado a cambio de cumplir AACF.

## Estado

Propuesto.
