# Gate de comprensión, frescura y mirror

## 1. Composición con el gate actual

El gate actual de `main.py` valida acceso/protocolo mediante `SOURCE`, `PROFILE`, `ACCESS`, `FIN` y `NO_TOUCH`. Ese gate debe conservarse como **Gate 0: Access & Safety**.

Después de cargar DFL-CURRENT-STATE, el onboarding ejecuta **Gate 1: Understanding & Freshness**. Gate 1 no reemplaza las protecciones de acceso; las extiende con estado verificable.

La secuencia conceptual es:

```text
Gate 0: acceso, perfil, transporte, superficies protegidas
        ↓
cargar DFL-CURRENT-STATE + news window
        ↓
resolver misión y dependencias
        ↓
Gate 1: comprensión, supersesión, contradicciones, frescura
        ↓
receipt emitido por el sistema
        ↓
READY_TO_ACT / degraded / blocked
```

## 2. Respuesta obligatoria del Gate 1

El consumidor debe producir exactamente estos campos semánticos:

```text
CURRENT_STATE_LOADED: yes|no
STATE_VERSION: <immutable state_version>
RECENT_NEWS_WINDOW: <cursor + date range + count>
OPEN_MISSION_IDENTIFIED: <mission_id|none|unknown>
SUPERSEDED_STATES_FILTERED: yes|no
CONTRADICTIONS: 0|[contradiction_id,...]
READY_TO_ACT: yes|no|degraded
```

El sistema valida los valores contra DCSA; no acepta una respuesta textual incompatible con el artefacto cargado.

### Criterios de paso

- `CURRENT_STATE_LOADED=yes` y `STATE_VERSION` existe en DCSA.
- `RECENT_NEWS_WINDOW` contiene cursor y ventana solicitada.
- `OPEN_MISSION_IDENTIFIED` es explícito; `none` es válido solo si el sistema confirma que no hay misión abierta.
- `SUPERSEDED_STATES_FILTERED=yes` y el conjunto de topics no contiene una aserción marcada `SUPERSEDED` como vigente.
- `CONTRADICTIONS=0`, salvo que la misión no dependa de esos topics y el resultado sea `degraded`.
- `READY_TO_ACT` solo puede ser `yes` cuando las dependencias críticas son frescas y no contradictorias.

## 3. Bloqueo por dependencia, no por contaminación global

El gate calcula:

```text
critical = required_domains ∪ required_topics ∪ required_evidence_classes
blocking = assertions in critical with status
           ∈ {DEGRADED_STALE, CONTRADICTED, UNKNOWN}
```

Reglas:

1. Si `blocking` está vacío: `READY_TO_ACT=yes`.
2. Si hay degradación solo en una dependencia opcional: `READY_TO_ACT=degraded`.
3. Si hay `CONTRADICTED` o `UNKNOWN` en una dependencia crítica: `BLOCKED_CONTRADICTION` o `UNKNOWN_STATE`.
4. Si una dependencia crítica es `DEGRADED_STALE`: `BLOCKED_DEPENDENCY` para esa misión, no para todo DFL.
5. El receipt debe listar exactamente qué assertion/topic produjo el bloqueo.

Esto implementa D5: no se bloquea toda publicación por una fuente stale, pero tampoco se permite actuar sobre una misión que depende de esa fuente.

## 4. Regla de misión abierta

La misión se identifica en este orden:

1. misión explícita de la entrada del usuario;
2. misión abierta de DCSA, con `mission_id`, autoridad, estado y evidence;
3. ninguna misión, si DCSA confirma que no hay una abierta;
4. `UNKNOWN` si existen señales incompatibles y no hay resolución.

El agente no puede elegir silenciosamente una misión histórica porque aparezca primero en Engram o en el mirror.

## 5. Hotfix mínimo de honestidad del mirror

El mirror actual debe dejar de afirmar sin condiciones:

> “Any agent reading this file has current DFL operational state.”

Diseño mínimo:

- reemplazarlo por “This file is a cached projection; consult DCSA/live `/go` for current state”;
- mostrar `generated_at` visible;
- mostrar `snapshot_age_seconds` o `snapshot_age_human` calculada contra `now` al generar;
- mostrar `state_version` y commit del productor;
- mostrar `source_versions` y frescura por dominio;
- mostrar `STALE` si la edad supera el TTL del mirror;
- mostrar `CONTRADICTED` si la proyección contiene contradicciones abiertas;
- exigir al consumidor contrastar `state_version` con DCSA/live `/go` antes de actuar;
- si no puede contrastar, clasificar la sesión como snapshot/degraded y prohibir `READY_TO_ACT=yes` para misiones críticas.

El hotfix no debe inventar un TTL global de dominio. El TTL del mirror mide edad del archivo; la frescura semántica sigue viniendo de cada assertion/domain.

## 6. Contrato de transporte del mirror

El front matter o encabezado visible debe incluir:

```text
MIRROR_KIND: cached_projection
GENERATED_AT: RFC3339
SNAPSHOT_AGE: duration
STATE_VERSION: immutable version or UNKNOWN
PRODUCER_VERSION: amOS/DCSA version
MIRROR_COMMIT: git commit
DOMAIN_FRESHNESS: map[domain] status
OPEN_CONTRADICTIONS: count
REVALIDATION_REQUIRED: yes|no
```

Un mirror sin `STATE_VERSION` no puede ser presentado como onboarding completo. Un mirror con `STATE_VERSION` pero sin evidencia de revalidación solo permite `READY_WITH_DEGRADED_CONTEXT`.

## 7. Receipts y auditoría del gate

El receipt debe preservar:

- payload/estado consumido por `state_version`;
- news cursor y ventana;
- respuestas del Gate 0 y Gate 1;
- lista de topics cargados;
- contradicciones presentes, aunque no bloqueen la misión;
- dependencia que determinó el resultado;
- fuente que fue stale o unknown.

Esto permite responder posteriormente “qué sabía el agente” sin confundir la hora del onboarding con la frescura de cada dominio.

## 8. No implementación en esta misión

Este documento no modifica ni crea:

- `/opt/dfl-context-proxy`;
- `/opt/dfl-context-proxy-f1b-*`;
- WRU, KNL, grafos o Engram;
- Conserje o NEXUS;
- cron, systemd, variables de entorno o mirror público.

La carpeta de diseño contiene únicamente la especificación aprobada para una futura PRP/implementación separada.
