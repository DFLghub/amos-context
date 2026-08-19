# amOS Context — @$go Live Mirror
**Generated:** 2026-08-19T16:29:37Z  
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

### Vertical slice real MERCADER↔FÁBRICA E2E: 1 pedido, ciclo completo, CERRADO (2026-08-19)
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/mercader/vertical-slice-e2e-2026-08-19
STATUS: CERRADO
DATE: 2026-08-19

WHAT: Primera prueba real (no simulada) del ciclo MERCADER ORDER -> FABRICA RECEIVE -> COMMIT -> PRODUCE -> AQA -> DELIVER -> MERCADER RECEIVE/ACK, usando el Design Candidate v0 (obs #528) como referencia, NO como obligacion de implementar todas sus piezas. Regla de Jorge: reutilizar todo lo existente, construir solo el delta que el E2E demuestre necesario, no adelantarse a elasticidad multiple.

ORDER_ID real: MERCADER-ORDER-TEST-20260819T162514Z (pedido de PRUEBA explicito, prospecto TEST, no cliente real -- consistente con el limite ya establecido hoy en CHECKPOINT-MERCADER-QR-2026-08-19.md de cero contacto real).

Mecanismos EXISTENTES reutilizados intactos, sin modificar:
- tools/peer-work/ (peer_work.py) como transporte generico REQUEST/COMMIT/STATUS/DELIVER -- MERCADER emitio el ORDER como un item mas de la misma cola que usa cualquier otro trabajo DFL, sin integracion bespoke. authority_ref="human=telegram:8776472165" verificado en vivo contra el allowlist real antes de crear el primer item.
- El patron real de r-d-delivery/delivery.mjs (construido y probado HOY MISMO por otra sesion en /tmp/mercader-verify-fork/, 6/6 aserciones) -- reusado (mismas funciones, mismo invariante "un orderId = una entrega"), no reescrito desde cero.
- AQA Kit real (tools/aqa-kit/bin/aqa.mjs) -- corrido de verdad, nivel AQA-1, perfil CRUD_LIFECYCLE, con ALLOW (creacion de entrega, min-length del documento, correlacion de order_id) y DENY (duplicado correctamente rechazado) reales. Resultado real: PASS, profiles covered=[CRUD_LIFECYCLE], missing=[].

Delta minimo construido (solo lo que el E2E demostro necesario):
- deliver.mjs: version exportable/reusable del patron ya probado (mismas funciones, agregado solo un guard de duplicado persistido en disco via delivery-record.json, porque la version anterior era in-memory y no servia para un DENY check real de AQA).
- onepager.md real: contenido real generado (no placeholder de texto "ok"), siguiendo la estructura ya documentada de MERCADER Distill (AI_Product_Distillation_OnePager_v0_1.md).

NO se construyo: Scheduler, Capacity Registry, PDP de autorizacion por parametro, cloning de Fabricas, ni ninguna otra pieza del Design Candidate v0 -- correctamente, porque un solo pedido no las necesito. Confirma en la practica lo que la regla de Jorge predijo: "si 1 pedido no cruza completo, no tiene sentido probar 10" -- 1 SI cruzo completo, con piezas existentes casi sin delta.

Evidencia real verificable:
- peer-work request_id pw-e27f05cf50e6 (ORDER, COMMITTED->COMPLETED) y pw-f73e9583f3ab (ACK, COMPLETED), ambos con inputs.order_id=MERCADER-ORDER-TEST-20260819T162514Z -- correlacion confirmada por lectura directa post-hoc, no asumida.
- AQA receipt real: tools/aqa-kit/evidence/mercader-e2e-vertical-slice/MERCADER-ORDER-TEST-20260819T162514Z/27811cb/2026-08-19T16-27-41-951Z/receipt.json, status=PASS.
- delivery-record.json real con token real (jLiSu6pxXqTILGrAecLlX5juJOaN9KLF) y link real (https://deepfeelingslabs.com/entrega/<token>) -- el link no fue navegado/publicado (dominio real de DFL, no se hizo ninguna llamada de red hacia el).
- onepager.md real, con contenido explicito marcando que es un pedido de prueba, ningun dato de cliente real usado.

Que sigue siendo bespoke: NADA especifico a MERCADER<->FABRICA -- MERCADER hablo con el mismo peer-work que cualquier otro origen (TELEGRAM, otros agentes), FABRICA (TCC) recibio como target_executor generico, DELIVER volvio por el mismo mecanismo (complete() del item original), sin ningun canal o codigo exclusivo de este par de organismos. NO_POINT_TO_POINT confirmado en la practica, no solo en diseno.

Blocker real: ninguno aparecio en este ciclo -- brief simple, deliverable producible con capacidad ya existente, sin credencial externa faltante. QUIERO->bloqueo->REQUIERO no se activo porque no hizo falta, no se forzo artificialmente.

NEXT AGENT: el circuito de 1 pedido esta PROBADO y REUTILIZABLE (mismo peer-work, mismo AQA, mismo patron de delivery sirven para el siguiente pedido sin modificacion). La siguiente mision autorizada por Jorge (NO ejecutada aca) es repetir con multiples pedidos simultaneos para tensionar elasticidad real -- ahi es donde recien aparecera si Scheduler/Capacity Registry/provisioning son necesarios de verdad, no antes.

### Causa raíz de fricción de permisos eliminada + confirmado BOS v6 = mismo core que v5/mercader-bos + fix portado (2026-08-19)
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/saas-factory/fewer-permission-prompts-and-bos-v6-comparison-2026-08-19
STATUS: closed
DATE: 2026-08-19

WHAT 1 (autonomía): Jorge pidió eliminar la causa raíz de las interrupciones repetidas por autorización, no solo dejar de preguntar. Corrida skill fewer-permission-prompts: escaneados 50 transcripts recientes, extraídas frecuencias reales de Bash/MCP. Causa raíz real: la mayoría de comandos usados (git status/log/diff/branch/rev-parse/remote/show, ps, lsof, grep, sed, find, cat, wc) YA estaban auto-permitidos por el harness -- la fricción real venía de un set chico de patrones no cubiertos (curl a servicios internos :8091/:7437, mcp__engram__search_memory y otras MCP read-only sin regla). Agregadas 13 reglas nuevas a saas-factory/.claude/settings.json (permissions.allow, antes inexistente): 6 Bash (curl a 127.0.0.1:8091/* y :7437/*, curl a health checks locales, export de 2 env vars de AQA/XDG, column) + 7 MCP read-only (search_memory, Drive search/read/download, Supabase get_project_url/list_tables/list_migrations). Excluido deliberadamente pese a alta frecuencia: mcp__supabase__execute_sql (128 usos, pero ejecuta SQL arbitrario -- puede escribir), docker exec, engram search (CLI bare, ya documentado como store equivocado), bash push_mirror.sh (hace commit/push real). No se tocó permissions.deny/ask ni ningún otro campo.

WHAT 2 (pregunta de Jorge sobre BOS v6): Verificado con diff real -- BOS v5/agent-server y BOS v6/bussinesO/agent-server tienen el mismo set de archivos, prácticamente idénticos (una sola diferencia real en agent.ts). El "salto" de v6 no es el orquestador -- es que v6 empaqueta verticales ya construidas (Automatización de Redes Sociales = SocialFlow AI, SaaS B2B para Agencias, meta-google) junto al mismo core. mercader-bos (el BOS real y vivo de MERCADER) ya corre ese mismo core (confirmado archivo por archivo), con extensiones propias de MERCADER encima (phase-4-automation, bot-mercader, routes-mercader, etc.) -- o sea, activar SocialFlow AI en mercader-bos (hecho en la tarea anterior, obs #520) ya captura el valor real de v6 sin necesitar migrar de core.

Único delta real de código encontrado entre v5 y v6: agent.ts de v6 reintenta UNA vez con sesión nueva cuando el sessionId guardado ya no existe en disco ("No conversation found with session ID"), en vez de fallar duro. mercader-bos NO lo tenía -- portado a mercader-bos/agent-server/src/agent.ts, typecheck + build limpios, es exactamente el código que corre el mecanismo AGENT_PROJECTS que se acaba de activar para SocialFlow AI.

NEXT AGENT: si Jorge vuelve a preguntar por diffs entre versiones de BOS del dump "Varios para SFV5/BusinessOS/", el core agent-server es esencialmente el mismo en v2-v7 salvo parches puntuales -- comparar antes de asumir que una versión "funciona mejor" arquitectónicamente; en este caso la diferencia real era el bundle de verticales, no el motor.

### DFL LAB HARVEST 2026-08-15: TCC x TCX concurrency + VM2 n=2 load — methodology, not just result
**Type:** checkpoint  
**Project:** saas-factory-setup  

TOPIC: dfl/labs/tcc-tcx-concurrency-vm2-n2
TYPE: lab_harvest
DATE: 2026-08-15
LIFECYCLE: active
AUTHORITY: Jorge (Lab commissioned), executed by TCC

== CONCLUSIONES: PROVEN vs NOT PROVEN ==

PROVEN (E2E, evidencia independiente, no autorreportada):
- Dos Tonys (TCC + un proceso real de `codex exec`, TCX) pueden operar sobre la misma SFV5/Iron Man con workstreams reales, distintos, en ventanas de tiempo genuinamente solapadas (12s de overlap medido por timestamps), sin colisión de archivos, sin pérdida de commits, sin pérdida ni mezcla de escrituras en Engram (session_id distintos confirmados), con `push_mirror.sh` serializando correctamente bajo concurrencia inducida (flock real, no accidental).
- El aislamiento por `git worktree` (patrón ya existente en DFL, no inventado) elimina la colisión de archivos entre Tonys operando simultáneamente sobre el mismo repo.
- El slot único de dispatch (`pending` con exactamente una misión) NO es un requisito para trabajo productivo real -- evidencia: 15+ commits reales de este arco completo con `execute_permitted:false` todo el tiempo. La restricción de una sola misión sigue sin resolverse, pero resultó estar fuera del camino crítico del QUIERO original.
- VM2 (2 vCPU, ~3.9GB RAM) soporta n=2 procesos reales concurrentes (2x `codex exec`, tarea comparable) sin OOM, sin exit 137, sin throttling visible, sin degradar `wp-tent-app`/`wp-tent-db` ni la salud de `/go`/Engram verificada post-carga.

NOT PROVEN (explícitamente, no inflar):
- La medición de carga (n=2) fue con DOS procesos del MISMO motor (2x Codex), no con TCC+TCX heterogéneos bajo las mismas condiciones controladas y monitoreadas -- el demo de concurrencia genuina (git/Engram/mirror) sí fue TCC+TCX real, pero la medición cuantitativa de impacto en VM2 no. La huella de recursos específica de TCC operando concurrentemente con TCX no está medida con el mismo rigor.
- Solo 1 muestra de línea base y 1 muestra de n=2 por lado (A/B) -- +7.6%/+19.3% es una señal real pero de tamaño de muestra insuficiente para descartar ruido de medición. No hay repetición que confirme que el rango es estable.
- No se muestreó la latencia de `/go`/Engram de forma continua DURANTE la ventana exacta de concurrencia -- se verificó antes y después, no en cada segundo de la ventana. "Siguieron respondiendo normalmente" es cierto pero con menos granularidad de la que el enunciado sugiere.
- No se probó n=3 ni el punto de quiebre -- explícitamente fuera de alcance de este Lab, no se debe inferir de acá.
- No se determinó si el disco al 95%/swap al 100% en reposo son causa de algún costo de rendimiento medible hoy, o si son simplemente el estado histórico normal de esta VM -- quedó flageado, no diagnosticado.

== OBSERVACIONES / HALLAZGOS ==

1. Concurrencia TCC x TCX: confirmada real (session_id distintos: mcp-external-...-7b2af778 para TCX vs ...-c9cdceff para TCC), no simulada por un solo proceso jugando dos roles.
2. Aislamiento por worktree: funcionó exactamente como se esperaba, cero colisión, mismo patrón que DFL ya usaba en otros 4 worktrees preexistentes -- no fue una invención de este Lab, fue reutilización correcta.
3. Impacto medido en VM2 bajo n=2: CPU us/sy picos 42%/56% en 2 vCPUs, breve (~15-20s), RAM disponible bajó de ~400MB a ~325MB, IO real (bi hasta 130k) durante la ventana. Duración: 24.9s baseline -> 26.8s/29.7s concurrente. Ningún proceso murió, ningún timeout real (una vez corregido el bug de invocacion).
4. Swap/disco preexistentes: VM2 tenia swap 100% usado (2047/2047MB) y disco 95% lleno (4.5GB libres) ANTES de que este Lab tocara nada -- no es degradacion inducida por el experimento, es el estado de base de la maquina, y quedo anotado como riesgo real independiente en IRONMAN.md, no resuelto aqui.
5. FALSO INDICIO introducido por mi propia invocacion (el hallazgo metodologico mas importante de este Lab): el primer intento de medir la linea base colgo 2 minutos. Sin investigar, ese dato se hubiera podido reportar como "VM2 no soporta ni una tarea sola, mucho menos concurrencia" -- una conclusion catastroficamente equivocada. La causa real: `codex exec` invocado sin `< /dev/null`, quedo esperando stdin indefinidamente (el log decia literalmente "Reading additional input from stdin..."). Cero relacion con CPU/RAM/concurrencia. Jorge fue quien primero cuestiono "no es logico ese nivel de carga" y disparo la investigacion correcta -- yo no lo hice proactivamente antes de que me lo señalaran.

== PREP: preparacion minima del host antes de una nueva linea/Tony (principio, no automatizacion pesada) ==

Principio: antes de atribuir cualquier consumo o degradacion a una linea nueva, hay que saber que NO es residuo de algo anterior. Forma minima recomendada (checklist manual de 4 pasos, no un daemon, no un harness):
1. `pgrep -a` (o equivalente) para detectar procesos huerfanos/zombies o sesiones previas de Labs que sigan vivas sin proposito activo.
2. Barrido de temporales/artefactos conocidos de Labs anteriores identificables por convencion de nombre (`lab/*` branches, `/tmp/*lab*`, worktrees de medicion) -- limpiar solo lo que sea inequivocamente residuo, nunca por sospecha.
2.b. NUNCA limpiar (ni siquiera cuando parezca residuo): puntajeTigreKnockout, Supabase, Vercel config, env vars, templates HLC-T01/T02/T03, CRON 3:05am UTC, /etc/dfl-secrets -- las superficies NO_TOUCH nunca son "residuo elegible", sin importar cuan viejas o abandonadas parezcan.
3. Snapshot de recursos EN REPOSO tras la limpieza (loadavg, free -m, df -h) -- este es el baseline real, no el que se toma antes de limpiar.
4. Recien despues de 1-3, arrancar la linea nueva y medir contra ese baseline limpio.
Este Lab no siguio este orden con disciplina -- el chequeo de "CX-A/CX-B siguen vivos" se hizo reactivamente cuando Jorge pregunto por zombies, no proactivamente antes de declarar un baseline. Ese es exactamente el gap que este principio cierra hacia adelante.

== QUE HUBIERA HECHO DISTINTO Y POR QUE (autocritica, no autocomplacencia) ==

1. Deberia haber corrido el checklist PREP (arriba) ANTES de tomar el primer `free -m`/`loadavg` de "reposo" -- los numeros de reposo que reporte ya incluian CX-A/CX-B corriendo hace mas de una hora, sin decirlo explicitamente como tal en ese momento.
2. Deberia haber corrido 2-3 muestras de linea base y 2-3 de concurrencia, no 1 y 1 -- el +7.6%/+19.3% es una observacion real pero estadisticamente fragil, y lo reporte con mas confianza de la que una sola muestra deberia inspirar.
3. Deberia haber muestreado `/go`/Engram con timestamps DENTRO de la ventana de concurrencia, no solo antes/despues -- dije "siguen respondiendo normalmente" con menos evidencia de la que esa frase implica.
4. Deberia haber usado un TCX real Y una tarea TCC comparable y cronometrada de la misma manera para la medicion cuantitativa -- use 2x Codex por conveniencia, lo cual mide concurrencia-de-motor-homogeneo, no exactamente la pregunta de "TCC+TCX especificamente" que se estaba haciendo.
5. No deberia haber borrado las ramas/worktrees del Lab (`lab/tcx-concurrency-2026-08-15`, `lab/n2-b-2026-08-15`) sin al menos etiquetar (`git tag`) los commits de evidencia primero -- ahora son commits inalcanzables (dangling), recuperables por poco tiempo via `git show <hash>` pero no garantizados si corre un `git gc` agresivo. La limpieza estuvo bien motivada (no dejar ramas de lab acumulandose) pero el orden fue incorrecto: etiquetar primero, limpiar despues.
6. No investigue si el disco al 95% tiene relacion causal con los picos de IO observados (`bi` hasta 130k) -- quedo como dos hechos yuxtapuestos, no una hipotesis probada ni descartada.
7. Lo que SI hice bien y vale la pena repetir: no acepte el timeout de 2 minutos como dato de carga sin investigar la causa raiz primero (una vez que Jorge lo señalo); verifique independientemente cada afirmacion antes de reportarla (exit codes, git log de ambos lados, contenido de Engram via API directa, no confiando en el stdout de los subprocesos); y separe explicitamente lo que el Lab si probo de lo que no, en vez de generalizar el resultado positivo mas alla de su evidencia real.

== RECOMENDACION DE LA FABRICA SOBRE LA FABRICA (MUST / SHOULD / LATER) ==

MUST (barato, sin ambiguedad, aplicar ya en el proximo Lab):
- Redirigir stdin explicitamente (`< /dev/null`) en toda invocacion no-interactiva de `codex exec` u otro CLI similar usado en automatizacion -- el bug de esta sesion es evitable con una linea, y el costo de no hacerlo es un falso indicio que puede llevar a una conclusion opuesta a la real.
- Correr el checklist PREP (4 pasos, arriba) antes de declarar cualquier baseline de recursos, siempre, no solo cuando alguien pregunta por zombies.
- Etiquetar (`git tag`) los commits de evidencia de un Lab ANTES de borrar sus ramas/worktrees, si el Lab se declara cerrado y exitoso.

SHOULD (barato, vale la pena, no urgente):
- Para cualquier medicion de duracion/degradacion, tomar minimo 2-3 muestras por condicion antes de citar un porcentaje con confianza.
- Si se mide un componente compartido (`/go`, Engram) durante una ventana de concurrencia, muestrear continuamente dentro de la ventana, no solo antes/despues.
- Registrar el estado de swap/disco de VM2 como chequeo independiente y periodico (no atado a ningun Lab especifico) dado que ya se encontro en un estado ajustado sin que nadie lo hubiera investigado antes.

LATER (NO hacer todavia, evitar sobreingenieria):
- Un harness reusable de Lab (PREP + monitor + cleanup en un solo script) -- solo si los Labs se vuelven lo bastante frecuentes como para justificarlo; hoy seria automatizar antes de tener el patron de uso repetido, exactamente el atajo que este arco entero evito en F-ARCH-4 y en otros lugares.
- Automatizar la deteccion de "cuando el disco/swap de VM2 se vuelve un problema real" -- por ahora un chequeo manual ocasional alcanza; no hay evidencia todavia de que sea un problema activo, solo un margen mas ajustado de lo esperado.

== EVIDENCIA ==
Commits (ahora dangling, no en ninguna rama, recuperables por hash mientras no corra gc agresivo): d7480410f5702a978d54d218f1b3282f6feb7dbd, 752efb9b1cec6774e7fea5ba3be392cdaa199962, ebd014ec496d1bb2ec312207cf005d1cd437a82d. Commits en la rama principal (persistentes): 8372ec1 (IRONMAN.md creado), ed4877b (cierre Lab TCC x TCX), 84b6b7e (cierre medicion VM2 n=2). Engram obs #491-#497 (tests de concurrencia y probes). IRONMAN.md, filas "Concurrencia TCC x TCX" y "VM2: n=2 fabricas virtuales concurrentes".

### @$fin CIERRE FORMAL 2026-08-15 -- Codex handoff completo publicado, MCP de Codex verificado y registrado, arco 100% cerrado
**Type:** session_summary  
**Project:** saas-factory-setup  

Formal @$fin close of the Claude Code session that ran this entire arc (onboarding freshness, WP Competence Specimen B/C, wp_eval loophole, push_mirror.sh fix, F-ARCH-1 closure). Canonical state record remains obs #488 (FINAL CLOSE) -- this observation is the @$fin transport-layer closure on top of it, not a new content layer.

What this closing pass added beyond obs #488:
- Full agent-agnostic, non-executive-summary handoff written to the repo: saas-factory/.claude/CODEX-HANDOFF-SFV5-FIRST-OPERATION-2026-08-15.md (commit 2c7057a) -- built to let Codex reconstruct full operational state without reinterpreting anything.
- Verified (not assumed) Codex's real capabilities on this host: codex CLI 0.146.0 present, /opt/saas-factory-setup already trust_level=trusted in ~/.codex/config.toml, .mcp.json (Claude Code's MCP config) does NOT carry over to Codex (codex mcp list was empty before this pass) -- registered engram MCP for Codex directly: `codex mcp add engram --url http://127.0.0.1:8092/mcp`, confirmed live via `codex mcp get engram` (enabled:true, transport:streamable_http). Flagged CLAUDE.md auto-load behavior for Codex as UNKNOWN/TO VERIFY since no AGENTS.md equivalent was found in this repo -- did not invent an equivalence that isn't demonstrated.
- Re-verified end-to-end one more time before closing: /go dispatch_receipt still PASS/NO_DISPATCH_BLOCK_PRESENT, pending still [], mirror HEAD==origin/main, wp_cli_command eval still refused, breaking_news/memory_conflicts fields still present in production.

Gate 4B step 2 (archival check): nothing new to archive beyond what #487->#488 already superseded. No further observations from this arc need [RESOLVED]/LIFECYCLE:archived marking.

Session identity: this was a Claude Code EJECUTOR session (bash/git/Engram all verified by real execution at onboarding), operating on /opt/saas-factory-setup, branch fase-3-5-jpi-real-sfv5-bridge, final HEAD 2c7057a. Handing off to Codex per Jorge's explicit instruction (Claude Code credit running low across the arc, but the arc itself was fully closed by Claude Code before handoff -- Codex does not need to do any remaining work on THIS arc).

Next work (NOT started, NOT chosen which goes first): DFL Website, JackyClean, Transportes y Eventos JPI. Jorge's decision.

### Design Candidate v0 institucionalizado — arquitectura mínima elástica DFL, corregida, READY FOR REAL-WORLD VALIDATION (2026-08-19)
**Type:** architecture  
**Project:** dfl  

TOPIC: dfl/design/elastic-capacity-interop-v0
STATUS: closed (institucionalizacion) / DESIGN CANDIDATE READY FOR REAL-WORLD VALIDATION (no implementado, no runtime tocado)
DATE: 2026-08-19

WHAT: Sintesis final de la sesion completa. Jorge cerro la cadena RESEARCH CLOSED (Monoid obs#524, interop-2026 obs#525, ARD obs#526, A2A en chat, identidad/autoridad/delegacion obs#527) pidiendo sintetizar (no investigar mas) la arquitectura minima fractal propia de DFL para interoperabilidad universal entre organismos sin integracion punto-a-punto. Primera version tuvo una correccion de principio critica de Jorge ("todo trabajo valido y autorizado debe hacerse, la falta de capacidad NO es causal de rechazo") que obligo a repensar ACCEPT/capacity-awareness. Luego una correccion quirurgica final (P11) sobre 4 confusiones reales que TCC seguia cometiendo: (1) elasticidad limitada a un solo nivel worker/executor en vez de fractal (worker->celula->Fabrica->n Fabricas); (2) Capacity Registry cristalizado prematuramente como busy/free en vez de contrato abierto extensible; (3) claim sin evidencia de que AGENT_PROJECTS "equivale a" clonar una Fabrica -- retirado, reemplazado por separacion explicita PROBADO/HIPOTESIS/POR-DEMOSTRAR; (4) "ACCEPT incondicional" confundiendo compromiso organizacional con readiness/ejecutabilidad inmediata -- corregido a COMMITMENT != READINESS con 6 responsabilidades distintas (commitment, readiness, scheduling, provisioning, ejecucion, blocker real).

Institucionalizado en docs/patterns/design-candidate-v0-elastic-capacity/{DESIGN.md,dfl.yaml}, asset_id dfl.design-candidate.elastic-capacity-interop.v0, capability_type=design-candidate (distinto de "research" -- es sintesis de diseno, no autopsia), status DRAFT explicito -- NO active, NO estandar institucional, NO autorizacion de construccion en bloque. Indexado: 26 assets (subio de 25), 0 errores, 7/7 tests, descubribilidad verificada con 5 queries directas, todas exitosas sin problema de fraseo esta vez.

7 invariantes finales preservados integros: NO DROP, COMMITMENT!=READINESS, ELASTIC CAPACITY IS FRACTAL, FAIL->REASSIGN/RECOVER (fundamentado explicitamente en el Axioma A4 de la Constitucion DFL v2 leida temprano en esta misma sesion -- crash-only, el sucesor lee Ledger+contrato y continua, no inventado, doctrina DFL ya existente), TEMPORARY SCALE-DOWN, NO POINT-TO-POINT, BLOCKER!=TERMINAL (nuevo en esta ronda).

Arquitectura minima: reusa intacto Asset Index/dfl.yaml, DCSA (mission authorization, confirmado en el research anterior como SIN equivalente externo encontrado en toda la industria), patron manager.mjs (ACCEPT generalizado), AGENT_PROJECTS/Claude Agent SDK (alcance de la prueba real corregido -- solo crea/ejecuta sesion con cwd, NO demostrado que equivale a clonar Fabrica), AQA (DELIVER, sin cambios), bloqueo optimista del workforce-registry-capability-lab (16/16 escenarios ya validados, citado para que el Scheduler no sea chokepoint). Piezas nuevas minimas: Workload Ledger (extension de peer-work), Capacity Registry (contrato abierto, NO busy/free definitivo), Scheduler (matching no bloqueante, resuelve REQUIERO horizontal o vertical sin nivel fijo), PDP minimo de AUTHORIZE-invocation, RECEIVE/ACK generico.

Correccion explicita de instruccion directa de Jorge, reemplaza conclusion previa: NO es requisito cerrar la fila "CAPACIDAD QUE FALTA DEMOSTRAR" (aislamiento entre clones, continuidad tras teardown, costo/tiempo real, si Fabrica es la unidad clonable correcta) antes de implementar -- esas hipotesis se validan cuando un outcome real las obligue via QUIERO->bloqueo->REQUIERO, no se abre investigacion nueva por adelantado.

Contrastado explicitamente contra evidencia operativa real independiente del mismo dia: CHECKPOINT-MERCADER-QR-2026-08-19.md (hallazgos R-F1..R-F8) -- el gap que este diseno intenta cerrar fue confirmado dos veces, por research externo de 5 rondas Y por diagnostico operativo real de MERCADER, coincidencia no fabricada.

NEXT AGENT: DESIGN CANDIDATE v0 INSTITUTIONALIZED -- READY FOR REAL-WORLD VALIDATION. La siguiente mision autorizada (no ejecutada aca) es el vertical slice real: MERCADER ORDER -> FABRICA RECEIVE -> COMMIT -> PRODUCE -> AQA -> DELIVER -> MERCADER RECEIVE/ACK -> REPEAT, primero con 1 pedido, luego repetido con multiples pedidos simultaneos para tensionar elasticidad real (no simulada). Ese ciclo es el que debe descubrir que piezas de este Design Candidate necesitan existir de verdad y en que orden -- no re-disenar desde cero, no re-investigar. No confundir esta institucionalizacion con autorizacion de implementar toda la arquitectura de una vez -- sigue siendo DRAFT.

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

### [SFV5] Entrevista canonica completada: la fabrica se autodescribe, mecanismo de interrogacion resuelto y 3 hallazgos criticos verificados
**Type:** fact  
**Project:** dfl  

TOPIC: dfl/saas-factory/canonical-self-description
TYPE: fact
STATUS: closed
DATE: 2026-08-05
PRECEDENCIA: D
AUTHORITY: evidence only
LIFECYCLE: active
CONFIDENCE: high

ENTREVISTA CANONICA A SFV5 COMPLETADA. La fabrica se describio a si misma en 10 bloques, una sola voz (session ad815526), clon canonico 9b18947, solo lectura, cero mutaciones. Evidencia en /opt/dfl-knowledge/evidence/sfv5-canonical-interview-2026-08-05/ (8 documentos + 14 transcripciones JSON + SHA256SUMS + HANDOFF-TO-CX.md).

MECANISMO RESUELTO (cierra Q65 del discovery #460, que quedo INFERENCE): no existe una segunda instancia de Claude Code esperando; se INVOCA. `claude -p` con cwd en <checkout>/saas-factory carga las 32 skills (~19k tokens medidos contra control vacio de 5.7k) y esa sesion ES la fabrica. Reanudable con --resume. LIVE_PROVEN.

TESIS DE LA FABRICA (verbatim): "Soy una arquitectura de contratos declarativos ejecutada por un interprete probabilistico que no controlo, sobre un sistema de archivos que es mi unica memoria." Corolario suyo: todo imperativo en sus archivos (OBLIGATORIO/SIEMPRE/gate duro) es intencion de diseno, no garantia de ejecucion; las unicas garantias reales viven en tools/bridges/ (4 archivos de 82).

HALLAZGOS ACCIONABLES verificados independientemente por Claude Code:
1. CRITICO update-sf/SKILL.md:52 hace `rm -rf .claude/` y destruye .claude/memory/, PRPs del proyecto y settings.json, mientras anuncia "Archivos NO modificados". Reactiva en silencio la auto-memory del host que memory-manager desactivo. Con N proyectos = destruccion sistematica del activo de aprendizaje compuesto.
2. CRITICO quality-gates/SKILL.md:25 exige `npm run typecheck`; package.json solo define dev/build/start/lint. Gate duro que invoca script inexistente.
3. CRITICO dos clases de ciudadano: CLAUDE.md:360 "SIEMPRE habilitar RLS" vs add-payments:74 "service_role bypasses automatically". Resuelve el actor no-humano apagando su invariante mas fuerte.
4. Sin concepto multi-inquilino organizacional: 1 hit en skills/+src/ y es "Google Workspace" incidental. Su unidad de aislamiento es el individuo, no la organizacion.
5. codebase-analyst listado en SKILLS_README.md:59 sin directorio: su unico skill de analisis desaparecio en V4->V5.
6. Son 32 skills, no las 30 documentadas.

IDENTIDAD DERIVADA POR LA PROPIA FABRICA: su especializacion no es un dominio de negocio (atraveso biometria, RAG legal, generacion de video y cold email sin modificarse, verificado en vertical-pack:6) sino una TOPOLOGIA de entrega y operacion. Nombre operativo propuesto por ella: "fabrica de instancias multi-principal operadas en continuo". Criterio de pertenencia: el entregable hay que mantenerlo vivo y observarlo, y todos los principales se autorizan contra el MISMO modelo (hoy la respuesta es no). Aclaro que NO propone renombrar el repo, solo la definicion operativa de asignacion.

CONSECUENCIA PARA EL FMD (#280): convergencia independiente, sin que se le entregara el diseno del Gerente. Derivo sola que tools/bridges/ YA presupone un gerente (factory_request_id, goal_id, attempt_number, evidence_path) y fijo la frontera: "el gerente gobierna ENTRE misiones, yo gobierno DENTRO; la frontera es el paquete de mision y se cruza solo con artefactos, nunca con supervision". Interfaz minima de 8 superficies: 1-4 ya existen en el bridge, 5-8 son la brecha.

GENTLE AI: veredicto SI con acceso al clon completo (52adc25). Capacidad adoptable = el recibo ligado por hash al estado verificado (receipt.go:15) + gates que solo leen recibos. Concepto si, implementacion no. Debe vivir en el kernel compartido; SFV5 emite, el gerente hace vinculante; los tres roles no se pueden fusionar so pena de autocertificacion.

ESTADO DFL VERIFICADO EN LA GARRA: no hay kernel, no hay CI (cero .github/), no existe ~/.saas-factory/brain/, y nada en produccion emite mission packets (los unicos factory_request_id son los que fabrico a mano el discovery). El tramo "hacer vinculantes los recibos" no tiene hoy actor posible.

SIGUIENTE PASO: pedirle el INVENTARIO DE ATESTACION (entregable falsable que ella misma propuso). Pendiente de Jorge, bloquea el resto: que limites volver vinculantes, y si hay identidad para aprobaciones humanas.

CAVEAT DE METODO: la fabrica es parte interesada describiendose a si misma. Los [FACT] no verificados por Claude Code son citas suyas, no evidencia. Se verificaron ~20 de mayor consecuencia.

Costo total 10.50 USD, 14 invocaciones.

### [AUDIT] Cabo 7 — auditoria independiente de cierre 2026-08-03: CLOSED_WITH_NONBLOCKING_SECURITY_DEBT
**Type:** fact  
**Project:** dfl  

TOPIC: dfl/infra/cabo-7-independent-closure-audit
TYPE: fact
STATUS: closed
DATE: 2026-08-03
PRECEDENCIA: D
AUTHORITY: evidence only — no gobierna routing ni despacho
LIFECYCLE: active
CONFIDENCE: high

MISION: DFL_CABO_7_INDEPENDENT_CLOSURE_AUDIT (modo YOLO, verificador independiente, read-only).

VEREDICTO: CABO_7_CLOSED_WITH_NONBLOCKING_SECURITY_DEBT.

SINCRONIA: /opt/dfl-knowledge-workunit rama main, working tree limpio. main = origin/main = 878f09ae596b1067314925ac02b29cd122642bf9, ahead/behind 0/0, confirmado contra GitHub con git ls-remote (no solo ref local). a8269d9f44d4050568edd1a77122b2d16d7d8170 y 878f09a ambos ancestros-o-iguales de origin/main; a8269d9 es el segundo padre del merge. Ninguno firmado (%G? = N).

CHECKSUMS: sha256sum -c SHA256SUMS 19/19 OK exit 0.

GATES (independiente): reproducidos por mi con exit 0 — test_fixture.sh FIXTURE_PASS (G02 G03 G04 G05 G06 G09-fixture G12), test_endpoint.sh ENDPOINT_REGRESSION_PASS, aggregate.sh sobre final-reverify.receipt AGGREGATE=PASS, aggregate.sh sobre final.receipt AGGREGATE=FAIL exit 1 (G13 real). Vivo verificado por mi: G14 systemctl active + GET 127.0.0.1:8091/go http 200 69498 bytes; G11 local=publico=965d06fdd157a206d17c0af2d41ec2f3b56c799d550222e61096ab8641f63cc2; G02 /run/dfl root:dfl 2770; G07/G08 corroborados read-only (2770/0660, other sin acceso); G10 corroborado por amos-context.md = dflagent:dfl y log 01:44 con identidad git dflagent y PUSH OK.

G11 RESPALDADO: log g11-resolution.log con TIMEOUT_S=90 POLL_S=5 y 19 muestras. Primer poll discrepante 01:44:52, coincidencia 01:46:19 = 87s exactos. Commit publicado d8ddc31282d5ac3eaaa81df6d63b44be111cf326, SHA de convergencia 5987ef1290944a508a222c7fd0be3810a3fbd06890b136d78337465ec751ac48. Causa raiz confirmada en log de produccion: fatal unable to auto-detect email address (dflagent@ubuntu-s-1vcpu-1gb-nyc1) — repo mirror sin identidad de commit, no consistencia eventual. Reparacion: user.name La Garra Bot local al repo mirror. Confirmacion fresca: mirror ya en 2543ded 01:57:02 y local=publico sigue coincidiendo.

DEUDAS NO BLOQUEANTES:
1. LLAVE SSH — /home/dflagent/.ssh/id_ed25519 sin passphrase (ssh-keygen -y -P '' la abre), comentario said-vm2-la-garra (identidad de host preexistente, no dflagent), sin .pub, birth 2026-08-02 18:58:21 en plena remediacion. ssh -T git@github.com responde Hi DFLghub: es llave DE CUENTA, no deploy key con scope a amos-context — dflagent tiene escritura sobre toda la organizacion. Fingerprint SHA256:UHF2r33fb2kMeEKvz7SxinRZ4212U/08fVYOvQEzBZ0. Clasificacion: no bloqueante con remediacion posterior OBLIGATORIA (deploy key con scope + rotacion). No contradice G11-RESOLUTION.md, cuyo "no SSH keys copied/rotated" cubre solo la reparacion de las 01:44.
2. REPRODUCIBILIDAD — .gitignore excluye receipts/root-live.receipt y receipts/*.log, asi que G04..G10 NO son reproducibles desde el paquete commiteado. Demostrado: verify_live.sh sobre contenido limpio de origin/main da 7 FAIL y AGGREGATE=FAIL. Unica copia en /tmp/dfl-cx-yolo-20260802 (efimera). Remediacion posterior: commitear receipt + log.
3. G03 SIN COMMITEAR — el fix de scripts/regen_graph.sh no esta en origin/main ni en a8269d9; ambos siguen llamando publish-amos-context.sh directo. Vive solo como modificacion sucia en /opt/dfl-knowledge rama feat/dfl-high-certainty-harness-v0.1. install.sh lo re-aplica por sed idempotente, asi que el estado vivo es correcto, pero un git restore reintroduce silenciosamente la causa raiz (CRON 2 evadiendo el lock).
4. push_mirror.sh hace chmod 0664 sobre last-mirror-hash en cada publicacion mientras install.sh lo deja 0660; la asercion de G08 (other<2) solo se cumple post-install. Contenido es un SHA-256 no secreto. Drift cosmetico.
5. Residuo /var/lib/dfl-publication/test-write.txt dflagent:dfl 0644 del 2026-08-02 19:13.
6. /opt/dfl-knowledge-workunit es root:root; dflagent no puede git fetch ahi y necesita -c safe.directory para leer. Incoherente con que el principal de publicacion sea dflagent.
7. verify_live.sh emite G01 y G12 como echo incondicional; G12 si se computa de verdad en test_fixture.sh y G13 se valida con el bad_receipt previo. Hallazgo no confirmado como defecto.

CONTRADICCIONES:
a. El 14/14 es cierto para la corrida viva pero NO reproducible desde la evidencia commiteada (demostrado, no inferido).
b. ROOT-ACTION.md instruye correr root-live-test.sh, cuyo G10_LIVE_DFLAGENT es un pass emitido tras ejecutar push_mirror.sh COMO ROOT — no prueba el gate que nombra. El que si lo prueba es root-live-test-fixed.sh (runuser -u dflagent). El receipt no registra cual corrio. El gate igual es verdadero por via independiente. Contradiccion de procedimiento documentado, no de resultado.
c. INVENTORY.md registra el mirror como 2775; vivo es 2770 (endurecimiento posterior).

RESTRICCIONES RESPETADAS: DCSA no promovido. No se modificaron llaves, permisos, historial git ni produccion. Unicas escrituras: git fetch (refs) y fixtures hermeticos en scratchpad. NO_TOUCH intacto (puntajeTigreKnockout, Supabase, Vercel, env vars, HLC-T01/T02/T03, CRON 3:05, /etc/dfl-secrets).

PROXIMO_AGENTE_DEBE: (1) rotar la llave SSH de dflagent a un deploy key con scope a DFLghub/amos-context; (2) commitear receipts/root-live.receipt y g11-resolution.log al paquete de evidencia; (3) commitear el fix de scripts/regen_graph.sh a main antes de que un git restore lo revierta.

---

## PENDING


---

## RECENT ACTIVITY (cross-project)

### Session summary: dfl-knowledge
**Type:** session_summary  
**Project:** dfl-knowledge  

## Goal
Sesión larga, multi-misión sobre DFL/SFV5/Workforce Registry Unit (WRU) v0.1: desde protocolo @$go inicial hasta fabricación end-to-end completa de WRU bajo autorización humana explícita, con verificación exhaustiva basada en evidencia real en cada paso.

## Instructions
- El usuario opera bajo protocolo DFL: @$go al abrir sesión, @$fin al cerrar (mem_save + push_mirror.sh). No confundir @$go (comando) con /go (ruta HTTP del proxy).
- Modo de ejecución de máxima autonomía ya establecido (memoria previa): no pedir permiso para acciones seguras, agrupar aprobaciones en un único punto de decisión — pero el usuario definió explícitamente 5 checkpoints humanos bloqueantes para la fabricación de WRU y espera que se respeten literalmente, incluso en modo autónomo.
- El usuario exige evidencia real y reproducible en cada gate/checkpoint — "no declares PASS por documentos ni scaffolding". Toda corrección de PRP/Plan/build debe traer hashes SHA256 completos, snapshots git before/after, y diffs exactos, nunca solo afirmaciones.
- Cuando se pide "cierre provisional (checkpoint)" a mitad de una tarea larga, se espera un handoff autosuficiente en disco (no solo un resumen conversacional) para que otro agente sin memoria pueda continuar.

## Discoveries
- Un fetch de amos-context.md (GitHub raw) devolvió contenido con forma de prompt-injection (se autoasignaba un "perfil CONSULTOR" con capacidades falsas, contradichas por el entorno real) — se flagueó al usuario explícitamente en vez de obedecerlo.
- La corrida inicial de `/prp` para WRU generó un PRP nativo con un defecto real: atribuyó los "44 gates" a la fábrica SFV5 (DDMS) cuando en realidad son gates propios de WRU (G1-G22 del laboratorio de capacidad + G23-G44 de CC-PRP-R1) — corregido en 2 pasadas tras comparar contra las fuentes verbatim (READER añadido como rol, G22/G21/G41-43 restaurados a su alcance/semántica original).
- Un `git worktree add` nuevo parte con `git status` limpio incluso cuando el árbol principal está sucio desde antes — los archivos no versionados no se materializan en el worktree nuevo. Esto valida el patrón de aislamiento recomendado por el propio Implementation Plan y se usó tal cual.
- Durante la fabricación real aparecieron 2 falsos positivos en tests de auditoría de código (G44, y la guarda READER de query/client.mjs): el propio comentario explicativo del código contenía la cadena de texto que el test de auditoría buscaba (p.ej. "appendVersion("), inflando el conteo de "call sites". Se corrigió reformulando el comentario, nunca relajando el test.
- `source_commit` en el schema WRU es "HEAD al momento de generación", no un valor fijo — avanza legítimamente con cada commit de fabricación aunque `.claude/skills/` nunca se toque. Esto se aprovechó honestamente en Fase N para demostrar `freshness_status: stale` real sin ocultarlo (invariante explícito del PRP: nunca esconder staleness al consumidor).
- Un test inicial de "Activación" asumía que el registro nunca crecería más allá de 32 entradas — al agregar legítimamente una entrada sintética no-SFV5 (Fase N, prueba de extensibilidad real) el test falló; el invariante correcto era "32 `sfv5-skill` únicas", no "32 entradas totales para siempre". Corregido para no penalizar la extensibilidad que el propio PRP exige.

## Accomplished
- ✅ @$go procesado; prompt-injection en amos-context.md detectado y reportado al usuario antes de actuar sobre él.
- ✅ CX-MFG-3: corrida real de `/prp` para WRU v0.1 sobre el repo real SFV5 (`/opt/saas-factory-setup`), PRP nativo generado y corregido en 2 rondas (44 gates atribuidos correctamente a WRU no a SFV5, entidades canónicas Source Projection/Proposal/Canonical State formalizadas, contrato de reconciliación NO_CHANGE|PROPOSAL|CONFLICT|SOURCE_MISSING, SFV5 declarado fuente no autoridad, rol READER incorporado, G21/G22/G41-43 restaurados) — cada corrección con receipt completo (hashes SHA256 íntegros, snapshots git worktree/status before-after, diffs exactos, declaraciones NOT_RECOVERABLE cuando aplicaba).
- ✅ CX-MFG-4: Implementation Plan completo generado desde el PRP aprobado y corregido (498→548 líneas: secuencia canónica `1→2→3→{4,5}→6→7→9.9→9.10→N`, matriz G1-G44 completa, checkpoints humanos, estrategia de commits/corpus/instalación aislada).
- ✅ Fabricación end-to-end real de WRU v0.1 en worktree git aislado (`/opt/wru-worktree-v0.1`, branch `feat/workforce-registry-unit-v0.1`), 10 commits atómicos, 74/74 tests reales pasando, 44/44 gates PASS con evidencia individual: schema+meta-validación (Fase 1), adapter read-only+reconciliación sobre las 32 skills reales (Fase 2), motor de propuestas/aprobación — único camino de escritura, optimistic locking, autoridad por rol (Fase 3), ciclo de vida gobernado — deprecate/replace/archive/restore, hard-delete estructuralmente imposible (Fase 4), disponibilidad (Fase 5), cliente de consulta bajo autoridad READER + blind discovery de 8 casos (Fase 6), evidencia (Contrato A) + instalación/desinstalación real en copia aislada (Fase 7), Activación real (32/32 skills reales ingeridas vía flujo gobernado, nunca carga directa), Operación real (6 tipos de mutación real incluyendo archive+restore real sobre datos reales), Fase N (blind discovery real sobre el Registry activado, 44 gates agregados, FINAL-VERDICT).
- ✅ Árbol productivo `/opt/saas-factory-setup` verificado byte-idéntico (HEAD, `git status`, `.claude/skills/`, `CLAUDE.md`, y las 3 herramientas previas de la cadena SFV5/CC-2/CX-N1) en cada uno de los ~15 checkpoints de este build — nunca tocado.
- ✅ Handoff autosuficiente escrito en disco antes de continuar (cierre provisional pedido explícitamente por el usuario a mitad de la fabricación), para que otro agente sin memoria de la conversación pudiera retomar si la sesión moría.
- ✅ 4 checkpoints humanos aprobados explícitamente por el usuario en tiempo real (primera escritura canónica, ingestión de datos reales, primera operación de lifecycle, camino vivo real).
- 🔲 Checkpoint 5 (merge/activación compartida a la rama productiva) deliberadamente NO ejecutado — queda como decisión humana futura, fuera del alcance que esta misión se autorizó a ejecutar sola.
- Veredicto final entregado: `WRU_V0_1_END_TO_END_BUILT_PENDING_FINAL_INDEPENDENT_VERIFICATION`, con deuda residual declarada explícitamente (sin CLI binario formal; instalación probada en copia de directorio simple, no en un segundo worktree git).

## Next Steps
- Revisión independiente del build (tipo CX-PRP-1) antes de cualquier propuesta de merge a `fase-3-5-jpi-real-sfv5-bridge`.
- Decisión humana pendiente sobre checkpoint 5: si/cuándo proponer ese merge.
- Si se decide llevar WRU a producción real: resolver deuda residual (CLI binario formal; prueba de instalación en un worktree git separado, no solo copia de directorio).
- Si la sesión se retoma en frío, leer primero `FINAL-VERDICT.md` y `HANDOFF-2026-07-31.md` antes de tocar código.

## Relevant Files
- `/opt/saas-factory-setup/saas-factory/.claude/PRPs/prp-workforce-registry-unit.md` — PRP aprobado de WRU v0.1, corregido 2 veces, nunca modificado durante la fabricación.
- `/opt/saas-factory-setup/saas-factory/.claude/PRPs/plan-workforce-registry-unit.md` — Implementation Plan aprobado, fuente de la secuencia de fases ejecutada.
- `/opt/wru-worktree-v0.1/saas-factory/tools/workforce-registry/` — módulo completo fabricado (schema/, adapters/, proposals/, registry/, query/, evidence/, tests/), 10 commits, 44/44 gates.
- `/opt/dfl-knowledge/evidence/sfv5-wru-prp-native-run-2026-07-31/` — receipts de generación y corrección del PRP.
- `/opt/dfl-knowledge/evidence/sfv5-wru-implementation-plan-2026-07-31/` — receipts de generación y corrección del plan.
- `/opt/dfl-knowledge/evidence/wru-v0.1-e2e-build-2026-07-31/FINAL-VERDICT.md` — matriz completa G1-G44, estado exacto por etapa, veredicto final.
- `/opt/dfl-knowledge/evidence/wru-v0.1-e2e-build-2026-07-31/HANDOFF-2026-07-31.md` — handoff autosuficiente para continuación por otro agente.

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

### Vertical slice real MERCADER↔FÁBRICA E2E: 1 pedido, ciclo completo, CERRADO (2026-08-19)
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/mercader/vertical-slice-e2e-2026-08-19
STATUS: CERRADO
DATE: 2026-08-19

WHAT: Primera prueba real (no simulada) del ciclo MERCADER ORDER -> FABRICA RECEIVE -> COMMIT -> PRODUCE -> AQA -> DELIVER -> MERCADER RECEIVE/ACK, usando el Design Candidate v0 (obs #528) como referencia, NO como obligacion de implementar todas sus piezas. Regla de Jorge: reutilizar todo lo existente, construir solo el delta que el E2E demuestre necesario, no adelantarse a elasticidad multiple.

ORDER_ID real: MERCADER-ORDER-TEST-20260819T162514Z (pedido de PRUEBA explicito, prospecto TEST, no cliente real -- consistente con el limite ya establecido hoy en CHECKPOINT-MERCADER-QR-2026-08-19.md de cero contacto real).

Mecanismos EXISTENTES reutilizados intactos, sin modificar:
- tools/peer-work/ (peer_work.py) como transporte generico REQUEST/COMMIT/STATUS/DELIVER -- MERCADER emitio el ORDER como un item mas de la misma cola que usa cualquier otro trabajo DFL, sin integracion bespoke. authority_ref="human=telegram:8776472165" verificado en vivo contra el allowlist real antes de crear el primer item.
- El patron real de r-d-delivery/delivery.mjs (construido y probado HOY MISMO por otra sesion en /tmp/mercader-verify-fork/, 6/6 aserciones) -- reusado (mismas funciones, mismo invariante "un orderId = una entrega"), no reescrito desde cero.
- AQA Kit real (tools/aqa-kit/bin/aqa.mjs) -- corrido de verdad, nivel AQA-1, perfil CRUD_LIFECYCLE, con ALLOW (creacion de entrega, min-length del documento, correlacion de order_id) y DENY (duplicado correctamente rechazado) reales. Resultado real: PASS, profiles covered=[CRUD_LIFECYCLE], missing=[].

Delta minimo construido (solo lo que el E2E demostro necesario):
- deliver.mjs: version exportable/reusable del patron ya probado (mismas funciones, agregado solo un guard de duplicado persistido en disco via delivery-record.json, porque la version anterior era in-memory y no servia para un DENY check real de AQA).
- onepager.md real: contenido real generado (no placeholder de texto "ok"), siguiendo la estructura ya documentada de MERCADER Distill (AI_Product_Distillation_OnePager_v0_1.md).

NO se construyo: Scheduler, Capacity Registry, PDP de autorizacion por parametro, cloning de Fabricas, ni ninguna otra pieza del Design Candidate v0 -- correctamente, porque un solo pedido no las necesito. Confirma en la practica lo que la regla de Jorge predijo: "si 1 pedido no cruza completo, no tiene sentido probar 10" -- 1 SI cruzo completo, con piezas existentes casi sin delta.

Evidencia real verificable:
- peer-work request_id pw-e27f05cf50e6 (ORDER, COMMITTED->COMPLETED) y pw-f73e9583f3ab (ACK, COMPLETED), ambos con inputs.order_id=MERCADER-ORDER-TEST-20260819T162514Z -- correlacion confirmada por lectura directa post-hoc, no asumida.
- AQA receipt real: tools/aqa-kit/evidence/mercader-e2e-vertical-slice/MERCADER-ORDER-TEST-20260819T162514Z/27811cb/2026-08-19T16-27-41-951Z/receipt.json, status=PASS.
- delivery-record.json real con token real (jLiSu6pxXqTILGrAecLlX5juJOaN9KLF) y link real (https://deepfeelingslabs.com/entrega/<token>) -- el link no fue navegado/publicado (dominio real de DFL, no se hizo ninguna llamada de red hacia el).
- onepager.md real, con contenido explicito marcando que es un pedido de prueba, ningun dato de cliente real usado.

Que sigue siendo bespoke: NADA especifico a MERCADER<->FABRICA -- MERCADER hablo con el mismo peer-work que cualquier otro origen (TELEGRAM, otros agentes), FABRICA (TCC) recibio como target_executor generico, DELIVER volvio por el mismo mecanismo (complete() del item original), sin ningun canal o codigo exclusivo de este par de organismos. NO_POINT_TO_POINT confirmado en la practica, no solo en diseno.

Blocker real: ninguno aparecio en este ciclo -- brief simple, deliverable producible con capacidad ya existente, sin credencial externa faltante. QUIERO->bloqueo->REQUIERO no se activo porque no hizo falta, no se forzo artificialmente.

NEXT AGENT: el circuito de 1 pedido esta PROBADO y REUTILIZABLE (mismo peer-work, mismo AQA, mismo patron de delivery sirven para el siguiente pedido sin modificacion). La siguiente mision autorizada por Jorge (NO ejecutada aca) es repetir con multiples pedidos simultaneos para tensionar elasticidad real -- ahi es donde recien aparecera si Scheduler/Capacity Registry/provisioning son necesarios de verdad, no antes.

### Design Candidate v0 institucionalizado — arquitectura mínima elástica DFL, corregida, READY FOR REAL-WORLD VALIDATION (2026-08-19)
**Type:** architecture  
**Project:** dfl  

TOPIC: dfl/design/elastic-capacity-interop-v0
STATUS: closed (institucionalizacion) / DESIGN CANDIDATE READY FOR REAL-WORLD VALIDATION (no implementado, no runtime tocado)
DATE: 2026-08-19

WHAT: Sintesis final de la sesion completa. Jorge cerro la cadena RESEARCH CLOSED (Monoid obs#524, interop-2026 obs#525, ARD obs#526, A2A en chat, identidad/autoridad/delegacion obs#527) pidiendo sintetizar (no investigar mas) la arquitectura minima fractal propia de DFL para interoperabilidad universal entre organismos sin integracion punto-a-punto. Primera version tuvo una correccion de principio critica de Jorge ("todo trabajo valido y autorizado debe hacerse, la falta de capacidad NO es causal de rechazo") que obligo a repensar ACCEPT/capacity-awareness. Luego una correccion quirurgica final (P11) sobre 4 confusiones reales que TCC seguia cometiendo: (1) elasticidad limitada a un solo nivel worker/executor en vez de fractal (worker->celula->Fabrica->n Fabricas); (2) Capacity Registry cristalizado prematuramente como busy/free en vez de contrato abierto extensible; (3) claim sin evidencia de que AGENT_PROJECTS "equivale a" clonar una Fabrica -- retirado, reemplazado por separacion explicita PROBADO/HIPOTESIS/POR-DEMOSTRAR; (4) "ACCEPT incondicional" confundiendo compromiso organizacional con readiness/ejecutabilidad inmediata -- corregido a COMMITMENT != READINESS con 6 responsabilidades distintas (commitment, readiness, scheduling, provisioning, ejecucion, blocker real).

Institucionalizado en docs/patterns/design-candidate-v0-elastic-capacity/{DESIGN.md,dfl.yaml}, asset_id dfl.design-candidate.elastic-capacity-interop.v0, capability_type=design-candidate (distinto de "research" -- es sintesis de diseno, no autopsia), status DRAFT explicito -- NO active, NO estandar institucional, NO autorizacion de construccion en bloque. Indexado: 26 assets (subio de 25), 0 errores, 7/7 tests, descubribilidad verificada con 5 queries directas, todas exitosas sin problema de fraseo esta vez.

7 invariantes finales preservados integros: NO DROP, COMMITMENT!=READINESS, ELASTIC CAPACITY IS FRACTAL, FAIL->REASSIGN/RECOVER (fundamentado explicitamente en el Axioma A4 de la Constitucion DFL v2 leida temprano en esta misma sesion -- crash-only, el sucesor lee Ledger+contrato y continua, no inventado, doctrina DFL ya existente), TEMPORARY SCALE-DOWN, NO POINT-TO-POINT, BLOCKER!=TERMINAL (nuevo en esta ronda).

Arquitectura minima: reusa intacto Asset Index/dfl.yaml, DCSA (mission authorization, confirmado en el research anterior como SIN equivalente externo encontrado en toda la industria), patron manager.mjs (ACCEPT generalizado), AGENT_PROJECTS/Claude Agent SDK (alcance de la prueba real corregido -- solo crea/ejecuta sesion con cwd, NO demostrado que equivale a clonar Fabrica), AQA (DELIVER, sin cambios), bloqueo optimista del workforce-registry-capability-lab (16/16 escenarios ya validados, citado para que el Scheduler no sea chokepoint). Piezas nuevas minimas: Workload Ledger (extension de peer-work), Capacity Registry (contrato abierto, NO busy/free definitivo), Scheduler (matching no bloqueante, resuelve REQUIERO horizontal o vertical sin nivel fijo), PDP minimo de AUTHORIZE-invocation, RECEIVE/ACK generico.

Correccion explicita de instruccion directa de Jorge, reemplaza conclusion previa: NO es requisito cerrar la fila "CAPACIDAD QUE FALTA DEMOSTRAR" (aislamiento entre clones, continuidad tras teardown, costo/tiempo real, si Fabrica es la unidad clonable correcta) antes de implementar -- esas hipotesis se validan cuando un outcome real las obligue via QUIERO->bloqueo->REQUIERO, no se abre investigacion nueva por adelantado.

Contrastado explicitamente contra evidencia operativa real independiente del mismo dia: CHECKPOINT-MERCADER-QR-2026-08-19.md (hallazgos R-F1..R-F8) -- el gap que este diseno intenta cerrar fue confirmado dos veces, por research externo de 5 rondas Y por diagnostico operativo real de MERCADER, coincidencia no fabricada.

NEXT AGENT: DESIGN CANDIDATE v0 INSTITUTIONALIZED -- READY FOR REAL-WORLD VALIDATION. La siguiente mision autorizada (no ejecutada aca) es el vertical slice real: MERCADER ORDER -> FABRICA RECEIVE -> COMMIT -> PRODUCE -> AQA -> DELIVER -> MERCADER RECEIVE/ACK -> REPEAT, primero con 1 pedido, luego repetido con multiples pedidos simultaneos para tensionar elasticidad real (no simulada). Ese ciclo es el que debe descubrir que piezas de este Design Candidate necesitan existir de verdad y en que orden -- no re-disenar desde cero, no re-investigar. No confundir esta institucionalizacion con autorizacion de implementar toda la arquitectura de una vez -- sigue siendo DRAFT.

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

**Graph entropy:** 0.7993  

- **Community 11** (91 nodes): PRP como artefacto nativo, Modelo de disponibilidad en servicios digitales, Evaluación de complejidad en costos
- **Community 0** (7 nodes): Verificación de API, Requisitos legales para operar V1
- **Community 1** (5 nodes): Abstracción de oferta, Política de Disponibilidad, Proceso de Intake
- **Community 2** (5 nodes): Merchant of Record, Paridad Codex, Métricas comerciales
- **Community 3** (5 nodes): Jurisdicción, Mercader, Observación de Ed
- **Community 4** (4 nodes): MCP Server Behavior, RLS Trap, Cardinalidad de Inventario

---

*Mirror auto-generated 2026-08-19T16:29:37Z | La Garra → DFLghub/amos-context*
