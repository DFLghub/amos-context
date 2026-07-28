# amOS Context — @$go Live Mirror
**Generated:** 2026-07-28T00:39:02Z  
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

### JPI erradicación total de etapa intermedia en /opt/360eventos
**Type:** decision  
**Project:** dfl  

Fecha: 2026-07-26
Repositorio: /opt/360eventos
Rama: feat/jpi-fase-5-real-runtime-v0.1
SHA revisado: a241ef5b2aeb76bfd0eae45f7dcf49166a4739dc
Trabajo realizado: erradicación literal del término eliminado en todo el repo, sin allowlist, sin excepciones y sin conservarlo en decisión ni gate.
Cambios clave: reescritura positiva de la decisión canónica en domain/02_knowledge/decisions/NO_INTERMEDIATE_STAGE.md; reescritura positiva del gate scripts/jpi-domain-term-guard.mjs; reescritura del test src/features/jpi/domain-term-guard.test.mjs; limpieza de superficies activas de runtime, ontología, business rules, business logic, discovery y knowledge.
Evidencia: búsqueda exhaustiva en /opt/360eventos con TOTAL_OCCURRENCES=0 y ALLOWLISTED_OCCURRENCES=0; CHANGELOG.md inspeccionado aparte sin coincidencias.
Validación: node scripts/jpi-domain-term-guard.mjs PASS; node --test src/features/jpi/domain-term-guard.test.mjs 3/3 PASS; npm run test:jpi PASS; node --test business-os/tests/fmd-jpi-fase5-e2e.test.js business-os/tests/fmd-runtime-integration.test.js business-os/tests/fmd-runtime-invariants-fix.test.js 49/49 PASS.
Resultado: PASS / PRECOTIZACION_PERMANENTLY_ERADICATED.

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

### JPI Fase 5 autorizada — E2E empresarial completo
**Type:** decision  
**Project:** dfl  

**What**: Autorización para iniciar JPI Fase 5 — E2E empresarial completo de la Empresa Sintética JPI, sobre happy path probado de Fase 4.1.

**Why**: Fase 4.1 completó portabilidad de factory (contrato neutral, intercambio sfv5/test-double), pero quedó con 7 fallos en resilience (timeout, retry, recovery). Deuda registrada como FACTORY_BRIDGE_RESILIENCE_BACKLOG. Fase 5 avanza el objetivo empresarial sin depender de esa deuda.

**Where**: /opt/360eventos (JPI) y /opt/saas-factory-setup/saas-factory (SFV5 mock). Rama aislada para Fase 5. Documentación: /opt/360eventos/JPI-FACTORY-PHASE4.1-FINAL-STATE.md.

**Learned**: 
- Happy path operativo: creación idempotente, polling, E2E básico, artefactos reales, SHA256, metadata
- Factory seleccionable vía DFL_FACTORY_ID solamente
- No payload.adapter, no lógica SFV5 específica en BOS
- Secuencia: SOLICITUD → COTIZACIÓN → RESERVA → OPERACIÓN → CIERRE (no PRECOTIZACION)
- Criterio éxito: escenario completo de punta a punta, brecha activa fabricación neutral, artefacto validado, BOS cierra post-validación, evidencia trazable
- Máximo 1 revisión posterior
- Veredicto final: READY_FOR_FINAL_REVIEW o FAILED_TO_COMPLETE_PHASE_5

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

### KNL v1.0 contrato operativo validado
**Project:** dfl  

TOPIC: dfl/knl/v1
TYPE: decision
STATUS: active
DATE: 2026-06-28
SUMMARY: KNL v1.0 queda operativo como contrato oficial en /go. knl.json valida schema dfl.knl.v1 con semantic communities/entropy, navigation neighbors, memory, policy, provenance, comparator y validation. graph_context no aparece en /go. knl_compare.py ahora soporta snapshots previos con links y genera comparator status changed con previous_available=true. dfl-nav --brief muestra neighbors. P0/P4 quedan pendientes de confirmacion: regen_graph.sh aun usa OPENAI_API_KEY y graphify como productor; contrato KNL requiere ag_topologo.py como productor canonico de graph.json y Graphify solo como consumidor/analisador. P3 gap: ag_topologo local declara v0.1; no se encontro v0.3 instalable.
EVIDENCE: python3 /opt/dfl-context-proxy/tests/test_knl_contract.py => knl contract ok. Public /go has knl=true, graph_context=false, validation ok.

### Session summary: futbolweb-app
**Project:** futbolweb-app  

## Goal
[CHECKPOINT COMPLETO] HLC Reconciliación Final de Consolidación DFL v1: contrastar artefactos contra el estado institucional más reciente (cierre residuales Ola 1), eliminar pendientes obsoletos, verificador con solo residuales reales, commit+Engram+mirror. COMPLETADA.

## Instructions
- No nuevas remediaciones; no GOBERNAR; no tocar futbolweb (Codex de producto commiteó 5595c24/e55d2c5 durante la sesión); no conservar pendientes solo por aparecer en docs antiguos.
- Regla de método nueva: antes de declarar pendiente en el registro vivo, contrastar contra el ÚLTIMO doc de cierre Y contra realidad ejecutable.

## Discoveries
- Causa raíz de contradicciones: consolidación v1 se construyó sobre 06/09/obs#221, ANTERIORES al cierre de residuales Ola 1 (docs 11/12/13).
- B-2 era FALSA: bundles off-host SÍ están en VM3 bajo /data/dfl-backups/engram/organ-preservation/2026-07-11-wave1/ (prefijo engram/ permitido); SHA-256 revalidados idénticos hoy vía pull rsync.
- D-1 PAT: revocado (202/401), cero holders revalidado hoy (0 archivos, 0 proc) → retiro prediccion2026 DESBLOQUEADO, sin fecha dura.
- D-2: DFLghub/saas-factory-setup existe, main=5e42124=local, upstream push DISABLED.
- HALLAZGO NUEVO Drive: 12_FutbolWeb/backups/1Password.txt (204B, 2026-07-06, fileId 1g4-4BoWbdQ0JRvggnTTFxwnjjXVASczZ) — NO leído, posible credencial → revisión Jorge. ZIP antiguo sigue presente (D-5, CC puede borrarlo a la orden).

## Accomplished
- ✅ 5 hechos revalidados contra realidad (evidencia: EVIDENCE/reconciliacion-revalidacion.txt).
- ✅ Artefactos corregidos: 00/01/02/04/05/06; nuevos: 08-RECONCILIACION-FINAL.md, HANDOFF-CODEX.md; registro-vivo.json reconciliado (JSON válido).
- ✅ Verificador: 5 residuales reales únicamente (SIN-PUSH prediccion2026 + 4 SIN-RESPALDO D-4/B-3); run3 en EVIDENCE.
- ✅ Commit 7b77b78 pusheado a DFLghub (scan de secretos limpio).
- ✅ Gate 4B: obs #236 (cierre); #218→[RESOLVED] archivada; #221/#231 marcadas como superadas/corregidas.
- ✅ Mirror publicado: MIRROR: updated | commit e525b9a3854998b56ff338291b3305c5308ce5a2 | 2026-07-12 20:11:48 +0000.

## Next Steps
- Jorge decide: retiros B-5 (sf-test, prediccion2026 — desbloqueados), D-4 copias únicas (~2.2GB), D-5 borrar ZIP Drive (CC ejecuta a la orden), revisar 1Password.txt, B-3 crear repos manuales (nq-factory/engram-mcp/fork engram), B-1 Drive-Codex.
- Codex: retomar con HANDOFF-CODEX.md (audits/consolidacion-institucional-dfl-v1/) — arranque seguro §1, residuales esperados §3, cierre contingente §5.

## Relevant Files
- audits/consolidacion-institucional-dfl-v1/08-RECONCILIACION-FINAL.md — corte de verdad más reciente
- audits/consolidacion-institucional-dfl-v1/HANDOFF-CODEX.md — handoff operativo para Codex
- governance/registro-vivo/registro-vivo.json — fuente canónica reconciliada
- EVIDENCE/reconciliacion-revalidacion.txt, registro-vivo-check-run3-reconciliacion.txt

### Session summary: futbolweb-app
**Project:** futbolweb-app  

## Goal
Cierre de incidente P0 de seguridad en 360Eventos: service role key de Supabase comprometida, rotada y verificada en producción.

## Instructions
Sin cambios de preferencia registrados en esta sesión.

## Discoveries
- Engram obs #112 fue creado en proyecto `futbolweb-app` por herencia del cwd (`/opt/futbolweb`) aunque el incidente pertenecía a 360Eventos. `mem_update` no soporta reasignación de proyecto — workaround: crear nuevo obs en proyecto correcto + marcar el original como MIGRADO.
- `mem_delete` no está disponible como herramienta deferred en este entorno.

## Accomplished
- ✅ Bootstrap @$go completado — contexto DFL activo al 2026-06-30
- ✅ Incidente P0 360Eventos cerrado: key `sb_secret_qcasL...` eliminada, producción migrada a `sb_secret_5E52V...`, Vercel actualizado, /cotizar verificado
- ✅ Engram obs #121 creado en proyecto `360eventos` con resolución completa
- ✅ Engram obs #112 marcado como MIGRADO (apunta a #121)

## Next Steps
- Pendientes FutbolWeb activos: knockout DB layer wiring, case-sensitivity de realAdvancingTeam, diagnóstico webhook GitHub-Vercel
- Verificar deploy commit `50316e3` en Vercel

## Relevant Files
- Supabase proyecto 360Eventos: uvdunupmjrbndistyrwn (key rotada)
- Vercel env vars 360Eventos: actualizadas con secret_key_2

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

---

## RECENT ACTIVITY (cross-project)

### @$fin cierre Codex - Modelo B2 Salud Institucional
**Type:** fact  
**Project:** dfl  

Cierre @$fin ejecutado por Codex el 2026-07-10. Trabajo realizado: creado el artefacto /opt/dfl-knowledge/audits/organismo-v1/MODELO-SALUD-INSTITUCIONAL-DFL-B2.md con el Modelo B2 de Salud Institucional de DFL; copiado tambien a /root/DFL-ChatGPT/MODELO-SALUD-INSTITUCIONAL-DFL-B2.md como destino Google Drive/local solicitado. El documento cambia el marco desde auditoria de organos/componentes hacia medicina interna institucional: dimensiones de salud estrategica, arquitectonica, operacional, metabolica, cognitiva, organizacional, economica, Digital Workforce, fabricas, BOS y ecosistema; incluye sistema permanente de vigilancia, panel clinico, critica del informe anterior y relectura del stack Gentleman Programming/Alan. Tambien se dejo previamente el suplemento /opt/dfl-knowledge/audits/organismo-v1/INFORME-CODEX-AUDITORIA-ORGANISMO.md. No se hizo commit de estos nuevos artefactos B2.

### Session summary: futbolweb-app
**Type:** session_summary  
**Project:** futbolweb-app  

## Goal
Construir el circuito @$go → Graphify → Engram: regenerar el grafo semántico de /opt/dfl-knowledge/, conectarlo al endpoint /go del dfl-context-proxy, y mejorar el onboarding DFL para que cualquier agente arranque con contexto topológico preciso.

## Instructions
- METABOLISMO: Conserve. Respuestas cortas. Solo output de comandos cuando se pide diagnóstico.
- No sugerir próximos pasos cuando el usuario pide solo diagnóstico.
- Reportar cada entrega cuando está completa antes de continuar a la siguiente.
- Circuit breakers en producción: reportar criterio antes de activar, no ejecutar regen completa sin autorización.

## Discoveries
- graphify detecta automáticamente OPENAI_API_KEY como backend — no necesita GEMINI para extracción semántica en cron
- El grafo de dfl-knowledge tiene 51 componentes conectados y grado promedio 1.36 — la sparsidad es estructural. El parámetro `resolution` en cluster() no reduce singletons aislados. Fix real: filtrar display, no ajustar algoritmo.
- graphify cluster() acepta `resolution: float = 1.0` pero no ayuda con grafos muy dispersos
- 52 comunidades para 118 nodos: 44 son singletons/pares, solo 8 son comunidades activas (>2 nodos)
- El endpoint /go previo viajaba con ~4KB de markdown en cada request sin importar si se necesitaba
- gen_summary.py original perdía hiperedges y alertas vs. el GRAPH_SUMMARY.md escrito manualmente — divergencia silenciosa
- graphify.build.build_from_json() + cluster() + god_nodes() + surprising_connections() funcionan sin LLM — pueden correr en cron
- El campo `recent_engram_dfl` del /go filtra por topic_key o title conteniendo "dfl" u "onboarding"
- regen_graph.sh necesitaba circuit breaker porque CRON 2 en producción puede fallar silenciosamente y sobreescribir un graph.json válido con uno vacío
- @$go (comando del agente) y /go (ruta HTTP) son capas distintas — confusión frecuente en onboarding

## Accomplished
- ✅ Diagnóstico completo del servidor: todos los servicios activos (engram 7437/8090, dfl-context-proxy 8091, MCP 8092, caddy, next-server 3001, n8n 5678)
- ✅ Mapeado /opt/dfl-knowledge/ como directorio fuente principal de documentación DFL: 66 .md, 29 .docx, 5 .pdf, estructura 00-12 + CO-001
- ✅ Regenerado graph.json de /opt/dfl-knowledge/ con extracción semántica completa (118 nodos, 80 edges, 60 archivos desde caché, 12 nuevos via subagente)
- ✅ Labeleadas 15 comunidades principales: FutbolWeb Identity & UAS (0.67), MERCADER BOS (0.67), FutbolWeb Scoring Pipeline (0.50), DFL Core Doctrine (0.50), etc.
- ✅ Creado /opt/dfl-knowledge/graphify-out/GRAPH_SUMMARY.md con god nodes, comunidades, sorpresas, hiperedges, alertas
- ✅ Modificado /opt/dfl-context-proxy/main.py: endpoint /go agrega graph_summary y recent_engram_dfl
- ✅ Fix 1: GET /go → graph_context ligero (top-3 nodes + 1 sorpresa, ~500 bytes). GET /go?deep=1 → graph_summary completo (~4KB)
- ✅ Fix 2: gen_summary.py con hash guard — no sobreescribe GRAPH_SUMMARY.md si graph.json no cambió. Hash en .summary_graph_hash
- ✅ Fix 3: display corregido — header muestra "8 comunidades activas (44 singletons/pares filtrados)" en vez de "52 comunidades"
- ✅ Fix 4: circuit breaker en regen_graph.sh — si nuevo graph.json tiene <90% nodos del anterior, aborta y restaura desde backup (.prev)
- ✅ Fix 5: nota explícita en DFL_Agent_Onboarding_Config.md — @$go vs /go son capas distintas, nunca intercambiar
- ✅ Cron 1 (diario 3am UTC): daily_check.sh — regenera GRAPH_SUMMARY.md; si >5 .md modificados desde última regen completa, dispara CRON 2
- ✅ Cron 2 (domingo 4am UTC): regen_graph.sh — extracción semántica completa con OpenAI + circuit breaker + actualiza summary
- ✅ DFL_Agent_Onboarding_Config.md actualizado a v0.3: @go → @$go en todas las ocurrencias (sin tocar URLs), sección 1.1 con circuito completo
- ✅ /opt/dfl-knowledge/graphify-out/.last_full_regen inicializado con timestamp 2026-06-26T23:26:49Z

## Next Steps
- Instalar poppler-utils para habilitar extracción de PDFs (5 archivos actualmente inaccesibles: agPattern INGENIERIA-DE-NBLMS + 3 PDFs de Guia Personal)
- Considerar excluir Guia Personal/ de futuros runs de graphify (documentos inmobiliarios personales sin relación con DFL)
- Renovar cron-job.org ESPN sync antes de 2026-07-20 (alerta activa)
- Evaluar si los campos `recent_engram_dfl` y `graph_context` en /go tienen el tamaño correcto — el response ligero sigue siendo ~21KB por el contenido completo de memorias Engram
- El graph_context light podría incluir la "key_bridge" (nodo con mayor betweenness centrality) además del key_surprise

## Relevant Files
- /opt/dfl-knowledge/graphify-out/graph.json — grafo semántico completo (118 nodos, 80 edges), generado 2026-06-26
- /opt/dfl-knowledge/graphify-out/GRAPH_SUMMARY.md — resumen humano-legible del grafo (god nodes, comunidades, sorpresas)
- /opt/dfl-knowledge/graphify-out/graph_context_light.json — payload mínimo para /go sin ?deep=1
- /opt/dfl-knowledge/graphify-out/.summary_graph_hash — hash md5 de graph.json al momento de última escritura de GRAPH_SUMMARY.md
- /opt/dfl-knowledge/graphify-out/.last_full_regen — timestamp de última ejecución exitosa de CRON 2
- /opt/dfl-context-proxy/main.py — proxy HTTP: /go (light/deep), /context/dfl, /health
- /opt/dfl-knowledge/scripts/gen_summary.py — regenera GRAPH_SUMMARY.md + graph_context_light.json sin LLM
- /opt/dfl-knowledge/scripts/regen_graph.sh — CRON 2: extracción semántica completa con circuit breaker
- /opt/dfl-knowledge/scripts/daily_check.sh — CRON 1: check condición + regen summary o full
- /opt/dfl-knowledge/DFL_Agent_Onboarding_Config.md — onboarding v0.3: @$go corregido, sección 1.1 con circuito completo

### Candidate Vault 04: estructura y estado 2026-06-24
**Type:** fact  
**Project:** dfl  

Ubicación: DFL-ChatGPT/04_Candidate_Vault/. Ciclo de vida de artefactos: pending_review → audited_pass → promoted → hibernated → rejected. Contenido actual: audited_pass/ tiene Gate_Engine_Caso01/02/03_PRP001, Gate_Engine_MVP_Spec_v0.2, Gate_Engine_v0_Checklist_Manual, MEMO_CIERRE_Gate_Engine_v0, PERIMETRO_DECLARADO_v1.0, SDLC_Matriz_Correspondencia. pending_review/ tiene: NBLM2-FISIO-DFL-01_Matriz_de_Fisiologia_Contextual_CANDIDATE.md, agPattern-INGENIERIA-DE-NBLMS-v1_CANDIDATE.pdf, agProtocol-METRICS-REALITY-ROI-v2.5.3.5-C.md. En raíz del vault: agLego-PATTERN-ASYNC_INSPECTION_SPLIT.md/.docx (estado candidate/sealed, sin HI approval), agPattern-ECDA-Topologia_de_Roles-v1.0_CANDIDATE.md (duplicado). Nota verificación ONBOARDING: los archivos agLego-PATTERN-ASYNC_INSPECTION_SPLIT.docx/.md y agPattern-ECDA-Topologia_de_Roles-v1.0_CANDIDATE (1).md están en la raíz del vault (no en root ni en subdirectorios incorrectos) — condición de Zapata3 sobre artefactos stray no se aplica aquí.

**Type:** manual  
**Project:** dfl-knowledge  

**TOPIC**: dfl/concierge/f1a-dual-exploration-addendum
**TYPE**: decision
**DATE**: 2026-07-28
**MISSION**: CONCIERGE F1A — DUAL EXPLORATION WITH GRAPHIFY AND agTOPÓLOGO (reabre focalizadamente [[dfl/concierge/f1a-reconciliation]] obs #373)

**WHAT**: Addendum en `audits/concierge-f1a-reconciliation-2026-07-27/CONCIERGE-F1A-DUAL-EXPLORATION-ADDENDUM.md`. Repitió la reconciliación de Concierge por 3 caminos independientes: lineal extendido (git worktree list, git stash list, git fsck --unreachable — ninguno corrido en la ronda anterior), Graphify (codebase-memory-mcp sobre el proyecto ya indexado `dfl-knowledge-workunit` = main@9f364c0, 3873 nodos/5716 aristas), y agTopólogo (graph.json semántico institucional, 140 nodos).

**HALLAZGO MÁS IMPORTANTE**: 56 tests adversariales de `WorkUnitLedger` (boundary, tiempo, máquina de estados, concurrencia real, serialización, replay) existen SOLO como commits huérfanos (`1f9d1ec`/`66cb84c`/`d909147`, construidos sobre `6670429` = tip remoto de `feat/dfl-concierge-workunit-claims`, nunca fusionados ni referenciados por ninguna rama). Verifiqué EJECUTÁNDOLOS (no solo leyendo) contra el `concierge/workunit.py` real de `main` hoy: 56/56 PASS. Recuperable con `git branch <nombre> d909147318003b3b79dc25f554fd8059fc2df140` antes de cualquier `git gc`.

**SEGUNDO HALLAZGO**: 3 documentos de arquitectura (`F1-COMPILER-CONFORMANCE-KIT.md`, `CP-F1-CONFORMANCE-KIT.md`, `DRG-002-R1-WORK-UNIT-COORDINATION-DESIGN.md`) existen solo en un commit-stash huérfano (`34b6a7d3`/`521a4e1e`), nunca commiteados en ninguna rama — el código relacionado (conformance/independent.py, kit.py) SÍ está en `feat/dfl-concierge-cc-render-validator`, pero su documentación de diseño no.

**HALLAZGO METODOLÓGICO CLAVE**: el grafo semántico institucional (`graphify-out/graph.json`, productor ag_topologo.py, el que KNL ordena consultar ANTES de exploración manual) solo tiene 6 de 140 nodos relacionados con Concierge, y NINGUNO posterior a 2026-07-23 — falta toda la arbitración, congelamiento de API, DeepSeek review, authz, workunit, conformance, dogfood. Un agente que siguiera literalmente "graph preflight mandatorio" de KNL antes de explorar manualmente habría visto solo el veredicto v1 NO_GO original y concluido erróneamente que Concierge es un diseño temprano abandonado — exactamente el error que causó la primera auditoría live-path antes de su propio addendum de corrección.

**TRIANGULACIÓN DEL CAMINO VIVO**: 0% de conexión reconfirmado por 3 métodos independientes (grep textual, search_graph BM25 sobre dfl-context-proxy, query Cypher de aristas IMPORTS sobre main completo) — 0 resultados en los 3. Hallazgo más sólido de toda la reconciliación, sin ninguna discrepancia entre métodos.

**BASELINE**: main@9f364c0 CONFIRMED (no cambia) — reforzado, no cuestionado, por los 3 métodos.

**VEREDICTO**: LINEAR_INVENTORY=COMPLETE (ahora sí), GRAPHIFY_COVERAGE=PARTIAL, AGTOPOLOGO_COVERAGE=PARTIAL (casi nula para Concierge específicamente), CANONICAL_BASELINE=CONFIRMED, EXISTING_WORK_RECONCILED=WARN (2 nuevos ítems de recuperación + los 3 previos), F1B_READY=NO (sin cambio), PREVIOUS_ANALYSIS_ERRORS=IDENTIFIED.

**CAUSA RAÍZ DEL ERROR METODOLÓGICO ANTERIOR**: tratar "recorrido por refs" (git branch -a -v + git log --all) como equivalente a "recorrido completo de la historia del repo" — git fsck --unreachable encuentra objetos que --all nunca muestra. Además, un flag `[ahead 1, behind 1]` se reportó como dato de estado sin investigar QUÉ commit era la divergencia real.

**PRÓXIMA ACCIÓN**: agregar 2 pasos al plan de consolidación ya propuesto: (4) recuperar la cadena de 56 tests adversariales, (5) recuperar los 3 docs de conformance — antes de que corra un git gc que los elimine.

**Type:** manual  
**Project:** dfl-knowledge  

**TOPIC**: dfl/skill-engineer/cc-cross-review-v0.1
**TYPE**: decision
**DATE**: 2026-07-28
**MISSION**: DFL SKILL ENGINEER v0.1 — CROSS-REVIEW BY CC (repo /opt/dfl-knowledge)

**WHAT**: Revisión independiente de ingeniería sobre `/opt/dfl-knowledge/experiments/dfl-skill-engineer-v0.1/first-materialization/` (materialización de CX). Entregable completo en `CC-CROSS-REVIEW.md` dentro de esa misma carpeta. Modo read-only + ejecución de tests/validadores existentes (sin editar nada, sin tocar SFV5).

**VEREDICTO**: `CANDIDATE_TESTED` — coincide con el nivel superior de la afirmación de CX. Reproduje independientemente: 32 skills SFV5 (criterio correcto, sin omisiones), 2 con scripts, 8 con references, 1 con assets, 0 con schemas, 64 archivos — todo correcto. El fallo del fixture (`normalizeUserName`) se reproduce igual al log guardado. Los 7 tests (`artifacts.test.mjs` + `debugging-skill.test.mjs`) corren en verde de forma independiente. El validador de schemas (`validate_artifacts.mjs`) pasa 6/6.

**DESACUERDO CON CX**: "1 skill con tests detectables" es un falso positivo — el único match es `skill-creator/references/SKILL_SPECIFICATION.md` (contiene "spec"), no un test real. El valor correcto es 0. Bug en `tools/generate_sfv5_inventory.mjs` línea 65/79: regex `/test|spec/i` sin distinguir convención real de test.

**HALLAZGO CRÍTICO (C-1)**: el bloqueo de autopromoción NO está mecánicamente implementado en ningún schema (cero uso de if/then/allOf condicional en los 6 `*.schema.json`, cero campos de identidad reviewer/actor/author en cualquiera). El único "check" (`evaluate_debugging_skill.mjs` línea 32) es una relectura post-hoc de un JSON ya escrito por la misma sesión que generó la candidata — trivialmente evadible. La invariante "revisión independiente obligatoria para promover" (SKILL_ENGINEER_CONTRACT.md) hoy depende de convención, no de código.

**OTROS DEFECTOS**: H-2 bug de extracción de "dependencies" (regex de backtick simple captura bloques de código triple-backtick completos, corrompe 18/32 entradas del inventario con >200 chars de código crudo). H-3 ausencia total de validación relacional entre artefactos (SKILL_REQUEST↔CONTRACT↔CANDIDATE↔EVALUATION↔PROMOTION nunca se contrastan por ID, solo se valida cada JSON aislado contra su propio schema). M-1 política de reintento declarada pero no ejecutable (nada cuenta intentos reales). M-2 3 de 7 tests verifican forma de JSON co-autorado con la candidata, no una re-derivación en vivo — riesgo de circularidad. M-3 cobertura de fixtures angosta (solo 1 tipo de fallo de 3 categorías que el propio playbook nombra).

**ESTADOS FINALES**: SFV5_CORPUS=INVENTORIED (con la corrección de tests), CONTRACTS=PARTIAL, DETERMINISTIC_LAYER=PARTIAL, FIRST_DEBUGGING_SKILL=TESTED, PROMOTION=HOLD (correcto), CANDIDATE_GATE=NEEDS_REDESIGN (construirlo hoy escalaría C-1/H-3 en vez de resolverlos).

**SIGUIENTE ACCIÓN DE MAYOR APALANCAMIENTO**: cerrar C-1 antes que nada — agregar regla condicional al schema PROMOTION_DECISION (si decision==promote, exigir reviewer_id distinto del autor + requires_independent_review==true) antes de construir cualquier candidate gate reutilizable.

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

**Graph entropy:** 0.5961  

- **Community 11** (101 nodes): Onboarding Capability, Índices Canónicos, Artifact and Runtime Matrix
- **Community 0** (4 nodes): PAT clásico, Componentes DFL
- **Community 1** (4 nodes): KDL, Jsonnet, Mermaid
- **Community 2** (4 nodes): FutbolWeb - Reality Sync, FutbolWeb - Ranking Summary, Dependencias Críticas
- **Community 4** (4 nodes): agLego, Soberana + Triunvirato, ag10
- **Community 3** (4 nodes): Motor de scoring knockout

---

*Mirror auto-generated 2026-07-28T00:39:02Z | La Garra → DFLghub/amos-context*
