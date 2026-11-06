# ADR-005 — Nivel de gobernanza

## Contexto

AACF define cuatro niveles de gobernanza ([`aacf/governance/tier-checklists.md`](../../aacf/governance/tier-checklists.md)):

- **T1:** autoservicio, herramientas de IDE.
- **T2:** aprobado por IS, herramientas internas.
- **T3:** IS + datos, con acceso al almacén de datos.
- **T4:** producción, IS + TI conjunto.

El nivel decide qué controles, revisiones y endurecimiento (`codebase-hardening`) son obligatorios. JDR sirve a usuarios internos, maneja documentos de referencia que pueden ser de cliente y llama a proveedores LLM externos. Por eso no está claro si es T2, T3 o T4.

## Decisión

Por decidir entre T2, T3 y T4 ([pregunta abierta 6](README.md#preguntas-abiertas)). Mientras tanto, cada encargo aplica como mínimo los controles de T2, y E14 (seguridad, hardening y despliegue) se planifica con `codebase-hardening`.

## Consecuencias

- Los checklists de E03 (auth y config), E07 (subida de referencias) y E08 (cliente LLM) dependen del nivel: retención de datos, registro de auditoría y proveedores permitidos.
- Si el nivel es T3 o T4, `security-reviewer` pasa a ser obligatorio en más encargos y E14 requiere el informe de endurecimiento completo.

## Estado

Propuesto (nivel por decidir).
