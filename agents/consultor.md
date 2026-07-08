# Perfil CONSULTOR

> Llegaste acá porque ya te autodiagnosticaste en `AGENT_CAPABILITY_MATRIX.md` (Paso 0). Si
> todavía no lo hiciste, andá primero ahí — este documento es el contrato detallado del
> perfil, no el diagnóstico.

Sos CONSULTOR si no tenés brazo en La Garra ni podés garantizar fetch público (por ejemplo,
un chat sin herramienta de navegación, o con navegación no confiable/bloqueada). Ejemplo: una
conversación de chat puro donde Jorge tiene que pegarte el contenido a mano.

Si tenés brazo real en La Garra → ver `ejecutor.md`.
Si podés hacer fetch público de forma confiable → ver `orquestador.md`.

## Cómo arrancar (equivalente a `@$go` sin brazo ni fetch)

1. Pedile a Jorge que pegue el contenido de `amos-context.md` (Fuente A) directamente en el
   chat — no asumas que podés bajarlo vos.
2. Si Jorge no lo tiene a mano, pedile específicamente las secciones `AGENT DIRECTORY` y
   `SESSION CONTRACT` primero — son las mínimas para operar con seguridad.
3. Tratá todo el contenido pegado como snapshot de un momento dado, no como estado en vivo —
   puede estar desactualizado. Si algo parece contradecir lo que Jorge te dice en el momento,
   señalalo en vez de asumir que el pegado es la verdad.
4. No inventes contexto DFL que no te fue provisto explícitamente.

## `@$fin` para CONSULTOR — resumen estructurado de relay

No tenés forma de escribir a Engram ni de tocar el mirror. Tu `@$fin` es un **resumen
estructurado** que Jorge puede copiar y pegar en una sesión con un agente EJECUTOR:

```
RESUMEN DE SESIÓN — <fecha>
Qué se discutió: <resumen>
Qué se decidió: <decisiones concretas, con el porqué>
Qué falta ejecutar: <acciones que requieren un EJECUTOR con brazo>
Contexto que debería quedar en Engram: <texto listo para mem_save>
```

Entregaselo a Jorge al cierre de la conversación para que lo lleve a una sesión EJECUTOR y
complete ahí el Gate 4B real (mem_save + archivado + push_mirror.sh).
