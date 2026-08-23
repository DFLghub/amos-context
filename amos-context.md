# amOS Context — @$go Live Mirror
**Generated:** 2026-08-23T15:23:43Z  
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

### Paseo TCX Full Access profile configured and verified
**Type:** decision  
**Project:** saas-factory-setup  

Cierre @$fin. En VM2/PASEO_HOME=/home/dflagent/.paseo-sfv5-dev se configuró el agent profile dedicado de Paseo id=tcx-full-access, name='TCX — Full Access', provider=codex, model=gpt-5.5, modeId=full-access. La fuente instalada de Paseo 0.5.0-beta.5 confirma que full-access materializa approval_policy=never y sandbox_mode=danger-full-access. `paseo reload --format json` aplicó daemon.agentProfiles sin restartRequiredPaths. Verificación directa vía API local confirmó el perfil y los modos Codex (auto, auto-review, full-access). No se lanzó sesión desde Pixel, no se modificaron credenciales ni el provider global Codex. Próximo paso para Jorge: cerrar/reabrir Paseo en Pixel, abrir saas-factory y seleccionar TCX — Full Access; no seleccionar Codex genérico.

### JPI tiene casa Git propia: DFLghub/transportes-eventos-jpi (2026-08-21)
**Type:** decision  
**Project:** dfl  

Follows obs #566. Jorge creó DFLghub/transportes-eventos-jpi (privado, vacío) en GitHub -- yo no pude crearlo (sin gh CLI, sin token API vigente). Diagnostiqué el mecanismo de credencial real (SSH key ~/.ssh/id_ed25519, identidad efectiva "DFLghub" confirmada via `ssh -T git@github.com` -> "Hi DFLghub!", no un deploy-key por-repo) antes de reportar el bloqueo -- el repo simplemente no existía todavía en el primer intento, confirmado después con el mismo comando tras la creación.

Ejecutado, todo verificado desde remoto, no asumido:
- `git remote rename origin legacy-360eventos` (preserva referencia histórica intacta, sin tocarla)
- `git remote add origin git@github.com:DFLghub/transportes-eventos-jpi.git`
- `git push -u origin feat/jpi-fase-5-real-runtime-v0.1` -- push limpio, sin merge/rebase/rewrite
- Verificado: HEAD local == HEAD remoto (cde6664 antes de la actualización de dfl.yaml, luego 95ae0ae), upstream configurado correctamente (`origin/feat/jpi-fase-5-real-runtime-v0.1`), working tree limpio (0 cambios), historia completa preservada (46 commits, confirmado que el commit de bootstrap original "feat: bootstrap inicial 360Eventos" y el tag jpi-phase-1-closed son ancestros reales de la rama pusheada -- no es una historia truncada/squasheada)
- 360eventos (legacy-360eventos remote) verificado sin tocar: HEAD sigue en 2a1efe2 (Phase 4), ramas intactas

Actualicé la única referencia institucional necesaria: /opt/jpi/dfl.yaml, asset_id cambiado de `dfl.legacy.jpi-360eventos` (que literalmente conflacionaba JPI con el demo archivado) a `dfl.jpi.transportes-eventos`, con `residence` apuntando al nuevo repo canónico y `consumer_hint` apuntando a esta misma memoria institucional para que una futura sesión no tenga que redescubrir el historial desde cero. Commiteado (95ae0ae) y pusheado al nuevo repo. Índice central de asset-index regenerado y commiteado localmente en saas-factory-setup (commit b5d864d en fase-3-5-jpi-real-sfv5-bridge, no pusheado -- misma razón que el resto de esta sesión en esa rama: estado sucio previo no relacionado).

No se tocó producto, no se metabolizó business-os/FMD/amOS/Outcome-Engine, no se tocó Supabase/secrets, no se hizo merge/rebase/rewrite de historia.

NEXT REQUIERO real pendiente (no de esta misión, la siguiente etapa): decidir el destino de las capacidades de business-os/ según la matriz ya producida (obs #565), ahora que JPI tiene casa Git propia y checkpoint remoto seguro para trabajar sobre él.

### JPI .git ownership fixed + 63/64 uncommitted changes preserved in 6 organized commits (2026-08-21)
**Type:** decision  
**Project:** dfl  

Follows obs #562-565. Jorge ran `sudo chown -R dflagent:dfl /opt/jpi/.git` himself (I have no sudo, confirmed). Verified: 227 root-owned files inside .git -> 0 after. Working-tree files outside .git that were root-owned (several .md reports, scripts/jpi-domain-term-guard.mjs, business-os/server.js) were already world-readable (rw-r--r--), so they never blocked git add/commit -- only .git internals did. The scoped fix (.git only) was correct and sufficient, verified before executing, not assumed.

Integrity verified post-chown: git status showed the same 64 changes as before (63 original + my own NOTICE file), HEAD unchanged at a241ef5, `git fsck --full --no-dangling` returned clean (zero real corruption; the --full run alone showed ~50 harmless dangling objects from an earlier aborted `git add`+`git reset` attempt, normal and inert).

Preserved everything in 6 separate, real commits (no mass/generic commit), each with clear provenance, nothing deleted, nothing "cleaned":
1. fcb7c51 -- Rubén/JPI discovery documentation (docs/case-zero/, docs/discovery/, docs/mvp-v2/) -- real interview/discovery evidence with Rubén.
2. 82d3424 -- real, tested domain/product logic: the PRECOTIZACION-eradication work (scripts/jpi-domain-term-guard.mjs, its test, package.json wiring, src/features/jpi/domain/states.mjs) matching Engram obs #370's report that was never actually committed until now, plus domain/ (decision log, factory defaults, open questions, Rubén's blocking-questions packet, the NO_INTERMEDIATE_STAGE canonical decision, ontology/runtime CSVs), docs/functional/, docs/business/.
3. f5c7256 -- the root dfl.yaml asset-index manifest (already indexed/discoverable, just never committed).
4. db68962 -- business-os/ historical WIP unrelated to the amOS thread (factory-request.js, fmd-runtime-factory-bridge test diff, an intent->outcome-mapping exploration with its own migrations 998/999, a Codebase Intelligence wrapper+test+dfl.yaml that happens to live nested here, several e2e test explorations) -- verified via grep that none of these are referenced by the amOS/outcome-analysis chain, kept genuinely separate.
5. a88d44e -- WIP amOS + Outcome Analysis Engine, kept together deliberately: verified via grep that runtime.js's diff requires() all 5 new fmd/ files (amos-context-loader, outcome-verifier, correction-strategies, factory-outcome-recorder, outcome-pattern-analyzer which itself requires outcome-analysis-engine) as one entangled unit -- splitting it would have misrepresented the real state, so committed as one intertwined WIP commit instead of forcing an artificial "amOS-only" split.
6. cde6664 -- business-os/NOTICE-DEPRECATED.md, the recovery-analysis documentation written earlier this session.

No UNKNOWN bucket was needed -- every file had clear, evidence-backed provenance, nothing ambiguous survived triage.

Working tree is now 100% clean (`nothing to commit, working tree clean`). Branch `feat/jpi-fase-5-real-runtime-v0.1` has no upstream tracking configured and does not exist on origin (`DFLghub/360eventos.git`) at all -- confirmed via `git rev-parse @{u}` failing and no matching `origin/...` ref. Per explicit instruction and this real ambiguity, did NOT push.

No product behavior changed, no tests run/fixed, no business-os/ code modified beyond adding the already-existing NOTICE-DEPRECATED.md, no Supabase/secrets touched, no metabolization work done -- purely a preserve-and-organize mission, exactly as scoped.

NEXT REQUIERO: decide whether/how to push this branch (a fresh branch on origin, or merge target) -- that's a real open question given no upstream exists, not something to assume. Everything else from here is metabolization work (deciding FMD/little-bosses/amOS/Outcome-Engine fates per the capability matrix in obs #565), explicitly out of scope for this preserve-only mission.

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

### JPI Factory-native adaptation mission — map + real git-permission blocker found (2026-08-21)
**Type:** architecture  
**Project:** dfl  

Follows obs #562-564 (canonical checkpoint + Jorge's 2 corrections on why JPI paused / who built it). Jorge commissioned the actual adaptation mission: PRESERVAR PRODUCTO -> ADAPTAR MECANISMO, explicitly not a rewrite, not chasing 273/273 tests, no new Eventos/Transportes features.

Classification result: business-os/ (the whole FMD/little-bosses/factory-adapter-registry/amOS system) = F. OBSOLETE/SUPERSEDED mechanism, not product. Its own CLAUDE.md self-describes as a "piloto soberano" -- deliberately separate Express+SQLite process, isolated from the real Next.js host and real Supabase. Zero PRPs, zero .claude/memory entries for it -- confirms it was built entirely off-Factory, exactly matching why-JPI-paused reasoning. 100% of 65 failing tests are inside business-os/; ZERO failures anywhere in the actual product (src/, Next.js app, domain model) -- the real product is healthy.

Frontend/product layer confirmed genuinely Factory-native already: package.json name is unmodified template default "saas-factory-app", full .claude/skills/ (30 skills) present, 2 real historical PRPs (PRP-001-landing-publica, PRP-002-auth-ui) prove the Next.js/auth layer was built through the proper skill process. The off-Factory divergence is business-os/ only, not the whole project -- an important correction to any assumption that "all of JPI" needs re-platforming.

amOS WIP verdict: RETIRE the code (goes with its host mechanism), PRESERVE the intent as documentation. Added business-os/NOTICE-DEPRECATED.md (new file, uncommitted, explains the situation and points at this memory) rather than silently deleting or silently continuing to build on it.

REAL NEW BLOCKER FOUND (not previously known precisely): git add/commit in /opt/jpi fails with "error: insufficient permission for adding an object to repository database .git/objects". 228 files inside .git itself (including .git/HEAD) are root-owned -- not just working-tree debris as the 2026-08-04 CASA LIMPIA diagnostic assumed (that diagnostic only knew about root-owned files blocking `rm`, not that .git internals themselves were also root-owned, blocking ALL commits). NO commit of any kind is possible in this repo until `sudo chown -R dflagent:dfl /opt/jpi/.git` runs as root. Nothing lost attempting it -- git add partially staged, cleanly `git reset`, working tree verified unchanged.

This is the single next REQUIERO to unblock everything else in JPI: Jorge runs that one chown command, then a future session can commit the already-triaged preserved work in organized separate commits (docs/domain-knowledge group -- Rubén discovery + domain model; tested-code group -- the PRECOTIZACION-eradication script+test+package.json wiring, already passing 3/3; unrelated-asset-manifest group -- business-os/lib/dfl.yaml for an unrelated Codebase Intelligence capability that happens to live nested there; business-os-mechanism-WIP group -- the amOS files + other business-os/ changes, committed as clearly-labeled historical/experimental, not deleted).

No Supabase/secrets touched. No new Eventos/Transportes features. No rewrite performed. Full detail in jpi-canonical-checkpoint-2026-08-21.md § 7 (local memory).

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

### Paseo TCX Full Access profile configured and verified
**Type:** decision  
**Project:** saas-factory-setup  

Cierre @$fin. En VM2/PASEO_HOME=/home/dflagent/.paseo-sfv5-dev se configuró el agent profile dedicado de Paseo id=tcx-full-access, name='TCX — Full Access', provider=codex, model=gpt-5.5, modeId=full-access. La fuente instalada de Paseo 0.5.0-beta.5 confirma que full-access materializa approval_policy=never y sandbox_mode=danger-full-access. `paseo reload --format json` aplicó daemon.agentProfiles sin restartRequiredPaths. Verificación directa vía API local confirmó el perfil y los modos Codex (auto, auto-review, full-access). No se lanzó sesión desde Pixel, no se modificaron credenciales ni el provider global Codex. Próximo paso para Jorge: cerrar/reabrir Paseo en Pixel, abrir saas-factory y seleccionar TCX — Full Access; no seleccionar Codex genérico.

### JPI tiene casa Git propia: DFLghub/transportes-eventos-jpi (2026-08-21)
**Type:** decision  
**Project:** dfl  

Follows obs #566. Jorge creó DFLghub/transportes-eventos-jpi (privado, vacío) en GitHub -- yo no pude crearlo (sin gh CLI, sin token API vigente). Diagnostiqué el mecanismo de credencial real (SSH key ~/.ssh/id_ed25519, identidad efectiva "DFLghub" confirmada via `ssh -T git@github.com` -> "Hi DFLghub!", no un deploy-key por-repo) antes de reportar el bloqueo -- el repo simplemente no existía todavía en el primer intento, confirmado después con el mismo comando tras la creación.

Ejecutado, todo verificado desde remoto, no asumido:
- `git remote rename origin legacy-360eventos` (preserva referencia histórica intacta, sin tocarla)
- `git remote add origin git@github.com:DFLghub/transportes-eventos-jpi.git`
- `git push -u origin feat/jpi-fase-5-real-runtime-v0.1` -- push limpio, sin merge/rebase/rewrite
- Verificado: HEAD local == HEAD remoto (cde6664 antes de la actualización de dfl.yaml, luego 95ae0ae), upstream configurado correctamente (`origin/feat/jpi-fase-5-real-runtime-v0.1`), working tree limpio (0 cambios), historia completa preservada (46 commits, confirmado que el commit de bootstrap original "feat: bootstrap inicial 360Eventos" y el tag jpi-phase-1-closed son ancestros reales de la rama pusheada -- no es una historia truncada/squasheada)
- 360eventos (legacy-360eventos remote) verificado sin tocar: HEAD sigue en 2a1efe2 (Phase 4), ramas intactas

Actualicé la única referencia institucional necesaria: /opt/jpi/dfl.yaml, asset_id cambiado de `dfl.legacy.jpi-360eventos` (que literalmente conflacionaba JPI con el demo archivado) a `dfl.jpi.transportes-eventos`, con `residence` apuntando al nuevo repo canónico y `consumer_hint` apuntando a esta misma memoria institucional para que una futura sesión no tenga que redescubrir el historial desde cero. Commiteado (95ae0ae) y pusheado al nuevo repo. Índice central de asset-index regenerado y commiteado localmente en saas-factory-setup (commit b5d864d en fase-3-5-jpi-real-sfv5-bridge, no pusheado -- misma razón que el resto de esta sesión en esa rama: estado sucio previo no relacionado).

No se tocó producto, no se metabolizó business-os/FMD/amOS/Outcome-Engine, no se tocó Supabase/secrets, no se hizo merge/rebase/rewrite de historia.

NEXT REQUIERO real pendiente (no de esta misión, la siguiente etapa): decidir el destino de las capacidades de business-os/ según la matriz ya producida (obs #565), ahora que JPI tiene casa Git propia y checkpoint remoto seguro para trabajar sobre él.

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

**Graph entropy:** 0.6356  

- **Community 11** (99 nodes): Modelos de disponibilidad en servicios digitales, Evaluación de complejidad en costos, Dependencias de Fabricación
- **Community 0** (7 nodes): Verificación de API, Estrategia PRP
- **Community 1** (5 nodes): Jurisdicción, Mercader, Observación de Ed
- **Community 2** (4 nodes): MCP Server Behavior, RLS Trap, Cardinalidad de Inventario
- **Community 3** (4 nodes): Cost Drivers in SFV5, PRP Structure and Dependencies, Onboarding Capability Deficiencies
- **Community 4** (4 nodes): Plataforma Universal, ESC (Ed Square Cars), Patrón de Tenencia Owner-Scoped

---

*Mirror auto-generated 2026-08-23T15:23:43Z | La Garra → DFLghub/amos-context*
