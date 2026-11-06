# ADR-002 — Prioridad de reglas: AACF frente a brdp-manager

## Contexto

JDR se rige por AACF v2.0.0 (copia en [`aacf/`](../../aacf/)). Para diseñar la capa IA se toma como referencia `Juanma-GA/brdp-manager` (su `CLAUDE.md` y `src/api/`), que resuelve problemas parecidos: extracción de documentos, troceado, reintentos y chat. Algunos de sus patrones contradicen las Reglas Estrictas de AACF (ver la tabla de [ADR-003](ADR-003-capa-ia.md)).

## Decisión

Si brdp-manager y AACF chocan, gana AACF. De brdp-manager se toman ideas de diseño de la capa IA, nunca su pila, su UI ni su código. Dentro de AACF, cuando una regla general y una Regla Estricta se solapan, prevalece la Regla Estricta, como indica el propio AACF.

## Consecuencias

- Cada patrón que se tome de brdp-manager debe pasar la autocomprobación HR0–HR21 antes de entrar en un checklist.
- Las desviaciones conocidas están en la tabla de ADR-003. Si aparece otra, se añade ahí.
- Hay que confirmar si el repo de referencia (fork revisado) coincide con ATEXIS/BRDP-Manager ([pregunta abierta 7](README.md#preguntas-abiertas)).

## Estado

Propuesto.
