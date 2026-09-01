# amOS Context — @$go Live Mirror
**Generated:** 2026-09-01T04:05:06Z  
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

### P11 — DFL Cybernetic Rewiring L1-L8 + Health-of-the-Loop: sesión completa cerrada (2026-09-01)
**Type:** decision  
**Project:** dfl  

TCC, sesión 2026-09-01. Misión: cartografía AS-IS de lazos de control DFL (R→S→C→A→Retorno), auditoría adversarial de TCX, luego loop autónomo de rewiring L1-L8, luego primitive #8 (health-of-the-loop). Artefacto canónico único: docs/BASELINE-CERO-AS-IS-2026-09-01.md (repo saas-factory). Resumen nocturno completo y todas las entradas cronológicas en IRONMAN.md del mismo repo, buscar "P11 — LOOP AUTÓNOMO" y "P11 — Health-of-the-loop CERRADO".

INCIDENTE FUNDACIONAL (previo al loop, real, reparado en vivo): auto-renovación de dispatch freshness (authorization_renewal.py, cron 10min via activate-peer.sh) fallaba 100% desde 2026-08-19 por permisos root:root en renewal-policy.json, ilegible para dflagent. TCX bloqueado por E_DISPATCH_STALE hasta que Jorge (root) corrió el fix: chown root:dfl + chmod 640 + process manual. Verificado post-fix: /go real PASS. Bug menor NO reparado (fuera de alcance autorizado): dispatch_gate.py:207-210 lee mission.authorization_lifecycle (campo inexistente) en vez de dispatch[0].authorization.lifecycle, cae siempre a default 24h en vez de 30 días reales.

ESTADO FINAL POR TRAMO (todos con evidencia real, ningún FORMAL sin retorno probado):
- L1 (DCSA autorización): INCOMPLETO general -> FORMAL vía E2E real autorizado por Jorge: lead sintético POST a mercader-bos real (127.0.0.1:9099/api/mercader/leads) -> cron TCX automático claim/produce/AQA/deliver/ACK en ~3min sin intervención manual -> mercader_leads.order_status=ACKED confirmado en SQLite directo. Watchdog L1b (check_mission_progress.py) re-midió en vivo y pasó SILENT->OK.
- L2 (peer-work ACK/NACK): sub-tramo L2b FORMAL. Bug real en tools/telegram-bos/bot.mjs: marcaba delivered_to_telegram sin revisar el campo ok de la respuesta de Telegram. Fix + delivery-outcome.mjs (primitive pura extraída) + 7 tests aislados.
- L3 (EXTERIOR->MERCADER->FABRICA): INCOMPLETO -> FORMAL con salvedad, reclasificado por la misma evidencia del E2E de L1 (regla "si la evidencia nueva contradice otro L, corrígelo también").
- L4 (AQA orchestrator): sub-tramo FORMAL. cmdOrchestrate en tools/aqa-kit/bin/aqa.mjs no persistía GLOBAL_STATE (solo stdout) — ahora usa writeEvidenceRecord existente. Nueva primitive tools/aqa-kit/lib/re-aqa-silence.mjs envuelve metaAudit() sin tocarlo. Cron diario, budget 7 días (no 24h, para no generar ruido sobre gaps ya conocidos como JPI AQA-0 FAIL desde 2026-08-30).
- L5 (JPI reservas): sub-tramo FORMAL. Gap real: jpi_reservations.status nunca transiciona de PENDING_DEPOSIT en NINGÚN código del repo — verificado por grep exhaustivo. Watchdog reservation-watchdog.mjs en /opt/jpi, filtro SQL fuente_datos=sintetico (mismo límite de seguridad que sweep.sh ya usa). STOP respetado: login Admin JPI requiere credenciales Vercel Production inexistentes, no tocado.
- L6 (Realtor WhatsApp delivery): sub-tramo FORMAL EN REPO, NO DESPLEGADO A PRODUCCIÓN. repository.js ganó updateWhatsAppOutboundStatus (ambas clases, Local y Neon) simétrico a updateWhatsAppInbound. whatsapp-webhook.js reconcilia SENT->DELIVERED->READ real. 9/9 tests incl. 3 nuevos + suite existente sin regresión. Deploy real requiere autorización separada, explícitamente no hecho.
- L7 (Website Manager sweep): INCOMPLETO -> FORMAL. sweep.mjs existía completo (SLA-breach + heartbeat) pero sin cron. Wrapper cron-sweep.sh instalado. INCIDENTE REAL #4: validar el wrapper corrió el sweep() real contra producción con token real, 3 alertas reales enviadas — pero contenido correcto (3 items genuinamente descuidados desde 2026-08-31, nunca notificados antes).
- L8 (Engram memoria institucional): sub-tramo FORMAL. curl http://127.0.0.1:7437/conflicts?status=pending (API real, no CLI local) reveló 3 conflictos pendientes, los 3 creados 2026-07-25T02:22:38Z, 38 días sin juzgar. Watchdog tools/engram-watchdog/check_pending_conflicts.py reusa tools/lib/silence_watchdog.py (misma primitive de L1b). Cron 6h instalado sin disparar notify manualmente — el primer tick real mandará 3 alertas legítimas a Jorge.
- Primitive #8 (health-of-the-loop, decisión por ROI de Jorge, construida el mismo día que se nombró): tools/lib/loop_health_watchdog.py vigila frescura de los 5 receipts/logs de los watchdogs anteriores, cron 15min. Confirmó que los crons de L1b/L4/L7 ya corrían solos correctamente. No vigila su propia salud — límite razonable de la recursión, nombrado explícitamente.

CINCO INCIDENTES REALES AUTORREPORTADOS (ninguno encontrado por tercero): (1) alerta Telegram con silence_seconds mal calculado, sensor no leía IRONMAN.md todavía; (2) alerta Telegram con fecha simulada 2026-09-05 filtrada de un test de Retorno mal aislado — motivó la regla institucional de testing (sección 8 del baseline: ninguna primitive nueva se prueba contra store_dir/tokens/canales reales salvo E2E final autorizado); (3) referencia rota transitoria ~1s en bot.mjs durante edición multi-paso, autosanada; (4) sweep.mjs real disparado sin dry-check, 3 alertas reales pero contenido correcto — motivó práctica "dry-check-then-wire" aplicada desde entonces sin excepción (L5, L8, primitive #8 todos dry-chequeados antes de instalar cron).

PRIMITIVES P4 REUSABLES REALES (no diseño especulativo, consumidores reales): tools/lib/silence_watchdog.py (Python, 3 consumidores: L1b/L5/L8), tools/aqa-kit/lib/re-aqa-silence.mjs (JS, envuelve metaAudit), tools/telegram-bos/delivery-outcome.mjs (patrón ok:boolean), tools/lib/loop_health_watchdog.py (meta-nivel).

FRAMEWORK ESTRATÉGICO (Jorge, afinado 2026-09-01, guardado también en .claude/memory/dfl-cybernetic-rewiring-framework.md del repo saas-factory): P1=jerarquía de madurez flujo->lazo->convergencia demostrada (no dos piezas intercambiables). P2=escalar el mismo principio de control de máquina a negocio. P3/BOS=grafos conectados por contratos+memoria+herramientas+jobs+autoridad. P4=primitives acumulativas, no lista de componentes. 8 primitives transversales: (1) jerarquía de madurez, (2) sensor mide el mundo no autopercepción, (3) autoridad=ley del sistema, (4) maduración humano->agente->script, (5) contratos entre grafos, (6) P4 como compilación progresiva, (7) testing isolation, (8) health-of-the-loop.

GAPS EXPLÍCITAMENTE ABIERTOS, NO INFLADOS: login Admin JPI + concurrencia real (credenciales inexistentes); deploy Realtor a producción (autorización separada pendiente); actuador de confirmación de depósito JPI (decisión de negocio no infraestructura); juicio automático de conflictos Engram (acto cognitivo, no automatizado); bug de campo mal nombrado en dispatch_gate.py; auditoría adversarial de TCX sobre todo este cierre, pendiente de autorización de Jorge.

PRÓXIMO PASO EXPLÍCITO PARA QUIEN RETOME: 1) Confirmar en IRONMAN.md si hay entradas posteriores a esta (buscar fecha >2026-09-01 o "L9"). 2) Si Jorge pide seguir, la auditoría adversarial de TCX sobre L1-L8 + primitive #8 es el trabajo pendiente más directo. 3) Ningún STOP duro se disparó durante toda la misión — los límites de autoridad reales (JPI Admin, Realtor deploy) se respetaron sin rodeos, no requieren re-visitarse salvo que Jorge cambie esa decisión explícitamente.

### DFL BIG PICTURE P1-P4 y primitives de rewiring
**Type:** decision  
**Project:** dfl  

Marco rector confirmado por Jorge el 2026-09-01 para los próximos días hasta alcanzar el quiero mayor de cada área. P1 es jerarquía de madurez: flujo -> lazo -> convergencia demostrada. El grafo organiza pero no garantiza el goal; un lazo requiere R->S->C->A->Retorno y convergencia exige sensor del mundo real/observable y re-medición después de actuar. P2 escala el principio de control de máquina autónoma a negocio completo, no solo más automatización. P3/BOS conecta grafos especializados por contratos explícitos: qué se entrega, estado, evidencia y condición habilitante, además de conocimiento, memoria, herramientas, jobs y autoridad; el dueño gobierna mapa y referencias, no rutina. P4 es compilación progresiva de criterio humano a infraestructura ejecutable mediante primitives reutilizables y acumulativas. Dirección DFL: automatismo operativo creciente, humano soberano como decisor/gobernante, nunca cuello de botella rutinario. Reglas transversales: sensor no puede ser autopercepción/PASS/log/estado declarado; autoridad y NO_TOUCH son ley del sistema; maduración humano->agente->script cuando el juicio se estabiliza; testing isolation antes del E2E real; health-of-the-loop debe demostrar que el propio control sigue vivo. El rewiring L1-L8 debe generar primitives reutilizables, no fixes aislados ni reducción de organismos/headcount. Fuente AS-IS: docs/BASELINE-CERO-AS-IS-2026-09-01.md; memoria local: .claude/memory/dfl-cybernetic-rewiring-framework.md.

### Paseo TCX Full Access profile configured and verified
**Type:** decision  
**Project:** saas-factory-setup  

Cierre @$fin. En VM2/PASEO_HOME=/home/dflagent/.paseo-sfv5-dev se configuró el agent profile dedicado de Paseo id=tcx-full-access, name='TCX — Full Access', provider=codex, model=gpt-5.5, modeId=full-access. La fuente instalada de Paseo 0.5.0-beta.5 confirma que full-access materializa approval_policy=never y sandbox_mode=danger-full-access. `paseo reload --format json` aplicó daemon.agentProfiles sin restartRequiredPaths. Verificación directa vía API local confirmó el perfil y los modos Codex (auto, auto-review, full-access). No se lanzó sesión desde Pixel, no se modificaron credenciales ni el provider global Codex. Próximo paso para Jorge: cerrar/reabrir Paseo en Pixel, abrir saas-factory y seleccionar TCX — Full Access; no seleccionar Codex genérico.

### Cierre de sesión 2026-09-01 — baseline cero auditado y marco P1-P4 persistido
**Type:** fact  
**Project:** dfl  

Cierre de sesión 2026-09-01. Se revalidó /go con decisión PASS para TCX y alcance en saas-factory, JPI y whatsapp-realtor-mvp. Se creó docs/BASELINE-CERO-AS-IS-2026-09-01.md como único baseline AS-IS dentro del alcance autorizado. Auditoría adversarial del mismo artefacto: L1 bajó a INCOMPLETO (gate formal), L2 quedó FORMAL solo para lifecycle interno, L4 bajó a INCOMPLETO (RE_AQA_REQUIRED prescribe otro run), L6 documentó falta de reconciliación outbound/status, L7 bajó a INCOMPLETO por scheduler recurrente no demostrado y L8 bajó a AUSENTE como lazo de control. Se añadieron F-01..F-08 con evidencia. Se guardó el BIG PICTURE P1-P4 y las reglas de madurez, sensor externo, autoridad, contratos entre grafos, compilación progresiva, testing isolation y health-of-the-loop en .claude/memory/dfl-cybernetic-rewiring-framework.md y Engram #649. Se actualizó .claude/settings.json para autoMemoryEnabled=false y autoMemory.enabled=false. No se implementaron fixes operativos, no se tocó producción, DB, credenciales ni se creó otro censo/baseline.

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

### P11 — DFL Cybernetic Rewiring L1-L8 + Health-of-the-Loop: sesión completa cerrada (2026-09-01)
**Type:** decision  
**Project:** dfl  

TCC, sesión 2026-09-01. Misión: cartografía AS-IS de lazos de control DFL (R→S→C→A→Retorno), auditoría adversarial de TCX, luego loop autónomo de rewiring L1-L8, luego primitive #8 (health-of-the-loop). Artefacto canónico único: docs/BASELINE-CERO-AS-IS-2026-09-01.md (repo saas-factory). Resumen nocturno completo y todas las entradas cronológicas en IRONMAN.md del mismo repo, buscar "P11 — LOOP AUTÓNOMO" y "P11 — Health-of-the-loop CERRADO".

INCIDENTE FUNDACIONAL (previo al loop, real, reparado en vivo): auto-renovación de dispatch freshness (authorization_renewal.py, cron 10min via activate-peer.sh) fallaba 100% desde 2026-08-19 por permisos root:root en renewal-policy.json, ilegible para dflagent. TCX bloqueado por E_DISPATCH_STALE hasta que Jorge (root) corrió el fix: chown root:dfl + chmod 640 + process manual. Verificado post-fix: /go real PASS. Bug menor NO reparado (fuera de alcance autorizado): dispatch_gate.py:207-210 lee mission.authorization_lifecycle (campo inexistente) en vez de dispatch[0].authorization.lifecycle, cae siempre a default 24h en vez de 30 días reales.

ESTADO FINAL POR TRAMO (todos con evidencia real, ningún FORMAL sin retorno probado):
- L1 (DCSA autorización): INCOMPLETO general -> FORMAL vía E2E real autorizado por Jorge: lead sintético POST a mercader-bos real (127.0.0.1:9099/api/mercader/leads) -> cron TCX automático claim/produce/AQA/deliver/ACK en ~3min sin intervención manual -> mercader_leads.order_status=ACKED confirmado en SQLite directo. Watchdog L1b (check_mission_progress.py) re-midió en vivo y pasó SILENT->OK.
- L2 (peer-work ACK/NACK): sub-tramo L2b FORMAL. Bug real en tools/telegram-bos/bot.mjs: marcaba delivered_to_telegram sin revisar el campo ok de la respuesta de Telegram. Fix + delivery-outcome.mjs (primitive pura extraída) + 7 tests aislados.
- L3 (EXTERIOR->MERCADER->FABRICA): INCOMPLETO -> FORMAL con salvedad, reclasificado por la misma evidencia del E2E de L1 (regla "si la evidencia nueva contradice otro L, corrígelo también").
- L4 (AQA orchestrator): sub-tramo FORMAL. cmdOrchestrate en tools/aqa-kit/bin/aqa.mjs no persistía GLOBAL_STATE (solo stdout) — ahora usa writeEvidenceRecord existente. Nueva primitive tools/aqa-kit/lib/re-aqa-silence.mjs envuelve metaAudit() sin tocarlo. Cron diario, budget 7 días (no 24h, para no generar ruido sobre gaps ya conocidos como JPI AQA-0 FAIL desde 2026-08-30).
- L5 (JPI reservas): sub-tramo FORMAL. Gap real: jpi_reservations.status nunca transiciona de PENDING_DEPOSIT en NINGÚN código del repo — verificado por grep exhaustivo. Watchdog reservation-watchdog.mjs en /opt/jpi, filtro SQL fuente_datos=sintetico (mismo límite de seguridad que sweep.sh ya usa). STOP respetado: login Admin JPI requiere credenciales Vercel Production inexistentes, no tocado.
- L6 (Realtor WhatsApp delivery): sub-tramo FORMAL EN REPO, NO DESPLEGADO A PRODUCCIÓN. repository.js ganó updateWhatsAppOutboundStatus (ambas clases, Local y Neon) simétrico a updateWhatsAppInbound. whatsapp-webhook.js reconcilia SENT->DELIVERED->READ real. 9/9 tests incl. 3 nuevos + suite existente sin regresión. Deploy real requiere autorización separada, explícitamente no hecho.
- L7 (Website Manager sweep): INCOMPLETO -> FORMAL. sweep.mjs existía completo (SLA-breach + heartbeat) pero sin cron. Wrapper cron-sweep.sh instalado. INCIDENTE REAL #4: validar el wrapper corrió el sweep() real contra producción con token real, 3 alertas reales enviadas — pero contenido correcto (3 items genuinamente descuidados desde 2026-08-31, nunca notificados antes).
- L8 (Engram memoria institucional): sub-tramo FORMAL. curl http://127.0.0.1:7437/conflicts?status=pending (API real, no CLI local) reveló 3 conflictos pendientes, los 3 creados 2026-07-25T02:22:38Z, 38 días sin juzgar. Watchdog tools/engram-watchdog/check_pending_conflicts.py reusa tools/lib/silence_watchdog.py (misma primitive de L1b). Cron 6h instalado sin disparar notify manualmente — el primer tick real mandará 3 alertas legítimas a Jorge.
- Primitive #8 (health-of-the-loop, decisión por ROI de Jorge, construida el mismo día que se nombró): tools/lib/loop_health_watchdog.py vigila frescura de los 5 receipts/logs de los watchdogs anteriores, cron 15min. Confirmó que los crons de L1b/L4/L7 ya corrían solos correctamente. No vigila su propia salud — límite razonable de la recursión, nombrado explícitamente.

CINCO INCIDENTES REALES AUTORREPORTADOS (ninguno encontrado por tercero): (1) alerta Telegram con silence_seconds mal calculado, sensor no leía IRONMAN.md todavía; (2) alerta Telegram con fecha simulada 2026-09-05 filtrada de un test de Retorno mal aislado — motivó la regla institucional de testing (sección 8 del baseline: ninguna primitive nueva se prueba contra store_dir/tokens/canales reales salvo E2E final autorizado); (3) referencia rota transitoria ~1s en bot.mjs durante edición multi-paso, autosanada; (4) sweep.mjs real disparado sin dry-check, 3 alertas reales pero contenido correcto — motivó práctica "dry-check-then-wire" aplicada desde entonces sin excepción (L5, L8, primitive #8 todos dry-chequeados antes de instalar cron).

PRIMITIVES P4 REUSABLES REALES (no diseño especulativo, consumidores reales): tools/lib/silence_watchdog.py (Python, 3 consumidores: L1b/L5/L8), tools/aqa-kit/lib/re-aqa-silence.mjs (JS, envuelve metaAudit), tools/telegram-bos/delivery-outcome.mjs (patrón ok:boolean), tools/lib/loop_health_watchdog.py (meta-nivel).

FRAMEWORK ESTRATÉGICO (Jorge, afinado 2026-09-01, guardado también en .claude/memory/dfl-cybernetic-rewiring-framework.md del repo saas-factory): P1=jerarquía de madurez flujo->lazo->convergencia demostrada (no dos piezas intercambiables). P2=escalar el mismo principio de control de máquina a negocio. P3/BOS=grafos conectados por contratos+memoria+herramientas+jobs+autoridad. P4=primitives acumulativas, no lista de componentes. 8 primitives transversales: (1) jerarquía de madurez, (2) sensor mide el mundo no autopercepción, (3) autoridad=ley del sistema, (4) maduración humano->agente->script, (5) contratos entre grafos, (6) P4 como compilación progresiva, (7) testing isolation, (8) health-of-the-loop.

GAPS EXPLÍCITAMENTE ABIERTOS, NO INFLADOS: login Admin JPI + concurrencia real (credenciales inexistentes); deploy Realtor a producción (autorización separada pendiente); actuador de confirmación de depósito JPI (decisión de negocio no infraestructura); juicio automático de conflictos Engram (acto cognitivo, no automatizado); bug de campo mal nombrado en dispatch_gate.py; auditoría adversarial de TCX sobre todo este cierre, pendiente de autorización de Jorge.

PRÓXIMO PASO EXPLÍCITO PARA QUIEN RETOME: 1) Confirmar en IRONMAN.md si hay entradas posteriores a esta (buscar fecha >2026-09-01 o "L9"). 2) Si Jorge pide seguir, la auditoría adversarial de TCX sobre L1-L8 + primitive #8 es el trabajo pendiente más directo. 3) Ningún STOP duro se disparó durante toda la misión — los límites de autoridad reales (JPI Admin, Realtor deploy) se respetaron sin rodeos, no requieren re-visitarse salvo que Jorge cambie esa decisión explícitamente.

### Cierre de sesión 2026-09-01 — baseline cero auditado y marco P1-P4 persistido
**Type:** fact  
**Project:** dfl  

Cierre de sesión 2026-09-01. Se revalidó /go con decisión PASS para TCX y alcance en saas-factory, JPI y whatsapp-realtor-mvp. Se creó docs/BASELINE-CERO-AS-IS-2026-09-01.md como único baseline AS-IS dentro del alcance autorizado. Auditoría adversarial del mismo artefacto: L1 bajó a INCOMPLETO (gate formal), L2 quedó FORMAL solo para lifecycle interno, L4 bajó a INCOMPLETO (RE_AQA_REQUIRED prescribe otro run), L6 documentó falta de reconciliación outbound/status, L7 bajó a INCOMPLETO por scheduler recurrente no demostrado y L8 bajó a AUSENTE como lazo de control. Se añadieron F-01..F-08 con evidencia. Se guardó el BIG PICTURE P1-P4 y las reglas de madurez, sensor externo, autoridad, contratos entre grafos, compilación progresiva, testing isolation y health-of-the-loop en .claude/memory/dfl-cybernetic-rewiring-framework.md y Engram #649. Se actualizó .claude/settings.json para autoMemoryEnabled=false y autoMemory.enabled=false. No se implementaron fixes operativos, no se tocó producción, DB, credenciales ni se creó otro censo/baseline.

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

*Mirror auto-generated 2026-09-01T04:05:06Z | La Garra → DFLghub/amos-context*
