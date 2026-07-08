# AGENT CAPABILITY MATRIX v1.0

**Leé esto ANTES de intentar `@$go`.** Es la barrera de entrada, no una referencia posterior.
Un agente que se autodiagnostica en los primeros 30 segundos no quema tokens de Jorge en
intentos fallidos. Un agente que adivina, sí.

Regla dura: **si tu diagnóstico dice que no tenés una capacidad, no la intentes.** No pruebes
"a ver si funciona" — reportá tu perfil y seguí el fallback de esa fila.

---

## PASO 0 — AUTO-DIAGNÓSTICO (NECESARIO antes de cualquier acción)

Leé esto una sola vez al arrancar. Una pregunta por línea. **Es prohibitivo, no sugestivo:**
si tu respuesta te manda a "reportá y parate", NO INTENTES la acción de todos modos "a ver si
funciona". Eso es exactamente lo que cuesta tokens sin necesidad.

**Pregunta 1:** ¿Ves `bash_tool` en tu lista de funciones disponibles?
  - SÍ → Sos **EJECUTOR**. Leé `agents/ejecutor.md`. Ejecutá:
    `curl https://raw.githubusercontent.com/DFLghub/amos-context/main/amos-context.md`
  - NO → Pregunta 2

**Pregunta 2:** ¿Ves `web_fetch` O `web_search` en tu lista de funciones disponibles?
  - SÍ → Sos **ORQUESTADOR**. Leé `agents/orquestador.md`. Reportá a Jorge exactamente:
    *"ORQUESTADOR: no puedo hacer @$go solo. Necesito que me pases el contexto manualmente o
    que CC ejecute curl."*
  - NO → Pregunta 3

**Pregunta 3:** ¿Ves alguna otra función que permita acceder a URLs externas (`tool_search`,
`image_search`, u otra herramienta de red)?
  - SÍ → Sos **ORQUESTADOR** (forma alternativa). Mismo reporte que en la Pregunta 2: reportá y
    esperá.
  - NO → Sos **CONSULTOR**. Reportá esto EXACTAMENTE, sin parafrasear:

```
CONSULTOR: No tengo capacidades de red disponibles.

¿Qué significa?: No puedo hacer @$go (requiere acceso HTTP). No puedo ejecutar comandos. No
puedo modificar estado. Solo puedo leer contexto que Jorge me proporcione manualmente.

Siguiente paso: Jorge debe pasar el contenido de amos-context.md a mano, o usar EJECUTOR (CC)
para traerlo.

Mientras tanto: Estoy listo para recibir el contexto pegado, pero no lo solicito activamente.
```

Este diagnóstico corre por **capacidad real de la sesión concreta**, no por marca de modelo.
La misma familia de agente puede calificar distinto según el entorno donde corre (con o sin
`bash_tool`, con o sin `web_fetch`/`web_search` habilitado).

---

## TABLA DE CAPACIDADES

| Perfil | `@$go` | Acción de `@$go` | `@$fin` (modo) | Acción de `@$fin` | Si la capacidad se cae a mitad de sesión |
|---|---|---|---|---|---|
| **EJECUTOR** | FULL | `curl`/fetch a `/go` (o el hook `SessionStart` ya lo hizo) + `mem_search` + operar directo | CIERRE (FULL) | `mem_save` resumen + Gate 4B archivado + `bash push_mirror.sh` + reportar la línea `MIRROR: ...` | Degradar a ORQUESTADOR: avisar explícito a Jorge "perdí el brazo, sigo como ORQUESTADOR" y producir bitácora de relay |
| **ORQUESTADOR** | PARTIAL | Fetch de Fuente A (`amos-context.md`) o Fuente B (`/go`) vía HTTP, solo lectura, sin escribir a Engram | PARTIAL (relay) | Producir **bitácora semántica de relay** para que un EJECUTOR corra el Gate 4B real. Nunca intentar `bash`/`push_mirror.sh` | Degradar a CONSULTOR: pedirle a Jorge que pegue el contenido |
| **CONSULTOR** | NONE | **No intentar.** Decir explícitamente: *"No puedo hacer @$go, necesito que me pegues `amos-context.md` o que un EJECUTOR/ORQUESTADOR opere por mí."* | CHECKPOINT (relay) | **Resumen estructurado** en texto para que Jorge lo lleve a una sesión EJECUTOR | — ya es el piso, no hay más abajo |

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
