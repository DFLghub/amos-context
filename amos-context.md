# amOS Context — @$go Live Mirror
**Generated:** 2026-07-26T05:24:02Z  
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

### JPI Phase 4 Independent Review Complete — PASS / READY_TO_MERGE
**Type:** decision  
**Project:** dfl-knowledge  

**What**: Independent verification of JPI Phase 4 BOS/FMD automation without relay completed. Verdict: PASS / READY_TO_MERGE.

**Why**: Phase 4 introduces autonomous BOS/FMD mission orchestration without human relay. Critical verification required to confirm: all state transitions, scenario matrix, producer SHA resolution, and Phase 3.5 guarantee preservation.

**Where**:
- JPI: /opt/360eventos (origin/fase-4-bos-fmd-sfv5-automation: 4d21c01...)
- SFV5: /opt/saas-factory-setup (origin/fase-4-bos-fmd-sfv5-automation: 6bc82f5...)
- Evidence: /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-4/05-INDEPENDENT-REVIEW-CC.md

**Learned**:

CRITICAL FINDING — producer_sha discrepancy explained:
- Prior evidence showed producer_sha = d126939... (Phase 3.5) when SFV5 branch head = 6bc82f... (Phase 4)
- Independent reproduction shows this was SFV5_FACTORY_ROOT configuration issue, NOT code defect
- When factory root correctly set to Phase 4 branch → producer_sha reports correctly (6bc82f...)
- Phase 4 commit 6bc82f5 added proper git metadata resolution to getProducerSha()
- Code is correct; requires proper env configuration

Test results:
- JPI full regression: 247/247 PASS
- Phase 4 focal suite (factory automation): 8/8 PASS
- Scenario matrix: 10/10 scenarios verified
- E2E real from clean job root: PASS

State transitions verified: REQUESTED → DISPATCHED → RUNNING → READY → VALIDATED → CONSUMED → CLOSED

Phase 3.5 guarantees preserved: freshness, fingerprint consistency, path confinement

No blockers. Ready for merge.

### JPI Phase 3.5 Post-Merge Independent Verification — PASS / PHASE_CLOSED
**Type:** decision  
**Project:** dfl-knowledge  

**What**: Independent verification of JPI Phase 3.5 post-merge closure completed. All criteria verified: SHAs, tags, commits, tests, E2E, correlations, path confinement.

**Why**: Institutional checkpoint required before phase closure. Verifies that merge was successful, all approved commits are on origin/main, tests pass, and E2E flow works end-to-end from empty job root.

**Where**: 
- SFV5: /opt/saas-factory-setup (origin/main: d12693998c38c7d5b1f83a74135dd65bb8ab57bf)
- JPI: /opt/360eventos (origin/main: 58b6546c4d92a562afdd1a6dc2a0a7b576566888)
- Evidence: /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-3-5/08-INDEPENDENT-POST-MERGE-REVIEW-CC.md

**Learned**: 
- SFV5 clean clone bridge tests: 8/8 PASS (path confinement, traversal rejection verified)
- JPI full regression: 237/237 PASS (from merged main)
- E2E real from clean job root: 1/1 PASS (artifact generated +69ms after mission packet)
- mission_fingerprint consistent across 5 outputs
- producer_sha consistent, equals SFV5 main HEAD
- Tags published and dereferenced correctly
- All path escapes rejected; confinement enforced
- Strict correlation verified: mission, goal, request, attempt, producer, artifact SHA
- No blockers found

Verdict: PASS / PHASE_CLOSED. Ready for next phase entry.

### JPI Phase 3.5 post-review closure
**Type:** decision  
**Project:** dfl  

2026-07-26 UTC. Session closed after a single consolidated correction round for JPI Phase 3.5 real SFV5 integration. Repos: /opt/360eventos and /opt/saas-factory-setup/saas-factory. Corrected SHAs: JPI 58b6546c4d92a562afdd1a6dc2a0a7b576566888 on branch fase-3-5-real-sfv5-bridge; SFV5 d12693998c38c7d5b1f83a74135dd65bb8ab57bf on branch fase-3-5-jpi-real-sfv5-bridge. Closed R35-01 by removing dependency on untracked cognitive-core helpers and proving bridge tests from a clean archive of the corrected SFV5 SHA. Closed R35-02 by enforcing strict correlation in JPI across status.json, artifact, test-report, producer-evidence, mission_id, goal_id, factory_request_id, attempt_number, mission_fingerprint, producer identity and expected producer SHA. Closed R35-03 by adding deterministic mission_fingerprint propagation and only allowing idempotent reuse when all identifiers and fingerprint match exactly. Closed R35-04 by enforcing canonical confinement of evidence_path in SFV5 and path confinement checks in JPI for producer-reported artifact/test/evidence paths. Verification: SFV5 relevant tests 21/21 PASS; JPI regression 237/237 PASS; fresh real E2E 1/1 PASS from empty job root; artifact generated after mission packet with recorded timestamps/checksums. Evidence updated under /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-3-5/ including 03-REPORT.md, 04-CC-FINDINGS-RESPONSE.md, 05-POST-REVIEW-VERIFICATION.md, CX-STATUS.md and raw logs. Remote verified: origin/fase-3-5-real-sfv5-bridge -> 58b6546..., origin/fase-3-5-jpi-real-sfv5-bridge -> d126939.... No merge to main performed.

### 360eventos Phase 3 SFV5 bridge completed
**Type:** decision  
**Project:** dfl  

Date: 2026-07-25 UTC
Repo: /opt/360eventos
Branch: fase-3-bos-fmd-sfv5-bridge
Final SHA: 4cac081e2d7fe914fc362a8861c66b5498c55679
Base SHA: b80c10ddfee9cd651847e9e85367387295cb983e
Summary:
- Completed Phase 2 zero by publishing main, fase-2-organizational-runtime, and tag jpi-phase-2-closed; verified with ls-remote; mirror push succeeded.
- Implemented BOS/FMD -> SFV5 bridge on business-os using existing factory_requests lifecycle plus new factory_request_bridge persistence.
- Added replaceable SFV5 adapter registry: sfv5 fails explicitly when no producer is connected, sfv5-test is deterministic local for tests.
- Added operational availability artifact consumer integrated with validateByOperaciones.
- Added HTTP endpoints for create/poll/review/use factory requests.
- Added migration 016_factory_request_bridge.
- Full business-os regression passed: 224/224 tests.
- Branch pushed to origin/fase-3-bos-fmd-sfv5-bridge.
Evidence:
- /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-3/00-FROZEN-SCOPE.md
- /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-3/01-ARCHITECTURE.md
- /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-3/02-REVIEW.md
- /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-3/03-COMMANDS.md
- /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-3/04-REPORT.md

**Type:** decision  
**Project:** futbolweb-app  

TOPIC: futbolweb/knockout-placeholders/deploy-verificado
TYPE: decision
STATUS: active
DATE: 2026-07-15

[RESUELVE pendiente de obs #262] Deploy de producción del commit 2a12586 VERIFICADO en Vercel: deployment id 5452404203, environment Production, state success, 2026-07-15T05:55:26Z (via GitHub Deployments API pública; sin CLI de Vercel en esta VM).

Evidencia en https://www.futbolweb.app (host canónico; apex futbolweb.app hace 307 → www):
- /match/mundial-2026-partido-089/grupo → "Paraguay vs Francia" (feeders resueltos, antes placeholders)
- /match/mundial-2026-partido-093/grupo → "Portugal vs España"
- /match/mundial-2026-partido-104/grupo → "España vs Ganador Partido 102" (SF 102 pendiente conserva placeholder)
- /mis-pronosticos → 200, RSC payload contiene completedResults con 101 filas y advancing_team reales (España×4 = finalista) — getCompletedMatchResultsSafe funcionando (no cayó al fallback [])
- /api/my-predictions contrato intacto {"ok":true}; /api/admin/reminder-candidates → 401 sin token (handler vivo, no se disparó); predict 104 → 200

Limitación: logs de función de Vercel no accesibles desde esta VM (sin token; /etc/dfl-secrets protegido) — evidencia indirecta: 200s, sin marcadores de error en HTML, resolución canónica operando.

**Type:** decision  
**Project:** futbolweb-app  

TOPIC: futbolweb/knockout-placeholders/fix-desplegado
TYPE: decision
STATUS: active
DATE: 2026-07-15

**Fix ejecutado y pusheado**: commit 2a12586 en main (origin) — "fix(knockout): resolve KO bracket names on mis-pronosticos, grupo page and reminder candidates". Resuelve la obs #261 (diagnóstico).

**Diseño (aprobado por Jorge, sin pipeline paralelo)**:
- `resolveWorldCupMatches(completedResults, locale)` en lib/knockout-reality.ts — función pura que compone localizeWorldCupMatches + applyKnockoutBracketAssignments (autoridad existente). Client-safe.
- `getCompletedMatchResultsSafe(now)` en lib/tournament-reality.ts — fetch canónico con degradación a [] ante fallo (placeholders, nunca crash).
- /mis-pronosticos: page.tsx (server) pasa completedResults como prop; MyPredictionsClient resuelve client-side manteniendo relocalización por locale.
- grupo page y reminder-candidates usan el mismo par de funciones (reminder con locale "es"; getOpenKoMatches ahora acepta matches como parámetro con default estático).

**Validación**: 84/84 tests (9 nuevos: KO resuelto/pendiente/sin resultados, mensaje reminder con nombres reales, render smoke MyPredictionsClient), lint limpio, build limpio (/mis-pronosticos ahora dinámica). Evidencia E2E local (next start + Supabase prod, solo lectura): partido 89 "Paraguay vs Francia", 93 "Portugal vs España", final 104 "España vs Ganador Partido 102" (SF 102 pendiente conserva placeholder — regla cumplida).

**No tocado**: puntajeTigreKnockout, scoring, Supabase schema/data, contratos de predicción, superficies ya correctas (upcoming/predict/today/oracle).

**Pendiente**: verificar deploy Vercel del commit 2a12586 en producción.

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

### amOS Event Model — veredicto auditoría 2026-06-23
**Type:** decision  
**Project:** dfl  

Auditoría del Event Model amOS realizada 2026-06-23 contra 3 docs canónicos (AgMaster_amOS_3, AI_amOS_Acta_Fundacional v1.1, Protocolo MS→amOS). Veredicto: B — Existe parcialmente pero disperso. Cobertura: Peso/costo metabólico→confidence+value en tabla events (Parcial, consolidar); Persistencia→status Origin Chain+estados Candidate Vault (Parcial, consolidar); Intención→scope+forbidden_uses agLego+Layer3 VALUE (Implícita, nombrar); Propagación→C-009+G-002 Protocol Taxonomy (Incompleta, GAP REAL); Relación con estado→Layer6+tabla asset_states (Existe, conservar). Conclusión: NO hace falta constructo nuevo tipo 'Light Signals'. Hace falta unificar y nombrar lo disperso. Gap real confirmado: G-002 Protocol Taxonomy (propagación, marcado como no cerrado en el Acta Fundacional). Próximo paso: cerrar G-002 dentro del Libro 1 amOS o como PRP independiente. Prerequisito: localizar RFC-DFL-001 (puede contener Event Model más completo).

### amOS — ontología activa 13 capas (Acta Fundacional v1.1)
**Type:** fact  
**Project:** dfl  

13 Capas ratificadas del ecosistema amOS (AI_amOS_Acta_Fundacional v1.1, 2026-06-15 FINAL): L1=REALITY (amOS models reality, never IS reality); L2=CONTEXT (architectural law, el contexto manda); L3=VALUE (produce/protect/enable/avoid consequences); L4=INFORMATION (utility is in relationship, not information); L5=ASSETS (Entity+ContextualValue+Identity+State+Relationships); L6=STATE (amOS revolves around State, not AI/GPTs/documents); L7=REGISTRIES (Asset+Protocol+State Registry); L8=PROTOCOLS (biggest gap, without protocols agMesh=concept); L9=HOMEOSTASIS (habits reducing degradation probability, not deterministic); L10=ATTENTION (scarcest resource is attention, not storage/tokens/compute); L11=ENERGY (ATP-D: consumes/costs/produces/recovers); L12=EVOLUTION (Candidate Vault→Triunvirato→Ratification→Doctrine); L13=CONSTITUTION (what can change/cannot/who governs/how it changes). Constitución activa: C-001 contexto determina valor; C-002 amOS modela realidad; C-005 ningún componente se autoaprueba; C-006 candidate only hasta ratificación HI; C-008 nada entra al núcleo sin TRIAGE; C-009 domain sovereignty (hard boundaries); C-013 Doctrine first-governance second-software third; C-015 amOS produce coherencia, no software.

### JPI Phase 4 Independent Review Complete — PASS / READY_TO_MERGE
**Type:** decision  
**Project:** dfl-knowledge  

**What**: Independent verification of JPI Phase 4 BOS/FMD automation without relay completed. Verdict: PASS / READY_TO_MERGE.

**Why**: Phase 4 introduces autonomous BOS/FMD mission orchestration without human relay. Critical verification required to confirm: all state transitions, scenario matrix, producer SHA resolution, and Phase 3.5 guarantee preservation.

**Where**:
- JPI: /opt/360eventos (origin/fase-4-bos-fmd-sfv5-automation: 4d21c01...)
- SFV5: /opt/saas-factory-setup (origin/fase-4-bos-fmd-sfv5-automation: 6bc82f5...)
- Evidence: /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-4/05-INDEPENDENT-REVIEW-CC.md

**Learned**:

CRITICAL FINDING — producer_sha discrepancy explained:
- Prior evidence showed producer_sha = d126939... (Phase 3.5) when SFV5 branch head = 6bc82f... (Phase 4)
- Independent reproduction shows this was SFV5_FACTORY_ROOT configuration issue, NOT code defect
- When factory root correctly set to Phase 4 branch → producer_sha reports correctly (6bc82f...)
- Phase 4 commit 6bc82f5 added proper git metadata resolution to getProducerSha()
- Code is correct; requires proper env configuration

Test results:
- JPI full regression: 247/247 PASS
- Phase 4 focal suite (factory automation): 8/8 PASS
- Scenario matrix: 10/10 scenarios verified
- E2E real from clean job root: PASS

State transitions verified: REQUESTED → DISPATCHED → RUNNING → READY → VALIDATED → CONSUMED → CLOSED

Phase 3.5 guarantees preserved: freshness, fingerprint consistency, path confinement

No blockers. Ready for merge.

### JPI Phase 3.5 Post-Merge Independent Verification — PASS / PHASE_CLOSED
**Type:** decision  
**Project:** dfl-knowledge  

**What**: Independent verification of JPI Phase 3.5 post-merge closure completed. All criteria verified: SHAs, tags, commits, tests, E2E, correlations, path confinement.

**Why**: Institutional checkpoint required before phase closure. Verifies that merge was successful, all approved commits are on origin/main, tests pass, and E2E flow works end-to-end from empty job root.

**Where**: 
- SFV5: /opt/saas-factory-setup (origin/main: d12693998c38c7d5b1f83a74135dd65bb8ab57bf)
- JPI: /opt/360eventos (origin/main: 58b6546c4d92a562afdd1a6dc2a0a7b576566888)
- Evidence: /opt/dfl-knowledge/evidence/jpi-synthetic-company-pilot/phase-3-5/08-INDEPENDENT-POST-MERGE-REVIEW-CC.md

**Learned**: 
- SFV5 clean clone bridge tests: 8/8 PASS (path confinement, traversal rejection verified)
- JPI full regression: 237/237 PASS (from merged main)
- E2E real from clean job root: 1/1 PASS (artifact generated +69ms after mission packet)
- mission_fingerprint consistent across 5 outputs
- producer_sha consistent, equals SFV5 main HEAD
- Tags published and dereferenced correctly
- All path escapes rejected; confinement enforced
- Strict correlation verified: mission, goal, request, attempt, producer, artifact SHA
- No blockers found

Verdict: PASS / PHASE_CLOSED. Ready for next phase entry.

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

**Graph entropy:** 0.8274  

- **Community 11** (95 nodes): Asunciones de Verificación, Digest de Contenido, Artifact and Runtime Matrix
- **Community 0** (7 nodes): Capacidades Comunes Reutilizables, dfl-secrets
- **Community 1** (4 nodes): Onboarding Capability
- **Community 2** (4 nodes): Blade en Laravel, Beancount, MCP (Model Context Protocol)
- **Community 3** (4 nodes): NBLM2, Working Memory, Active Library
- **Community 4** (4 nodes): Documentación JSDoc, Estado de la casa DFL, Onboarding multi-agente

---

*Mirror auto-generated 2026-07-26T05:24:02Z | La Garra → DFLghub/amos-context*
