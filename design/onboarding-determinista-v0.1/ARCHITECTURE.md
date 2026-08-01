# Arquitectura del productor canónico

## 1. DCSA: responsabilidad y límites

DFL Current State Authority (DCSA) es una pieza de amOS. Su salida es una proyección semántica, no un reemplazo de las fuentes.

| Componente | Puede hacer | No puede hacer |
|---|---|---|
| amOS/DCSA | definir contrato, resolver precedencia, declarar estados, producir gates | inventar evidencia o sustituir la fuente de dominio |
| Engram | conservar observaciones/eventos y sus referencias | decidir verdad vigente |
| WRU/KNL/grafos | publicar estado de su dominio, versión, cobertura y frescura | declarar el estado total de DFL |
| Conserje | consultar DCSA, orientar y derivar | compilar, decidir verdad u onboardear |
| Onboarding | consumir DCSA, verificar comprensión y escribir receipt | reinterpretar precedencia |
| NEXUS | gestionar lifecycle del activo | ser autoridad semántica |

## 2. Ingestión de fuentes

DCSA ingiere adaptadores read-only, cada uno con un manifiesto de fuente:

```text
source_id
domain
authority_tier
source_version
observed_at
coverage
freshness_policy
adapter_result
evidence_refs
```

Adaptadores previstos:

- **Engram:** observaciones, decisiones y eventos; conserva `id`, `sync_id`, timestamps, lifecycle y referencias. `LIFECYCLE: archived` se trata como no vigente, no como borrado.
- **WRU query:** consume exclusivamente `wru.query.v1` con autoridad `READER`; conserva `source_commit_observed`, `freshness_status`, cobertura y resultado.
- **KNL/graph:** consume `knl.json`, `graph_context_light.json` o interfaz equivalente con commit/fecha de construcción, cobertura y skips.
- **Evidence CURRENT-STATE locales:** solo se aceptan si llevan `source_version`, autoridad declarada, ámbito, evidencia y commit. Una carpeta de evidencia no se convierte automáticamente en verdad global.
- **`/go`:** se acepta como transporte/agregación observada, nunca como fuente semántica superior. Cada bloque debe conservar su `source_id` original; el `generated_at` del proxy solo describe la agregación.

El adaptador no puede convertir ausencia de respuesta en `CURRENT`. Debe producir `UNKNOWN` con evidencia del error o de la ausencia.

## 3. Identidad de una afirmación

Cada afirmación recibe un `assertion_id` determinista calculado sobre `topic`, `value`, `source`, `source_version`, `observed_at` y `evidence`. El ID permite deduplicación y referencias de `supersedes`; no reemplaza la evidencia.

El productor mantiene dos vistas:

1. **Assertion ledger:** todas las afirmaciones recibidas, incluyendo históricas y contradichas.
2. **Current projection:** una decisión por topic, más contradicciones y dependencias abiertas.

## 4. Precedencia por topic

La resolución ocurre por topic canónico, no por payload completo. El algoritmo es:

1. Normalizar topic y comparar solo afirmaciones del mismo ámbito semántico.
2. Eliminar de la vista candidata las afirmaciones `HISTORICAL`/`SUPERSEDED` ya resueltas.
3. Aplicar relaciones explícitas: si `A.supersedes = B.assertion_id`, B queda `SUPERSEDED` por A, aunque B tenga un timestamp posterior de ingestión.
4. Si no hay relación explícita, ordenar por autoridad doctrinal: `A > B > C > D > E`.
5. Dentro de la misma autoridad, gana el timestamp de decisión/observación más reciente **solo si** la fuente declara que la afirmación reemplaza el estado anterior. Si no lo declara, ambas permanecen activas y se evalúan como posible contradicción.
6. Resolver empates por `source_version` semver/commit comparable.
7. Resolver empate final por `assertion_id` solo para determinismo técnico; nunca se presenta como evidencia semántica.
8. Si dos afirmaciones incompatibles permanecen activas sin cadena válida de supersesión, la proyección del topic queda `CONTRADICTED`, aunque se exponga una candidata provisional para diagnóstico.

La precedencia elige el resultado; no borra la disputa. Una autoridad superior puede hacer que una afirmación sea `SUPERSEDED` únicamente cuando su alcance y evidencia permiten invalidarla. “Más nueva” por sí sola no borra una contradicción doctrinal.

### Caso WRU obligatorio

Para el topic `wru/adoption/status`:

```text
BUILD_VERIFIED_READY_FOR_MERGE
  → SUPERSEDED por INSTITUTIONALLY_ADOPTED
  → SUPERSEDED por GRAPH_LIVE_PROMOTION_COMPLETE
```

La cadena debe aparecer en `supersedes` y en el historial. La proyección actual es `GRAPH_LIVE_PROMOTION_COMPLETE` solo si la evidencia de grafo vivo, commit y cobertura está presente y fresca. Si el grafo está stale, el valor puede conservarse como `HISTORICAL`/`DEGRADED_STALE`, no como CURRENT fresco.

### Caso F1B

`shadow-ready` y `REQUIRES_CHANGES` para el mismo ámbito de integración no se resuelven por la mera proximidad temporal. Si no existe `supersedes` explícito y ambos siguen afirmando readiness incompatible, DCSA publica:

- una candidata de precedencia para inspección;
- `CONTRADICTED` en el topic;
- ambos `assertion_id` en `contradictions`;
- bloqueo solo para misiones que dependan de la readiness F1B.

## 5. Frescura por afirmación y dominio

Cada dominio tiene una política versionada:

```text
domain
observed_at
verified_at
max_age
source_version
coverage
freshness_status
```

`source_freshness` se calcula así:

- `FRESH`: versión/commit observado y fecha dentro de `max_age`.
- `DEGRADED_STALE`: existe evidencia, pero excede `max_age`, tiene commit divergente o cobertura incompleta.
- `UNKNOWN`: no se pudo comprobar versión, fecha o cobertura.

La frescura del agregado se expresa como un mapa por dominio y por assertion. Nunca se reduce a un `generated_at` único. Un `/go` generado ahora puede contener WRU stale y KNL stale simultáneamente.

## 6. Salida DFL-CURRENT-STATE

Nombre lógico: `DFL-CURRENT-STATE`.

Proyección propuesta:

```text
/opt/dfl-artifacts/amos/current-state/
  DFL-CURRENT-STATE.json
  manifest.json
  assertions.jsonl
```

La ruta es un diseño objetivo; esta misión no la crea ni modifica.

El artefacto es publicado por DCSA, firmado por su `state_version` content-addressed y expuesto mediante una interfaz read-only de amOS. El proxy `/go` debe envolverse después para transportar una referencia al estado canónico y, si se mantiene el mirror, una proyección explícitamente marcada como snapshot.

Cadencia:

- evento: nueva observación, transición, supersesión o cambio de freshness;
- reconciliación periódica: fallback operativo, recomendado cada 5 minutos;
- publicación pública: después de reconciliación exitosa, sin afirmar frescura si no se verificó.

## 7. Relación con la cadena actual

La cadena existente es `Engram → main.py → /go → publish-amos-context.sh → mirror → raw`. El diseño no la reemplaza de golpe:

1. DCSA se inserta entre las fuentes y `/go` como autoridad semántica.
2. `/go` conserva temporalmente su payload legacy, pero añade `state_version`, referencia a DCSA y frescura por assertion.
3. `publish-amos-context.sh` deja de presentar el mirror como estado actual global y publica una proyección con edad, versión y advertencias.
4. El raw mirror queda como transporte cacheado, no como autoridad.

La justificación es directa: el código actual categoriza observaciones, descarta algunos archivados y genera Markdown desde el payload, pero no contiene algoritmo de precedencia por topic ni productor canónico. DCSA debe envolver esa cadena, no hacer que Engram, WRU o Conserje asuman autoridad que D1–D3 les niegan.
