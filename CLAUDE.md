# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Expense/finance tracker built with React 19 + Vite. This is a course starter project (Claude Code course, codewithmosh.com) that **intentionally ships with a bug, poor UI, and messy code** — the codebase is meant to be incrementally cleaned up and fixed, so don't be surprised by rough edges; fixing them may be the point of the task at hand rather than an accident to silently work around.

## Commands

```bash
npm install       # install deps
npm run dev        # start Vite dev server (http://localhost:5173)
npm run build       # production build
npm run preview     # preview the production build
npm run lint        # run ESLint
```

There is no test suite configured in this project.

### Node version

Vite 7 requires Node **20.19+ or 22.12+**. If the active Node is older (e.g. an nvm default of v18), `npm run dev` fails with `crypto.hash is not a function`. Check available versions with `nvm ls` and switch with `nvm use <version>` before running dev/build commands if the default Node is too old.

## Architecture

This is a minimal, single-screen app — nearly all logic lives in one place:

- `src/main.jsx` — standard Vite/React entry point, mounts `<App />` into `#root`.
- `src/App.jsx` — the entire application. All state (transactions list, form fields, filters), derived values (income/expense/balance totals, filtered transaction list), and the add-transaction handler live in this one component via `useState`. There is no component decomposition, no routing, and no external state management.
- `src/App.css` / `src/index.css` — plain CSS, no CSS-in-JS or utility framework.
- No backend, no persistence layer — transactions are seeded as an in-memory array and reset on page reload.

Key data shape (transaction object): `{ id, description, amount, type: "income"|"expense", category, date }`. Note `amount` is stored as a **string** (from the seed data and from form input values), not a number — arithmetic on it (e.g. in the income/expense reduce totals) is a known source of bugs in this codebase since `+` on strings concatenates rather than adds.

`categories` is a hardcoded array (`food`, `housing`, `utilities`, `transport`, `entertainment`, `salary`, `other`) shared between the add-transaction form and the filter dropdown.
