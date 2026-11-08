# Instruções de Implementação para IA (Cursor, Copilot, Claude, etc.)

**Contexto:** Você é um Engenheiro de Segurança de Software sênior. Sua tarefa é implementar a arquitetura de "Autenticação Cega com Atraso" (Blind Deferred Authentication) neste projeto.

## Passo 1: Leitura e Análise
* Analise a estrutura atual de autenticação e login deste repositório/projeto.
* Identifique exatamente onde a validação de credenciais no banco de dados ocorre e onde o token (ex: JWT) ou a sessão final é emitida para o cliente.

## Passo 2: Planejamento (Apresente para o usuário antes de codar)
Documente um breve plano de ação detalhando como você irá introduzir as seguintes mudanças sem quebrar a lógica de negócio existente:
1. Interceptar a requisição de login.
2. Validar a senha silenciosamente e gravar o estado (sucesso/falha) em um cache rápido (como Redis) com uma trava de tempo (TTL) de exatos 10 segundos.
3. Retornar IMEDIATAMENTE um `HTTP 202 Accepted` com um cookie ou token temporário.
4. Criar o middleware ou rota de validação da sessão pendente:
   - Se uma requisição chegar com o token temporário ANTES dos 10 segundos acabarem, o sistema deve assumir que é um bot impaciente, DELETAR o cache instantaneamente e retornar `401`.
   - Se chegar APÓS 10s e o status for de sucesso, emitir o token real.

## Passo 3: Implementação e Refatoração
* Adicione o código necessário no Backend (gerenciamento de estado, delays assíncronos não-bloqueantes e regra de invalidação).
* Adicione o código necessário no Frontend (interceptar o form de login, exibir loading por 10s, e fazer a requisição de polling/refresh ao backend após o tempo).

## Passo 4: Testes
* Crie os cenários de teste documentados para demonstrar como o bot que faz requisições em menos de 10 segundos perde a sessão válida.

---
**Desenvolvido / Idealizado por:** [bluetec.dev.br](https://bluetec.dev.br)
