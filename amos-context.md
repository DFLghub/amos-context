# amOS Context — @$go Live Mirror
**Generated:** 2026-07-14T20:32:41Z  
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
| **CONSULTOR** | snapshot pegado o memoria local de sesión; no intenta fetch bloqueado | RESUMEN DE SESIÓN listo para EJECUTOR/Engram | relay |

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

**Failure rule:** Una corrección permitida. Segundo fallo: degradar a CONSULTOR o pedir EJECUTOR; no operar sobre producto ni estado DFL.

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

### [FINAL] Visualizer focused final reaudit GO
**Type:** decision  
**Project:** dfl  

LIFECYCLE: final
Date: 2026-07-14
Scope: Final focused adversarial reaudit of Visualizer remediation.
Target commit audited: 26312584773ae4854f455856e0c6438ec3630e25
Original audited base: b1b81e755c4a3120a05d0d0b4fb4a7de4e5e6cac
Harness branch/worktree: audit/focused-reaudit at /opt/visualizer-codex-audit
Verdict literal from /opt/visualizer-codex-audit/evidence/reaudit-codex-final/VERDICT.md: GO
Contract results: Honest reversibility PASS; Institutional traceability PASS; Layout and complete export PASS; Corruption recovery PASS.
Audit commits: 24dc0ab, b86a112, 635feeb, f5e993c, c8a5362, 57c1b7a, b841ff6.
Residual risks: approval.by is declarative; one lint warning; stale README origin wording; export remains browser-driven.
Next gate: Jorge human test from the iMac before institutional publication.
Restrictions honored: did not modify product, did not merge, did not push Visualizer repository.

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

### Paridad CC/Codex (shell+Engram) — provisioning EJECUTOR verificado + credencial SSH endurecida tras revisión Codex
**Type:** decision  
**Project:** futbolweb-app  

TOPIC: dfl/session/2026-07-14-codex-provisioning-complete
TYPE: decision
STATUS: active
DATE: 2026-07-14

**What**: Paridad práctica de SHELL y ENGRAM entre CC y Codex por orden ratificada de Jorge (Opción B, 2026-07-14). NO es "acceso idéntico a CC" en sentido estricto — no hay paridad garantizada de políticas de sesión, tooling ni controles de aprobación; el handoff operativo es simétrico, los controles de sesión no necesariamente. Provisioning: (1) SSH key ed25519 sin passphrase /opt/dfl-secrets/ssh-keys/codex-la-garra (root 0600, fingerprint SHA256:1944e3aTx8rmcrcCzLVUMBEqs0CV18taermfsVt7N7U); (2) pubkey en /root/.ssh/authorized_keys; (3) smoke-test PASS SSH-CODEX-OK; (4) config /opt/dfl-secrets/codex-amOS-config.env (root 0600); (5) handoff /opt/dfl-knowledge/agents/codex-paridad-handoff.md.

**Hardening aplicado 2026-07-14 tras revisión de seguridad de Codex** (la implementación inicial era una clave root ABIERTA — regresión reconocida): línea authorized_keys ahora `from="127.0.0.1,::1",restrict,pty` — atada a loopback, forwarding/agent/X11 desactivados (verificado: port-forward devuelve "administratively prohibited"), solo TTY. Shell EJECUTOR local intacta (smoke-test re-PASS). Backup /root/.ssh/authorized_keys.bak.20260714. Passphrase NO añadida (uso headless, decisión de Jorge). Riesgo residual asumido y documentado: SSH loopback permite a Codex eludir las aprobaciones de su propio sandbox; alcance root local si hay error/prompt-injection — clave tratada como secreto de máxima criticidad.

**Desviaciones del script original, verificadas**: (a) `engram auth-token create` NO existe en engram vdev; acceso Codex a Engram = `engram mcp` stdio (paridad 9422ab3), sin token; /opt/engram/.env NO tocado. Carpeta engram-tokens NO creada. (b) Nada bajo /opt/dfl-secrets entra a git (higiene, cf. 3957967); solo el handoff doc se commitea en dfl-knowledge.

**Why**: Reducir el punto único de falla en handoff, con superficie de credencial mínima.
**Learned**: /opt/dfl-secrets (nuevo, root 0700) distinto del protegido /etc/dfl-secrets (intacto). Lección de método: una clave SSH "de conveniencia" para atravesar sandbox debe nacer restringida (from/restrict), no abierta — el hardening posterior lo corrige pero el patrón correcto es capar desde el minuto cero.
**PROXIMO_AGENTE_DEBE**: Codex sourcea /opt/dfl-secrets/codex-amOS-config.env, valida su @$go EJECUTOR, y trata la clave como crítica.

### Limpieza 2026-07-14: Reminder 1a cerrada; 1Password.txt eliminado de Drive por Jorge (verificado)
**Type:** decision  
**Project:** futbolweb-app  

TOPIC: dfl/session/2026-07-14-limpieza-1p-reminder
TYPE: decision
STATUS: active
DATE: 2026-07-14

**What**: Sesión CC de limpieza con dos frentes, ambos CERRADOS. (1) Reminder Layer Phase 1a CERRADA: Copa del Mundo 2026 finalizada, los 5 partidos KO pendientes de Alejo ya se jugaron — obs #110 marcada [RESOLVED] + LIFECYCLE: archived. Verificado que NO existía entrada Reminder_Layer_1a en registro-vivo.json ni en ningún archivo de /opt/dfl-knowledge (el pendiente vivía solo en Engram) — sin edición ni commit porque no había nada que editar. (2) 1Password.txt en Drive (fileId 1g4-4BoWbdQ0JRvggnTTFxwnjjXVASczZ, 204B): ELIMINADO manualmente por Jorge en la UI de Drive el 2026-07-14, tras blocker inicial (el conector MCP de Drive no expone delete). Borrado VERIFICADO por CC: get_file_metadata devuelve "Requested entity was not found". Residual D-5/1Password de la Reconciliación v1 cerrado.
**Why**: Orden directa de Jorge 2026-07-14: eliminar 1Password.txt (noise backup antiguo, ya revisado por él) y cerrar Reminder 1a por fin de torneo.
**Learned**: El conector claude.ai Google Drive es read-mostly (sin delete/trash) — limpiezas destructivas en Drive requieren UI manual o rclone tras OAuth institucional (B-1, aún pendiente). Paridad CC/Codex sigue pendiente de ratificación de Jorge.

### [CIERRE DE SESIÓN] Paridad Codex y protección dfl-secrets completadas
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/session/2026-07-12-parity-secrets-close
TYPE: session_summary
STATUS: active
DATE: 2026-07-12
SUMMARY: Se cerró paridad operacional Codex=CC: perfiles Codex 0.144.1 migrados, trusts y reglas institucionales ajustados, sesión nueva con cero prompts, Engram manual multiproyecto PASS, VM3 rsync PASS; commit 9422ab3 pusheado y cierre previo Engram #248. Luego se tomó relevo CC: backup GPG de /etc/dfl-secrets verificado local+VM3, SHA cipher 33df04c5159de1f2c0a2b880f29a32d06317d0aa83aff4dd06a0415af926bdd8, restore off-host idéntico SHA e7e78d8f0f0f2628ec6f9232ffb8a6ff12ae2db879aacfd92b57e32f82e63b66, passphrase nueva solo keyfile root-0600, ZIP legacy retirado del HEAD/ignorado, historia preservada por secreto revocado; commit 3957967 pusheado, Engram #249. Residuales: ZIP Drive y 1Password.txt requieren OAuth/rclone; no tocados. Git limpio origin/main. NO_TOUCH preservado.

### Link demo enviado a Rubén — modo prueba, no oferta comercial
**Type:** decision  
**Project:** 360eventos  

**Qué**: Jorge confirmó que ya envió el link de https://360eventos.vercel.app a Rubén, explícitamente para que "abra y mire el demo funcional (agMVP) — un producto mínimo viable PERO FUNCIONAL". Lo enmarca como "Prueba" (test), no como oferta comercial.

**Impacto en Price Authority**: esto NO es una confirmación de Nivel 0B (catálogo comercial oficialmente aprobado). Sigue pendiente. El catálogo de 8 servicios reales en Supabase sigue siendo Nivel 0A (real vivo, válido para demo), usado aquí explícitamente en modo demo/prueba para Rubén, consistente con la política definida en FOF_CASE_01_360EVENTOS_PRICE_AUTHORITY_AND_SERVICE_TIERS.md (ready_to_send demo, no comercial).

**Why**: Jorge quiere que quede claro que el criterio de éxito ahora mismo es "MVP funcional demostrable", no "catálogo comercial validado". No se requiere ninguna acción adicional de mi parte — es una actualización de estado del Caso 01, no una nueva misión.

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

### AgMaster_amOS_3 — vocabulario y reglas IAIM
**Type:** fact  
**Project:** dfl  

AgMaster_amOS_3 es el documento maestro v3 del ecosistema (USAR ESTA, versiones 1 y 2 obsoletas). Vocabulario mínimo para IA invitada: amOS=sistema operativo conceptual/metodológico para absorber/metabolizar información manteniendo soberanía; IAIM=Invisible Augmented Intelligence Mesh, red invisible de IAs aliadas sin nodos fijos, orquestada por HI; HI=Jorge, decisión final soberana; ag10=ChatGPT como router/integrador/destilador/última yarda (no oráculo ni jefe); agPregunta=pregunta aumentada con propósito/dominio/límite/criterio; agLego=pieza conceptual candidata modular trazable; Candado Soberano=restricciones no negociables: no-exec, candidate_only, human-review-first. Prefijo 'ag'=augmented+governed+generative-but-contained. AG10-AUSTERITY-LOCK para fases de cierre/patch/gate. Origin Chain obligatorio para todo agLego. Frase núcleo v3: 'La red ilumina. La HI orquesta. ag10 destila. El candado audita. amOS asimila la cicatriz, no la herida.' Paralelo permitido: máximo 2 nodos, candidate_only, revisión HI.

### MERCADER — contexto operativo completo y restricciones duras
**Type:** fact  
**Project:** dfl  

MERCADER BOS v0.1 (2026-05-20): motor económico digital. Jerarquía: amOS/IAIM→Apps Factory→MERCADER→MERCADER BOS. Objetivo inmediato: cerrar circuito económico mínimo (lead capture→Gmail→registro→seguimiento→venta→aprendizaje). Las 5 capas BOS: (1)Contexto — qué es, qué vende, a quién, límites éticos; (2)Datos — 11 campos: nombre/email/tel/ciudad/fuente/producto/urgencia/presupuesto/estado/próx.acción/resultado; (3)Inteligencia — síntesis diaria: leads calientes, objeciones, canales; (4)Automatización — permitida: capturar/notificar/registrar/recordar; PROHIBIDA: cobrar sin revisión, inventar precios/inventario, hacerse pasar por Jorge; (5)Build — landing+formulario+Gmail+Sheets/Postgres+dashboard leads. Restricciones duras: sin empleados; sin storage propio; sin capital inicial fuerte; bajo costo operativo; humano en el cierre sensible; NO mezclar MERCADER con Bazaar ni con videos bioarmónicos. Modelo comercial: broker/coordinador/conector liviano. Score de oportunidad: Dolor+Urgencia+Capacidad+Claridad+Fit (máx 50). Morning Briefing + Evening Reflection operativos. Primer agLego: agLego-MERCADER-LEAD-CAPTURE-v0.1.

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

### [FINAL] Visualizer focused final reaudit GO
**Type:** decision  
**Project:** dfl  

LIFECYCLE: final
Date: 2026-07-14
Scope: Final focused adversarial reaudit of Visualizer remediation.
Target commit audited: 26312584773ae4854f455856e0c6438ec3630e25
Original audited base: b1b81e755c4a3120a05d0d0b4fb4a7de4e5e6cac
Harness branch/worktree: audit/focused-reaudit at /opt/visualizer-codex-audit
Verdict literal from /opt/visualizer-codex-audit/evidence/reaudit-codex-final/VERDICT.md: GO
Contract results: Honest reversibility PASS; Institutional traceability PASS; Layout and complete export PASS; Corruption recovery PASS.
Audit commits: 24dc0ab, b86a112, 635feeb, f5e993c, c8a5362, 57c1b7a, b841ff6.
Residual risks: approval.by is declarative; one lint warning; stale README origin wording; export remains browser-driven.
Next gate: Jorge human test from the iMac before institutional publication.
Restrictions honored: did not modify product, did not merge, did not push Visualizer repository.

### Session summary: dfl-knowledge
**Type:** session_summary  
**Project:** dfl-knowledge  

## Goal
Sesión @$go→@$fin 2026-07-14: MISIÓN remediación autónoma de Visualizer v0.1 — convertir el NO-GO controlado de la auditoría Codex (b1b81e7) en versión técnicamente homologable cerrando exactamente 4 bloqueantes.

## Accomplished
- Rama `remediation/visualizer-v01` en /opt/visualizer (local, sin remoto/push). Checkpoints: 4186465 (contrato honesto de reversibilidad), 48aa422 (proveniencia Caso Cero), f9f1a84 (dependencias/circuitos/export completo), 83be519 (recuperación ante corrupción), 2631258 (cierre consolidado). Árbol limpio, historial preservado.
- Bloqueante 1: VMD/JSON única representación reversible probada (deep-equal); Markdown degradado a proyección narrativa en README/UI/código/tests; compilación siempre nueva, nunca auto-aprobada.
- Bloqueante 2: origin extracted|inferred|modeled + approval {by,at} obligatorio por integridad + evidencia derivada located|cited|unsupported verificada contra fuentes; fixture reclasificado (0 approved, 8 extracted con spans reales, 83 modeled con ausencia registrada); endpoint /traceability; anti-concentración de evidencia.
- Bloqueante 3: viewInsight (connected/undeclared/isolated + fuera de alcance), roles de circuito (inicio/cierre/bifurcación), banda separada para no conectados, export PNG/SVG de grafo COMPLETO validado por IHDR exacto + pixel-sampling de nodos extremos (PASS 4/4 vistas).
- Bloqueante 4: ModelCorruptError, cuarentena sin destruir evidencia, restauración atómica desde revisión válida más reciente, fallo seguro, 14 escenarios de test + E2E HTTP 422→recover→200.
- Validación final: npm ci limpio, 83/83 tests (base 40), lint/typecheck/build 0, loopback verificado, npm audit runtime 0 vulnerabilidades (happy-dom critical resuelto con fix no-breaking; 5 findings toolchain vite/vitest documentados sin forzar majors).

## Discoveries
- El fixture original fabricaba certeza institucional: 86 elementos approved/source respaldados por UNA frase de 203 caracteres con una sola quote genérica.
- pkill -f con patrón que aparece en la propia línea de comando del shell se mata a sí mismo (exit 144) — matar servers por puerto (fuser -k).
- html-to-image no resuelve variables CSS de React Flow en etiquetas de aristas (cajas negras en export) — fix: estilos SVG inline (labelStyle/labelBgStyle).

## Next Steps
- Jorge: revisión visual 4 vistas en iMac/tablet (túnel SSH) con fixture ya honesto.
- Aportar fuentes doctrinales reales como sources del Caso Cero → reclasificar modeled con spans y aprobar con registro.
- Upgrade planificado vite@8+vitest@4 con suite 83 tests como red.
- NO avanzar a Business Genoma sin nueva orden.

## Relevant Files
- /opt/visualizer/evidence/remediation-cc/{WORKLOG,VALIDATION,REMAINING_RISKS,CASO_CERO_TRACEABILITY}.md
- /opt/visualizer/src/core/vmd/traceability.ts, src/core/views/insight.ts, src/web/export/exportViewport.ts, src/server/store.ts (recoverModel)
- /opt/visualizer/scripts/{reclassify-caso-cero,traceability-report,export-validate}.ts

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

**Graph entropy:** 1.3245  

- **Community 11** (77 nodes): Estado real de la casa, Onboarding multi-agente, Engram Cloud en VM2
- **Community 0** (12 nodes): amOS, agLego, HI
- **Community 1** (8 nodes): Tránsitos programados, Salud Estratégica, Decisiones Pendientes
- **Community 2** (8 nodes): Soberana - Gobernadora / Reductora, n8n, DO-INVENTORY
- **Community 3** (7 nodes): Metabolismo, Marco de Medición del Impacto, Fisiología Operacional
- **Community 4** (4 nodes): Soberanía de SaaS Factory V5, Clasificación de patrones externos

---

*Mirror auto-generated 2026-07-14T20:32:41Z | La Garra → DFLghub/amos-context*
