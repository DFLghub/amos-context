# amOS Context — @$go Live Mirror
**Generated:** 2026-08-29T05:45:36Z  
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

### Cierre 2026-08-25 — Realtor no validado; patrón operativo recuperado de RSVP y JackyClean
**Type:** checkpoint  
**Project:** dfl  

CIERRE DE SESIÓN — 2026-08-25

Estado honesto:
- whatsapp-realtor-mvp NO queda validado como producto operativo ni READY.
- La autenticación/OWNER de Realtor no fue demostrada de forma confiable contra el runtime público actual; no debe usarse el PASS histórico como prueba suficiente.
- No se cambiaron código, credenciales, Vercel, Neon ni otros proyectos durante esta revisión final.

Corrección metodológica:
- Fue incorrecto revisar Realtor para resolver el problema de Realtor: era evidencia circular.
- Las referencias operativas relevantes son RSVP y JackyClean.

Evidencia recuperada:
- RSVP: /opt/dfl-products/event-rsvp-waitlist, repo canónico github:DFLghub/event-rsvp-waitlist@main, producción event-rsvp-waitlist.vercel.app, persistencia Supabase. IRONMAN rows 93-95 lo registran como cierre con verificación de producción/AQA y correcciones adversariales reales. dfl.yaml confirma residencia externa y operación independiente.
- JackyClean: /home/dflagent/worktrees/jackyclean, repo remoto DFLghub/saas-factory-setup, branch project/jackyclean. Vercel linkage local confirma projectName jackyclean, projectId y orgId del team DFL. Persistencia Neon/Postgres mediante DATABASE_URL y un boundary único en saas-factory/src/lib/db/client.ts. IRONMAN row 115 registra ciclos reales de operación, autoridad OWNER/ADMIN, configuración y verificación E2E.
- AQA/evidence de Realtor existe, pero el valid-auth histórico usó credenciales inyectadas localmente en el proceso; no prueba por sí solo que Vercel Production usara esos mismos valores.

Engram:
- búsquedas exactas de Realtor/auth, RSVP y JackyClean no devolvieron observaciones específicas útiles; la evidencia válida está en IRONMAN, handoffs, dfl.yaml y artefactos AQA.

Próximo enfoque:
1. Usar RSVP/JackyClean como patrón de comparación.
2. Verificar Realtor únicamente desde source canónico -> deployment -> runtime real -> browser/HTTP externo -> AQA.
3. No declarar PASS/READY por auth local, variables inyectadas, build o receipts históricos aislados.
4. No repetir investigación circular ni tocar credenciales sin necesidad.

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

### P11 JPI — manuales y handoff persistente cerrados
**Type:** fact  
**Project:** jpi  

CIERRE 2026-08-28. Se entregaron /opt/jpi/docs/MANUAL-USUARIO-JPI.md y MANUAL-PRODUCTO-JPI.md, contrastados contra runtime real. Se incluyó docs/screenshots/jpi-login.png, captura real de login. Verificaciones: npm run test:jpi PASS (7 pruebas + jpi entry-model), typecheck PASS, build PASS, revisión documental PASS. Documentación distingue superficies actuales (login/roles/panel/perfil/escalaciones/catalogo/productos/precios/inventario/activos/Venue/Asset Types/Asset Instances/ofertas/logout) de pendientes UI (Booking/calendario/Event Packages/flujo UI de cotización/visor de auditoría/horarios/depósitos/reglas/fotos). Estado honesto del catálogo: fuente física disponible, 10 ofertas verificables, 78 esperadas, desglose completo ausente; no inventar faltantes; PRICE_ON_REQUEST nunca es gratis. Se dejó handoff persistente en /opt/jpi/docs/P11-JPI-HANDOFF-2026-08-28.md. Seguridad preservada: sin Supabase, Vercel, pagos, deploy, producción ni exposición de credenciales.

---

## ACTIVE CONSTRAINTS — DO NOT TOUCH WITHOUT PRP

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

### Event RSVP MVP — full session close: blind AQA, Back/Forward fix, José delegated-admin grant, lifecycle-gap diagnostic, handoff sent (2026-08-17)
**Type:** decision  
**Project:** dfl  

TOPIC: dfl/event-rsvp/session-close-2026-08-17-tcc-post-jose-remediation
STATUS: closed
DATE: 2026-08-17
PRECEDENCIA: C
AUTHORITY: evidence only
LIFECYCLE: active
CONFIDENCE: high

WHAT: Single long TCC session, four threads, all closed, no open blockers for the José handoff.

1) Blind adversarial AQA (Playwright + OWASP ZAP, not seeded with José's known findings, per Jorge's explicit "keep it simple, minimum tools" instruction): Playwright 24 checks (auth abuse, direct-Supabase-REST IDOR/bypass attempts, business-logic bounds, stored XSS, 10-way concurrent RSVP race) + ZAP baseline+full active scan (135+ rules incl. SQLi/XSS/SSRF/SSTI/RCE/Log4Shell/Spring4Shell) = 0 injection/RCE findings, RLS defense-in-depth confirmed on every bypass attempt. One real NEW bug found (not on José's list): signup with a taken username silently "succeeded" (redirected to /events) while the actual DB insert failed, leaving an orphaned profile-less authenticated session — root cause was auth.signUp() opening a session before the users-table uniqueness check could fail, and a page-level useEffect reacting to that transient truthy `user` state. Fixed (signOut in the error path + stopped reacting to global auth state for the signup redirect) + added CSP/X-Content-Type-Options/X-Frame-Options/Permissions-Policy headers. Verified live post-deploy. Commit 112ab97.

2) Multi-tab auth investigation (Jorge's real Firefox report: 2 tabs, logout in one deauthed both, a reopened tab showed authenticated then deauthed): reproduced exactly via Playwright pages sharing one BrowserContext. Confirmed root cause: single shared localStorage session per browser origin/profile + GoTrue's own cross-tab storage-event sync — this is expected upstream Supabase behavior, NOT a bug. The 6-8 independent router.push-based auth-guard useEffects reacting uncoordinated to that shared state IS implementation-level amplification, diagnosed but explicitly NOT fixed in that pass per Jorge's scope (diagnosis only, no new investigation lines).

3) Back/Forward bug (found via Jorge's real repro description, fixed for real this time): Back into /login or /signup while authenticated ate the Back action and bounced straight back to where you started — root cause was those two pages redirecting away from themselves the instant global `user` was truthy, including when reached via popstate (browser Back), which is a real implementation bug distinct from #2. Fix: /login and /signup now render a static non-navigating "already signed in" panel instead of an effect-driven redirect; all 5 protected-route guards (+root "/") switched router.push('/login') to router.replace('/login') so a lost-auth redirect never leaves a trapped history entry. All 7 required scenarios (Back/Forward authenticated, normal nav, refresh, logout, Back after logout, direct access to protected route after logout) verified PASS live in production.

4) José capability grant: JoseIncer (confirmed is_admin=true, is_owner=false in production) can now execute remove_demo_data() (DEMO->REAL transition) — migration 018 widened that one RPC from owner-only to any-admin, with an explicit is_owner=false added to its own DELETE as defense in depth. Every other owner/delegated-admin boundary from migration 015 re-verified untouched via real auth.uid() simulation against the LIVE functions/triggers (not mocks, not assumptions): José structurally cannot grant/revoke admin, disable the owner or himself, or change is_owner through any authenticated-role path. NOTE (transparency, self-caught error): a WITH-CTE test harness bug (an unreferenced `select set_config(...)` CTE got planner-pruned and never executed) made one is_owner-bypass test appear to succeed against AdminDFL's real row; caught within ~1 minute, reverted immediately, re-verified correctly with a DO $$ block confirming the real trigger blocks it — root cause was the test methodology, not the app. Documented honestly rather than hidden.

5) Deploy incident (real, separate from all of the above): the GitHub->Vercel webhook for event-rsvp-waitlist silently stopped firing — two full pushes (17c09f5, f022acd) sat completely unbuilt for 50+ minutes, confirmed via `vercel ls` showing the most recent deployment was still 2h old (not just a slow build, zero new deployments triggered at all). Resolved by an interactive `vercel login` device-flow (Jorge authorized twice, first code expired unused) followed by a direct `vercel --prod` deploy, bypassing the broken webhook entirely. NOT root-caused — if pushes to main stop auto-deploying again, this same workaround applies, but the underlying webhook problem is still open and undiagnosed.

6) Event lifecycle gap diagnostic (Jorge-initiated, explicitly asked NOT to fix yet): a regular user can create up to 3 events (events_insert_own RLS, real server-side cap, verified) but the UI has zero edit path (EventForm only renders under /events/new, no edit mode anywhere) and zero delete/cancel-event path (the only "Cancel" button anywhere in the app is "Cancel RSVP", a guest cancelling their own attendance -- /events/[id]/page.tsx never once checks user.id === event.creator_id). The DB-layer delete capability already exists and works (events_delete_own RLS) -- verified live via a real signup->create->direct-REST-DELETE round trip -- it's just never exposed in the UI. Practical consequence Jorge named precisely: a creator's only in-product fix for a mistake is creating ANOTHER event, which still burns one of the 3 slots -- "3 events" can degrade into "3 mistakes and you're locked out." Duplicates (same title, close time) explicitly should NOT be blocked -- legitimate multi-session use case; the real gap is the forced workaround, not the duplicate. Classified by Jorge as product-evolution backlog, non-blocking, NOT a defect Factory hid, and explicitly NOT authorization to build it later without a fresh go-ahead. Methodological lesson captured as a standing feedback memory (feedback-aqa-full-crud-lifecycle): future AQA must exercise CREATE->READ->UPDATE->CANCEL/DELETE for any user-created object, not just CREATE -- a CREATE-only pass gave a clean READY over an object that was otherwise create-only/frozen.

FINAL STATE: production event-rsvp-waitlist.vercel.app serving commit f022acd (deployed via `vercel --prod`, dpl_Dv7MUpeRRFij4vf4JpGFCX6hQX8R), migration 018 applied directly to the DB. Jorge sent José the official re-review message this same session, after the READY_FOR_JOSE verdict. All synthetic test accounts/events from every phase of this session cleaned, 0 residue confirmed by SQL each time. IRONMAN.md has the full detailed rows (3 separate entries: AQA close, Back/Forward+admin-grant+webhook-incident close, lifecycle-gap diagnostic) plus a "HANDOFF SENT" note on the closing row.

WHY IT MATTERS: this is the authoritative narrative for anyone picking up Event RSVP work after this point -- what's actually fixed vs. diagnosed-only vs. deliberately deferred, and why.

NEXT AGENT SHOULD: read IRONMAN.md rows for event-rsvp-waitlist before touching that repo again. If José's re-review surfaces something new, treat it as a fresh finding against this baseline, not against the earlier 2026-08-13 remediation. Do not build event edit/delete/cancel UI without an explicit fresh go-ahead from Jorge, even though this observation documents the gap in detail. If a push to event-rsvp-waitlist/main doesn't deploy within a few minutes, check `vercel ls` for the actual deployment list before assuming it's just slow -- the webhook has failed silently once already.

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

### P11 JPI — manuales y handoff persistente cerrados
**Type:** fact  
**Project:** jpi  

CIERRE 2026-08-28. Se entregaron /opt/jpi/docs/MANUAL-USUARIO-JPI.md y MANUAL-PRODUCTO-JPI.md, contrastados contra runtime real. Se incluyó docs/screenshots/jpi-login.png, captura real de login. Verificaciones: npm run test:jpi PASS (7 pruebas + jpi entry-model), typecheck PASS, build PASS, revisión documental PASS. Documentación distingue superficies actuales (login/roles/panel/perfil/escalaciones/catalogo/productos/precios/inventario/activos/Venue/Asset Types/Asset Instances/ofertas/logout) de pendientes UI (Booking/calendario/Event Packages/flujo UI de cotización/visor de auditoría/horarios/depósitos/reglas/fotos). Estado honesto del catálogo: fuente física disponible, 10 ofertas verificables, 78 esperadas, desglose completo ausente; no inventar faltantes; PRICE_ON_REQUEST nunca es gratis. Se dejó handoff persistente en /opt/jpi/docs/P11-JPI-HANDOFF-2026-08-28.md. Seguridad preservada: sin Supabase, Vercel, pagos, deploy, producción ni exposición de credenciales.

### JPI human test runtime ready via SSH tunnel
**Type:** fact  
**Project:** dfl  

2026-08-28 P11 human-test handoff: generated distinct OWNER/ADMIN passwords and session secret in memory, encrypted credential payload at /opt/jpi/.jpi-runtime-secrets.gpg with DFL Secret Store public key, mode 600, ignored by git. Runtime started as background Next dev server with env decrypted only into process memory, bound exclusively to 127.0.0.1:3000; public IP port 3000 verified closed. Exact secure access is SSH local tunnel to VM then http://127.0.0.1:3000/jpi-admin/login. Credentials delivered privately to Jorge via Telegram; no password values in logs, repo, or Paseo response. Real Playwright against loopback passed OWNER full render/data/reload/logout and ADMIN login/invalid password/owner-only boundary/reload/logout. test:jpi 7/7, typecheck, build, diff check PASS. Catalog source remains definitively missing the verifiable 78-item breakdown; only 10 composite PDF offerings exist. No public deploy, Supabase, Vercel, payments, production, or external DB touched.

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

**Graph entropy:** 0.8541  

- **Community 11** (88 nodes): PRP como artefacto nativo, Modelo de disponibilidad en servicios digitales, Complejidad en la evaluación de costos
- **Community 0** (7 nodes): Verificación de API
- **Community 1** (7 nodes): Jurisdicción, Mercader, Observación de Ed
- **Community 2** (6 nodes): Universal Platform vs. ESC, Mercader Boundary
- **Community 3** (5 nodes): Merchant of Record, Métricas comerciales, Integraciones Externas
- **Community 4** (4 nodes): MCP Server Behavior, RLS Trap, Semántica de Inventario

---

*Mirror auto-generated 2026-08-29T05:45:36Z | La Garra → DFLghub/amos-context*
