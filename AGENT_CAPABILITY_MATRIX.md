# AGENT CAPABILITY MATRIX v1.0

**Leé esto ANTES de intentar `@$go`.** Es la barrera de entrada, no una referencia posterior.
Un agente que se autodiagnostica en los primeros 30 segundos no quema tokens de Jorge en
intentos fallidos. Un agente que adivina, sí.

Regla dura: **si tu diagnóstico dice que no tenés una capacidad, no la intentes.** No pruebes
"a ver si funciona" — reportá tu perfil y seguí el fallback de esa fila.

Principio de lobby: `@$go` y `@$fin` son uniformes por **contrato semántico**, no por
transporte. Todos entran al mismo lobby informacional, pero cada perfil usa el adaptador que
sus capacidades reales permiten: shell/Engram/git, fetch público, o snapshot de chat con relay.

**ALERTA DE ACTUALIZACIÓN — 2026-07-08:** antes de operar, todo agente debe pasar el
`@$go VALIDATION GATE`. No alcanza con declarar perfil; debe demostrar fuente, timestamp,
perfil, access model, modo `@$fin` y superficies protegidas.

---

## PASO 0 — AUTO-DIAGNÓSTICO (NECESARIO antes de cualquier acción)

Leé esto una sola vez al arrancar. Una pregunta por línea. **Es prohibitivo, no sugestivo:**
si tu respuesta te manda a "reportá y parate", NO INTENTES la acción de todos modos "a ver si
funciona". Eso es exactamente lo que cuesta tokens sin necesidad.

**Pregunta 1:** ¿Ves `bash_tool` en tu lista de funciones disponibles?
  - SÍ → Sos **EJECUTOR**. Leé `agents/ejecutor.md`. Ejecutá:
    `curl https://raw.githubusercontent.com/DFLghub/amos-context/main/amos-context.md`
  - NO → Pregunta 2

**Pregunta 2:** ¿Ves `web_fetch`, `web_search` o cloud browser en tu lista de funciones disponibles?
  - SÍ → Intentá **UNA vez** `curl`/fetch a `https://raw.githubusercontent.com/DFLghub/amos-context/main/amos-context.md`.
    - Si te devuelve el contenido real → Sos **ORQUESTADOR**. Leé `agents/orquestador.md`.
      Completá el VALIDATION GATE con la fuente y timestamp recuperados; no necesitás que un
      EJECUTOR vuelva a traerte el mismo contexto. Tu límite aparece recién al escribir estado
      o ejecutar `@$fin`, que requieren relay.
    - En **ChatGPT Work**, si el fetch técnico falla pero existe cloud browser, hacé un único
      intento sobre la página HTML pública del repositorio:
      `https://github.com/DFLghub/amos-context/blob/main/amos-context.md`. Es otro adaptador,
      no otro mirror. Si devuelve contenido real → sos **ORQUESTADOR**.
    - Si devuelve un bloqueo de política/allowlist (`DisabledError`, "not safe to open", o
      equivalente) y no hay otro adaptador real → sos **CONSULTOR**. El bloqueo es una
      clasificación válida, **no un onboarding fallido**. Usá el offline bootstrap capsule
      presente en las instrucciones de la sesión; no pruebes otro hosting.
  - NO → Pregunta 3

**Pregunta 3:** ¿Ves alguna otra función que permita acceder a URLs externas (`tool_search`,
`image_search`, u otra herramienta de red)? Mismo criterio que arriba: un intento real
bloqueado por política cuenta como NO, no como SÍ.
  - SÍ (y un intento real tuvo éxito) → Sos **ORQUESTADOR** (forma alternativa). Mismo reporte
    que en la Pregunta 2: reportá y esperá.
  - NO (o la tool existe pero todo intento real es bloqueado por política) → Sos **CONSULTOR**.
    Si las instrucciones contienen el offline bootstrap capsule, completá el VALIDATION GATE
    con ese snapshot y seguí en modo CONSULTOR. Si no lo contienen, reportá:

```
CONSULTOR: No tengo capacidades de red disponibles.

¿Qué significa?: No puedo hacer @$go (requiere acceso HTTP). No puedo ejecutar comandos. No
puedo modificar estado. Solo puedo leer contexto que Jorge me proporcione manualmente.

Siguiente paso: Jorge debe pasar el offline bootstrap capsule o un snapshot mínimo, o usar un
EJECUTOR para traerlo.

Mientras tanto: Estoy listo para recibir el contexto pegado, pero no lo solicito activamente.
```

Este diagnóstico corre por **capacidad real de la sesión concreta**, no por marca de modelo.
La misma familia de agente puede calificar distinto según el entorno donde corre (con o sin
`bash_tool`, con o sin `web_fetch`/`web_search` habilitado).

---

## @$go VALIDATION GATE

One-shot gate compacto. Nadie queda operativo después de `@$go` hasta responder, en máximo
6 líneas:

```
SOURCE: <URL o snapshot pegado + generated_at/Generated exacto>
PROFILE: <EJECUTOR|ORQUESTADOR|CONSULTOR> porque <capacidad real observada>
ACCESS: contrato uniforme; transporte por adaptador
FIN: <cierre real|relay|checkpoint> + qué NO puedo hacer
NO_TOUCH: puntajeTigreKnockout, Supabase, Vercel config, env vars, templates HLC-T01/T02/T03, CRON 3:05am UTC, /etc/dfl-secrets
```

PASS: fuente+timestamp exactos, perfil por capacidad real, access model correcto, modo `@$fin`
sin cierre falso, y lista completa de zonas protegidas. Una corrección permitida; segundo
fallo = degradar a CONSULTOR o pedir EJECUTOR.

---

## CANALES YA PROBADOS Y DESCARTADOS (no reintentar como mirrors)

Investigado en vivo el 2026-07-08 contra una sesión real de ChatGPT (`web.run`, sin Code
Interpreter ni Custom GPT Action registrada). Los cuatro intentos fallaron con el mismo patrón
— allowlist de red cerrado a nivel de sesión, no un problema de reputación de un dominio en
particular:

| URL probada | Resultado exacto |
|---|---|
| `https://context.deepfeelingslabs.com/go` | `not safe to open` |
| `https://raw.githubusercontent.com/DFLghub/amos-context/main/amos-context.md` | `DisabledError` (bloqueo explícito de allowlist) |
| `https://cdn.jsdelivr.net/gh/DFLghub/amos-context@main/amos-context.md` (y el mismo para `AGENT_CAPABILITY_MATRIX.md`) | `not safe to open` |
| `https://360eventos.vercel.app` | `not safe to open` |

**Conclusión operativa**: para una sesión `web.run` con este nivel de restricción, **ningún
hosting público nuevo va a funcionar** — dominio propio, GitHub raw, un CDN de máxima
reputación (jsDelivr) y una app en un dominio genérico (`vercel.app`) fallaron igual. No
propongas un quinto espejo "a ver si ese sí". El bloqueo es de sesión/allowlist, no de sitio.

**Actualización ChatGPT Work — 2026-07-18:** Work puede exponer cloud browser, una capacidad
distinta del antiguo `web.run`. Se permite un intento sobre la página HTML de GitHub indicada
arriba. Si también falla, el offline bootstrap capsule hace que `@$go` degrade a CONSULTOR sin
quedar bloqueado. `DisabledError` nunca se reporta como "VALIDATION GATE — FALLIDO".

---

## CONSULTOR (perfil por capacidad, no por marca)

`@$go`: NO directo (sin brazo ni fetch confiable)
`@$fin`: PARTIAL (reporta, espera EJECUTOR)

Cómo funciona:
- Jorge proporciona "memoria local" o snapshot fechado: campos mínimos (`identity`,
  `recent_decisions`, `active_constraints`, `pending`, `SESSION CONTRACT`)
- CONSULTOR lee eso, trabaja desde ahí
- Al cerrar: reporta qué hizo, genera un `RESUMEN DE SESIÓN` listo para relay
- EJECUTOR luego hace `@$fin` real (archiva, cierra, pushea)

**Estado: FINAL para sesiones sin brazo ni fetch confiable.** No es un fallback de último
recurso a mejorar con un bridge público genérico. Si en el futuro una sesión concreta recibe
una herramienta real alcanzable (por ejemplo una Action/connector aprobado), esa sesión deja
de ser CONSULTOR y se re-diagnostica como ORQUESTADOR o EJECUTOR según corresponda.

---

## TABLA DE CAPACIDADES

| Perfil | `@$go` | Acción de `@$go` | `@$fin` (modo) | Acción de `@$fin` | Si la capacidad se cae a mitad de sesión |
|---|---|---|---|---|---|
| **EJECUTOR** | FULL | `curl`/fetch a `/go` (o el hook `SessionStart` ya lo hizo) + `mem_search` + operar directo | CIERRE (FULL) | `mem_save` resumen + Gate 4B archivado + `bash push_mirror.sh` + reportar la línea `MIRROR: ...` | Degradar a ORQUESTADOR: avisar explícito a Jorge "perdí el brazo, sigo como ORQUESTADOR" y producir bitácora de relay |
| **ORQUESTADOR** | PARTIAL | Fetch de Fuente A (`amos-context.md`) o Fuente B (`/go`) vía HTTP, solo lectura, sin escribir a Engram | PARTIAL (relay) | Producir **bitácora semántica de relay** para que un EJECUTOR corra el Gate 4B real. Nunca intentar `bash`/`push_mirror.sh` | Degradar a CONSULTOR: pedirle a Jorge que pegue el contenido |
| **CONSULTOR** | NONE directo | **No intentar fetch bloqueado.** Usar snapshot pegado/memoria local de sesión y declarar fecha/fuente del snapshot. | CHECKPOINT (relay) | **Resumen estructurado** en texto para que Jorge lo lleve a una sesión EJECUTOR | — ya es el piso, no hay más abajo |

---

## MAPEO DE EJEMPLO (no exhaustivo — la capacidad manda sobre la marca)

- Claude Code / Codex con shell real en La Garra → **EJECUTOR**
- ChatGPT / Gemini con navegación o fetch habilitado pero sin shell en la VM → **ORQUESTADOR**
- Chat puro sin herramientas, o con navegación bloqueada/no confiable → **CONSULTOR**

---

## POR QUÉ ESTA MATRIZ EXISTE ACÁ Y NO REPETIDA EN CADA ANEXO

Los anexos (`agents/ejecutor.md`, `agents/orquestador.md`, `agents/consultor.md`) son el
**contrato detallado** de cada perfil — cómo ejecutar cada paso, formato de reporte, etc. Esta
matriz es el **único lugar** donde vive la comparación entre los tres. Si el diagnóstico de un
perfil cambia, se edita acá una sola vez — no en tres archivos que pueden divergir.

*Fuente canónica: `https://raw.githubusercontent.com/DFLghub/amos-context/main/AGENT_CAPABILITY_MATRIX.md`*
