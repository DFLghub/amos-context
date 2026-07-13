# amOS Context — @$go Live Mirror
**Generated:** 2026-07-13T03:05:01Z  
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

### [CIERRE DE SESIÓN] Paridad Codex y protección dfl-secrets completadas
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/session/2026-07-12-parity-secrets-close
TYPE: session_summary
STATUS: active
DATE: 2026-07-12
SUMMARY: Se cerró paridad operacional Codex=CC: perfiles Codex 0.144.1 migrados, trusts y reglas institucionales ajustados, sesión nueva con cero prompts, Engram manual multiproyecto PASS, VM3 rsync PASS; commit 9422ab3 pusheado y cierre previo Engram #248. Luego se tomó relevo CC: backup GPG de /etc/dfl-secrets verificado local+VM3, SHA cipher 33df04c5159de1f2c0a2b880f29a32d06317d0aa83aff4dd06a0415af926bdd8, restore off-host idéntico SHA e7e78d8f0f0f2628ec6f9232ffb8a6ff12ae2db879aacfd92b57e32f82e63b66, passphrase nueva solo keyfile root-0600, ZIP legacy retirado del HEAD/ignorado, historia preservada por secreto revocado; commit 3957967 pusheado, Engram #249. Residuales: ZIP Drive y 1Password.txt requieren OAuth/rclone; no tocados. Git limpio origin/main. NO_TOUCH preservado.

### [CIERRE] /etc/dfl-secrets protegido off-host y ZIP legacy retirado
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/security/dfl-secrets-offhost-zip-close
TYPE: decision
STATUS: active
DATE: 2026-07-12
SUMMARY: backup GPG /opt/backups/organ-preservation/dfl-secrets-20260712.env.gpg confirmado en VM3 bajo /data/dfl-backups/engram/organ-preservation/2026-07-11-wave1; SHA-256 cipher local/remoto 33df04c5159de1f2c0a2b880f29a32d06317d0aa83aff4dd06a0415af926bdd8; restore desde copia off-host coincide byte a byte con /etc/dfl-secrets, SHA-256 e7e78d8f0f0f2628ec6f9232ffb8a6ff12ae2db879aacfd92b57e32f82e63b66; passphrase nueva únicamente en keyfile root-0600; ZIP 12_FutbolWeb/futbolweb-env-backup.zip retirado del HEAD y bloqueado en gitignore; historia no reescrita porque contiene solo Supabase key revocada; commit 3957967 pusheado origin/main. Drive ZIP y 1Password.txt NO tocados, pendientes OAuth/rclone. Evidencia audits/diagnostico-institucional-dfl-v1/13-CIERRE-ZIP-ANTIGUO.md y EVIDENCE/b14-relevo-dfl-secrets-y-zip.md. NO_TOUCH preservado.

### HLC cierre paridad Codex: evaluada NO completable 100% por CC (gate OAuth Drive) — handoff a Codex commiteado (aefdd96)
**Type:** decision  
**Project:** futbolweb-app  

**What**: Evaluación del HLC "Cerrar paridad operacional Codex=CC": NO completable al 100% por ningún agente solo — el Resultado #4 (conector Drive con OAuth institucional) exige consentimiento interactivo de Jorge en navegador (jtigre@gmail.com); no hay service account ni conector local (verificado). El propio HLC prohíbe declarar paridad con Drive bloqueado. Resultados 1/2/3/5/6 SÍ ejecutables por Codex solo. Handoff completo commiteado y pusheado: audits/consolidacion-institucional-dfl-v1/HANDOFF-PARIDAD-CIERRE.md (aefdd96).
**Why**: Mandato de Jorge: evaluar si CC alcanza a terminar con el crédito restante; si no, handoff a Codex.
**Where**: HANDOFF-PARIDAD-CIERRE.md — incluye estado verificado (falta trust /opt/dfl-context-proxy; MCP engram approval_mode=approve = fricción; default.rules ad-hoc 35 reglas), plan por resultado, parte exacta de Jorge (rclone config OAuth headless ~10 min), formato de CIERRE, reversibilidad.
**Learned**: DATO CLAVE: /opt/engram-mcp/server.py corregido HOY 20:11:45 UTC y servicio reiniciado 20:12:00 — el "adaptador Engram antiguo" que reporta Codex se resuelve con sesión NUEVA (Resultado 5 trivial). SSH "general a VMs" no existe para nadie: VM3 es forced-command por diseño (Resultado 3 = documentar, no ampliar). Codex debe pedir ratificación de rclone como conector institucional UNA vez antes de instalar.

### [CIERRE] Reconciliación final Consolidación DFL v1 (7b77b78) — D-1/D-2/B-2 resueltas, hallazgo Drive nuevo
**Type:** decision  
**Project:** futbolweb-app  

**What**: Reconciliación final de la Consolidación Institucional v1 (HLC 2026-07-12) COMPLETADA. Causa raíz de contradicciones: los artefactos de consolidación se construyeron sobre 06/09/obs#221, anteriores al cierre de residuales Ola 1 (docs 11/12/13, Codex 2026-07-11 noche). Hechos REVALIDADOS HOY contra realidad: (1) PAT: cero holders (0 archivos, 0 proc), remote prediccion2026 SSH sano — D-1 RESUELTA, sin fecha dura, retiro DESBLOQUEADO; (2) bundles off-host: pull rsync real desde /data/dfl-backups/engram/organ-preservation/2026-07-11-wave1/, SHA-256 idénticos los 4 — B-2 de la consolidación ERA FALSA (ruta correcta va bajo prefijo engram/); (3) SaaS Factory: DFLghub/saas-factory-setup main=5e42124=HEAD local, upstream push DISABLED — D-2 RESUELTA; (4) Drive vía conector CC: ZIP antiguo presente (524B) + HALLAZGO NUEVO 12_FutbolWeb/backups/1Password.txt (204B, 2026-07-06, fileId 1g4-4BoWbdQ0JRvggnTTFxwnjjXVASczZ) — NO LEÍDO, posible material de credenciales, requiere revisión Jorge; (5) paridad: Drive sigue única brecha material, perfil dfl-mission intacto. Artefactos corregidos: 00/01/02/04/05/06 + 08-RECONCILIACION-FINAL + HANDOFF-CODEX nuevos; registro-vivo.json actualizado. Verificador post-commit: solo 5 residuales reales (SIN-PUSH prediccion2026; SIN-RESPALDO engram-mcp/futbolweb-v2/mercader-comisiones/roof-issues-mini). Commit 7b77b78 pusheado. Nota: futbolweb recibió 2 commits de Codex producto (5595c24, e55d2c5) durante la sesión — no tocado.
**Why**: Mandato HLC — eliminar pendientes obsoletos antes de cerrar la Consolidación v1.
**Where**: /opt/dfl-knowledge/audits/consolidacion-institucional-dfl-v1/ (08-, HANDOFF-CODEX, EVIDENCE/reconciliacion-*), governance/registro-vivo/registro-vivo.json
**Learned**: Regla de método: antes de declarar pendiente en el registro vivo, contrastar contra el ÚLTIMO doc de cierre del expediente Y contra realidad ejecutable. Pendientes de Jorge tras reconciliación: retiros B-5 (desbloqueados), D-4 copias únicas, D-5 ZIP (CC ejecuta a la orden), revisión 1Password.txt, B-3 repos manuales, B-1 Drive-Codex.

### Link demo enviado a Rubén — modo prueba, no oferta comercial
**Type:** decision  
**Project:** 360eventos  

**Qué**: Jorge confirmó que ya envió el link de https://360eventos.vercel.app a Rubén, explícitamente para que "abra y mire el demo funcional (agMVP) — un producto mínimo viable PERO FUNCIONAL". Lo enmarca como "Prueba" (test), no como oferta comercial.

**Impacto en Price Authority**: esto NO es una confirmación de Nivel 0B (catálogo comercial oficialmente aprobado). Sigue pendiente. El catálogo de 8 servicios reales en Supabase sigue siendo Nivel 0A (real vivo, válido para demo), usado aquí explícitamente en modo demo/prueba para Rubén, consistente con la política definida en FOF_CASE_01_360EVENTOS_PRICE_AUTHORITY_AND_SERVICE_TIERS.md (ready_to_send demo, no comercial).

**Why**: Jorge quiere que quede claro que el criterio de éxito ahora mismo es "MVP funcional demostrable", no "catálogo comercial validado". No se requiere ninguna acción adicional de mi parte — es una actualización de estado del Caso 01, no una nueva misión.

### QA online demo 360eventos.vercel.app completado
**Type:** decision  
**Project:** 360eventos  

**Qué**: QA online del agMVP 360Eventos completado. URL pública validada: `https://360eventos.vercel.app` (encontrada manualmente por Jorge, proyecto Vercel `360eventos`). Documento: `/opt/dfl-knowledge/projects/fof/cases/360eventos/FOF_CASE_01_360EVENTOS_ONLINE_DEMO_QA.md` (commit 1a0fe6a).

**Resultado**: `/`, `/cotizar`, `/login` responden 200. `/cotizar` confirmado usando Nivel 0A (catálogo real Supabase, 8 servicios) — coincide exacto con la query directa hecha en la misión anterior: Producción general del evento, Decoración temática, Sonido profesional, Iluminación arquitectónica, Fotografía profesional, Video y transmisión en vivo, Transporte y montaje de equipos, Maestro de ceremonias. Formulario de cotización bien wireado a `submitCotizacion` (server action → insert en `solicitudes`), no se completó envío real para no escribir en DB de producción. Viewport responsive presente en las 3 páginas; home con 14 clases responsive Tailwind, cotizar con solo 2 (más simple pero funcional). Sin errores críticos visibles. No se aplicó ningún fix — no fue necesario.

**Limitaciones para Rubén**: es demo funcional no comercial (Nivel 0B pendiente), cualquier envío real de /cotizar genera fila real en `solicitudes` de producción (no hay modo sandbox), no se validó /dashboard ni login con credenciales reales, no se hizo validación visual real en dispositivo móvil (solo estructural).

**No se tocó**: DB (solo lecturas HTTP públicas), FutbolWeb, secrets, migraciones, seed. Cambios preexistentes ajenos en graphify-out/ag_topologo.py siguen intactos.

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

---

## PENDING

### @go Protocol v1.0 — protocolo de arranque DFL
**Project:** dfl  

Protocolo @go desplegado el 2026-06-24. Permite a cualquier IA (Claude, ChatGPT, Gemini) arrancar con contexto completo DFL en un paso. Fuente A: amos-context.md en DFLghub/amos-context (commit edd5f51). Fuente B: GET https://context.deepfeelingslabs.com/go — devuelve JSON con identity, recent_decisions, active_constraints, pending, generated_at. Backend: dfl-context-proxy en 127.0.0.1:8091 consulta Engram local 127.0.0.1:7437 con queries: "decisiones activas estado", "restricciones prohibido no tocar", "pendientes criticos". Sin auth requerida en /go. @go v1.0 activo y verificado.

### Reminder Layer Phase 1a — contacto manual Alejo iniciado
**Project:** futbolweb-app  

**What**: Jorge contactó manualmente a Alejo vía WhatsApp para corregir 5 pronósticos KO incompletos (1-1 sin advancing_team).
**Why**: Phase 1a aprobada — ciclo manual documentado antes de activar automatización.
**Where**: prediction_intake, grupo EGRESADOS FRANCISCANOS TULUÁ, phone +57316****311
**Partidos pendientes**: mundial-2026-partido-074 (Alemania vs Paraguay), -075 (Países Bajos vs Marruecos), -079 (México vs Ecuador), -082 (Bélgica vs Senegal), -084 (España vs Austria)
**Fecha contacto**: 2026-06-28
**Estado**: esperando corrección de Alejo. Verificar con dry-run GET /api/admin/reminder-candidates — cuando caseBKoDrawIncomplete = 1 (solo KO Prod Probe), Phase 1a cerrada.
**Condición para Phase 1b**: reminder_log en DB + template Meta aprobado + este ciclo documentado con corrección confirmada.

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

### amOS Event Model — veredicto auditoría 2026-06-23
**Type:** decision  
**Project:** dfl  

Auditoría del Event Model amOS realizada 2026-06-23 contra 3 docs canónicos (AgMaster_amOS_3, AI_amOS_Acta_Fundacional v1.1, Protocolo MS→amOS). Veredicto: B — Existe parcialmente pero disperso. Cobertura: Peso/costo metabólico→confidence+value en tabla events (Parcial, consolidar); Persistencia→status Origin Chain+estados Candidate Vault (Parcial, consolidar); Intención→scope+forbidden_uses agLego+Layer3 VALUE (Implícita, nombrar); Propagación→C-009+G-002 Protocol Taxonomy (Incompleta, GAP REAL); Relación con estado→Layer6+tabla asset_states (Existe, conservar). Conclusión: NO hace falta constructo nuevo tipo 'Light Signals'. Hace falta unificar y nombrar lo disperso. Gap real confirmado: G-002 Protocol Taxonomy (propagación, marcado como no cerrado en el Acta Fundacional). Próximo paso: cerrar G-002 dentro del Libro 1 amOS o como PRP independiente. Prerequisito: localizar RFC-DFL-001 (puede contener Event Model más completo).

### amOS — ontología activa 13 capas (Acta Fundacional v1.1)
**Type:** fact  
**Project:** dfl  

13 Capas ratificadas del ecosistema amOS (AI_amOS_Acta_Fundacional v1.1, 2026-06-15 FINAL): L1=REALITY (amOS models reality, never IS reality); L2=CONTEXT (architectural law, el contexto manda); L3=VALUE (produce/protect/enable/avoid consequences); L4=INFORMATION (utility is in relationship, not information); L5=ASSETS (Entity+ContextualValue+Identity+State+Relationships); L6=STATE (amOS revolves around State, not AI/GPTs/documents); L7=REGISTRIES (Asset+Protocol+State Registry); L8=PROTOCOLS (biggest gap, without protocols agMesh=concept); L9=HOMEOSTASIS (habits reducing degradation probability, not deterministic); L10=ATTENTION (scarcest resource is attention, not storage/tokens/compute); L11=ENERGY (ATP-D: consumes/costs/produces/recovers); L12=EVOLUTION (Candidate Vault→Triunvirato→Ratification→Doctrine); L13=CONSTITUTION (what can change/cannot/who governs/how it changes). Constitución activa: C-001 contexto determina valor; C-002 amOS modela realidad; C-005 ningún componente se autoaprueba; C-006 candidate only hasta ratificación HI; C-008 nada entra al núcleo sin TRIAGE; C-009 domain sovereignty (hard boundaries); C-013 Doctrine first-governance second-software third; C-015 amOS produce coherencia, no software.

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

### [CIERRE DE SESIÓN] Paridad Codex y protección dfl-secrets completadas
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/session/2026-07-12-parity-secrets-close
TYPE: session_summary
STATUS: active
DATE: 2026-07-12
SUMMARY: Se cerró paridad operacional Codex=CC: perfiles Codex 0.144.1 migrados, trusts y reglas institucionales ajustados, sesión nueva con cero prompts, Engram manual multiproyecto PASS, VM3 rsync PASS; commit 9422ab3 pusheado y cierre previo Engram #248. Luego se tomó relevo CC: backup GPG de /etc/dfl-secrets verificado local+VM3, SHA cipher 33df04c5159de1f2c0a2b880f29a32d06317d0aa83aff4dd06a0415af926bdd8, restore off-host idéntico SHA e7e78d8f0f0f2628ec6f9232ffb8a6ff12ae2db879aacfd92b57e32f82e63b66, passphrase nueva solo keyfile root-0600, ZIP legacy retirado del HEAD/ignorado, historia preservada por secreto revocado; commit 3957967 pusheado, Engram #249. Residuales: ZIP Drive y 1Password.txt requieren OAuth/rclone; no tocados. Git limpio origin/main. NO_TOUCH preservado.

### [CIERRE] /etc/dfl-secrets protegido off-host y ZIP legacy retirado
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/security/dfl-secrets-offhost-zip-close
TYPE: decision
STATUS: active
DATE: 2026-07-12
SUMMARY: backup GPG /opt/backups/organ-preservation/dfl-secrets-20260712.env.gpg confirmado en VM3 bajo /data/dfl-backups/engram/organ-preservation/2026-07-11-wave1; SHA-256 cipher local/remoto 33df04c5159de1f2c0a2b880f29a32d06317d0aa83aff4dd06a0415af926bdd8; restore desde copia off-host coincide byte a byte con /etc/dfl-secrets, SHA-256 e7e78d8f0f0f2628ec6f9232ffb8a6ff12ae2db879aacfd92b57e32f82e63b66; passphrase nueva únicamente en keyfile root-0600; ZIP 12_FutbolWeb/futbolweb-env-backup.zip retirado del HEAD y bloqueado en gitignore; historia no reescrita porque contiene solo Supabase key revocada; commit 3957967 pusheado origin/main. Drive ZIP y 1Password.txt NO tocados, pendientes OAuth/rclone. Evidencia audits/diagnostico-institucional-dfl-v1/13-CIERRE-ZIP-ANTIGUO.md y EVIDENCE/b14-relevo-dfl-secrets-y-zip.md. NO_TOUCH preservado.

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

**Graph entropy:** 1.1833  

- **Community 11** (88 nodes): Matriz de Fisiología Contextual, Drift en gestión de versiones, Acumulación de patrones operacionales
- **Community 0** (12 nodes): ag10, agLego, amOS
- **Community 1** (4 nodes): Soberanía de SaaS Factory V5, Análisis de patrones externos
- **Community 2** (4 nodes): Salud Institucional DFL, Arquitectura Multi-Servicio en VM2
- **Community 3** (4 nodes): Durabilidad de Engram, Scripts sin commit, Producto en modo development
- **Community 4** (4 nodes): Salud Cognitiva

---

*Mirror auto-generated 2026-07-13T03:05:01Z | La Garra → DFLghub/amos-context*
