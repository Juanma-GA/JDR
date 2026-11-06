# ADR-004 — Idiomas de la UI

## Contexto

El mockup de referencia ofrece la interfaz en chino (zh), kazajo (kk) y español (es), con un diccionario en línea y el idioma guardado en `localStorage`. AACF exige que todo el texto de la UI sea localizable (HR15) y humanizado (HR21), y prohíbe guardar estado o preferencias en el navegador (HR1).

## Decisión

- La UI se ofrece en zh, kk y es, igual que el mockup, mediante una capa i18n. Ninguna cadena visible se escribe directamente en los componentes (HR15).
- El idioma elegido es una preferencia de usuario y se guarda en Postgres, no en `localStorage` (HR1).
- Los textos se humanizan antes de mostrarse (HR21).
- El idioma del **contenido** del mockup generado (paso 6 del asistente) es un dato del mockup, independiente del idioma de la UI.

## Consecuencias

- E02 (Diseño e i18n) elige la biblioteca i18n compatible con React + Vite, crea los catálogos `zh`, `kk` y `es` y migra los textos del mockup.
- Cada encargo posterior que añada texto de UI debe darlo en los tres idiomas (definición de hecho).
- Falta decidir si se añade inglés (pregunta abierta 4).

## Estado

Propuesto.
