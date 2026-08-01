# DFL Onboarding Determinista v0.1

## Estado del documento

- **Modo:** diseño y especificación; no implementación.
- **Dueño institucional:** amOS.
- **Componente provisional:** DFL Current State Authority (DCSA).
- **Alcance:** productor canónico de estado, noticiero institucional, receipts de onboarding, gate de comprensión/frescura y hotfix de honestidad del mirror.
- **No autoriza:** cambios en `dfl-context-proxy`, Engram, WRU, KNL, Conserje, NEXUS, cron, variables de entorno ni superficies protegidas.

## Decisiones cerradas

1. La falla es la ausencia de un productor canónico de estado con precedencia temporal; no se reabre el diagnóstico de closer/opener.
2. amOS es dueño semántico e institucional del estado actual de DFL.
3. Engram conserva historia; WRU/KNL/grafos publican estado de dominio; Conserje consulta y deriva; onboarding consume y deja receipt; NEXUS gestiona lifecycle.
4. La frescura es por afirmación y por dominio. `generated_at` de un payload nunca prueba frescura global.
5. Una fuente stale no bloquea todo: bloquea solo decisiones/misiones que dependan de ella.

## Documentos

- [`ARCHITECTURE.md`](ARCHITECTURE.md): productor, ingestión, precedencia, contradicciones y frescura.
- [`CONTRACTS.md`](CONTRACTS.md): contrato atómico, artefactos, noticiero y receipts.
- [`GATES.md`](GATES.md): gate de comprensión/frescura, dependencia de misión, consultas y hotfix del mirror.

## Principio rector

El sistema no debe preguntar “¿cuál fue la última observación?” sino “¿cuál es la afirmación canónica vigente para este topic, con qué autoridad, evidencia, frescura y contradicciones?”.
