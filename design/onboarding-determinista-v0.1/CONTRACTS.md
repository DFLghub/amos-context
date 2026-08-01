# Contratos y artefactos

## 1. Contrato atómico de afirmación

Toda afirmación publicada debe transportar, como mínimo, estos campos:

| Campo | Tipo | Obligatorio | Valores / reglas |
|---|---|---:|---|
| `topic` | string | sí | Identificador jerárquico normalizado, por ejemplo `wru/adoption/status`; no texto libre ambiguo. |
| `value` | JSON scalar/object/array | sí | Valor declarado por la fuente; no puede ser omitido ni sustituido por una inferencia silenciosa. |
| `status` | enum | sí | `CURRENT`, `DEGRADED_STALE`, `CONTRADICTED`, `UNKNOWN`, `HISTORICAL`, `SUPERSEDED`. |
| `observed_at` | RFC3339 UTC | sí | Momento en que la fuente observó el valor; no es el momento de ingestión. |
| `source` | object | sí | `source_id`, dominio, adaptador y referencia estable. |
| `source_version` | string | sí | Commit, versión de protocolo, registro o digest verificable. |
| `source_freshness` | object | sí | Estado, política aplicada, `verified_at`, `max_age`, cobertura y razón de degradación. |
| `precedence` | object | sí | Tier doctrinal, `decision_at` si existe, regla aplicada y `authority_basis`. |
| `evidence` | array | sí | Una o más referencias reproducibles: ruta, endpoint, receipt, commit, hash y fragmento/selector. |
| `supersedes` | array[string] | sí | IDs de afirmaciones reemplazadas; array vacío si no reemplaza ninguna. Nunca se infiere por silencio. |

Campos de integridad recomendados y obligatorios para el artefacto, aunque no sustituyen los diez campos atómicos:

```text
assertion_id
ingested_at
scope
producer_version
```

`status` es el estado de la afirmación dentro de la proyección DCSA. La observación original permanece en el ledger aunque la proyección la marque `SUPERSEDED` o `HISTORICAL`.

## 2. Artefacto DFL-CURRENT-STATE

El documento raíz debe contener:

```text
schema: dfl.current_state.v0.1
state_version: content-addressed immutable version
produced_at: RFC3339
producer: amOS/DCSA + producer_version
authority_model: A>B>C>D>E
domains: map[domain] freshness/coverage summary
assertions: array[atomic assertion]
contradictions: array[contradiction record]
open_missions: array[mission dependency record]
news_cursor: monotonic cursor
```

`state_version` cambia cuando cambia cualquier afirmación, contradicción, dependencia o frescura relevante. `produced_at` describe la producción; no prueba frescura de las fuentes.

Cada `contradiction` debe incluir:

```text
topic
assertion_ids
incompatible_values
reason
blocking_missions
resolution_required
```

## 3. Noticiero institucional append-only

Ruta lógica propuesta:

```text
/opt/dfl-artifacts/amos/onboarding-news/YYYY-MM-DD.jsonl
```

Una noticia tiene:

```text
news_id
sequence
date
topic
system
change
state_before
state_after
evidence
commit
authority
tags
state_version
created_at
```

`state_before` y `state_after` son snapshots resumidos con `status`, no texto narrativo libre. `tags` incluye etiquetas como `F1B`, `WRU`, `SFV5`, `JPI`; no decide autoridad.

### Generación

La fuente primaria debe ser la transición detectada por DCSA, no el texto de `@$fin`.

- `@$fin` puede producir un evento/observación en Engram.
- DCSA ingiere el evento, resuelve precedencia y solo entonces genera una noticia si existe transición real.
- Una noticia nunca se genera porque un agente declaró “cerrado”.
- La noticia conserva evidencia y `state_version`; si la transición se revierte, se agrega otra noticia, no se edita la anterior.

### Consultas mínimas

- `since(sequence|cursor)` para “qué cambió desde cursor X”.
- `date_range(start,end)` para “últimos 7 días”.
- `tag(F1B)` o `topic_prefix(...)` para “todo sobre F1B”.
- `status(SUPERSEDED)` para “qué quedó superseded”.
- `state_version(version)` para reconstruir el contexto de una noticia.

El cursor debe ser monotónico por partición diaria y globalmente ordenable mediante `sequence`; no se debe depender del mtime del archivo.

## 4. Receipt de onboarding

Ruta lógica propuesta:

```text
/opt/dfl-artifacts/amos/onboarding-receipts/YYYY-MM.jsonl
```

Contrato:

```text
receipt_id
agent
timestamp
sources_read: array[source_read]
current_state_version
topics_loaded
contradictions_detected
last_news_cursor
open_mission_identified
result
gate_access
gate_comprehension
dependency_decision
```

`sources_read` debe guardar endpoint/ruta, `source_version`, `observed_at`, `freshness_status` y cobertura. `topics_loaded` enumera los topics realmente cargados; no basta con declarar “contexto completo”.

`result` debe ser uno de:

```text
READY_TO_ACT
READY_WITH_DEGRADED_CONTEXT
BLOCKED_DEPENDENCY
BLOCKED_CONTRADICTION
UNKNOWN_STATE
```

El receipt lo escribe el sistema de onboarding después de ejecutar el gate. El agente puede enviar sus respuestas, pero no puede autoemitir ni autoaprobar el receipt.

Consultas mínimas:

- `agent + date_range`;
- `topics contains F1B`;
- `last_news_cursor` range;
- `result != READY_TO_ACT`;
- `current_state_version` exacto.

## 5. Modelo de dependencia de misión

Una misión declarada debe resolver a un manifiesto:

```text
mission_id
mission_topics
required_domains
required_evidence_classes
blocking_statuses
non_blocking_statuses
```

Ejemplos:

| Misión | Topics | Fuentes críticas |
|---|---|---|
| F1B readiness | `concierge/f1b/readiness`, `concierge/live-path` | revisión CX, proxy operativo, receipts de propagación |
| WRU promoción | `wru/adoption/status`, `wru/graph/live` | WRU query, commit fuente, graph live evidence |
| onboarding general | protocolo, restricciones, misión abierta | DCSA, news cursor, sources de misión |

Una fuente stale fuera de `required_domains` produce `READY_WITH_DEGRADED_CONTEXT`, no bloqueo global.
