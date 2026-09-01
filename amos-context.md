# amOS Context — @$go Live Mirror
**Generated:** 2026-09-01T15:40:48Z  
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

### P11 — Weglot dependency audit + free bilingual strategy + vendor-lifecycle gate (2026-09-01)
**Type:** decision  
**Project:** dfl  

Mission: eliminate Weglot recurring-license dependency on the DFL website and design a free EN/ES bilingual strategy, without buying/upgrading any Weglot plan and without introducing another paid SaaS.

AUDIT (real, from live production HTML fetch 2026-09-01): www.deepfeelingslabs.com's rendered HTML contains a language-picker block with `data-wg-notranslate` markup -- Weglot's own DOM signature, delivered via Squarespace's Weglot Extensions-marketplace integration. The picker's dropdown currently shows only "Español" (empty language-picker-content); `/en` and `/en/` both return HTTP 404; no hreflang alternates exist. Conclusion: zero live bilingual content depends on Weglot today -- it's wired into the theme but no second language is published. Could NOT verify the exact trial expiration date this session: no live authenticated Squarespace session (127.0.0.1:9223 CDP port refused connection), no browser binary installable (chromium not present, `npx playwright install chrome` failed for lack of root/sudo password), and no GARDIPEDIA_IMAP_APP_PASSWORD available to read the Weglot trial/billing emails referenced in prior sessions' inbox scans. This is an explicit, named gap, not silently assumed safe.

STRATEGY (designed, not yet executed in production): compared A (Squarespace native multi-language -- doesn't exist independent of an extension like Weglot), B (DFL-owned manual /en page tree: duplicate the current ~8 pages, real English copy, native link-block switcher ES|EN, hreflang via page SEO/code injection -- $0 recurring, full DFL ownership, real per-URL SEO) and C (another free/OSS widget e.g. GTranslate -- rejected, same SaaS-lock-in/MT-quality problem the mission explicitly ruled out). Recommended and documented: Option B. Full plan with concrete 7-step migration sequence written to docs/DFL_WEBSITE_BILINGUAL_STRATEGY.md. NOT executed against production -- needs a live Squarespace admin editing session (unavailable this session), per the mission's own instruction to propose an architectural decision before touching production.

BUILT AND REAL-E2E-VERIFIED this session (step 7 of the mission, done in full): tools/dfl-website-manager/vendor-lifecycle.mjs + vendor-registry.json (manually maintained vendor facts, no vendor billing API exists) + vendor-lifecycle.test.mjs. Reuses the exact classify->decide->gate->act->notify pipeline already proven by check-site-health.mjs/task-act.mjs (same repo, same author lineage) rather than inventing new architecture. Key design choice: a recurring-license vendor with an active trial and NO verified ends_at is classified CRITICAL by design (unknown runway is treated as risk, never defaults to OK) -- this is the exact fix for "vendor trial/expiration/upcoming charge must be an operational event, not marketing noise" the mission asked for. Thresholds: WARNING <=14 days, CRITICAL <=3 days or unknown ends_at, EXPIRED past ends_at. Idempotent per-vendor task (vendor-lifecycle:<id>), re-notifies only on a real status transition (verified by test: unchanged status does NOT re-notify, a transition DOES). Test suite: 12/12 new tests PASS, full tools/dfl-website-manager suite 127/127 PASS (was 42/42 at last IRONMAN mention of this tool -- grown since). Real E2E run against the live registry (Weglot, CRITICAL, unknown ends_at) created task vendor-lifecycle:weglot and sent a real Telegram alert via the existing @DFL_BOS_bot channel/notify-telegram.mjs (message id 496, verified live, not simulated). Wired into cron: `0 8 * * * ... cron-check-vendor-lifecycle.sh` (same flock+timestamped-log pattern as the other Website Manager sensors), confirmed present in the live crontab via `crontab -l` after install.

Nothing was bought, no Weglot plan was upgraded, no Squarespace content was edited, no other paid SaaS was introduced. IRONMAN.md row added (P11 -- Website: eliminar dependencia de Weglot y resolver bilingüe sin licencia). Next real step needs a human/agent with live Squarespace admin access: execute the 7-step migration in docs/DFL_WEBSITE_BILINGUAL_STRATEGY.md, or confirm the real Weglot trial end date from the account/email and update vendor-registry.json with verified data (today it is explicitly marked verification_note=unverified, not guessed).

### Paseo TCX Full Access profile configured and verified
**Type:** decision  
**Project:** saas-factory-setup  

Cierre @$fin. En VM2/PASEO_HOME=/home/dflagent/.paseo-sfv5-dev se configuró el agent profile dedicado de Paseo id=tcx-full-access, name='TCX — Full Access', provider=codex, model=gpt-5.5, modeId=full-access. La fuente instalada de Paseo 0.5.0-beta.5 confirma que full-access materializa approval_policy=never y sandbox_mode=danger-full-access. `paseo reload --format json` aplicó daemon.agentProfiles sin restartRequiredPaths. Verificación directa vía API local confirmó el perfil y los modos Codex (auto, auto-review, full-access). No se lanzó sesión desde Pixel, no se modificaron credenciales ni el provider global Codex. Próximo paso para Jorge: cerrar/reabrir Paseo en Pixel, abrir saas-factory y seleccionar TCX — Full Access; no seleccionar Codex genérico.

### WhatsApp two-way capability extracted to dfl.messaging-adapters.meta-whatsapp.v1 -- real T2/T3 evidence, drift-guard installed (2026-09-01)
**Type:** architecture  
**Project:** dfl  

Direct follow-up to the same-day CAPABILITY_ACQUISITION AQA mission (obs #657): Jorge asked to actually extract Realtor's real, production WhatsApp two-way messaging capability into a reusable DFL asset, with an explicit lock: "no extraer código porque se parece reusable... extraer el contrato mínimo que hace reusable la capacidad, mantener Realtor funcionando, y después demostrar transferencia en un segundo contexto real. Si solo sacamos archivos a otra carpeta, tendremos PORTABLE. Si otro producto puede usarla sin cirugía específica, tendremos TRANSFERRED."

CRITICAL FINDING BEFORE WRITING ANY CODE (real audit, not assumed): whatsapp-realtor-mvp/src/whatsapp-webhook.js's adapter file carried a comment claiming to be a "Deployment-local copy of DFL's institutional Meta adapter", sourced from tools/mercader-autonomy/messaging_meta_whatsapp.mjs. Verified this claim rather than trusting it: `diff` between the two files showed they had already SILENTLY DIVERGED (different internal function names, a health() method present only in the mercader copy, a `recipient` field on delivery events present only in Realtor's copy). `grep` confirmed the "institutional" copy in tools/mercader-autonomy/ had ZERO real consumers outside its own directory -- and the multi-provider registry built around it (tools/comms-registry/) also had zero real consumers beyond MERCADER itself, which itself has no live cron/daemon (confirmed earlier the same day: zero `mercader` entries in crontab -l). Diagnosis: this capability was PORTABLE (someone copied the file once) but never ACQUIRED (zero real second consumer) and its TRANSFERABLE claim was asserted in a code comment, never verified -- exactly the failure mode the CAPABILITY_ACQUISITION ladder built earlier the same day exists to catch, discovered failing FOR REAL inside DFL's own codebase before the ladder was ever formally applied to it.

DECISION: did not repeat the mistake by extracting a third copy. Reconciled the original instead.

BUILT: tools/messaging-adapters/meta-whatsapp/adapter.mjs -- the single canonical source. Content base = Realtor's version (the only one with real production evidence: deployment dpl_GAjXbwt8R1sGdE3ruCeSsNsg5xd7, real Meta Graph API round-trip, real signed webhook traffic), with exactly 2 purely-additive generalizations merged in from the abandoned mercader copy (health() method, channel:'whatsapp' tag on normalized events) -- nothing behaviorally removed or changed for Realtor's real caller. dfl.yaml manifest written using ONLY the existing asset-index schema fields (invoke/params/returns/restrictions/authority/evidence/consumer_hint) -- no new schema field added, respecting the schema-creep guard. 6/6 real tests (adapter.test.mjs) covering health/send, invalid recipient/body rejection, 401/429/5xx error mapping, real HMAC signature verification, and normalization of exact-shaped Meta payloads.

REALTOR KEPT WORKING, PROVEN NOT ASSUMED: captured a real baseline (all 3 of Realtor's own pre-existing test files run and their exact output recorded) BEFORE touching anything. Built sync-to-realtor.mjs which overwrites whatsapp-realtor-mvp/src/messaging_meta_whatsapp.mjs with a byte-reconciled, header-stamped (GENERATED DO NOT HAND-EDIT + sha256 hash) copy of the canonical file. Vendoring is a REAL infra requirement, not laziness -- confirmed whatsapp-realtor-mvp/vercel.json uses the legacy `builds` config with no workspace/monorepo setup, so Vercel's build genuinely cannot reach outside the project's own root. After running the real sync: re-ran all 3 of Realtor's own test files (tests/whatsapp-webhook.test.mjs, npm test -> tests/e2e.mjs, tests/owner-instances-legacy-exclusion.test.mjs) and confirmed output byte-identical to the pre-sync baseline -- zero regression, verified not assumed.

THE ACTUAL FIX FOR THE ROOT CAUSE (not just a one-time reconciliation): tools/messaging-adapters/meta-whatsapp/drift-check.test.mjs -- a permanent, real test comparing the stamped hash in Realtor's vendored copy against a FRESH hash of the canonical source, every run. This is the mechanism the FIRST extraction never had. Proven both directions: run BEFORE the sync, it correctly FAILED (caught the real pre-existing drift); run AFTER the sync, 2/2 PASS. This is what turns a one-time "it worked" into a durably-enforced "it stays synced."

REAL T2 (same-repo reuse): tools/mercader-autonomy/messaging_surface.mjs rewired to import the canonical module directly (one-line import path change), its own drifted duplicate (messaging_meta_whatsapp.mjs) and that duplicate's now-redundant test file DELETED rather than left to rot alongside the new canonical. Full tools/mercader-autonomy test suite re-run: 24/24 PASS, zero regression. tools/comms-registry/README.md and tools/mercader-autonomy/MESSAGING_SEAM.md references updated to point at the new canonical location.

APPLIED THE MISSION'S OWN T0-T6 EXAM TO THIS EXTRACTION, HONESTLY: T0 Recognition -- directed (Jorge asked explicitly), not spontaneous, not inflated as T6. T1 Execution -- real, 6/6 tests. T2 Same-context reuse -- real, MERCADER, 24/24 PASS. T3 Cross-context transfer -- real, Realtor is a genuinely separate repo with a real deploy constraint, zero regression proven, permanent drift-guard installed (not just a one-time event like the failed first attempt). T4 Perturbation -- partial (tested against a differently-shaped real payload, not against a different runtime/Node version). T5 Negative case -- real, dfl.yaml's `restrictions` field explicitly documents what this adapter does NOT do (persistence, correlation, dedupe -- always the caller's job). T6 Independent invocation -- explicitly left UNPROVEN, not inflated -- this extraction was requested, not spontaneously recognized.

Verified real discoverability improvement: `node tools/asset-index/query.mjs search "whatsapp meta cloud api two-way messaging"` went from 0 results (verified in the prior mission, obs #657) to 1 real result (dfl.messaging-adapters.meta-whatsapp.v1) after running discover.mjs to regenerate the index.

Scope discipline: no deploy to Vercel/production this session -- the sync only writes to Realtor's local working tree, no push, no `vercel deploy` -- that would be a separate, explicitly-confirmed action given it affects a live production app. Found (not fixed, unrelated) one pre-existing dfl.yaml parse error at docs/patterns/mercader-messaging-channel-2026/dfl.yaml during discover.mjs's run -- noted so it isn't rediscovered as a surprise later, out of scope to fix here.

Files: docs/DFL_WHATSAPP_CAPABILITY_EXTRACTION_2026-09-01.md (full report), tools/messaging-adapters/meta-whatsapp/{adapter.mjs,adapter.test.mjs,dfl.yaml,sync-to-realtor.mjs,drift-check.test.mjs} (new), tools/mercader-autonomy/{messaging_surface.mjs,MESSAGING_SEAM.md} (rewired), tools/comms-registry/README.md (references updated), whatsapp-realtor-mvp/src/messaging_meta_whatsapp.mjs (synced, only file touched outside saas-factory, only its vendored content -- webhook/engine/repository business logic untouched), tools/asset-index/index.json (regenerated), IRONMAN.md row added.

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

### WhatsApp two-way capability extracted to dfl.messaging-adapters.meta-whatsapp.v1 -- real T2/T3 evidence, drift-guard installed (2026-09-01)
**Type:** architecture  
**Project:** dfl  

Direct follow-up to the same-day CAPABILITY_ACQUISITION AQA mission (obs #657): Jorge asked to actually extract Realtor's real, production WhatsApp two-way messaging capability into a reusable DFL asset, with an explicit lock: "no extraer código porque se parece reusable... extraer el contrato mínimo que hace reusable la capacidad, mantener Realtor funcionando, y después demostrar transferencia en un segundo contexto real. Si solo sacamos archivos a otra carpeta, tendremos PORTABLE. Si otro producto puede usarla sin cirugía específica, tendremos TRANSFERRED."

CRITICAL FINDING BEFORE WRITING ANY CODE (real audit, not assumed): whatsapp-realtor-mvp/src/whatsapp-webhook.js's adapter file carried a comment claiming to be a "Deployment-local copy of DFL's institutional Meta adapter", sourced from tools/mercader-autonomy/messaging_meta_whatsapp.mjs. Verified this claim rather than trusting it: `diff` between the two files showed they had already SILENTLY DIVERGED (different internal function names, a health() method present only in the mercader copy, a `recipient` field on delivery events present only in Realtor's copy). `grep` confirmed the "institutional" copy in tools/mercader-autonomy/ had ZERO real consumers outside its own directory -- and the multi-provider registry built around it (tools/comms-registry/) also had zero real consumers beyond MERCADER itself, which itself has no live cron/daemon (confirmed earlier the same day: zero `mercader` entries in crontab -l). Diagnosis: this capability was PORTABLE (someone copied the file once) but never ACQUIRED (zero real second consumer) and its TRANSFERABLE claim was asserted in a code comment, never verified -- exactly the failure mode the CAPABILITY_ACQUISITION ladder built earlier the same day exists to catch, discovered failing FOR REAL inside DFL's own codebase before the ladder was ever formally applied to it.

DECISION: did not repeat the mistake by extracting a third copy. Reconciled the original instead.

BUILT: tools/messaging-adapters/meta-whatsapp/adapter.mjs -- the single canonical source. Content base = Realtor's version (the only one with real production evidence: deployment dpl_GAjXbwt8R1sGdE3ruCeSsNsg5xd7, real Meta Graph API round-trip, real signed webhook traffic), with exactly 2 purely-additive generalizations merged in from the abandoned mercader copy (health() method, channel:'whatsapp' tag on normalized events) -- nothing behaviorally removed or changed for Realtor's real caller. dfl.yaml manifest written using ONLY the existing asset-index schema fields (invoke/params/returns/restrictions/authority/evidence/consumer_hint) -- no new schema field added, respecting the schema-creep guard. 6/6 real tests (adapter.test.mjs) covering health/send, invalid recipient/body rejection, 401/429/5xx error mapping, real HMAC signature verification, and normalization of exact-shaped Meta payloads.

REALTOR KEPT WORKING, PROVEN NOT ASSUMED: captured a real baseline (all 3 of Realtor's own pre-existing test files run and their exact output recorded) BEFORE touching anything. Built sync-to-realtor.mjs which overwrites whatsapp-realtor-mvp/src/messaging_meta_whatsapp.mjs with a byte-reconciled, header-stamped (GENERATED DO NOT HAND-EDIT + sha256 hash) copy of the canonical file. Vendoring is a REAL infra requirement, not laziness -- confirmed whatsapp-realtor-mvp/vercel.json uses the legacy `builds` config with no workspace/monorepo setup, so Vercel's build genuinely cannot reach outside the project's own root. After running the real sync: re-ran all 3 of Realtor's own test files (tests/whatsapp-webhook.test.mjs, npm test -> tests/e2e.mjs, tests/owner-instances-legacy-exclusion.test.mjs) and confirmed output byte-identical to the pre-sync baseline -- zero regression, verified not assumed.

THE ACTUAL FIX FOR THE ROOT CAUSE (not just a one-time reconciliation): tools/messaging-adapters/meta-whatsapp/drift-check.test.mjs -- a permanent, real test comparing the stamped hash in Realtor's vendored copy against a FRESH hash of the canonical source, every run. This is the mechanism the FIRST extraction never had. Proven both directions: run BEFORE the sync, it correctly FAILED (caught the real pre-existing drift); run AFTER the sync, 2/2 PASS. This is what turns a one-time "it worked" into a durably-enforced "it stays synced."

REAL T2 (same-repo reuse): tools/mercader-autonomy/messaging_surface.mjs rewired to import the canonical module directly (one-line import path change), its own drifted duplicate (messaging_meta_whatsapp.mjs) and that duplicate's now-redundant test file DELETED rather than left to rot alongside the new canonical. Full tools/mercader-autonomy test suite re-run: 24/24 PASS, zero regression. tools/comms-registry/README.md and tools/mercader-autonomy/MESSAGING_SEAM.md references updated to point at the new canonical location.

APPLIED THE MISSION'S OWN T0-T6 EXAM TO THIS EXTRACTION, HONESTLY: T0 Recognition -- directed (Jorge asked explicitly), not spontaneous, not inflated as T6. T1 Execution -- real, 6/6 tests. T2 Same-context reuse -- real, MERCADER, 24/24 PASS. T3 Cross-context transfer -- real, Realtor is a genuinely separate repo with a real deploy constraint, zero regression proven, permanent drift-guard installed (not just a one-time event like the failed first attempt). T4 Perturbation -- partial (tested against a differently-shaped real payload, not against a different runtime/Node version). T5 Negative case -- real, dfl.yaml's `restrictions` field explicitly documents what this adapter does NOT do (persistence, correlation, dedupe -- always the caller's job). T6 Independent invocation -- explicitly left UNPROVEN, not inflated -- this extraction was requested, not spontaneously recognized.

Verified real discoverability improvement: `node tools/asset-index/query.mjs search "whatsapp meta cloud api two-way messaging"` went from 0 results (verified in the prior mission, obs #657) to 1 real result (dfl.messaging-adapters.meta-whatsapp.v1) after running discover.mjs to regenerate the index.

Scope discipline: no deploy to Vercel/production this session -- the sync only writes to Realtor's local working tree, no push, no `vercel deploy` -- that would be a separate, explicitly-confirmed action given it affects a live production app. Found (not fixed, unrelated) one pre-existing dfl.yaml parse error at docs/patterns/mercader-messaging-channel-2026/dfl.yaml during discover.mjs's run -- noted so it isn't rediscovered as a surprise later, out of scope to fix here.

Files: docs/DFL_WHATSAPP_CAPABILITY_EXTRACTION_2026-09-01.md (full report), tools/messaging-adapters/meta-whatsapp/{adapter.mjs,adapter.test.mjs,dfl.yaml,sync-to-realtor.mjs,drift-check.test.mjs} (new), tools/mercader-autonomy/{messaging_surface.mjs,MESSAGING_SEAM.md} (rewired), tools/comms-registry/README.md (references updated), whatsapp-realtor-mvp/src/messaging_meta_whatsapp.mjs (synced, only file touched outside saas-factory, only its vendored content -- webhook/engine/repository business logic untouched), tools/asset-index/index.json (regenerated), IRONMAN.md row added.

### P11 — Capability Acquisition AQA: maturity ladder, T0-T6 profile, real transfer benchmark, WhatsApp reuse gap (2026-09-01)
**Type:** architecture  
**Project:** dfl  

Third AQA institutionalization mission of the same day (chain: obs #653 storm incident -> #654 RECURRING_CAPABILITY -> #655 Jorge's LAZO thesis -> #656 LOOP AQA taxonomy -> this one). Different axis: not "does a loop converge" but "was a capability actually acquired, or does it just work once." Principle Jorge gave: a capability is not acquired because it's installed, nor because it worked once. It's acquired when the system knows WHEN to use it, HOW, and WHEN NOT TO. It's transferable when it survives a real change of context.

AUDIT FIRST: tools/asset-index/manifest.mjs's dfl.yaml schema (REQUIRED_FIELDS: asset_id/name/description/consumer_hint; CAPABILITY_FIELDS: invoke/params/returns/restrictions/surface/authority; plus evidence/status/residence) already covers almost the entire "evidence contract" the mission asked for, field-by-field: CAPABILITY_ID=asset_id, INPUT/OUTPUT CONTRACT=params/returns, DENY CONDITIONS=restrictions, AUTHORITY REQUIRED=authority (exact match), EVIDENCE CASES=evidence (exact match). No new field added -- the 3 without a dedicated slot (TRANSFER CASES/FAILURES-LIMITS/VERSION-PROVENANCE) stay as prose convention inside the existing `evidence` field (already how Skill Dock records them in practice), respecting the schema's own explicit "schema creep guard" (every field must have a stated consumer) rather than adding fields on a single case's demand.

MATURITY LADDER formalized with objective evidence per transition, not vague percentages: SEEN(installed)->EXECUTED(worked once, real case)->ACQUIRED(system recognizes when to apply + executes without full manual reconstruction)->REUSABLE(used correctly in a second comparable case, same domain)->TRANSFERABLE(survives a real context change: another repo/product/factory/runtime/generation)->GENERALIZED(a Tony spontaneously recognizes a new situation and invokes it without being told). Codified as a 10th real AQA Test Profile, tools/aqa-kit/lib/profiles.mjs:CAPABILITY_ACQUISITION, same mandatory schema as the other 9 (this is now an established, proven institutional pattern: extend the existing Test Profile organ rather than build a parallel framework, done 3 times today for 3 different concerns). required_checks are literally T0-T6 from the mission (Recognition/Execution/Same-context reuse/Cross-context transfer/Perturbation/Negative case/Independent invocation), each with its own real-evidence requirement, not just enumerated as prose.

PORTABLE != ACQUIRED != TRANSFERABLE codified with a real example: Skill Dock's test suite (tools/skills-dock/tests/sync-skills.test.mjs, 11/11) proves the ARTIFACT copies correctly between locations (PORTABLE, a property of the artifact) -- none of those 11 tests prove a Tony knows WHEN to invoke Skill Dock without being told (ACQUIRED, a property of the system using it) or that it survives a real factory-generation change unaided (TRANSFERABLE, a property of the capability across contexts). These are three distinct claims needing distinct evidence; the new profile's deny_cases blocks conflating them.

THREE REAL BENCHMARKS, honest, no maturity inflated:

(A) Skill Dock: EXECUTED/REUSED confirmed with real evidence (idempotent second run; reused in a LATER, genuinely different session for a different question -- cross-generation portability, not TCC/TCX parity -- with zero code changes, real T2). TRANSFERRED: real event happened (docking factory-brain into a real SFV4 snapshot commit 98364ad, validated by V4's own quick_validate.py -> PASS, per IRONMAN 2026-08-29) BUT with an honest, important nuance discovered by actually grepping the test file: that genericity is NOT codified as a permanent regression test (zero mentions of "SFV2"/"throwaway"/"generic" in sync-skills.test.mjs, confirmed by direct grep) -- it happened once, was verified once, is not automatically re-verified if genericity regresses tomorrow. This is exactly the "doesn't count just because it worked once" principle biting even a genuinely good, real capability. GENERALIZED=UNPROVEN, not inflated.

(B) Graphify: the benchmark's strongest finding -- a REAL, organic (not manufactured) T5 negative case. During the ACTUAL storm investigation earlier today (not the later Graphify fitness-test, which Jorge explicitly ordered: "Probarlo... No asumir"), this session never reached for Graphify at all -- chose sweep.log + direct DB queries + Telegram message IDs on its own, without being told to avoid Graphify or use those specific sources. Documented in docs/DFL_LOOP_TAXONOMY_2026-09-01.md §7 as "Graphify contributed nothing to that specific diagnosis." This is concrete evidence of knowing when NOT to reach for a tool -- exactly the mission's point that a correct deny/negative case counts as positive acquisition evidence, not a failure. REUSED/TRANSFERRED/GENERALIZED all UNPROVEN for Graphify.

(C) Loop AQA (loop-behavior.mjs, built earlier the same day): REUSED confirmed (same unmodified classifier already used in 2 real fixtures today: the storm sentinel and the 6-type A-F test file). TRANSFERRED executed LIVE within this very mission, not just referenced: tools/aqa-kit/lib/loop-behavior-transfer-benchmark.test.mjs imports and runs the classifier, with ZERO modification, against the REAL /opt/jpi/scripts/jpi-autonomy/reservation-watchdog.mjs -- a genuinely different repo, different product, different DB engine (better-sqlite3 vs node:sqlite), read cold once, no case-specific coaching. Real result: CONVERGED, exactly 1 notification across 100 real cycles -- 1/1 PASS, and as a side benefit confirms JPI's own wasAlready guard is real and correct. GENERALIZED still UNPROVEN (built and used within the same session that designed it -- cannot yet have spontaneous-future-selection evidence by definition of timing).

INTELLIGENCE MEMORY format proposed (not a new mechanism -- Engram already is the mechanism, just needed a consistent shape): SITUATION -> CAPABILITY CHOSEN (or explicitly NOT chosen) -> WHY -> RESULT -> EVIDENCE -> LIMIT DISCOVERED. Applied retroactively as an example to today's own obs #654 (situation=reconstruct the storm; capability chosen=sweep.log+DB+Telegram, NOT Graphify; why=Graphify has no temporal plane; result=exact root cause reconstructed; evidence=sweep.log/website_events; limit discovered=Graphify models zero time/state in any configuration tested).

REAL MOTIVATING CASE Jorge gave live, mid-mission: "build it once, use it N times, N tending to infinity" -- concretely, if Realtor already knows two-way WhatsApp (real, in production, Meta Cloud API signed-webhook validation + inbound normalization + idempotent outbound send, deployment dpl_GAjXbwt8R1sGdE3ruCeSsNsg5xd7), that capability should be available to any new or existing app, not welded into one product. VERIFIED, not assumed: `node tools/asset-index/query.mjs search "whatsapp meta cloud api two-way messaging"` -> 0 results; `search "realtor mvp meta whatsapp webhook"` -> 1 result, but it's only a mention of whatsapp-realtor-mvp as an AQA-kit dogfood PRODUCT, not a registered reusable messaging CAPABILITY. Real, confirmed gap: this capability sits at EXECUTED/ACQUIRED-within-one-product today, neither REUSABLE nor TRANSFERABLE by this session's own ladder. NOT extracted this session (explicitly out of scope -- this mission was the AQA framework itself, not a product extraction). Registered as an explicit candidate for a future mission: treat "WhatsApp two-way messaging" the way Skill Dock treated "skill-catalog sync" -- extract to its own module/asset with a dfl.yaml, then pursue real T2 (a second real product uses it) and T3 (survives context change) evidence, not the argument that "it already works in Realtor."

No new registry built; tools/asset-index/manifest.mjs untouched (existing schema sufficient, schema-creep guard respected). Full tools/aqa-kit suite: 112/113 PASS (same one pre-existing unrelated flake documented earlier today in docs/DFL_STATE_GRAPH_LOOP_AQA_2026-09-01.md, unaffected).

Files: docs/DFL_CAPABILITY_ACQUISITION_AQA_2026-09-01.md (full report), tools/aqa-kit/lib/profiles.mjs (CAPABILITY_ACQUISITION profile added, 10th total), tools/aqa-kit/lib/loop-behavior-transfer-benchmark.test.mjs (new, real cross-repo T3 test, 1/1 PASS), IRONMAN.md row added.

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

*Mirror auto-generated 2026-09-01T15:40:48Z | La Garra → DFLghub/amos-context*
