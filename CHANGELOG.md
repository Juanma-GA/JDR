# Changelog

Todos los cambios relevantes de JDR se registran aquí, por encargo. El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/).

## [E00] Repo y reglas — 2026-11-06

### Añadido

- **AACF:** copia literal de AACF v2.0.0 en `aacf/` (origen `Juanma-GA/aacf`, rama `claude/crear-carpetas-en-sp-e57wrh`, commit `d0ab40c`, carpeta `SP/`).
- **`AGENTS.md`:** en la raíz, el `AGENTS.md` de AACF con las rutas ajustadas a `aacf/`.
- **`CLAUDE.md`:**
  - lectura obligatoria;
  - producto;
  - capa IA (10 reglas);
  - cómo trabajamos;
  - adaptaciones de AACF a JDR;
  - arquitectura (pendiente hasta E01).
- **Subagentes:** los 7 subagentes de Claude Code en `.claude/agents/`, generados desde `aacf/agents/`. Siguen el principio de mínimo privilegio: los revisores no tienen Write ni Edit.
- **ADR:** ADR-001 a ADR-005 en `docs/decisions/`, todos en estado «Propuesto».
- **Plan:** plan de encargos E00–E14 en `docs/plan/encargos.md` y checklist de E00 en `docs/plan/checklists/`.
- **Mockup:** mockup de referencia en `docs/reference/mockup-builder-v1.1.html`.
- **README:** `README.md` mínimo.

### Eliminado

- `test.md`, de la prueba de conexión (HR13).
