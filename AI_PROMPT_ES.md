# Prompt de Implementación para IA (Cursor, Copilot, Claude, etc.)

**Contexto:** Eres un Ingeniero de Seguridad de Software Senior. Tu tarea es implementar la arquitectura de "Autenticación Ciega Diferida" (Blind Deferred Authentication) en este proyecto.

## Paso 1: Lectura y Análisis
* Analiza la estructura actual de autenticación e inicio de sesión de este repositorio.
* Identifica exactamente dónde ocurre la validación de credenciales y dónde se emite el token o la sesión final.

## Paso 2: Planificación (Presentar al usuario antes de programar)
Documenta un breve plan de acción detallando cómo introducirás los siguientes cambios sin romper la lógica existente:
1. Interceptar la petición de login.
2. Validar silenciosamente la contraseña y guardar el estado (éxito/fallo) en una caché rápida (como Redis) con un bloqueo de tiempo de exactamente 10 segundos.
3. Devolver INMEDIATAMENTE un `HTTP 202 Accepted` con un cookie temporal.
4. Crear la regla de invalidación o middleware:
   - Si llega una petición con el token temporal ANTES de los 10 segundos, asumir que es un bot impaciente, BORRAR instantáneamente la caché y devolver `401`.
   - Si llega DESPUÉS de 10s y el estado es de éxito, emitir el token real.

## Paso 3: Implementación
* Añade el código necesario en el Backend (gestión del estado y reglas de invalidación).
* Añade el código necesario en el Frontend (interceptar el login, mostrar pantalla de carga por 10s y hacer la petición final).

## Paso 4: Pruebas
* Documenta o crea escenarios de prueba para demostrar cómo un bot que intenta acceder en menos de 10 segundos pierde una sesión válida.

---
**Desarrollado / Ideado por:** [bluetec.dev.br](https://bluetec.dev.br)
