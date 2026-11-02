# Nutrition App — Consultorio Nutriológico

**Clinic management platform: public booking, doctor panel, patient read-only panel, and full clinical history.**

![Status](https://img.shields.io/badge/status-in%20development-yellow)
![Type](https://img.shields.io/badge/type-clinic%20management-informational)
![Next.js](https://img.shields.io/badge/Next.js-000000?logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?logo=tailwindcss&logoColor=white)
![D1](https://img.shields.io/badge/Cloudflare%20D1-F38020?logo=cloudflare&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare%20Pages-F38020?logo=cloudflare&logoColor=white)

**Live link:** Coming soon

> ⚠️ **In development.** Features and screenshots are work-in-progress and may change.

---

## The problem

A nutrition clinic tracked patient history, measurements, meal plans, and routines across paper files and spreadsheets. Booking was done over WhatsApp with no availability view. Patients had no way to review their plan between appointments, and the doctor had no structured way to look back at a patient's clinical trajectory.

## The solution

### Public
- Booking page with available appointment slots.

### Doctor panel
- Patient list with clinical summaries.

- Full history per patient: measurements, weight, BMI, plans, routines.

- Plan editor and routine assignment.

- Appointment overview.

### Patient panel (read-only)
- View current plan, routine, and measurement history.

- No editing — data flows one way from the doctor.

### System
- Automated WhatsApp confirmation links on booking.

- Role-based access separating doctor and patient views.

---

## Tech stack

- **Frontend:** Next.js (App Router) + React + TypeScript + Tailwind + shadcn/ui.

- **Backend & Data:** Cloudflare D1 (serverless SQLite) accessed through Next.js Route Handlers.

- **Hosting:** Cloudflare Pages (edge runtime).

- **Email / Notifications:** Brevo API.

- **Auth:** Custom session layer (PBKDF2 hashing via Web Crypto, secure cookies).

---

## Architecture

1. The browser connects to the **Next.js application** hosted on Cloudflare Pages (running on the Edge runtime).

2. Page renders happen server-side. 

3. Mutations flow through explicit Next.js **Route Handlers** rather than Server Actions.

4. The handlers communicate directly with **Cloudflare D1** (serverless SQLite) via database bindings.

5. Session state is securely managed via **httpOnly cookies**.

---

## Notable engineering decisions

- **D1 bindings via Route Handlers, not Server Actions.** Server Actions intermittently failed when interacting with D1 bindings under the Edge runtime. Routing all mutations through explicit `POST`/`PUT` Route Handlers sidesteps the issue and makes the request/response contract auditable.

- **Edge-compatible password hashing.** PBKDF2 via the Web Crypto API — no Node `crypto` dependency, so the auth layer runs unchanged on the Edge runtime alongside the rest of the app.

- **App-layer authorization.** D1 is serverless SQLite and does not offer Row-Level Security. Every query is scoped by the authenticated user's role in the app layer — doctor queries and patient queries are separated at the handler level, not the database level.

- **httpOnly cookie sessions.** No tokens in `localStorage`. Sessions are signed, expiry-scoped, and never exposed to client-side JS.

- **Routing fix for layout thrashing.** Next.js `<Link>` navigation was causing visible layout re-flow in the panel shell. Replaced with plain `<a>` hard navigation for the affected routes — a small, deliberate tradeoff: slower transitions, but a shell that renders correctly every time.

- **Mobile-first responsive.** Doctor and patient panels are designed for phone use first — the doctor often updates notes between appointments.

---

## Screenshots

> ⚠️ **Work-in-progress screenshots.** These represent current in-development UI and may change before release. Customer names and clinical data are fictional or redacted.

![Public booking](./screenshots/public-booking.png)
*Caption: Public booking page — available appointment slots.*

![Doctor dashboard](./screenshots/doctor-dashboard.png)
*Caption: Doctor dashboard — today's appointments and patient overview.*

![Patient list](./screenshots/patient-list.png)
*Caption: Patient list with search and clinical summary.*

![Measurement view](./screenshots/measurement-view.png)
*Caption: Patient measurements — weight and BMI trajectory.*

![Plan editor](./screenshots/plan-editor.png)
*Caption: Meal plan editor.*

![Routine assignment](./screenshots/routine-assignment.png)
*Caption: Exercise routine assignment per patient.*

![Patient panel](./screenshots/patient-panel.png)
*Caption: Patient read-only panel — current plan and history.*

---

## Status

**In development.** Core flows (booking, doctor panel, patient panel, clinical history) are functional; polishing and testing before release.

### Roadmap / known limitations
- No billing or invoicing module yet.

- WhatsApp integration currently produces confirmation links; deeper automation is planned.

- No analytics dashboard — priority is the clinical workflow first.

- PDF export of plans is on the roadmap.

---

*Source code is private. This folder is a showcase only.*