# amOS Context — @$go Live Mirror
**Generated:** 2026-07-31T01:23:15Z  
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

### CX-R2 SFV5 independently verified — supersedes operational handoff of #393
**Type:** decision  
**Project:** dfl  

SESSION CLOSURE: CX-R2 — 2026-07-31

**Result**: SFV5_AUDIT_INDEPENDENTLY_VERIFIED.

**Inputs**: CC-R2 `0bfc5c93bdcd4968d7c655204c32df234a0e7dce`; CX-R1 `1f84021300f71a4e60a990f7cea43c9f017e9f12`.

**Evidence**: CX-R2 commit `60316d9124f2375aebce848b2893bf4e525d7ef9`, root `/opt/dfl-knowledge/evidence/sfv5-forensic-inspection-2026-07-30-cx-r2/`.

**Independent verdict**: SELF_REFERENCE ABSENT; CIRCULAR_DEPENDENCY ABSENT; CONTENT_FILES_DECLARED 11; CONTENT_FILES_HASHED 11; UNDECLARED_FILES 0; MISSING_FILES 0; CLEAN_VERIFY PASS; TAMPER_TEST PASS; MANIFEST_COMPLETE YES. Scanner is honestly PARTIAL: ADDED NOT_DETECTED, REMOVED PASS, MODIFIED PASS. Gates: 17 PASS, 1 PARTIAL (G13), 0 FAIL, 0 NOT_PROVEN. No positive 20/20 PASS claim.

**Handoff correction**: Historical obs #393 is preserved as the CX-R1 result, but this observation supersedes it operationally for current closure. Mirror `344c5d982e11f4b9d037f9c8aed417edc9750a86` propagated the intermediate stale handoff `Return to CC-R2` after CC-R2 had concluded; history was not rewritten. Next handoff: CC-2 may start.

**Scope**: No product, graph, SFV5 source, Engram history, or protected surface was modified.

### [INSTITUTIONAL_DECISION] Preservación no Equivale a Integración
**Type:** decision  
**Project:** dfl  

**What**: Regla institucional transversal. **Ninguna de estas acciones promueve el estado de una capacidad**: guardar un archivo en el repo, commitearlo, registrar una observación en Engram, pasar tests aislados, escribir un documento de diseño, o publicar el mirror. Preservar es dejar constancia; integrar es cambiar el comportamiento del sistema vivo y demostrarlo.

**Why**: Es la generalización del "falso integrado" ya identificado en Skill Engineering (declarar una skill integrada por copiar sus archivos, omitiendo el registro real de descubrimiento). El mismo error apareció en Concierge F1B: 34/34 PASS y un artefacto preservado, con el bridge sin cablear al camino vivo. Sin esta regla, la evidencia de preservación se confunde con evidencia de funcionamiento y produce un falso positivo indistinguible del real.

**Where**: Caso testigo verificado directamente en La Garra el 2026-07-28:
- `git diff --stat main..0701a52 -- main.py` → vacío (el candidato no toca el entrypoint)
- `grep -n "concierge" /opt/dfl-context-proxy/main.py` → 0 coincidencias (el proxy vivo no conoce el bridge)
- `ls /opt/dfl-artifacts` → no existe (la raíz soberana "decidida" nunca se materializó)
- Snapshot: `/opt/dfl-knowledge/audits/analisis-longitudinal-2026-07-28.md` — SHA256 `202bcc49f6fe3e7496478c9888719b3d9503171822bb7c40f64f04e024d3f941`
- Addendum: `/opt/dfl-knowledge/audits/analisis-longitudinal-2026-07-28-addendum-f1b-review.md`
- Commit: `72ed69b7a2a870d11bfbbb71f218fe6edca34a63`
- Revisión independiente: CX, 2026-07-28, candidato `0701a52`, veredicto `REQUIRES_CHANGES`

**Learned**: El test decisivo de integración es siempre el mismo — **¿cambia el comportamiento observable del sistema vivo, y hay un log que lo pruebe?** Si la respuesta necesita un "debería", no está integrado. Aplica a skills (¿el orquestador la descubre sin que le pasen la ruta?), a bridges (¿una request real lo atraviesa?) y a rollbacks (¿se ejecutó alguna vez?).

**PROXIMO_AGENTE_DEBE**: al reportar estado, separar siempre "preservado" de "integrado" en dos líneas distintas. Ver [[dfl/institutional/f1b-gates-evidenciados]] y [[dfl/institutional/snapshot-longitudinal-canonico]].

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

### Auditoría Engram 2026-07-08 — sin limpieza programada y sync parcial por proyectos
**Project:** dfl  

**Qué se revisó**: Estado operativo de Engram local en La Garra tras la implementación de @$go VALIDATION GATE.

**Hallazgos principales**:
- Engram local sano: `/health` OK, DB `/root/.engram/engram.db` ~2.9MB.
- Volumen actual: 177 observations, 307 user_prompts, 54 sessions, 31 memory_relations.
- Distribución observations: dfl 104, futbolweb-app 53, 360eventos 16, tdf-01 4.
- No hay relaciones pendientes: `memory_relations.judgment_status='pending'` = 0.
- Hay backup off-host cada 6h y sync cron cada 5 min.
- No se encontró limpieza/depuración semántica programada.
- `engram-sync-cron.sh` sincroniza solo proyectos `dfl` y `futbolweb`; quedan mutaciones sin ACK en proyectos usados realmente: `futbolweb-app` 744, `360eventos` 38, `tdf-01` 7, además de otros namespaces menores.
- Calidad semántica: 92 observations sin `review_after`, 8 títulos vacíos, ~54 observations con señales de cierre/resuelto/snapshot/stale, y varias observaciones recientes sobre onboarding/outboarding solapadas que podrían compactarse.

**Riesgo**: Engram tiene durabilidad, pero no metabolismo: acumula snapshots/cierres/iteraciones sin ciclo formal de compactación, archivado y promoción a canonical facts.

**Recomendación preliminar**: crear `engram-maintenance` semanal o quincenal: audit-only primero, luego compactación supervisada. No borrar por defecto; archivar/compactar/promover. Ajustar sync cron para cubrir proyectos activos reales (`futbolweb-app`, `360eventos`, `tdf-01`) o normalizar nombres de proyecto.

### Limpieza 2026-07-14: Reminder 1a cerrada; 1Password.txt eliminado de Drive por Jorge (verificado)
**Project:** futbolweb-app  

TOPIC: dfl/session/2026-07-14-limpieza-1p-reminder
TYPE: decision
STATUS: active
DATE: 2026-07-14

**What**: Sesión CC de limpieza con dos frentes, ambos CERRADOS. (1) Reminder Layer Phase 1a CERRADA: Copa del Mundo 2026 finalizada, los 5 partidos KO pendientes de Alejo ya se jugaron — obs #110 marcada [RESOLVED] + LIFECYCLE: archived. Verificado que NO existía entrada Reminder_Layer_1a en registro-vivo.json ni en ningún archivo de /opt/dfl-knowledge (el pendiente vivía solo en Engram) — sin edición ni commit porque no había nada que editar. (2) 1Password.txt en Drive (fileId 1g4-4BoWbdQ0JRvggnTTFxwnjjXVASczZ, 204B): ELIMINADO manualmente por Jorge en la UI de Drive el 2026-07-14, tras blocker inicial (el conector MCP de Drive no expone delete). Borrado VERIFICADO por CC: get_file_metadata devuelve "Requested entity was not found". Residual D-5/1Password de la Reconciliación v1 cerrado.
**Why**: Orden directa de Jorge 2026-07-14: eliminar 1Password.txt (noise backup antiguo, ya revisado por él) y cerrar Reminder 1a por fin de torneo.
**Learned**: El conector claude.ai Google Drive es read-mostly (sin delete/trash) — limpiezas destructivas en Drive requieren UI manual o rclone tras OAuth institucional (B-1, aún pendiente). Paridad CC/Codex sigue pendiente de ratificación de Jorge.

---

## RECENT ACTIVITY (cross-project)

### Session summary: futbolweb-app
**Type:** session_summary  
**Project:** futbolweb-app  

## Cierre DFL/KNL/FutbolWeb — 2026-06-27

### Goal
Cerrar carril institucional DFL (@$go, KNL, hooks, context-proxy) y dejar FutbolWeb limpio de dirty files y factory artifacts.

### Accomplished
- Engram #101: payload /go slim — graph_context eliminado, knl canónico único en payload
- cc-atgo-hook.sh: header @go → @$go corregido
- dfl-nav fmt_brief: mensaje no-match → "sin god_node — intenta la raíz del concepto"
- FutbolWeb repo limpio: Blueprint audit movido a /opt/dfl-knowledge/07_Chat_History/FutbolWeb/Auditorias/, graphify-out/ eliminado, .gitignore actualizado, commit 3fd5801
- Engram #102: higiene FutbolWeb documentada
- Bitácora creada: /opt/dfl-knowledge/07_Chat_History/FutbolWeb/Actas/BITACORA_ODA+Standard_2026-06-27_CIERRE_DFL_KNL_FUTBOLWEB.md

### Discoveries
- graph_context era alias redundante del payload /go — eliminado sin romper consumidores
- agProtocol_ATP-D_ROJA_v0.1-1: 3 archivos con MD5 idéntico en corpus (duplicados de indexación)
- "estado" como nombre de god_node produce colisión léxica en español con el grafo
- Blueprint_v0.6 audit era inconclusa (Blueprint no disponible en VM2) — conservada en Auditorias/

### Next Steps
1. FutbolWeb producto — runtime estable, knockout scoring deployado (91a4531)
2. KNL próximo ciclo — nota stale graph_context en knl_builder.py, health test local, evaluar renombrar estado → context-proxy
3. MERCADER — agregar a KNL si se activa como área de trabajo
4. Corpus — eliminar agProtocol duplicados (-1 variants)

### Relevant Files
/opt/dfl-context-proxy/main.py, /opt/dfl-context-proxy/cc-atgo-hook.sh, /usr/local/bin/dfl-nav, /opt/futbolweb/.gitignore, /opt/dfl-knowledge/07_Chat_History/FutbolWeb/Actas/BITACORA_ODA+Standard_2026-06-27_CIERRE_DFL_KNL_FUTBOLWEB.md

### Stack FutbolWeb — runtime activo
**Type:** fact  
**Project:** futbolweb-app  

FutbolWeb corre en /opt/futbolweb en La Garra (DigitalOcean, IP 67.205.166.199). Caddy en 80/443. n8n en 5678. yt-ingest en 8080. Engram Cloud en 8090. Supabase externo para scoring/ranking. No tocar puertos 80/443/3001/5678/8080 sin autorización.

### [CERTIFIED] Roadmap DFL @$go/KNL/hooks — orquestación 2026-06-27
**Type:** decision  
**Project:** dfl  

**What**: Orquestación ejecutiva del stack DFL post-certificación: incidente FW, slim /go, fallback local, handoff bidireccional, modelo de confianza.

**Decisiones:**
- Incidente FW 2026-06-19: STALE. Engram #14 lo cierra (fix aplicado 2026-06-24). El `pending` en /go es ruido — debe limpiarse. Los archivos dirty en /opt/futbolweb (espn-world-cup.ts +175l, scoring-propagation.ts +30l) son trabajo en progreso nuevo, no el incidente.
- Slim /go: servir solo restrictions + god_nodes + pending + recent_decisions(max 4) por defecto. Identity → 2 líneas. graph_summary → ?full=1. -40% payload estimado.
- Fallback local: cc-atgo-hook.sh debe leer /opt/dfl-knowledge/graphify-out/knl.json si /go falla. Banner MODO FALLBACK para agente. Elimina único punto de fallo silencioso.
- Handoff bidireccional: agent-lock.json en /opt/dfl-context-proxy/. Read en SessionStart, write al iniciar trabajo, cleanup en session_end. Sin infraestructura distribuida.
- Modelo confianza: convención [VERIFIED]/[CERTIFIED]/[CLAIMED]/[STALE] en títulos Engram. Cero código. Si restricción es CLAIMED, preguntar a Jorge antes de actuar.

**Orden de ejecución recomendado:**
1. Limpiar pending Engram #14 (5 min)
2. Fallback local cc-atgo-hook.sh (30 min)
3. Slim /go payload main.py (1h)
4. Convención evidence_level en Engram (inmediato, convención)
5. Agent lock file — solo cuando haya colisiones reales documentadas

**Why**: Reducir ruido en bootstrap inter-agente y eliminar puntos de fallo sin tocar producción.

**Where**: cc-atgo-hook.sh, main.py (/go endpoint), Engram project dfl.

**Learned**: Codex demostró que /go ya transfiere suficiente contexto para reconstruir el testigo sin intervención humana. El sistema funciona — necesita afinamiento, no rediseño. Los dirty files de FutbolWeb son trabajo pendiente en la pipeline ESPN/scoring; requieren sesión dedicada con PRP antes de commit.

### CX-R2 SFV5 independently verified — supersedes operational handoff of #393
**Type:** decision  
**Project:** dfl  

SESSION CLOSURE: CX-R2 — 2026-07-31

**Result**: SFV5_AUDIT_INDEPENDENTLY_VERIFIED.

**Inputs**: CC-R2 `0bfc5c93bdcd4968d7c655204c32df234a0e7dce`; CX-R1 `1f84021300f71a4e60a990f7cea43c9f017e9f12`.

**Evidence**: CX-R2 commit `60316d9124f2375aebce848b2893bf4e525d7ef9`, root `/opt/dfl-knowledge/evidence/sfv5-forensic-inspection-2026-07-30-cx-r2/`.

**Independent verdict**: SELF_REFERENCE ABSENT; CIRCULAR_DEPENDENCY ABSENT; CONTENT_FILES_DECLARED 11; CONTENT_FILES_HASHED 11; UNDECLARED_FILES 0; MISSING_FILES 0; CLEAN_VERIFY PASS; TAMPER_TEST PASS; MANIFEST_COMPLETE YES. Scanner is honestly PARTIAL: ADDED NOT_DETECTED, REMOVED PASS, MODIFIED PASS. Gates: 17 PASS, 1 PARTIAL (G13), 0 FAIL, 0 NOT_PROVEN. No positive 20/20 PASS claim.

**Handoff correction**: Historical obs #393 is preserved as the CX-R1 result, but this observation supersedes it operationally for current closure. Mirror `344c5d982e11f4b9d037f9c8aed417edc9750a86` propagated the intermediate stale handoff `Return to CC-R2` after CC-R2 had concluded; history was not rewritten. Next handoff: CC-2 may start.

**Scope**: No product, graph, SFV5 source, Engram history, or protected surface was modified.

### @$go 2026-07-31 — reconciliación final verificada y pending histórico archivado
**Type:** fact  
**Project:** dfl  

@$go ejecutado con payload local generated_at=2026-07-31T01:12:29Z y search_memory('contexto DFL'). Se retomó el primer pending mediante HANDOFF-CODEX.md. `check_registro_vivo.py` confirmó el estado, pero reportó además 30 hallazgos de infraestructura/dirty state fuera del alcance: no se remediaron. 08-RECONCILIACION-FINAL.md confirma que la misión ya estaba completada (commit 7b77b78, obs #236 archivada, mirror e525b9a). Se archivó Engram obs #239 como [RESOLVED] para evitar que el resumen histórico siguiera apareciendo como pending genérico. No se modificaron archivos ni superficies protegidas; no se tocaron decisiones reservadas a Jorge.

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

**Graph entropy:** 0.7393  

- **Community 11** (95 nodes): MCP Server Behavior, Evaluación de Plantillas, Preguntas para el Desarrollador
- **Community 0** (5 nodes): FutbolWeb, AG10-AUSTERITY-LOCK, Dependencias Externas
- **Community 1** (4 nodes): Onboarding Capability
- **Community 2** (4 nodes): Redis como proyecto C, Controladores Apex en Salesforce, Módulos Bicep en Azure
- **Community 3** (4 nodes): Smithy, SOQL en Apex
- **Community 4** (4 nodes): KDL

---

*Mirror auto-generated 2026-07-31T01:23:15Z | La Garra → DFLghub/amos-context*
