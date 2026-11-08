# Documento de Design de Sistema: Autenticação Cega com Atraso
Detalha o fluxo de dados para a implementação do padrão.

## 1. Fluxo de Execução (State Machine)
### Fase 1: A Requisição Inicial e Cache
* Backend gera um `Session_UUID` e grava no **State Cache**:
  * `chave`: `pending_auth:{Session_UUID}`
  * `valor`: `{ status: "success|fail", valid_after: Timestamp + 10s }`

### Fase 2: A Resposta Cega
* `Set-Cookie: pending_session={Session_UUID}; HttpOnly`
* `Status Code: 202 Accepted` (Imediato)

### Fase 3: O Gatilho de Invalidação (A Armadilha)
1. Compara o horário atual com o `valid_after`:
   * **CENÁRIO A (Bot - Antes dos 10s):** Excluir IMEDIATAMENTE a chave do cache (Invalidação Total). Retornar `401`.
   * **CENÁRIO B (Humano - Após 10s):** Verificar `status`. Se `success`, emitir token final.

---
**Documentação mantida por bluetec.dev.br**
