# amOS Context — @$go Live Mirror
**Generated:** 2026-08-18T15:46:44Z  
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

### [RESOLVED] TCX work package — Challenge Intake post-submit journey
**Type:** decision  
**Project:** dfl  

LIFECYCLE: archived/resolved 2026-08-18. This TCC→TCX work package was executed and returned through peer-work item pw-17b2da40495a. Earlier acceptance evidence covered synthetic J3/J4 transitions and explicitly left real email delivery unproven. Follow-on hardening was committed as badd123; see Engram #512 and closure summary #513. Preserve the original specification below as historical context.

ORIGINAL SPECIFICATION:
TCC→TCX work package for Challenge Intake qualification mechanism, reusable dfl-email follow-up, safe lifecycle transitions, J3/J4 evidence, commit on project/dfl-website-t3, and report via Engram/IRONMAN. No automated scoring, no new queue, no dfl-email reimplementation, and honest CODE_READY/TRANSPORT_CONFIGURED/DELIVERY_PROVEN reporting.

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

### Session summary: dfl-knowledge
**Type:** session_summary  
**Project:** dfl-knowledge  

## Goal
Preregister the canonical PATCH_RISK policy for classifying SF upstream / Factory Extras / future Docking System coupling touchpoints, before any probe (DOCK_OBSERVATION_RUN, JPI, DCSA, Docking System) runs against real touchpoints.

## Instructions
- MODO: YOLO — proceed autonomously without per-step confirmation.
- Create ONLY: PATCH-RISK-POLICY.md, PATCH-RISK-POLICY.json, schema(s), minimal tests, preregistration receipt. Explicitly forbidden: running DOCK_OBSERVATION_RUN/JPI/DCSA/Docking System, modifying the sfv5 headless candidate, promoting anything.
- Version the work in one exclusive git commit.

## Discoveries
- Repo has no package.json/npm deps anywhere — JSON Schema files (draft 2020-12) exist as documented contracts but are validated by hand-rolled JS logic in tests, not ajv. Node test convention is `node --test tests/*.test.mjs` with plain `.mjs` ESM.
- Evidence-dir convention: `evidence/<slug>-<date>/` with a root `SHA256SUMS`, a `receipts/` subfolder, and receipts shaped like `{schema, receipt_id, gate, status, reason, producer, product, baseline{sha}, declared_at, evidence[]{declared_path,exists,sha256}, receipt_sha256-style provenance}`.
- The user's stated GREEN/YELLOW/RED rules leave one case implicit: an INTERNAL_PATCH with GOLDEN_PATH_CRITICAL/ORCHESTRATION_CRITICAL criticality that DOES have a stable hook doesn't trip any of the five named RED triggers, but YELLOW explicitly excludes orchestration/golden-path criticality — resolved by falling through to a conservative RED-by-default (documented explicitly in the policy, flagged to the user for confirmation, not yet confirmed).

## Accomplished
- ✅ Created `evidence/patch-risk-policy-preregistration-2026-08-01/` with PATCH-RISK-POLICY.md, PATCH-RISK-POLICY.json (5 classes, 8 mandatory factors, 4 derived fields, rule engine spec), schemas/patch-risk-policy.schema.json, schemas/patch-risk-touchpoint.schema.json, lib/patch-risk-classify.mjs (pure `classify()`/`redTriggers()`), tests/patch-risk-policy.test.mjs (14/14 PASS), receipts/preregistration-receipt.json, SHA256SUMS.
- ✅ Verified no other repo state touched (candidate untouched, only new evidence dir staged) and committed exclusively as `6f71e5e` on branch `feat/dfl-high-certainty-harness-v0.1`, baseline `d79fffdf4ab1739e45049bae9c3933794788c1df`.
- ✅ Incremental Gate 4B mem_save done mid-session (obs id 421) at commit time.
- 🔲 User has not yet confirmed the RED-default interpretation for the golden-path/stable-hook edge case — surfaced but unresolved.

## Next Steps
- If user confirms or amends the golden-path/stable-hook RED-default interpretation, update PATCH-RISK-POLICY.md/.json + tests accordingly (would be a policy_version bump, e.g. 0.1.1).
- Actual DOCK_OBSERVATION_RUN / JPI / DCSA / Docking System work is explicitly NOT started — this session only preregistered the classification contract they must conform to.

## Relevant Files
- evidence/patch-risk-policy-preregistration-2026-08-01/PATCH-RISK-POLICY.md — canonical policy prose
- evidence/patch-risk-policy-preregistration-2026-08-01/PATCH-RISK-POLICY.json — machine-checkable source of truth
- evidence/patch-risk-policy-preregistration-2026-08-01/lib/patch-risk-classify.mjs — verdict algorithm
- evidence/patch-risk-policy-preregistration-2026-08-01/tests/patch-risk-policy.test.mjs — 14/14 green coverage
- evidence/patch-risk-policy-preregistration-2026-08-01/receipts/preregistration-receipt.json — attributable receipt

### Session closure (@$fin) — peer-work/Telegram-BOS/Challenge-Manager mission, 2026-08-18
**Type:** fact  
**Project:** dfl  

TCC session closure, CIERRE mode (default, not CHECKPOINT). Full detail lives in IRONMAN.md rows (all dated 2026-08-18) and two new docs -- this is a pointer/summary, not a duplicate.

WHAT WAS BUILT, REAL AND VERIFIED LIVE:
1. tools/peer-work/ -- file-based dispatch queue for TCC<->TCX/CC/CX/human work, with authority genuinely separated from specification (verify-authority re-checks live against /go's projected routing_receipts.mission.role, or against a live Telegram allowlist for human-originated work). Headless cron activation (codex exec/claude -p, zero window), stale-claim recovery (requeue_stale, a dead claimant's items un-stick automatically), real exit-code capture. Proven with real TCC<->TCX round trips and real dispatched/completed production work (Engram #508's Challenge Intake package, and a real technical-assessment dispatch from the Challenge Manager).
2. tools/telegram-bos/ -- real, live Telegram bot (@DFL_BOS_bot) bridging Jorge's phone to the Factory via the same queue, no container/inbound networking needed. Two real production incidents found and fixed live during actual use (a delivery-dedup re-send loop; a 3-concurrent-process race from restart racing), both Jorge-confirmed resolved in his real Telegram chat.
3. functions/challenge-intake/tools/manager.mjs + AUTHORITY_ENVELOPE.md -- real process ownership for DFL Website Challenges (previously nobody owned a case after submit). Policy-bounded autonomy / escalate-by-exception governance, actually enforced in code (concurrency ceiling, decline-reason allowlist), not just documented. Ran against all 7 real production Challenge submissions; drove one the full lifecycle including a real peer-work-dispatched technical assessment.
4. Asset Registry: 3 new dfl.yaml manifests (dfl.peer-work-queue, dfl.telegram-bos-adapter, dfl.challenge-process-manager), index regenerated 14->17 assets, all independently query-verified discoverable.
5. Two durable docs: .claude/HANDOFF-TCC-TCX-PEER-COLLABORATION-2026-08-18.md (full mechanics + standing lessons for whoever operates this next) and .claude/BUSINESSOS-CORPUS-INVENTORY-2026-08-18.md (real inventory of the ~55-project BusinessOS corpus for manager/staff patterns, MCP libraries, WhatsApp/Telegram/marketing/video automation -- concrete REUSE/CUSTOMIZE/INSTALL/METABOLIZE/IGNORE/RETIRE calls, not vague).
6. TCX notified via both a real peer-work FYI item (claimed, read, acknowledged -- pw-4daa4bec5196) and Engram obs #515.

PUSH/DURABILITY STATE (final Gate 4B-relevant fact): all of the above is pushed to the correct origins and remote-verified (saas-factory-setup@fase-3-5-jpi-real-sfv5-bridge commit 27811cb; same repo's project/dfl-website-t3 commit 0227f73). One exception, not a loss: dfl-knowledge's feat/dfl-high-certainty-harness-v0.1 branch (carrying real work from OTHER, prior sessions, not this one) is blocked from pushing by two >100MB files in a pre-existing commit unrelated to this session -- this session's one real dfl-knowledge commit (the /go role-field authorization fix, ee0b315) was cherry-picked clean onto a new branch fix/tcx-go-role-visibility-2026-08-18 (commit 475155c) and pushed successfully instead. Nothing from this session is local-only/unreachable.

STANDING INFRASTRUCTURE LEFT RUNNING (do not stop): @DFL_BOS_bot (run.sh+bot.mjs, supervised, single-instance-locked), 3 cron entries (TCX/TCC peer-work activation every 10min, Telegram supervisor liveness every 5min).

Cleanup performed: verified zero stray processes, zero non-terminal queue items, zero orphaned worktrees, zero leftover scratch files from this session before closing.

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

### Session closure (@$fin) — peer-work/Telegram-BOS/Challenge-Manager mission, 2026-08-18
**Type:** fact  
**Project:** dfl  

TCC session closure, CIERRE mode (default, not CHECKPOINT). Full detail lives in IRONMAN.md rows (all dated 2026-08-18) and two new docs -- this is a pointer/summary, not a duplicate.

WHAT WAS BUILT, REAL AND VERIFIED LIVE:
1. tools/peer-work/ -- file-based dispatch queue for TCC<->TCX/CC/CX/human work, with authority genuinely separated from specification (verify-authority re-checks live against /go's projected routing_receipts.mission.role, or against a live Telegram allowlist for human-originated work). Headless cron activation (codex exec/claude -p, zero window), stale-claim recovery (requeue_stale, a dead claimant's items un-stick automatically), real exit-code capture. Proven with real TCC<->TCX round trips and real dispatched/completed production work (Engram #508's Challenge Intake package, and a real technical-assessment dispatch from the Challenge Manager).
2. tools/telegram-bos/ -- real, live Telegram bot (@DFL_BOS_bot) bridging Jorge's phone to the Factory via the same queue, no container/inbound networking needed. Two real production incidents found and fixed live during actual use (a delivery-dedup re-send loop; a 3-concurrent-process race from restart racing), both Jorge-confirmed resolved in his real Telegram chat.
3. functions/challenge-intake/tools/manager.mjs + AUTHORITY_ENVELOPE.md -- real process ownership for DFL Website Challenges (previously nobody owned a case after submit). Policy-bounded autonomy / escalate-by-exception governance, actually enforced in code (concurrency ceiling, decline-reason allowlist), not just documented. Ran against all 7 real production Challenge submissions; drove one the full lifecycle including a real peer-work-dispatched technical assessment.
4. Asset Registry: 3 new dfl.yaml manifests (dfl.peer-work-queue, dfl.telegram-bos-adapter, dfl.challenge-process-manager), index regenerated 14->17 assets, all independently query-verified discoverable.
5. Two durable docs: .claude/HANDOFF-TCC-TCX-PEER-COLLABORATION-2026-08-18.md (full mechanics + standing lessons for whoever operates this next) and .claude/BUSINESSOS-CORPUS-INVENTORY-2026-08-18.md (real inventory of the ~55-project BusinessOS corpus for manager/staff patterns, MCP libraries, WhatsApp/Telegram/marketing/video automation -- concrete REUSE/CUSTOMIZE/INSTALL/METABOLIZE/IGNORE/RETIRE calls, not vague).
6. TCX notified via both a real peer-work FYI item (claimed, read, acknowledged -- pw-4daa4bec5196) and Engram obs #515.

PUSH/DURABILITY STATE (final Gate 4B-relevant fact): all of the above is pushed to the correct origins and remote-verified (saas-factory-setup@fase-3-5-jpi-real-sfv5-bridge commit 27811cb; same repo's project/dfl-website-t3 commit 0227f73). One exception, not a loss: dfl-knowledge's feat/dfl-high-certainty-harness-v0.1 branch (carrying real work from OTHER, prior sessions, not this one) is blocked from pushing by two >100MB files in a pre-existing commit unrelated to this session -- this session's one real dfl-knowledge commit (the /go role-field authorization fix, ee0b315) was cherry-picked clean onto a new branch fix/tcx-go-role-visibility-2026-08-18 (commit 475155c) and pushed successfully instead. Nothing from this session is local-only/unreachable.

STANDING INFRASTRUCTURE LEFT RUNNING (do not stop): @DFL_BOS_bot (run.sh+bot.mjs, supervised, single-instance-locked), 3 cron entries (TCX/TCC peer-work activation every 10min, Telegram supervisor liveness every 5min).

Cleanup performed: verified zero stray processes, zero non-terminal queue items, zero orphaned worktrees, zero leftover scratch files from this session before closing.

### FYI for TCX — peer collaboration handoff + BusinessOS corpus inventory (2026-08-18)
**Type:** fact  
**Project:** dfl  

FYI, no action required. TCC has published a full, detailed, engine-agnostic handoff at `.claude/HANDOFF-TCC-TCX-PEER-COLLABORATION-2026-08-18.md` (repo `saas-factory-setup`, branch `fase-3-5-jpi-real-sfv5-bridge`, commit `27811cb`) covering how TCC<->TCX peer collaboration works from now on: the `tools/peer-work/` queue (contract, discovery, the two live authority schemes and why `authority_ref` must be a literally-checkable assertion, headless activation via cron, stale-claim recovery), real production use so far in both directions (including your own successful pickups of Engram #508's Challenge Intake work and the later `qualify.mjs`/`lifecycle.mjs` hardening pass on `badd123`), the new Telegram/BOS phone bridge (`@DFL_BOS_bot`, real live use with Jorge), the Challenge Process Manager and its authority envelope (`functions/challenge-intake/tools/manager.mjs` + `AUTHORITY_ENVELOPE.md`), and three new `dfl.yaml` asset-index entries (`dfl.peer-work-queue`, `dfl.telegram-bos-adapter`, `dfl.challenge-process-manager` -- query the asset index before building anything that smells like a dispatch mechanism, human bridge, or process owner).

Separately, a full BusinessOS corpus inventory is at `.claude/BUSINESSOS-CORPUS-INVENTORY-2026-08-18.md` (same commit) -- two passes (manager/staff roles, then MCP/WhatsApp/Telegram/marketing/video automation) with concrete REUSE_AS_IS/CUSTOMIZE/INSTALL/METABOLIZE/IGNORE/RETIRE calls. Headline finds worth knowing about before building anything similar from scratch: a real, correctly-built WhatsApp MCP server at `Varios para SFV5/BusinessOS/wacrm/mcp-server/` (Meta WhatsApp Business Cloud API, needs a real business number from Jorge to go live); a mature Grammy-based Telegram layer at `openclaw/src/telegram/` that's meaningfully more capable than the new `tools/telegram-bos/bot.mjs` (group chat, media/voice, streaming) -- worth a multi-day integration if that becomes a real need, not done yet; and the strongest single find, `business-os-v6/Automatización de Redes Sociales/` ("SocialFlow AI") -- a real multi-platform social publisher AND a real video-assembly pipeline (ffmpeg+ElevenLabs), already built on the SaaS Factory V4 stack, needing only real platform credentials to go live.

Also worth noting one honest, disclosed miss from this same session: a real bug was found in your `badd123` lifecycle rewrite when TCC ran it against live production (not your mocked test suite) -- `lifecycle.mjs`'s TRANSITIONS table didn't know about the manager's fuller lifecycle states, and `getSubmission()` referenced a `locale` column that didn't exist yet. Both fixed (`c150a2e`), not a criticism -- exactly the class of gap a mocked-query suite can't catch, and your own handoff was honest about not having tested against real production. Standing lesson for both of us either way: any lifecycle/schema change either of us makes needs at least one real-production round-trip before being trusted.

Read the two files directly for full detail -- this note is a pointer, not a duplicate of the content, per the established no-duplicate-content convention.

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

**Graph entropy:** 0.8083  

- **Community 11** (95 nodes): Abstracción de oferta, Política de disponibilidad, PRP como artefacto nativo
- **Community 0** (5 nodes): Merchant of Record, Integraciones Externas
- **Community 1** (4 nodes): MCP Server Behavior, RLS Trap, Semántica de Inventario
- **Community 2** (4 nodes): Plataforma Universal, ESC (Ed Square Cars), Patrón de Tenencia Owner-Scoped
- **Community 3** (4 nodes): Licenciamiento y acceso
- **Community 4** (4 nodes): Ciclo de vida automatizado en comercio

---

*Mirror auto-generated 2026-08-18T15:46:44Z | La Garra → DFLghub/amos-context*
