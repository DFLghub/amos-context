# amOS Context — @$go Live Mirror
**Generated:** 2026-07-31T21:45:32Z  
**Protocol:** @$go v1.1  
**Rule:** Any agent reading this file has current DFL operational state.  
**Source B (live JSON):** https://context.deepfeelingslabs.com/go  

> This file updates on event (`@$fin` cierre ordenado, or the resilient-close  
> watchdog) with a daily 3:05am UTC cron as fallback — not the primary cadence.  
> For real-time payload: `GET https://context.deepfeelingslabs.com/go`  
> For deep graph: `GET https://context.deepfeelingslabs.com/go?deep=1`

> **PROTOCOL UPDATE ALERT:** Antes de operar, todo agente debe pasar el @$go VALIDATION GATE. No alcanza con declarar perfil: debe reportar fuente, timestamp, perfil, access model, modo @$fin y superficies protegidas.

---

## AGENT DIRECTORY

**Paso 0 — autodiagnóstico obligatorio antes de intentar `@$go`:** leé [`AGENT_CAPABILITY_MATRIX.md`](https://raw.githubusercontent.com/DFLghub/amos-context/main/AGENT_CAPABILITY_MATRIX.md) primero. Es la barrera de entrada, no una referencia posterior — si tu diagnóstico dice que no tenés una capacidad, no la intentes, seguí el fallback de esa fila.

Landing here for the first time? Find your profile, read your annex, obey its contract.

> **ChatGPT Work / offline fallback:** `DisabledError` o `not safe to open` no significa 
> onboarding fallido: clasifica la sesión como CONSULTOR. Usá el offline bootstrap capsule 
> de las instrucciones de la sesión y completá el gate; si Work ofrece cloud browser, se 
> permite antes un único intento sobre la página HTML pública del repositorio GitHub.

| Perfil | ¿Sos vos? | `@$go` | `@$fin` | Anexo |
|---|---|---|---|---|
| **EJECUTOR** | ¿Tenés brazo en La Garra (bash/Engram/git)? Sí → sos EJECUTOR. | FULL | CIERRE (FULL) | https://raw.githubusercontent.com/DFLghub/amos-context/main/agents/ejecutor.md |
| **ORQUESTADOR** | ¿Sin brazo, pero con fetch público confiable (HTTP/navegación)? Sí → sos ORQUESTADOR. | PARTIAL | PARTIAL (relay) | https://raw.githubusercontent.com/DFLghub/amos-context/main/agents/orquestador.md |
| **CONSULTOR** | ¿Sin brazo y sin fetch público garantizado (chat puro)? Sí → sos CONSULTOR. | NONE — no intentar | CHECKPOINT (relay) | https://raw.githubusercontent.com/DFLghub/amos-context/main/agents/consultor.md |

---

## SESSION CONTRACT

Contrato universal para cualquier agente en el ecosistema DFL/amOS, sea cual sea su perfil:

- **`@$go`** — comando del agente que activa el bootstrap. **`/go`** — ruta HTTP del proxy. No son lo mismo, no se intercambian.
- **`@$fin`** — comando de cierre del agente, simétrico a `@$go`. Local, no tiene ruta HTTP.
- **Uniformidad real:** `@$go`/`@$fin` son uniformes por contrato semántico, no por transporte. EJECUTOR usa shell/Engram/git; ORQUESTADOR usa fetch público si puede; CONSULTOR usa snapshot pegado o memoria local y entrega relay.
- **Perfil por capacidad, no por marca:** Claude, Codex, ChatGPT, Hermes, OpenClaw u otros agentes se clasifican por capacidades observables de esa sesión, no por nombre del modelo.
  - **Modo CIERRE** (default): Gate 4B final (`mem_save` del resumen + `mem_search`/`mem_update` de lo que este cierre archiva) + `push_mirror.sh` + reportar la línea `MIRROR: ...` que imprime (commit real vía git log — nunca re-consultar `/go` para esto).
  - **Modo CHECKPOINT** (solo si Jorge lo pide explícitamente con esa palabra): `mem_save` del progreso parcial, sin barrido de archivado y sin `push_mirror.sh` — la sesión sigue abierta.
- **Gate 4B incremental**: `mem_save` en cada commit, decisión o blocker resuelto durante la sesión — no esperar al cierre. Es lo que hace sobrevivir el estado si la sesión muere sin `@$fin`.
- **Zonas protegidas** (no tocar sin PRP explícito): `puntajeTigreKnockout`, Supabase, Vercel config, environment variables, templates HLC-T01/T02/T03, CRON 3:05am UTC, `/etc/dfl-secrets`.
- **Precedencia**: A (Constitution) > B (Routing/MASTER_INDEX) > C (Jurisprudence/MASTER_BITACORA) > D (Operation — Engram, PRPs, skills) > E (Archive). Engram es capa D — nunca invalida A ni B.

---

## ACCESS MODEL — UNIFORM CONTRACT, DIFFERENT TRANSPORTS

- **Principle:** @$go y @$fin son comandos uniformes por contrato semántico; el transporte no es uniforme. Cada agente usa el adaptador permitido por sus capacidades reales de sesión.
- **Not by brand:** El perfil se decide por capacidades observables, no por marca de modelo. Codex, Claude, ChatGPT, Hermes u OpenClaw pueden caer en perfiles distintos según tengan shell/Engram/git, fetch público confiable o solo chat.
- **Snapshot rule:** Para CONSULTOR, el contexto pegado es snapshot fechado, no verdad viva. Debe razonar con ese contexto y marcar cualquier acción concreta para relay.

| Perfil | `@$go` adapter | `@$fin` adapter | Escritura de estado |
|---|---|---|---|
| **EJECUTOR** | curl/fetch a /go + leer anexo + search_memory('contexto DFL') | save_memory/mem_save + archivado Gate 4B + push_mirror.sh | direct |
| **ORQUESTADOR** | fetch público del mirror/payload si la red lo permite | bitácora de relay para que un EJECUTOR cierre Gate 4B | relay |
| **CONSULTOR** | offline bootstrap capsule, snapshot pegado o memoria local; un fetch bloqueado clasifica CONSULTOR y no falla el onboarding | RESUMEN DE SESIÓN listo para EJECUTOR/Engram | relay |

---

## @$go VALIDATION GATE

**Purpose:** One-shot gate compacto para demostrar lectura mínima antes de operar.
**Max response:** 6 lines.

Antes de operar, respondé:

- `SOURCE: <URL o snapshot pegado + generated_at/Generated exacto>`
- `PROFILE: <EJECUTOR|ORQUESTADOR|CONSULTOR> porque <capacidad real observada>`
- `ACCESS: contrato uniforme; transporte por adaptador`
- `FIN: <cierre real|relay|checkpoint> + qué NO puedo hacer`
- `NO_TOUCH: puntajeTigreKnockout, Supabase, Vercel config, env vars, templates HLC-T01/T02/T03, CRON 3:05am UTC, /etc/dfl-secrets`

**PASS criteria:**
- Incluye fuente y timestamp exacto.
- Perfil decidido por capacidad real, no por marca/modelo.
- Distingue contrato uniforme de transporte por adaptador.
- ORQUESTADOR/CONSULTOR no reclaman escribir Engram ni correr push_mirror.sh.
- Lista completa de superficies protegidas.

**Failure rule:** Una corrección permitida. Segundo fallo: degradar a CONSULTOR o pedir EJECUTOR; no operar sobre producto ni estado DFL. Un bloqueo HTTP (DisabledError/not safe to open) clasifica como CONSULTOR y no constituye un onboarding fallido.

---

## IDENTITY

- **Ecosystem:** DFL / amOS
- **Grounding anchor:** La Garra — 67.205.166.199 (DigitalOcean NYC1)
- **Source A — PRIMARY (this file):** https://raw.githubusercontent.com/DFLghub/amos-context/main/amos-context.md
  - PRIMARY — probá esto primero, siempre. GitHub raw está allowlisteado en la gran mayoría de sandboxes de agentes LLM (lo necesitan pip/npm/git); un dominio custom como context.deepfeelingslabs.com casi nunca lo está.
- **Source B — SECONDARY (live JSON):** https://context.deepfeelingslabs.com/go
  - SECONDARY — payload JSON vivo (decisiones/constraints/pending/KNL), más rico que el markdown estático de Source A. Intentalo después de Source A, y solo si tu sandbox permite egress a dominios custom (no solo a github.com).
- **Restriction:** Este payload es suficiente para operar. PROHIBIDO solicitar MASTER_INDEX, MASTER_BITACORA o archivos adicionales de /opt/dfl-knowledge/ después de recibir este payload. Para grafo completo: GET /go?deep=1
- **Sandbox network note:** Muchos sandboxes de agentes LLM restringen el egress HTTP a un allowlist que incluye github.com/raw.githubusercontent.com pero NO dominios custom como context.deepfeelingslabs.com. Si el fetch a Source B falla, cuelga o hace timeout, no es un error del servicio — es una restricción de red de tu entorno. Seguí operando con Source A (amos-context.md), que ya tenés si estás leyendo esto. Si reportás el bloqueo, decilo así de claro: 'No pude alcanzar context.deepfeelingslabs.com (probable restricción de egress del sandbox); usando amos-context.md (GitHub raw) como fuente operativa.'

---

## RECENT DECISIONS

### SESSION CLOSURE: WRU v0.1 adopción institucional y relay para reanudación
**Type:** decision  
**Project:** dfl  

CIERRE DE SESIÓN — WRU v0.1. No se abrió investigación nueva ni se modificó código, merge, dirty state, índices o grafos.

ESTADO TÉCNICO: WRU v0.1 mergeado en /opt/saas-factory-setup; merge bec766e; HEAD final reportado bc8d6b89e7e4db1ba4b83da8055579c56bc89759; tag wru-v0.1-institutional-final2. Módulo canónico reportado en saas-factory/tools/workforce-registry; interfaces wru.query.v1, wru-refresh.v1 y wru-status.v1. Integrity 23/23; suite WRU 83/83 PASS; G1-G44 reportados PASS.

ESTADO INSTITUCIONAL: Adoption Gate 9/9 PASS; consumidor externo DFL Concierge en /opt/dfl-knowledge-workunit; consumer HEAD reportado 5f01fb29a2bfd8e8422524f616108a8996317488; suite Concierge 258/258 PASS. Camino vivo probado: Concierge solicita capability por wru.query.v1 READER-only → WRU consulta Canonical State → devuelve availability/freshness/provenance → Concierge selecciona sfv5-skill.acquisition → deriva WorkUnit IDs → WorkUnitLedger claim/release → receipt externo.

RECEIPT: /opt/dfl-knowledge/evidence/wru-live-concierge-20260731-r3/wru-receipt.json; digest reportado e5da6697cfedd9a0e0062ab81978e57f79d0488336283d16fd8801669a0c4932; active_claims=[]; resultado WORK_UNIT_CLAIMED_AND_RELEASED. Evidencia principal reportada: /opt/dfl-knowledge/evidence/wru-institutional-adoption-2026-07-31/ADOPTION-GATE.md. El usuario indicó /opt/saas-factory-setup/evidence/wru-institutional-adoption-2026-07-31/ADOPTION-GATE.md; conservar ambas referencias en el relay sin investigar ahora.

FRESHNESS: la respuesta viva fue DEGRADED_STALE porque los records canónicos conservan source_commit histórico distinto del source HEAD observado; el consumidor no lo trató como autoridad plena y registró READER_DEGRADED con provenance/availability explícitos.

DISTINCIÓN OBLIGATORIA: WRU CAMINO_VIVO_INSTITUCIONAL_PROBADO: YES. Capacidades catalogadas: 0/32 PROBADO_EN_CAMINO_VIVO.

DIRTY STATE: el árbol compartido tenía dirty state preexistente ajeno; no se investigó ni limpió en este cierre. Queda pendiente de aclaración factual futura y no debe asumirse como fallo de WRU sin evidencia.

ÍNDICES/GRAFOS: se generaron índices/grafos candidatos y luego se registró un estado canónico post-merge. No afirmar más de lo demostrado. Queda pendiente para una sesión futura verificar rutas exactas, conteos finales canónicos, motor que los sirve, integración real con Codebase Memory/Visualizer/Graphify/agTopólogo, frescura, mecanismo de actualización y promoción al grafo institucional mayor. JSON/DOT en evidence no bastan: debe probarse que forman parte del sistema gráfico vivo y no de otro closet documental.

PRINCIPIO INSTITUCIONAL: todo elemento nuevo DFL debe generar índices, nodos, relaciones, procedencia y frescura como parte de la cultura de fabricación, para que un invento exitoso e integrado sea conocido por el sistema institucional y pueda incorporarse al grafo mayor, sus nodos, relaciones y ramas.

DEUDA RESIDUAL REAL: freshness canónica todavía degradada hasta reconciliación gobernada; verificación de integración con el grafo institucional mayor pendiente; dirty state preexistente pendiente de aclaración factual. Deuda hipotética no se presenta como real.

REANUDACIÓN EXACTA: validar primero la ubicación efectiva de ADOPTION-GATE.md y luego comprobar el registro canónico vivo de índices/grafos y su mecanismo de servicio/actualización, sin reabrir la fabricación WRU. Próxima sesión sugerida: WRU-GRAPH-LIVE-PROMOTION-2026-08-01.

### SESSION CLOSURE: WRU remediation final verification and candidate graph audit
**Type:** decision  
**Project:** dfl  

Gate 4B cierre 2026-07-31. Workforce Registry Unit v0.1 remediation independently verified in isolated worktree /opt/wru-worktree-v0.1, branch feat/workforce-registry-unit-v0.1, HEAD 31dcbace506a8b83ca8e16b93cf7ac15f2a07db1. Productive tree /opt/saas-factory-setup remained at d12693998c38c7d5b1f83a74135dd65bb8ab57bf; no merge or shared activation. Full suite 80/80 and remediation suite 6/6 passed. Four defects verified closed: multiprocess optimistic locking, mandatory canonical schema validation, durable/queryable proposal lifecycle including READER, and real installed-copy tamper test. G1-G44 all PASS with current code/tests/evidence. Candidate-only indexes and DOT/JSON graphs generated under /opt/dfl-knowledge/evidence/wru-final-verification-2026-07-31/, marked SCOPE=candidate, AUTHORITY=non-canonical, PROMOTION_STATUS=pending-merge. Final verdict: WRU_BUILD_VERIFIED_READY_FOR_MERGE_PLANNING. Residual debt is non-blocking: no formal CLI, tests excluded from runtime manifest by documented rule, installation via isolated copy.

### JPI Fase 5 Real E2E Completion Summary
**Type:** decision  
**Project:** dfl-knowledge  

**What**: JPI Fase 5 E2E vertical successfully reconstructed using ONLY real runtime APIs; all 6 tests pass; RESERVA/OPERACION clarified as state transitions, not entities

**Why**: Prior implementation bypassed 3 critical APIs (reviewOperationalFactoryRequest, useOperationalFactoryArtifact, closeGoal) and wrote directly to DB; mission required using real APIs exclusively and clarifying domain model

**Where**: 
- Implementation: business-os/fmd/jpi-fase5.js (184 insertions, 24 deletions)
- Tests: business-os/tests/fmd-jpi-fase5-e2e.test.js (all 6 PASS)
- Base commit: 8155a4f on feat/jpi-fase-5-real-runtime-v0.1
- Prior base: 51a1ef177d795f3babbb67825e8e0f0e9da60886

**Learned**:
1. closeGoal() has strict preconditions; all must be satisfied before transition:
   - quote_request.status = 'qualified' (requires qualifyByComercial call)
   - operaciones assignment + operations_validation=PASS decision
   - all deviations resolved
   - factory_bridge_records review_status='approved' AND operations_status='used'

2. Real domain entities:
   - quote_requests (1:1 goal, RECEIVED→QUALIFIED→CLOSED flow)
   - jpi_solicitudes (synthetic pilot, RECIBIDA→REQUIERE_INFORMACION→CALIFICADA→CERRADA)
   - jpi_cotizaciones (synthetic pilot, BORRADOR→EMITIDA→ACEPTADA)
   - factory_requests + factory_request_bridges (artifact lifecycle)
   - goal_deviations (tracked, can be auto-resolved by operations validation)
   - goal_closures (final evidence, requires closure_data structure)
   - organizational_events (audit trail, acts as guard against terminal-state violations)

3. RESERVA and OPERACION are NOT real entities:
   - RESERVA maps to jpi_cotizaciones.estado='ACEPTADA' transition
   - OPERACION maps to factory artifact ready→reviewed→used→goal_closed sequence
   - Both emerge from coordinating existing entities via state transitions

4. Sequence of real APIs (happy path):
   - createRuntimeGoal(payload.quote_request) → creates goal + quote_request(received)
   - assignLittleBoss(comercial) + qualifyByComercial() → quote_request(qualified)
   - assignLittleBoss(operaciones) → pre-requisite for close
   - createOperationalFactoryRequest() → factory dispatch
   - pollOperationalFactoryRequest() → await ready
   - reviewOperationalFactoryRequest(approve=true) → review_status=approved
   - useOperationalFactoryArtifact() → operations_status=used + runs validations
   - closeGoal(closureData) → validates preconditions, records closure, transitions goal_runtime_state→closed

5. Factory bridge test failures (7 preexisting):
   - NOT in scope; they test edge cases (timeouts, retries, rejection+re-approval)
   - Happy path works; edge paths were already failing before JPI Fase 5 work
   - Can be addressed separately in FACTORY_BRIDGE_RESILIENCE_BACKLOG

**Outcome**: READY_FOR_FINAL_REVIEW
- Happy path proven real E2E via all APIs
- All E2E tests pass (6/6)
- No manual DB writes
- No bypasses
- Domain model clarified
- Preconditions documented and validated

### JPI Phase 4 Post-Merge Closure Final — PASS / PHASE_CLOSED
**Type:** decision  
**Project:** dfl-knowledge  

**What**: Independent post-merge verification of JPI Phase 4 completed. Verdict: PASS / PHASE_CLOSED. All institutional closure criteria verified end-to-end.

**Why**: Final checkpoint before operational deployment. Requires verification that merge was clean, all tests pass from post-merge main, E2E executes real, and all guarantees are preserved.

**Where**:
- SFV5 origin/main: 9b18947fab2c0874caba729fdb464025dfdde8f0 (tag: sfv5-bos-fmd-automation-v0.1)
- JPI origin/main: 2a1efe243e564a30e273b9ccf0e7077032e65d33 (tag: jpi-phase-4-closed)
- Evidence: /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-4/08-INDEPENDENT-POST-MERGE-REVIEW-CC.md

**Learned**:

Critical verifications completed:
- Both SHAs verified via git ls-remote and clean clone
- Tags published and dereferenced to correct commits
- SFV5 bridge tests: 8/8 PASS (post-merge main, clean checkout)
- JPI regression: 247/247 PASS (post-merge main, clean checkout)
- E2E from empty job root with post-merge mains: 8/8 PASS
- producer_sha exact: 9b18947fab2c0874caba729fdb464025dfdde8f0
- mission_fingerprint consistent across 5 outputs
- All state transitions verified (REQUESTED→DISPATCHED→RUNNING→READY→VALIDATED→CONSUMED→CLOSED)
- Artifact freshness: all +60ms after Mission Packet
- Validation enforced before consumption
- Closure enforced after consumption confirmed
- No mocks, no fallback, no manual relay
- Factory root fail-closed validation
- Merge scope clean (no foreign changes)
- Idempotency verified

No blockers. No residual risks. Ready for operational deployment.

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

---

## PENDING

---

## RECENT ACTIVITY (cross-project)

### ag_topologo v0.1 — LLM mode implementado y operativo 2026-06-27
**Type:** decision  
**Project:** dfl  

OBS_ID: DFL-OBS-20260627-001
TIPO: decision
PROYECTO: dfl
PLATFORM: vm2
SUBSISTEMA: graphify/ag_topologo
PRECEDENCIA: D
AUTHORITY: operational
LIFECYCLE: active
CONFIDENCE: high
LAST_VERIFIED: 2026-06-27
SOURCE: session
SOURCE_REF: MPGE_2026-06-27

## Qué
ag_topologo.py en /opt/dfl-knowledge/scripts/ ahora soporta --llm, --target-concepts N, --max-llm-calls N.

## Implementación
- LLMExtractor class: llama endpoint OpenAI-compatible via urllib (stdlib), budget tracking
- Env vars: AG_TOPOLOGO_LLM_PROVIDER, AG_TOPOLOGO_LLM_ENDPOINT, AG_TOPOLOGO_LLM_MODEL, AG_TOPOLOGO_LLM_API_KEY
- Secretos en /etc/dfl-secrets (permisos 600). Cargar con: set -a && source /etc/dfl-secrets && set +a
- ag_topologo ahora escribe graph_context_light.json (además de graph_context.json) — formato que lee /go endpoint

## Resultado del primer run --llm
- Modelo: gpt-4o-mini (OpenAI)
- 3261 nodos, 7804 aristas, 12 comunidades, 80 LLM calls (budget agotado)
- God nodes: FutbolWeb, estado, IAIM, MERCADER, Función
- /go endpoint: top_nodes=['FutbolWeb','estado','IAIM'], key_surprise='IAIM ↔ Apps Factory'

## Comando de ejecución
set -a && source /etc/dfl-secrets && set +a && python3 /opt/dfl-knowledge/scripts/ag_topologo.py --full --llm --source /opt/dfl-knowledge --out /opt/dfl-knowledge/graphify-out --target-concepts 140 --max-llm-calls 80

## Nota de compatibilidad
ag_topologo escribe schema "agTopologo-DFL-v0.1". gen_summary.py (usa graphify API) no puede leer este formato. Para futuros runs con ag_topologo --llm, el GRAPH_SUMMARY.md lo genera ag_topologo directamente — no necesitar gen_summary.py.

### AgMaster_amOS_3 — vocabulario y reglas IAIM
**Type:** fact  
**Project:** dfl  

AgMaster_amOS_3 es el documento maestro v3 del ecosistema (USAR ESTA, versiones 1 y 2 obsoletas). Vocabulario mínimo para IA invitada: amOS=sistema operativo conceptual/metodológico para absorber/metabolizar información manteniendo soberanía; IAIM=Invisible Augmented Intelligence Mesh, red invisible de IAs aliadas sin nodos fijos, orquestada por HI; HI=Jorge, decisión final soberana; ag10=ChatGPT como router/integrador/destilador/última yarda (no oráculo ni jefe); agPregunta=pregunta aumentada con propósito/dominio/límite/criterio; agLego=pieza conceptual candidata modular trazable; Candado Soberano=restricciones no negociables: no-exec, candidate_only, human-review-first. Prefijo 'ag'=augmented+governed+generative-but-contained. AG10-AUSTERITY-LOCK para fases de cierre/patch/gate. Origin Chain obligatorio para todo agLego. Frase núcleo v3: 'La red ilumina. La HI orquesta. ag10 destila. El candado audita. amOS asimila la cicatriz, no la herida.' Paralelo permitido: máximo 2 nodos, candidate_only, revisión HI.

### Session summary: futbolweb-app
**Type:** session_summary  
**Project:** futbolweb-app  

## Goal
Completar el Diagnóstico Institucional DFL v1 (misión retomada tras corte de conexión): auditoría solo-lectura de La Garra con separación evidencia/inventario/interpretación/diagnóstico y clasificación explícita de certeza. Cierre con estado de la casa y orden institucional, NO propuesta tecnológica.

## Accomplished
- Verificado estado persistido tras el corte: 17 archivos de evidencia en EVIDENCE/ sobrevivieron; ningún artefacto de análisis existía; Engram sin registro de la misión (murió antes del Gate 4B).
- Cerrados 2 huecos de evidencia (solo lectura): git-remotes-redactado.txt (PAT detectado por prefijo, valor jamás registrado) y git-dirty-detalle.txt.
- Escritos los 5 artefactos en /opt/dfl-knowledge/audits/diagnostico-institucional-dfl-v1/: 00-README (método + taxonomía [V]/[I]/[NV]/[D]), 01-INVENTARIO-INFRAESTRUCTURA, 02-INTERPRETACION, 03-HALLAZGOS (18 hallazgos: 1 crítico, 6 altos, 5 medios, 6 bajos), 04-DIAGNOSTICO-INSTITUCIONAL (orden: ACLARAR → VERIFICAR → CONSOLIDAR → GOBERNAR → RETIRAR).

## Discoveries (clave)
- H-01 CRÍTICO: PAT GitHub en remote de prediccion2026 (obs #218) — registrado sin rotar por mandato.
- H-02 ALTO: backups off-host de Engram NO verificables desde La Garra (ssh denegado + receptor rechaza inspección) — durabilidad es hipótesis, no hecho.
- H-03 ALTO: engram-backup-offhost.sh y engram-sync-cron.sh corren desde working tree sin commit — crons ejecutan código que git no declara.
- H-04 ALTO: producto público (360eventos demo a Rubén, futbolweb) servido por next dev servers NODE_ENV=development en puertos públicos; explica swap 1.4Gi/2.0Gi.
- H-05 ALTO: saas-factory-setup V5 (5e42124) local con remote apuntando a org externa saas-factory-community — push reflejo filtraría IP de fábrica.
- H-06/H-07: n8n público dormido desde 2026-05-17; futbolweb-env-backup.zip en Drive sin verificar.
- Organismo intacto: cero correcciones, reinicios, limpiezas o actualizaciones.

## Next Steps
- Bloques 1º-2º del diagnóstico antes de cualquier evolución: aclarar alcance del PAT, registro vivo/muerto de órganos /opt, rol n8n, dry-run metabolismo, destino V5; verificar backups desde VM3, auth n8n/8080, vigencia PAT.
- Commitear el expediente de auditoría en dfl-knowledge (C-2) cuando Jorge autorice.

## Relevant Files
- /opt/dfl-knowledge/audits/diagnostico-institucional-dfl-v1/{00-README,01-INVENTARIO-INFRAESTRUCTURA,02-INTERPRETACION,03-HALLAZGOS,04-DIAGNOSTICO-INSTITUCIONAL}.md
- EVIDENCE/ (19 archivos, incl. git-remotes-redactado.txt y git-dirty-detalle.txt nuevos)

### SESSION CLOSURE: WRU v0.1 adopción institucional y relay para reanudación
**Type:** decision  
**Project:** dfl  

CIERRE DE SESIÓN — WRU v0.1. No se abrió investigación nueva ni se modificó código, merge, dirty state, índices o grafos.

ESTADO TÉCNICO: WRU v0.1 mergeado en /opt/saas-factory-setup; merge bec766e; HEAD final reportado bc8d6b89e7e4db1ba4b83da8055579c56bc89759; tag wru-v0.1-institutional-final2. Módulo canónico reportado en saas-factory/tools/workforce-registry; interfaces wru.query.v1, wru-refresh.v1 y wru-status.v1. Integrity 23/23; suite WRU 83/83 PASS; G1-G44 reportados PASS.

ESTADO INSTITUCIONAL: Adoption Gate 9/9 PASS; consumidor externo DFL Concierge en /opt/dfl-knowledge-workunit; consumer HEAD reportado 5f01fb29a2bfd8e8422524f616108a8996317488; suite Concierge 258/258 PASS. Camino vivo probado: Concierge solicita capability por wru.query.v1 READER-only → WRU consulta Canonical State → devuelve availability/freshness/provenance → Concierge selecciona sfv5-skill.acquisition → deriva WorkUnit IDs → WorkUnitLedger claim/release → receipt externo.

RECEIPT: /opt/dfl-knowledge/evidence/wru-live-concierge-20260731-r3/wru-receipt.json; digest reportado e5da6697cfedd9a0e0062ab81978e57f79d0488336283d16fd8801669a0c4932; active_claims=[]; resultado WORK_UNIT_CLAIMED_AND_RELEASED. Evidencia principal reportada: /opt/dfl-knowledge/evidence/wru-institutional-adoption-2026-07-31/ADOPTION-GATE.md. El usuario indicó /opt/saas-factory-setup/evidence/wru-institutional-adoption-2026-07-31/ADOPTION-GATE.md; conservar ambas referencias en el relay sin investigar ahora.

FRESHNESS: la respuesta viva fue DEGRADED_STALE porque los records canónicos conservan source_commit histórico distinto del source HEAD observado; el consumidor no lo trató como autoridad plena y registró READER_DEGRADED con provenance/availability explícitos.

DISTINCIÓN OBLIGATORIA: WRU CAMINO_VIVO_INSTITUCIONAL_PROBADO: YES. Capacidades catalogadas: 0/32 PROBADO_EN_CAMINO_VIVO.

DIRTY STATE: el árbol compartido tenía dirty state preexistente ajeno; no se investigó ni limpió en este cierre. Queda pendiente de aclaración factual futura y no debe asumirse como fallo de WRU sin evidencia.

ÍNDICES/GRAFOS: se generaron índices/grafos candidatos y luego se registró un estado canónico post-merge. No afirmar más de lo demostrado. Queda pendiente para una sesión futura verificar rutas exactas, conteos finales canónicos, motor que los sirve, integración real con Codebase Memory/Visualizer/Graphify/agTopólogo, frescura, mecanismo de actualización y promoción al grafo institucional mayor. JSON/DOT en evidence no bastan: debe probarse que forman parte del sistema gráfico vivo y no de otro closet documental.

PRINCIPIO INSTITUCIONAL: todo elemento nuevo DFL debe generar índices, nodos, relaciones, procedencia y frescura como parte de la cultura de fabricación, para que un invento exitoso e integrado sea conocido por el sistema institucional y pueda incorporarse al grafo mayor, sus nodos, relaciones y ramas.

DEUDA RESIDUAL REAL: freshness canónica todavía degradada hasta reconciliación gobernada; verificación de integración con el grafo institucional mayor pendiente; dirty state preexistente pendiente de aclaración factual. Deuda hipotética no se presenta como real.

REANUDACIÓN EXACTA: validar primero la ubicación efectiva de ADOPTION-GATE.md y luego comprobar el registro canónico vivo de índices/grafos y su mecanismo de servicio/actualización, sin reabrir la fabricación WRU. Próxima sesión sugerida: WRU-GRAPH-LIVE-PROMOTION-2026-08-01.

### @$go 2026-07-31 — pending histórico de limpieza reconciliado
**Type:** fact  
**Project:** dfl  

@$go ejecutado con payload local generated_at=2026-07-31T21:23:27Z y search_memory('contexto DFL'). Primer pending: observación #252 sobre Reminder 1a/1Password.txt del 2026-07-14. La búsqueda en Engram confirmó que los dos frentes ya estaban cerrados y verificados, pero la observación seguía STATUS active. Se actualizó #252 a [RESOLVED], STATUS resolved y LIFECYCLE: archived para eliminar el pending histórico. No se modificaron archivos ni superficies protegidas; no se hizo ninguna operación sobre Drive.

---

## CC BOOTSTRAP (Claude Code session startup)

- **Step 1:** mem_search('contexto DFL') — Engram MCP activo, consultar antes de operar
- **Step 2:** PROXIMO_AGENTE_DEBE en cada recent_decision = instrucción de acción inmediata
- **Step 3:** Al guardar obs: usar DFL Writing Convention v0.3 (topic: dfl/engram/writing-convention)
- **Precedencia:** A > B > C > D > E — Engram es capa D, nunca invalida Blueprint (A) ni MASTER_INDEX (B)
- **Protegido:** NO modificar puntajeTigreKnockout, Supabase, Vercel config sin PRP explícito

---

## KNL POLICY

**Precedence:** A > B > C > D > E; KNL and Engram are operational layer D.  

**Required agent behavior:**
- Graph preflight MANDATORY: consult knl.navigation.god_nodes before any find/grep/ls/manual filesystem exploration.
- agTopologo is the sole producer of graph.json; Graphify is a consumer/analyzer. Pipeline: agTopologo → graph.json → Graphify. Not cross-redundant — producer/consumer.
- Local /go fallback available: cc-atgo-hook.sh has three levels (remote → KNL local → hardcoded). No reimplementar.

**Restrictions:**
- No tocar puntajeTigreKnockout sin PRP explícito.
- No tocar Supabase, Vercel config ni variables de entorno sin autorización explícita.
- No modificar Engram salvo integración/lectura operacional.
- No introducir dependencias innecesarias.
- DFL debe seguir funcionando sin Graphify/graph.json.

---

## KNL NAVIGATION — GOD NODES

> Consult navigation.god_nodes before manual filesystem exploration.

### FutbolWeb
**Root:** `/opt/futbolweb`  
**Restriction:** No tocar puntajeTigreKnockout sin PRP explícito.  
**Restriction:** No tocar Supabase/Vercel/env sin autorización explícita.  

**Key files:**
- `/opt/futbolweb/lib/espn-world-cup.ts` — ESPN reality sync and official result capture.
- `/opt/futbolweb/lib/scoring-propagation.ts` — Dispatches pending results to group or knockout scorer.
- `/opt/futbolweb/lib/tournament-reality.ts` — Reads match_results and result row shape.
- `/opt/futbolweb/lib/puntaje-tigre-knockout.ts` — Protected knockout scorer; inspect/tests only. ⚠️ `no tocar`

**Entrypoints:**
- `cd /opt/futbolweb && npm test -- lib/espn-world-cup.test.ts lib/scoring-propagation.test.ts lib/puntaje-tigre-knockout.harness.test.ts` — Focused scoring pipeline evidence.

### context-proxy
**Root:** `/opt/dfl-context-proxy`  
**Restriction:** No mostrar secretos.  
**Restriction:** No modificar graph.json desde KNL.  

**Key files:**
- `/opt/dfl-context-proxy/main.py` — Serves /go, /go?deep=1, /context/dfl.
- `/opt/dfl-knowledge/graphify-out/knl.json` — Official KNL contract.
- `/opt/dfl-knowledge/graphify-out/graph_context_light.json` — Compatibility alias for legacy consumers.

**Entrypoints:**
- `systemctl restart dfl-context-proxy` — Apply proxy changes.

### IAIM
**Root:** `/opt/dfl-knowledge`  
**Restriction:** Prefer KNL navigation before manual search.  
**Restriction:** No LLM-costly runs without explicit need.  

**Key files:**
- `/opt/dfl-knowledge/scripts/knl_builder.py` — Builds KNL v1.0.
- `/opt/dfl-knowledge/scripts/knl_compare.py` — Compares graph snapshots; generates comparator report.
- `/opt/dfl-knowledge/scripts/ag_topologo.py` — Sole producer of graph.json (agTopologo-DFL-v0.1 schema).
- `/opt/dfl-knowledge/DFL_Agent_Onboarding_Config.md` — Agent onboarding contract.

**Entrypoints:**
- `GET /go` — Agent bootstrap.
- `Consult knl.navigation before find/grep/ls` — Avoid blind exploration.

---

## KNL SEMANTIC COMMUNITIES

**Graph entropy:** 1.0588  

- **Community 11** (93 nodes): MCP Server Behavior, Evaluación de Plantillas, Preguntas para el Desarrollador
- **Community 0** (7 nodes): Capacidades Comunes Reutilizables, Reconciliación F1A
- **Community 1** (4 nodes): Consistencia de Iteraciones, Evaluación de Repositorios, Veredicto Modular
- **Community 2** (4 nodes): agLego-PTE-001
- **Community 3** (4 nodes): Smithy
- **Community 4** (4 nodes): mission-control, release preparation

---

*Mirror auto-generated 2026-07-31T21:45:32Z | La Garra → DFLghub/amos-context*
