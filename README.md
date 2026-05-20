# adminVAMO

Painel administrativo da [VAMO](https://github.com/diegoartursc/VAMO) — gestão interna de agências, criadores, roteiros, vendas e financeiro.

> O **site público + área logada** vive em [`VAMOsite`](https://github.com/diegoartursc/VAMOsite).
> O **backend (API + banco)** e o **app mobile** vivem em [`VAMO`](https://github.com/diegoartursc/VAMO).

---

## Stack

- Next.js 16 (App Router, Turbopack)
- React 19
- TypeScript
- Lucide React

Roda em **porta 3034** (para não conflitar com o site em 3033).

---

## Rodando local

### Pré-requisitos
- Node 20+
- Backend VAMO rodando em `http://localhost:3333` (clone [diegoartursc/VAMO](https://github.com/diegoartursc/VAMO) e `npm run dev:backend`)

### Setup
```bash
npm install
cp .env.example .env.local
npm run dev                      # http://localhost:3034
```

### Build
```bash
npm run build
npm start
```

---

## Variáveis de ambiente

```env
NEXT_PUBLIC_API_URL=http://localhost:3333/api
```

---

## Estrutura

```
src/app/
  layout.tsx          # Layout admin (sidebar + topo)
  page.tsx            # Dashboard inicial
  login/              # Login admin
  shared.tsx          # AdminDataProvider + tipos compartilhados (PendingPackage, Status…)
  globals.css
  agencias/           # Gestão de agências
  roteiristas/        # Gestão de criadores
  clientes/           # Gestão de viajantes
  roteiros/           # Aprovação/gestão de roteiros
  pacotes/            # Aprovação/gestão de pacotes
  financeiro/         # Financeiro
  conversao/          # Métricas de conversão
  historico/          # Logs e histórico
  configuracoes/      # Configurações do painel
src/lib/
  auth.ts             # Sessão (cópia de VAMOsite — drift risk documentado)
public/               # Assets
```

---

## Dependência da API VAMO

Este projeto **não tem backend próprio**. Roda contra a API do repo [`VAMO`](https://github.com/diegoartursc/VAMO):

```bash
# Em outro terminal, no repo VAMO:
npm run dev:backend
```

Endpoints consumidos (a partir de `shared.tsx`):
- `GET /api/admin/...` (pending packages, pending itineraries, audit logs)
- `GET /api/agencies`, `/api/creators`, `/api/travelers`
- `PATCH /api/admin/.../approve|reject`
- `GET /api/admin/audit-logs`

---

## Acoplamentos conhecidos (drift risk)

- `src/lib/auth.ts` é **cópia** do mesmo arquivo em `VAMOsite`. Mudanças no modelo de sessão devem ser propagadas manualmente até virarmos pacote npm compartilhado.
- `globals.css` é cópia do site. O design pode divergir no tempo.

---

## Status

Inicializado em 2026-05-20 a partir de `diegoartursc/VAMO @ chore/sync-current-vamo-state`, extraindo:
- `apps/site/src/app/admin/` → `src/app/`
- `apps/site/src/lib/auth.ts` → `src/lib/auth.ts`
- `apps/site/src/app/globals.css` + `editor-ux.css`
- `apps/site/public/` → `public/`
