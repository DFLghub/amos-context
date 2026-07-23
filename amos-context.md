# amOS Context — @$go Live Mirror
**Generated:** 2026-07-23T03:05:11Z  
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

### DeepSeek SFAT failed — independent reviewer not enabled
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/concierge/deepseek-sfat-01
TYPE: review_enablement
STATUS: active
DATE: 2026-07-23
BRANCH: feat/dfl-concierge
COMMIT: 1ff9d0b
RESULT: DEEPSEEK_SFAT_FAIL
WHAT: Configured a read-only DeepSeek review runner and prepared a closed-scope SFAT packet, but OpenRouter chat/completions for deepseek/deepseek-v4-flash returned 401 Missing Authentication header using the host credential reference. GET /models returned 200, but that did not establish a usable reviewer session. FASE B was not executed.
COST: 0 billable review tokens accepted; no valid DeepSeek review session established.
PROXIMO_AGENTE_DEBE: expose a bearer that OpenRouter accepts on POST /chat/completions, rerun FASE A, and only if SFAT passes, dispatch FASE B for the five CP-F1 remediation findings. Do not open CP_F1_03_GATE in the meantime.

### CP-F1-01/02 remediation implemented — awaiting independent review
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/concierge/cp-f1-remediation-01
TYPE: implementation
STATUS: active
DATE: 2026-07-23
BRANCH: feat/dfl-concierge
IMPLEMENTATION_COMMIT: bc5e6d3
RECEIPT_COMMIT: fda6006
STATE: REMEDIATED_AWAITING_INDEPENDENT_REVIEW
WHAT: Remediated the five findings from the independent review of CP-F1-01 and CP-F1-02. Full SHA-256 is now preserved in digest_sha256 with digest_display separated as a non-constitutive alias; provenance validation now checks source_commit existence, payload staleness, branch mismatch and dirty canonical input; ownership is constrained to exactly one CONCIERGE_MAINTAINER; CP-F1-01 and CP-F1-02 receipts were reconciled to real push and Engram state; tests now verify full-digest semantics independently from the display alias.
TESTS: python3 -m unittest concierge.tests.test_cp_f1_01_layout concierge.tests.test_cp_f1_02_validator => PASS
PROXIMO_AGENTE_DEBE: CC or another independent reviewer must repeat the 4R review only for the original five findings: digest truncation, source/branch drift, exclusive ownership, receipt truthfulness, and test confidence. Do not authorize CP-F1-03 unless that review is no longer REJECTED.

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

### @go Protocol v1.0 — protocolo de arranque DFL
**Project:** dfl  

Protocolo @go desplegado el 2026-06-24. Permite a cualquier IA (Claude, ChatGPT, Gemini) arrancar con contexto completo DFL en un paso. Fuente A: amos-context.md en DFLghub/amos-context (commit edd5f51). Fuente B: GET https://context.deepfeelingslabs.com/go — devuelve JSON con identity, recent_decisions, active_constraints, pending, generated_at. Backend: dfl-context-proxy en 127.0.0.1:8091 consulta Engram local 127.0.0.1:7437 con queries: "decisiones activas estado", "restricciones prohibido no tocar", "pendientes criticos". Sin auth requerida en /go. @go v1.0 activo y verificado.

### [VERIFIED] /go pending filter — resolved/stale cleanup
**Project:** dfl  

**What**: /opt/dfl-context-proxy/main.py ahora excluye de pending observaciones con título [RESOLVED] o [STALE], y aplica _is_archived(obs) — consistente con el loop de decisions/constraints.

**Why**: Engram #14 (incidente FW 2026-06-19, RESOLVED) seguía apareciendo en pending porque el loop no tenía filtros de estado. El query "pendientes" matchea cualquier obs que mencione la palabra, sin importar si está resuelta.

**Where**: /opt/dfl-context-proxy/main.py — loop pending en _handle_go(), +4 líneas.

**Learned**: El patrón [RESOLVED]/[STALE] como prefijo de título es suficiente para filtrar sin tocar el schema de Engram. _is_archived() ya existía pero no se aplicaba al loop de pending — ahora es consistente en los tres loops (decisions, constraints, pending). Engram #14 ya no aparece como pending activo. recent_decisions permanece intacto.

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

### Final FASE B accepted; CP-F1-03 gate open
**Type:** fact  
**Project:** dfl  

Final FASE B executed from baseline b8c25bb03519f4f9b3aa1ce2add039cb73c4a44b using the deterministic evidence catalog and response_contract deepseek-review-v1. DeepSeek returned ACCEPTED with five findings F1-F5 CLOSED, all coverage booleans true, no BLOCKER/HIGH, and adapter schema validation valid. Codex final mechanical validation returned INDEPENDENT_REVIEW_VALID: 18 evidence objects checked, hashes/ranges/exact_text valid, no failures. CP_F1_03_GATE: OPEN. CP-A F4 was reconciled by proving historical push commit ancestry and replacing the HEAD placeholder; CP-B adapter passed 6 synthetic tests and 28 total tests. PROXIMO_AGENTE_DEBE: implement CP-F1-03 only by checkpoints; preserve independent 4R review and do not alter protected contracts. Final reviewer session provider AtlasCloud via OpenRouter; no secrets recorded.

### CP-B deterministic DeepSeek adapter implemented
**Type:** fact  
**Project:** dfl  

CP-B implemented without changing F1 remediation: tools/deepseek_review_adapter.py plus strict JSON Schema and six synthetic tests. The adapter preserves raw_response, performs only logged safe normalization (unambiguous outer JSON fence, leading whitespace, exact singleton findings object), rejects ambiguous/truncated JSON, unknown evidence IDs, incoherent verdict/coverage, and evidence references in reason, and emits normalized_response, normalization_log, and schema_validation_result. CP-B plus existing tests pass 28/28. No native response_format support is claimed. Status IMPLEMENTED_AWAITING_INDEPENDENT_REVIEW. PROXIMO_AGENTE_DEBE: push this checkpoint, prepare final evidence packet with response_contract deepseek-review-v1 and allowed evidence IDs, run one final FASE B, and open no CP-F1-03 unless DeepSeek ACCEPTED, all five CLOSED, coverage true, and Codex validation valid.

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

**Graph entropy:** 0.8646  

- **Community 11** (92 nodes): MCP Server Behavior, Evaluación de Plantillas, Asunciones de Verificación
- **Community 0** (10 nodes): amOS, IAIM, ag10
- **Community 1** (4 nodes): Express Framework, Bicep, Developer Certificate of Origin (DCO)
- **Community 2** (4 nodes): Onboarding multi-source
- **Community 3** (4 nodes): Verificación de capacidades
- **Community 4** (4 nodes): HLSL en MiniEngine, Kconfig en Buildroot, Slang como compilador de sombreado

---

*Mirror auto-generated 2026-07-23T03:05:11Z | La Garra → DFLghub/amos-context*
