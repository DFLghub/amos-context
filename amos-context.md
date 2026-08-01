# amOS Context — @$go Live Mirror
**Generated:** 2026-08-01T03:05:02Z  
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

### DFL New Element Institutionalization System v0.1 operational
**Type:** decision  
**Project:** dfl  

VEREDICTO: DFL_NEW_ELEMENT_INSTITUTIONALIZATION_SYSTEM_OPERATIONAL

Misión ejecutada en /opt/dfl-knowledge, branch feat/dfl-high-certainty-harness-v0.1. Sistema entregado: versioned schema dfl.new-element.v0.1; executable DFL_NEW_ELEMENT_ADOPTION_GATE; DFL_ANTI_CLOSET_GATE; recurrent refresh connected to scripts/daily_check.sh; live graph source architecture/institutional-graph/NEW-ELEMENTS-LIVE.md; existing agTopologo -> graphify-out -> KNL/context proxy reused; PRP Golden Path overlay in /opt/saas-factory-setup/saas-factory/.claude/PRPs/prp-base.md.

Commits: 47d96d2 add executable new element institutionalization gates; 5d9ccfb product PRP overlay; final verification commits 26d5913 and 3c48f55. Tag: dfl-new-element-institutionalization-v0.1 at 47d96d2. Final DFL knowledge HEAD: 3c48f558940d10574c9ba01521124f2493fce549.

Evidence: /opt/dfl-knowledge/evidence/dfl-new-element-institutionalization-v0.1-2026-07-31/ with FINAL-REPORT.md, CURRENT-STATE.json, PILOT-RESULTS.json, CHECKPOINTS.jsonl, COMMANDS.jsonl, DECISIONS.md, BLOCKERS.md, ARTIFACT-MANIFEST.json, SHA256SUMS. Hash validation PASS. New system tests 9/9 PASS; DFL health 14/14 PASS; WRU 83/83 PASS; Concierge 258/258 PASS; graph regenerated with 140 semantic nodes, 560 edges, 10285 structural nodes; live queries WRU and Business OS FRESH/CANONICAL.

Pilots: WRU real manifest PASS/ACTIVE, external DFL Concierge consumer and live path DFL Concierge -> wru.query.v1 -> WRU Canonical State -> WorkUnitLedger, receipt and freshness present. Preserve distinction: WRU institutional live path proven YES; 32 catalogued capabilities proven in live path 0/32. Real adversarial /opt/experiments/business-os-new-audit is FAIL/IN_CLOSET with 12 missing invariants, including rollback proof; this is intentional detection evidence, not a closure failure.

Fix: gate.py now rejects blank proof paths, so rollback evidence cannot pass by resolving the repository root. Graph freshness was reconciled to repository HEAD 5d9ccfb (artifact WRU commit remains bc8d6b); consumer HEAD 5f01fb.

Residual debt: automatic manifest creation/lint at PRP creation time is not yet wired; protected 03:05 UTC cron was untouched. Pre-existing untracked dirty state remains untouched and must not be treated as a system failure without factual attribution. NO_TOUCH surfaces untouched.

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

### Engram Cloud — instalación en DFL (2026-06-24)
**Type:** decision  
**Project:** dfl  

Engram Cloud instalado en La Garra el 2026-06-24. Stack: engram-prod-cloud (Go) + engram-prod-postgres (PostgreSQL 16) via docker-compose.prod.yml en /opt/engram. Puerto interno: 8090 (Caddy reverse proxy). URL pública: https://engram.deepfeelingslabs.com. Tokens en /opt/engram/.env. Binario cliente en /usr/local/bin/engram (copiado del container). MCP configurado para Claude Code en ~/.claude/mcp/engram.json (herramientas: agent profile). Proyectos enrolados: futbolweb, dfl. Propósito: memoria institucional persistente del ecosistema DFL — sobrevive cambios de terminal, modelo y proveedor.

### Rotación ENGRAM_CLOUD_TOKEN + migración secretos crontab → /etc/dfl-secrets
**Type:** bugfix  
**Project:** dfl  

TOPIC: crontab-secrets-migration
TYPE: security-fix (DFL Writing Convention v0.3)
STATUS: completed
DATE: 2026-07-02
PLATFORM: vm2 (La Garra)

**What**: Token ENGRAM_CLOUD_TOKEN comprometido (expuesto en output de crontab en sesión de chat) rotado y todos los secretos inline del crontab migrados al patrón canónico /etc/dfl-secrets.

**Why**: El crontab contenía ENGRAM_CLOUD_TOKEN hardcodeado en dos entradas de engram sync (*/5). El token quedó visible en transcript de sesión. Compromiso confirmado → rotación obligatoria.

**Where**:
- `/opt/engram/.env` — ENGRAM_CLOUD_TOKEN actualizado con nuevo token (openssl rand -hex 32)
- `/etc/dfl-secrets` — añadidos: ENGRAM_CLOUD_TOKEN, ENGRAM_CLOUD_SERVER, ENGRAM_DATA_DIR (chmod 600)
- `/opt/dfl-context-proxy/engram-sync-cron.sh` — wrapper nuevo: source /etc/dfl-secrets + engram sync dfl + engram sync futbolweb
- `crontab` — dos entradas inline reemplazadas por una sola: `*/5 * * * * /opt/dfl-context-proxy/engram-sync-cron.sh`
- `/opt/engram/docker-compose.prod.yml` — contenedor recreado con nuevo token (docker-compose v1 bug: stop+rm manual, luego up)

**Learned**:
- docker-compose v1.29.2 tiene bug 'ContainerConfig' al recrear contenedores con Docker engine moderno → workaround: docker stop + docker rm manual, luego docker-compose up
- Engram Cloud auth endpoint: GET /sync/pull (no /api/... ni /v1/...)
- Token viejo confirmado rechazado HTTP 401; token nuevo HTTP 200 en GET /sync/pull?project=dfl
- sync manual exitoso: "Nothing new to sync" para dfl y futbolweb
- /root/.engram/cloud.json tiene token vacío — el token de sync viene exclusivamente de ENGRAM_CLOUD_TOKEN env var
- Alerta secundaria detectada: AG_TOPOLOGO_LLM_API_KEY (OpenAI) también quedó expuesta en la misma sesión al hacer cat /etc/dfl-secrets — requiere rotación independiente

### Engram Cloud instalado en La Garra
**Type:** decision  
**Project:** futbolweb-app  

Engram Cloud v1.17.0 desplegado en /opt/engram via docker-compose.prod.yml. Backend Go + Postgres en puertos internos 8090/5434. Caddy reverse proxy en engram.deepfeelingslabs.com con TLS automático. Token de acceso en /opt/engram/.env.

### DFL New Element Institutionalization System v0.1 operational
**Type:** decision  
**Project:** dfl  

VEREDICTO: DFL_NEW_ELEMENT_INSTITUTIONALIZATION_SYSTEM_OPERATIONAL

Misión ejecutada en /opt/dfl-knowledge, branch feat/dfl-high-certainty-harness-v0.1. Sistema entregado: versioned schema dfl.new-element.v0.1; executable DFL_NEW_ELEMENT_ADOPTION_GATE; DFL_ANTI_CLOSET_GATE; recurrent refresh connected to scripts/daily_check.sh; live graph source architecture/institutional-graph/NEW-ELEMENTS-LIVE.md; existing agTopologo -> graphify-out -> KNL/context proxy reused; PRP Golden Path overlay in /opt/saas-factory-setup/saas-factory/.claude/PRPs/prp-base.md.

Commits: 47d96d2 add executable new element institutionalization gates; 5d9ccfb product PRP overlay; final verification commits 26d5913 and 3c48f55. Tag: dfl-new-element-institutionalization-v0.1 at 47d96d2. Final DFL knowledge HEAD: 3c48f558940d10574c9ba01521124f2493fce549.

Evidence: /opt/dfl-knowledge/evidence/dfl-new-element-institutionalization-v0.1-2026-07-31/ with FINAL-REPORT.md, CURRENT-STATE.json, PILOT-RESULTS.json, CHECKPOINTS.jsonl, COMMANDS.jsonl, DECISIONS.md, BLOCKERS.md, ARTIFACT-MANIFEST.json, SHA256SUMS. Hash validation PASS. New system tests 9/9 PASS; DFL health 14/14 PASS; WRU 83/83 PASS; Concierge 258/258 PASS; graph regenerated with 140 semantic nodes, 560 edges, 10285 structural nodes; live queries WRU and Business OS FRESH/CANONICAL.

Pilots: WRU real manifest PASS/ACTIVE, external DFL Concierge consumer and live path DFL Concierge -> wru.query.v1 -> WRU Canonical State -> WorkUnitLedger, receipt and freshness present. Preserve distinction: WRU institutional live path proven YES; 32 catalogued capabilities proven in live path 0/32. Real adversarial /opt/experiments/business-os-new-audit is FAIL/IN_CLOSET with 12 missing invariants, including rollback proof; this is intentional detection evidence, not a closure failure.

Fix: gate.py now rejects blank proof paths, so rollback evidence cannot pass by resolving the repository root. Graph freshness was reconciled to repository HEAD 5d9ccfb (artifact WRU commit remains bc8d6b); consumer HEAD 5f01fb.

Residual debt: automatic manifest creation/lint at PRP creation time is not yet wired; protected 03:05 UTC cron was untouched. Pre-existing untracked dirty state remains untouched and must not be treated as a system failure without factual attribution. NO_TOUCH surfaces untouched.

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

### WRU
**Root:** `/opt/saas-factory-setup`  
**Restriction:** Do not treat evidence JSON/DOT as graph authority.  
**Restriction:** Stale graph authority must be degraded.  
**Restriction:** Do not claim 32 skills are live-tested.  

**Key files:**
- `/opt/dfl-knowledge/architecture/institutional-graph/WRU-LIVE-NODE.md` — Canonical institutional graph source for WRU identity, provenance and cross-repo relations.
- `/opt/saas-factory-setup/saas-factory/tools/workforce-registry` — Installed WRU module and stable interfaces.
- `/opt/dfl-knowledge-workunit/concierge/wru_consumer.py` — External Concierge consumer contract.
- `/opt/dfl-knowledge-workunit/evidence/wru-live-concierge-20260731-r3/wru-receipt.json` — Live institutional usage receipt.
- `/opt/dfl-knowledge/scripts/query_institutional_graph.py` — Read-only query interface over the live graph.
- `/opt/dfl-knowledge/scripts/wru_graph_refresh.py` — Detects source HEAD drift before the existing graph refresh pipeline.

**Entrypoints:**
- `python3 /opt/dfl-knowledge/scripts/query_institutional_graph.py --term "DFL Concierge"` — Discover the external consumer and its relations.
- `bash /opt/dfl-knowledge/scripts/regen_graph.sh` — Existing circuit-breaker-protected graph refresh, when the scheduled daily check detects source changes.

---

## KNL SEMANTIC COMMUNITIES

**Graph entropy:** 0.8246  

- **Community 11** (95 nodes): MCP Server Behavior, Evaluación de Plantillas, Preguntas para el Desarrollador
- **Community 0** (5 nodes): Cierre de Goal, Componentes Heredables
- **Community 1** (4 nodes): Certainty Gate
- **Community 2** (4 nodes): DFL_Agent_Onboarding_Config, ProductController
- **Community 3** (4 nodes): Módulos Bicep, Interfaz HTTP para Gestión de Objetivos
- **Community 4** (4 nodes): RON, Versioning and lifecycle

---

*Mirror auto-generated 2026-08-01T03:05:02Z | La Garra → DFLghub/amos-context*
