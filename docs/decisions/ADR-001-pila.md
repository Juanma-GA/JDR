# ADR-001 — Pila tecnológica

## Contexto

JDR sustituye un mockup HTML de un solo fichero ([`docs/reference/mockup-builder-v1.1.html`](../reference/mockup-builder-v1.1.html)) por una aplicación real con usuarios, persistencia y una capa IA en el servidor. AACF ofrece una plantilla de aplicación web ([`aacf/templates/web-app.md`](../../aacf/templates/web-app.md)) y fija dos reglas que condicionan la elección:

- Postgres es la fuente de verdad y nada de estado en el navegador (HR1).
- La autenticación va en cookies HTTP-only fijadas por el servidor.

brdp-manager, que sirve de referencia para la capa IA, usa otra pila (Vite + JS con un `server.js` de Node). Eso es precisamente lo que plantea la pregunta abierta 1.

## Decisión

Usar la plantilla `aacf/templates/web-app.md`:

- **Frontend:** React 18 + TypeScript + Vite 6
- **Estilos:** Tailwind CSS 4 + shadcn/ui
- **Estado de cliente:** Zustand, solo para estado de UI y nunca como fuente de verdad (HR1)
- **Backend:** FastAPI + SQLAlchemy 2.0 async
- **Base de datos:** PostgreSQL 16
- **Autenticación:** Keycloak OIDC; la sesión va en una cookie HTTP-only fijada por el servidor
- **Estructura:** `frontend/` y `backend/`, como en la plantilla

## Consecuencias

- E01 monta el andamiaje con esta pila (docker-compose, CI y dependencias fijadas).
- De brdp-manager no se reutiliza código; solo se toma como referencia de diseño para la capa IA (ADR-002, ADR-003).
- Hace falta un Keycloak disponible, en desarrollo y en despliegue (pregunta abierta 2).
- Aplican las reglas de lenguaje `aacf/rules/python.mdc` y `aacf/rules/javascript.mdc`. Además, nunca se usa `from __future__ import annotations` en ficheros de rutas FastAPI.

## Estado

Propuesto.
