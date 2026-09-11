# amOS Context — @$go Live Mirror
**Generated:** 2026-09-11T16:52:23Z  
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

## PROVISIONAL ROUTING GATE

**Authority:** `DFL_BOOTSTRAP.pending`  
**Decision:** `FAIL_CLOSED`  
**State version:** `UNKNOWN`  
**State SHA:** `UNKNOWN`  
**Freshness:** `UNKNOWN`  
**Contradictions:** `[]`  
**Authorized actions:** `[]`  
**Blocked actions:** `['ALL_ACTIONS']`  

**FAIL_CLOSED:** no mission selection or operational action is authorized.

---

## RECENT DECISIONS

### Session summary: root
**Type:** session_summary  
**Project:** root  

Goal: Cerrar P11 (loops anidados → graph → BOS / autonomía verificable) sin abrir trabajo nuevo; consolidar QUIERO soberano y evidencia empírica de dos rondas de cierre de falsos supuestos.

Discoveries: QUIERO soberano congelado: max A_v s.a. C_v≥C_min(A_v) [creciente, convexa, con techo], V≥V_min, Authority∈Gates, ΔR/ΔC_min(·)/CalibrationCadence(F) owner-protected, H_L(F) liveness automatizable vs Calibration(F) periódica humana no-recursiva. Principio: MÁS AUTONOMÍA → MÁS EVIDENCIA. Empírico: session-watchdog tiene falso positivo real confirmado (reapeó esta misma sesión); FutbolWeb Return confirmado real vía GitHub Actions ko-reality-sync.yml (no Vercel Cron); daily_check/wru_graph_refresh confirmado PASS en logs reales; DCSA owner-authorization-gateway ya existe y funciona (prohibited_actions:AUTOPROMOTE, expiración temporal); R1/R2 sigue INCOMPLETE (evidencia en proyecto Engram "dfl", no accesible desde "root"). "Business OS v7 de Ricardo" NO EXISTE — es una copia mal etiquetada de Hermes Command Center (cc-hermes-cc), confirmado por sus propios commits; la versión real más alta es v6 ("el agrupador").

Accomplished: Handoff completo escrito en /root/HANDOFF-P11-2026-09-02.md. Guardadas 6 observaciones Engram (ids 662-667) documentando: tesis P1-P4, QUIERO mayor, QUIERO vectorial canónico, hallazgos vuelta 1 y vuelta 2 de P11, y el descubrimiento de que Gates/Authority ya existen implementados vía DCSA. Comparación exploratoria de 3 ecosistemas "Business OS" (VM2/mercader-bos, business-os-new, business-os-v6 de Ricardo) entregada sin decisión, a pedido de Jorge.

Next Steps: Construir agregador C soberano mínimo reusando señales ya existentes (degraded de FutbolWeb, UNCHANGED/CHANGED de daily_check), colgado del cron existente, sin scheduler nuevo. Cerrar R1/R2 accediendo al proyecto Engram "dfl". No desplegar el fix de session-watchdog sin autorización explícita de Jorge.

Relevant Files: /root/HANDOFF-P11-2026-09-02.md, /opt/futbolweb/lib/{espn-world-cup,scoring-propagation,tournament-reality}.ts, /opt/futbolweb/.github/workflows/ko-reality-sync.yml, /opt/dfl-context-proxy/session-watchdog.sh, /opt/dfl-knowledge/scripts/{wru_graph_refresh.py,daily_check.sh}, /opt/dfl-knowledge/governance/dispatch/store/owner-authorization-drafts/, /opt/saas-factory-setup/mercader-bos/, /root/downloads/{business-os-new,business-os-template}

**Type:** manual  
**Project:** root  

R1/R2 (MERCADER_AUTONOMOUS_R1_R2_TCX_2026_08_19) — CERRADO, con evidencia real encontrada esta vez en /root/.engram/engram.db proyecto \"dfl\" (obs #528, #529, #532, #534; no en el proyecto \"dfl\" de dflagent, que no tenía nada de esto).\n\nVEREDICTO: la preocupación de la misión P11 vuelta 2 (\"¿una sola corrida E2E bastó para subir confianza?\") queda CONFIRMADA como válida, con evidencia, no descartada.\n\nEvidencia real (obs #534, 2026-08-19): R1 (executor automático de MERCADER_ORDER PENDING, reusa peer-work queue + activate-peer.sh) y R2 (tools/mercader-autonomy/ack_callback.py, ACK automático + update idempotente de mercader_leads.order_status) se construyeron y se demostraron con UNA prueba sintética end-to-end: lead-1787179288964-amokp → MERCADER-ORDER-EXT-BUY-2026-08-19T224128969Z → pw-a14969e5de8e COMPLETED por TCX → AQA-1 CRUD_LIFECYCLE PASS → ACK pw-1f006af20139c COMPLETED por R2 → SQLite order_status=ACKED. Más UN check adicional de idempotencia (segunda llamada al callback devolvió \"already_acked\" correctamente).\n\nEs decir: 1 corrida principal + 1 verificación de idempotencia, ejecutadas y reportadas por el mismo rol (TCX) que construyó R1/R2 — sin verificación adversarial independiente (sin inyección de fallos, sin carga concurrente, sin un TCX/TCC distinto re-probando). Esto es exactamente el patrón de riesgo \"self-attested, no Falsification_PASS\" del marco Av/Cv de esta sesión.\n\nCONCLUSIÓN PARA A_v: R1/R2 NO debe promoverse a FORMAL ni contar como A_v alto todavía — correcto mantenerlo en su banda actual (evidencia básica/operacional, no adversarial). No es que falte evidencia (ya no es INCOMPLETE por falta de acceso) — es que la evidencia que existe es de un solo tipo (una corrida feliz + un retry), insuficiente para el nivel de autonomía que R1/R2 ya está ejerciendo en producción (MERCADER real).\n\nContexto adicional (obs #532, mismo día, anterior a R1/R2): antes de esta misión, R1 y R2 eran pasos MANUALES (TCC ejecutaba a mano, UPDATE manual de SQLite) — R1/R2 se construyeron específicamente para eliminar esa intervención manual, siguiendo la regla de Jorge \"reutilizar todo lo existente, construir solo el delta que el E2E demuestre necesario\".\n\nRecomendación para TCX en la próxima vuelta: probar R1/R2 con inyección de fallos (AQA DENY, peer-work timeout, dos leads BUY concurrentes para el mismo cliente) antes de considerar subir su nivel de A_v.

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

---

## PENDING


---

## RECENT ACTIVITY (cross-project)

### Session summary: root
**Type:** session_summary  
**Project:** root  

Goal: Cerrar P11 (loops anidados → graph → BOS / autonomía verificable) sin abrir trabajo nuevo; consolidar QUIERO soberano y evidencia empírica de dos rondas de cierre de falsos supuestos.

Discoveries: QUIERO soberano congelado: max A_v s.a. C_v≥C_min(A_v) [creciente, convexa, con techo], V≥V_min, Authority∈Gates, ΔR/ΔC_min(·)/CalibrationCadence(F) owner-protected, H_L(F) liveness automatizable vs Calibration(F) periódica humana no-recursiva. Principio: MÁS AUTONOMÍA → MÁS EVIDENCIA. Empírico: session-watchdog tiene falso positivo real confirmado (reapeó esta misma sesión); FutbolWeb Return confirmado real vía GitHub Actions ko-reality-sync.yml (no Vercel Cron); daily_check/wru_graph_refresh confirmado PASS en logs reales; DCSA owner-authorization-gateway ya existe y funciona (prohibited_actions:AUTOPROMOTE, expiración temporal); R1/R2 sigue INCOMPLETE (evidencia en proyecto Engram "dfl", no accesible desde "root"). "Business OS v7 de Ricardo" NO EXISTE — es una copia mal etiquetada de Hermes Command Center (cc-hermes-cc), confirmado por sus propios commits; la versión real más alta es v6 ("el agrupador").

Accomplished: Handoff completo escrito en /root/HANDOFF-P11-2026-09-02.md. Guardadas 6 observaciones Engram (ids 662-667) documentando: tesis P1-P4, QUIERO mayor, QUIERO vectorial canónico, hallazgos vuelta 1 y vuelta 2 de P11, y el descubrimiento de que Gates/Authority ya existen implementados vía DCSA. Comparación exploratoria de 3 ecosistemas "Business OS" (VM2/mercader-bos, business-os-new, business-os-v6 de Ricardo) entregada sin decisión, a pedido de Jorge.

Next Steps: Construir agregador C soberano mínimo reusando señales ya existentes (degraded de FutbolWeb, UNCHANGED/CHANGED de daily_check), colgado del cron existente, sin scheduler nuevo. Cerrar R1/R2 accediendo al proyecto Engram "dfl". No desplegar el fix de session-watchdog sin autorización explícita de Jorge.

Relevant Files: /root/HANDOFF-P11-2026-09-02.md, /opt/futbolweb/lib/{espn-world-cup,scoring-propagation,tournament-reality}.ts, /opt/futbolweb/.github/workflows/ko-reality-sync.yml, /opt/dfl-context-proxy/session-watchdog.sh, /opt/dfl-knowledge/scripts/{wru_graph_refresh.py,daily_check.sh}, /opt/dfl-knowledge/governance/dispatch/store/owner-authorization-drafts/, /opt/saas-factory-setup/mercader-bos/, /root/downloads/{business-os-new,business-os-template}

**Type:** manual  
**Project:** root  

P11 vuelta 2 (TCC cierra falsos supuestos, 2026-09-02) — resultados verificados:\n\n1. VERCEL CRON para /api/tournament-reality/sync: CONFIRMADO AUSENTE. `vercel crons ls --project futbolweb-app` (CLI autenticada como dflghub, solo lectura) devolvió \"No cron jobs found for dflghubs-projects/futbolweb-app\". El supuesto anterior (\"puede que Vercel Cron lo dispare\") queda descartado.\n\n2. Return real de FutbolWeb identificado: `.github/workflows/ko-reality-sync.yml` (GitHub Actions, repo DFLghub/futbolweb-app). Contiene decenas de ventanas cron específicas por partido (todas fechadas jun-jul 2026, ya pasadas) MÁS una reconciliación rodante sin restricción de fecha: `15 */3 * * *` (cada 3h, todo el año 2026). El job siempre llama a `/api/tournament-reality/sync` con CRON_SECRET real vía curl, sin importar cuál entrada de cron disparó. Conclusión: el Return SÍ existe y sigue activo hoy (vía la reconciliación rodante cada 3h), aunque las ventanas de alta densidad específicas por partido ya expiraron (correcto, el torneo terminó). No se pudo confirmar historial real de ejecuciones (gh CLI no instalado, no se buscaron credenciales) — el diseño está verificado por archivo, no por logs de ejecución real. Marca: PASS con evidencia de diseño, NO PASS con evidencia de ejecución histórica (sigue abierto para TCX).\n\n3. session-watchdog.sh — propuesta de fix (NO desplegada, solo diseñada, pendiente autorización de Jorge): (a) subir STALE_SECONDS de 600s a ~1800s; (b) exigir 2 lecturas consecutivas de staleness antes de reap (separa sospecha de acción, ~3min de confirmación extra); (c) reconocer explícitamente que para sesiones CC no existe ninguna señal positiva de muerte (no hay PID expuesto, cc-heartbeat-hook.sh solo toca un archivo por session_id; SessionEnd solo cubre salidas limpias) — esto es un límite estructural real, no resoluble con ajustes locales, y queda INCOMPLETE.\n\n4. R1/R2 MERCADER_AUTONOMOUS_R1_R2_TCX_2026_08_19 (revisión de si una sola corrida E2E bastó para subir confianza): INCOMPLETE — la evidencia de validación real vive en observaciones Engram del proyecto \"dfl\" (no \"root\"), no accesible desde el mem_search de esta sesión/proyecto. No se puede afirmar ni descartar sobre-confianza sin esa auditoría. Requiere sesión/acceso al proyecto Engram \"dfl\".\n\n5. Hallazgo adicional confirmado: el listado NO_TOUCH/restricciones tiene una única fuente canónica real (`/opt/dfl-context-proxy/main.py` líneas ~578/723) — las ~100 coincidencias de grep son capturas históricas de /go, no copias mantenidas. No hace falta consolidar nada ahí.

**Type:** manual  
**Project:** root  

R1/R2 (MERCADER_AUTONOMOUS_R1_R2_TCX_2026_08_19) — CERRADO, con evidencia real encontrada esta vez en /root/.engram/engram.db proyecto \"dfl\" (obs #528, #529, #532, #534; no en el proyecto \"dfl\" de dflagent, que no tenía nada de esto).\n\nVEREDICTO: la preocupación de la misión P11 vuelta 2 (\"¿una sola corrida E2E bastó para subir confianza?\") queda CONFIRMADA como válida, con evidencia, no descartada.\n\nEvidencia real (obs #534, 2026-08-19): R1 (executor automático de MERCADER_ORDER PENDING, reusa peer-work queue + activate-peer.sh) y R2 (tools/mercader-autonomy/ack_callback.py, ACK automático + update idempotente de mercader_leads.order_status) se construyeron y se demostraron con UNA prueba sintética end-to-end: lead-1787179288964-amokp → MERCADER-ORDER-EXT-BUY-2026-08-19T224128969Z → pw-a14969e5de8e COMPLETED por TCX → AQA-1 CRUD_LIFECYCLE PASS → ACK pw-1f006af20139c COMPLETED por R2 → SQLite order_status=ACKED. Más UN check adicional de idempotencia (segunda llamada al callback devolvió \"already_acked\" correctamente).\n\nEs decir: 1 corrida principal + 1 verificación de idempotencia, ejecutadas y reportadas por el mismo rol (TCX) que construyó R1/R2 — sin verificación adversarial independiente (sin inyección de fallos, sin carga concurrente, sin un TCX/TCC distinto re-probando). Esto es exactamente el patrón de riesgo \"self-attested, no Falsification_PASS\" del marco Av/Cv de esta sesión.\n\nCONCLUSIÓN PARA A_v: R1/R2 NO debe promoverse a FORMAL ni contar como A_v alto todavía — correcto mantenerlo en su banda actual (evidencia básica/operacional, no adversarial). No es que falte evidencia (ya no es INCOMPLETE por falta de acceso) — es que la evidencia que existe es de un solo tipo (una corrida feliz + un retry), insuficiente para el nivel de autonomía que R1/R2 ya está ejerciendo en producción (MERCADER real).\n\nContexto adicional (obs #532, mismo día, anterior a R1/R2): antes de esta misión, R1 y R2 eran pasos MANUALES (TCC ejecutaba a mano, UPDATE manual de SQLite) — R1/R2 se construyeron específicamente para eliminar esa intervención manual, siguiendo la regla de Jorge \"reutilizar todo lo existente, construir solo el delta que el E2E demuestre necesario\".\n\nRecomendación para TCX en la próxima vuelta: probar R1/R2 con inyección de fallos (AQA DENY, peer-work timeout, dos leads BUY concurrentes para el mismo cliente) antes de considerar subir su nivel de A_v.

**Type:** manual  
**Project:** root  

HALLAZGO CLAVE (P11 v2, 2026-09-02): el framework Gates/Authority/ΔR-approval del QUIERO vectorial (ver dfl/thesis/quiero-vectorial-canonico) YA EXISTE parcialmente implementado en producción, no es solo teoría — no hay que construirlo desde cero:\n\n- **DCSA owner-authorization-gateway** (`/opt/dfl-knowledge/governance/dispatch/store/owner-authorization-drafts/*.json`, schema `dfl.dcsa.owner-authorization-gateway-draft.v1`): mecanismo real de ampliar/restringir el scope de autoridad de una misión (ej. widen-MERCADER_AUTONOMOUS_R1_R2_TCX_2026_08_19). Cada draft tiene: reason, old_target/new_target/added, previous_expires_at/new_expires_at (autoridad con EXPIRACIÓN por tiempo, no permanente), renewal_count, provenance.selected_by (\"Jorge_direct_authorization\"), amendments (log de cambios). Las misiones llevan allowed_actions/prohibited_actions explícitos — ej. `prohibited_actions: [\"AUTOPROMOTE\"]`, i.e. una misión tiene prohibido auto-ampliar su propia autoridad. Esto es una instancia real y ya probada de \"Authority ∈ Gates\" + \"ΔAuthority ⇒ Approval(R_owner)\".\n- **provisional-routing-state.json** (`/opt/dfl-knowledge/governance/onboarding/`, schema `dfl.onboarding.provisional-routing.v1`): es la fuente real del \"PROVISIONAL ROUTING GATE / FAIL_CLOSED\" que aparece en cada /go — lista misiones `pending` con executor, target repos, policy, status, y freshness con expiración (`max_age_seconds`). Confirma que el gate FAIL_CLOSED que vi al inicio de esta sesión es el estado *default* cuando ninguna misión pending coincide con el executor/sesión actual — no un bug ni ambigüedad, es el diseño esperado (fail-closed por defecto, opt-in explícito por misión).\n- Fuente canónica única del texto NO_TOUCH/restricciones: `/opt/dfl-context-proxy/main.py` (líneas ~578 y ~723) — todo lo demás que grep encuentra (~100 archivos) son capturas/logs históricos de respuestas /go pasadas, NO copias mantenidas por separado. No hace falta consolidar nada — ya está consolidado en una sola fuente; la aparente duplicación es solo artefacto de logging, no un riesgo de drift real.\n\nImplicación para cualquier implementación futura de Av/Cv/Gates: reutilizar DCSA + provisional-routing-state como la capa de Gates/Authority, no construir un registro nuevo. El TCX ya existe como rol ejecutor con expiración y prohibición explícita de autopromoción — es la base real sobre la que colgar Av (autonomía verificable) sin inventar framework nuevo.

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

**Graph entropy:** 0.7141  

- **Community 11** (95 nodes): PRP como artefacto nativo, Modelo de disponibilidad en servicios digitales, Complejidad en la evaluación de costos
- **Community 0** (7 nodes): Verificación de API, Estrategia PRP, Riesgos de implementación
- **Community 1** (5 nodes): Jurisdicción, Mercader, Observación de Ed
- **Community 2** (4 nodes): MCP Server Behavior, RLS Trap, Cardinalidad de Inventario
- **Community 4** (4 nodes): Owner-Based RLS, Mercader Boundary
- **Community 3** (4 nodes): Merchant of Record, Métricas comerciales, Integraciones externas

---

*Mirror auto-generated 2026-09-11T16:52:23Z | La Garra → DFLghub/amos-context*
