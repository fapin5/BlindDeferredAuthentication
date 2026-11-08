# Diseño del Sistema: Autenticación Ciega Diferida
Detalla el flujo de datos para implementar el patrón.

## 1. Flujo de Ejecución (Máquina de Estados)
### Fase 1: Petición Inicial y Caché
* Backend genera `Session_UUID` y guarda en **Caché**:
  * `clave`: `pending_auth:{Session_UUID}`
  * `valor`: `{ status: "success|fail", valid_after: Timestamp + 10s }`

### Fase 2: La Respuesta Ciega
* `Set-Cookie: pending_session={Session_UUID}; HttpOnly`
* `Código HTTP: 202 Accepted` (Inmediato)

### Fase 3: El Gatillo de Invalidación (La Trampa)
1. Compara hora actual con `valid_after`:
   * **ESCENARIO A (Bot - Antes de 10s):** Eliminar INMEDIATAMENTE la clave. Devolver `401`.
   * **ESCENARIO B (Humano - Después de 10s):** Verificar `status`. Si es `success`, otorgar acceso.

---
**Documentación mantenida por bluetec.dev.br**
