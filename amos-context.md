# amOS Context — @$go Live Mirror
**Generated:** 2026-07-02T21:27:47Z  
**Protocol:** @$go v1.1  
**Rule:** Any agent reading this file has current DFL operational state.  
**Source B (live JSON):** https://context.deepfeelingslabs.com/go  

> This file is auto-updated by La Garra (daily 3:05am UTC).  
> For real-time payload: `GET https://context.deepfeelingslabs.com/go`  
> For deep graph: `GET https://context.deepfeelingslabs.com/go?deep=1`

---

## IDENTITY

- **Ecosystem:** DFL / amOS
- **Grounding anchor:** La Garra — 67.205.166.199 (DigitalOcean NYC1)
- **Source A (this file):** https://raw.githubusercontent.com/DFLghub/amos-context/main/amos-context.md
- **Source B (live JSON):** https://context.deepfeelingslabs.com/go
- **Restriction:** Este payload es suficiente para operar. PROHIBIDO solicitar MASTER_INDEX, MASTER_BITACORA o archivos adicionales de /opt/dfl-knowledge/ después de recibir este payload. Para grafo completo: GET /go?deep=1

---

## RECENT DECISIONS

### FutbolWeb — fix amnesia pronósticos KO (ce766fd)
**Type:** decision  

**What**: Commit ce766fd resuelve amnesia completa del sistema de pronósticos.

**Cambios clave:**
- PredictDemoForm detecta pronóstico existente en mount (fetch proactivo por matchSlug, sin necesitar ?edit=ID). Pre-llena form + muestra banner amber.
- Partido cerrado: panel read-only con marcador + equipo clasificado. Sin formulario.
- Panel de confirmación post-submit muestra marcador real y advancing_team.
- PATCH /api/predictions/[id] ahora acepta y persiste advancing_team (bug silencioso corregido).
- GET /api/my-predictions devuelve advancing_team.
- 409 POST retorna prediction existente en body (code: "duplicate").
- MyPredictionsClient muestra advancing_team en tarjeta.
- GroupMatchPredictionsTable muestra advancing_team para partidos KO.
- 10 archivos, 202 inserciones.

**Why**: Usuarios no recibían feedback adecuado y el equipo clasificado (KO empate) nunca se mostraba tras enviarlo.

**Where**: components/PredictDemoForm.tsx, app/api/my-predictions/route.ts, app/api/predictions/route.ts, app/api/predictions/[id]/route.ts, components/MyPredictionsClient.tsx, components/GroupMatchPredictionsTable.tsx, lib/group-match-predictions.ts, app/match/[slug]/predict/page.tsx, dictionaries/es.ts, dictionaries/en.ts.

### ESPN sync cron expiry 2026-07-20 — diseño intencional, no alerta
**Type:** decision  

**What**: El cron de ESPN sync en cron-job.org expira el 2026-07-20, un día después del fin del Mundial (2026-07-19).

**Why**: Diseño deliberado de Jorge — el día extra captura cualquier dato rezagado del torneo. No es un olvido ni un gap operativo.

**Where**: cron-job.org — job ESPN sync FutbolWeb.

**Learned**: NO marcar como alerta. NO sugerir renovación. NO tratar como deuda técnica. La expiración es el cierre natural del ciclo del producto. Agentes futuros deben suprimir cualquier advertencia sobre este cron.

### Preferencia operativa: usar grafos cuando ROI > exploración manual
**Type:** decision  

**What**: Jorge instruyó que siempre que el ROI sea mayor, usar grafos (KNL/dfl-nav/graph.json) para tareas, actividades y procesos en lugar de exploración manual (find/grep/ls).

**Why**: Alineado con KNL policy v1.0 activa — "Graph preflight MANDATORY before any find/grep/ls". El usuario lo refuerza como preferencia operativa explícita: cuando navegar el grafo da más contexto en menos tokens/tiempo que exploración ciega, el grafo gana.

**Where**: Aplica a toda sesión en FutbolWeb y DFL. Antes de find/grep/rg/ls, consultar knl.navigation.god_nodes o dfl-nav --brief. Solo usar exploración manual cuando el grafo no tiene cobertura del concepto buscado.

**Learned**: La regla es ROI-driven, no absoluta — si el grafo no cubre el concepto (community miss), exploración manual es válida como fallback.

### [DECISION:DFL] HLC templates P6 v1.0 creados — T01/T02/T03 activos
**Type:** decision  

TOPIC: dfl/governance/hlc-templates
TYPE: decision
STATUS: active
DATE: 2026-06-28
SUMMARY: Creados templates HLC P6 v1.0 en /opt/dfl-knowledge/governance/hlc-templates/: HLC-T01 Regen Grafo Semanal, HLC-T02 Limpieza Engram, HLC-T03 Auditoría FutbolWeb Pre-Deploy. Los tres quedan ACTIVO con esquema canónico MISIÓN / CONTEXTO / PERMISOS / METABOLISMO / LOOP / CRITERIOS. Fuente de diseño: DFL_P6_P7_DESIGN_2026-06-27.md.

### KNL v1.0 contrato operativo validado
**Type:** decision  

TOPIC: dfl/knl/v1
TYPE: decision
STATUS: active
DATE: 2026-06-28
SUMMARY: KNL v1.0 queda operativo como contrato oficial en /go. knl.json valida schema dfl.knl.v1 con semantic communities/entropy, navigation neighbors, memory, policy, provenance, comparator y validation. graph_context no aparece en /go. knl_compare.py ahora soporta snapshots previos con links y genera comparator status changed con previous_available=true. dfl-nav --brief muestra neighbors. P0/P4 quedan pendientes de confirmacion: regen_graph.sh aun usa OPENAI_API_KEY y graphify como productor; contrato KNL requiere ag_topologo.py como productor canonico de graph.json y Graphify solo como consumidor/analisador. P3 gap: ag_topologo local declara v0.1; no se encontro v0.3 instalable.
EVIDENCE: python3 /opt/dfl-context-proxy/tests/test_knl_contract.py => knl contract ok. Public /go has knl=true, graph_context=false, validation ok.

### [VERIFIED] /go payload slim — graph_context alias eliminado
**Type:** decision  

Eliminado el alias `graph_context` del payload GET /go en /opt/dfl-context-proxy/main.py.

Cambio: `payload["graph_context"] = knl` removido. Solo queda `payload["knl"]`.

Verificado: curl http://127.0.0.1:8091/go keys = ['identity', 'recent_decisions', 'active_constraints', 'pending', 'cc_bootstrap', 'generated_at', 'recent_engram_dfl', 'knl']. graph_context: False, knl: True.

Servicio reiniciado y saludable. Consumidores activos (cc-atgo-hook.sh, Codex) nunca leyeron graph_context del payload — solo leen identity/decisions/constraints/pending/cc_bootstrap.

KNL policy y CLAUDE.md en /opt/dfl-knowledge/ mencionan graph_context como legacy — no se tocaron (documentación, no runtime).

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

### FutbolWeb — Motor Scoring Knockout (puntajeTigreKnockout) Protegido
**Type:** fact  

OBS_ID: DFL-OBS-20260624-007
TIPO: fact
PROYECTO: futbolweb
PLATFORM: vercel
SUBSISTEMA: futbolweb/knockout
PRECEDENCIA: D
AUTHORITY: operational
LIFECYCLE: active
CONFIDENCE: high
LAST_VERIFIED: 2026-06-24
SOURCE: session
SOURCE_REF_TYPE: session_id
SOURCE_REF: MPGE_2026-06-23
SUPERSEDE: ninguno
PROMOTION_CANDIDATE: no
PROMOTION_TARGET: none
TOPIC_KEY: futbolweb/knockout/scoring-rules

QUÉ: Motor de scoring knockout en FutbolWeb: función puntajeTigreKnockout en packages/scoring. Engine protegido y estable. Tests exhaustivos agregados 2026-06-23 (synthetic harness). Lógica: evaluación de resultados en fase eliminatoria con cálculo de puntaje Tigre incluyendo penaltis y goles de visita.

POR QUÉ IMPORTA: Es la lógica core del Oráculo Futbolero para torneos knockout. Cualquier cambio afecta predicciones en producción.

DÓNDE APLICA: packages/scoring, Vercel runtime, toda UI que muestre predicciones de fase eliminatoria.

PRÓXIMO AGENTE DEBE: NO modificar puntajeTigreKnockout sin PRP explícito y autorización. Tests de synthetic harness deben seguir pasando antes de cualquier deploy.

---

## PENDING

### FutbolWeb — estado y stack (caso 001 Apps Factory)

FutbolWeb / Oráculo Futbolero: producto operativo en producción durante Mundial 2026. Es simultáneamente un producto real y el caso de estudio 001 de DFL Apps Factory. Stack: Next.js + Supabase + Vercel (hobby). Repo: github.com/DFLghub/futbolweb-app. Código en /opt/futbolweb en La Garra. Sistema de scoring: puntaje_tigre (grupos) + puntaje_tigre_knockout (eliminatorias, commit bfca1ac). Sync ESPN via cron-job.org (expira 2026-07-20). Backup: GitHub Actions diario. Pendientes críticos: (1) wiring DB layer KnockoutEngine contra Neon; (2) sensibilidad a mayúsculas en realAdvancingTeam con datos reales; (3) confirmar deploy commit 50316e3; (4) diagnosticar webhook GitHub-Vercel roto. Regla de oro: no tocar Supabase/scoring/deployment sin autorización explícita.

### Gate Engine v0 Caso 01: Gate 1 es el gate más débil (PRP-001 retroactivo)

Evaluación retroactiva del PRP-001 contra Gate Engine v0 checklist (2026-06-21). Gate 2 (Execution Perimeter): PASS limpio — perímetro declarado con claridad inusual desde el diseño. Gate 4 (Closure Integrity): PASS — 859/859 harness, diff cero, pendientes explícitos. Gate 1 (Decision Resurrection): ESCALATE — el Candidate Vault NO fue consultado en ningún momento del PRP-001; TSL sirvió como validación de riesgo pero no como chequeo de decisiones archivadas. Hallazgo clave: el checklist necesita una tercera categoría de respuesta (NOT_VERIFIABLE / PARTIAL) además de SÍ/NO — se improvisó durante la evaluación. Gate 4 es el más automatizable (diff/harness son verificables mecánicamente). Gate 1 requiere lectura semántica humana (distinguir 'consulta a TSL' de 'consulta al Candidate Vault'). Conclusión operacional: antes de ejecutar cualquier PRP, consultar 04_Candidate_Vault/ activamente — no solo a agentes. Este hallazgo viene de Gate_Engine_Caso01_PRP001.md en audited_pass/.

---

## CC BOOTSTRAP (Claude Code session startup)

- **Step 1:** mem_search('contexto DFL') — Engram MCP activo, consultar antes de operar
- **Step 2:** PROXIMO_AGENTE_DEBE en cada recent_decision = instrucción de acción inmediata
- **Step 3:** Al guardar obs: usar DFL Writing Convention v0.3 (topic: dfl/engram/writing-convention)
- **Precedencia:** A > B > C > D > E — Engram es capa D, nunca invalida Blueprint (A) ni MASTER_INDEX (B)
- **Protegido:** NO modificar puntajeTigreKnockout, Supabase, Vercel config sin PRP explícito
- **Alerta:** cron-job.org ESPN sync expira 2026-07-20 — cierre natural, torneo termina 2026-07-19, no requiere renovación

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

### estado
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

**Graph entropy:** 3.101  

- **Community 11** (657 nodes): Codex, CLI, DFL Engram Writing Convention v0.3
- **Community 0** (465 nodes): Gobernanza, CANDIDATE VAULT, Jorge
- **Community 1** (452 nodes): patrón, Declarar, Crear
- **Community 2** (442 nodes): amOS, DFL, SaaS Factory
- **Community 3** (267 nodes): CO-001, OpenAI, MCP
- **Community 4** (231 nodes): ROJA, API, Ejemplo

---

*Mirror auto-generated 2026-07-02T21:27:47Z | La Garra → DFLghub/amos-context*
