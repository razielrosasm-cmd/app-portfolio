# Raziel Rosas — Production Web Apps

![Status](https://img.shields.io/badge/portfolio-active-brightgreen)
![Projects](https://img.shields.io/badge/projects-3-blue)
![Focus](https://img.shields.io/badge/focus-production%20systems-informational)
![Open to](https://img.shields.io/badge/open%20to-junior%20roles%20%7C%20freelance-7c5e3c)

I build full-stack web applications from start to finish: the data model, the login system, the admin tools, the deployment, and the small details that make software truly usable by real businesses.

The three projects below are **live products running real operations** — not tutorials or school exercises. Each one replaces a manual process for a real operator: a shop owner, a workshop teacher, a clinic doctor.

This repository is a **showcase**. It holds architecture notes, engineering decisions, and screenshots. The source code is private because the apps are in active production.

---

## Projects

| Project | What it does | Stack | Status | Links |
| :--- | :--- | :--- | :--- | :--- |
| **Élan Boutique** | Fashion e-commerce with per-variant inventory (size × color), order tracking, and a private admin panel. Replaced manual social-media sales for a real boutique. | ![Apps Script](https://img.shields.io/badge/Google%20Apps%20Script-4285F4?logo=google&logoColor=white) ![Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?logo=googlesheets&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?logo=supabase&logoColor=white) ![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white) ![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?logo=tailwindcss&logoColor=white) | ![Status](https://img.shields.io/badge/status-live-brightgreen) | [Live](https://elan-boutique.pages.dev) · [Details →](./elan-boutique/README.md) |
| **Taller Maché** | Art workshop booking and catalog with live seat availability, atomic capacity control, and order requests. Replaced WhatsApp bookings for a real workshop. | ![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black) ![Vite](https://img.shields.io/badge/Vite-646CFF?logo=vite&logoColor=white) ![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?logo=tailwindcss&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?logo=supabase&logoColor=white) ![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white) | ![Status](https://img.shields.io/badge/status-live-brightgreen) | [Live](https://taller-mache.pages.dev) · [Details →](./taller-mache/README.md) |
| **NutriBalance** | Clinic management platform with public booking, a doctor panel, a read-only patient portal, and full clinical history. Replaced paper files for a real nutrition practice. | ![Next.js](https://img.shields.io/badge/Next.js-000000?logo=nextdotjs&logoColor=white) ![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white) ![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?logo=tailwindcss&logoColor=white) ![D1](https://img.shields.io/badge/Cloudflare%20D1-F38020?logo=cloudflare&logoColor=white) | ![Status](https://img.shields.io/badge/status-live-brightgreen) | [Live](https://nutrition-app.razielrosas-m.workers.dev/) · [Details →](./nutrition-app/README.md) |

---

## Lean thinking applied to software

Before I wrote production software, I worked with **lean manufacturing and continuous improvement**. I am not new to this field: I have used these methods to analyze real problems and to move them from theory into practice — in administrative and service processes, and also on the factory floor.

My experience covers process efficiency on both sides of a business. The administrative side comes first here, because it is where most of my work applies:

| Administrative and business processes | Industrial processes |
| :--- | :--- |
| Accounting and financial flows | Production lines |
| Sales and marketing | Quality |
| Customer service and follow-up | Warehouse |
| Order and booking management | Logistics |
| Inventory and purchasing | Maintenance |

Most of my improvement work has been in administrative areas: financial flows, sales and marketing, customer follow-up, order and booking management, and inventory and purchasing. These are the processes where waste is often invisible — an order lost in a chat, a booking handled twice, a report rebuilt by hand every week — and where the right system saves the most time.

Lean does not only belong to manufacturing. The same principles — find the waste, remove the manual steps, standardize the flow, measure the result — apply just as well to a financial process, an administrative workflow, or a technology team. That is how I approach every project: I study how the work really happens, find where time and information are lost, and design a system the operator can trust.

The apps in this portfolio are the result of that mindset. A boutique losing orders in chat, a workshop losing seats in WhatsApp, a clinic losing history in paper files — each one is a process with waste. Each solution removes the manual steps and gives the owner a clear, measurable flow.

---

## What I focus on

- **Data integrity.** Orders and bookings are transactional. Stock decrements and workshop capacity are enforced atomically, so two customers cannot buy the last item or book the last seat at the same time.

- **Security by default.** I use Row Level Security where the platform supports it, and app-layer authorization where it doesn't. Passwords are hashed, sessions live in secure cookies, and login is rate-limited against brute force.

- **Real-world operations.** These apps are used by people, not test suites. The admin panels follow the actual workflow: create, edit, publish, notify, and refund.

- **Debugging under constraints.** When email delivery stopped working or routing broke, I found the cause by reading the platform deeply — not by switching stacks.

- **Solutions that fit the requirement.** I design the system around what the business actually needs and can afford. When a client asks for more — online payments, automation, integrations — I build the more complex version. The goal is always to reduce process time with technology, at a cost the business can accept.

---

## Engineering highlights

| Challenge | How I solved it |
| :--- | :--- |
| **Double-selling** (two customers, last unit) | Every order write is serialized through a server-side lock. Stock is reconciled inside the same transaction, so inventory stays accurate. |
| **Overbooking** (two customers, last seat) | Booking runs through an atomic database function. The database itself rejects the operation if capacity would be exceeded. |
| **Performance and quotas** | A three-layer cache (server cache, per-request memory, browser storage) keeps pages fast and reduces database reads. |
| **Email that bounced** | I traced the error to the provider's free-plan restriction, isolated the email logic in one function, and moved to a provider with open delivery. |
| **Protecting admin data** | Rotating server tokens with 12-hour expiry, hashed passwords, Row Level Security, and XSS escaping at every boundary. |
| **Historical accuracy** | Orders capture an immutable price snapshot, so changing a product later never rewrites past receipts. |

---

## What each project replaced

A quick view of the real-world impact, without the technical detail:

| Project | Before | After |
| :--- | :--- | :--- |
| **Élan Boutique** | Stock tracked from memory, orders lost in chat, no sales data | Per-variant inventory, order history, live dashboard with revenue and low-stock alerts |
| **Taller Maché** | Bookings over WhatsApp, no view of free seats, no product list | Live seat availability, booking records, product catalog, automatic confirmations |
| **NutriBalance** | Paper files and phone bookings, no patient history | Online booking, structured clinical records, patient portal, automatic emails |

---

## Portfolio structure

1. `README.md` — this file
2. `NOTICE.md` — usage and licensing
3. `elan-boutique/` — project README and screenshots
4. `taller-mache/` — project README and screenshots
5. `nutrition-app/` — project README and screenshots

Each project folder is a **showcase only** — no source code, no configuration, no credentials. The working code lives in private repositories.

---

## About me

- **Background:** self-taught developer with a strong background in lean manufacturing and continuous improvement, now focused on production systems for small businesses.
- **Strengths:** end-to-end ownership — from data model to deployment — plus the ability to take improvement methodologies from theory to real application, mainly in administrative environments (accounting, sales and marketing, customer follow-up, order management, inventory) and also in industrial ones (production lines, quality, warehouse, logistics, maintenance).
- **Approach:** I analyze the real problem first, then build the simplest system that solves it. If the business needs more automation or complexity, I design and build that too.
- **Currently:** open to a full-time junior role where I can keep growing inside a team, and available for freelance work on e-commerce, booking, and internal tools.

---

## Work with me

I am open to:

- **Junior developer roles** — I want to bring this problem-solving mindset to a full-time engineering team.
- **Freelance and contract work** — custom e-commerce, booking systems, and internal tools for small businesses.
- **Collaborations** — lean, operator-first software.

The admin panels are **not publicly accessible**. I am happy to do a live code walkthrough during an interview.

**GitHub:** [@razielrosasm-cmd](https://github.com/razielrosasm-cmd)

---
<div align="center">
<sub>Source code is private. This repository is a showcase of my production work.</sub>
</div>