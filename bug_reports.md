# 🐞 Bug Report — Exemplo de Portfólio

**Módulo:** Autenticação de Usuário (Login)
**Reportado por:** Anemilton Leite
**Data:** 20/07/2026
**Ambiente:** Staging / QA
**Build/Versão:** v1.2.3

---

## BUG-001 — Bloqueio por tentativas falhas não é aplicado

| Campo | Detalhe |
|---|---|
| **ID** | BUG-001 |
| **Severidade** | Alta (falha de segurança) |
| **Prioridade** | Alta |
| **Status** | 🔴 Aberto |
| **Caso de teste relacionado** | TC-006 — Bloqueio após múltiplas tentativas falhas |
| **Navegador/Dispositivo** | Chrome 126, Windows 11 |
| **Ambiente** | Staging |

### Resumo
O sistema não bloqueia a conta nem exibe CAPTCHA após 5 tentativas consecutivas de login com senha incorreta, permitindo tentativas ilimitadas.

### Passos para reproduzir
1. Acessar a tela de login (`/login`)
2. Inserir e-mail válido: `usuario@teste.com`
3. Inserir senha incorreta: `SenhaErrada1`
4. Clicar em "Entrar"
5. Repetir os passos 3-4 mais 4 vezes (total de 5 tentativas)
6. Tentar logar novamente com senha incorreta pela 6ª vez

### Resultado atual
A 6ª tentativa é processada normalmente pela API, sem bloqueio, CAPTCHA ou mensagem de aviso adicional.

### Resultado esperado
Após 5 tentativas falhas consecutivas, o sistema deveria:
- Bloquear temporariamente a conta (ex.: 15 minutos), **ou**
- Exigir resolução de CAPTCHA antes de aceitar novas tentativas

### Evidências
> 📎 *(Anexar aqui prints de tela, vídeo da reprodução ou request/response da API — ex.: via DevTools ou Postman)*

- Screenshot 1: tela de login após a 6ª tentativa
- Log da API: resposta `200 OK` mesmo após múltiplas falhas (esperado: `429 Too Many Requests` ou similar)

### Impacto
Abre brecha para ataques de força bruta contra contas de usuário.

### Observações adicionais
Comportamento testado apenas via interface web. Recomenda-se verificar também se a API aceita chamadas diretas sem o mesmo controle de rate limiting.

---

## BUG-002 — Mensagem de erro expõe se o e-mail existe na base

| Campo | Detalhe |
|---|---|
| **ID** | BUG-002 |
| **Severidade** | Média |
| **Prioridade** | Média |
| **Status** | 🔴 Aberto |
| **Caso de teste relacionado** | TC-003 — Login com e-mail não cadastrado |
| **Navegador/Dispositivo** | Chrome 126, Windows 11 |
| **Ambiente** | Staging |

### Resumo
Ao tentar logar com um e-mail não cadastrado, o sistema exibe a mensagem "E-mail não encontrado", diferente da mensagem exibida quando o e-mail existe mas a senha está errada ("Senha incorreta"). Isso permite que um atacante descubra quais e-mails estão cadastrados na base (enumeração de usuários).

### Passos para reproduzir
1. Acessar a tela de login
2. Inserir um e-mail não cadastrado, ex.: `naoexiste@teste.com`
3. Inserir qualquer senha
4. Clicar em "Entrar"
5. Repetir o teste com um e-mail cadastrado e senha errada, comparando as mensagens

### Resultado atual
Mensagens diferentes para "e-mail não existe" e "senha errada".

### Resultado esperado
Mensagem genérica e idêntica em ambos os casos, ex.: "E-mail ou senha inválidos".

### Evidências
> 📎 *(Anexar prints comparando as duas mensagens de erro)*

### Impacto
Facilita ataques de enumeração de contas válidas.

---

## 📊 Resumo dos Bugs

| ID | Título | Severidade | Prioridade | Status |
|---|---|---|---|---|
| BUG-001 | Bloqueio por tentativas falhas não aplicado | Alta | Alta | 🔴 Aberto |
| BUG-002 | Mensagem de erro expõe existência de e-mail | Média | Média | 🔴 Aberto |

---

### 💡 Notas para o portfólio

- Esse arquivo pode ir na pasta `bug-reports/` do repositório, referenciado pelo link "📂 Bug Reports" do seu README
- Em um cenário real, cada bug teria um card equivalente no Jira — vale linkar aqui o número do ticket (ex.: `JIRA-123`) quando estiver usando uma ferramenta de rastreamento
- Sempre relacione o bug ao caso de teste que o originou (como feito acima com TC-006 e TC-003), isso mostra rastreabilidade entre teste e defeito
