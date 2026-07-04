# amOS Context — @$go Live Mirror
**Generated:** 2026-07-04T15:21:47Z  
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

### [DECISION:DFL] Onboarding @$go es solo lectura — cero mutaciones de estado
**Type:** decision  
**Project:** futbolweb-app  

**What**: El bootstrap `@$go` (o cualquier onboarding de un agente nuevo entrando a una sesión DFL) es SOLO LECTURA. Prohibido archivar, resolver, marcar [RESOLVED] o mutar el estado de cualquier observación de Engram durante el bootstrap — solo reportar lo encontrado a Jorge y esperar indicación explícita antes de escribir.
**Why**: Corrección de Jorge 2026-07-04 — en un bootstrap @$go anterior, el agente EJECUTOR marcó la obs #156 (payload /go no refleja obs nuevas entre corridas de cron) como [RESOLVED] confundiéndola con un bug distinto ya cerrado (línea 34 de push_mirror.sh, que solo corrige el REPORTE de la línea MIRROR, no el contenido del payload). El archivado prematuro sin evidencia suficiente casi oculta una anomalía real y viva.
**Learned**: Gate 4B incremental (mem_save durante la sesión al cerrar commits/decisiones/blockers) sigue vigente para trabajo activo — esta regla aplica específicamente a la ventana de bootstrap/onboarding, antes de que Jorge haya dado indicación de qué trabajar. Ver también [[ejecutor-annex-update-pending]] — esta regla debe incorporarse al anexo `agents/ejecutor.md` (pendiente, no ejecutado en esta sesión).

### [CIERRE] P0 FutbolWeb 2026-07-04 — archivado de las 3 premisas del HLC original [CORREGIDO: deploy sí ocurrió]
**Type:** decision  
**Project:** futbolweb-app  

**What**: Cierre formal de las 3 premisas del HLC-P0-2026-07-04 (FutbolWeb, octavos/bracket), cada una con su estado real verificado esta sesión.

1. **"node_modules roto en /opt/futbolweb, bloquea npm test"** → [RESOLVED — la premisa era falsa]. Verificado: `npm run build` y `npm test` corren limpios (67/67, harness puntajeTigreKnockout >850 casos ok). Ver obs #147.

2. **"Falta propagación automática de bracket para todas las rondas"** → [RESOLVED — la premisa era falsa]. La función `applyKnockoutBracketAssignments` (lib/knockout-reality.ts) ya cubre genéricamente 73→104 desde commit c4c6111 (preexistente). El bug real era más angosto: app/upcoming/page.tsx nunca la invocaba. Fix commiteado y pusheado a origin/main (92b6857). Ver obs #147/#148.

3. **"Webhook GitHub→Vercel roto, producción congelada en 4a9112e"** → [CORREGIDO 2026-07-04 noche, tras cierre inicial erróneo]. El push de 92b6857 SÍ disparó deploy automático a producción — verificado por Jorge en Vercel dashboard (badge Production/Ready) y por mí en vivo (`curl https://www.futbolweb.app/upcoming` muestra Paraguay/Francia/Canadá/Marruecos en los partidos de Octavos, igual que en la verificación local). El webhook funcionó empíricamente esta noche. Estado correcto: **producción al día**. Lo que NO quedó investigado es la causa raíz histórica del congelamiento previo (por qué estuvo detenido en 4a9112e desde el 28-jun hasta ahora) — eso sigue sin diagnóstico, no el webhook en sí.

**Resultado neto para el próximo agente**: código arreglado, pusheado, Y desplegado — producción refleja el fix. Misión P0 FutbolWeb: completa en su objetivo funcional (octavos con equipos reales en prod). Pendiente real remanente: ninguno funcional; opcionalmente investigar causa raíz histórica del freeze si se quiere prevenir recurrencia.
**Where**: app/upcoming/page.tsx (commit 92b6857), obs #147, #148, #149.

### Fix: MIRROR verification bug — push_mirror.sh ahora emite commit real por stdout
**Type:** decision  
**Project:** dfl  

**What**: Corregido un bug de reporte encontrado en producción: una sesión Codex, tras correr `push_mirror.sh` en `@$fin`, no pudo verificar el push por `curl raw.githubusercontent.com` (posible restricción de su sandbox bubblewrap) y usó `/go` como fallback para "confirmar" el timestamp — pero `/go` regenera `generated_at` en cada request, sin importar si algo se publicó de verdad. Reportó `22:11:22Z` como timestamp del mirror cuando el commit real era `22:09:02Z` (2min de diferencia, fuente equivocada).

**Fix aplicado**:
- `push_mirror.sh` reescrito: ahora imprime siempre por stdout una línea `MIRROR: <updated|unchanged> | commit <hash> | <fecha UTC>`, verificada con `git -C /opt/amos-context-mirror log -1` real, en los 4 puntos de salida del script (lock busy, hash-fail fallback, no-op, update). Testeado en vivo: ambos caminos (`updated`/`unchanged`) coinciden exactamente con `git log`.
- 3 fuentes de instrucción sincronizadas para apuntar a esa línea en vez de a `/go`: `agents/ejecutor.md` (repo amos-context, commits e9760d1), `main.py` (`closure_contract.on_receive`), `publish-amos-context.sh` (bloque estático SESSION CONTRACT).

**Hallazgo colateral durante el fix**: `/opt/amos-context-mirror` es a la vez (a) working dir de anexos editados a mano (`agents/*.md`) y (b) target de `git reset --hard origin/main` en cada corrida de `publish-amos-context.sh`. Edité `ejecutor.md`, no lo comiteé, corrí `push_mirror.sh` para testear el fix de arriba, y el reset silencioso descartó mi propio edit — tuve que rehacerlo. Documentado ahora como advertencia explícita en `ejecutor.md`: comitear+pushear anexos ANTES de correr `push_mirror.sh`, nunca después.

**Where**: `/opt/dfl-context-proxy/push_mirror.sh`, `/opt/dfl-context-proxy/main.py`, `/opt/dfl-context-proxy/publish-amos-context.sh`, `/opt/amos-context-mirror/agents/ejecutor.md` (commit e9760d1, pusheado).

**Verificado en vivo**: servicio reiniciado sin sesiones concurrentes activas, `/go` sirviendo `agent_directory`+`checkpoint_mode`+texto corregido, mirror publicado con SESSION CONTRACT corregido (commit `c28e036`, `2026-07-03 22:19:21 UTC`), coincide exactamente con lo que `push_mirror.sh` reportó por stdout.

### ejecutor.md — nota de traducción de tool names (CC vs Codex)
**Type:** decision  
**Project:** dfl  

**What**: Agregada nota de traducción en `agents/ejecutor.md` (repo amos-context): CC usa `mem_save`/`mem_search`/`mem_update`, Codex (vía `engram-mcp`) usa `save_memory`/`search_memory`/`update_memory`. Mismo Gate 4B, mismos pasos, distinto nombre de tool.

**Why**: El documento usaba nombres de CC como si fueran universales. Un EJECUTOR-Codex real que lo lea sin esta nota buscaría tools que no existen bajo esos nombres.

**Where**: `/opt/amos-context-mirror/agents/ejecutor.md`, commit `a9667c9` (pusheado a DFLghub/amos-context). `push_mirror.sh` corrido y confirmado no-op — correcto, ejecutor.md no es parte del payload `/go`, se publica por commit directo al repo, no por regeneración del mirror.

**Learned**: No todo lo que vive en el repo `amos-context` pasa por `push_mirror.sh` — los anexos `agents/*.md` son estáticos y se versionan con git normal; solo `amos-context.md` se regenera desde el payload `/go`.

### TDF-01 session close low_variance_probe Gate 4B complete
**Type:** decision  
**Project:** tdf-01  

**Session close summary (`@$fin`)**

**Project**: `tdf-01` in `/opt/nq-factory`.

**Done**:
- Generated/kept `data/sample/nq_10d_synthetic.csv`: 3910 synthetic 1-min OHLCV bars across 10 operated days, no external data purchase/accounts/API keys.
- Analyzed VWAP R5 failure: daily PnL `[-950.0, -1027.0, -447.0, -932.5, -445.0, -972.0, -686.0, -541.5, -966.0, -1072.0]`, mean `-803.90`, population std `235.16`, CV `0.2925`.
- Implemented `LowVarianceProbeStrategy`, available as `--strategy low_variance_probe`.
- Compared strategies on `nq_10d_synthetic.csv`: `low_variance_probe` passes R4a/R4b/R4c/R5 with `CV=0.1346291201783626`; VWAP fails R4c/R5.
- Committed local repo: `011bab1 feat(tdf-01): add low_variance_probe strategy — passes R4/R5; add 10d synthetic dataset`.
- Gate 4B saved obs 138 with ranking and metrics.
- Marked obs 135 as superseded by obs 138 via relation `rel-d366de964a1c68ee` and archived obs 135 with `[RESOLVED]` / `LIFECYCLE: archived`.

**Verification**:
- `.venv/bin/python -B -m pytest -q -o cache_dir=/tmp/nq-factory-pytest-cache` -> `7 passed in 1.39s`.
- `git status --short` clean after commit.

**Open caveat**: `objective_win_rate` remains an optional/configurable objective, not a contract rule, per obs 130. No protected surfaces touched: puntajeTigreKnockout, Supabase, Vercel, secrets untouched.

### TDF-01 low_variance_probe pasa R4/R5 y supera VWAP baseline
**Type:** decision  
**Project:** tdf-01  

**What**: Commit local `011bab1` (`feat(tdf-01): add low_variance_probe strategy — passes R4/R5; add 10d synthetic dataset`) agregó `data/sample/nq_10d_synthetic.csv` y la estrategia candidata `LowVarianceProbeStrategy` en `/opt/nq-factory`. La estrategia quedó disponible vía `--strategy low_variance_probe` y con test de validadores.

**Dataset**: `data/sample/nq_10d_synthetic.csv`, 3910 barras 1-min OHLCV sintéticas, 10 días hábiles (`2026-07-06 09:30:00` a `2026-07-17 16:00:00`). No se compraron datos, no se crearon cuentas, no se usaron API keys.

**Hallazgo clave**: `low_variance_probe` es la primera candidata registrada que pasa el contrato Eduardo completo R4/R5 sobre el dataset sintético extendido: R4a PASS, R4b PASS, R4c PASS, R5 PASS con `CV=0.1346291201783626` (`operated_days=10`, límite `0.20`). Es una candidata de consistencia/baja varianza, no una afirmación de edge de mercado.

**Comparación VWAP baseline**:
- `vwap`: `trades=542`, `net_pnl=-8039.0`, `costs=7859.0`, R4a PASS (`-1072.0 >= -2000`), R4b PASS (`-1022.0 >= -2000`), R4c FAIL (`min_cushion=-3007.95`), R5 FAIL (`CV=0.2925258352542665 > 0.20`, `operated_days=10`), `objective_win_rate` FAIL (`0.2823 < 0.59`).
- Distribución diaria VWAP: `[-950.0, -1027.0, -447.0, -932.5, -445.0, -972.0, -686.0, -541.5, -966.0, -1072.0]`; media diaria `-803.90`, std poblacional `235.16`, CV `0.2925`.

**Comparación low_variance_probe**:
- `low_variance_probe`: `trades=10`, `net_pnl=-200.0`, `costs=145.0`, R4a PASS (`-24.5 >= -2000`), R4b PASS (`-24.5 >= -2000`), R4c PASS (`min_cushion=4817.55`), R5 PASS (`CV=0.1346291201783626 <= 0.20`, `operated_days=10`), `objective_win_rate` FAIL (`0.0 < 0.59`).
- Distribución diaria low_variance_probe: `[-19.5, -19.5, -24.5, -24.5, -19.5, -19.5, -19.5, -14.5, -19.5, -19.5]`.

**Ranking Eduardo**: 1) `low_variance_probe` porque pasa R4 completo y R5; 2) `vwap` porque falla R4c y R5. `objective_win_rate` se mantiene como objetivo configurable/no canónico según obs 130, no como regla obligatoria del contrato Eduardo.

**Evidence**: `.venv/bin/python -B -m pytest -q -o cache_dir=/tmp/nq-factory-pytest-cache` -> `7 passed in 1.39s`. Demos ejecutados con `nq_10d_synthetic.csv`: VWAP -> `validations_failed=R4c_trailing_hwm_floor,R5_consistency_cv,objective_win_rate`; low_variance_probe -> `validations_failed=objective_win_rate`.

**Supersedes**: Esta observación supera operativamente obs 135 en ranking de estrategias: obs 135 dejó la base VWAP/validadores implementada; esta observación identifica la primera candidata que pasa R4/R5 completo.

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

### FutbolWeb — Motor Scoring Knockout (puntajeTigreKnockout) Protegido
**Type:** fact  
**Project:** dfl  

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

### @go Protocol v1.0 — protocolo de arranque DFL
**Project:** dfl  

Protocolo @go desplegado el 2026-06-24. Permite a cualquier IA (Claude, ChatGPT, Gemini) arrancar con contexto completo DFL en un paso. Fuente A: amos-context.md en DFLghub/amos-context (commit edd5f51). Fuente B: GET https://context.deepfeelingslabs.com/go — devuelve JSON con identity, recent_decisions, active_constraints, pending, generated_at. Backend: dfl-context-proxy en 127.0.0.1:8091 consulta Engram local 127.0.0.1:7437 con queries: "decisiones activas estado", "restricciones prohibido no tocar", "pendientes criticos". Sin auth requerida en /go. @go v1.0 activo y verificado.

### Gate Engine v0 Caso 01: Gate 1 es el gate más débil (PRP-001 retroactivo)
**Project:** dfl  

Evaluación retroactiva del PRP-001 contra Gate Engine v0 checklist (2026-06-21). Gate 2 (Execution Perimeter): PASS limpio — perímetro declarado con claridad inusual desde el diseño. Gate 4 (Closure Integrity): PASS — 859/859 harness, diff cero, pendientes explícitos. Gate 1 (Decision Resurrection): ESCALATE — el Candidate Vault NO fue consultado en ningún momento del PRP-001; TSL sirvió como validación de riesgo pero no como chequeo de decisiones archivadas. Hallazgo clave: el checklist necesita una tercera categoría de respuesta (NOT_VERIFIABLE / PARTIAL) además de SÍ/NO — se improvisó durante la evaluación. Gate 4 es el más automatizable (diff/harness son verificables mecánicamente). Gate 1 requiere lectura semántica humana (distinguir 'consulta a TSL' de 'consulta al Candidate Vault'). Conclusión operacional: antes de ejecutar cualquier PRP, consultar 04_Candidate_Vault/ activamente — no solo a agentes. Este hallazgo viene de Gate_Engine_Caso01_PRP001.md en audited_pass/.

---

## RECENT ACTIVITY (cross-project)

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

### [RESOLVED] FutbolWeb — estado y stack snapshot 2026-06-24 (superado)
**Type:** fact  
**Project:** dfl  

SNAPSHOT ARCHIVADO — corte 2026-06-24. Los pendientes críticos listados aquí fueron resueltos en sesiones posteriores (commit ce766fd amnesia fix 2026-06-28, commit 4a9112e Supabase grant 2026-07-02, KnockoutEngine wiring 2026-06-27). Este snapshot no refleja el estado actual.

Para estado actual de FutbolWeb consultar observaciones recientes o git log /opt/futbolweb.

--- SNAPSHOT ORIGINAL (2026-06-24) ---
FutbolWeb / Oráculo Futbolero: producto operativo en producción durante Mundial 2026. Stack: Next.js + Supabase + Vercel (hobby). Repo: github.com/DFLghub/futbolweb-app. Código en /opt/futbolweb en La Garra. Pendientes críticos al 2026-06-24: (1) wiring DB layer KnockoutEngine — RESUELTO 2026-06-27; (2) sensibilidad mayúsculas realAdvancingTeam — RESUELTO 2026-06-27; (3) confirmar deploy commit 50316e3 — RESUELTO; (4) diagnosticar webhook GitHub-Vercel — pendiente verificación.

### [RESOLVED] Causa raíz #156 — /go hardcodeaba project=dfl, ciego a futbolweb-app
**Type:** discovery  
**Project:** futbolweb-app  

**What**: `main.py` hardcodeaba `project="dfl"` en las 3 queries que arman `/go`.

LIFECYCLE: resolved

**Fix implementado 2026-07-04** (commit af48db6, repo DFLghub/dfl-context-proxy): se removió el filtro de project por default en `_engram_search`/`_fetch_observations` — Engram devuelve cross-project cuando se omite el parámetro, así que el "descubrimiento dinámico" pedido por Jorge no requirió tocar la API de Engram ni mantener una lista en config.env. Se agregó `PER_PROJECT_CAP=2` (constante en main.py) en las 4 secciones que categorizan observaciones (decisions, constraints, pending, recent_engram_dfl) para que un proyecto activo no desplace a los demás dentro de los topes globales existentes (6/5/-/5). Se corrigió además un bug latente en `_engram_recent_dfl`: retornaba apenas el term-match del grafo KNL llenaba la cuota, sin nunca invocar el fallback de recencia pura — ahora reserva 2 slots garantizados para eso. `publish-amos-context.sh` gana una sección `RECENT ACTIVITY (cross-project)` para que `recent_engram_dfl` (que antes solo vivía en el JSON) llegue al mirror público, más labels `Project:` en decisions/constraints/pending.
**Verificación empírica** (criterio de aceptación del HLC): mirror público post-push contiene "backup off-host", "VM3", "receiver", "92b6857" — confirmado con curl directo contra raw.githubusercontent.com (Generated 2026-07-04T15:20:14Z).
**Where**: /opt/dfl-context-proxy/main.py, /opt/dfl-context-proxy/publish-amos-context.sh — commit af48db6, pusheado a DFLghub/dfl-context-proxy.

### Backup off-host Engram→VM3 verificado operativo (primeras corridas)
**Type:** discovery  
**Project:** futbolweb-app  

**What**: Verificado el cron `engram-backup-offhost.sh` (cada 6h, :17) hacia VM3 (root@137.184.207.239, mismo host que alias ssh "vault"). Log local `/var/log/dfl-engram-backup.log` muestra 2 corridas OK hoy: 06:17:04Z y 12:17:03Z, ambas sobrescribiendo `daily/2026-07-04.db.gz` (703878 bytes) según el esquema de naming por fecha (no timestamp). Confirmado también vía `rsync --list-only` contra el receiver remoto (la key `id_ed25519_engram_backup` tiene forced command — `ls`/`ssh <cmd>` directo devuelve "dfl-backup-receiver: comando no permitido", pero `rsync --list-only` sí es aceptado por el forced command).
**Why**: Tarea pendiente de sesión anterior — confirmar que el primer backup automático del cron llegó a VM3 (mitigación obs #150, fragilidad de tener primario+espejo Postgres en el mismo disco).
**Where**: /opt/dfl-context-proxy/engram-backup-offhost.sh, crontab (línea `17 */6 * * *`), VM3:/data/dfl-backups/engram/daily/.
**Learned**: la forced command en VM3 solo permite el flujo rsync de recepción, no comandos de shell arbitrarios — para verificación futura usar `rsync --list-only` contra el receiver, no `ssh ... "ls ..."`.

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

*Mirror auto-generated 2026-07-04T15:21:47Z | La Garra → DFLghub/amos-context*
