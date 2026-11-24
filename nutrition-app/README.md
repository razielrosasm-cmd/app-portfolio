<div align="center">

# NutriBalance

### Replacing paper files and WhatsApp booking with a complete clinic management system.

Public booking · doctor panel · patient read-only portal · full clinical history — built for a real nutrition practice, running in production.

[![Status](https://img.shields.io/badge/status-live-brightgreen?style=flat-square)](#status)
[![Type](https://img.shields.io/badge/type-clinic%20management-informational?style=flat-square)](#)
[![Built for](https://img.shields.io/badge/built%20for-a%20real%20practice-2d6a4f?style=flat-square)](#the-problem)
[![License](https://img.shields.io/badge/source-private-lightgrey?style=flat-square)](#)

**[Live app](https://nutrition-app.razielrosas-m.workers.dev)** · **[Screenshots](#screenshots)** · **[Get in touch](#work-with-me)**

</div>

---

## The Problem

A nutrition clinic was running its entire practice on **paper files and WhatsApp**. Patient measurements, meal plans, exercise routines, and clinical notes were spread across notebooks and spreadsheets. Booking was a phone conversation with no view of available slots, and patients had no way to review their plan between appointments. The doctor had no structured history of a patient's clinical progress.

The practice needed a real system that matched how a clinic actually works.

## What I Built

A production web application that replaced the paper workflow end-to-end. Patients book online and receive a confirmation email with their appointment details. The doctor manages patients, measurements, plans, and routines from a private dashboard. Patients log in to a read-only portal to review their current plan and history.

<div align="center">

| Before (Paper and WhatsApp) | After (The Automated System) |
|---|---|
| Booking by phone with no availability view | Public booking page with live available slots |
| Patient history in notebooks and spreadsheets | Structured clinical record per patient: measurements, weight, BMI, plans, routines |
| Patients had no way to review their plan | Read-only patient portal with current plan and history |
| Manual WhatsApp confirmation | Automated confirmation emails for doctor and patient |
| No separation between doctor and patient data | Role-based access with secure sessions on both sides |

</div>

---

## Engineering Methodology

To make this system reliable for a real practice, I had to solve several technical challenges.

**1. Serverless runtime, real database.**
*   **The Challenge:** The app needed to run on Cloudflare's edge network with a real relational database, not a toy store.
*   **The Solution:** Cloudflare D1 (serverless SQLite) accessed through Next.js Route Handlers. Mutations are explicit HTTP requests, not Server Actions — the request/response contract is auditable end to end.

**2. Reliable background jobs.**
*   **The Challenge:** Sending confirmation emails from a serverless Worker after the HTTP response is returned. Fire-and-forget promises are silently killed when the response flushes.
*   **The Solution:** Notification calls are awaited with explicit error handling. The booking is committed to the database first, so a failed email never rolls back a confirmed appointment.

**3. Auth that works on the edge.**
*   **The Challenge:** Cloudflare Workers do not expose Node's `crypto` module, but the auth layer needs proper password hashing.
*   **The Solution:** PBKDF2 through the Web Crypto API. No Node dependencies, so the same auth code runs unchanged on the edge runtime. Sessions are signed, expiry-scoped, and stored in httpOnly cookies — never in `localStorage`.

**4. Security and data isolation without RLS.**
*   **The Challenge:** D1 is serverless SQLite and does not offer Row-Level Security. Every query has to be scoped correctly in application code.
*   **The Solution:** Authorization is enforced at the handler level. Doctor routes and patient routes are separated by role, and every query is scoped to the authenticated user. Patients can only read their own records.

**5. Mobile-first for the way clinics actually work.**
*   **The Challenge:** Doctors update notes between appointments, on a phone. A desktop-only layout would be unusable.
*   **The Solution:** The doctor and patient panels are designed for phone use first. The sidebar, forms, and reading views are all tuned for small screens before desktop.

---

## Business Value

The system does more than store records; it removes the daily friction of running a practice.

*   **Booking without phone calls.** Patients see real availability and book in under a minute.
*   **Clinical history at a glance.** The doctor opens a patient and sees weight, BMI, plans, and routines in one place.
*   **Patients stay engaged between visits.** The read-only portal keeps the current plan available on the patient's phone.
*   **Confirmation without manual work.** Both doctor and patient receive an email the moment a booking is confirmed.
*   **One system of record.** Measurements, plans, routines, and notes live in one place instead of three.

---

## Architecture and Tech Stack

The application is built on a modern, serverless stack that runs entirely on Cloudflare's edge network:

*   **Frontend:** Next.js 15 (App Router) + React 19 + TypeScript, styled with Tailwind CSS and Base UI.
*   **Backend:** Next.js Route Handlers, deployed to Cloudflare Workers through OpenNext.
*   **Database:** Cloudflare D1 (serverless SQLite) accessed through typed database bindings.
*   **Email:** Brevo transactional API for booking confirmations to both doctor and patient.
*   **Auth:** Custom session layer using PBKDF2 hashing via Web Crypto and httpOnly cookies.
*   **Hosting:** Cloudflare Workers with the Node.js compatibility layer enabled.

---

## Screenshots

<div align="center">

### Public site and booking
| | |
|---|---|
| ![Home](./screenshots/public-home.png) <br/> **Home** — practice overview | ![Booking](./screenshots/public-booking.png) <br/> **Booking** — available slots |
| ![Confirmation](./screenshots/public-booking-confirmed.png) <br/> **Confirmation** — booking banner | ![Services](./screenshots/public-services.png) <br/> **Services** — offered consultations |

### Doctor panel
| | |
|---|---|
| ![Dashboard](./screenshots/doctor-dashboard.png) <br/> **Dashboard** — today's appointments | ![Patients](./screenshots/doctor-patients.png) <br/> **Patients** — list with clinical summary |
| ![Measurements](./screenshots/doctor-measurements.png) <br/> **Measurements** — weight and BMI history | ![Plans](./screenshots/doctor-plans.png) <br/> **Plans** — meal plan editor |

### Patient portal
| | |
|---|---|
| ![Patient panel](./screenshots/patient-panel.png) <br/> **Overview** — current plan and history | ![Agenda](./screenshots/patient-agenda.png) <br/> **Agenda** — upcoming appointments |

</div>

*Patient names, emails, phone numbers, and clinical data in published screenshots are fictional or redacted for privacy.*

---

## Work With Me

I built NutriBalance as a real production system for a real nutrition practice. The doctor and patient panels are **not publicly reachable**; access is scoped to authenticated users.

I am currently open to:
*   **Junior Developer Roles:** I am looking to bring this problem-solving mindset to a full-time engineering team.
*   **Collaborations:** Partnering on lean, operator-first software for small businesses.
*   **Freelance and Contract Work:** Building custom booking systems, internal tools, and clinical management platforms.

*Note: The source code is private as it is actively used in production. I am happy to do a live code walkthrough during an interview.*

**Reach me:** [Email](mailto:razielrosas.m@gmail.com) · [LinkedIn](https://www.linkedin.com/in/raziel-rosas) · [https://github.com/razielrosasm-cmd](https://github.com/razielrosasm-cmd)

---
<div align="center">
<sub>Source code is private. This repository is a showcase of my production work.</sub>
</div>