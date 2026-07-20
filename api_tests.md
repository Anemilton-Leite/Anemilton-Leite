# 📡 API Test Suite — Autenticação (`/auth/login`)

**Módulo:** API de Autenticação
**Autor:** Anemilton Leite
**Data:** 20/07/2026
**Ambiente:** Staging (`https://api-staging.exemplo.com`)
**Ferramenta sugerida:** Postman / Insomnia
**Método:** `POST`
**Endpoint:** `/api/v1/auth/login`

---

## Contrato esperado do endpoint

**Request:**
```json
{
  "email": "usuario@teste.com",
  "senha": "Senha@123"
}
```

**Response (200 — sucesso):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "expiresIn": 3600,
  "user": {
    "id": 42,
    "nome": "Usuário Teste",
    "email": "usuario@teste.com"
  }
}
```

---

## AT-001 — Login com credenciais válidas

| Campo | Detalhe |
|---|---|
| **Método** | POST |
| **Endpoint** | `/api/v1/auth/login` |
| **Prioridade** | Alta |

**Payload:**
```json
{
  "email": "usuario@teste.com",
  "senha": "Senha@123"
}
```

**Validações:**
- Status code: `200 OK`
- Corpo da resposta contém `token` (string não vazia)
- Corpo da resposta contém `expiresIn` (number > 0)
- Corpo da resposta contém objeto `user` com `id`, `nome`, `email`
- Tempo de resposta < 1000ms

**Status:** ⬜ Não executado

---

## AT-002 — Login com senha incorreta

| Campo | Detalhe |
|---|---|
| **Prioridade** | Alta |

**Payload:**
```json
{
  "email": "usuario@teste.com",
  "senha": "SenhaErrada1"
}
```

**Validações:**
- Status code: `401 Unauthorized`
- Corpo da resposta contém campo `message` com erro genérico (ex.: `"Credenciais inválidas"`)
- Resposta **não** deve conter `token`
- Resposta **não** deve indicar se o e-mail existe ou não

**Status:** ⬜ Não executado

---

## AT-003 — Login com e-mail em formato inválido

| Campo | Detalhe |
|---|---|
| **Prioridade** | Média |

**Payload:**
```json
{
  "email": "usuarioteste.com",
  "senha": "Senha@123"
}
```

**Validações:**
- Status code: `400 Bad Request`
- Corpo da resposta contém erro de validação referente ao campo `email`
- Requisição não deve alcançar a camada de autenticação (validação ocorre antes)

**Status:** ⬜ Não executado

---

## AT-004 — Payload com campos obrigatórios ausentes

| Campo | Detalhe |
|---|---|
| **Prioridade** | Média |

**Payload:**
```json
{
  "email": "usuario@teste.com"
}
```
*(campo `senha` ausente)*

**Validações:**
- Status code: `400 Bad Request`
- Corpo da resposta indica claramente qual campo obrigatório está faltando (`senha`)

**Status:** ⬜ Não executado

---

## AT-005 — Rate limiting / bloqueio por tentativas excessivas

| Campo | Detalhe |
|---|---|
| **Prioridade** | Alta |
| **Relacionado a** | BUG-001 |

**Passos:**
1. Enviar 5 requisições consecutivas com senha incorreta para o mesmo `email`
2. Enviar uma 6ª requisição, mesmo payload

**Validações:**
- A partir da 6ª tentativa, status code esperado: `429 Too Many Requests`
- Header `Retry-After` presente na resposta, indicando tempo de espera

**Status:** ⬜ Não executado — *(atualmente falha, ver BUG-001)*

---

## AT-006 — Injeção via payload (segurança básica)

| Campo | Detalhe |
|---|---|
| **Prioridade** | Alta |
| **Tipo** | Segurança |

**Payload:**
```json
{
  "email": "usuario@teste.com' OR '1'='1",
  "senha": "qualquer"
}
```

**Validações:**
- Status code: `400` ou `401` (nunca `200`)
- Payload malicioso não deve causar erro `500` nem expor stack trace
- Nenhuma informação sensível deve vazar na resposta

**Status:** ⬜ Não executado

---

## AT-007 — Token expira corretamente

| Campo | Detalhe |
|---|---|
| **Prioridade** | Média |

**Passos:**
1. Realizar login válido (AT-001) e obter `token`
2. Aguardar o tempo definido em `expiresIn` (ou manipular o horário em ambiente de teste)
3. Fazer uma requisição autenticada a uma rota protegida usando o token expirado

**Validações:**
- Status code: `401 Unauthorized`
- Corpo da resposta indica que o token expirou (ex.: `"token_expired"`)

**Status:** ⬜ Não executado

---

## 📊 Resumo da Suíte de API

| ID | Cenário | Status Code Esperado | Prioridade | Status |
|---|---|---|---|---|
| AT-001 | Login válido | 200 | Alta | ⬜ |
| AT-002 | Senha incorreta | 401 | Alta | ⬜ |
| AT-003 | E-mail inválido | 400 | Média | ⬜ |
| AT-004 | Campo obrigatório ausente | 400 | Média | ⬜ |
| AT-005 | Rate limiting | 429 | Alta | ⬜ (falha conhecida) |
| AT-006 | Injeção via payload | 400/401 | Alta | ⬜ |
| AT-007 | Token expirado | 401 | Média | ⬜ |

---

### 💡 Notas para o portfólio

- Esses casos foram desenhados para virar uma **Postman Collection** exportável (`.json`) — o próximo passo natural do seu portfólio
- AT-002 e AT-005 já têm ligação direta com os bugs reportados (BUG-002 e BUG-001), reforçando a rastreabilidade entre teste de API → caso de teste manual → bug
- Boa base também para o primeiro script de automação de API com **Playwright** (`request` context) ou **Cypress** (`cy.request`)
