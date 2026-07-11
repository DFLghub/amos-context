# amOS Context — @$go Live Mirror
**Generated:** 2026-07-11T23:37:01Z  
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

### Bloque ACLARAR completado (commit 42b9da6): A-1 PAT crítico, A-6 zip cifrado, A-4 metabolismo real, V-3 n8n auth+expuesto — 5 decisiones para Jorge
**Type:** decision  
**Project:** futbolweb-app  

Bloque ACLARAR del Diagnóstico Institucional DFL v1 completado y commiteado local: 42b9da6b38e7ce218601e5e60e233c613866a0b0 (2026-07-11 22:55 UTC, dfl-knowledge main ahead 2, SIN push). Solo verificación, cero remediación. Artefactos: 05-ACLARAR-VERIFICACIONES, 06-REGISTRO-VIVO-DE-ORGANOS-OPT, 07-DECISIONES-PENDIENTES + 6 evidencias.

RESULTADOS:
- A-1 PAT prediccion2026: VIGENTE Y CRÍTICO. Verificado vía api.github.com (token nunca impreso). Scope=repo (write total a TODOS los repos privados de la cuenta DFLghub: futbolweb-app, dfl-knowledge, dfl-context-proxy, 360eventos, amos-context, prediccion2026). Expira 2026-09-13. admin+push confirmado en prediccion2026. NO rotado (mandato).
- A-6 futbolweb-env-backup.zip: CIFRADO ZipCrypto (mitigante), 1 archivo .env.local (300B, de Mac macgopro, 2026-05-25), duplicado en Drive, 0644 root. Nombres de clave/valores NO legibles sin password. NO_VERIFICABLE con indicio de secreto de prod. Original intacto.
- A-4 metabolismo: dry-run es flag manual ($1) intencional; cron 0 3 * * 0 corre REAL; métrica SÍ persiste (1 OBS-METRIC SYSTEM_HEALTH_CHECK en Engram dfl). '100%'=liveness de 6 chequeos de infra memoria/navegabilidad, NO salud institucional. Cron dominical aún sin primera ocurrencia (script creado 2026-07-09, próx domingo 07-13).
- V-3 n8n: auth FORZADA (401 en workflows/executions/credentials/login, local Y público; email auth, setup completo). PERO ufw abre 5678/tcp + 3000 + 3010 a Anywhere → n8n alcanzable directo por HTTP sin TLS; sin consumidor desde 2026-05-17.

DECISIONES PARA JORGE (5): D-1 autorizar rotación PAT+limpieza remote; D-2 destino soberano V5 (DFLghub vs community — origin actual es org externa saas-factory-community, 5e42124 inédito local, push reflejo filtraría IP); D-3 futuro n8n (Nivel 2 con dueño+fecha o retirar); D-4 ratificar registro /opt y dar remote/backup a co-001+nq-factory y versionar ~2.4GB sin git (futbolweb-v2 807M, mercader-comisiones 790M, roof-issues-mini 595M, mercader); D-5 verificar/decidir env zip.

RIESGOS ACTIVOS: R-1 PAT crítico, R-2 push V5 externo, R-3 2.4GB copia única, R-4 exposición directa 5678/3000/3010, R-5 env zip ZipCrypto en Drive, R-6 durabilidad Engram sin verificar (heredado), R-7 engram-mcp+ingest sin git.

PRÓXIMO: revisar con Jorge y decidir; NO remediar aún. Encadena [[cr-tico-h-01-pat-github...]] y [[expediente-diagn-stico-v1-commiteado-local-e2265bf...]].

### Expediente diagnóstico v1 commiteado local (e2265bf) tras revisión de secretos limpia — próximo: bloque ACLARAR
**Type:** decision  
**Project:** futbolweb-app  

Commit local del expediente Diagnóstico Institucional DFL v1 en dfl-knowledge: e2265bf5857fa8865859b2df4cc7d23327d2839e (2026-07-11 22:41 UTC, main, ahead 1 de origin, SIN push por mandato). 23 archivos, 932 líneas: 5 artefactos + 19 evidencias (incl. 2 nuevas: git-remotes-redactado.txt, git-dirty-detalle.txt). Revisión de exposición de secretos PREVIA al commit: limpia — sin PATs completos, tokens, claves privadas, contraseñas ni URLs autenticadas; verificado por comparación booleana contra valores reales de /etc/dfl-secrets, DFL_TOKEN, token cloud.json y el PAT real de prediccion2026 (valores jamás impresos). Dos falsos positivos descartados: ENGRAM_CLOUD_SERVER=endpoint local 127.0.0.1:8090 y ENGRAM_DATA_DIR=/root/.engram son config no sensible dentro de dfl-secrets. Esto cierra C-2/H-13 SOLO para este expediente; MISION_A1.md, audits/health-v1/ y audits/organismo-v1/ siguen untracked deliberadamente (no mezclar). PRÓXIMO PASO acordado con Jorge: revisar el diagnóstico y decidir el bloque ACLARAR (A-1..A-6 de 04-DIAGNOSTICO-INSTITUCIONAL.md) — no corregir aún los 18 hallazgos.

### SaaS Factory V5 consolidada en commit local 5e42124
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/saas-factory/v5-local-commit
TYPE: decision
STATUS: active
DATE: 2026-07-11
SUMMARY: Con autorización explícita de Jorge, se consolidó la capa operativa SaaS Factory V5 existente sobre la base Git V4 del repositorio /opt/saas-factory-setup. Commit local: 5e42124aa0a070701f0a400b714d2a133b361a86, mensaje `feat: establish SaaS Factory V5 operational layer`, rama main, base previa 99f51b3. Alcance: 67 archivos, incluyendo CHANGELOG 5.0.0, CLAUDE/GEMINI y nuevos skills V5 con referencias. Validación: 32 SKILL.md físicos con frontmatter mínimo válido; escaneo de patrones conocidos sin secretos; commit verificado. Exclusión intencional: saas-factory/graphify-out/ permanece untracked por ser salida diagnóstica generada. No se hizo push.
PROXIMO_AGENTE_DEBE: antes de publicar, revisar remoto/destino y solicitar o confirmar autorización explícita de push; no incluir graphify-out salvo orden específica.

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

### AgMaster_amOS_3 — vocabulario y reglas IAIM
**Type:** fact  
**Project:** dfl  

AgMaster_amOS_3 es el documento maestro v3 del ecosistema (USAR ESTA, versiones 1 y 2 obsoletas). Vocabulario mínimo para IA invitada: amOS=sistema operativo conceptual/metodológico para absorber/metabolizar información manteniendo soberanía; IAIM=Invisible Augmented Intelligence Mesh, red invisible de IAs aliadas sin nodos fijos, orquestada por HI; HI=Jorge, decisión final soberana; ag10=ChatGPT como router/integrador/destilador/última yarda (no oráculo ni jefe); agPregunta=pregunta aumentada con propósito/dominio/límite/criterio; agLego=pieza conceptual candidata modular trazable; Candado Soberano=restricciones no negociables: no-exec, candidate_only, human-review-first. Prefijo 'ag'=augmented+governed+generative-but-contained. AG10-AUSTERITY-LOCK para fases de cierre/patch/gate. Origin Chain obligatorio para todo agLego. Frase núcleo v3: 'La red ilumina. La HI orquesta. ag10 destila. El candado audita. amOS asimila la cicatriz, no la herida.' Paralelo permitido: máximo 2 nodos, candidate_only, revisión HI.

### agLego PATTERN-ASYNC_INSPECTION_SPLIT: patrón de inspección asíncrona
**Type:** fact  
**Project:** dfl  

Origen: mined by Gemini/Soberana+Triunvirato v2.5.3.3, packaged by ChatGPT/ag10, audited PASS 2026-05-09. HI approval: PENDING. Estado: candidate_only/sealed en raíz de Candidate Vault. Qué extrae: bifurcación entre payload pesado y metadata ligera; asociación obligatoria por inspection_id/correlation_id; doble proyección (alerta inmediata + histórico operacional); confirmación de evidencia antes de liberar notificación o métrica. State-Gate: alerta no se libera solo porque llegó el formulario — metadata queda en pending_evidence hasta que payload pesado confirma status:committed con mismo correlation_id. Si evidencia no llega: orphaned_alert + corrección en campo. Riesgos preservados: estado fantasma, alerta sin evidencia, métrica sin respaldo, fatiga de notificaciones, dependencia de rol. Próxima pregunta segura: cómo valida amOS que evidencia+alerta+métrica comparten el mismo correlation_id antes de proyección. NO debe ejecutarse, ir a producción ni conectarse a sistemas reales hasta HI approval. Archivo: 04_Candidate_Vault/agLego-PATTERN-ASYNC_INSPECTION_SPLIT.md.

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

### Bloque ACLARAR completado (commit 42b9da6): A-1 PAT crítico, A-6 zip cifrado, A-4 metabolismo real, V-3 n8n auth+expuesto — 5 decisiones para Jorge
**Type:** decision  
**Project:** futbolweb-app  

Bloque ACLARAR del Diagnóstico Institucional DFL v1 completado y commiteado local: 42b9da6b38e7ce218601e5e60e233c613866a0b0 (2026-07-11 22:55 UTC, dfl-knowledge main ahead 2, SIN push). Solo verificación, cero remediación. Artefactos: 05-ACLARAR-VERIFICACIONES, 06-REGISTRO-VIVO-DE-ORGANOS-OPT, 07-DECISIONES-PENDIENTES + 6 evidencias.

RESULTADOS:
- A-1 PAT prediccion2026: VIGENTE Y CRÍTICO. Verificado vía api.github.com (token nunca impreso). Scope=repo (write total a TODOS los repos privados de la cuenta DFLghub: futbolweb-app, dfl-knowledge, dfl-context-proxy, 360eventos, amos-context, prediccion2026). Expira 2026-09-13. admin+push confirmado en prediccion2026. NO rotado (mandato).
- A-6 futbolweb-env-backup.zip: CIFRADO ZipCrypto (mitigante), 1 archivo .env.local (300B, de Mac macgopro, 2026-05-25), duplicado en Drive, 0644 root. Nombres de clave/valores NO legibles sin password. NO_VERIFICABLE con indicio de secreto de prod. Original intacto.
- A-4 metabolismo: dry-run es flag manual ($1) intencional; cron 0 3 * * 0 corre REAL; métrica SÍ persiste (1 OBS-METRIC SYSTEM_HEALTH_CHECK en Engram dfl). '100%'=liveness de 6 chequeos de infra memoria/navegabilidad, NO salud institucional. Cron dominical aún sin primera ocurrencia (script creado 2026-07-09, próx domingo 07-13).
- V-3 n8n: auth FORZADA (401 en workflows/executions/credentials/login, local Y público; email auth, setup completo). PERO ufw abre 5678/tcp + 3000 + 3010 a Anywhere → n8n alcanzable directo por HTTP sin TLS; sin consumidor desde 2026-05-17.

DECISIONES PARA JORGE (5): D-1 autorizar rotación PAT+limpieza remote; D-2 destino soberano V5 (DFLghub vs community — origin actual es org externa saas-factory-community, 5e42124 inédito local, push reflejo filtraría IP); D-3 futuro n8n (Nivel 2 con dueño+fecha o retirar); D-4 ratificar registro /opt y dar remote/backup a co-001+nq-factory y versionar ~2.4GB sin git (futbolweb-v2 807M, mercader-comisiones 790M, roof-issues-mini 595M, mercader); D-5 verificar/decidir env zip.

RIESGOS ACTIVOS: R-1 PAT crítico, R-2 push V5 externo, R-3 2.4GB copia única, R-4 exposición directa 5678/3000/3010, R-5 env zip ZipCrypto en Drive, R-6 durabilidad Engram sin verificar (heredado), R-7 engram-mcp+ingest sin git.

PRÓXIMO: revisar con Jorge y decidir; NO remediar aún. Encadena [[cr-tico-h-01-pat-github...]] y [[expediente-diagn-stico-v1-commiteado-local-e2265bf...]].

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

**Graph entropy:** 1.1206  

- **Community 11** (88 nodes): Engram, Activo en amOS, agLego-PATTERN-ASYNC_INSPECTION_SPLIT
- **Community 0** (9 nodes): IAIM, agLego, Candado Soberano
- **Community 1** (6 nodes): ROI_REALITY_SCORE, AG, Circuito de recepción de pronósticos
- **Community 2** (5 nodes): KNL v1.0, Viabilidad Económica, Jerarquía Correcta
- **Community 3** (4 nodes): Engram Cloud, Arquitectura Multi-Servicio, Business OS
- **Community 4** (4 nodes): Materia Prima Metabólica, Convención de Escritura DFL, Incidente de Seguridad

---

*Mirror auto-generated 2026-07-11T23:37:01Z | La Garra → DFLghub/amos-context*
