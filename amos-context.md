# amOS Context — @$go Live Mirror
**Generated:** 2026-07-25T03:51:16Z  
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

### SAFE_PARTIAL_CHECKPOINT — WORK_UNIT remediation paused
**Type:** decision  
**Project:** dfl  

Checkpoint provisional solicitado por Jorge. Rama/worktree: feat/dfl-concierge-workunit-claims en /opt/dfl-knowledge-workunit, HEAD aaf740a6737d4148515050571f9485602c0f08fb, base 3d0cc64. No se modificaron archivos, no hubo commit ni push durante este checkpoint. La remediación de los findings 4R quedó detenida para otro agente.

Findings pendientes: HIGH 1 — idempotency_key personalizado se busca por operation_key y retorna idempotente sin comparar operación, unit_id, scope, actor/owner y transición; requiere fingerprint constitutivo y rechazo ERR_WORK_UNIT_IDEMPOTENCY_REUSE ante reutilización incompatible. HIGH 2 — falta validador central estricto antes de _read_events(), reconcile() y _fold(); eventos incompletos/desconocidos pueden producir KeyError o READY falso; requiere validación de tipo, campos, IDs, timestamps, event_key, operation_key/fingerprint y semántica por transición, con ContractError/quarantine. MEDIUM — expire_stale() devuelve solo strings unit_id; debe devolver estructura inequívoca con unit_id, scope y actor.

Pruebas existentes antes de remediar: focused WORK_UNIT 8/8, suite Concierge 84/84, git diff --check y smoke claim→handoff→release PASS. PROXIMO_AGENTE_DEBE: inspeccionar nuevamente estado limpio; implementar únicamente esos tres findings; agregar pruebas adversariales para reutilización de clave, repetición legítima, eventos inválidos/missing/unknown, reconcile mixto y expiraciones del mismo unit_id en scopes distintos; ejecutar focalizada, suite completa, diff-check, smoke; actualizar receipt, commit separado y push solo de esta rama. No tocar AuthZ, register.py, scheduler, UI, main ni otros slices. Estado: SAFE_PARTIAL_CHECKPOINT / IMPLEMENTED_AWAITING_REMEDIATION.

### WORK_UNIT claims implemented awaiting review
**Type:** decision  
**Project:** dfl  

CP-F1-WORKUNIT-CLAIMS-01 implementado por Codex en feat/dfl-concierge-workunit-claims, commit aaf740a sobre base 3d0cc64. Added dedicated concierge/workunit.py ledger with CLAIMED/RELEASED/EXPIRED/HANDED_OFF, lock+append+fsync, stable operation idempotency, conflict, owner-only release, expiry/reclaim, explicit handoff, scope support, corruption quarantine/reconcile, and public exports. Tests: focused 8/8, full concierge 84/84, diff-check and API smoke PASS. Receipt architecture/receipts/CP-F1-WORKUNIT-CLAIMS-01.md. Status IMPLEMENTED_AWAITING_INDEPENDENT_REVIEW. PROXIMO_AGENTE_DEBE: independent 4R review; no self-approval or main merge.

### CP-01 completado: DRG-002-R1 DFL Concierge diseño normativo commiteado (rama feat/dfl-concierge, sin código, pendiente auditoría Codex)
**Type:** decision  
**Project:** dfl-knowledge  

TOPIC: dfl/onboarding/drg-002-r1-cp01
TYPE: decision
STATUS: active
DATE: 2026-07-22

**CP-01 COMPLETADO** — DRG-002-R1 (DFL Concierge / ACS) diseño normativo redactado y commiteado. Rama `feat/dfl-concierge`: commit 7cbf471 (doc `architecture/DRG-002-R1-dfl-concierge.md`) + e8edaef (receipt `architecture/receipts/CP-01.md`). Sin código, sin push (offhost_durable=false: recuperable en La Garra + Engram, NO durable off-host — decisión abierta).

Diseño = síntesis onboarding ACS (propuesta GPT) + mis 5 disciplinas + los 4 ajustes obligatorios de Jorge: [A1] drift prevenible+detectable con artefactos generados inmutables (version+source_commit+hashes), NO "imposible"; [A2] paridad sobre Canonical Onboarding Packet normalizado con hashes por sección + coverage manifest por adaptador + degradaciones DECLARADAS (READY_WITH_LIMITATIONS, no fingir igualdad); [A3] identidad separada agent/runtime/session + capability + authorization, niveles de confianza ATTESTED/CORROBORATED/ASSERTED/UNKNOWN, tope de authz por trust (columna de seguridad — ASSERTED nunca escribe estado); [A4] checkpoint receipt versionado + durabilidad off-host honesta. participant_type admite AI_AGENT/SYSTEM_AGENT/AUTOMATION/HUMAN/SERVICE desde ya; build F1 solo AI_AGENT (3 tiers). Corte mínimo F1: fuente canónica estructurada + compilador determinista + validador (staleness/reproducibilidad/cobertura/paridad) + register JSONL/receipts + identidad ATTESTED/ASSERTED + CLI local-first, SIN daemon/HTTP/UI. Supersede borrador R0 AMOS-LOBBY-REDESIGN.md.

**Next / PROXIMO_AGENTE_DEBE**: Jorge decide (1) las 4 decisiones abiertas inevitables §11 (durabilidad off-host, mecanismo de atestación, tabla scope→min_trust, hogar de la fuente canónica) y (2) si despacha el Mission Packet (DRG-002-R1 Apéndice A) a Codex para auditoría de constructibilidad. NO construir hasta veredicto GO de Codex. Relevo: reanudar desde rama feat/dfl-concierge (e8edaef) + este obs.

### DRG-002 amOS Lobby: rediseño onboarding (conserjería ejecutada + paridad por state_version + libro de registro) — diseño, pendiente build
**Type:** decision  
**Project:** dfl-knowledge  

TOPIC: dfl/onboarding/amos-lobby-redesign
TYPE: decision
STATUS: active
DATE: 2026-07-22

**What**: DRG-002 emitido — rediseño del onboarding/outboarding DFL como "amOS Lobby". Doc en /opt/dfl-knowledge/architecture/AMOS-LOBBY-REDESIGN.md (solo diseño, sin implementar, sin commit). Responde el pedido de Jorge (metáfora Directorio de hotel/condominio + conserjería que registra entradas/salidas y enruta por fallout). Tres garantías: G1 acceso único = Conserjería que EJECUTA detección→ruta→log (CLI `dfl onboard/outboard/capsule/register` + `GET /onboard`), no self-serve desde prosa (que es la causa raíz de todos los fallos vistos). G2 PARIDAD llegado↔incumbente = exponer un `state_version` único; primitiva YA existe: push_mirror.sh hashea el payload /go sin generated_at en .last-mirror-hash — solo falta exponerlo; incumbentes comparan vía GET /board/version y re-sincronizan; divergencia se vuelve detectable+resoluble. G3 libro de registro = onboarding_register.jsonl append-only (ts, runtime, profile, transport, state_version, gate_result, errors[], outcome) escrito por la conserjería en cada check-in/out — da a Jorge quién onboardeó/resultado/errores consultable, telemetría (ej. atgo_not_triggered).

Recomendación decidida: SÍ construir la conserjería, como capa fina de orquestación que REUSA /go (Board) + hash de push_mirror (versión) + lógica de la matriz (routing) — integración, no obra nueva. Frontera física: CONSULTOR (ChatGPT sin red) no lo alcanza ninguna app; se le genera el capsule vigente para pegar. Consolidación: colapsar los 5 artefactos divergentes (DFL_Agent_Onboarding_Config.md, /root/AGENTS.md, /root/.codex/AGENTS.md, CHATGPT_WORK_ATGO_INSTRUCTION.md, mi ONBOARDING_CAPSULE.md redundante→descartar) a una fuente única + proyecciones versionadas. Plan 3 fases: F1 Fundación (exponer state_version + register + hook Codex check-in), F2 Conserjería (CLI+HTTP), F3 Consolidación.

**Next**: Jorge decide: (1) construir F2 completa o F1 primero; (2) register JSONL vs SQLite; (3) F1 toca main.py (reload proxy) + hook Codex = lote de estado a aprobar.

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

### @$fin cierre Codex - intento tmux bloqueado por sandbox
**Type:** fact  
**Project:** dfl  

Cierre @$fin ejecutado por Codex el 2026-07-14. Actividad de la sesion: el usuario pidio `tmux new-session -A`; el intento fallo con `error connecting to /tmp/tmux-0/default (Operation not permitted)`, por restriccion de permisos del sandbox sobre el socket tmux. Se explico que seria necesario ejecutar el comando fuera del sandbox/con permisos elevados. No se hicieron cambios de archivos, commits ni modificaciones de repositorios.

---

## PENDING

### [VERIFIED] Hook SessionStart @go operativo en sesión CC 2026-07-11 — PROXIMO_AGENTE_DEBE FutbolWeb cumplido
**Project:** futbolweb-app  

[VERIFIED] Hook SessionStart @go apareció correctamente en sesión CC del 2026-07-11: el hook inyectó el payload @go v1.1 completo (decisiones activas, pendientes, CC bootstrap, cierre @$fin) al arrancar la sesión en /opt/futbolweb. PROXIMO_AGENTE_DEBE de FutbolWeb ("verificar que el hook aparece en la próxima sesión CC") queda cumplido. Proxy dfl-context-proxy /go responde HTTP 200 en 127.0.0.1:8091. Perfil EJECUTOR confirmado vía amos-context AGENT DIRECTORY (anexo agents/ejecutor.md).

### [CIERRE] Reconciliación final Consolidación DFL v1 (7b77b78) — D-1/D-2/B-2 resueltas, hallazgo Drive nuevo
**Project:** futbolweb-app  

**What**: Reconciliación final de la Consolidación Institucional v1 (HLC 2026-07-12) COMPLETADA. Causa raíz de contradicciones: los artefactos de consolidación se construyeron sobre 06/09/obs#221, anteriores al cierre de residuales Ola 1 (docs 11/12/13, Codex 2026-07-11 noche). Hechos REVALIDADOS HOY contra realidad: (1) PAT: cero holders (0 archivos, 0 proc), remote prediccion2026 SSH sano — D-1 RESUELTA, sin fecha dura, retiro DESBLOQUEADO; (2) bundles off-host: pull rsync real desde /data/dfl-backups/engram/organ-preservation/2026-07-11-wave1/, SHA-256 idénticos los 4 — B-2 de la consolidación ERA FALSA (ruta correcta va bajo prefijo engram/); (3) SaaS Factory: DFLghub/saas-factory-setup main=5e42124=HEAD local, upstream push DISABLED — D-2 RESUELTA; (4) Drive vía conector CC: ZIP antiguo presente (524B) + HALLAZGO NUEVO 12_FutbolWeb/backups/1Password.txt (204B, 2026-07-06, fileId 1g4-4BoWbdQ0JRvggnTTFxwnjjXVASczZ) — NO LEÍDO, posible material de credenciales, requiere revisión Jorge; (5) paridad: Drive sigue única brecha material, perfil dfl-mission intacto. Artefactos corregidos: 00/01/02/04/05/06 + 08-RECONCILIACION-FINAL + HANDOFF-CODEX nuevos; registro-vivo.json actualizado. Verificador post-commit: solo 5 residuales reales (SIN-PUSH prediccion2026; SIN-RESPALDO engram-mcp/futbolweb-v2/mercader-comisiones/roof-issues-mini). Commit 7b77b78 pusheado. Nota: futbolweb recibió 2 commits de Codex producto (5595c24, e55d2c5) durante la sesión — no tocado.
**Why**: Mandato HLC — eliminar pendientes obsoletos antes de cerrar la Consolidación v1.
**Where**: /opt/dfl-knowledge/audits/consolidacion-institucional-dfl-v1/ (08-, HANDOFF-CODEX, EVIDENCE/reconciliacion-*), governance/registro-vivo/registro-vivo.json
**Learned**: Regla de método: antes de declarar pendiente en el registro vivo, contrastar contra el ÚLTIMO doc de cierre del expediente Y contra realidad ejecutable. Pendientes de Jorge tras reconciliación: retiros B-5 (desbloqueados), D-4 copias únicas, D-5 ZIP (CC ejecuta a la orden), revisión 1Password.txt, B-3 repos manuales, B-1 Drive-Codex.

### [VERIFIED] /go pending filter — resolved/stale cleanup
**Project:** dfl  

**What**: /opt/dfl-context-proxy/main.py ahora excluye de pending observaciones con título [RESOLVED] o [STALE], y aplica _is_archived(obs) — consistente con el loop de decisions/constraints.

**Why**: Engram #14 (incidente FW 2026-06-19, RESOLVED) seguía apareciendo en pending porque el loop no tenía filtros de estado. El query "pendientes" matchea cualquier obs que mencione la palabra, sin importar si está resuelta.

**Where**: /opt/dfl-context-proxy/main.py — loop pending en _handle_go(), +4 líneas.

**Learned**: El patrón [RESOLVED]/[STALE] como prefijo de título es suficiente para filtrar sin tocar el schema de Engram. _is_archived() ya existía pero no se aplicaba al loop de pending — ahora es consistente en los tres loops (decisions, constraints, pending). Engram #14 ya no aparece como pending activo. recent_decisions permanece intacto.

### KNL v1.0 contrato operativo validado
**Project:** dfl  

TOPIC: dfl/knl/v1
TYPE: decision
STATUS: active
DATE: 2026-06-28
SUMMARY: KNL v1.0 queda operativo como contrato oficial en /go. knl.json valida schema dfl.knl.v1 con semantic communities/entropy, navigation neighbors, memory, policy, provenance, comparator y validation. graph_context no aparece en /go. knl_compare.py ahora soporta snapshots previos con links y genera comparator status changed con previous_available=true. dfl-nav --brief muestra neighbors. P0/P4 quedan pendientes de confirmacion: regen_graph.sh aun usa OPENAI_API_KEY y graphify como productor; contrato KNL requiere ag_topologo.py como productor canonico de graph.json y Graphify solo como consumidor/analisador. P3 gap: ag_topologo local declara v0.1; no se encontro v0.3 instalable.
EVIDENCE: python3 /opt/dfl-context-proxy/tests/test_knl_contract.py => knl contract ok. Public /go has knl=true, graph_context=false, validation ok.

### Session summary: dfl-knowledge
**Project:** dfl-knowledge  

## Goal
Segunda fase de la sesión 2026-07-10 (post @$fin anterior): completar pendientes B3 de Codex con herramientas de La Garra + renombrado contractual A1.

## Accomplished
- 360eventos verificado con acceso directo: npm install limpio, lint/typecheck/test pasan con cero errores (confirma commit 6a86b5b de Codex). Nota: test es alias de typecheck, no hay suite real.
- graphify-out/ a .gitignore con des-trackeo completo (126 archivos, git rm -r --cached): commit 4745376 pusheado. Cierra mitad "drift" de F4. Disco intacto, KNL/proxy leen filesystem.
- tdf-01 remote POSPUESTO por Jorge: repos candidatos no existen bajo DFLghub, sin gh CLI ni token GitHub en secrets. /opt/nq-factory sigue local (HEAD 011bab1).
- Renombrado contractual A1: AUDIT_HEALTH_V1.md→INVENTARIO-A1.md, EVIDENCE.md→EVIDENCIAS-A1.md, cross-refs actualizadas, commit c464578 pusheado (primera vez versionados). Para downstream B1.
- OBS: #208 (CODEX_B3_COMPLETADA), #209 (renombrado).

## Next Steps
- B1 consolidación consume INVENTARIO-A1.md + EVIDENCIAS-A1.md.
- tdf-01: cuando Jorge cree el repo o dé URL → remote add + push (OBS #208).
- Abiertos del audit: F5-F8 + mitad comparator de F4.
- Domingo 2026-07-12 3am UTC: primera corrida dominical del metabolismo sin CRON 2 — revisar logs.
- Untracked restantes: MISION_A1.md, crontab-backup-1783708852.txt.

## Relevant Files
/opt/dfl-knowledge/audits/health-v1/INVENTARIO-A1.md, EVIDENCIAS-A1.md, /opt/dfl-knowledge/.gitignore, /opt/360eventos/package.json, /opt/nq-factory

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

### Codex @$fin 2026-07-25 — dogfood WORK_UNIT claims pilot completado
**Type:** fact  
**Project:** dfl  

**What**: Sesión cerrada con `@$fin` tras dos frentes. (1) Revisión focal de WORK_UNIT en `/opt/dfl-knowledge-workunit`: rama `feat/dfl-concierge-workunit-claims`, HEAD real `d909147318003b3b79dc25f554fd8059fc2df140`; findings HIGH 1 / HIGH 2 / MEDIUM verificados como CLOSED; suite focal `python3 -m unittest concierge.tests.test_workunit` OK; smoke `claim -> handoff -> release` OK; `git diff --check` OK; diff `aaf740a..b899c0c` sin cambios en `concierge/register.py` ni capas AuthZ; veredicto WARN solo por desalineación de HEAD respecto de la precondición declarada. (2) Dogfooding real aislado de WORK_UNIT en worktree `/tmp/dfl-workunit-dogfood`, rama `feat/dfl-concierge-workunit-dogfood`, base `767c5e2`, commit nuevo `994c007`, push a `origin/feat/dfl-concierge-workunit-dogfood`. Se generaron: script `scripts/workunit_dogfood_pilot.py`, helper `concierge/dogfood_workunit.py`, test `concierge/tests/test_workunit_dogfood_pilot.py`, protocolo `architecture/WORK_UNIT-DOGFOOD-PROTOCOL.md`, receipt `architecture/receipts/CP-F1-WORKUNIT-DOGFOOD-01.md`, ledger real `evidence/workunit-dogfood-pilot/ledger/work_units.jsonl`, summary `evidence/workunit-dogfood-pilot/pilot-summary.json`.\n\n**Evidence**: `python3 -m unittest concierge.tests.test_workunit concierge.tests.test_workunit_dogfood_pilot` => 18 tests OK; `python3 scripts/workunit_dogfood_pilot.py --branch feat/dfl-concierge-workunit-dogfood --commit 767c5e2 --output-dir evidence/workunit-dogfood-pilot` => PASS; ledger real con secuencia `CLAIMED -> HANDED_OFF -> RELEASED` para `dogfood.claims.protocol` y `CLAIMED -> EXPIRED -> CLAIMED` para `dogfood.claims.expiry`; `git diff --check` limpio; `git push origin feat/dfl-concierge-workunit-dogfood` exitoso.\n\n**Protected surfaces respected**: sin cambios en `main`, `AuthZ`, `register.py`, `scheduler`, `UI`, `NO_TOUCH`, Supabase, Vercel, env vars ni scoring protegido.\n\n**Outcome**: dogfood verdict `DOGFOOD_PASS`; cierre ordenado listo para mirror.

### Diagnóstico @$go 2026-07-25 — /go pending expone observaciones cerradas con prefijos [VERIFIED]/[CIERRE]
**Type:** bugfix  
**Project:** dfl  

**What**: Durante `@$go` con payload local `generated_at=2026-07-25T03:37:43Z`, el primer `pending` activo resultó ser una observación ya cumplida (`[VERIFIED] Hook SessionStart @go operativo...`). La inspección del god node `context-proxy` mostró que `/opt/dfl-context-proxy/main.py` en `ProxyHandler._handle_go()` filtra `pending` por prefijos `[RESOLVED]`, `[STALE]` y `[PARCIALMENTE RESUELTO]`, además de `LIFECYCLE: archived`, pero no excluye títulos cerrados con prefijos `[VERIFIED]` ni `[CIERRE]`.

**Why**: Esto hace que `/go` siga exponiendo como `pending` observaciones semánticamente cerradas, degradando el onboarding al priorizar trabajo ya resuelto.

**Evidence**:
- Payload hook local: primer pending = `[VERIFIED] Hook SessionStart @go operativo en sesión CC 2026-07-11 — PROXIMO_AGENTE_DEBE FutbolWeb cumplido`.
- `dfl-context-proxy.main.ProxyHandler._handle_go` contiene `if title.startswith("[RESOLVED]") or title.startswith("[STALE]") or title.startswith("[PARCIALMENTE RESUELTO]"):`.
- `_is_archived()` solo respeta `LIFECYCLE: archived` en contenido.

**Status**: diagnóstico persistido; no se aplicó remediación porque esta sesión no escribió fuera de `/opt/dfl-knowledge`.

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

**Graph entropy:** 0.884  

- **Community 11** (96 nodes): MCP Server Behavior, Evaluación de Plantillas, Preguntas para el Desarrollador
- **Community 0** (4 nodes): Express Framework, Bicep, RON (Rusty Object Notation)
- **Community 1** (4 nodes): JSON Schema
- **Community 2** (4 nodes): agLego-PTE-001
- **Community 3** (4 nodes): Preocupaciones operativas
- **Community 4** (4 nodes): amOS, IAIM, ag10

---

*Mirror auto-generated 2026-07-25T03:51:16Z | La Garra → DFLghub/amos-context*
