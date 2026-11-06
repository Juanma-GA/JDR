# JDR: Mockup Builder

JDR es la versión real del **Mockup Builder v1.1**: una herramienta web que guía al usuario por un asistente de 7 pasos con un chat IA y genera mockups HTML interactivos. El mockup de referencia está en [`docs/reference/mockup-builder-v1.1.html`](docs/reference/mockup-builder-v1.1.html).

> Estado: **E00**. El repo tiene las reglas de trabajo y las decisiones propuestas, pero todavía no hay código de aplicación. El andamiaje llega en E01.

## Reglas

El proyecto se rige por el **ATEXIS AI-Assisted Coding Framework (AACF) v2.0.0**:

- [`AGENTS.md`](AGENTS.md): punto de entrada para cualquier agente de IA.
- [`aacf/`](aacf/): copia literal del framework. No se edita.
- [`aacf/rules/atexis-hard-rules.md`](aacf/rules/atexis-hard-rules.md): Reglas Estrictas HR0–HR21.
- [`CLAUDE.md`](CLAUDE.md): reglas específicas de JDR (capa IA, ciclo de trabajo y adaptaciones de AACF).
- [`docs/decisions/`](docs/decisions/README.md): ADR.

## Cómo se trabaja

El trabajo se reparte en encargos (E00–E14, ver [`docs/plan/encargos.md`](docs/plan/encargos.md)). Cada encargo es una sesión, una rama `enc/EXX-nombre` y un PR. El ciclo es:

1. Checklist.
2. Aprobación.
3. Validación.
4. Implementación.
5. Verificación ejecutable.
6. Revisión.
7. PR.

Los subagentes de Claude Code están en [`.claude/agents/`](.claude/agents/). El ciclo completo y la definición de hecho están en [`CLAUDE.md`](CLAUDE.md#cómo-trabajamos).
