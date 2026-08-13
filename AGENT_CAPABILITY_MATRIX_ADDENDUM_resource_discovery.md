# ADDENDUM — AGENT_CAPABILITY_MATRIX.md
## Gate: RESOURCE_DISCOVERY_BEFORE_DECLARING_ABSENCE

**Fecha:** 2026-08-13
**Origen:** Incidente real, debugging event-rsvp-waitlist (DEFECT #2, RLS blocker). SFV5 declaró "no usable authorized credential found in current environment" para Supabase; existía un Access Token de cuenta (`DFL-SFV5-VM2`, usado hace 1 día, expira 2027) que ninguna sesión buscó a nivel de cuenta de plataforma — solo se verificó entorno local.
**Precedencia:** D (Operación — extiende Paso 0 del autodiagnóstico obligatorio)
**Aplica a:** Cualquier agente en el ecosistema DFL/amOS, sin distinción de perfil (EJECUTOR/ORQUESTADOR/CONSULTOR).

---

### EL PROBLEMA, CON PRECISIÓN

No fue una falla de memoria del agente en el sentido estricto — fue una **falla de alcance de búsqueda**. El agente verificó lo que tenía a mano (entorno local, variables de entorno, `.env`) y no verificó niveles superiores donde el recurso realmente vivía (cuenta de plataforma, no proyecto). Declaró ausencia sin haber agotado el descubrimiento.

Encima, incluso si una sesión anterior sí llegó a ver ese recurso, el hallazgo no quedó escrito en Engram como hecho institucional reutilizable — así que cada sesión nueva repite la búsqueda incompleta desde cero, y repite el mismo "me falta esto y aquello."

Esto no es específico de Supabase ni de tokens. Es el mismo riesgo en cualquier declaración de carencia: service keys, permisos de git, credenciales de otros servicios, accesos de red. Cada "me falta X" no verificado en dos niveles genera presión para crear un recurso nuevo — cuando el recurso correcto ya podría existir, con lo cual el resultado neto es **sprawl de credenciales sin trazabilidad**, exactamente lo que las HLC de autonomía condicional buscan evitar.

---

### EL GATE

Antes de que cualquier agente reporte "recurso ausente / credencial no encontrada / no tengo acceso a X":

1. **Buscar en al menos dos niveles**, no solo el más inmediato:
   - Nivel local/sesión (entorno, `.env`, config del proyecto actual).
   - Nivel de cuenta/plataforma (dashboard de la plataforma, tokens de cuenta, no solo de proyecto).
   - Si aplica, nivel de máquina compartida (La Garra / VM2: buscar si otra sesión o agente ya dejó una config o token utilizable).

2. **Nombrar explícitamente dónde se buscó**, no solo el resultado. "No encontré credencial" sin decir en qué lugares se buscó es una declaración no verificable y no auditable — la próxima sesión no puede confiar en ella ni construir sobre ella.

3. **Escribir el resultado a memoria institucional (Engram) inmediatamente**, sea positivo o negativo:
   - Si se encontró: qué es, dónde vive, cómo se usa — para que la próxima sesión no repita la búsqueda.
   - Si no se encontró tras buscar en los niveles declarados: registrar eso también, como hecho verificado, no como suposición. Así una sesión futura sabe que ya se buscó y no hace falta repetir, salvo que haya cambiado algo.

4. **No crear un recurso nuevo (token, credencial, cuenta de servicio) sin antes completar los pasos 1–3.** Crear un recurso nuevo cuando uno ya existe, sin haberlo buscado, es la causa raíz directa del sprawl de credenciales.

---

### CRITERIO DE FALLA

Un agente que declara "recurso ausente" sin haber documentado en qué niveles buscó, y sin haber consultado memoria institucional para ver si una sesión previa ya resolvió esa misma pregunta, no cumple este gate — independientemente de si la declaración termina siendo cierta o falsa.

---

### RELACIÓN CON OTRAS HLC

Este gate es prerequisito operativo para cualquier HLC de autonomía condicional que involucre credenciales de terceros (ver `HLC_SFV5_SUPABASE_MCP_AUTONOMIA_v0.1.md`, sección "Mecanismo de Acceso") — antes de generar un token/credencial nuevo bajo esa autonomía, el agente debe pasar primero por este gate.
