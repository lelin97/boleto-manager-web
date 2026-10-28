# Boleto Manager — Web

> Technical challenge built for a job application process. Shared here as a code sample.

Front-end for managing **boletos** (Brazilian bank payment slips): register, list, view, edit and delete them, and mark one or many as paid at once. It consumes the REST API in [boleto-manager-api](https://github.com/lelin97/boleto-manager-api).

## The challenge

Build a full CRUD for boletos, with a front-end and a back-end. Each boleto has a description, due date, amount, late fee and interest, and can be marked as paid ("baixa", in Brazilian finance terms).

## What was delivered

- **Boleto list** in a data grid with amount, late fee, due date and a pending/paid status
- **Bulk payment:** select several pending boletos and mark them all as paid in one action (paid ones can't be selected again)
- **Create and edit** form with validation and currency inputs in BRL
- **Detail view** with every field of the boleto, including total amount and payment date
- **Delete** with feedback messages
- Global loading screen and alert system shared across the app

## Tech stack

React 18 · TypeScript · Vite · Material UI · AG Grid · React Hook Form · Zod · Zustand · Axios · React Router · date-fns

## Getting started

This app needs the API running. Start [boleto-manager-api](https://github.com/lelin97/boleto-manager-api) first, which listens on port 4050 by default.

**Requirements:** Node.js 18 (see `.nvmrc`) and yarn.

```bash
yarn
yarn dev
```

The API address is set in `.env.development` as `VITE_HOST` (default `http://localhost:4050`).

## Project structure

- **`src/pages/PayCheck`** — List, create/edit form, detail view and the Zod validation schema
- **`src/services`** — Axios instance, a typed `api` wrapper (`get`, `post`, `put`, `delete`) and one module per endpoint in `Endpoints/PayChecks`
- **`src/shared/components`** — Layout, app bar, sidebar, data grid, pop-up, alert and loading screen
- **`src/shared/contexts`** — Zustand stores for the loading screen and alerts, plus the MUI theme provider
- **`src/types`** — Shared types, including the `RetornoDB<T>` response contract

## Design decisions

- **A single response contract:** every request goes through one wrapper that returns `RetornoDB<T>`, a typed union of success (`data`) or failure (`mensagem`, `erro`). Network errors are normalized into the same shape, so screens never deal with raw Axios errors.
- **One module per endpoint:** each operation (list, create, update, delete, mark as paid) lives in its own file with optional success and error callbacks, keeping screens free of HTTP details.
- **Zustand for cross-cutting UI state:** the loading screen and alerts are small global stores that can be triggered from anywhere, including the API layer, without wrapping the app in extra providers.
- **Zod coercion for money fields:** amount, late fee and interest come from text inputs and are validated and converted to numbers in the schema itself.

## With more time, I would add

- Component and integration tests with Jest and React Testing Library
- Filters and search on the list (by status, due date or description)
- Automatic calculation of late fee and interest based on the due date
- Authentication, so the user recorded on each action comes from a real login
