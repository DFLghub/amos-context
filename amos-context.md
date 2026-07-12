# amOS Context — @$go Live Mirror
**Generated:** 2026-07-12T20:17:12Z  
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

### [CIERRE] Reconciliación final Consolidación DFL v1 (7b77b78) — D-1/D-2/B-2 resueltas, hallazgo Drive nuevo
**Type:** decision  
**Project:** futbolweb-app  

**What**: Reconciliación final de la Consolidación Institucional v1 (HLC 2026-07-12) COMPLETADA. Causa raíz de contradicciones: los artefactos de consolidación se construyeron sobre 06/09/obs#221, anteriores al cierre de residuales Ola 1 (docs 11/12/13, Codex 2026-07-11 noche). Hechos REVALIDADOS HOY contra realidad: (1) PAT: cero holders (0 archivos, 0 proc), remote prediccion2026 SSH sano — D-1 RESUELTA, sin fecha dura, retiro DESBLOQUEADO; (2) bundles off-host: pull rsync real desde /data/dfl-backups/engram/organ-preservation/2026-07-11-wave1/, SHA-256 idénticos los 4 — B-2 de la consolidación ERA FALSA (ruta correcta va bajo prefijo engram/); (3) SaaS Factory: DFLghub/saas-factory-setup main=5e42124=HEAD local, upstream push DISABLED — D-2 RESUELTA; (4) Drive vía conector CC: ZIP antiguo presente (524B) + HALLAZGO NUEVO 12_FutbolWeb/backups/1Password.txt (204B, 2026-07-06, fileId 1g4-4BoWbdQ0JRvggnTTFxwnjjXVASczZ) — NO LEÍDO, posible material de credenciales, requiere revisión Jorge; (5) paridad: Drive sigue única brecha material, perfil dfl-mission intacto. Artefactos corregidos: 00/01/02/04/05/06 + 08-RECONCILIACION-FINAL + HANDOFF-CODEX nuevos; registro-vivo.json actualizado. Verificador post-commit: solo 5 residuales reales (SIN-PUSH prediccion2026; SIN-RESPALDO engram-mcp/futbolweb-v2/mercader-comisiones/roof-issues-mini). Commit 7b77b78 pusheado. Nota: futbolweb recibió 2 commits de Codex producto (5595c24, e55d2c5) durante la sesión — no tocado.
**Why**: Mandato HLC — eliminar pendientes obsoletos antes de cerrar la Consolidación v1.
**Where**: /opt/dfl-knowledge/audits/consolidacion-institucional-dfl-v1/ (08-, HANDOFF-CODEX, EVIDENCE/reconciliacion-*), governance/registro-vivo/registro-vivo.json
**Learned**: Regla de método: antes de declarar pendiente en el registro vivo, contrastar contra el ÚLTIMO doc de cierre del expediente Y contra realidad ejecutable. Pendientes de Jorge tras reconciliación: retiros B-5 (desbloqueados), D-4 copias únicas, D-5 ZIP (CC ejecuta a la orden), revisión 1Password.txt, B-3 repos manuales, B-1 Drive-Codex.

### [CIERRE] Consolidación DFL v1 (9847568) — [CORREGIDO por obs #236: B-2 era falsa, D-1/D-2 ya resueltas, sin fecha dura PAT]
**Type:** decision  
**Project:** futbolweb-app  

**What**: Misión completada 2026-07-12 (CC EJECUTOR). Entregables:
1. PARIDAD CODEX=CC: **funcional con brechas menores** — Run A 10/10, Run B 3/4 (única falla: bridge MCP no acepta type=manual; ciclo Gate 4B sí funcionó). 14/17 superficies paridad completa. ÚNICA BLOQUEADA: Google Drive (Codex sin vía — decisión Jorge). Perfil `dfl-mission` en ~/.codex/config.toml (backup .bak-paridad): `codex exec -p dfl-mission` = autonomía CC sin Jorge-courier, smoke test PASS.
2. PERFIL CANÓNICO: 02-DFL-AGENT-ACCESS-PROFILE-v1.md — independiente del agente, con procedimiento de alta de agentes futuros (batería de paridad como plantilla).
3. REGISTRO VIVO CANÓNICO: governance/registro-vivo/{registro-vivo.json (21 órganos, 9 servicios, tmux), check_registro_vivo.py} — SUPERSEDE 06/09 del diagnóstico v1 y DO-INVENTORY 2026-05-10. Detecta: órgano nuevo/ausente, dirty real, sin-push, sin-respaldo, revisión vencida, servicio caído, divergencias, registro stale. SIN cron (GOBERNAR no autorizado). El run2 detectó EN VIVO trabajo concurrente en futbolweb (codex de Jorge, tournament-reality, ~19:39 UTC) — mecanismo validado.
4. CONSOLIDADO: H-03 CERRADO (dfl-context-proxy abde599 pusheado); dfl-knowledge 8 commits pusheados (e2265bf..9847568); digitalocean RESUELTO=infra-vendor; artefactos huérfanos versionados (health-v1, organismo-v1, MISION_A1 reubicado); todo pausado con dueño+fecha (n8n/ingest/painradar 2026-08-10, labs 2026-08-15, co-001 2027-01-15); nada retirado, producción intacta.
**Why**: Mandato Jorge — paridad operacional + casa consolidada antes de GOBERNAR.
**Where**: /opt/dfl-knowledge/audits/consolidacion-institucional-dfl-v1/ (00-07 + EVIDENCE 14 archivos), governance/registro-vivo/, ~/.codex/config.toml
**Learned**: BLOQUEOS PARA JORGE: B-1 Drive-Codex, B-2 receiver VM3 solo acepta /data/dfl-backups/engram/* (off-host bundles imposible desde La Garra), B-3 crear repos DFLghub sin PAT sano (bloquea nq-factory/engram-mcp/fork engram/V5), D-4 ~2.2GB copias únicas, retiros sf-test+prediccion2026. FECHA DURA: PAT expira 2026-09-13. Codex activo trabajando futbolweb en paralelo — coordinar antes de tocar futbolweb.

### Cierre condicionado ZIP antiguo — Drive inaccesible; respaldo nuevo íntegro
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/institutional-remediation/zip-residual-close
TYPE: decision
STATUS: active
DATE: 2026-07-11
SUMMARY: Se ejecutó el checkpoint parcial #225 y se intentó cerrar el único residual. Codex no tiene conector Google Drive ni OAuth/API local autorizada; por ello no fue posible localizar/eliminar el fileId 1eeYfC0o8hyP2z9QqfIYQVRFDcPJ8T29e, vaciar papelera ni verificar búsqueda posterior. No se borró el ZIP Drive ni la copia local, evitando un estado asimétrico.

EVIDENCIA: metadatos locales registrados (524 bytes, sha256 4c3dd829569cf799616f870431e77d35eef0cfc70539686038819c3e7f6b4192); Drive metadata previamente registrada owner-only, owner jtigre@gmail.com, created 2026-06-13. Nuevo env cifrado sha256 404f5a567bea73e941b53acf5e6833335e378761fee88908bf0f4881143a4120 y MANIFEST sha256 fc2e9c7d0f5c27b34a272713ea0eb3d053e39b454df7e1e0746f0ea4b2314a6e intactos; restore off-host repetido OK, sha_match=yes, 2 claves.

COMMIT: 05234cf (`docs(dfl): record zip residual closure gate`), 5 archivos, sin push. Expediente actualizado con 13-CIERRE-ZIP-ANTIGUO.md y evidencia b11-b13. Residual real único: ejecutar en sesión con Google Drive borrar solo ese fileId, vaciar papelera, verificar ausencia y recién después borrar la copia local equivalente.

### Cierre de residuales críticos Ola 1 — PAT/n8n/SaaS/off-host cerrados; ZIP Drive pendiente
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/institutional-remediation/wave-1-critical-residuals-close
TYPE: decision
STATUS: active
DATE: 2026-07-11
SUMMARY: Cierre acotado posterior a Ola 1 completado en commit local dfl-knowledge 0395a50 (`chore(dfl): close wave 1 critical residuals`), 9 archivos/132 inserciones, sin push de dfl-knowledge.

CERRADO: (1) PAT clásico revocado vía GitHub credentials/revoke (202) y autenticación posterior 401; transcript único saneado atómicamente, JSONL válido, cero holders por patrón completo en scopes solicitados/procesos, remote prediccion2026 SSH operativo. (2) n8n: sonda externa 5/5 timeouts a 67.205.166.199:5678; HTTPS/TLS/auth 401, contenedor/datos y DOCKER-USER+cron intactos. (3) SaaS Factory V5 publicada en repo privado DFLghub/saas-factory-setup, main remoto 5e42124aa0a070701f0a400b714d2a133b361a86; upstream comunitario trazable y push bloqueado. (4) co-001, nq-factory, env FutbolWeb y MANIFEST copiados a VM3 /data/dfl-backups/engram/organ-preservation/2026-07-11-wave1; hashes/tamaños 4/4 coinciden y restore env desde off-host verificado; sin passphrases off-host.

RESIDUAL REAL EXTERNO: ZIP antiguo FutbolWeb en Google Drive. Los tres gates técnicos cumplen, pero Codex no tiene conector Drive ni credencial API local; no se eliminó de Drive ni localmente y no se tocó otro archivo. Requiere sesión con Drive: borrar solo fileId 1eeYfC0o8hyP2z9QqfIYQVRFDcPJ8T29e y verificar ausencia en ubicación original y papelera accesible.

PROTECCIONES: no Ola 2, no diagnóstico nuevo, no cambios firewall adicionales, no publicación comunitaria, no push dfl-knowledge.

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

### Session summary — prueba real paridad Codex=CC y cierre 2026-07-12
**Type:** fact  
**Project:** dfl  

TOPIC: dfl/session/parity-close
STATUS: closed
DATE: 2026-07-12
MISIÓN: probar privilegios efectivos Codex en DFL sin inferencia.
RESULTADO: núcleo operacional PASS: /go, Engram, root La Garra, GitHub reversible, dfl-knowledge, KNL, agTopologo, Graphify, logs, Docker, systemd, cron, tmux y Gate 4B/mirror simulado. PARTIAL: SSH remoto; claves generales rechazadas, clave backup limitada por forced-command. BLOCKED: Google Drive sin conector/CLI/OAuth.
CORRECCIÓN: /opt/engram-mcp/server.py ahora usa session_id separado por proyecto; servicio reiniciado y escrituras dfl+futbolweb-app verificadas y archivadas.
EVIDENCIA: GitHub rama temporal creada/pusheada/verificada/eliminada; observaciones de prueba 235,237,238,240,241 archivadas; resultado institucional #242. No se tocaron NO_TOUCH ni producción.
CIERRE: @$fin modo CIERRE; ejecutar mirror y reportar salida real.

### Paridad operativa Codex probada 2026-07-12
**Type:** fact  
**Project:** dfl  

TOPIC: dfl/capability/codex-cc-parity
STATUS: verified-partial
Codex probó directamente: /go, Engram read-write-update-archive, root local, GitHub push reversible, dfl-knowledge write reversible, KNL, agTopologo check, Graphify query/diagnose, logs/Docker/systemd/cron, tmux y Gate4B+mirror simulado. Brechas reales: Google Drive sin conector/CLI/OAuth; SSH general a n8n/vault denegado, clave backup solo forced-command; sandbox Codex exige aprobaciones para loopback, /opt, systemd, Docker, tmux, SSH y red. Fix aplicado a /opt/engram-mcp/server.py: session_id separado por proyecto; servicio reiniciado y multiproyecto verificado con obs 240/241 archivadas. No se tocaron NO_TOUCH ni producción.

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

*Mirror auto-generated 2026-07-12T20:17:12Z | La Garra → DFLghub/amos-context*
