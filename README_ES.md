# 🛡️ Autenticación Ciega Diferida

**Una arquitectura de defensa activa contra ataques de fuerza bruta, enfocada en agotar los recursos (RAM/CPU) y la paciencia de bots automatizados.**

## 💡 El Concepto
Invierte la lógica tradicional de inicio de sesión introduciendo un **peaje de tiempo ciego y obligatorio** en el backend, junto con una **penalización por impaciencia**.

## ⚙️ Cómo Funciona (La Trampa)
1. **La Petición:** El cliente envía las credenciales.
2. **La Respuesta Ciega:** El servidor devuelve un token temporal (HTTP 202) inmediatamente.
3. **El Congelamiento (Backend):** El servidor guarda el resultado en una caché (Redis) con un **bloqueo temporal de 10 segundos**. 
4. **La Penalización:** Si el bot intenta usar el token antes de los 10 segundos, el token es **destruido**.
5. **Acceso Legítimo:** El usuario humano espera la pantalla de carga de 10s y accede automáticamente.

---
**Desarrollado / Ideado por:** [bluetec.dev.br](https://bluetec.dev.br)
