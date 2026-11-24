<div align="center">

# NutriBalance

### Replacing paper files and WhatsApp booking with a complete clinic management system.

Online booking · doctor panel · patient portal · full clinical history — built for a real nutrition practice, running in production.

[![Status](https://img.shields.io/badge/status-live-brightgreen?style=flat-square)](#status)
[![Type](https://img.shields.io/badge/type-clinic%20management-informational?style=flat-square)](#)
[![Built for](https://img.shields.io/badge/built%20for-a%20real%20practice-2d6a4f?style=flat-square)](#the-problem)
[![License](https://img.shields.io/badge/source-private-lightgrey?style=flat-square)](#)

**[Live app](https://nutrition-app.razielrosas-m.workers.dev)** · **[Screenshots](#screenshots)** · **[Get in touch](#work-with-me)**

</div>

---

## The Problem

A nutrition clinic was running its whole practice on **paper files and WhatsApp**. Patient measurements, meal plans, and clinical notes lived in notebooks and spreadsheets. Booking was a phone call with no view of available times, and patients had no way to review their plan between visits. The doctor had no clear history of how a patient was progressing over time.

The practice needed a real system that matched how a clinic actually works.

## What I Built

A working web application that replaced the paper workflow from start to finish. Patients book online and receive a confirmation email with their appointment details. The doctor manages patients, measurements, and plans from a private dashboard. Patients log in to a read-only portal to review their current plan and history.

<div align="center">

| Before (Paper and WhatsApp) | After (The Automated System) |
|---|---|
| Booking by phone, no view of free slots | Public booking page with live availability |
| Patient history spread across notebooks | One structured record per patient: measurements, plans, history |
| Patients could not review their plan | Read-only patient portal, always available |
| Manual WhatsApp confirmation | Automatic confirmation emails to doctor and patient |
| No separation between doctor and patient data | Secure, role-based access on both sides |

</div>

---

## How I Solved It

To make this system reliable for a real practice, I had to solve four concrete problems.

**1. Booking that never loses an appointment.**
*   The system saves every appointment to a real database the moment the patient submits the form, before anything else happens. If email delivery fails, the appointment is still confirmed and the patient is not affected.

**2. Confirmations that actually arrive.**
*   The most common failure in serverless apps is work that is started but never finishes, because the server stops the moment it responds to the user. I made the notification step wait for completion and handle errors explicitly, so both doctor and patient receive their email every time.

**3. Secure login without heavy infrastructure.**
*   Passwords are hashed with a modern, standard algorithm, and sessions are stored in secure cookies that JavaScript cannot read. The doctor and the patient see completely different parts of the app, and the server enforces that separation on every request.

**4. A layout that works on a phone first.**
*   Doctors update notes between appointments, on their phone. I designed the panels for small screens first and scaled up to desktop, instead of the other way around.

---

## What the Practice Gains

*   **Bookings without phone calls.** Patients see real availability and confirm in under a minute.
*   **A clinical history at a glance.** The doctor opens a patient and sees the full record in one place.
*   **Patients stay on track.** The portal keeps the current plan on the patient's phone, between visits.
*   **No manual confirmation work.** Both sides receive an email automatically.
*   **One system of record.** Measurements, plans, and notes live in one place instead of three.

---

## Built With

A modern, serverless stack that runs entirely on the cloud, with no dedicated server to maintain:

*   **Frontend:** Next.js and React for the user interface.
*   **Backend:** Route handlers running on Cloudflare's global network.
*   **Database:** Cloudflare D1, a serverless SQL database.
*   **Email:** Brevo for transactional emails.
*   **Authentication:** Custom session layer using secure cookies.

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

I built NutriBalance as a real production system for a real nutrition practice. The doctor and patient panels are **not publicly reachable**; access is limited to authenticated users.

I am currently open to:
*   **Junior Developer Roles:** I am looking to bring this problem-solving mindset to a full-time engineering team.
*   **Collaborations:** Partnering on lean, operator-first software for small businesses.
*   **Freelance and Contract Work:** Building custom booking systems, internal tools, and clinical management platforms.

*Note: The source code is private because the app is actively used in production. I am happy to do a live code walkthrough during an interview.*

**Reach me:** [Email](mailto:razielrosas.m@gmail.com) · [LinkedIn](https://www.linkedin.com/in/raziel-rosas) · [GitHub](https://github.com/razielrosasm-cmd)

---
<div align="center">
<sub>Source code is private. This repository is a showcase of my production work.</sub>
</div>