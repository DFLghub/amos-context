# amOS Context — @$go Live Mirror
**Generated:** 2026-08-05T22:24:02Z  
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
**State version:** `dfl.onboarding.provisional-routing.v1`  
**State SHA:** `b2bcfa79c24950fb0e130a67f66375a61893afe3a5914cd7dcff51f0743cf5cc`  
**Freshness:** `UNKNOWN`  
**Contradictions:** `['bootstrap_stale', 'E_AUTH_EXPIRED', 'E_DISPATCH_STALE']`  
**Authorized actions:** `[]`  
**Blocked actions:** `['ALL_ACTIONS']`  

**FAIL_CLOSED:** no mission selection or operational action is authorized.

---

## RECENT DECISIONS

### [CONVERGENCIA] El Gerente de Fabrica (FMD, #280) ya nombro los dos gaps del discovery SFV5 14 dias antes; el discovery no lo cito nunca
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/saas-factory/gap-convergence-fmd-sfv5
TYPE: decision
STATUS: open
DATE: 2026-08-04
PRECEDENCIA: D
AUTHORITY: evidence only
LIFECYCLE: active
CONFIDENCE: high

ENCUADRE DECLARADO POR JORGE: Claude Code y Codex son quienes RECIBEN los pedidos de fabricacion y los gestionan con el "gerente de la fabrica". No somos auditores externos de SFV5: somos su runtime. El discovery #460/#462 ya lo habia probado tecnicamente sin sacar la consecuencia: la fabricacion general de SFV5 ES una sesion de Claude Code leyendo prosa.

HALLAZGO: EL DISCOVERY TUVO UN PUNTO CIEGO REAL. Auditó SFV5 en aislamiento y nunca pregunto si DFL ya tenia disenado el orquestador faltante. Lo tenia, desde hacia 14 dias. Verificado por grep: evidence/sfv5-internal-factory-reality-2026-08-04/ tiene CERO menciones de "gerente", "first-operable", "factory_manager" o "management_daemon". El enlace es de una sola direccion: el diseno del Gerente SI conocia SFV5 (7 menciones en MANAGEMENT_DAEMON_SPEC, 6 en EVIDENCE_BASE, 6 en FACTORY_BUILD_MISSION_PACKET); el discovery no conocia el Gerente.

EL GERENTE EXISTE COMO DISENO FORMAL: obs #280, 2026-07-21, /opt/dfl-knowledge/architecture/first-operable-factory-v01/, 12 documentos, 788 lineas, diseno puro sin codigo. Piezas: FACTORY_MANAGER_CONTRACT_V0.1.md (management loop de 10 pasos interpretar->planificar->asignar->ejecutar->observar->detectar desvio->replanificar->validar->cerrar->aprender; tabla de autoridad de 4 niveles; politica de excepcion graduada N0-N3 que extiende SILENT_CRON_JOBS) y MANAGEMENT_DAEMON_SPEC_V0.1.md (FMD = factory-manager-daemon, se sienta sobre el contrato AGENT-SERVER.md de BOS v2 sin reemplazarlo).

CONVERGENCIA, EL HALLAZGO CENTRAL: dos misiones independientes, con 14 dias de diferencia y por caminos distintos, derivaron EL MISMO PAR DE GAPS.
- FMD 2026-07-21, MANAGEMENT_DAEMON_SPEC linea 45: "Adaptador hacia SFV5 - AUSENTE hoy (EVIDENCE_BASE #34), debe construirse". Y linea 127, fuera de alcance explicito: "Validacion semantica de calidad del entregable (no solo verificacion de estado)".
- Discovery 2026-08-04: falta (a) superficie de invocacion no interactiva y (b) verificador de correspondencia pedido->producto.
Son el mismo par. El gap deja de ser hipotesis de una sola mision.

ASIMETRIA IMPORTANTE ENTRE (a) Y (b): la (a) YA TIENE CONTRATO ESCRITO — el FMD especifica que debe hacer el invocador, con autoridad y excepciones. La (b) SIGUE SIN DUENO EN AMBOS DISENOS: el FMD la declaro fuera de alcance y SFV5 no tiene ningun precedente de comparar producto contra pedido. Es la unica pieza sin ancestro en el sistema.

LA RECURSION QUE NADIE HABIA NOMBRADO: MANAGEMENT_DAEMON_SPEC linea 21 dice que el FMD "No escribe codigo de producto - invoca a SFV5 (u otra capacidad productiva) para eso". Pero el discovery probo que la capacidad productiva de SFV5 no es un binario: es una sesion interactiva de Claude Code. Por lo tanto el "Adaptador hacia SFV5" es, literalmente, un adaptador hacia una sesion como la mia. El FMD no invoca una fabrica: invoca un agente que sabe leer 32 manuales.

CORRECCION AL "REUTILIZABLE INTACTO" DE #460/#462: faltaba el activo mas grande. Sumar first-operable-factory-v01/ completo — contrato de autoridad, politica de excepcion N0-N3, management loop de 10 pasos y el modelo goals/plans/success_criteria. El adaptador headless NO es pieza nueva: es la implementacion del "Adaptador hacia SFV5" ya especificado. No inventar otro contrato de autoridad.

PROXIMO_AGENTE_DEBE: antes de abrir cualquier mision sobre SFV5, el headless gap o la fabrica, leer first-operable-factory-v01/ — sobre todo EVIDENCE_BASE.md (#34 adaptador ausente, #13 goals ausente, #11 tasks.status sin gate, #15 replanificacion ausente, #17 plasticidad aspiracional) y MANAGEMENT_DAEMON_SPEC_V0.1.md. Y buscar diseno previo en architecture/ y en Engram ANTES de declarar que algo no existe: el discovery no lo hizo y por eso le falto el activo principal.

### [ADDENDUM] SFV5 discovery respondido pregunta-por-pregunta: existe routing y pipeline DOCUMENTADOS (27/32 y 16 pasos), falta el motor
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/saas-factory/internal-factory-reality-discovery
TYPE: decision
STATUS: closed
DATE: 2026-08-04
PRECEDENCIA: D
AUTHORITY: evidence only
LIFECYCLE: active
CONFIDENCE: high

AMPLIA obs #460 (mismo TOPIC). Commit ceec81f, evidence/sfv5-internal-factory-reality-2026-08-04/ADDENDUM-RESPUESTAS-PREGUNTA-POR-PREGUNTA.md, sha256sum -c 24/24 OK. Read-only, cero modificaciones a SFV5, sin uso de root.

MOTIVO: el informe de #460 respondia por tema, no por numero. Jorge reenvio el prompt senalando que faltaban respuestas. Faltaban de verdad: seccion A enumerada, inventario Q11 renderizado, seccion C completa (Q22-Q30, AUSENTE ENTERA) y tabla por arista de I (Q83-Q86). Ahora estan las 87 numeradas + las 14 de J.

TRES CORRECCIONES A #460, todas en la misma direccion: el informe SUBESTIMO lo que SFV5 tiene escrito.

C1. "cero tabla de routing" es FALSO. saas-factory/CLAUDE.md:48-140 contiene un Decision Tree explicito de 27 ramas frase-gatillo -> skill. Medido: 26 por regex 'Ejecutar skill X' + bucle-agentico como segundo paso de la rama PRP en CLAUDE.md:72. 27/32 ruteadas; 5 FUERA del arbol, invisibles al routing documentado: pack-cold-email, primer, skill-creator, update-sf, video-visuals. Rama terminal: "No encaja en nada -> Usar tu juicio." Formulacion correcta: existe tabla de routing DOCUMENTED y casi completa; lo que no existe es un router IMPLEMENTED. Nada la parsea, nada la impone, nada falla si se la ignora.

C2. "no existe maquina de estados/pipeline explicito" es FALSO. CLAUDE.md:184-260 define 5 flujos numerados; Flujo 1 tiene 16 pasos ordenados (0 factory-brain, 1 new-app, 3 i18n, 4 add-login ... 10 quality-gates, 12 deploy, 13 outcomes, 16 factory-brain). Es una maquina de estados DE PAPEL: sin estado persistido, sin gate bloqueante, sin ejecutor. Prueba empirica: roof-issues-mini, el unico producto real, NO siguio Flujo 1 sino Flujo 2 (prp -> bucle-agentico), y su src/ no tiene i18n, compliance, onboarding ni outcomes.

C3. CLAUDE.md:140 declara "30 Herramientas Especializadas"; el filesystem canonico tiene 32.

HALLAZGOS NUEVOS DEL RESPALDO DE ROOF-ISSUES-MINI (roof-issues-mini-source.tar.gz, 212 archivos, NO desempaquetado en la pasada previa):
a) roof-issues-mini/.claude/settings.local.json declara los permisos que hicieron funcionar la fabrica: Bash(npm install *), Bash(npm run *), Bash(npx next *), Bash(npx eslint *) + enabledMcpjsonServers [next-devtools, playwright, supabase]. NO ESTA EN CANONICO. La configuracion que hizo funcionar la fabrica vive en un archivo por-maquina, no versionado.
b) roof-issues-mini/.claude/logs/ NO EXISTE. El hook log-tool-usage.sh no solo carece de settings.json en canonico: tampoco corrio en el unico producto real. Es la causa mecanica probada de que 29/32 skills no tengan ninguna evidencia de ejecucion.
c) Los 6 criterios de exito de PRP-001:25-31 quedan verbatim como ground truth del experimento minimo.
d) 12 archivos de dominio en src/, correspondientes 1:1 con las 6 fases del Blueprint.

SECCION C, RUNTIME REAL, respondida por primera vez. Q23, lo que ejecuta Claude Code y ningun binario: seleccion de skill, la entrevista adaptativa, la DESCOMPOSICION JIT EN SUBTAREAS (bucle-agentico: "Solo se definen FASES. Las subtareas se generan al entrar a cada fase"), la generacion de codigo y el juicio de los 6 gates sin runner. Q27: cero pins de modelo; las menciones a anthropic son nombres de paquete MCP. Q28: no hay contrato cross-agente, y la evidencia decisiva es que GEMINI.md, unico archivo nominalmente para otro agente, esta stale en V3, referencia .claude/prompts/ y .claude/commands/ inexistentes, y su pie dice "Este archivo es para que Claude Code entienda el repositorio". Q30: siete suposiciones ocultas verificadas (cwd raiz Next.js, alias shell saas-factory, estructura feature-first obligatoria, settings.local.json no versionado, env vars, placeholders MCP sin completar, ownership root que hace irreproducible quick_validate 32/32 como dflagent).

NEGATIVOS HEADLESS RE-VERIFICADOS sobre 9b18947 con falsos positivos descartados uno por uno: los 2 hits de 'headless' son "UI headless" (diseno de UI) en skills/ai/references/; los 2 de @anthropic-ai son nombres de paquete MCP en CHANGELOG y example.mcp.json; el unico child_process es git rev-parse HEAD en el bridge. Cero .claude/commands, .claude/agents, CI, Dockerfile, Makefile, settings.json.

Q21 queda con cifra final: 0/32 tests, 3/32 camino vivo evidenciado (prp, bucle-agentico, skill-creator), 29/32 sin ninguna evidencia de ejecucion.

VEREDICTO SOSTENIDO Y REFORZADO: SFV5_AGENTIC_FACTORY_LIVE_PROGRAMMATIC_ENTRY_GAP. Las correcciones no lo debilitan, lo afilan: al existir routing documentado (27/32) y pipeline documentado (16 pasos), el gap toma su forma exacta. SFV5 TIENE EL MAPA DE ORQUESTACION ESCRITO Y NO TIENE EL MOTOR QUE LO RECORRA. Lo que falta no es disenar el flujo, ya esta disenado en CLAUDE.md; falta la puerta de invocacion y el verificador semantico.

REUTILIZABLE QUE #460 NO LISTABA: el decision tree de CLAUDE.md como especificacion de routing YA ESCRITA, insumo directo del adaptador headless.

PROXIMO_AGENTE_DEBE: ejecutar SFV5_HEADLESS_ORDER_TO_PRODUCT_MINIMAL_PROOF con los 6 criterios de PRP-001 como aserciones ejecutables. Criterio >=5/6 criterios DEL PEDIDO en PASS. Contra-criterio explicito: falla si reporta PASS solo porque la ejecucion termino sin error.

**Type:** decision  
**Project:** dfl-knowledge  

## PATCH_RISK canonical policy preregistered (2026-08-01)

**What**: Created `evidence/patch-risk-policy-preregistration-2026-08-01/` — PATCH-RISK-POLICY.md + .json, two JSON Schemas (policy shape + touchpoint record), a pure `classify()`/`redTriggers()` node lib, 14/14 green `node --test` tests, SHA256SUMS, and a preregistration receipt. Committed exclusively as `6f71e5e` on `feat/dfl-high-certainty-harness-v0.1`, baseline `d79fffdf4ab1739e45049bae9c3933794788c1df`.

**Policy content**: 5 canonical touchpoint classes (INSTITUTIONAL_EXTERNAL, ADDITIVE_EXTRA, BOUNDARY_ADAPTER, INTERNAL_PATCH, UPSTREAM_CANDIDATE) classifying coupling between SF upstream, Factory Extras and the future Docking System. 8 mandatory per-touchpoint factors (DEPTH, VERSION_SENSITIVITY, CRITICALITY, REAPPLY_EFFORT, AUTOMATION_RISK, AUTO_ATTACH_FEASIBLE, AUTO_DETACH_FEASIBLE, ABORT_ON_DRIFT) + 4 derived fields (requires_exact_preconditions, has_stable_hook, rollback_byte_identical, manual_intervention_permanent). GREEN/YELLOW/RED verdict per user-specified rules, with 5 single-factor RED triggers (R1 critical patch w/o stable hook, R2 >2 internal patches, R3 permanent manual intervention, R4 can't abort on drift, R5 rollback not byte-identical) plus a conservative RED-by-default fallback for anything not explicitly GREEN/YELLOW (e.g. a GOLDEN_PATH_CRITICAL internal patch with a stable hook still fails YELLOW's explicit exclusion of orchestration/golden-path criticality).

**Why**: User explicitly required this preregistered *before* any probe (DOCK_OBSERVATION_RUN, JPI, DCSA, Docking System) so future touchpoint classification is deterministic and auditable rather than judged ad hoc during a live run.

**Scope discipline**: Did NOT execute DOCK_OBSERVATION_RUN/JPI/DCSA/Docking System, did NOT modify the sfv5-headless-profile-candidate-2026-08-01 candidate, did NOT promote anything — matches user's explicit non-goals.

**Convention confirmed**: This repo's JSON-schema-validated artifacts (schemas/*.schema.json, draft 2020-12) are validated by hand-rolled JS logic, not ajv (no package.json/npm deps in repo) — tests do manual required-key/enum checks instead of a schema-validation library. Node test convention is `node --test tests/*.test.mjs`, `.mjs` ESM files, evidence dirs named `evidence/<slug>-<date>/` with SHA256SUMS at the root and a receipts/ subfolder for gate receipts.

VEREDICTO: DFL_PATCH_RISK_POLICY_PREREGISTERED

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

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

### [SFV5] Entrevista canonica completada: la fabrica se autodescribe, mecanismo de interrogacion resuelto y 3 hallazgos criticos verificados
**Type:** fact  
**Project:** dfl  

TOPIC: dfl/saas-factory/canonical-self-description
TYPE: fact
STATUS: closed
DATE: 2026-08-05
PRECEDENCIA: D
AUTHORITY: evidence only
LIFECYCLE: active
CONFIDENCE: high

ENTREVISTA CANONICA A SFV5 COMPLETADA. La fabrica se describio a si misma en 10 bloques, una sola voz (session ad815526), clon canonico 9b18947, solo lectura, cero mutaciones. Evidencia en /opt/dfl-knowledge/evidence/sfv5-canonical-interview-2026-08-05/ (8 documentos + 14 transcripciones JSON + SHA256SUMS + HANDOFF-TO-CX.md).

MECANISMO RESUELTO (cierra Q65 del discovery #460, que quedo INFERENCE): no existe una segunda instancia de Claude Code esperando; se INVOCA. `claude -p` con cwd en <checkout>/saas-factory carga las 32 skills (~19k tokens medidos contra control vacio de 5.7k) y esa sesion ES la fabrica. Reanudable con --resume. LIVE_PROVEN.

TESIS DE LA FABRICA (verbatim): "Soy una arquitectura de contratos declarativos ejecutada por un interprete probabilistico que no controlo, sobre un sistema de archivos que es mi unica memoria." Corolario suyo: todo imperativo en sus archivos (OBLIGATORIO/SIEMPRE/gate duro) es intencion de diseno, no garantia de ejecucion; las unicas garantias reales viven en tools/bridges/ (4 archivos de 82).

HALLAZGOS ACCIONABLES verificados independientemente por Claude Code:
1. CRITICO update-sf/SKILL.md:52 hace `rm -rf .claude/` y destruye .claude/memory/, PRPs del proyecto y settings.json, mientras anuncia "Archivos NO modificados". Reactiva en silencio la auto-memory del host que memory-manager desactivo. Con N proyectos = destruccion sistematica del activo de aprendizaje compuesto.
2. CRITICO quality-gates/SKILL.md:25 exige `npm run typecheck`; package.json solo define dev/build/start/lint. Gate duro que invoca script inexistente.
3. CRITICO dos clases de ciudadano: CLAUDE.md:360 "SIEMPRE habilitar RLS" vs add-payments:74 "service_role bypasses automatically". Resuelve el actor no-humano apagando su invariante mas fuerte.
4. Sin concepto multi-inquilino organizacional: 1 hit en skills/+src/ y es "Google Workspace" incidental. Su unidad de aislamiento es el individuo, no la organizacion.
5. codebase-analyst listado en SKILLS_README.md:59 sin directorio: su unico skill de analisis desaparecio en V4->V5.
6. Son 32 skills, no las 30 documentadas.

IDENTIDAD DERIVADA POR LA PROPIA FABRICA: su especializacion no es un dominio de negocio (atraveso biometria, RAG legal, generacion de video y cold email sin modificarse, verificado en vertical-pack:6) sino una TOPOLOGIA de entrega y operacion. Nombre operativo propuesto por ella: "fabrica de instancias multi-principal operadas en continuo". Criterio de pertenencia: el entregable hay que mantenerlo vivo y observarlo, y todos los principales se autorizan contra el MISMO modelo (hoy la respuesta es no). Aclaro que NO propone renombrar el repo, solo la definicion operativa de asignacion.

CONSECUENCIA PARA EL FMD (#280): convergencia independiente, sin que se le entregara el diseno del Gerente. Derivo sola que tools/bridges/ YA presupone un gerente (factory_request_id, goal_id, attempt_number, evidence_path) y fijo la frontera: "el gerente gobierna ENTRE misiones, yo gobierno DENTRO; la frontera es el paquete de mision y se cruza solo con artefactos, nunca con supervision". Interfaz minima de 8 superficies: 1-4 ya existen en el bridge, 5-8 son la brecha.

GENTLE AI: veredicto SI con acceso al clon completo (52adc25). Capacidad adoptable = el recibo ligado por hash al estado verificado (receipt.go:15) + gates que solo leen recibos. Concepto si, implementacion no. Debe vivir en el kernel compartido; SFV5 emite, el gerente hace vinculante; los tres roles no se pueden fusionar so pena de autocertificacion.

ESTADO DFL VERIFICADO EN LA GARRA: no hay kernel, no hay CI (cero .github/), no existe ~/.saas-factory/brain/, y nada en produccion emite mission packets (los unicos factory_request_id son los que fabrico a mano el discovery). El tramo "hacer vinculantes los recibos" no tiene hoy actor posible.

SIGUIENTE PASO: pedirle el INVENTARIO DE ATESTACION (entregable falsable que ella misma propuso). Pendiente de Jorge, bloquea el resto: que limites volver vinculantes, y si hay identidad para aprobaciones humanas.

CAVEAT DE METODO: la fabrica es parte interesada describiendose a si misma. Los [FACT] no verificados por Claude Code son citas suyas, no evidencia. Se verificaron ~20 de mayor consecuencia.

Costo total 10.50 USD, 14 invocaciones.

### [AUDIT] Cabo 7 — auditoria independiente de cierre 2026-08-03: CLOSED_WITH_NONBLOCKING_SECURITY_DEBT
**Type:** fact  
**Project:** dfl  

TOPIC: dfl/infra/cabo-7-independent-closure-audit
TYPE: fact
STATUS: closed
DATE: 2026-08-03
PRECEDENCIA: D
AUTHORITY: evidence only — no gobierna routing ni despacho
LIFECYCLE: active
CONFIDENCE: high

MISION: DFL_CABO_7_INDEPENDENT_CLOSURE_AUDIT (modo YOLO, verificador independiente, read-only).

VEREDICTO: CABO_7_CLOSED_WITH_NONBLOCKING_SECURITY_DEBT.

SINCRONIA: /opt/dfl-knowledge-workunit rama main, working tree limpio. main = origin/main = 878f09ae596b1067314925ac02b29cd122642bf9, ahead/behind 0/0, confirmado contra GitHub con git ls-remote (no solo ref local). a8269d9f44d4050568edd1a77122b2d16d7d8170 y 878f09a ambos ancestros-o-iguales de origin/main; a8269d9 es el segundo padre del merge. Ninguno firmado (%G? = N).

CHECKSUMS: sha256sum -c SHA256SUMS 19/19 OK exit 0.

GATES (independiente): reproducidos por mi con exit 0 — test_fixture.sh FIXTURE_PASS (G02 G03 G04 G05 G06 G09-fixture G12), test_endpoint.sh ENDPOINT_REGRESSION_PASS, aggregate.sh sobre final-reverify.receipt AGGREGATE=PASS, aggregate.sh sobre final.receipt AGGREGATE=FAIL exit 1 (G13 real). Vivo verificado por mi: G14 systemctl active + GET 127.0.0.1:8091/go http 200 69498 bytes; G11 local=publico=965d06fdd157a206d17c0af2d41ec2f3b56c799d550222e61096ab8641f63cc2; G02 /run/dfl root:dfl 2770; G07/G08 corroborados read-only (2770/0660, other sin acceso); G10 corroborado por amos-context.md = dflagent:dfl y log 01:44 con identidad git dflagent y PUSH OK.

G11 RESPALDADO: log g11-resolution.log con TIMEOUT_S=90 POLL_S=5 y 19 muestras. Primer poll discrepante 01:44:52, coincidencia 01:46:19 = 87s exactos. Commit publicado d8ddc31282d5ac3eaaa81df6d63b44be111cf326, SHA de convergencia 5987ef1290944a508a222c7fd0be3810a3fbd06890b136d78337465ec751ac48. Causa raiz confirmada en log de produccion: fatal unable to auto-detect email address (dflagent@ubuntu-s-1vcpu-1gb-nyc1) — repo mirror sin identidad de commit, no consistencia eventual. Reparacion: user.name La Garra Bot local al repo mirror. Confirmacion fresca: mirror ya en 2543ded 01:57:02 y local=publico sigue coincidiendo.

DEUDAS NO BLOQUEANTES:
1. LLAVE SSH — /home/dflagent/.ssh/id_ed25519 sin passphrase (ssh-keygen -y -P '' la abre), comentario said-vm2-la-garra (identidad de host preexistente, no dflagent), sin .pub, birth 2026-08-02 18:58:21 en plena remediacion. ssh -T git@github.com responde Hi DFLghub: es llave DE CUENTA, no deploy key con scope a amos-context — dflagent tiene escritura sobre toda la organizacion. Fingerprint SHA256:UHF2r33fb2kMeEKvz7SxinRZ4212U/08fVYOvQEzBZ0. Clasificacion: no bloqueante con remediacion posterior OBLIGATORIA (deploy key con scope + rotacion). No contradice G11-RESOLUTION.md, cuyo "no SSH keys copied/rotated" cubre solo la reparacion de las 01:44.
2. REPRODUCIBILIDAD — .gitignore excluye receipts/root-live.receipt y receipts/*.log, asi que G04..G10 NO son reproducibles desde el paquete commiteado. Demostrado: verify_live.sh sobre contenido limpio de origin/main da 7 FAIL y AGGREGATE=FAIL. Unica copia en /tmp/dfl-cx-yolo-20260802 (efimera). Remediacion posterior: commitear receipt + log.
3. G03 SIN COMMITEAR — el fix de scripts/regen_graph.sh no esta en origin/main ni en a8269d9; ambos siguen llamando publish-amos-context.sh directo. Vive solo como modificacion sucia en /opt/dfl-knowledge rama feat/dfl-high-certainty-harness-v0.1. install.sh lo re-aplica por sed idempotente, asi que el estado vivo es correcto, pero un git restore reintroduce silenciosamente la causa raiz (CRON 2 evadiendo el lock).
4. push_mirror.sh hace chmod 0664 sobre last-mirror-hash en cada publicacion mientras install.sh lo deja 0660; la asercion de G08 (other<2) solo se cumple post-install. Contenido es un SHA-256 no secreto. Drift cosmetico.
5. Residuo /var/lib/dfl-publication/test-write.txt dflagent:dfl 0644 del 2026-08-02 19:13.
6. /opt/dfl-knowledge-workunit es root:root; dflagent no puede git fetch ahi y necesita -c safe.directory para leer. Incoherente con que el principal de publicacion sea dflagent.
7. verify_live.sh emite G01 y G12 como echo incondicional; G12 si se computa de verdad en test_fixture.sh y G13 se valida con el bad_receipt previo. Hallazgo no confirmado como defecto.

CONTRADICCIONES:
a. El 14/14 es cierto para la corrida viva pero NO reproducible desde la evidencia commiteada (demostrado, no inferido).
b. ROOT-ACTION.md instruye correr root-live-test.sh, cuyo G10_LIVE_DFLAGENT es un pass emitido tras ejecutar push_mirror.sh COMO ROOT — no prueba el gate que nombra. El que si lo prueba es root-live-test-fixed.sh (runuser -u dflagent). El receipt no registra cual corrio. El gate igual es verdadero por via independiente. Contradiccion de procedimiento documentado, no de resultado.
c. INVENTORY.md registra el mirror como 2775; vivo es 2770 (endurecimiento posterior).

RESTRICCIONES RESPETADAS: DCSA no promovido. No se modificaron llaves, permisos, historial git ni produccion. Unicas escrituras: git fetch (refs) y fixtures hermeticos en scratchpad. NO_TOUCH intacto (puntajeTigreKnockout, Supabase, Vercel, env vars, HLC-T01/T02/T03, CRON 3:05, /etc/dfl-secrets).

PROXIMO_AGENTE_DEBE: (1) rotar la llave SSH de dflagent a un deploy key con scope a DFLghub/amos-context; (2) commitear receipts/root-live.receipt y g11-resolution.log al paquete de evidencia; (3) commitear el fix de scripts/regen_graph.sh a main antes de que un git restore lo revierta.

---

## PENDING


---

## RECENT ACTIVITY (cross-project)

### amOS Event Model — veredicto auditoría 2026-06-23
**Type:** decision  
**Project:** dfl  

Auditoría del Event Model amOS realizada 2026-06-23 contra 3 docs canónicos (AgMaster_amOS_3, AI_amOS_Acta_Fundacional v1.1, Protocolo MS→amOS). Veredicto: B — Existe parcialmente pero disperso. Cobertura: Peso/costo metabólico→confidence+value en tabla events (Parcial, consolidar); Persistencia→status Origin Chain+estados Candidate Vault (Parcial, consolidar); Intención→scope+forbidden_uses agLego+Layer3 VALUE (Implícita, nombrar); Propagación→C-009+G-002 Protocol Taxonomy (Incompleta, GAP REAL); Relación con estado→Layer6+tabla asset_states (Existe, conservar). Conclusión: NO hace falta constructo nuevo tipo 'Light Signals'. Hace falta unificar y nombrar lo disperso. Gap real confirmado: G-002 Protocol Taxonomy (propagación, marcado como no cerrado en el Acta Fundacional). Próximo paso: cerrar G-002 dentro del Libro 1 amOS o como PRP independiente. Prerequisito: localizar RFC-DFL-001 (puede contener Event Model más completo).

### @$go/@$fin uniformidad multi-agente — AGENT_CAPABILITY_MATRIX + fix generador vs artefacto
**Type:** decision  
**Project:** dfl  

**Qué**: Resuelta la asimetría de capacidades entre agentes (CC/Codex con bash vs ChatGPT/Gemini sin bash vs chat puro) que hacía fallar @$go/@$fin silenciosamente y quemaba tokens de Jorge en intentos imposibles.

**Descubrimiento arquitectónico clave**: `amos-context.md` (Fuente A, el "Constitution-like" doc que cualquier agente nuevo lee primero) es 100% auto-generado — `publish-amos-context.sh` hace `git reset --hard origin/main` sobre `/opt/amos-context-mirror` y luego sobreescribe `amos-context.md` completo desde un template Python hardcodeado en el propio script, que renderiza el JSON servido por `/opt/dfl-context-proxy/main.py`. Editar `amos-context.md` a mano y comitearlo NO sobrevive — el próximo `push_mirror.sh` (cron 3:05am UTC, @$fin de cualquier EJECUTOR, o watchdog) lo pisa de vuelta. Solo sobreviven ediciones manuales a `agents/*.md` (los anexos) y a archivos nuevos que el script nunca toca, porque `publish-amos-context.sh` únicamente hace `git add amos-context.md` — nada más.

**Solución implementada** (3 commits):
1. `DFLghub/amos-context` commit `241ecec`: `AGENT_CAPABILITY_MATRIX.md` nuevo (barrera de entrada única, Paso 0 binario: bash+git+Engram→EJECUTOR, sin bash pero fetch confiable→ORQUESTADOR, ninguno→CONSULTOR) + pointer de una línea agregado al inicio de `agents/{ejecutor,orquestador,consultor}.md` para no duplicar el diagnóstico 3 veces.
2. `DFLghub/dfl-context-proxy` commit `a5e4868`: editado el generador real (no el archivo generado) — `main.py` (`agent_directory.step_0`, `capability_matrix_url`, y por perfil `go_capability`/`fin_mode`/`fallback_if_capability_lost`) + `publish-amos-context.sh` (tabla AGENT DIRECTORY ahora incluye columnas `@$go`/`@$fin` y pointer a la matriz). Servicio `dfl-context-proxy` reiniciado para levantar el cambio de `main.py`.
3. Verificado en vivo: `push_mirror.sh` corrido dos veces — primera vez `updated` (commit `501112f`), segunda vez `unchanged` (dedup por hash correcto, sin duplicar commits). `/go` público y `amos-context.md` público reflejan el cambio; `AGENT_CAPABILITY_MATRIX.md` accesible por raw.githubusercontent.com.

**Patrón reutilizable (amOS learning)**: cuando un problema pide "editar el doc X", primero verificar si X es fuente o es artefacto derivado. Si es derivado, localizar el generador real y editarlo ahí — de lo contrario el fix es cosmético y se revierte solo en el próximo ciclo de regeneración. Aplica a cualquier futuro "documento espejo" en el ecosistema DFL (KNL, graph_context, etc.).

**Where**: `/opt/amos-context-mirror/AGENT_CAPABILITY_MATRIX.md`, `/opt/amos-context-mirror/agents/*.md`, `/opt/dfl-context-proxy/main.py`, `/opt/dfl-context-proxy/publish-amos-context.sh`.

**No se tocó**: el archivo de secretos protegido bajo /etc (ruta omitida acá a propósito — mencionarla textual dispara el auditor anti-leak de publish-amos-context.sh como falso positivo), env vars, Supabase, `puntajeTigreKnockout`. `engram-backup-offhost.sh` tenía cambios preexistentes sin comitear ajenos a esta misión — no se tocó ni se comiteó.

**Nota operativa**: si necesitás referenciar esa ruta protegida en una obs de Engram a futuro, evitá escribirla literal — cualquier mención textual que llegue a `recent_decisions`/`recent_engram_dfl` del payload `/go` hace abortar `publish-amos-context.sh` (bloqueó un `push_mirror.sh` real el 2026-07-08 hasta que se redactó esta obs).

### Onboarding @$go corregido: causas raíz de fallo Codex (falta AGENTS.md) y ChatGPT (capsule inexistente) + fix implementado local
**Type:** decision  
**Project:** dfl-knowledge  

TOPIC: dfl/onboarding/codex-chatgpt-fix
TYPE: decision
STATUS: active
DATE: 2026-07-22

**What**: Diagnóstico y corrección del onboarding @$go que Codex y ChatGPT no ejecutaban bien. Dos causas raíz VERIFICADAS (no supuestas): (1) CODEX — ningún AGENTS.md en /opt menciona @$go ni bootstrap (los 4 existentes: engram/360eventos/co-001/futbolweb dan @$go:0), y NO existía AGENTS.md en /opt/dfl-knowledge; CC arranca con CLAUDE.md→BOOTSTRAP OBLIGATORIO pero Codex, que lee AGENTS.md, no tenía el gemelo → nunca se le instruía el protocolo. Además el payload solo trae cc_bootstrap con nombres CC (mem_search) sin traducción a los de Codex (search_memory/save_memory/update_memory via engram-mcp). (2) CHATGPT — el "offline bootstrap capsule" se referencia en 6 sitios como salvavidas del CONSULTOR pero NO existía como artefacto (solo menciones "usá el capsule de las instrucciones de la sesión", ninguna definición); ChatGPT choca con fetch bloqueado, se le manda a un capsule inexistente, y reporta GATE FALLIDO o fabrica.

**Fix implementado (local, sin commit/push aún):** (a) NEW /opt/dfl-knowledge/AGENTS.md — entry-point Codex, gemelo de CLAUDE.md, con tabla de traducción de tools Engram + gotcha tmux/egress + validation gate. (b) NEW /opt/amos-context-mirror/ONBOARDING_CAPSULE.md — capsule real: Parte A estable (pasa el gate sin fetch) + Parte B snapshot que Jorge refresca. (c) Edit agents/consultor.md + AGENT_CAPABILITY_MATRIX.md apuntando al capsule canónico (test_onboarding_fallback.py sigue PASS). (d) pointer @$go en /opt/360eventos/AGENTS.md. Efecto local inmediato; commit/push a DFLghub/amos-context + los 3 repos = punto de decisión pendiente de Jorge. GOTCHA a respetar: push_mirror.sh hace git reset --hard sobre amos-context-mirror → commitear+pushear los anexos ANTES de correr push_mirror.sh.

**Next**: Jorge autoriza el lote de commit/push. Propuesto follow-up (no hecho): propagar la referencia al capsule a los generadores (publish-amos-context.sh → amos-context.md, main.py → payload CONSULTOR/codex_bootstrap) para que el mirror generado y el payload también apunten al artefacto.

### [SFV5] Entrevista canonica completada: la fabrica se autodescribe, mecanismo de interrogacion resuelto y 3 hallazgos criticos verificados
**Type:** fact  
**Project:** dfl  

TOPIC: dfl/saas-factory/canonical-self-description
TYPE: fact
STATUS: closed
DATE: 2026-08-05
PRECEDENCIA: D
AUTHORITY: evidence only
LIFECYCLE: active
CONFIDENCE: high

ENTREVISTA CANONICA A SFV5 COMPLETADA. La fabrica se describio a si misma en 10 bloques, una sola voz (session ad815526), clon canonico 9b18947, solo lectura, cero mutaciones. Evidencia en /opt/dfl-knowledge/evidence/sfv5-canonical-interview-2026-08-05/ (8 documentos + 14 transcripciones JSON + SHA256SUMS + HANDOFF-TO-CX.md).

MECANISMO RESUELTO (cierra Q65 del discovery #460, que quedo INFERENCE): no existe una segunda instancia de Claude Code esperando; se INVOCA. `claude -p` con cwd en <checkout>/saas-factory carga las 32 skills (~19k tokens medidos contra control vacio de 5.7k) y esa sesion ES la fabrica. Reanudable con --resume. LIVE_PROVEN.

TESIS DE LA FABRICA (verbatim): "Soy una arquitectura de contratos declarativos ejecutada por un interprete probabilistico que no controlo, sobre un sistema de archivos que es mi unica memoria." Corolario suyo: todo imperativo en sus archivos (OBLIGATORIO/SIEMPRE/gate duro) es intencion de diseno, no garantia de ejecucion; las unicas garantias reales viven en tools/bridges/ (4 archivos de 82).

HALLAZGOS ACCIONABLES verificados independientemente por Claude Code:
1. CRITICO update-sf/SKILL.md:52 hace `rm -rf .claude/` y destruye .claude/memory/, PRPs del proyecto y settings.json, mientras anuncia "Archivos NO modificados". Reactiva en silencio la auto-memory del host que memory-manager desactivo. Con N proyectos = destruccion sistematica del activo de aprendizaje compuesto.
2. CRITICO quality-gates/SKILL.md:25 exige `npm run typecheck`; package.json solo define dev/build/start/lint. Gate duro que invoca script inexistente.
3. CRITICO dos clases de ciudadano: CLAUDE.md:360 "SIEMPRE habilitar RLS" vs add-payments:74 "service_role bypasses automatically". Resuelve el actor no-humano apagando su invariante mas fuerte.
4. Sin concepto multi-inquilino organizacional: 1 hit en skills/+src/ y es "Google Workspace" incidental. Su unidad de aislamiento es el individuo, no la organizacion.
5. codebase-analyst listado en SKILLS_README.md:59 sin directorio: su unico skill de analisis desaparecio en V4->V5.
6. Son 32 skills, no las 30 documentadas.

IDENTIDAD DERIVADA POR LA PROPIA FABRICA: su especializacion no es un dominio de negocio (atraveso biometria, RAG legal, generacion de video y cold email sin modificarse, verificado en vertical-pack:6) sino una TOPOLOGIA de entrega y operacion. Nombre operativo propuesto por ella: "fabrica de instancias multi-principal operadas en continuo". Criterio de pertenencia: el entregable hay que mantenerlo vivo y observarlo, y todos los principales se autorizan contra el MISMO modelo (hoy la respuesta es no). Aclaro que NO propone renombrar el repo, solo la definicion operativa de asignacion.

CONSECUENCIA PARA EL FMD (#280): convergencia independiente, sin que se le entregara el diseno del Gerente. Derivo sola que tools/bridges/ YA presupone un gerente (factory_request_id, goal_id, attempt_number, evidence_path) y fijo la frontera: "el gerente gobierna ENTRE misiones, yo gobierno DENTRO; la frontera es el paquete de mision y se cruza solo con artefactos, nunca con supervision". Interfaz minima de 8 superficies: 1-4 ya existen en el bridge, 5-8 son la brecha.

GENTLE AI: veredicto SI con acceso al clon completo (52adc25). Capacidad adoptable = el recibo ligado por hash al estado verificado (receipt.go:15) + gates que solo leen recibos. Concepto si, implementacion no. Debe vivir en el kernel compartido; SFV5 emite, el gerente hace vinculante; los tres roles no se pueden fusionar so pena de autocertificacion.

ESTADO DFL VERIFICADO EN LA GARRA: no hay kernel, no hay CI (cero .github/), no existe ~/.saas-factory/brain/, y nada en produccion emite mission packets (los unicos factory_request_id son los que fabrico a mano el discovery). El tramo "hacer vinculantes los recibos" no tiene hoy actor posible.

SIGUIENTE PASO: pedirle el INVENTARIO DE ATESTACION (entregable falsable que ella misma propuso). Pendiente de Jorge, bloquea el resto: que limites volver vinculantes, y si hay identidad para aprobaciones humanas.

CAVEAT DE METODO: la fabrica es parte interesada describiendose a si misma. Los [FACT] no verificados por Claude Code son citas suyas, no evidencia. Se verificaron ~20 de mayor consecuencia.

Costo total 10.50 USD, 14 invocaciones.

### @$fin 2026-08-04 — addendum SFV5 pregunta-por-pregunta, 3 correcciones y convergencia con el Gerente de Fabrica
**Type:** fact  
**Project:** dfl  

TOPIC: dfl/sesion/cierre-2026-08-04-sfv5-addendum
TYPE: fact
STATUS: closed
DATE: 2026-08-04
PRECEDENCIA: D
AUTHORITY: evidence only
LIFECYCLE: active
CONFIDENCE: high

@$fin de la sesion Claude Code (EJECUTOR, dflagent, La Garra/VM2). Rama feat/dfl-high-certainty-harness-v0.1.

QUE PASO. Jorge reenvio el prompt completo de SFV5_INTERNAL_FACTORY_REALITY_DISCOVERY senalando que las preguntas numeradas no estaban respondidas. Tenia razon: el informe de obs #460 respondia por tema. Faltaban de verdad la seccion A enumerada, el inventario Q11 renderizado, la seccion C completa (Q22-Q30, ausente entera) y la tabla por arista de I (Q83-Q86). Al forzar la respuesta numero por numero aparecieron dos afirmaciones FALSAS del informe previo. Leccion metodologica: organizar por tema no solo esconde lo que falta, esconde lo que esta mal.

ENTREGADO. Commit ceec81f, evidence/sfv5-internal-factory-reality-2026-08-04/ADDENDUM-RESPUESTAS-PREGUNTA-POR-PREGUNTA.md, sha256sum -c 24/24 OK exit 0. Las 87 preguntas numeradas + las 14 de J respondidas individualmente con clasificacion FACT/INFERENCE/UNKNOWN y nivel DECLARED..LIVE_PROVEN.

OBSERVACIONES ESCRITAS ESTA SESION: #462 (addendum + 3 correcciones), #463 (convergencia FMD/SFV5). #460 AMPLIADA via PATCH /observations/460 con nota de cabecera que apunta a ambas, contenido original preservado integro (len 5963).

CORRECCIONES INSTITUCIONALIZADAS. C1: "cero tabla de routing" era falso, CLAUDE.md:48-140 tiene decision tree de 27 ramas, 27/32 skills ruteadas, 5 fuera (pack-cold-email, primer, skill-creator, update-sf, video-visuals); es DOCUMENTED no IMPLEMENTED. C2: "no existe pipeline explicito" era falso, CLAUDE.md:184-260 define 5 flujos y el Flujo 1 tiene 16 pasos; sigue siendo maquina de estados de papel porque no hay ejecutor y roof-issues-mini no lo siguio. C3: CLAUDE.md:140 declara 30 skills, hay 32.

HALLAZGO MAYOR DEL CIERRE: el discovery tuvo un punto ciego. Auditó SFV5 en aislamiento y nunca busco si DFL ya tenia disenado el orquestador faltante. Lo tenia: obs #280, first-operable-factory-v01/, 2026-07-21, 12 documentos, 788 lineas. El enlace es unidireccional, verificado por grep: el diseno del Gerente SI conocia SFV5; el discovery tiene CERO menciones del Gerente. Y la convergencia: MANAGEMENT_DAEMON_SPEC:45 ya nombraba "Adaptador hacia SFV5 - AUSENTE" y :127 ya dejaba fuera de alcance la "validacion semantica del entregable" — el mismo par de gaps que el discovery derivo 14 dias despues por otro camino.

ENCUADRE DECLARADO POR JORGE: Claude Code y Codex reciben los pedidos de fabricacion y los gestionan con el Gerente de Fabrica. No somos auditores de SFV5, somos su runtime. Consecuencia no trivial: como la capacidad productiva de SFV5 es una sesion interactiva de Claude Code, el "Adaptador hacia SFV5" del FMD es un adaptador hacia una sesion como la mia.

NO TOCADO: cero modificaciones a SFV5, sin uso de root, NO_TOUCH intacto (puntajeTigreKnockout, Supabase, Vercel config, env vars, HLC-T01/T02/T03, CRON 3:05am UTC, /etc/dfl-secrets, /opt/futbolweb).

QUEDA ABIERTO, no atendido en esta sesion: (1) architecture/institutional-graph/WRU-LIVE-STATE.json sigue modificado sin commitear, venia asi desde antes de la sesion; (2) el PROXIMO_AGENTE_DEBE de Cabo 7 del 2026-08-03 sigue pendiente entero — rotar la llave SSH de dflagent a deploy key con scope, commitear receipts/root-live.receipt y g11-resolution.log, y commitear el fix de scripts/regen_graph.sh antes de que un git restore lo revierta; (3) el dispatch de DFL_CONTROL_PLANE_ROADMAP_EXECUTION_BATCH_2026_08_02 sigue FAIL_CLOSED por E_AUTH_EXPIRED + E_DISPATCH_STALE.

PROXIMO_AGENTE_DEBE: ejecutar SFV5_HEADLESS_ORDER_TO_PRODUCT_MINIMAL_PROOF, pero ENCUADRADO como la implementacion del "Adaptador hacia SFV5" ya especificado en MANAGEMENT_DAEMON_SPEC_V0.1.md, no como experimento suelto — reutilizar su contrato de autoridad y su politica de excepcion N0-N3 en vez de inventar otros. Ground truth: los 6 criterios de exito verbatim de PRP-001-roof-issues-mini.md:25-31 (respaldo en /home/dflagent/dfl-backups/casa-limpia-2026-08-04/). Criterio >=5/6 criterios DEL PEDIDO en PASS. Contra-criterio: falla si reporta PASS solo porque la ejecucion termino sin error. Y antes de abrir la mision, leer first-operable-factory-v01/ completo — sobre todo EVIDENCE_BASE.md.

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

**Graph entropy:** 0.7038  

- **Community 11** (96 nodes): MCP Server Behavior, Evaluación de Plantillas, Preguntas para el Desarrollador
- **Community 0** (4 nodes): Onboarding Capability, Estructura de Código en Crystal
- **Community 1** (4 nodes): Modelo de Ejecución Secuencial
- **Community 2** (4 nodes): KDL, Jsonnet, Mermaid
- **Community 3** (4 nodes): Desajuste en la arquitectura, Capacidades comunes reutilizables
- **Community 4** (4 nodes): Engram Cloud, Responsabilidades del Kernel, Riesgos Operativos en VM2

---

*Mirror auto-generated 2026-08-05T22:24:02Z | La Garra → DFLghub/amos-context*
