# AGENT CAPABILITY MATRIX v1.0

**Leé esto ANTES de intentar `@$go`.** Es la barrera de entrada, no una referencia posterior.
Un agente que se autodiagnostica en los primeros 30 segundos no quema tokens de Jorge en
intentos fallidos. Un agente que adivina, sí.

Regla dura: **si tu diagnóstico dice que no tenés una capacidad, no la intentes.** No pruebes
"a ver si funciona" — reportá tu perfil y seguí el fallback de esa fila.

---

## PASO 0 — Autodiagnóstico (binario, antes de leer nada más)

Respondé en orden. Frená en la primera respuesta SÍ.

1. **¿Puedo ejecutar comandos de shell reales sobre La Garra** (no "sugerir" comandos — correrlos
   de verdad), con **git** y con las **tools de Engram** (`mem_save`/`mem_search` o sus
   equivalentes `save_memory`/`search_memory`) disponibles en la misma sesión?
   → **SÍ: sos EJECUTOR.** Leé `agents/ejecutor.md`. Fin del diagnóstico.
   → NO: seguí a la pregunta 2.

2. **¿Puedo hacer fetch HTTP a URLs públicas y ver el contenido real** (no inventado ni
   alucinado) de forma confiable?
   → **SÍ: sos ORQUESTADOR.** Leé `agents/orquestador.md`. Fin del diagnóstico.
   → NO: sos CONSULTOR.

3. **Si llegaste hasta acá: sos CONSULTOR.** Leé `agents/consultor.md`. No intentes `@$go`.

Este diagnóstico corre por **capacidad real de la sesión concreta**, no por marca de modelo.
La misma familia de agente puede calificar distinto según el entorno donde corre (con o sin
shell, con o sin browsing habilitado).

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
