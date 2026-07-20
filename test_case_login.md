# 🧪 Test Case Suite — Login / Autenticação

**Módulo:** Autenticação de Usuário
**Autor:** Anemilton Leite
**Data:** 20/07/2026
**Ambiente:** Staging / QA
**Tipo de teste:** Funcional (Manual)

---

## Pré-condições Gerais

- Usuário possui uma conta cadastrada e ativa no sistema
- Aplicação está acessível na URL de ambiente de testes
- Usuário não está autenticado (sessão limpa / navegador anônimo)

---

## TC-001 — Login com credenciais válidas

| Campo | Detalhe |
|---|---|
| **ID** | TC-001 |
| **Prioridade** | Alta |
| **Tipo** | Positivo |
| **Pré-condição** | Usuário cadastrado com e-mail `usuario@teste.com` e senha `Senha@123` |

**Passos:**
1. Acessar a tela de login
2. Inserir e-mail válido: `usuario@teste.com`
3. Inserir senha válida: `Senha@123`
4. Clicar em "Entrar"

**Resultado esperado:**
Usuário é autenticado com sucesso e redirecionado para a tela inicial (dashboard/home).

**Status:** ⬜ Não executado

---

## TC-002 — Login com senha incorreta

| Campo | Detalhe |
|---|---|
| **ID** | TC-002 |
| **Prioridade** | Alta |
| **Tipo** | Negativo |
| **Pré-condição** | Usuário cadastrado com e-mail `usuario@teste.com` |

**Passos:**
1. Acessar a tela de login
2. Inserir e-mail válido: `usuario@teste.com`
3. Inserir senha incorreta: `SenhaErrada1`
4. Clicar em "Entrar"

**Resultado esperado:**
Sistema exibe mensagem de erro genérica (ex.: "E-mail ou senha inválidos") sem indicar qual campo está incorreto. Usuário permanece na tela de login.

**Status:** ⬜ Não executado

---

## TC-003 — Login com e-mail não cadastrado

| Campo | Detalhe |
|---|---|
| **ID** | TC-003 |
| **Prioridade** | Média |
| **Tipo** | Negativo |

**Passos:**
1. Acessar a tela de login
2. Inserir e-mail não cadastrado: `naoexiste@teste.com`
3. Inserir qualquer senha
4. Clicar em "Entrar"

**Resultado esperado:**
Sistema exibe mensagem de erro genérica, sem confirmar se o e-mail existe ou não na base (evita enumeração de usuários).

**Status:** ⬜ Não executado

---

## TC-004 — Campos obrigatórios vazios

| Campo | Detalhe |
|---|---|
| **ID** | TC-004 |
| **Prioridade** | Média |
| **Tipo** | Negativo |

**Passos:**
1. Acessar a tela de login
2. Deixar os campos de e-mail e senha em branco
3. Clicar em "Entrar"

**Resultado esperado:**
Sistema impede o envio do formulário e exibe validação indicando que os campos são obrigatórios.

**Status:** ⬜ Não executado

---

## TC-005 — Formato de e-mail inválido

| Campo | Detalhe |
|---|---|
| **ID** | TC-005 |
| **Prioridade** | Baixa |
| **Tipo** | Negativo |

**Passos:**
1. Acessar a tela de login
2. Inserir e-mail com formato inválido: `usuarioteste.com`
3. Inserir qualquer senha
4. Clicar em "Entrar"

**Resultado esperado:**
Sistema exibe validação de formato de e-mail inválido antes ou no momento do envio, sem chamar a API de autenticação.

**Status:** ⬜ Não executado

---

## TC-006 — Bloqueio após múltiplas tentativas falhas

| Campo | Detalhe |
|---|---|
| **ID** | TC-006 |
| **Prioridade** | Alta |
| **Tipo** | Segurança |

**Passos:**
1. Acessar a tela de login
2. Inserir e-mail válido: `usuario@teste.com`
3. Inserir senha incorreta 5 vezes consecutivas
4. Tentar logar novamente com a senha correta

**Resultado esperado:**
Após o número configurado de tentativas (ex.: 5), a conta é temporariamente bloqueada ou um CAPTCHA é exibido, mesmo que a senha correta seja informada em seguida.

**Status:** ⬜ Não executado

---

## TC-007 — Persistência de sessão ("Lembrar-me")

| Campo | Detalhe |
|---|---|
| **ID** | TC-007 |
| **Prioridade** | Baixa |
| **Tipo** | Positivo |

**Passos:**
1. Acessar a tela de login
2. Inserir credenciais válidas
3. Marcar a opção "Lembrar-me"
4. Clicar em "Entrar"
5. Fechar o navegador e reabrir a URL da aplicação

**Resultado esperado:**
Usuário permanece autenticado ao reabrir o navegador, sem precisar logar novamente.

**Status:** ⬜ Não executado

---

## TC-008 — Campo de senha oculta caracteres (masking)

| Campo | Detalhe |
|---|---|
| **ID** | TC-008 |
| **Prioridade** | Baixa |
| **Tipo** | Positivo |

**Passos:**
1. Acessar a tela de login
2. Digitar qualquer valor no campo de senha

**Resultado esperado:**
Os caracteres digitados são exibidos como pontos ou asteriscos (`•••••`), nunca em texto plano. Deve existir opção de "mostrar senha" (ícone de olho).

**Status:** ⬜ Não executado

---

## 📊 Resumo da Suíte

| ID | Cenário | Tipo | Prioridade | Status |
|---|---|---|---|---|
| TC-001 | Login com credenciais válidas | Positivo | Alta | ⬜ |
| TC-002 | Senha incorreta | Negativo | Alta | ⬜ |
| TC-003 | E-mail não cadastrado | Negativo | Média | ⬜ |
| TC-004 | Campos obrigatórios vazios | Negativo | Média | ⬜ |
| TC-005 | Formato de e-mail inválido | Negativo | Baixa | ⬜ |
| TC-006 | Bloqueio após tentativas falhas | Segurança | Alta | ⬜ |
| TC-007 | Persistência de sessão | Positivo | Baixa | ⬜ |
| TC-008 | Máscara do campo de senha | Positivo | Baixa | ⬜ |

---

### 💡 Notas para o portfólio

- Esse arquivo pode ir direto na pasta `test-cases/` do repositório sugerido no seu README (📂 Test Cases)
- Depois de executar, atualize o campo **Status** para ✅ Passou / ❌ Falhou e anexe evidências (prints, logs)
- Esses mesmos casos servem de base para automatizar com **Cypress** ou **Playwright** — TC-001 e TC-002 são os candidatos ideais para o primeiro script automatizado
