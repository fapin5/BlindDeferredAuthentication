# 🛡️ Blind Deferred Authentication (Autenticação Cega com Atraso)

**Uma arquitetura de defesa ativa contra ataques de força bruta, focado em exaurir os recursos (RAM/CPU) e a paciência de bots automatizados (Selenium, Puppeteer, Hydra).**

## 💡 O Conceito
A Autenticação Cega com Atraso (ou *Impatience Tarpit*) inverte a lógica tradicional de login. Em vez de bloquear um atacante baseando-se apenas em taxas de erro, introduz um **pedágio de tempo obrigatório e cego** no backend, aliado a uma **penalidade de impaciência**.

## ⚙️ Como Funciona (A Armadilha)
1. **A Requisição:** O cliente envia as credenciais.
2. **A Resposta Cega:** O servidor retorna um token temporário com status HTTP 202 na mesma hora, independente de acerto ou erro.
3. **O Congelamento (Backend):** O servidor valida a senha e salva o resultado em um cache (Redis) com uma **trava temporal de 10 segundos**. 
4. **A Penalidade:** Se o servidor receber *qualquer* requisição usando aquele cookie antes dos 10 segundos acabarem, o token é **incinerado**. 
5. **O Acesso Legítimo:** O usuário humano aguarda a tela de carregamento (10s), e o frontend atualiza liberando a sessão.

---
**Desenvolvido / Idealizado por:** [bluetec.dev.br](https://bluetec.dev.br)
