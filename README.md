# Raziel Rosas — Production Web Apps

![Status](https://img.shields.io/badge/portfolio-active-brightgreen)
![Projects](https://img.shields.io/badge/projects-3-blue)
![Focus](https://img.shields.io/badge/focus-production%20systems-informational)

I design and build full-stack web applications end to end — data model, authentication, admin tooling, deployment, and the small operational details that make software actually usable by real businesses. 

The three projects below are live (or in active development) products, not tutorials. Each one runs a real workflow for a real operator.

This repository is a **showcase** — architecture notes, engineering decisions, and screenshots. Source code is private.

---

## Projects

| Project | What it is | Stack | Status | Links |
| :--- | :--- | :--- | :--- | :--- |
| **Élan Boutique** | Women's fashion e-commerce with per-variant inventory, order management, and an admin panel. | ![Apps Script](https://img.shields.io/badge/Google%20Apps%20Script-4285F4?logo=google&logoColor=white) ![Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?logo=googlesheets&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?logo=supabase&logoColor=white) ![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white) ![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?logo=tailwindcss&logoColor=white) | ![Status](https://img.shields.io/badge/status-live-brightgreen) | [Live](https://elan-boutique.pages.dev) · [Details →](./elan-boutique/README.md) |
| **Taller Maché** | Art workshop booking + catalog with atomic capacity control and an order-request system. | ![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black) ![Vite](https://img.shields.io/badge/Vite-646CFF?logo=vite&logoColor=white) ![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?logo=tailwindcss&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?logo=supabase&logoColor=white) ![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white) | ![Status](https://img.shields.io/badge/status-live-brightgreen) | [Live](https://taller-mache.pages.dev) · [Details →](./taller-mache/README.md) |
| **Nutrition App** | Clinic management platform: public booking, doctor panel, patient read-only panel, clinical history. | ![Next.js](https://img.shields.io/badge/Next.js-000000?logo=nextdotjs&logoColor=white) ![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white) ![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?logo=tailwindcss&logoColor=white) ![D1](https://img.shields.io/badge/Cloudflare%20D1-F38020?logo=cloudflare&logoColor=white) | ![Status](https://img.shields.io/badge/status-in%20development-yellow) | Coming soon · [Details →](./nutrition-app/README.md) |

---

## What I focus on

- **Data integrity.** Orders and bookings are transactional. Inventory decrements and workshop capacity are enforced atomically, meaning concurrent users cannot oversell stock or overbook a seat.

- **Security by default.** I use Row-Level Security where the platform supports it, app-layer authorization where it doesn't, alongside hashed credentials, secure session cookies, and rate-limited authentication.

- **Real-world operations.** These apps are run by people, not test suites. Admin panels are built for the actual workflow: create, edit, publish, notify, and refund.

- **Debugging under constraints.** When email deliverability dropped, or layout routing thrashed, the fixes came from reading the platform deeply, not from swapping stacks.

- **Pragmatic product thinking.** Each app solves a specific business problem. Payments are intentionally out of scope where a human confirmation is the safer flow; login is scoped only to the panel that needs it.

---

## Portfolio structure

1. `README.md` (This file)
2. `NOTICE.md` (Usage and licensing)
3. `elan-boutique/` (Project README and screenshots folder)
4. `taller-mache/` (Project README and screenshots folder)
5. `nutrition-app/` (Project README and screenshots folder)

Each project folder is a **showcase only** — no source code, no configuration, no credentials. Architecture, decisions, and screenshots live here; the working code lives in private repositories.

---

## Contact

**GitHub:** [@razielrosasm-cmd](https://github.com/razielrosasm-cmd)

Admin panels are not publicly accessible. Walkthroughs are available on request during an interview or call.