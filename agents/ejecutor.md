# Perfil EJECUTOR

> Llegaste acá porque ya te autodiagnosticaste en `AGENT_CAPABILITY_MATRIX.md` (Paso 0). Si
> todavía no lo hiciste, andá primero ahí — este documento es el contrato detallado del
> perfil, no el diagnóstico.

Sos EJECUTOR si tenés brazo real en La Garra: shell (bash), Engram (mem_save/mem_search/mem_update),
y git. Ejemplo: una sesión de Claude Code u otro agente con herramientas de shell corriendo
directamente en la VM (o con acceso remoto equivalente a ella).

Si no tenés brazo pero sí podés hacer fetch público → ver `orquestador.md`.
Si no tenés ninguno de los dos → ver `consultor.md`.

**Nota de traducción de tools:** este documento usa los nombres de Claude Code
(`mem_save`/`mem_search`/`mem_update`) como convención genérica de referencia. Tu agente
concreto puede exponerlos con otro nombre — traducí según corresponda:
- **CC** (Claude Code, plugin Engram) usa `mem_save`/`mem_search`/`mem_update`.
- **Codex** (vía `engram-mcp`) usa `save_memory`/`search_memory`/`update_memory`.
Mismo Gate 4B, mismos pasos — solo cambia el nombre de la tool que invocás.

## Cómo ejecutar `@$go`

1. `curl -s http://127.0.0.1:8091/go` (o `https://context.deepfeelingslabs.com/go` si no estás en la VM)
   — o el hook `SessionStart` ya lo hizo por vos (`cc-atgo-hook.sh`).
2. Leer el payload completo: `identity`, `recent_decisions`, `active_constraints`, `pending`,
   `cc_bootstrap`, `closure_contract`, `agent_directory`, `knl`.
3. `mem_search("contexto DFL")` vía Engram MCP para observaciones adicionales no cubiertas en el payload.
4. Operar directamente con ese contexto. Prohibido pedir MASTER_INDEX ni archivos adicionales de
   `/opt/dfl-knowledge/` antes de haber procesado el payload — es suficiente para arrancar.

Distinción crítica: `@$go` es el comando del agente. `/go` es la ruta HTTP. No son lo mismo.

## Cómo ejecutar `@$fin` — dos modos

### Modo CIERRE (default)

Se usa al terminar la sesión o la tarea.

1. Gate 4B paso 1: `mem_save` del resumen — qué se hizo, evidencia, archivos afectados.
2. Gate 4B paso 2: `mem_search` de observaciones que este cierre invalida → `mem_update` con
   prefijo `[RESOLVED]` en el título, o `LIFECYCLE: archived` en su propia línea del `content`.
3. `bash /opt/dfl-context-proxy/push_mirror.sh` — regenera y publica el mirror público
   `amos-context`. Idempotente: hashea el payload `/go` (sin `generated_at`) y no-opea si no
   hay cambio sustantivo. Termina imprimiendo por stdout una línea `MIRROR: <updated|unchanged>
   | commit <hash> | <fecha UTC>` — ese commit viene de `git log` real sobre el repo del mirror,
   no es una suposición.
4. Reportar a Jorge exactamente esa línea `MIRROR: ...`. **Nunca la reemplaces por una nueva
   consulta a `/go`** — `/go` regenera `generated_at` en cada request que recibe, así que
   siempre te va a devolver "recién actualizado" aunque no se haya publicado nada nuevo. Si por
   algún motivo necesitás verificarlo vos mismo en vez de confiar en la salida del script:
   `git -C /opt/amos-context-mirror log -1 --format='%H | %ci'`.

**Cuidado si editás anexos (`agents/*.md`) a mano:** `push_mirror.sh`/`publish-amos-context.sh`
hacen `git reset --hard origin/main` sobre este mismo repo antes de regenerar. Si editaste un
anexo y no lo commiteaste + pusheaste todavía, correr `push_mirror.sh` te lo va a descartar en
silencio. Commiteá y pusheá tus cambios a `agents/*.md` **antes** de correr `push_mirror.sh`,
nunca después.

### Modo CHECKPOINT (solo si Jorge lo pide explícitamente con esa palabra)

Se usa quiere un punto de guardado a mitad de tarea, sin cerrar sesión ni propagar al mirror
público todavía.

1. `mem_save` del progreso parcial (mismo formato que el paso 1 de cierre).
2. **No** correr el barrido de archivado (paso 2 de cierre) — la tarea sigue abierta.
3. **No** correr `push_mirror.sh` — el mirror público solo se actualiza en cierre real (o por
   cron/watchdog).
4. La sesión continúa normalmente.

## Gate 4B incremental (obligatorio durante toda la sesión, no solo al cierre)

`mem_save` al cerrar cada commit, decisión o blocker resuelto — no esperar al cierre de la
tarea completa. Esto es lo que permite que el cierre sobreviva a una sesión que muere sin
aviso: si el agente nunca llega a ejecutar `@$fin`, el estado semántico ya quedó escrito en
Engram paso a paso. El watchdog de La Garra (heartbeat vencido / PID `codex` desaparecido)
cubre la mitad mecánica (`push_mirror.sh`) si la sesión muere antes del cierre ordenado.

## Formato de reporte

Al reportar estado a Jorge (arranque de sesión, o cualquier resumen de contexto), usar:

```
SISTEMA: <qué proceso/servicio es, estado actual>
CONTEXTO: <decisiones/hechos relevantes recuperados de /go y Engram>
PENDIENTE URGENTE: <qué requiere acción o decisión de Jorge ahora>
NO TOCAR: <superficies protegidas activas para esta sesión>
LISTO: <qué está listo para operar / qué falta antes de poder operar>
```
