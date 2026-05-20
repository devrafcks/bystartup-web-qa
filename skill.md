name: bystartup-web-qa

description: |
  Skill de engenharia automatizada de testes E2E web utilizando Playwright.
  Analisa arquivos de interface web, identifica fluxos de usuário, estados visuais,
  formulários, chamadas de API, autenticação, responsividade e navegação,
  gerando, executando e corrigindo testes ponta-a-ponta automaticamente.

lib-dependency: "npm i -D @playwright/test"

triggers:
  - "use a skill bystartup-web-qa para gerar testes nesse arquivo"
  - "use a skill bystartup-web-qa para testar esse fluxo"
  - "gere testes e2e para esse componente"
  - "gere testes playwright para esse arquivo"
  - "crie testes ponta-a-ponta para essa funcionalidade"
  - "analise esse componente e gere testes e2e"

---

# Objetivo

Receber um ou mais arquivos de interface web e gerar testes ponta-a-ponta (E2E) inteligentes utilizando Playwright.

A skill deve:

- Ler e interpretar o código-fonte
- Identificar fluxos de usuário
- Detectar estados visuais e condicionais
- Entender formulários e validações
- Detectar chamadas de API
- Detectar autenticação e sessão
- Detectar comportamento responsivo
- Verificar testes já existentes
- Criar, atualizar ou substituir testes automaticamente
- Executar os testes gerados
- Corrigir falhas automaticamente
- Garantir boas práticas modernas de QA e acessibilidade

---

# Ativação

A skill só deve ser executada mediante comando explícito do usuário.

Nunca ativar automaticamente.

Exemplo:

```txt
use a skill bystartup-web-qa para gerar testes nesse arquivo
```

---

# Passo 1 — Validar ambiente Playwright

Verificar se `@playwright/test` existe em `devDependencies`.

Caso não exista:

```bash id="u2m9r1"
npm i -D @playwright/test
```

Verificar existência de:

```txt id="f7w3z2"
playwright.config.ts
```

Caso não exista, sugerir:

```ts id="v8k4d9"
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './testes',

  use: {
    baseURL: 'http://localhost:3000',
    headless: true,
    trace: 'on-first-retry',
  },
});
```

---

# Passo 2 — Estrutura de testes

Buscar os diretórios:

- `/testes`
- `/tests`
- `/src/testes`
- `/src/tests`

Caso nenhum exista:
- criar `/testes` na raiz do projeto

Os arquivos devem seguir:

```txt id="r4c8x6"
testes/[feature].e2e.test.ts
```

Exemplos:

| Origem | Teste |
|---|---|
| LoginForm.tsx | testes/login.e2e.test.ts |
| Checkout.vue | testes/checkout.e2e.test.ts |
| Dashboard.tsx | testes/dashboard.e2e.test.ts |

---

# Passo 3 — Verificar testes existentes

Antes de gerar qualquer código:

| Situação | Ação |
|---|---|
| Arquivo inexistente | Criar do zero |
| Arquivo quebrado/desatualizado | Sobrescrever |
| Arquivo parcialmente válido | Preservar cenários úteis e expandir |

A skill deve reaproveitar cobertura válida sempre que possível.

---

# Passo 4 — Analisar o código-fonte

A skill deve mapear automaticamente:

## Elementos interativos
- botões
- links
- inputs
- textareas
- selects
- checkboxes
- radios
- switches

## Formulários
- campos obrigatórios
- validações
- mensagens de erro
- máscaras
- estados inválidos

## Fluxos condicionais
- loading
- erro
- sucesso
- empty state
- renderização condicional
- skeletons
- modais
- drawers

## APIs
Detectar:
- fetch
- axios
- graphql
- useQuery
- useMutation
- SWR
- React Query

Cobrir:
- sucesso
- falha
- loading
- retry
- estados vazios

## Navegação
Detectar:
- router.push
- navigate()
- Link
- href
- redirects

## Estado
Detectar:
- props condicionais
- hooks de estado
- reducers
- contextos
- stores globais

---

# Estratégia de autenticação

Ao detectar autenticação ou rotas privadas, a skill deve implementar persistência de sessão.

Detectar:
- JWT
- cookies
- localStorage auth
- sessionStorage auth
- NextAuth
- Clerk
- Firebase Auth
- Auth0
- middleware
- auth guards

---

## Setup global obrigatório

Gerar:

```txt id="m5q1z7"
playwright/.auth/user.json
```

e:

```txt id="c9v3n2"
testes/setup/auth.setup.ts
```

---

## Persistência de sessão

Exemplo:

```ts id="x4b7k1"
import { test as setup } from '@playwright/test';

setup('autenticar usuário', async ({ page }) => {
  await page.goto('/login');

  await page.getByLabel(/e-mail/i).fill('teste@teste.com');
  await page.getByLabel(/senha/i).fill('123456');

  await page.getByRole('button', {
    name: /entrar/i,
  }).click();

  await page.context().storageState({
    path: 'playwright/.auth/user.json',
  });
});
```

---

## Reutilização de autenticação

Nos testes protegidos:

```ts id="p6w2r8"
test.use({
  storageState: 'playwright/.auth/user.json',
});
```

---

## Regras obrigatórias de autenticação

Nunca repetir login em todos os testes.

Utilizar setup compartilhado sempre que possível.

Cobrir:
- acesso autorizado
- acesso negado
- sessão expirada
- logout
- refresh token
- redirecionamentos

---

# Estratégia responsiva e mobile

Ao detectar:
- media queries
- Tailwind breakpoints
- hidden/block responsivos
- drawers
- menus mobile
- navbar colapsada
- viewport rendering
- matchMedia
- componentes adaptativos

A skill deve gerar testes específicos mobile.

---

## Breakpoints obrigatórios

Cobrir:
- mobile
- tablet
- desktop

---

## Estratégia Playwright

Utilizar devices oficiais:

```ts id="j7m4q3"
import { devices } from '@playwright/test';

test.use({
  ...devices['iPhone 13'],
});
```

ou viewport customizado:

```ts id="t3v9c6"
test.use({
  viewport: {
    width: 390,
    height: 844,
  },
});
```

---

## Cenários mobile obrigatórios

Cobrir:
- menu hamburguer
- drawer
- overlays
- modais mobile
- scroll horizontal indevido
- renderização condicional
- elementos ocultos
- touch interactions
- componentes adaptativos

Exemplo:

```ts id="d8r2w5"
test.describe('Navbar Mobile', () => {
  test.use({
    ...devices['iPhone 13'],
  });

  test('deve abrir menu mobile', async ({ page }) => {
    await page.goto('/');

    await page.getByRole('button', {
      name: /menu/i,
    }).click();

    await expect(
      page.getByRole('navigation')
    ).toBeVisible();
  });
});
```

---

# Estratégia de localizadores

Prioridade obrigatória:

## 1. getByRole
```ts id="k5x1p8"
page.getByRole('button', { name: /entrar/i })
```

## 2. getByLabel
```ts id="w2n7v4"
page.getByLabel('Senha')
```

## 3. getByPlaceholder
```ts id="q9m3c1"
page.getByPlaceholder('Digite seu e-mail')
```

## 4. getByText
```ts id="h6z8r2"
page.getByText('Cadastro realizado com sucesso!')
```

## 5. getByTestId
```ts id="s4b1x9"
page.getByTestId('submit-button')
```

---

## Último recurso

Somente se necessário:
- CSS selectors
- XPath

Nunca usar CSS/XPath se houver alternativa semântica.

---

# Estratégia de assertions

Todo `test()` deve possuir pelo menos um `expect()` explícito.

Sempre utilizar assertions assíncronas:

```ts id="g1r5n8"
await expect(page).toHaveURL('/dashboard');

await expect(
  page.getByText(/sucesso/i)
).toBeVisible();
```

Nunca utilizar assertions frágeis síncronas.

---

# Estrutura obrigatória dos testes

Utilizar:
- `test.describe`
- `test.beforeEach`
- isolamento total entre testes

Exemplo:

```ts id="y8q4m2"
import { test, expect } from '@playwright/test';

test.describe('Fluxo E2E — Login', () => {

  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('deve realizar login com sucesso', async ({ page }) => {

    await page.getByLabel('E-mail')
      .fill('usuario@teste.com');

    await page.getByLabel('Senha')
      .fill('123456');

    await page.getByRole('button', {
      name: /entrar/i,
    }).click();

    await expect(page)
      .toHaveURL('/dashboard');
  });

});
```

---

# APIs e estados assíncronos

Priorizar validação visual real.

Evitar mocks desnecessários.

Cobrir:
- loading
- erro
- retry
- timeout
- sucesso
- empty states

Quando necessário, utilizar:

```ts id="z2v6k1"
page.route()
```

Exemplo:

```ts id="f9m3r7"
await page.route('**/api/produtos', route =>
  route.fulfill({
    status: 500,
    body: 'Internal Server Error',
  })
);
```

---

# Execução automática dos testes

Após gerar ou atualizar os testes, a skill deve executar automaticamente a suíte Playwright correspondente.

Executar preferencialmente:

```bash id="u5q8w1"
npx playwright test
```

Ou executar apenas o arquivo relacionado:

```bash id="n7x2c4"
npx playwright test testes/[feature].e2e.test.ts
```

---

# Estratégia de validação automática

Após executar os testes:

## Se os testes passarem
- finalizar normalmente
- informar sucesso
- resumir cobertura gerada

## Se os testes falharem
A skill deve:

1. analisar o erro retornado
2. identificar:
   - seletor inválido
   - timeout
   - elemento inexistente
   - fluxo incorreto
   - autenticação quebrada
   - problema responsivo
   - estado assíncrono
3. corrigir automaticamente
4. executar novamente

---

# Loop de estabilização

A skill deve repetir o ciclo:

1. gerar ou corrigir
2. executar
3. validar

até que:
- os testes passem
ou
- o limite máximo seja atingido

---

# Limite de tentativas

Máximo recomendado:
- 3 tentativas automáticas por arquivo

Após atingir o limite:
- reportar falhas restantes
- explicar provável causa
- sugerir correção manual

---

# Relatório final obrigatório

Ao concluir, informar:

- arquivos criados
- arquivos atualizados
- quantidade de testes gerados
- quantidade de testes aprovados
- falhas restantes
- cobertura identificada
- fluxos autenticados cobertos
- cenários mobile cobertos

---

# Cenários obrigatórios

Toda funcionalidade identificada deve possuir:

- cenário principal
- cenário alternativo
- cenário de erro
- validação visual
- assertions explícitas

---

# Regras proibidas

Nunca usar:
- `page.waitForTimeout()`
- testes interdependentes
- CSS/XPath desnecessário
- testes sem assertions
- apenas happy-path
- login repetido em todos os testes

---

# Checklist obrigatório

Antes de finalizar:

- [ ] Todas as funcionalidades identificadas possuem cobertura
- [ ] Existe ao menos um cenário alternativo
- [ ] Existe ao menos um cenário de erro
- [ ] Todos os testes possuem assertions
- [ ] Nenhum seletor frágil foi usado sem necessidade
- [ ] O arquivo segue `.e2e.test.ts`
- [ ] O `describe` possui nome descritivo
- [ ] APIs possuem cobertura
- [ ] Estados de loading possuem cobertura
- [ ] Navegação foi validada
- [ ] Fluxos autenticados foram cobertos
- [ ] Sessão persistente foi implementada
- [ ] Componentes responsivos possuem testes mobile
- [ ] Breakpoints foram considerados
- [ ] Os testes foram executados
- [ ] As falhas foram corrigidas automaticamente quando possível
