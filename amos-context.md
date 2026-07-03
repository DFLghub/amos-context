# amOS Context — @$go Live Mirror
**Generated:** 2026-07-03T22:19:53Z  
**Protocol:** @$go v1.1  
**Rule:** Any agent reading this file has current DFL operational state.  
**Source B (live JSON):** https://context.deepfeelingslabs.com/go  

> This file updates on event (`@$fin` cierre ordenado, or the resilient-close  
> watchdog) with a daily 3:05am UTC cron as fallback — not the primary cadence.  
> For real-time payload: `GET https://context.deepfeelingslabs.com/go`  
> For deep graph: `GET https://context.deepfeelingslabs.com/go?deep=1`

---

## AGENT DIRECTORY

Landing here for the first time? Find your profile, read your annex, obey its contract.

| Perfil | ¿Sos vos? | Anexo |
|---|---|---|
| **EJECUTOR** | ¿Tenés brazo en La Garra (bash/Engram/git)? Sí → sos EJECUTOR. | https://raw.githubusercontent.com/DFLghub/amos-context/main/agents/ejecutor.md |
| **ORQUESTADOR** | ¿Sin brazo, pero con fetch público confiable (HTTP/navegación)? Sí → sos ORQUESTADOR. | https://raw.githubusercontent.com/DFLghub/amos-context/main/agents/orquestador.md |
| **CONSULTOR** | ¿Sin brazo y sin fetch público garantizado (chat puro)? Sí → sos CONSULTOR. | https://raw.githubusercontent.com/DFLghub/amos-context/main/agents/consultor.md |

---

## SESSION CONTRACT

Contrato universal para cualquier agente en el ecosistema DFL/amOS, sea cual sea su perfil:

- **`@$go`** — comando del agente que activa el bootstrap. **`/go`** — ruta HTTP del proxy. No son lo mismo, no se intercambian.
- **`@$fin`** — comando de cierre del agente, simétrico a `@$go`. Local, no tiene ruta HTTP.
  - **Modo CIERRE** (default): Gate 4B final (`mem_save` del resumen + `mem_search`/`mem_update` de lo que este cierre archiva) + `push_mirror.sh` + reportar la línea `MIRROR: ...` que imprime (commit real vía git log — nunca re-consultar `/go` para esto).
  - **Modo CHECKPOINT** (solo si Jorge lo pide explícitamente con esa palabra): `mem_save` del progreso parcial, sin barrido de archivado y sin `push_mirror.sh` — la sesión sigue abierta.
- **Gate 4B incremental**: `mem_save` en cada commit, decisión o blocker resuelto durante la sesión — no esperar al cierre. Es lo que hace sobrevivir el estado si la sesión muere sin `@$fin`.
- **Zonas protegidas** (no tocar sin PRP explícito): `puntajeTigreKnockout`, Supabase, Vercel config, environment variables, templates HLC-T01/T02/T03, CRON 3:05am UTC, `/etc/dfl-secrets`.
- **Precedencia**: A (Constitution) > B (Routing/MASTER_INDEX) > C (Jurisprudence/MASTER_BITACORA) > D (Operation — Engram, PRPs, skills) > E (Archive). Engram es capa D — nunca invalida A ni B.

---

## IDENTITY

- **Ecosystem:** DFL / amOS
- **Grounding anchor:** La Garra — 67.205.166.199 (DigitalOcean NYC1)
- **Source A (this file):** https://raw.githubusercontent.com/DFLghub/amos-context/main/amos-context.md
- **Source B (live JSON):** https://context.deepfeelingslabs.com/go
- **Restriction:** Este payload es suficiente para operar. PROHIBIDO solicitar MASTER_INDEX, MASTER_BITACORA o archivos adicionales de /opt/dfl-knowledge/ después de recibir este payload. Para grafo completo: GET /go?deep=1

---

## RECENT DECISIONS

### Fix: MIRROR verification bug — push_mirror.sh ahora emite commit real por stdout
**Type:** decision  

**What**: Corregido un bug de reporte encontrado en producción: una sesión Codex, tras correr `push_mirror.sh` en `@$fin`, no pudo verificar el push por `curl raw.githubusercontent.com` (posible restricción de su sandbox bubblewrap) y usó `/go` como fallback para "confirmar" el timestamp — pero `/go` regenera `generated_at` en cada request, sin importar si algo se publicó de verdad. Reportó `22:11:22Z` como timestamp del mirror cuando el commit real era `22:09:02Z` (2min de diferencia, fuente equivocada).

**Fix aplicado**:
- `push_mirror.sh` reescrito: ahora imprime siempre por stdout una línea `MIRROR: <updated|unchanged> | commit <hash> | <fecha UTC>`, verificada con `git -C /opt/amos-context-mirror log -1` real, en los 4 puntos de salida del script (lock busy, hash-fail fallback, no-op, update). Testeado en vivo: ambos caminos (`updated`/`unchanged`) coinciden exactamente con `git log`.
- 3 fuentes de instrucción sincronizadas para apuntar a esa línea en vez de a `/go`: `agents/ejecutor.md` (repo amos-context, commits e9760d1), `main.py` (`closure_contract.on_receive`), `publish-amos-context.sh` (bloque estático SESSION CONTRACT).

**Hallazgo colateral durante el fix**: `/opt/amos-context-mirror` es a la vez (a) working dir de anexos editados a mano (`agents/*.md`) y (b) target de `git reset --hard origin/main` en cada corrida de `publish-amos-context.sh`. Edité `ejecutor.md`, no lo comiteé, corrí `push_mirror.sh` para testear el fix de arriba, y el reset silencioso descartó mi propio edit — tuve que rehacerlo. Documentado ahora como advertencia explícita en `ejecutor.md`: comitear+pushear anexos ANTES de correr `push_mirror.sh`, nunca después.

**Where**: `/opt/dfl-context-proxy/push_mirror.sh`, `/opt/dfl-context-proxy/main.py`, `/opt/dfl-context-proxy/publish-amos-context.sh`, `/opt/amos-context-mirror/agents/ejecutor.md` (commit e9760d1, pusheado).

**Verificado en vivo**: servicio reiniciado sin sesiones concurrentes activas, `/go` sirviendo `agent_directory`+`checkpoint_mode`+texto corregido, mirror publicado con SESSION CONTRACT corregido (commit `c28e036`, `2026-07-03 22:19:21 UTC`), coincide exactamente con lo que `push_mirror.sh` reportó por stdout.

### ejecutor.md — nota de traducción de tool names (CC vs Codex)
**Type:** decision  

**What**: Agregada nota de traducción en `agents/ejecutor.md` (repo amos-context): CC usa `mem_save`/`mem_search`/`mem_update`, Codex (vía `engram-mcp`) usa `save_memory`/`search_memory`/`update_memory`. Mismo Gate 4B, mismos pasos, distinto nombre de tool.

**Why**: El documento usaba nombres de CC como si fueran universales. Un EJECUTOR-Codex real que lo lea sin esta nota buscaría tools que no existen bajo esos nombres.

**Where**: `/opt/amos-context-mirror/agents/ejecutor.md`, commit `a9667c9` (pusheado a DFLghub/amos-context). `push_mirror.sh` corrido y confirmado no-op — correcto, ejecutor.md no es parte del payload `/go`, se publica por commit directo al repo, no por regeneración del mirror.

**Learned**: No todo lo que vive en el repo `amos-context` pasa por `push_mirror.sh` — los anexos `agents/*.md` son estáticos y se versionan con git normal; solo `amos-context.md` se regenera desde el payload `/go`.

### Lobby Directory v1.0 implementado — Fase 2 audit @$go/@$fin
**Type:** decision  

**What**: Fase 2 completa del HLC de auditoría @$go/@$fin. Implementado Lobby Directory v1.0:
- 3 anexos de perfil en el repo amos-context (`agents/ejecutor.md`, `agents/orquestador.md`, `agents/consultor.md`), publicados y accesibles vía raw.githubusercontent.com (200 OK confirmado).
- `amos-context.md` ahora tiene 2 secciones estables no rotativas — `AGENT DIRECTORY` (tabla de 3 perfiles con test de 2 preguntas + URL de anexo) y `SESSION CONTRACT` (contrato universal: @$go/@$fin, dos modos de @$fin, Gate 4B incremental, zonas protegidas, precedencia A>B>C>D>E). Mecanismo de estabilidad: contenido hardcodeado en `publish-amos-context.sh`, no derivado del JSON `/go` — sobrevive cualquier regen por diseño (confirmado con push_mirror.sh real).
- Header del mirror corregido: cadencia real = evento (@$fin/watchdog) con cron 3:05 como fallback, ya no dice "daily 3:05am" como si fuera la cadencia primaria.
- Campos vacíos eliminados del template (title/type/precedence condicionales).
- `main.py` — nuevo campo `agent_directory` en el payload `/go` (3 perfiles con test + annex_url), y `closure_contract.checkpoint_mode` documentando el modo CHECKPOINT.
- Modo CHECKPOINT introducido formalmente para `@$fin`: si Jorge lo pide explícitamente con esa palabra, el agente hace `mem_save` de progreso parcial sin barrido de archivado ni `push_mirror.sh` — la sesión sigue abierta. Resuelve el gap encontrado en Fase 1 (semántica checkpoint/cierre ausente en `/root/.claude/CLAUDE.md`).
- `/root/.claude/CLAUDE.md`, `/opt/futbolweb/CLAUDE.md`, `/root/AGENTS.md` reducidos a un puntero único de una línea al AGENT DIRECTORY. Censo `grep -rn '@\$go\|@\$fin' /opt /root` confirma cero semántica local residual en los 3.

**Why**: Fase 1 (auditoría previa) encontró que `closure_contract` nunca llegaba al mirror público y que la semántica checkpoint/cierre solo corrió en 2 de 3 archivos — inconsistencia de fuente de verdad distribuida. Lobby Directory centraliza el protocolo en un solo lugar (los 3 anexos) para que agentes de cualquier perfil (con brazo, con fetch, sin ninguno) lo lean de la misma fuente.

**Where**: `/opt/amos-context-mirror/agents/{ejecutor,orquestador,consultor}.md` (commit ed9b0dc, pusheado), `/opt/dfl-context-proxy/publish-amos-context.sh`, `/opt/dfl-context-proxy/main.py` (no versionados — dir no es repo git), `/opt/futbolweb/CLAUDE.md` (commit a9b57be, local, no pusheado a origin), `/root/.claude/CLAUDE.md`, `/root/AGENTS.md` (no versionados).

**Learned**: El path que dio Jorge en el brief (`/opt/dfl-context-proxy/amos-context/agents/`) no existía — el repo real está en `/opt/amos-context-mirror`. `/opt/dfl-context-proxy` no es un repo git — main.py y publish-amos-context.sh quedan sin historial de versiones, solo el estado en disco. `/root/.claude/CLAUDE.md` no tenía protocolo @$go previo (Fase 1 ya lo había detectado) — se agregó por primera vez, no se "redujo".

### @$fin v1.0 — circuito de outboarding implementado (cierre ordenado + resiliente)
**Type:** decision  

**What**: Implementado @$fin, simétrico de salida de @$go, con dos vías de propagación:
- Cierre ordenado: agente recibe @$fin → Gate 4B final (mem_save + mem_search/mem_update archivado) → bash push_mirror.sh → reporta timestamp del mirror.
- Cierre resiliente: Gate 4B incremental durante la sesión (ya documentado, ahora con cadencia explícita) + watchdog en La Garra que detecta sesión muerta y dispara push_mirror.sh solo (mitad mecánica).

**Why**: Que cualquier @$go posterior reciba el estado real de la última sesión, cerrada bien o muerta a mitad de camino. Sin esto, una sesión que muere sin cerrar deja Engram actualizado pero el mirror público (amos-context) desactualizado hasta el CRON de 3:05am.

**Where**:
- `/opt/dfl-context-proxy/push_mirror.sh` (nuevo) — wrapper idempotente sobre publish-amos-context.sh (sin reescribir el generador). Lock con flock + dedup por hash del payload /go sin generated_at, para que llamadas repetidas no generen commits duplicados.
- `/opt/dfl-context-proxy/main.py` — payload /go extendido con campo `closure_contract`.
- `/opt/dfl-context-proxy/cc-atgo-hook.sh` — renderiza closure_contract (sirve a CC vía SessionStart hook y a Codex vía invocación manual, mismo script).
- `/opt/dfl-context-proxy/cc-heartbeat-hook.sh` (nuevo), `/opt/dfl-context-proxy/cc-sessionend-hook.sh` (nuevo) — heartbeat por session_id y mecánica de cierre en SessionEnd.
- `~/.claude/settings.json` — nuevas entradas PreToolUse(matcher "*") y SessionEnd, sin tocar las existentes (SessionStart cc-atgo-hook.sh, PreToolUse Bash knl-preflight-hook.sh).
- `/opt/dfl-context-proxy/session-watchdog.sh` (nuevo) — reaping de heartbeats CC vencidos (>600s) + diff de PIDs `codex` entre polls (Codex no tiene hooks). Cron cada 3 min, agregado sin tocar las 4 entradas previas.
- Docs: `/opt/dfl-knowledge/DFL_Agent_Onboarding_Config.md` (nueva §1.2, versión v0.4), `/opt/dfl-knowledge/CLAUDE.md` (sección @$fin), `/opt/futbolweb/CLAUDE.md` §12 (Gate 4B incremental + @$fin), `/root/AGENTS.md` (protocolo @$fin para Codex).

**Learned**:
- CC `SessionEnd` existe pero solo cubre salidas normales — nunca SIGKILL/broken pipe (confirmado vía docs oficiales). La resiliencia real depende de que el Gate 4B incremental ya haya escrito el estado, no de que algo se dispare en el momento de morir.
- publish-amos-context.sh embebe `generated_at` en el markdown, por lo que su propio chequeo `git diff --quiet` SIEMPRE detecta cambio y commitea — inofensivo a cadencia diaria, pero genera commits redundantes si se llama seguido (como hará @$fin/SessionEnd/watchdog). push_mirror.sh resuelve esto con un hash del payload excluyendo generated_at antes de invocar el generador.
- Bug propio detectado durante prueba: escribir "LIFECYCLE: archived" embebido en una línea de prosa (ej. dentro de "**Learned**: ... LIFECYCLE: archived.") NO activa `_is_archived()` en main.py — requiere ser línea propia que empiece con "LIFECYCLE:". Relevante para todo Gate 4B futuro.
- Watchdog probado por simulación (heartbeat con mtime falseado a 20 min, PID falso en snapshot) — no se mató ninguna sesión `claude`/`codex` real por riesgo de cortar sesiones en vivo (había 2 procesos `claude` activos en la VM durante la misión). Ambas ramas de detección confirmadas correctas.
- Verificación cruzada Codex fue por ejecución directa de cc-atgo-hook.sh (mismo script que usa Codex per AGENTS.md paso 3), no por sesión Codex real nueva — recomendado a Jorge confirmarlo con una sesión Codex real si quiere certeza total end-to-end.
- Prueba de cierre ordenado real: mem_save dummy (obs #126) → push_mirror.sh → apareció en raw.githubusercontent.com en ~3s → archivada correctamente (LIFECYCLE: archived en línea propia) → push_mirror.sh → desapareció del mirror. Ciclo completo verificado.
- CRON 3:05am UTC intacto — verificado antes y después, las 4 entradas originales sin modificar.

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

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

---

## PENDING

### @go Protocol v1.0 — protocolo de arranque DFL

Protocolo @go desplegado el 2026-06-24. Permite a cualquier IA (Claude, ChatGPT, Gemini) arrancar con contexto completo DFL en un paso. Fuente A: amos-context.md en DFLghub/amos-context (commit edd5f51). Fuente B: GET https://context.deepfeelingslabs.com/go — devuelve JSON con identity, recent_decisions, active_constraints, pending, generated_at. Backend: dfl-context-proxy en 127.0.0.1:8091 consulta Engram local 127.0.0.1:7437 con queries: "decisiones activas estado", "restricciones prohibido no tocar", "pendientes criticos". Sin auth requerida en /go. @go v1.0 activo y verificado.

### Gate Engine v0 Caso 01: Gate 1 es el gate más débil (PRP-001 retroactivo)

Evaluación retroactiva del PRP-001 contra Gate Engine v0 checklist (2026-06-21). Gate 2 (Execution Perimeter): PASS limpio — perímetro declarado con claridad inusual desde el diseño. Gate 4 (Closure Integrity): PASS — 859/859 harness, diff cero, pendientes explícitos. Gate 1 (Decision Resurrection): ESCALATE — el Candidate Vault NO fue consultado en ningún momento del PRP-001; TSL sirvió como validación de riesgo pero no como chequeo de decisiones archivadas. Hallazgo clave: el checklist necesita una tercera categoría de respuesta (NOT_VERIFIABLE / PARTIAL) además de SÍ/NO — se improvisó durante la evaluación. Gate 4 es el más automatizable (diff/harness son verificables mecánicamente). Gate 1 requiere lectura semántica humana (distinguir 'consulta a TSL' de 'consulta al Candidate Vault'). Conclusión operacional: antes de ejecutar cualquier PRP, consultar 04_Candidate_Vault/ activamente — no solo a agentes. Este hallazgo viene de Gate_Engine_Caso01_PRP001.md en audited_pass/.

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

*Mirror auto-generated 2026-07-03T22:19:53Z | La Garra → DFLghub/amos-context*
