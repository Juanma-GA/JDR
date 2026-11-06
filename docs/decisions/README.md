# Decisiones de arquitectura (ADR)

Cada ADR tiene cuatro secciones: Contexto, Decisión, Consecuencias y Estado. Un ADR solo pasa de «Propuesto» a «Aceptado» cuando lo aprueba el responsable del proyecto.

| ADR | Título | Estado |
|---|---|---|
| [ADR-001](ADR-001-pila.md) | Pila tecnológica | Propuesto |
| [ADR-002](ADR-002-prioridad-de-reglas.md) | Prioridad de reglas: AACF frente a brdp-manager | Propuesto |
| [ADR-003](ADR-003-capa-ia.md) | Capa IA | Propuesto |
| [ADR-004](ADR-004-idiomas-ui.md) | Idiomas de la UI | Propuesto |
| [ADR-005](ADR-005-gobernanza.md) | Nivel de gobernanza | Propuesto |

## Preguntas abiertas

Los ADR remiten a estas preguntas por su número. Un ADR pasa a «Aceptado» cuando el responsable del proyecto responde las preguntas que le afectan.

| # | Pregunta | ADR afectado |
|---|---|---|
| 1 | ¿Pila AACF o la de brdp-manager? | ADR-001 |
| 2 | ¿Hay Keycloak disponible? | ADR-001 |
| 3 | ¿Qué proveedores LLM están permitidos (¿plataforma interna ATEXIS?)? | ADR-003 |
| 4 | ¿Añadimos inglés a los idiomas? | ADR-004 |
| 5 | ¿Paletas del mockup o solo las de `aacf/styles/branding.md`? | Sin ADR todavía; se decide antes de E02 (Diseño e i18n) |
| 6 | ¿Nivel de gobernanza T2, T3 o T4? | ADR-005 |
| 7 | ¿Es ATEXIS/BRDP-Manager igual al fork revisado (`Juanma-GA/brdp-manager`)? | ADR-002, ADR-003 |
