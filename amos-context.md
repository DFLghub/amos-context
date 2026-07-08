# amOS Context — @$go Live Mirror
**Generated:** 2026-07-08T23:52:17Z  
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

### @$go VALIDATION GATE obligatorio para todos los agentes
**Type:** decision  
**Project:** dfl  

**Qué**: Se implementó un gate compacto obligatorio posterior a `@$go` para evitar onboarding falso, contexto viejo o confusión de perfil. Nadie queda operativo solo por declarar perfil: debe responder SOURCE, PROFILE, ACCESS, FIN y NO_TOUCH.

**Diseño**: One-shot gate de máximo 6 líneas. Una corrección permitida; segundo fallo implica degradar a CONSULTOR o pedir EJECUTOR. No es una skill ni un loop infinito: es un handshake de protocolo.

**Cambios**:
- `/opt/dfl-context-proxy` commit `029a5a1` (`feat: require @$go validation gate`): agrega `protocol_update_alert` y `validation_gate` a `/go`, renderizado en `publish-amos-context.sh`, y test de contrato.
- `/opt/amos-context-mirror` commit `f8f225b` (`docs: add @$go validation gate`): agrega alerta de actualización y gate en `AGENT_CAPABILITY_MATRIX.md`.

**Evidencia**: `systemctl restart dfl-context-proxy` OK; `/go` muestra `protocol_update_alert` y `validation_gate`; `python3 tests/test_knl_contract.py` OK.

**No tocado**: cambio preexistente en `/opt/dfl-context-proxy/engram-backup-offhost.sh` quedó intacto.

### CIERRE — acceso uniforme DFL por contrato, no por transporte
**Type:** decision  
**Project:** dfl  

**Qué se hizo**: Se corrigió el onboarding/outboarding DFL para resolver el error conceptual: MCP no es el lobby uniforme, sino una puerta posible. `@$go` y `@$fin` quedan definidos como comandos uniformes por contrato semántico, con adaptadores por capacidad real de sesión.

**Arquitectura resultante**:
- EJECUTOR: shell/Engram/git; `@$go` vía `/go` + Engram; `@$fin` real con Gate 4B + `push_mirror.sh`.
- ORQUESTADOR: fetch público si disponible; cierre por bitácora de relay.
- CONSULTOR: snapshot pegado/memoria local; no intenta fetch bloqueado; cierre por `RESUMEN DE SESIÓN` para EJECUTOR.
- El perfil se decide por capacidades observables de la sesión, no por marca de agente. Codex puede ser EJECUTOR si tiene shell+Engram; ChatGPT web queda CONSULTOR mientras su allowlist bloquee fetch.

**Commits**:
- `/opt/amos-context-mirror` commit `a941ad2` (`docs: clarify DFL access adapters`) y mirror generado commit `a9f5cc28c94b170feb19b069942715697a910ff1`.
- `/opt/dfl-context-proxy` commit `fee934d8ae8dea19673adcfa402385a71408182d` (`feat: expose DFL access model`), pusheado a origin/main.

**Evidencia**:
- `systemctl restart dfl-context-proxy` OK.
- `curl -s http://127.0.0.1:8091/health` OK.
- `python3 tests/test_knl_contract.py` OK fuera del sandbox.
- GitHub raw `amos-context.md` contiene `ACCESS MODEL — UNIFORM CONTRACT, DIFFERENT TRANSPORTS`.

**Archivos afectados**:
- `/opt/amos-context-mirror/AGENT_CAPABILITY_MATRIX.md`
- `/opt/amos-context-mirror/agents/consultor.md`
- `/opt/dfl-context-proxy/main.py`
- `/opt/dfl-context-proxy/publish-amos-context.sh`
- `/opt/dfl-context-proxy/tests/test_knl_contract.py`

**No tocado**: `/opt/dfl-context-proxy/engram-backup-offhost.sh` seguía modificado antes de la tarea y quedó intacto.

**Type:** decision  
**Project:** 360eventos  

**Qué**: Jorge confirmó que ya envió el link de https://360eventos.vercel.app a Rubén, explícitamente para que "abra y mire el demo funcional (agMVP) — un producto mínimo viable PERO FUNCIONAL". Lo enmarca como "Prueba" (test), no como oferta comercial.

**Impacto en Price Authority**: esto NO es una confirmación de Nivel 0B (catálogo comercial oficialmente aprobado). Sigue pendiente. El catálogo de 8 servicios reales en Supabase sigue siendo Nivel 0A (real vivo, válido para demo), usado aquí explícitamente en modo demo/prueba para Rubén, consistente con la política definida en FOF_CASE_01_360EVENTOS_PRICE_AUTHORITY_AND_SERVICE_TIERS.md (ready_to_send demo, no comercial).

**Why**: Jorge quiere que quede claro que el criterio de éxito ahora mismo es "MVP funcional demostrable", no "catálogo comercial validado". No se requiere ninguna acción adicional de mi parte — es una actualización de estado del Caso 01, no una nueva misión.

**Type:** decision  
**Project:** 360eventos  

**Qué**: QA online del agMVP 360Eventos completado. URL pública validada: `https://360eventos.vercel.app` (encontrada manualmente por Jorge, proyecto Vercel `360eventos`). Documento: `/opt/dfl-knowledge/projects/fof/cases/360eventos/FOF_CASE_01_360EVENTOS_ONLINE_DEMO_QA.md` (commit 1a0fe6a).

**Resultado**: `/`, `/cotizar`, `/login` responden 200. `/cotizar` confirmado usando Nivel 0A (catálogo real Supabase, 8 servicios) — coincide exacto con la query directa hecha en la misión anterior: Producción general del evento, Decoración temática, Sonido profesional, Iluminación arquitectónica, Fotografía profesional, Video y transmisión en vivo, Transporte y montaje de equipos, Maestro de ceremonias. Formulario de cotización bien wireado a `submitCotizacion` (server action → insert en `solicitudes`), no se completó envío real para no escribir en DB de producción. Viewport responsive presente en las 3 páginas; home con 14 clases responsive Tailwind, cotizar con solo 2 (más simple pero funcional). Sin errores críticos visibles. No se aplicó ningún fix — no fue necesario.

**Limitaciones para Rubén**: es demo funcional no comercial (Nivel 0B pendiente), cualquier envío real de /cotizar genera fila real en `solicitudes` de producción (no hay modo sandbox), no se validó /dashboard ni login con credenciales reales, no se hizo validación visual real en dispositivo móvil (solo estructural).

**No se tocó**: DB (solo lecturas HTTP públicas), FutbolWeb, secrets, migraciones, seed. Cambios preexistentes ajenos en graphify-out/ag_topologo.py siguen intactos.

### Commits: graphify-out state + 360eventos env/scripts
**Type:** decision  
**Project:** futbolweb-app  

**What**: Dos commits pusheados a pedido explícito de Jorge, siguiendo hallazgos de la auditoría de backup (drift sin commitear detectado).
1. `dfl-knowledge` repo, commit af61702: "chore(graphify): post-regen state v0.3 stable" — 4 archivos (.last_full_regen, .summary_graph_hash, GRAPH_SUMMARY.md, ag_topologo_alert.json). Quedaron OTROS archivos modificados sin commitear (graph.json, graph.json.prev deleted, graph_comparator.json, graph_context.json, graph_context_light.json, knl.json, scripts/ag_topologo.py) — no se tocaron porque no estaban en el pedido explícito.
2. `360eventos` repo, commit b8a3553: "chore: environment config + seed scripts" — next-env.d.ts + scripts/create-admin.js + scripts/seed-servicios.js (nuevos). Verificado antes de commitear que ambos scripts leen credenciales de `.env.local` en runtime, sin secrets hardcodeados.

**Why**: Jorge pidió explícitamente estos commits tras ver el reporte de auditoría que identificó working trees sucios como gap de protección.

**Where**: /opt/dfl-knowledge, /opt/360eventos

### CIERRE FutbolWeb P0 post-fix 2026-07-05 — knockout ESPN sync verificado hasta monitoreo 91 pre-kickoff
**Type:** decision  
**Project:** futbolweb-app  

Cierre de sesión FutbolWeb P0. Incidente investigado y corregido: ESPN cambió IDs de eventos para knockout posterior y los fixtures locales 89+ usan placeholders sin team codes; el matcher anterior dependía de fifaId o fecha+teamCode fijo, por eso no importó 89/90, no creó match_results, no corrió scoring, ranking no sumó y bracket 97 seguía con W89/W90. Fix commit b1b6d60 en futbolweb-app: matcher ESPN aplica bracket assignments con match_results previos y resuelve nombres/equipos antes de fallback por fecha/equipos; sync route carga existingResults antes de leer ESPN. Datos prod actualizados: 89 Paraguay 0-1 Francia y 90 Canada 0-3 Marruecos; scoring knockout ejecutado; prediction_scores 89=2 rows, 90=1 row; ranking Edgar Alberto P80 = 137.5; /api/tournament-reality muestra 97 Francia vs Marruecos; propagation pending []. Verificación post-fix para partido 91: al momento del monitoreo era pre-kickoff (generatedAt 2026-07-05T05:35Z, kickoff 2026-07-05T20:00Z); ESPN aún no incluye 91; match_results/scores 91 vacíos correcto; accepted predictions 91 existen para Alejo y Edgar Alberto. Próximo punto de monitoreo: después de FT de Brasil vs Noruega, confirmar ESPN import -> match_results 91 -> 2 scores -> ranking -> W91 en partido 99.

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

---

## PENDING

### @go Protocol v1.0 — protocolo de arranque DFL
**Project:** dfl  

Protocolo @go desplegado el 2026-06-24. Permite a cualquier IA (Claude, ChatGPT, Gemini) arrancar con contexto completo DFL en un paso. Fuente A: amos-context.md en DFLghub/amos-context (commit edd5f51). Fuente B: GET https://context.deepfeelingslabs.com/go — devuelve JSON con identity, recent_decisions, active_constraints, pending, generated_at. Backend: dfl-context-proxy en 127.0.0.1:8091 consulta Engram local 127.0.0.1:7437 con queries: "decisiones activas estado", "restricciones prohibido no tocar", "pendientes criticos". Sin auth requerida en /go. @go v1.0 activo y verificado.

### Gate Engine v0 Caso 01: Gate 1 es el gate más débil (PRP-001 retroactivo)
**Project:** dfl  

Evaluación retroactiva del PRP-001 contra Gate Engine v0 checklist (2026-06-21). Gate 2 (Execution Perimeter): PASS limpio — perímetro declarado con claridad inusual desde el diseño. Gate 4 (Closure Integrity): PASS — 859/859 harness, diff cero, pendientes explícitos. Gate 1 (Decision Resurrection): ESCALATE — el Candidate Vault NO fue consultado en ningún momento del PRP-001; TSL sirvió como validación de riesgo pero no como chequeo de decisiones archivadas. Hallazgo clave: el checklist necesita una tercera categoría de respuesta (NOT_VERIFIABLE / PARTIAL) además de SÍ/NO — se improvisó durante la evaluación. Gate 4 es el más automatizable (diff/harness son verificables mecánicamente). Gate 1 requiere lectura semántica humana (distinguir 'consulta a TSL' de 'consulta al Candidate Vault'). Conclusión operacional: antes de ejecutar cualquier PRP, consultar 04_Candidate_Vault/ activamente — no solo a agentes. Este hallazgo viene de Gate_Engine_Caso01_PRP001.md en audited_pass/.

### Session summary: futbolweb-app
**Project:** futbolweb-app  

## Goal
Resolver el onboarding incompleto del @go DFL para que cualquier agente nuevo llegue listo sin preguntas.

## Instructions
Mínimos tokens, máximo resultado. Solo bloqueadores y estado final al usuario.

## Discoveries
- El hook SessionStart en settings.json inyecta stdout del script como contexto — misma mecánica que el plugin Engram. No requiere formato especial, markdown plano funciona.
- PATCH /observations/{id} en Engram API (7437) acepta campos parciales — no hace falta reenviar todo el objeto.
- El filtro LIFECYCLE en /go debe parsear el contenido de texto de la obs línea por línea — no hay campo DB dedicado para lifecycle.
- OBS-005 tenía LIFECYCLE: active pero el incidente estaba resuelto — estaba contaminando el namespace de active_constraints.
- CLAUDE.md acepta `@/ruta/absoluta` para importar archivos externos al contexto CC.

## Accomplished
- ✅ OBS-005 archivada — LIFECYCLE: active → archived via PATCH Engram ID 64
- ✅ Proxy /go: filtro LIFECYCLE: archived (método _is_archived añadido a main.py)
- ✅ Campo cc_bootstrap en /go response (6 claves: step_1-3, precedencia, protegido, alerta)
- ✅ Hook SessionStart creado: /opt/dfl-context-proxy/cc-atgo-hook.sh
- ✅ settings.json actualizado con hook SessionStart → cc-atgo-hook.sh (timeout 10s)
- ✅ CLAUDE.md futbolweb: sección 11 (DFL Bootstrap) + @import DFL_Agent_Onboarding_Config.md
- ✅ dfl-context-proxy reiniciado y verificado activo
- ✅ Hook testeado: output completo con decisiones, constraints, pendientes, CC Bootstrap

## Next Steps
- Verificar que el hook aparece en la próxima sesión CC como sistema mensaje al inicio
- Considerar crear /opt/dfl-knowledge/CLAUDE.md para agentes que abran CC desde ese directorio
- OBS-10 (inventario servicios) tiene DNS pendiente como "aún no verificados" — actualizar a verified (DNS ya está activo)

## Relevant Files
- /opt/dfl-context-proxy/main.py — proxy @go: LIFECYCLE filter + cc_bootstrap field
- /opt/dfl-context-proxy/cc-atgo-hook.sh — nuevo hook SessionStart CC
- /root/.claude/settings.json — hook SessionStart registrado
- /opt/futbolweb/CLAUDE.md — sección 11 + @import onboarding config

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

### CIERRE — @$go VALIDATION GATE publicado y auditoría de mirror ajustada
**Type:** bugfix  
**Project:** dfl  

**Qué se hizo**: Se publicó el `@$go VALIDATION GATE` obligatorio para todos los agentes, junto con una `PROTOCOL UPDATE ALERT` visible al inicio del mirror público. El gate exige 5 líneas: SOURCE, PROFILE, ACCESS, FIN y NO_TOUCH. Una corrección permitida; segundo fallo degrada a CONSULTOR o pide EJECUTOR.

**Commits**:
- `/opt/amos-context-mirror` commit `f8f225b` (`docs: add @$go validation gate`) — agrega alerta y gate en `AGENT_CAPABILITY_MATRIX.md`.
- `/opt/dfl-context-proxy` commit `029a5a1` (`feat: require @$go validation gate`) — agrega `protocol_update_alert` y `validation_gate` a `/go`, renderizado en `amos-context.md`, y test de contrato.
- `/opt/dfl-context-proxy` commit `607f1af` (`fix: allow protected path mention in mirror`) — permite mencionar `/etc/dfl-secrets` como superficie protegida sin bloquear la publicación como secreto.

**Evidencia**:
- `systemctl restart dfl-context-proxy` OK.
- `/go` muestra `protocol_update_alert` y `validation_gate`.
- `python3 tests/test_knl_contract.py` OK.
- GitHub raw `amos-context.md` generado `2026-07-08T23:51:49Z` contiene `PROTOCOL UPDATE ALERT` y `## @$go VALIDATION GATE`.
- `push_mirror.sh` imprimió: `MIRROR: updated | commit 569d9c91d266d352d28070be79ee05764b7c6956 | 2026-07-08 23:51:50 +0000`.

**No tocado**: `/opt/dfl-context-proxy/engram-backup-offhost.sh` sigue modificado desde antes y quedó intacto.

### @$go VALIDATION GATE obligatorio para todos los agentes
**Type:** decision  
**Project:** dfl  

**Qué**: Se implementó un gate compacto obligatorio posterior a `@$go` para evitar onboarding falso, contexto viejo o confusión de perfil. Nadie queda operativo solo por declarar perfil: debe responder SOURCE, PROFILE, ACCESS, FIN y NO_TOUCH.

**Diseño**: One-shot gate de máximo 6 líneas. Una corrección permitida; segundo fallo implica degradar a CONSULTOR o pedir EJECUTOR. No es una skill ni un loop infinito: es un handshake de protocolo.

**Cambios**:
- `/opt/dfl-context-proxy` commit `029a5a1` (`feat: require @$go validation gate`): agrega `protocol_update_alert` y `validation_gate` a `/go`, renderizado en `publish-amos-context.sh`, y test de contrato.
- `/opt/amos-context-mirror` commit `f8f225b` (`docs: add @$go validation gate`): agrega alerta de actualización y gate en `AGENT_CAPABILITY_MATRIX.md`.

**Evidencia**: `systemctl restart dfl-context-proxy` OK; `/go` muestra `protocol_update_alert` y `validation_gate`; `python3 tests/test_knl_contract.py` OK.

**No tocado**: cambio preexistente en `/opt/dfl-context-proxy/engram-backup-offhost.sh` quedó intacto.

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

**Graph entropy:** 1.3944  

- **Community 11** (75 nodes): Matriz de Contención Operacional, Activo, Contextual Flow
- **Community 0** (10 nodes): agLego-PTE-001, agLego, ag
- **Community 1** (8 nodes): MERCADER BOS, Regla semántica del prefijo AG, Asset Index
- **Community 3** (7 nodes): NBLM2, FutbolWeb, La Fábrica CF
- **Community 2** (7 nodes): amOS, IAIM, @$go
- **Community 4** (7 nodes): Fase 0 — Fundación, Leads

---

*Mirror auto-generated 2026-07-08T23:52:17Z | La Garra → DFLghub/amos-context*
