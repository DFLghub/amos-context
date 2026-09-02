# amOS Context — @$go Live Mirror
**Generated:** 2026-09-02T00:04:28Z  
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

### SESSION SUMMARY 2026-09-01 (Claude/TCC) — @$fin, full-day handoff
**Type:** decision  
**Project:** dfl  

Full-day Claude Code (TCC) session, 2026-09-01, closing via @$fin. Consolidates and cross-references obs #652 through #660 (each mission's full narrative lives in its own observation and doc; this is the session-level index, not a replacement).

CHRONOLOGY / MAJOR THREADS (in order):

1. WEGLOT/BILINGUAL WEBSITE (obs #652, docs/DFL_WEBSITE_BILINGUAL_STRATEGY.md): audited Weglot on the live DFL Squarespace site (data-wg-notranslate markup confirmed, but zero live second-language content today). No plan bought/upgraded. Recommended DFL-owned /en page tree (Option B) over Squarespace-native (doesn't exist) or another paid tool (Option C, rejected). Migration NOT executed (needs a live Squarespace admin session this environment didn't have). Built and REAL-E2E-verified a vendor-lifecycle AQA gate (tools/dfl-website-manager/vendor-lifecycle.mjs + vendor-registry.json): a vendor with an active trial and unverified end date is CRITICAL by design, not OK-by-default. Real cron installed (0 8 * * *). Trial risk is now a tracked operational task, not silent marketing noise -- but not eliminated (real trial end date still unconfirmed).

2. WEBSITE MANAGER NOTIFICATION STORM (obs #653, docs/DFL_WEBSITE_MANAGER_NOTIFICATION_STORM_2026-09-01.md): HIGH severity real incident. sweep.mjs's SLA-breach loop had no idempotency check -- isBreached() compared against a clock start that never advanced, so notify()+markNotified() fired unconditionally on every 10-min cron cycle. 61 runs -> 162 duplicate Telegram alerts over ~10h on 3 real events (2 Weglot-domain, 1 Gemini-domain, both misclassified channel=unknown->ESCALATE). Matches the reported "~200 messages, Weglot/Gemini >50 each." Duplicate ingestion: NO (each email ingested once). Duplicate escalation: YES. Root cause: a persistent SLA-breached STATE was reinterpreted as a new EVENT on every sweep. Contained immediately (removed the cron entry right after the last scheduled run). Fixed for real: ESCALATED/ROUTED -> SLA breach -> notify ONCE -> AWAITING_HUMAN (a status absent from the SLA-clock map, so isBreached() is structurally false for it forever after -- the state machine itself is the guard, no separate cooldown table). Retry made per-row/isolated so a real delivery failure retries cleanly without duplicating events. classify.mjs gained sender-domain-based vendor_lifecycle/vendor_marketing channels (AUTO_RESOLVE, never escalate) for Weglot/Gemini specifically. Reprocessed the 3 stuck real events (reclassified + closed NO_ACTION_NEEDED). sweep.mjs/classify.mjs/store.mjs had ZERO test coverage before this incident -- itself part of the root cause; added sweep.test.mjs (8 tests incl. the 100-consecutive-sweep adversarial -> max 1 Telegram send) and classify.test.mjs (7 tests). Full suite grew from 127 to 143, all PASS. Cron restored with the fix; crontab diffed byte-identical to pre-incident except the intentional vendor-lifecycle addition -- TCC/TCX/telegram-bos paths confirmed untouched throughout.

3. NODE PASS != GRAPH PASS != LOOP PASS AQA (obs #654 + #656, docs/DFL_STATE_GRAPH_LOOP_AQA_2026-09-01.md + docs/DFL_LOOP_TAXONOMY_2026-09-01.md): elevated the storm incident into institutional AQA doctrine. Audited existing organs first (asset-index dfl.yaml schema, aqa-kit's 8 Test Profiles + sentinel-test discipline, tools/lib/silence_watchdog.py -- a shared Python watchdog primitive already correctly implementing fresh-transition-only notify, reused by 3 real crons before this incident ever happened). Added RECURRING_CAPABILITY as a 9th real AQA Test Profile (tools/aqa-kit/lib/profiles.mjs), same mandatory schema as the other 8, with a real sentinel (fixtures/sentinels/recurring-capability-sweep.mjs: 'vulnerable' mode reproduces the exact storm shape and correctly FAILs; 'fixed' mode runs the real shipped sweep.mjs and PASSes). Directive addendum v0.3 added (docs/standards/aqa/DFL_AQA_PRODUCTION_DIRECTIVE_V0.1.md section 12): a passing component set does not imply a passing graph; N>=100 consecutive-cycle adversarial established as institutional minimum. Deepened further: formalized LOOP = reference+sensor+comparator+actuator+re-entry+state+continuation/exit-condition as real code (tools/aqa-kit/lib/loop-behavior.mjs: classifyLoopBehavior() classifies CONVERGED/BOUNDED_RETRY/STABLE_NO_EFFECT/DEGENERATE/DIVERGENT/OSCILLATING from a real per-cycle effect sequence), validated all 6 mission-required loop types (A-F) with ONE shared generic harness (loop-behavior.test.mjs, 12/12 PASS). Honest finding: types E (degenerate/real bug) and F (legitimate feedback loop) can be statistically indistinguishable by count alone -- distinguishing them needs a semantic check (fresh external evidence vs. stale internal replay), which directly motivated reclassifying check-site-health.mjs as NEEDS_LOOP_AQA (structurally a legitimate Type-F candidate, but its per-minute re-alert has no governed reminder-throttle policy -- recommended, not executed). Graphify tested for real (not assumed) across every mode including watch/querylog/diagnose-multigraph: contributes exactly the static code/dependency plane, zero temporal/state signal in ANY configuration -- gap documented, no replacement built. Built a real 8-plane multi-plane observability matrix from what actually reconstructed the storm (temporal history + state graph + external effects; Graphify contributed nothing to that specific diagnosis). Reclassified the 3 prior NEEDS_GRAPH_AQA items with real code reads: jpi-autonomy's reservation-watchdog.mjs and runtime.mjs -> LOOP_AQA_COVERED (real wasAlready guard / self-documented per-flow idempotency markers); check-site-health.mjs stays NEEDS_LOOP_AQA with a precise reason now, not a vague one.

4. JORGE'S LAZO->GRAFO DE LAZOS->GRAFO DE GRAFOS->BUSINESS OS THESIS (obs #655): mid-mission correction on Daniel's course material (private, saasfactory.so classroom, not locally accessible). Jorge's 6 corrections absorbed and codified into the AQA directive §12: the enriched 7-part loop formula (4 classic cybernetic pieces = HOW it regulates; 3 added pieces -- re-entry/state/continuation-exit-condition -- = WHETHER repeating converges/stabilizes/oscillates/explodes), and the correction that Graphify is "one possible instrument to observe parts of the system," not "the tool for loops" -- independently confirmed by this session's own live Graphify testing the same day.

5. CAPABILITY ACQUISITION AQA (obs #657, docs/DFL_CAPABILITY_ACQUISITION_AQA_2026-09-01.md): a third AQA institutionalization axis -- not "does it loop correctly" but "was it actually acquired, or did it just work once." Maturity ladder SEEN->EXECUTED->ACQUIRED->REUSABLE->TRANSFERABLE->GENERALIZED with objective per-transition evidence, codified as a 10th real Test Profile CAPABILITY_ACQUISITION (T0-T6 required_checks). PORTABLE != ACQUIRED != TRANSFERABLE demonstrated with a real DFL example (Skill Dock's artifact-copy tests prove portability, not acquired-usage-knowledge). Three honest benchmarks: Skill Dock (REUSED/TRANSFERRED real but its genericity was never turned into a permanent regression test -- a real, own-goal finding); Graphify (the strongest result -- a real, organic T5 negative case: this session never reached for Graphify during the actual storm diagnosis, chose logs+DB+Telegram on its own); Loop AQA (T3 executed live in-mission via a real cross-repo transfer test against JPI's real reservation-watchdog.mjs, unmodified, 1/1 PASS). Intelligence Memory format proposed: SITUATION->CAPABILITY CHOSEN->WHY->RESULT->EVIDENCE->LIMIT DISCOVERED (not a new mechanism, just a shape for Engram entries, applied retroactively as an example). Real motivating case Jorge gave live: Realtor's WhatsApp two-way capability should be "build once, use N->infinity times" -- verified via asset-index it was NOT registered as reusable (0 search results), directly setting up threads 6-8.

6. WHATSAPP CAPABILITY EXTRACTION (obs #658, docs/DFL_WHATSAPP_CAPABILITY_EXTRACTION_2026-09-01.md): extracted Realtor's real, production-proven (deployment dpl_GAjXbwt8R1sGdE3ruCeSsNsg5xd7) Meta WhatsApp Cloud API adapter into tools/messaging-adapters/meta-whatsapp/adapter.mjs, the canonical single source (dfl.messaging-adapters.meta-whatsapp.v1). Before writing code, found the previous extraction attempt (tools/mercader-autonomy/messaging_meta_whatsapp.mjs, claiming "institutional" status in its own comment) had ALREADY silently drifted from Realtor's real version and had ZERO real consumers -- exactly the PORTABLE-not-ACQUIRED trap the capability-acquisition ladder (thread 5) was built to catch, found failing for real inside DFL's own codebase. Reconciled rather than re-forked: Realtor's proven behavior as base + 2 additive generalizations (health(), channel tagging) merged from the abandoned copy. Realtor kept working, PROVEN not assumed (baseline captured before any change, its 3 own test files re-run identical after the sync). Built sync.mjs+consumers.mjs+drift-check.test.mjs -- the actual fix for the root cause (a permanent test that would have caught the original drift, proven both directions: fails on real pre-existing drift, passes after sync). MERCADER rewired to import canonical directly (T2, 24/24 PASS, zero regression).

7. MULTI-PRODUCT WHATSAPP TRANSFER (obs #659, docs/DFL_WHATSAPP_MULTI_PRODUCT_TRANSFER_2026-09-01.md): extended the canonical to JPI (greenfield, built scripts/jpi-autonomy/whatsapp-adapter.mjs mapping WhatsApp replies to flow-aceptacion.mjs's own pre-existing, self-documented acceptance-evidence gap; 8/8 new + 255/255 full JPI suite PASS), JackyClean (found ANOTHER independently drifted hand-copy of the same abandoned mercader fork, reconciled identically, real domain code notify.ts/messaging_surface.mjs preserved untouched), and DFL Website (functions/whatsapp-intake/, reusing the exact existing functions/challenge-intake satellite-Vercel-function pattern rather than inventing one, since Squarespace itself cannot host a backend -- confirmed same day in thread 1). RSVP investigated and found genuinely BLOCKED at that point: a shallow clone showed a pure client+Supabase frontend with zero server API routes and a Python backend with no code in the repo -- correctly NOT forced. T6 left UNPROVEN for all 5 real consumers without exception, honoring the mission's explicit anti-inflation rule even under momentum toward a clean sweep.

8. RSVP BACKEND RESOLUTION (obs #660, docs/DFL_RSVP_WHATSAPP_BACKEND_2026-09-01.md): a FULL (not shallow) clone revealed RSVP's Python backend and an old /api/rsvp route were real once, and were DELIBERATELY DELETED 2026-08-17 as genuinely dead code (git blame confirmed, real commit message: "nothing in the app called them, writes already blocked by RLS"). Correctly did NOT recover them -- that would have reversed a real, correct decision. Built the real minimal backend instead using RSVP's own existing framework (Next.js App Router route.ts, zero new Vercel infra) -- proven with an actual `npm run build` (not just type-check), confirming the new route compiles as a real dynamic Vercel function alongside all 10 pre-existing routes with zero regression. Canonical wired via the same sync mechanism, zero hand-copying. Honest schema-level finding, not worked around: RSVP's real schema requires an auth.users-backed identity for every rsvps write and has no phone/guest column anywhere -- so no RSVP-state-writing "confirm via WhatsApp" flow was invented; only the safe verify->normalize->read-only-lookup->human-handoff path was built and proven E2E (11/11 new tests PASS). drift-check.test.mjs extended to support a genuinely different case (RSVP is deliberately non-VM2-resident per the externalization pattern) -- a missing external consumer now SKIPS rather than FAILS, verified in both directions. Before pushing the finished work to GitHub (the only way to durably preserve it, since RSVP is intentionally not kept locally), noticed RSVP was outside today's authorized dispatch scope and used AskUserQuestion rather than assuming -- Jorge explicitly authorized a new branch push, not a merge/deploy. Pushed feat/whatsapp-webhook-intake for real (commit 55c82f7) to github.com/DFLghub/event-rsvp-waitlist, NOT merged to main, no Meta credentials anywhere.

INSTITUTIONAL DOCTRINE THAT DIDN'T EXIST THIS MORNING AND EXISTS NOW:
- AQA directive §12 (v0.3): NODE PASS != GRAPH PASS, cycle/graph audit mandatory for recurring capabilities, N>=100 adversarial baseline, the 7-part cybernetic loop formula.
- tools/aqa-kit/lib/profiles.mjs: RECURRING_CAPABILITY (9th) and CAPABILITY_ACQUISITION (10th) Test Profiles, both with real sentinel tests, not just declared.
- tools/aqa-kit/lib/loop-behavior.mjs: the generic loop-behavior classifier + harness, reused by 3 real fixtures across 3 different repos (saas-factory's own storm sentinel, the synthetic A-F taxonomy tests, and the real cross-repo JPI transfer test).
- dfl.messaging-adapters.meta-whatsapp.v1: DFL's first fully-canonicalized, multi-consumer, drift-guarded shared capability with a real generalized N-consumer sync mechanism (consumers.mjs + sync.mjs + drift-check.test.mjs) -- 6 real consumers (Realtor, MERCADER, JackyClean, JPI, DFL Website, RSVP), each with distinct, non-inflated evidence.
- The `external: true` consumer pattern (skip-not-fail drift-check) for VM2-externalized products, extending the existing externalization doctrine into the drift-check mechanism for the first time.

WHAT STAYED HONESTLY UNPROVEN OR BLOCKED (do not silently claim these tomorrow):
- Weglot's real trial-end date (no admin/IMAP access this session).
- The actual EN/ES Squarespace migration (proposal ready, not executed -- needs a live Squarespace admin session).
- check-site-health.mjs's reminder-throttle policy (recommended, not built -- real per-10-min re-alert while a real outage persists, structurally Type-F-legitimate but ungoverned).
- T6 (independent/spontaneous capability invocation) for EVERY capability discussed today, without exception -- everything was explicitly directed.
- Skill Dock's cross-generation genericity is real (one proven event) but not covered by a permanent regression test.
- RSVP's WhatsApp-driven RSVP state writes (confirm/change/cancel) and outbound (invitations/reminders) -- both require a real phone-to-identity linking decision nobody has made; deliberately not built.
- Which of the 6 WhatsApp consumers (if any) should actually go to Meta production -- zero credentials configured anywhere, that's Jorge's call.
- MERCADER's own human/external gates (unrelated to today's work, still open from before).
- Realtor's final human verification + the OWNER_PASSWORD rotation (unrelated to today, still open from before, referenced in HANDOFF-TCC-SESSION-2026-08-30.md).

DO-NOT-REPEAT LIST (institutional, for any future session, any Tony):
- Do not hand-copy tools/messaging-adapters/meta-whatsapp/adapter.mjs into a new product ever again -- always use sync.mjs + consumers.mjs. Two real hand-copies (MERCADER's original, JackyClean's) already drifted silently before this was caught; a third is not acceptable.
- Do not declare a recurring capability (cron/watcher/retry/SLA/heartbeat) safe from a single successful run -- always run the N>=100 adversarial via loop-behavior.mjs's harness or an equivalent, per RECURRING_CAPABILITY.
- Do not declare T6 (spontaneous invocation) PASS for anything that was explicitly requested -- UNPROVEN is the honest default until real unprompted evidence exists.
- Do not revive RSVP's Python backend or /api/rsvp route -- confirmed real dead code, deliberately removed 2026-08-17, do not resurrect without a NEW real reason.
- Do not push/deploy any of the 6 WhatsApp consumers to Meta production without Jorge's explicit credential/gate authorization.
- Do not assume a product's git-visible architecture reflects a shallow clone's view -- RSVP's real history (a full clone) told a completely different story than the shallow depth-1 clone from the prior mission the same day.

Full doc index for tomorrow: docs/DFL_WEBSITE_BILINGUAL_STRATEGY.md, docs/DFL_WEBSITE_MANAGER_NOTIFICATION_STORM_2026-09-01.md, docs/DFL_STATE_GRAPH_LOOP_AQA_2026-09-01.md, docs/DFL_LOOP_TAXONOMY_2026-09-01.md, docs/DFL_CAPABILITY_ACQUISITION_AQA_2026-09-01.md, docs/DFL_WHATSAPP_CAPABILITY_EXTRACTION_2026-09-01.md, docs/DFL_WHATSAPP_MULTI_PRODUCT_TRANSFER_2026-09-01.md, docs/DFL_RSVP_WHATSAPP_BACKEND_2026-09-01.md. IRONMAN.md has one real row per mission (10 rows added today). Engram obs #652-#660 plus this index (#661).

### LAZO → GRAFO DE LAZOS → GRAFO DE GRAFOS → BUSINESS OS — tesis institucional corregida por Jorge (2026-09-01)
**Type:** decision  
**Project:** dfl  

Jorge dio correcciones puntuales sobre un documento externo (material de un curso de Daniel, paginas 2-3, no presente en este repo/filesystem -- lo comparte "para tu información y conocimiento", no como archivo a editar) que sintetiza la relación entre el incidente de notification storm de hoy (obs #653/#654) y la arquitectura institucional de DFL. Registrado aquí porque es la tesis conceptual que ata todo el trabajo de hoy, no solo una nota de sesión.

TESIS CENTRAL: la progresión no es "grafos" como tema aislado, es una jerarquía:
LAZO → GRAFO DE LAZOS → GRAFO DE GRAFOS → BUSINESS OS / SISTEMA VIABLE
El giro de hoy hacia "lazos" no se aparta del curso institucional -- vuelve a su fundamento.

CORRECCIONES PUNTUALES DE JORGE AL ESQUEMA DE DANIEL:
1. "Business OS = grafo de grafos" se mantiene, pero se precisa: los grafos no solo "se conectan" -- las SALIDAS de unos se convierten en ENTRADAS, REFERENCIAS, SENSORES o PERTURBACIONES de otros. Ahí nacen los lazos entre macroprocesos.
2. "El agente sabe TODO, en todo momento" es demasiado absoluto. Corrección: "El sistema puede saber lo que necesita saber, cuando lo necesita, SI está conectado, observable y autorizado." Justificación explícita de Jorge: "hoy vimos que tener información disponible no implica interpretarla correctamente" (referencia directa al storm: cada componente individual pasaba sus tests y el sistema seguía roto).
3. Sobre "jobs corren solos": falta pieza fundamental. Corrección: "Un job recurrente necesita lazo, estado y condición de salida/estabilidad." El Website Manager tenía cron, sensor, comparación y acción -- pero el lazo estaba mal cerrado. Resultado: 162 notificaciones duplicadas.
4. Las tres escalas se afinan agregando estado+autoridad+feedback (si no, "grafo de grafos" suena demasiado estructural/vacío):
   Lazo: una tarea se regula.
   Grafo: muchos lazos coordinan un trabajo.
   Business OS: muchos grafos coordinan un negocio, COMPARTIENDO ESTADO, MEMORIA, AUTORIDAD Y FEEDBACK.
5. La sección "por qué se rompe" (Ashby, Goodhart, "mi tormenta" de Daniel) ya no es solo el ejemplo de Daniel -- DFL tiene ahora un espécimen propio, independiente, que demuestra que el problema de los lazos se reproduce fuera del sistema de Daniel: Website Manager, 3 eventos reales, 61 sweeps, 162 Telegram duplicados, duplicate ingestion=NO, duplicate escalation=YES.
6. Nota editorial menor: error de numeración de página en el documento original (dice "Página 2" donde debía decir "Página 3" en la sección de cierre).

LA IDEA MÁS IMPORTANTE (enriquecimiento del lazo clásico de Daniel, no contradicción): un lazo de control clásico tiene 4 piezas -- Referencia + Sensor + Comparador + Actuador -- que explican CÓMO REGULA. Daniel menciona después "LA VUELTA: otra vez" pero no la desarrolla -- ahí es exactamente donde apareció el fallo de hoy. No bastaba con que las 4 piezas fueran correctas; había que controlar qué significa "volver a pasar". Formulación enriquecida de Jorge:

LAZO = Referencia + Sensor + Comparador + Actuador + Reentrada + Estado + condición de continuidad/salida

Los primeros 4 explican cómo regula. Los últimos 3 (Reentrada, Estado, condición de continuidad/salida) explican SI AL REPETIR converge, se estabiliza, oscila o explota. Ese es el puente directo entre el curso de Daniel y lo que se descubrió en AQA hoy: sweep.mjs tenía las 4 primeras piezas correctas (cron=reentrada, DB row=sensor, isBreached()=comparador, notify()=actuador) -- el defecto vivía enteramente en que la Reentrada no tenía condición de salida real y el Estado no cambiaba con ella.

CORRECCIÓN SOBRE GRAPHIFY (confirma independientemente lo que esta sesión ya había concluido probándolo en vivo, obs #654): "Graphify no es la herramienta de los lazos. Es potencialmente uno de los instrumentos para observar partes del sistema. El objeto que queremos comprender es el lazo, y ese lazo puede atravesar código, estado, tiempo, cron, memoria, autoridad y efectos externos." Ningún instrumento único observa el lazo completo.

FRASE DE CIERRE DE JORGE, el hilo que une ayer con hoy: "Aquí vemos la anatomía y la cibernética básica; el incidente de hoy nos mostró que DFL necesita además auditar la DINÁMICA de esos lazos cuando empiezan a correr en el tiempo."

ACCIÓN TOMADA esta sesión: se incorporó el fundamento cibernético (fórmula de 7 partes + progresión LAZO→GRAFO DE LAZOS→GRAFO DE GRAFOS→BUSINESS OS + corrección sobre Graphify) directamente en docs/standards/aqa/DFL_AQA_PRODUCTION_DIRECTIVE_V0.1.md §12 (el addendum ya creado hoy sobre el mismo incidente), como grounding conceptual del RECURRING_CAPABILITY Test Profile ya construido (tools/aqa-kit/lib/profiles.mjs) -- no se creó documento nuevo ni se duplicó fuente de verdad, se enriqueció la ya escrita hoy mismo.

Pendiente/no hecho: el documento original de Daniel (páginas 2-3) no existe en este filesystem/repo -- Jorge lo compartió como contexto, no como archivo a editar; si en el futuro se requiere reflejar estas correcciones en el documento original de Daniel, se necesita el archivo real (probablemente vive fuera de VM2, ver residence pattern de docs/patterns/vm2-externalization/METHOD.md).

### Paseo TCX Full Access profile configured and verified
**Type:** decision  
**Project:** saas-factory-setup  

Cierre @$fin. En VM2/PASEO_HOME=/home/dflagent/.paseo-sfv5-dev se configuró el agent profile dedicado de Paseo id=tcx-full-access, name='TCX — Full Access', provider=codex, model=gpt-5.5, modeId=full-access. La fuente instalada de Paseo 0.5.0-beta.5 confirma que full-access materializa approval_policy=never y sandbox_mode=danger-full-access. `paseo reload --format json` aplicó daemon.agentProfiles sin restartRequiredPaths. Verificación directa vía API local confirmó el perfil y los modos Codex (auto, auto-review, full-access). No se lanzó sesión desde Pixel, no se modificaron credenciales ni el provider global Codex. Próximo paso para Jorge: cerrar/reabrir Paseo en Pixel, abrir saas-factory y seleccionar TCX — Full Access; no seleccionar Codex genérico.

### RSVP backend/runtime resolved + Meta-WhatsApp canonical wired, real E2E, branch pushed not merged (2026-09-01)
**Type:** architecture  
**Project:** dfl  

Direct follow-up to the multi-product WhatsApp transfer mission (obs #659), which left RSVP BLOCKED based on a shallow (depth-1) clone that showed no server-side API routes and a Python backend with no application code. Jorge asked to actually resolve this: discover the real history, recover if recoverable, otherwise build the minimal viable backend using an already-proven DFL pattern, then wire the canonical adapter.

REAL DISCOVERY (full git clone this time, not shallow): backend/main.py (a real FastAPI app) and a Next.js /api/rsvp route DID exist historically, and were DELIBERATELY DELETED on 2026-08-17 in commit 9455aa6 ("Remediate Jose's Event RSVP MVP findings ... dead-code decommission"), with the commit message stating plainly: "nothing in the app called them, and their writes were already blocked by RLS." The real waitlist auto-promotion race-safety now lives entirely at the Postgres/RLS/constraint level, not in an application backend.

DECISION, evaluated with evidence not skipped: do NOT recover the Python backend -- reviving it would reverse a real, correct, already-documented decision, not fill an actual gap. Instead BUILD the minimal viable backend using the pattern DFL has already proven repeatedly today (JPI, JackyClean): RSVP already uses Next.js App Router, so a native API route (src/app/api/whatsapp-webhook/route.ts) is zero new infrastructure -- it deploys as part of the exact same Vercel "frontend" service already live (vercel.json's services.frontend.framework: nextjs), not a new service block.

CANONICAL WIRED, no core logic duplicated: src/lib/messaging/messaging_meta_whatsapp.mjs is a byte-identical, drift-checked sync of tools/messaging-adapters/meta-whatsapp/adapter.mjs via the same sync.mjs mechanism already used for the other 4 real consumers -- never hand-copied or edited.

HONEST SCHEMA-LEVEL FINDING (not ignored, not worked around by inventing something): RSVP's real schema (supabase/migrations/001_create_schema.sql) requires every rsvps row to reference a real auth.users-backed user_id -- there is NO phone/contact column anywhere on users or rsvps, and no guest-RSVP path exists. This means a WhatsApp message from an unauthenticated phone number cannot safely trigger a real RSVP state write (confirm/change/cancel) without first inventing a phone-to-account identity-linking mechanism -- a real product/schema decision this mission explicitly did NOT make unilaterally, honoring the mission's own "no inventes funciones que RSVP no requiera" rule. Symmetrically, no outbound (invitation/reminder) was built either, since there's nowhere to store a phone number to send to.

WHAT WAS ACTUALLY BUILT AND PROVEN E2E, safely, with zero new write surface: verify Meta signature (real HMAC) -> normalize via canonical adapter -> optional READ-ONLY lookup of a referenced event (using the exact same anon-key + RLS boundary every other read in the app already goes through, no privilege escalation) -> produce a human-handoff record (handoff always 'HUMAN_REQUIRED', never auto-processed). Same minimal shape already proven for the DFL website's own WhatsApp intake in the prior mission.

REAL EVIDENCE, not just declared: ran an actual `npm run build` (not just type-check) -- confirmed /api/whatsapp-webhook compiles as a genuine dynamic Vercel-function route alongside all 10 pre-existing routes, zero regression. `npm run type-check` clean, matching the exact baseline captured before any change. Added a `test` script (tsx --test) to package.json since none existed before -- 11/11 new tests PASS across rsvp-whatsapp.test.ts (3) and route.test.ts (4), plus the canonical adapter's own coverage.

DRIFT-CHECK MECHANISM EXTENDED for a genuinely different case: RSVP is deliberately NOT VM2-resident (docs/patterns/vm2-externalization/METHOD.md -- its local copy was intentionally removed from VM2 on 2026-08-21), unlike the other 4 consumers which are permanent local checkouts. Registered in consumers.mjs with `external: true`; drift-check.test.mjs updated so a missing target path for an `external` consumer reports SKIPPED (informational), never FAILED -- while the other 4 still hard-fail if their copy goes missing. Verified in BOTH directions this session: with the temp clone present, 17/17 PASS including a real byte-identity check against RSVP; after deleting the temp clone (end of mission, per the externalization pattern's own expectation that physical VM2 residence is temporary), re-ran the suite -- 16 PASS + 1 SKIPPED, nothing broken.

GOVERNANCE MOMENT, handled correctly: before pushing the finished branch to GitHub (the only way to durably preserve real work done in a temporarily-rematerialized clone, per the externalization pattern's own logic), noticed RSVP was NOT in today's explicit authorized dispatch scope (the /go payload seen earlier only listed saas-factory, JPI, and whatsapp-realtor-mvp). Used AskUserQuestion rather than assuming -- asked Jorge directly whether to push a new branch or hold. Jorge explicitly authorized pushing a new branch (not a merge, not a deploy). Pushed `feat/whatsapp-webhook-intake` for real to github.com/DFLghub/event-rsvp-waitlist (commit 55c82f7) -- NOT merged to main, no Meta credentials configured anywhere, matching the mission's "no producción sin gate" instruction. A Vercel preview build may auto-generate on the branch per standard GitHub integration behavior, but with no real Meta secrets the route would just answer 503.

dfl.yaml's evidence field updated with RSVP as a real 6th consumer entry (Realtor, MERCADER, JackyClean, JPI, DFL Website, now RSVP), each with distinct, real, non-inflated evidence. Asset-index regenerated. T6 (independent invocation) stays UNPROVEN for RSVP too, consistent with all 5 other consumers -- this was explicitly directed work, not spontaneous.

Files: docs/DFL_RSVP_WHATSAPP_BACKEND_2026-09-01.md (full report), tools/messaging-adapters/meta-whatsapp/{consumers.mjs,drift-check.test.mjs,dfl.yaml} (extended), and on the RSVP repo itself (branch feat/whatsapp-webhook-intake, not in this VM's filesystem after cleanup -- recoverable via `git clone git@github.com:DFLghub/event-rsvp-waitlist.git` then `git checkout feat/whatsapp-webhook-intake`): src/app/api/whatsapp-webhook/{route.ts,route.test.ts}, src/lib/messaging/{messaging_meta_whatsapp.mjs,rsvp-whatsapp.ts,rsvp-whatsapp.test.ts}, package.json (test script + tsx devDependency added). IRONMAN.md row added.

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

### RESUELTO: relación DFL/Jorge con Daniel Carreón — Jorge es miembro pagado Nivel 6 de la comunidad "SaaS Factory" (saasfactory.so)
**Type:** fact  
**Project:** dfl  

CORRECCIÓN/RESOLUCIÓN de una ambigüedad abierta desde la misión de ingestión de fabrica-de-miniaturas (obs #589) y el harvest de 75 repos (obs #595): Jorge confirmó directamente (2026-08-29) que ES MIEMBRO NIVEL 6 con suscripción ANUAL de `saasfactory.so`, la comunidad de pago que Daniel Carreón fundó y opera comercialmente bajo la marca "SaaS Factory". Jorge describe su participación con un ciclo explícito: "Recibo-Doy-Pruebo-Aplico-Rompo-Construyo".

QUÉ ES saasfactory.so (verificado por fetch directo 2026-08-29, no solo inferido): comunidad de membresía paga ($39/mes o $199/año subiendo a $299 el 31-ago-2026), Next.js+Supabase, self-hosted, dirigida a "Arquitectos de Software" que usan Claude Code. Vende explícitamente: "Repositorio + Business OS — los rieles para que tus agentes (Claude Code, skills, subagents) construyan enterprise-grade en tus propios servidores"; 9 cursos + 43 eventos grabados; agentes IA de soporte 24/7 (Trinity, Sensei, Levy); comunidad reclamada de "+700 personas" (el widget en vivo del sitio mostraba "0 Members/0 Online" al momento del fetch -- inconsistencia del propio sitio, no verificada, señalada por disciplina Claim!=Evidence). No se pudo ver el contenido del perfil específico compartido por Jorge (requiere sesión autenticada; la app es client-side-rendered y sin login solo devuelve el shell público).

RE-ENCUADRE INSTITUCIONAL IMPORTANTE: esto cambia la naturaleza de todo lo investigado hasta ahora sobre Daniel Carreón (obs #589, #590, #591, #595). No es inteligencia competitiva sobre un tercero desconocido -- es una relación comercial real y activa (membresía paga, Nivel 6, anual) con intercambio reconocido explícitamente por el propio Jorge. La hipótesis más fuerte para el parecido estructural encontrado en `saas-factory-agencia` (commands/agents/skills casi 1:1 con la migración V3->V4 de SFV5, documentado en obs #595) es que el vocabulario/patrón "SaaS Factory" de DFL fue aprendido/adaptado desde esta membresía, no inventado independientemente ni copiado sin más de GitHub público. "SFV2" muy probablemente es la próxima versión/tier que Daniel construye PARA su propia comunidad de pago -- lo que significa que DFL muy probablemente lo recibirá vía el canal de membresía (cursos/eventos/Business OS actualizado), no por scraping de repos públicos como se venía asumiendo.

IMPLICACIÓN DE GOBERNANZA PARA FUTUROS TONYS: el contenido de la membresía (cursos, eventos, "Repositorio + Business OS" interno) es propiedad comercial de Daniel/SaaS Factory bajo los términos de esa membresía, no material de código abierto libre para ingestar sin más -- distinto de los repos públicos de GitHub que sí son fair game como specimens (ya usados así en obs #589/#595). Cualquier futura ingesta de contenido DE ADENTRO de la membresía debe tratarse con el mismo cuidado de atribución/licencia que cualquier material de un partner comercial real, no como "specimen externo" genérico.

PENDIENTE: Jorge tiene acceso autenticado real; no se intentó ni se debe intentar sortear el login del sitio. Si se quiere ver contenido real de la membresía, la vía correcta es que Jorge lo comparta directamente o autorice el uso del mecanismo de browser-tunnel ya existente (127.0.0.1:9223, la misma sesión de Chrome autenticada usada para el trabajo de Squarespace) para navegar CON su sesión ya iniciada -- nunca intentar acceder sin autorización explícita para esa acción puntual.

---

## PENDING


---

## RECENT ACTIVITY (cross-project)

### Session summary: dfl-knowledge
**Type:** session_summary  
**Project:** dfl-knowledge  

## Goal
Sesión larga y multi-misión sobre DFL/SFV5: auditoría forense grounded de la copia local SaaS Factory (VM2), su censo estructurado, remediación en 3 rondas hasta verificación independiente cerrada, reconciliación de la arquitectura laboral completa de DFL (Workforce Registry / Factory Manager), y el PRP ejecutable del primer incremento vivo (Workforce Registry Unit v0.1), reconciliado con resultados de un laboratorio experimental de gobierno de mutaciones.

## Instructions
- Jorge dio autorización explícita para operar autónomamente en varias misiones sucesivas ("no solicites autorización intermedia", y luego "full authorization to perform this task/mission").
- Patrón de trabajo institucional confirmado y seguido en toda la sesión: nunca sobrescribir evidencia ya publicada/commiteada — toda corrección o ronda nueva va en un subdirectorio nuevo, con referencia explícita a lo que corrige.
- Verificación de colisión con CX (otro agente operando en paralelo sobre el mismo repo) antes de cada `git add`/commit: `git log --oneline`, `git status --short`, nunca `git add -A`.
- Contrato de integridad de evidencia consolidado y reutilizado en todas las misiones posteriores: manifest/checksum de dos pasos (MANIFEST.json escrito primero, excluyendo su propio nombre y el de SHA256SUMS.txt desde el listado inicial; SHA256SUMS.txt escrito después, nunca por `sha256sum * > archivo` ni por copiar/renombrar un archivo ya hasheado bajo otro nombre — ambas son causas raíz reales de bugs de autorreferencia ya encontrados en esta misma cadena).
- Jorge pidió un `@$fin` parcial (checkpoint) a mitad de una misión — se distinguió correctamente de un cierre canónico: `mem_save` incremental sin barrido de archivado ni `push_mirror.sh`, sesión sigue abierta. Ese checkpoint (obs #394) quedó archivado hoy al completarse y validarse la misión que dejaba pendiente.

## Discoveries
- **SFV5 local no es "SFV5 de Ricardo Silva".** El único autor real verificable del repo comunitario (`upstream/main`) es Daniel Carreón. Todo lo etiquetado "V5" localmente fue introducido en un commit único (`5e42124`) de Jorge Tigreros — es autoría DFL sobre el V4 comunitario, no una importación de terceros. Cero evidencia de "Ricardo Silva" en el historial git accesible.
- Ningún "minion" nombrado (Sensei/Trinity/AI Dani) existe en el repo; "Levy" es solo un asset de imagen (mascota) para la skill `video-visuals`, no un agente.
- El grafo de codebase-memory no cubre `.claude/` de SFV5 en absoluto (0 nodos) ni `tools/bridges/` — 4 índices duplicados para la misma ruta con conteos distintos pese al mismo `head_sha`, causa raíz confirmada: truncamiento de `max_rows` en ciertas queries (no corrupción de datos).
- El activo de mayor apalancamiento de todo el inventario DFL, descubierto en la reconciliación arquitectónica (CC-2), no es BOS/Concierge/SFV5 por separado — es un harness de alta certeza **genérico** ya construido y probado (`experiments/dfl-high-certainty-exploration-harness-v0.1/`, 2/2 tests, piloto real ejecutado) que ninguna auditoría previa había conectado con el resto del inventario. Existe una duplicación real (2 patrones HLC independientes: el genérico y la instancia específica de Concierge F1B con defectos de evidencia confirmados) — pero la revisión independiente posterior (CX-N1) determinó que NO son duplicados funcionales demostrados y que su unificación queda `DEFER`, no se reabre.
- WorkUnitLedger (`dfl-knowledge/concierge/workunit.py`, mergeado a main, dogfood real, 237/237 tests) es el activo más maduro para "Factory Manager" — más confiable que `parallel-build` de SFV5 (solo documentado).
- "Opportunity Inbox" y "Refinería y Distribución de Capacidades" están completamente ausentes de todo el corpus DFL bajo cualquier variante de nombre buscada.
- El laboratorio experimental de gobierno de mutaciones (`workforce-registry-capability-lab-2026-07-30`, 16/16 escenarios PASS) falsificó la intuición de que un CRUD simple sobre un Registry es suficiente: el estado canónico debe separarse de propuestas, con validación, aprobación, bloqueo optimista (`expected_version`), versionado append-only, verificación de dependencias y evidencia — nunca escritura directa, nunca hard delete, nunca "rollback = replay de audit log" (rollback real = commit gobernado de una versión restaurada).
- Bug de autorreferencia de checksum tiene 2 causas raíz distintas ya encontradas en esta cadena: (1) truncamiento de shell (`sha256sum * > archivo` trunca el archivo de salida antes de leerlo como argumento del glob), (2) captura de hash bajo un nombre temporal que luego se reutiliza al copiar/renombrar el archivo final. Ambas se evitan solo excluyendo el nombre de salida de la lista de entrada ANTES de hashear, nunca por post-filtro.

## Accomplished
- ✅ Informe forense original SFV5 — commit `a4589bf` (obs #390).
- ✅ Addendum de censo/registro/crosswalk/matrices — commit `c074c20` (obs #392).
- ✅ Resolución documental de 4 preguntas puntuales (12 vs 13 skills, promotion_state de skill-creator/image-generation, límites reales de `log-tool-usage.sh`) — commit `56633d1`.
- ✅ CC-R1: remediación de 3 defectos de CX-1 (checksum, `scan_delta.py` no reproducible, identidad de grafo) — commit `fa640a5`.
- ✅ CC-H1: plan de remediación (no implementación) de defectos de evidencia en el harness HLC específico de Concierge F1B — commit `cedb54a`.
- ✅ CC-R2: cierre del contrato de checksum/manifest de SFV5, retirado el claim "20/20 PASS", desglose honesto 17 PASS + 1 PARTIAL + 1 CORRECTED + 1 NOT_APPLICABLE — commit `0bfc5c9`. **Verificado independientemente por CX-R2 (`60316d9`): `SFV5_AUDIT_INDEPENDENTLY_VERIFIED`.**
- ✅ CC-2: reconciliación completa de la arquitectura laboral DFL (Workforce Registry + Factory Manager + WorkUnits/HLC + BOS + Engram + grafo), 19 activos inventariados, composición híbrida decidida como borde vivo (sin runtime nuevo) — commit `5e30326`.
- ✅ CC-3: PRP ejecutable de Workforce Registry Unit v0.1 (schema, adapter SFV5, Registry mínimo, validator, query consumer, blind discovery test de 8 casos, 22 gates) — commit `4dfb07d`. Validado por CX-N1 (`b902bc9`, decisión `REVISE_TO_REGISTRY_WITH_SFV5_ADAPTER`, 39/40).
- ✅ CC-PRP-R1: reconciliación por delta del PRP con los resultados del laboratorio de gobierno de mutaciones (16/16 escenarios) — modelo de proposal/validation/approval/commit, 6 actores tipados, versionado append-only, prohibición de hard delete, `wru-draft.md` preparado (no colocado aún en SFV5) — commit `500c0a1`.
- 🔲 `wru-draft.md` pendiente de `CX-PRP-1 independent review` y, tras eso, de colocarse en `.claude/PRPs/wru-draft.md` de SFV5 y someterse vía `/primer` + `/prp`.
- 🔲 CC-H1 (remediación del harness F1B) quedó como plan documentado, no implementado — pendiente de decisión de si se ejecuta.

## Next Steps
- Esperar/verificar `CX-PRP-1 independent review` sobre `500c0a1` antes de someter `wru-draft.md` a SFV5.
- Si CX-PRP-1 aprueba: colocar `wru-draft.md` en `.claude/PRPs/` de SFV5 y ejecutar `/primer` + `/prp` para iniciar la fabricación real (fuera de esta cadena de diseño).
- Decidir si se retoma la implementación del plan de remediación de CC-H1 (harness F1B) — quedó como diseño, no ejecutado.
- `push_mirror.sh` no se ejecutó en ningún punto de la sesión — pendiente para cuando Jorge lo autorice explícitamente (ejecutado recién al cierre de hoy, ver línea MIRROR reportada).

## Relevant Files
- `evidence/sfv5-forensic-inspection-2026-07-30/` — informe original + addendum + 2 rondas de remediación (r1, r2) + resolución documental.
- `evidence/sfv5-forensic-inspection-2026-07-30-cx{1,r1,r2}/`, `evidence/concierge-f1b-finalization-2026-07-30-r2{,-cx1,-remediation-h1}/` — revisiones independientes de CX y remediación de HLC F1B.
- `evidence/dfl-workforce-architecture-reconciliation-2026-07-30/` — reconciliación arquitectónica completa (CC-2).
- `evidence/dfl-first-workforce-increment-review-2026-07-30/` — validación CX-N1 del primer incremento.
- `evidence/workforce-registry-unit-v0.1-prp-2026-07-30/` — PRP original (CC-3).
- `evidence/workforce-registry-capability-lab-2026-07-30/` — laboratorio experimental de gobierno de mutaciones (CX-LAB-1).
- `evidence/workforce-registry-unit-v0.1-prp-r1-2026-07-30/` — PRP reconciliado con el laboratorio, incluye `wru-draft.md` listo para SFV5.

### SESSION SUMMARY 2026-09-01 (Claude/TCC) — @$fin, full-day handoff
**Type:** decision  
**Project:** dfl  

Full-day Claude Code (TCC) session, 2026-09-01, closing via @$fin. Consolidates and cross-references obs #652 through #660 (each mission's full narrative lives in its own observation and doc; this is the session-level index, not a replacement).

CHRONOLOGY / MAJOR THREADS (in order):

1. WEGLOT/BILINGUAL WEBSITE (obs #652, docs/DFL_WEBSITE_BILINGUAL_STRATEGY.md): audited Weglot on the live DFL Squarespace site (data-wg-notranslate markup confirmed, but zero live second-language content today). No plan bought/upgraded. Recommended DFL-owned /en page tree (Option B) over Squarespace-native (doesn't exist) or another paid tool (Option C, rejected). Migration NOT executed (needs a live Squarespace admin session this environment didn't have). Built and REAL-E2E-verified a vendor-lifecycle AQA gate (tools/dfl-website-manager/vendor-lifecycle.mjs + vendor-registry.json): a vendor with an active trial and unverified end date is CRITICAL by design, not OK-by-default. Real cron installed (0 8 * * *). Trial risk is now a tracked operational task, not silent marketing noise -- but not eliminated (real trial end date still unconfirmed).

2. WEBSITE MANAGER NOTIFICATION STORM (obs #653, docs/DFL_WEBSITE_MANAGER_NOTIFICATION_STORM_2026-09-01.md): HIGH severity real incident. sweep.mjs's SLA-breach loop had no idempotency check -- isBreached() compared against a clock start that never advanced, so notify()+markNotified() fired unconditionally on every 10-min cron cycle. 61 runs -> 162 duplicate Telegram alerts over ~10h on 3 real events (2 Weglot-domain, 1 Gemini-domain, both misclassified channel=unknown->ESCALATE). Matches the reported "~200 messages, Weglot/Gemini >50 each." Duplicate ingestion: NO (each email ingested once). Duplicate escalation: YES. Root cause: a persistent SLA-breached STATE was reinterpreted as a new EVENT on every sweep. Contained immediately (removed the cron entry right after the last scheduled run). Fixed for real: ESCALATED/ROUTED -> SLA breach -> notify ONCE -> AWAITING_HUMAN (a status absent from the SLA-clock map, so isBreached() is structurally false for it forever after -- the state machine itself is the guard, no separate cooldown table). Retry made per-row/isolated so a real delivery failure retries cleanly without duplicating events. classify.mjs gained sender-domain-based vendor_lifecycle/vendor_marketing channels (AUTO_RESOLVE, never escalate) for Weglot/Gemini specifically. Reprocessed the 3 stuck real events (reclassified + closed NO_ACTION_NEEDED). sweep.mjs/classify.mjs/store.mjs had ZERO test coverage before this incident -- itself part of the root cause; added sweep.test.mjs (8 tests incl. the 100-consecutive-sweep adversarial -> max 1 Telegram send) and classify.test.mjs (7 tests). Full suite grew from 127 to 143, all PASS. Cron restored with the fix; crontab diffed byte-identical to pre-incident except the intentional vendor-lifecycle addition -- TCC/TCX/telegram-bos paths confirmed untouched throughout.

3. NODE PASS != GRAPH PASS != LOOP PASS AQA (obs #654 + #656, docs/DFL_STATE_GRAPH_LOOP_AQA_2026-09-01.md + docs/DFL_LOOP_TAXONOMY_2026-09-01.md): elevated the storm incident into institutional AQA doctrine. Audited existing organs first (asset-index dfl.yaml schema, aqa-kit's 8 Test Profiles + sentinel-test discipline, tools/lib/silence_watchdog.py -- a shared Python watchdog primitive already correctly implementing fresh-transition-only notify, reused by 3 real crons before this incident ever happened). Added RECURRING_CAPABILITY as a 9th real AQA Test Profile (tools/aqa-kit/lib/profiles.mjs), same mandatory schema as the other 8, with a real sentinel (fixtures/sentinels/recurring-capability-sweep.mjs: 'vulnerable' mode reproduces the exact storm shape and correctly FAILs; 'fixed' mode runs the real shipped sweep.mjs and PASSes). Directive addendum v0.3 added (docs/standards/aqa/DFL_AQA_PRODUCTION_DIRECTIVE_V0.1.md section 12): a passing component set does not imply a passing graph; N>=100 consecutive-cycle adversarial established as institutional minimum. Deepened further: formalized LOOP = reference+sensor+comparator+actuator+re-entry+state+continuation/exit-condition as real code (tools/aqa-kit/lib/loop-behavior.mjs: classifyLoopBehavior() classifies CONVERGED/BOUNDED_RETRY/STABLE_NO_EFFECT/DEGENERATE/DIVERGENT/OSCILLATING from a real per-cycle effect sequence), validated all 6 mission-required loop types (A-F) with ONE shared generic harness (loop-behavior.test.mjs, 12/12 PASS). Honest finding: types E (degenerate/real bug) and F (legitimate feedback loop) can be statistically indistinguishable by count alone -- distinguishing them needs a semantic check (fresh external evidence vs. stale internal replay), which directly motivated reclassifying check-site-health.mjs as NEEDS_LOOP_AQA (structurally a legitimate Type-F candidate, but its per-minute re-alert has no governed reminder-throttle policy -- recommended, not executed). Graphify tested for real (not assumed) across every mode including watch/querylog/diagnose-multigraph: contributes exactly the static code/dependency plane, zero temporal/state signal in ANY configuration -- gap documented, no replacement built. Built a real 8-plane multi-plane observability matrix from what actually reconstructed the storm (temporal history + state graph + external effects; Graphify contributed nothing to that specific diagnosis). Reclassified the 3 prior NEEDS_GRAPH_AQA items with real code reads: jpi-autonomy's reservation-watchdog.mjs and runtime.mjs -> LOOP_AQA_COVERED (real wasAlready guard / self-documented per-flow idempotency markers); check-site-health.mjs stays NEEDS_LOOP_AQA with a precise reason now, not a vague one.

4. JORGE'S LAZO->GRAFO DE LAZOS->GRAFO DE GRAFOS->BUSINESS OS THESIS (obs #655): mid-mission correction on Daniel's course material (private, saasfactory.so classroom, not locally accessible). Jorge's 6 corrections absorbed and codified into the AQA directive §12: the enriched 7-part loop formula (4 classic cybernetic pieces = HOW it regulates; 3 added pieces -- re-entry/state/continuation-exit-condition -- = WHETHER repeating converges/stabilizes/oscillates/explodes), and the correction that Graphify is "one possible instrument to observe parts of the system," not "the tool for loops" -- independently confirmed by this session's own live Graphify testing the same day.

5. CAPABILITY ACQUISITION AQA (obs #657, docs/DFL_CAPABILITY_ACQUISITION_AQA_2026-09-01.md): a third AQA institutionalization axis -- not "does it loop correctly" but "was it actually acquired, or did it just work once." Maturity ladder SEEN->EXECUTED->ACQUIRED->REUSABLE->TRANSFERABLE->GENERALIZED with objective per-transition evidence, codified as a 10th real Test Profile CAPABILITY_ACQUISITION (T0-T6 required_checks). PORTABLE != ACQUIRED != TRANSFERABLE demonstrated with a real DFL example (Skill Dock's artifact-copy tests prove portability, not acquired-usage-knowledge). Three honest benchmarks: Skill Dock (REUSED/TRANSFERRED real but its genericity was never turned into a permanent regression test -- a real, own-goal finding); Graphify (the strongest result -- a real, organic T5 negative case: this session never reached for Graphify during the actual storm diagnosis, chose logs+DB+Telegram on its own); Loop AQA (T3 executed live in-mission via a real cross-repo transfer test against JPI's real reservation-watchdog.mjs, unmodified, 1/1 PASS). Intelligence Memory format proposed: SITUATION->CAPABILITY CHOSEN->WHY->RESULT->EVIDENCE->LIMIT DISCOVERED (not a new mechanism, just a shape for Engram entries, applied retroactively as an example). Real motivating case Jorge gave live: Realtor's WhatsApp two-way capability should be "build once, use N->infinity times" -- verified via asset-index it was NOT registered as reusable (0 search results), directly setting up threads 6-8.

6. WHATSAPP CAPABILITY EXTRACTION (obs #658, docs/DFL_WHATSAPP_CAPABILITY_EXTRACTION_2026-09-01.md): extracted Realtor's real, production-proven (deployment dpl_GAjXbwt8R1sGdE3ruCeSsNsg5xd7) Meta WhatsApp Cloud API adapter into tools/messaging-adapters/meta-whatsapp/adapter.mjs, the canonical single source (dfl.messaging-adapters.meta-whatsapp.v1). Before writing code, found the previous extraction attempt (tools/mercader-autonomy/messaging_meta_whatsapp.mjs, claiming "institutional" status in its own comment) had ALREADY silently drifted from Realtor's real version and had ZERO real consumers -- exactly the PORTABLE-not-ACQUIRED trap the capability-acquisition ladder (thread 5) was built to catch, found failing for real inside DFL's own codebase. Reconciled rather than re-forked: Realtor's proven behavior as base + 2 additive generalizations (health(), channel tagging) merged from the abandoned copy. Realtor kept working, PROVEN not assumed (baseline captured before any change, its 3 own test files re-run identical after the sync). Built sync.mjs+consumers.mjs+drift-check.test.mjs -- the actual fix for the root cause (a permanent test that would have caught the original drift, proven both directions: fails on real pre-existing drift, passes after sync). MERCADER rewired to import canonical directly (T2, 24/24 PASS, zero regression).

7. MULTI-PRODUCT WHATSAPP TRANSFER (obs #659, docs/DFL_WHATSAPP_MULTI_PRODUCT_TRANSFER_2026-09-01.md): extended the canonical to JPI (greenfield, built scripts/jpi-autonomy/whatsapp-adapter.mjs mapping WhatsApp replies to flow-aceptacion.mjs's own pre-existing, self-documented acceptance-evidence gap; 8/8 new + 255/255 full JPI suite PASS), JackyClean (found ANOTHER independently drifted hand-copy of the same abandoned mercader fork, reconciled identically, real domain code notify.ts/messaging_surface.mjs preserved untouched), and DFL Website (functions/whatsapp-intake/, reusing the exact existing functions/challenge-intake satellite-Vercel-function pattern rather than inventing one, since Squarespace itself cannot host a backend -- confirmed same day in thread 1). RSVP investigated and found genuinely BLOCKED at that point: a shallow clone showed a pure client+Supabase frontend with zero server API routes and a Python backend with no code in the repo -- correctly NOT forced. T6 left UNPROVEN for all 5 real consumers without exception, honoring the mission's explicit anti-inflation rule even under momentum toward a clean sweep.

8. RSVP BACKEND RESOLUTION (obs #660, docs/DFL_RSVP_WHATSAPP_BACKEND_2026-09-01.md): a FULL (not shallow) clone revealed RSVP's Python backend and an old /api/rsvp route were real once, and were DELIBERATELY DELETED 2026-08-17 as genuinely dead code (git blame confirmed, real commit message: "nothing in the app called them, writes already blocked by RLS"). Correctly did NOT recover them -- that would have reversed a real, correct decision. Built the real minimal backend instead using RSVP's own existing framework (Next.js App Router route.ts, zero new Vercel infra) -- proven with an actual `npm run build` (not just type-check), confirming the new route compiles as a real dynamic Vercel function alongside all 10 pre-existing routes with zero regression. Canonical wired via the same sync mechanism, zero hand-copying. Honest schema-level finding, not worked around: RSVP's real schema requires an auth.users-backed identity for every rsvps write and has no phone/guest column anywhere -- so no RSVP-state-writing "confirm via WhatsApp" flow was invented; only the safe verify->normalize->read-only-lookup->human-handoff path was built and proven E2E (11/11 new tests PASS). drift-check.test.mjs extended to support a genuinely different case (RSVP is deliberately non-VM2-resident per the externalization pattern) -- a missing external consumer now SKIPS rather than FAILS, verified in both directions. Before pushing the finished work to GitHub (the only way to durably preserve it, since RSVP is intentionally not kept locally), noticed RSVP was outside today's authorized dispatch scope and used AskUserQuestion rather than assuming -- Jorge explicitly authorized a new branch push, not a merge/deploy. Pushed feat/whatsapp-webhook-intake for real (commit 55c82f7) to github.com/DFLghub/event-rsvp-waitlist, NOT merged to main, no Meta credentials anywhere.

INSTITUTIONAL DOCTRINE THAT DIDN'T EXIST THIS MORNING AND EXISTS NOW:
- AQA directive §12 (v0.3): NODE PASS != GRAPH PASS, cycle/graph audit mandatory for recurring capabilities, N>=100 adversarial baseline, the 7-part cybernetic loop formula.
- tools/aqa-kit/lib/profiles.mjs: RECURRING_CAPABILITY (9th) and CAPABILITY_ACQUISITION (10th) Test Profiles, both with real sentinel tests, not just declared.
- tools/aqa-kit/lib/loop-behavior.mjs: the generic loop-behavior classifier + harness, reused by 3 real fixtures across 3 different repos (saas-factory's own storm sentinel, the synthetic A-F taxonomy tests, and the real cross-repo JPI transfer test).
- dfl.messaging-adapters.meta-whatsapp.v1: DFL's first fully-canonicalized, multi-consumer, drift-guarded shared capability with a real generalized N-consumer sync mechanism (consumers.mjs + sync.mjs + drift-check.test.mjs) -- 6 real consumers (Realtor, MERCADER, JackyClean, JPI, DFL Website, RSVP), each with distinct, non-inflated evidence.
- The `external: true` consumer pattern (skip-not-fail drift-check) for VM2-externalized products, extending the existing externalization doctrine into the drift-check mechanism for the first time.

WHAT STAYED HONESTLY UNPROVEN OR BLOCKED (do not silently claim these tomorrow):
- Weglot's real trial-end date (no admin/IMAP access this session).
- The actual EN/ES Squarespace migration (proposal ready, not executed -- needs a live Squarespace admin session).
- check-site-health.mjs's reminder-throttle policy (recommended, not built -- real per-10-min re-alert while a real outage persists, structurally Type-F-legitimate but ungoverned).
- T6 (independent/spontaneous capability invocation) for EVERY capability discussed today, without exception -- everything was explicitly directed.
- Skill Dock's cross-generation genericity is real (one proven event) but not covered by a permanent regression test.
- RSVP's WhatsApp-driven RSVP state writes (confirm/change/cancel) and outbound (invitations/reminders) -- both require a real phone-to-identity linking decision nobody has made; deliberately not built.
- Which of the 6 WhatsApp consumers (if any) should actually go to Meta production -- zero credentials configured anywhere, that's Jorge's call.
- MERCADER's own human/external gates (unrelated to today's work, still open from before).
- Realtor's final human verification + the OWNER_PASSWORD rotation (unrelated to today, still open from before, referenced in HANDOFF-TCC-SESSION-2026-08-30.md).

DO-NOT-REPEAT LIST (institutional, for any future session, any Tony):
- Do not hand-copy tools/messaging-adapters/meta-whatsapp/adapter.mjs into a new product ever again -- always use sync.mjs + consumers.mjs. Two real hand-copies (MERCADER's original, JackyClean's) already drifted silently before this was caught; a third is not acceptable.
- Do not declare a recurring capability (cron/watcher/retry/SLA/heartbeat) safe from a single successful run -- always run the N>=100 adversarial via loop-behavior.mjs's harness or an equivalent, per RECURRING_CAPABILITY.
- Do not declare T6 (spontaneous invocation) PASS for anything that was explicitly requested -- UNPROVEN is the honest default until real unprompted evidence exists.
- Do not revive RSVP's Python backend or /api/rsvp route -- confirmed real dead code, deliberately removed 2026-08-17, do not resurrect without a NEW real reason.
- Do not push/deploy any of the 6 WhatsApp consumers to Meta production without Jorge's explicit credential/gate authorization.
- Do not assume a product's git-visible architecture reflects a shallow clone's view -- RSVP's real history (a full clone) told a completely different story than the shallow depth-1 clone from the prior mission the same day.

Full doc index for tomorrow: docs/DFL_WEBSITE_BILINGUAL_STRATEGY.md, docs/DFL_WEBSITE_MANAGER_NOTIFICATION_STORM_2026-09-01.md, docs/DFL_STATE_GRAPH_LOOP_AQA_2026-09-01.md, docs/DFL_LOOP_TAXONOMY_2026-09-01.md, docs/DFL_CAPABILITY_ACQUISITION_AQA_2026-09-01.md, docs/DFL_WHATSAPP_CAPABILITY_EXTRACTION_2026-09-01.md, docs/DFL_WHATSAPP_MULTI_PRODUCT_TRANSFER_2026-09-01.md, docs/DFL_RSVP_WHATSAPP_BACKEND_2026-09-01.md. IRONMAN.md has one real row per mission (10 rows added today). Engram obs #652-#660 plus this index (#661).

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

### RSVP backend/runtime resolved + Meta-WhatsApp canonical wired, real E2E, branch pushed not merged (2026-09-01)
**Type:** architecture  
**Project:** dfl  

Direct follow-up to the multi-product WhatsApp transfer mission (obs #659), which left RSVP BLOCKED based on a shallow (depth-1) clone that showed no server-side API routes and a Python backend with no application code. Jorge asked to actually resolve this: discover the real history, recover if recoverable, otherwise build the minimal viable backend using an already-proven DFL pattern, then wire the canonical adapter.

REAL DISCOVERY (full git clone this time, not shallow): backend/main.py (a real FastAPI app) and a Next.js /api/rsvp route DID exist historically, and were DELIBERATELY DELETED on 2026-08-17 in commit 9455aa6 ("Remediate Jose's Event RSVP MVP findings ... dead-code decommission"), with the commit message stating plainly: "nothing in the app called them, and their writes were already blocked by RLS." The real waitlist auto-promotion race-safety now lives entirely at the Postgres/RLS/constraint level, not in an application backend.

DECISION, evaluated with evidence not skipped: do NOT recover the Python backend -- reviving it would reverse a real, correct, already-documented decision, not fill an actual gap. Instead BUILD the minimal viable backend using the pattern DFL has already proven repeatedly today (JPI, JackyClean): RSVP already uses Next.js App Router, so a native API route (src/app/api/whatsapp-webhook/route.ts) is zero new infrastructure -- it deploys as part of the exact same Vercel "frontend" service already live (vercel.json's services.frontend.framework: nextjs), not a new service block.

CANONICAL WIRED, no core logic duplicated: src/lib/messaging/messaging_meta_whatsapp.mjs is a byte-identical, drift-checked sync of tools/messaging-adapters/meta-whatsapp/adapter.mjs via the same sync.mjs mechanism already used for the other 4 real consumers -- never hand-copied or edited.

HONEST SCHEMA-LEVEL FINDING (not ignored, not worked around by inventing something): RSVP's real schema (supabase/migrations/001_create_schema.sql) requires every rsvps row to reference a real auth.users-backed user_id -- there is NO phone/contact column anywhere on users or rsvps, and no guest-RSVP path exists. This means a WhatsApp message from an unauthenticated phone number cannot safely trigger a real RSVP state write (confirm/change/cancel) without first inventing a phone-to-account identity-linking mechanism -- a real product/schema decision this mission explicitly did NOT make unilaterally, honoring the mission's own "no inventes funciones que RSVP no requiera" rule. Symmetrically, no outbound (invitation/reminder) was built either, since there's nowhere to store a phone number to send to.

WHAT WAS ACTUALLY BUILT AND PROVEN E2E, safely, with zero new write surface: verify Meta signature (real HMAC) -> normalize via canonical adapter -> optional READ-ONLY lookup of a referenced event (using the exact same anon-key + RLS boundary every other read in the app already goes through, no privilege escalation) -> produce a human-handoff record (handoff always 'HUMAN_REQUIRED', never auto-processed). Same minimal shape already proven for the DFL website's own WhatsApp intake in the prior mission.

REAL EVIDENCE, not just declared: ran an actual `npm run build` (not just type-check) -- confirmed /api/whatsapp-webhook compiles as a genuine dynamic Vercel-function route alongside all 10 pre-existing routes, zero regression. `npm run type-check` clean, matching the exact baseline captured before any change. Added a `test` script (tsx --test) to package.json since none existed before -- 11/11 new tests PASS across rsvp-whatsapp.test.ts (3) and route.test.ts (4), plus the canonical adapter's own coverage.

DRIFT-CHECK MECHANISM EXTENDED for a genuinely different case: RSVP is deliberately NOT VM2-resident (docs/patterns/vm2-externalization/METHOD.md -- its local copy was intentionally removed from VM2 on 2026-08-21), unlike the other 4 consumers which are permanent local checkouts. Registered in consumers.mjs with `external: true`; drift-check.test.mjs updated so a missing target path for an `external` consumer reports SKIPPED (informational), never FAILED -- while the other 4 still hard-fail if their copy goes missing. Verified in BOTH directions this session: with the temp clone present, 17/17 PASS including a real byte-identity check against RSVP; after deleting the temp clone (end of mission, per the externalization pattern's own expectation that physical VM2 residence is temporary), re-ran the suite -- 16 PASS + 1 SKIPPED, nothing broken.

GOVERNANCE MOMENT, handled correctly: before pushing the finished branch to GitHub (the only way to durably preserve real work done in a temporarily-rematerialized clone, per the externalization pattern's own logic), noticed RSVP was NOT in today's explicit authorized dispatch scope (the /go payload seen earlier only listed saas-factory, JPI, and whatsapp-realtor-mvp). Used AskUserQuestion rather than assuming -- asked Jorge directly whether to push a new branch or hold. Jorge explicitly authorized pushing a new branch (not a merge, not a deploy). Pushed `feat/whatsapp-webhook-intake` for real to github.com/DFLghub/event-rsvp-waitlist (commit 55c82f7) -- NOT merged to main, no Meta credentials configured anywhere, matching the mission's "no producción sin gate" instruction. A Vercel preview build may auto-generate on the branch per standard GitHub integration behavior, but with no real Meta secrets the route would just answer 503.

dfl.yaml's evidence field updated with RSVP as a real 6th consumer entry (Realtor, MERCADER, JackyClean, JPI, DFL Website, now RSVP), each with distinct, real, non-inflated evidence. Asset-index regenerated. T6 (independent invocation) stays UNPROVEN for RSVP too, consistent with all 5 other consumers -- this was explicitly directed work, not spontaneous.

Files: docs/DFL_RSVP_WHATSAPP_BACKEND_2026-09-01.md (full report), tools/messaging-adapters/meta-whatsapp/{consumers.mjs,drift-check.test.mjs,dfl.yaml} (extended), and on the RSVP repo itself (branch feat/whatsapp-webhook-intake, not in this VM's filesystem after cleanup -- recoverable via `git clone git@github.com:DFLghub/event-rsvp-waitlist.git` then `git checkout feat/whatsapp-webhook-intake`): src/app/api/whatsapp-webhook/{route.ts,route.test.ts}, src/lib/messaging/{messaging_meta_whatsapp.mjs,rsvp-whatsapp.ts,rsvp-whatsapp.test.ts}, package.json (test script + tsx devDependency added). IRONMAN.md row added.

### Meta-WhatsApp capability transferred to 3 new real products (JPI, JackyClean-reconciled, DFL Website) + RSVP honestly BLOCKED (2026-09-01)
**Type:** architecture  
**Project:** dfl  

Direct follow-up to the WhatsApp extraction mission (obs #658). Jorge asked to demonstrate real multi-product transfer of dfl.messaging-adapters.meta-whatsapp.v1 to JPI, JackyClean, RSVP, and (added mid-mission) DFL Website -- with the explicit rule that discovery must come first per product (don't assume identical flows) and no divergent copies may be created.

DISCOVERY-FIRST FINDING, real and repeated: JackyClean ALREADY had its own WhatsApp integration, and it had ALREADY drifted. tools/jackyclean-messaging/messaging_meta_whatsapp.mjs was a hand-copy of the ABANDONED tools/mercader-autonomy fork (same content, same "for MERCADER" comment left unedited, not even updated for the new owner) -- i.e. the disease from the previous mission (obs #658) had already spread to a second product before this mission started. Reconciled it the exact same way as MERCADER: added to consumers.mjs, synced from canonical, deleted the redundant local test file, corrected a comment that falsely claimed the copy was "the same capability MERCADER already proved in production" (MERCADER's copy never had real production evidence, per obs #658). JackyClean's real domain code (notify.ts's notifyCustomerAboutBooking, messaging_surface.mjs, the comms-registry wiring) was NOT touched -- it was already well-designed (fail-closed, best-effort non-blocking); only its adapter SOURCE was fixed. Real gap found (not created by this mission, just observed): notify.ts calls adapter.send({recipient: ''...}) -- never actually wired to a real customer phone number yet.

JPI was genuinely greenfield (zero WhatsApp/Meta/comms-registry references anywhere in the repo) but its own code already declared the exact real gap: flow-aceptacion.mjs's header comment says plainly "JPI has no real acceptance-channel ingestion wired yet" and documents the exact evidencia shape ({canal, referencia, texto, timestamp}) ejecutarRegistroAceptacion() already consumes. Built scripts/jpi-autonomy/whatsapp-adapter.mjs (thin, real): inboundEventToAceptacionEvidencia() maps a canonical normalized WhatsApp event to that exact pre-existing shape; findSolicitudByPhone() does a read-only lookup against contacto_telefono (a real column since migration 002, not invented) with the SAME fuente_datos='sintetico' safety filter every other real query in that directory already uses; cotizacionToOutboundMessage() stays deliberately generic (no fabricated price breakdown -- cotizaciones has no header-level monto column). 8 new tests (T1/T3/T5 real, T4 partial via phone-format-tolerance test), full jpi-autonomy suite re-run: 255/255 PASS, zero regression.

RSVP investigated with a real read-only git clone (github.com/DFLghub/event-rsvp-waitlist, deleted after this mission per residence convention -- it had been deliberately externalized from VM2 on 2026-08-21). Found genuinely BLOCKED, not a capability gap: the deployed Next.js frontend has ZERO server-side API routes (src/app/**/route.ts -> 0 results, confirmed by direct find) -- it's a pure client+Supabase app; the Python backend declared in render.yaml (backend.main:app via gunicorn) has NO application code checked into the repo (only Procfile/requirements.txt/seed_demo_data.py). There is structurally no Node.js server surface to host a thin WhatsApp adapter without first deciding architecture (add a new Next.js API route? actually build the missing Python backend and port the contract cross-language?) -- a real product/architecture decision that belongs to Jorge, not inferred or forced. Registered as BLOCKED_NO_SERVER_SURFACE, T1-T6 all N/A for RSVP, nothing built there.

DFL WEBSITE (added mid-mission, section 13 of the brief): the corporate site itself is Squarespace with no custom backend (confirmed the same day in the Weglot/bilingual mission) -- but the repo already has the exact right pattern for giving it one: functions/challenge-intake/ in the dfl-website-t3 worktree, a genuinely isolated, standalone-deployed Vercel Node function ("zero dependency on the Astro site's build"), already real and cron-scheduled. Reused that pattern rather than inventing a new one: functions/whatsapp-intake/handler.mjs, mirroring challenge-intake's own Web-standard Request/Response contract and "invent no infrastructure that isn't provisioned yet" discipline (no store, no notify wiring -- just verify signature, normalize inbound, tag provenance dfl-website:whatsapp-intake, force handoff:'HUMAN_REQUIRED' since this file deliberately never auto-replies with business content, matching the mission's explicit rule E "no convertir el Website Manager en ejecutor comercial general"). 7 new tests PASS; confirmed challenge-intake itself untouched and still 19/19 PASS.

MECHANISM GENERALIZED before touching any product (real infra work, not just documentation): tools/messaging-adapters/meta-whatsapp/sync-to-realtor.mjs (single-consumer) replaced by sync.mjs + consumers.mjs (a real registry of N consumers, each entry requiring a verified `reason` for why vendoring is needed, not assumed) + drift-check.test.mjs generalized into a loop over CONSUMERS -- adding a new consumer requires zero new test code. Proven with the 4 real consumers registered this mission (Realtor, JackyClean, JPI, dfl-website-whatsapp-intake): 15/15 PASS. This is literally "1 source of truth, N consumers, any divergence detected automatically" -- the exact closing criterion the mission asked for.

T0-T6 applied per product, honestly, per the mission's explicit anti-inflation instruction: T6 (independent/spontaneous invocation) left UNPROVEN for all 5 real consumers (Realtor, MERCADER, JackyClean, JPI, DFL Website) without exception -- every single one was explicitly directed by Jorge this session or the previous one, none qualifies as spontaneous selection. Not inflated even under mission pressure to declare a full "PASS" ladder.

dfl.yaml's evidence field rewritten to list all 5 real consumers with per-consumer evidence (not a bare claim of "N consumers") -- RSVP explicitly NOT listed as a consumer since it isn't one yet. Asset-index regenerated and confirmed discoverable.

Scope discipline: no Meta production credentials configured for any of the 3 new consumers, no webhook URL registered with Meta, no deploy/push executed anywhere (JPI, JackyClean worktree, or dfl-website-t3 worktree) -- matches the mission's explicit "NO PRODUCCIÓN SIN GATE, local/staging/integration first" instruction. All new code is real, tested, and ready pending Jorge's explicit production authorization.

Files: docs/DFL_WHATSAPP_MULTI_PRODUCT_TRANSFER_2026-09-01.md (full report), tools/messaging-adapters/meta-whatsapp/{consumers.mjs,sync.mjs,drift-check.test.mjs,dfl.yaml} (extended/generalized), scripts/jpi-autonomy/whatsapp-adapter.mjs + test/whatsapp-adapter.test.mjs (new, JPI repo), functions/whatsapp-intake/{handler.mjs,handler.test.mjs} (new, dfl-website-t3 worktree), tools/jackyclean-messaging/{messaging_meta_whatsapp.mjs (reconciled),messaging_surface.mjs (comment corrected)} (jackyclean worktree), IRONMAN.md row added.

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

- **Community 11** (98 nodes): PRP Estructura y Dependencias, Capacidades de Onboarding de DFL, Dependencias de Fabricación
- **Community 0** (7 nodes): Jurisdicción, Mercader, Observación de Ed
- **Community 1** (5 nodes): Merchant of Record, Métricas comerciales, Integraciones Externas
- **Community 2** (4 nodes): MCP Server Behavior, RLS Trap, Cardinalidad de Inventario
- **Community 3** (4 nodes): Abstracción de oferta, Modelo de disponibilidad
- **Community 4** (4 nodes): Verificación de API, Estrategia PRP

---

*Mirror auto-generated 2026-09-02T00:04:28Z | La Garra → DFLghub/amos-context*
