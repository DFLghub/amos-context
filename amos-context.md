# amOS Context — @$go Live Mirror
**Generated:** 2026-07-22T21:36:03Z  
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

### Checkpoint @$fin — SFV5→V6 Business OS + JPI MVP rebuild (sesión abierta, no cerrada)
**Type:** decision  
**Project:** dfl  

**What**: Sesión Claude Code (EJECUTOR) sobre /opt/futbolweb, trabajando cross-proyecto por pedido de Jorge. Cadena de misiones ejecutada en este orden:
1. `@$go` bootstrap — perfil EJECUTOR confirmado vía /go local (127.0.0.1:8091) + anexo agents/ejecutor.md.
2. Auditoría de factibilidad SFV5→V6 (fork) → `/opt/experiments/SFV5_TO_V6_FEASIBILITY.md`. Veredicto: GO parcial (mecanismo base viable; conectores Telegram/WhatsApp/Gmail NO-GO por falta de código base).
3. Misión 02 (fork) — piloto aislado del Business OS en `/opt/experiments/sfv5-business-os-pilot/`. PASS, 17/17 tests. Sin commits (no pedidos). Reporte: `/opt/experiments/SFV5_BUSINESS_OS_PILOT_REPORT.md`.
4. Misión 03 (fork) — build soberano del Business OS sobre `/opt/360eventos` (autorización explícita de Jorge para modificar 360eventos completo, revocando restricción previa; ver **Learned**). PASS, 7/7 tests. Commit local `78cfdf0`. Reporte: `/opt/experiments/SFV5_BUSINESS_OS_360EVENTOS_REPORT.md`.
5. Misión 04 (fork) — reconstrucción del MVP Transporte/Eventos JPI usando fuentes reales (catálogo digital de Rubén + docs DDMS), reemplazando oferta inventada anterior, integrando Business OS de (4). **Jorge pidió detener el fork a mitad de ejecución** (`Detén el fork. Guarda lo ya hecho...`). Fase de discovery quedó completa y commiteada (`d9d7bf2`). Fase de implementación quedó parcial, comiteada como WIP por mí tras el stop (`7d66327`) para no perder trabajo.

**Why**: Jorge evalúa si el patrón "Business OS" anunciado para SaaS Factory V6 es reproducible sobre SFV5 instalado, y quiere usarlo como capa operativa real para relanzar 360eventos (que ya tenía una estrategia de pivot a MVP V2 greenfield decidida el 2026-07-19, commit b766e37) usando el catálogo real de Rubén en vez de datos inventados.

**Where**:
- `/opt/experiments/SFV5_TO_V6_FEASIBILITY.md`, `SFV5_BUSINESS_OS_PILOT_REPORT.md`, `SFV5_BUSINESS_OS_360EVENTOS_REPORT.md`
- `/opt/experiments/sfv5-business-os-pilot/` (piloto aislado, worktree propio, no tocar desde 360eventos)
- `/opt/360eventos/business-os/` (Business OS soberano, Node/Express + SQLite local, puerto 4500 preferido con fallback automático) — commit `78cfdf0`
- `/opt/360eventos/docs/discovery/`, `domain/` (fuentes reales: catálogo Rubén en `docs/discovery/sources/catalogo-ruben/`, ontología DDMS, facts, business rules) — commit `d9d7bf2`
- `/opt/360eventos/src/features/jpi/` (db/migrations x6, domain/missing-information.mjs, domain/states.mjs, repository/catalogo.repository.mjs) — commit WIP `7d66327`, **incompleto**: sin migraciones ejecutadas, sin tests, sin capa API/UI, sin integración real con business-os/

**Learned**:
- `/opt/360eventos/.env.local` apunta a Supabase REAL activo (`uvdunupmjrbndistyrwn.supabase.co`), no placeholder — confirmado por mí antes de autorizar cualquier escritura. Jorge eligió explícitamente mantener todo storage nuevo (Business OS y JPI) aislado en SQLite local, sin tocar ese Supabase ni leer sus credenciales. Esta restricción sigue vigente para cualquier continuación futura, pese a que Jorge autorizó "modificar esquema y datos" de 360eventos en términos generales — esa autorización se interpretó y confirmó como NO aplicable al Supabase vivo.
- Semántica canónica obligatoria para JPI: `SOLICITUD_DE_COTIZACION` → `REQUIERE_INFORMACION` → `COTIZACION`. `PRECOTIZACION` es término legacy prohibido (decisión DFL 2026-07-19).
- No hardcodear lógica fiscal — perfil tributario es config, no código.
- No bloquear con interrogatorios — detección de info faltante debe ser no-bloqueante.
- Este es un **checkpoint** (@$fin modo CHECKPOINT pedido explícitamente por Jorge), no cierre real: sin barrido de archivado, sin `push_mirror.sh`. La sesión sigue abierta y puede continuar la Misión 04 desde el commit WIP `7d66327` (implementar API/UI, correr migraciones, escribir e integrar tests, conectar con business-os/) cuando Jorge lo pida.
- En todo el hilo: sin push, sin mirror, sin escritura previa a Engram — este es el primer mem_save de la sesión.

### 360Eventos MVP V2 greenfield strategy on SaaS Factory
**Type:** decision  
**Project:** dfl  

On 2026-07-19, 360Eventos was reoriented from patching the legacy MVP to building a new MVP V2 using the SaaS Factory checkout at /opt/saas-factory-setup/saas-factory as a greenfield base. Documentation was created under /opt/360eventos/docs/mvp-v2/2026-07-19-greenfield-strategy and committed as b766e37 docs(360eventos): define MVP V2 greenfield strategy. Decision: use SFV5 candidate as READY_AS_GREENFIELD_BASE_WITH_GUARDRAILS, treat /opt/360eventos legacy as read-only reference, do not continue FS-01 patching, do not apply migration-10, and transfer only domain semantics, rules, synthetic scenarios and selected lessons. Canonical semantics: SOLICITUD_DE_COTIZACION, REQUIERE_INFORMACION, COTIZACION; PRECOTIZACION does not exist and must not appear except as a forbidden legacy term. No functional code, data, Supabase, Vercel, infrastructure, migrations, secrets or NO_TOUCH zones were modified.

### DRG-001 emitido: cierre normativo FS-01 360Eventos (mapeo legacy, sunset dual-write, AuthZ oficial, atomicidad, impacto FS-02/03)
**Type:** decision  
**Project:** dfl-knowledge  

TOPIC: dfl/360eventos/drg-001-fs01-closure
TYPE: decision
STATUS: active
DATE: 2026-07-19

**What**: DRG-001 emitido — documento normativo de cierre de FS-01 en /opt/360eventos/docs/architecture/DRG-001-FS01-CLOSURE.md (sin commit; misión "no escribas código" cumplida: solo documento). Decisiones vinculantes: D1 mapeo legacy definitivo (nueva→RECIBIDA, en_revision→RECIBIDA porque PENDIENTE_INFORMACION exigiría nota inexistente, cotizada→CALIFICADA, cerrada→ARCHIVADA_LEGACY [valor terminal nuevo fuera de la máquina de estados], cancelada→RECHAZADA con motivo literal de migración; fallback silencioso a RECIBIDA PROHIBIDO). D2 filas irregulares → estado_ddms NULL + "requiere revisión de datos"; con datos reales de cliente toda migración de estados exige revisión humana nominal. D3 sunset: estado_ddms única verdad; estado legacy congelado read-only en migration-11, drop físico al cierre de FS-03; CALIFICADA→'cotizada' abolido. D4 dual-write prohibido como patrón; servicios congelada; cotizaciones (migration-05) declarada muerta para FS-03. D5 AuthZ de aplicación primaria + RLS honesta (correcta o eliminada); roles DDMS migran YA en profiles CHECK (admin→ADMINISTRADOR); INSERT anónimo muerto se elimina. D6 atomicidad: cambio de estado + historial en una transacción vía RPC Postgres; aplica a toda entidad con estado del proyecto; calificar NO re-valida fecha_evento pasada. D7 FS-02 tablas nuevas + snapshot service como gate de salida + enums en src/domain/ único; FS-03 versionado nativo + secuencias. Condiciones de cierre FS-01 bloquean FS-02, incluida la obligación de versionar docs/ y domain/ en git. migration-10 SUPERSEDIDA.

**Why**: Jorge ordenó misión DRG-001 (Architecture Review Board, decisiones no opciones) tras el informe de riesgos de la revisión read-only (obs #269). Evidencia habilitante de mapeos incondicionales: CLIENT_PRODUCTION_GATE BLOCKED + datos solo demo.

**Next**: migration-11 debe redactarse conforme a DRG-001 y aplicarse solo con autorización explícita de Jorge; Codex auditando migración en paralelo debe recibir DRG-001 como norma superior a migration-10.

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

### Visualizer v0.1 remediado: 4 bloqueantes NO-GO cerrados (rama remediation/visualizer-v01, 83/83 tests)
**Type:** decision  
**Project:** dfl-knowledge  

TOPIC: dfl/visualizer/remediation-v01-complete
TYPE: decision
STATUS: active
DATE: 2026-07-14

**What**: Remediación autónoma CC de los 4 bloqueantes del NO-GO de Visualizer v0.1 (auditoría Codex b1b81e7) COMPLETADA. Rama local `remediation/visualizer-v01` en /opt/visualizer (sin remoto, sin push, por diseño). Commits: 4186465 (contrato honesto de reversibilidad: VMD/JSON única representación canónica reversible, Markdown proyección narrativa, compilación siempre nueva nunca auto-aprobada), 48aa422 (proveniencia Caso Cero: origin extracted|inferred|modeled, approval {by,at} obligatorio por integridad, evidencia derivada located|cited|unsupported verificada contra fuentes, fixture reclasificado 0 approved / 8 extracted con spans reales / 83 modeled con ausencia registrada, endpoint /traceability, anti-concentración de evidencia >3), f9f1a84 (dependencias/circuitos: viewInsight connected/undeclared/isolated + fuera de alcance, roles inicio/cierre/bifurcación, banda separada para no conectados, export PNG/SVG de grafo COMPLETO validado por IHDR + pixel-sampling de nodos extremos), 83be519 (recuperación: ModelCorruptError, cuarentena sin destruir evidencia, restauración atómica desde revisión válida más reciente, fallo seguro, 14 escenarios de test), 2631258 (cierre consolidado).

**Why**: Codex emitió NO-GO controlado; Jorge ordenó remediación autónoma exclusiva de esos 4 bloqueantes para hacer Visualizer técnicamente homologable.

**Evidence**: Suite 40→83 tests PASS; lint/typecheck/build 0; npm ci limpio; loopback 127.0.0.1:4310 verificado; npm audit runtime 0 vulnerabilidades (5 findings toolchain vite/vitest documentados, majors no forzados). Evidencia en /opt/visualizer/evidence/remediation-cc/ (WORKLOG.md, VALIDATION.md, REMAINING_RISKS.md, CASO_CERO_TRACEABILITY.md, exports validados 4 vistas).

**Next**: (1) Jorge: revisión visual de las 4 vistas en iMac/tablet vía túnel SSH (README) con fixture ya reclasificado como proposed; (2) aportar fuentes doctrinales reales como sources del Caso Cero y aprobar elementos con registro; (3) upgrade planificado vite@8+vitest@4 con la suite como red; (4) NO avanzar a Business Genoma sin nueva orden.

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

**Type:** manual  
**Project:** futbolweb-app  

CIERRE Nivel A — Auditoría Codebase Memory VM2, 2026-07-21. Aplicado y validado como PASS con caveats (decisión y condiciones explícitas de Jorge).

CAMBIOS REALES:
- /root/.claude/settings.json: permissions.allow pasó de ["Bash"] a ["Bash"] + 14 tools mcp__codebase-memory-mcp__* (todas, incluidas delete_project y manage_adr — Jorge pidió explícitamente las 14 sin excepción, no la exclusión de destructivas que yo había propuesto). Backup: /root/.claude/settings.json.bak-cbm-nivelA-20260721-230809
- /root/.codex/config.toml: agregados 7 bloques [mcp_servers.codebase-memory-mcp.tools.<tool>] approval_mode="approve" (index_status, list_projects, delete_project, detect_changes, query_graph, manage_adr, ingest_traces) — total 14/14. Backup: /root/.codex/config.toml.bak-cbm-nivelA-20260721-230809
- No se tocó /opt/futbolweb/.claude/settings.local.json ni ninguna otra superficie (confirmado con git diff).
- Índices duplicados (saas-factory-sfv5/opt-saas-factory-v5, opt-360eventos/360eventos-legacy) siguen intactos — NO se borraron, instrucción explícita.

VALIDACIÓN:
- Claude Code: 4 tools nunca antes autorizadas (get_graph_schema, query_graph, manage_adr modo get, ingest_traces vacío) ejecutadas sin ningún prompt. query_graph devolvió 906 nodos para opt-futbolweb, consistente con list_projects.
- Codex: `codex doctor` confirma config.toml parse ok, 2 MCP servers, 0 disabled. `codex exec` invocó query_graph sin bloquear, PERO corre con approval:never por diseño headless — NO prueba el comportamiento real de aprobación en sesión interactiva. Queda pendiente esa verificación específica para una sesión futura con Codex interactivo real.

HALLAZGO ABIERTO (registrado, NO investigar sin pedido explícito): en el run de `codex exec`, la misma query Cypher que devolvió 906 en Claude Code devolvió "10" vía Codex. Verificado inmediatamente después con list_projects directo que el store NO está corrupto ni duplicado — los 8 proyectos reales mantienen sus conteos esperados. Causa no determinada (posible reformulación de query por el agente Codex, alcance de proyecto distinto, o artefacto de respuesta del modelo).

Entregables actualizados con este cierre: /opt/dfl-knowledge/audits/codebase-memory-v1/{00-README.md, CODEBASE_MEMORY_AUDIT.md (addendum), ROOT_CAUSE_MATRIX.md (filas 1-3 marcadas APLICADO), IMPLEMENTATION_OPTIONS.md (Nivel A con estado), TEST_PLAN.md (sección de verificación con resultados PASS/PARCIAL)}.

Pendiente para el futuro: puntos 3-4 de Nivel A (indexar los ~11 repos de /opt nunca indexados, limpiar duplicados confirmados), validación interactiva real de Codex, investigación de la anomalía "10 vs 906" si se decide abrirla, y Nivel C (DFL Knowledge Adapter) como arquitectura recomendada de destino — ver DFL_KNOWLEDGE_ADAPTER_PROPOSAL.md.

**Type:** manual  
**Project:** futbolweb-app  

AUDITORÍA Codebase Memory (codebase-memory-mcp v0.9.0) en VM2/La Garra — completada 2026-07-21. Entregables en /opt/dfl-knowledge/audits/codebase-memory-v1/ (00-README, CODEBASE_MEMORY_AUDIT, ROOT_CAUSE_MATRIX, DFL_KNOWLEDGE_ADAPTER_PROPOSAL, IMPLEMENTATION_OPTIONS, TEST_PLAN).

HALLAZGOS CLAVE:
1. Causa raíz de "autorizaciones repetidas": dos sistemas de permisos de cliente desincronizados — Claude Code usa allowlist por-proyecto en .claude/settings.local.json (solo /opt/futbolweb tiene entradas, 9/14 tools; falta query_graph, get_graph_schema, delete_project, manage_adr, ingest_traces) vs Codex usa approval_mode por tool en ~/.codex/config.toml (7/14 tools). Ningún otro proyecto activo de /opt (~11 repos: dfl-knowledge, 360eventos, mercader-comisiones, engram, etc.) tiene autorización. El propio SKILL.md del producto recomienda query_graph/get_graph_schema, que nunca están autorizadas — garantiza interrupción en el primer uso documentado.
2. El motor NO tiene timeout configurable ni es lento: reindexar futbolweb (906 nodos) tomó 0.11s vía CLI directo. No se pudo reproducir >300s con ningún repo real de la VM. Causa más probable de los reportes de "timeout": bug confirmado y reproducible que crashea el worker de indexación con inputs problemáticos (mensaje "Indexing worker crashed on a file... contained") — registrado también en logs reales del 2026-07-15. El timeout percibido es del cliente MCP, no del motor.
3. Sin locks explícitos (solo SQLite WAL) — probado limpio en régimen rápido (3 rondas de doble-index concurrente + 10 reads durante write, sin errores) pero régimen lento/minutos queda no probado.
4. Duplicación real confirmada por falta de idempotencia (repo+commit): saas-factory-sfv5/opt-saas-factory-v5 y opt-360eventos/360eventos-legacy son el MISMO repo+commit indexado dos veces bajo nombres distintos, grafos divergentes. No hay project_key canónico.
5. Servidor es child process por sesión (no daemon systemd/docker), persiste independientemente del timeout de un tool-call individual.

DECISIÓN RECOMENDADA: Nivel A (config global inmediata, 1-2h, bajo riesgo) ya + Nivel C (DFL Knowledge Adapter — servicio persistente delante del motor con jobs async/job_id, project_key canónico, locks explícitos, allowlist propio de rutas, health check, contrato uniforme CC/Codex) como destino estructural, 1-2 semanas. Nivel D (fork del núcleo) explícitamente NO justificado por evidencia — el motor es confiable en el régimen probado.

PENDIENTE [D] desconocido: compatibilidad con Visualizer/KNL/agTopólogo no investigada — requiere sesión de descubrimiento de contrato antes de diseñar el export de grafo del Adaptador. También pendiente: test de indexación real en régimen lento (repo grande, minutos) — no reproducible en esta VM por falta de repos suficientemente grandes.

Limpieza propia: se borraron 2 proyectos de prueba huérfanos (tmp-cbm-test-large, tmp-cbm-test-small) dejados por los forks de reproducción, confirmados por nombre/ruta antes de borrar. No se tocó ningún proyecto real ni configuración de ningún agente — la auditoría fue de solo lectura salvo esa limpieza y los índices desechables de prueba en /tmp.

Método: 4 subagentes fork en paralelo (auth/config, storage/logs/forensics, reproducción controlada, test de concurrencia dedicado) + verificación directa del orquestador.

### [CERTIFIED] Roadmap DFL @$go/KNL/hooks — orquestación 2026-06-27
**Type:** decision  
**Project:** dfl  

**What**: Orquestación ejecutiva del stack DFL post-certificación: incidente FW, slim /go, fallback local, handoff bidireccional, modelo de confianza.

**Decisiones:**
- Incidente FW 2026-06-19: STALE. Engram #14 lo cierra (fix aplicado 2026-06-24). El `pending` en /go es ruido — debe limpiarse. Los archivos dirty en /opt/futbolweb (espn-world-cup.ts +175l, scoring-propagation.ts +30l) son trabajo en progreso nuevo, no el incidente.
- Slim /go: servir solo restrictions + god_nodes + pending + recent_decisions(max 4) por defecto. Identity → 2 líneas. graph_summary → ?full=1. -40% payload estimado.
- Fallback local: cc-atgo-hook.sh debe leer /opt/dfl-knowledge/graphify-out/knl.json si /go falla. Banner MODO FALLBACK para agente. Elimina único punto de fallo silencioso.
- Handoff bidireccional: agent-lock.json en /opt/dfl-context-proxy/. Read en SessionStart, write al iniciar trabajo, cleanup en session_end. Sin infraestructura distribuida.
- Modelo confianza: convención [VERIFIED]/[CERTIFIED]/[CLAIMED]/[STALE] en títulos Engram. Cero código. Si restricción es CLAIMED, preguntar a Jorge antes de actuar.

**Orden de ejecución recomendado:**
1. Limpiar pending Engram #14 (5 min)
2. Fallback local cc-atgo-hook.sh (30 min)
3. Slim /go payload main.py (1h)
4. Convención evidence_level en Engram (inmediato, convención)
5. Agent lock file — solo cuando haya colisiones reales documentadas

**Why**: Reducir ruido en bootstrap inter-agente y eliminar puntos de fallo sin tocar producción.

**Where**: cc-atgo-hook.sh, main.py (/go endpoint), Engram project dfl.

**Learned**: Codex demostró que /go ya transfiere suficiente contexto para reconstruir el testigo sin intervención humana. El sistema funciona — necesita afinamiento, no rediseño. Los dirty files de FutbolWeb son trabajo pendiente en la pipeline ESPN/scoring; requieren sesión dedicada con PRP antes de commit.

**Type:** manual  
**Project:** futbolweb-app  

CHECKPOINT PROVISIONAL (@$fin modo Checkpoint — sesión activa continúa, NO es cierre final) — 2026-07-22. Handoff rápido para Codex u otra sesión si esta se corta.

ESTADO GENERAL: Las 4 misiones de esta sesión están COMPLETAS y cerradas correctamente, ninguna en curso ahora mismo. No hay trabajo pendiente sin resolver — el sistema está en punto de espera de nueva instrucción de Jorge.

RESUMEN DE LAS 4 MISIONES (en orden cronológico, cada una con su expediente completo en disco):

1. AUDITORÍA CODEBASE MEMORY (VM2) — /opt/dfl-knowledge/audits/codebase-memory-v1/ (6 docs + README). Nivel A aplicado y cerrado PASS con caveats. Backups en /root/.claude/settings.json.bak-cbm-nivelA-20260721-230809 y /root/.codex/config.toml.bak-cbm-nivelA-20260721-230809. Pendiente NO resuelto (dejado abierto a propósito): anomalía "10 vs 906" en codex exec, validación interactiva de Codex real.

2. REVISIÓN INDEPENDIENTE BOS v2 vs CODEX — /opt/dfl-knowledge/audits/business-os-peer-review-cc/ (6 docs). Veredicto: CONCUERDO CON CORRECCIONES IMPORTANTES. Hallazgo: BOS es infraestructura periférica real, sin cognición gerencial; daemon externo ausente en VM2.

3. DISEÑO "PRIMERA FÁBRICA DFL OPERABLE v0.1" — /opt/dfl-knowledge/architecture/first-operable-factory-v01/ (12 docs). Especifica el factory-manager-daemon (FMD), contrato amOS↔BOS, gates G0-G7, piloto JPI (JPI_MINUTES_PILOT.md), brechas TDL (ausente)/MERCADER. Solo diseño, cero código.

4. BOOTSTRAP FMD-G1 (ejecución real) — /opt/dfl-knowledge/evidence/first-operable-factory-bootstrap-g1/ (9 docs). Gate G1: PASS. Construido por 3 agentes workforce (implementador→tester→validador independiente, 0 reasignaciones) en /opt/360eventos/business-os/ (JPI, laboratorio con permiso total). Commits locales sin push: c7ff2ab (impl), 3c184d4 (tests). Detenido correctamente al cerrar G1, tal como se pidió. PRÓXIMO PASO NATURAL (no iniciado): conectar el esqueleto FMD-G1 con el dominio real de JPI (src/features/jpi/domain/states.mjs y missing-information.mjs) para poder correr JPI_MINUTES_PILOT.md completo.

RESTRICCIONES ACTIVAS QUE SIGUEN VIGENTES: no tocar Supabase real/credenciales/despliegues de BOS v2 ni SFV5 (JPI SÍ es laboratorio libre, declarado explícitamente por Jorge). No reabrir ni modificar Nivel A de Codebase Memory sin pedido explícito. No avanzar sobre FMD completo/aprendizaje/Plasticidad/Metabolismo/SFV5/TDL/MERCADER/piloto JPI completo sin nueva misión explícita de Jorge — la misión de bootstrap dijo "detente al cerrar Gate G1" y así se hizo.

Si esta sesión se corta acá: los 4 expedientes completos en disco + este mem_save son suficientes para que Codex retome sin preguntas — no hay estado intermedio roto, todo está en un punto de cierre limpio esperando la siguiente misión.

**Type:** manual  
**Project:** futbolweb-app  

MISIÓN BOOTSTRAP FMD-G1 COMPLETADA — Gate G1: PASS. Cierra el checkpoint provisional anterior (obs-ea46c04ae2e71f20) — esta es la conclusión real, no otro checkpoint.

Entregables en /opt/dfl-knowledge/evidence/first-operable-factory-bootstrap-g1/ (9 archivos): MISSION_ACTIVATION, WORKFORCE_ASSIGNMENTS, EXECUTION_LOG, G1_EVIDENCE_INDEX, INDEPENDENT_VALIDATION, AUTONOMY_METRICS, ROLLBACK, JPI_HANDOFF, FINAL_VERDICT.

QUÉ SE CONSTRUYÓ (por 3 agentes workforce, no por Claude Code): en /opt/360eventos/business-os/ (piloto Express+SQLite ya existente en JPI, laboratorio con permiso total) — migración 006_fmd_goals.js (tablas goals/plans/agents), módulo fmd/goals.js (Gate G0 validación, generador de plan determinístico 2 pasos, Gate G1 evaluación real contra DB de agentes), rutas POST/GET /business-os/api/goals*, 14 tests automatizados nuevos. Commits: c7ff2ab (implementación), 3c184d4 (tests) — locales, sin push.

WORKFORCE: Agente A (Implementador) → Agente B (Tester) → Agente C (Validador independiente), secuencial por dependencia real. LOS 3 CERRARON EN EL PRIMER INTENTO — cero reasignaciones, cero fallos, cero retrabajo. Validación independiente dio PASS con evidencia propia (no citó autorreportes): 21/21 tests, 3 casos adversariales incluyendo carrera real de 10 requests concurrentes con mismo idempotency_key → 1 sola fila sin duplicar, scope verificado limpio en todo /opt/360eventos y confirmado que /opt/experiments/business-os-new-audit y /opt/saas-factory-setup quedaron intocados. 1 caveat no bloqueante documentado (falta try/catch SQLITE_CONSTRAINT para futuro driver async).

AUTONOMÍA: cero intervenciones técnicas de Jorge durante la ejecución (sus 3 mensajes fueron administrativos: checkpoint de protocolo, pedido de reporte, autorización de continuar — no coordinación técnica). Claude Code actuó solo como supervisor: cero líneas de código de business-os/ escritas por el supervisor.

DECISIÓN DE ARQUITECTURA clave tomada por el supervisor (justificada en MISSION_ACTIVATION.md): construir en ruta aislada dentro de business-os/ (JPI) en vez de tocar BOS v2 auditado o SFV5, para evitar riesgo de credenciales/Supabase real sin necesidad — reutiliza patrón ya probado (migraciones idempotentes, tests reales) del propio JPI.

PRÓXIMO PASO NATURAL (fuera de esta misión, documentado en JPI_HANDOFF.md): conectar este esqueleto con el dominio real de JPI (states.mjs, missing-information.mjs) para poder correr JPI_MINUTES_PILOT.md completo — el FMD mínimo todavía NO está listo para dirigir el piloto completo, uso datos sintéticos propios del incremento, no el dominio real. Detenido en el cierre de G1 tal como la misión pidió explícitamente.

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

**Graph entropy:** 1.0406  

- **Community 11** (86 nodes): MCP Server Behavior, Evaluación de Plantillas, Preguntas para el Desarrollador
- **Community 0** (12 nodes): Tree-sitter Grammars, Matriz de Contención Operacional, Evaluación de Repositorios
- **Community 1** (6 nodes): NBLM2, Working Memory, Active Library
- **Community 2** (4 nodes): Estructura de datos en Rust
- **Community 3** (4 nodes): Blade en Laravel, Beancount, MCP (Model Context Protocol)
- **Community 4** (4 nodes): Documentación JSDoc, Estado de la casa DFL, Onboarding multi-agente

---

*Mirror auto-generated 2026-07-22T21:36:03Z | La Garra → DFLghub/amos-context*
