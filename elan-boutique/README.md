<div align="center">

# Élan Boutique

**A production e-commerce system replacing manual social-media sales with real inventory tracking, order management, and business analytics.**

[![Status](https://img.shields.io/badge/status-live-brightgreen?style=flat-square)](#)
[![Focus](https://img.shields.io/badge/focus-e--commerce%20%26%20automation-blue?style=flat-square)](#)
[![Built for](https://img.shields.io/badge/built%20for-a%20real%20business-7c5e3c?style=flat-square)](#)

**[🛍️ Live Demo](https://elan-boutique.pages.dev)** · **[📸 Screenshots](#screenshots)** · **[✉️ Contact Me](#contact)**

</div>

---

## 📌 The Problem

A small fashion boutique was managing its entire business through Instagram and WhatsApp DMs. This created three massive bottlenecks:
1. **No inventory control:** Stock for variants (size × color) was tracked mentally, leading to frequent overselling.
2. **Lost data:** Orders were buried in chat threads with no centralized history.
3. **No business insight:** The owner had no idea what their average order value was or what items needed restocking.

## 💡 The Solution

I built a custom, lightweight storefront and an administrative dashboard. The system allows customers to browse and checkout securely, while giving the owner a private panel to manage inventory, track orders, and view live sales metrics.

---

## 🛠️ Engineering Methodology & Highlights

Instead of just writing code, I had to solve real-world engineering challenges to make this system reliable for a business. Here is how I approached it:

**1. Preventing Race Conditions (Concurrency)**
*   *The Challenge:* What if two customers buy the last available shirt at the exact same time?
*   *The Solution:* I implemented a **Locking Mechanism** (`LockService`). When an order starts, the system locks the inventory database, checks the stock, decrements it, and completes the order before unlocking. This guarantees 100% inventory accuracy.

**2. Performance & Cost Optimization (Caching)**
*   *The Challenge:* Fetching large amounts of data from the database (Google Sheets) on every page load is slow and hits platform limits.
*   *The Solution:* I built a **3-layer caching strategy**. Data is stored in server memory (`CacheService`) and in the user's browser (`localStorage`). This makes the storefront load instantly while drastically reducing database reads.

**3. Security (Authentication & XSS)**
*   *The Challenge:* Protecting the owner's admin panel and customer data.
*   *The Solution:* I implemented **Token-based Authentication** with a 12-hour expiration and brute-force protection (failed logins lock the account temporarily). All customer input is sanitized to prevent cross-site scripting (XSS).

**4. Clean Data Architecture**
*   *The Challenge:* How to handle products with multiple sizes and colors without losing track of stock.
*   *The Solution:* I designed the database so that stock is tracked at the **variant level** (e.g., "Medium / Red"), and the main product's stock is automatically recalculated based on the variants. Orders also capture an immutable snapshot so changing a product price later doesn't ruin old order receipts.

---

## 💻 The Tech Stack

*   **Frontend:** HTML5, Tailwind CSS (responsive, modern UI)
*   **Backend:** Google Apps Script (serverless API)
*   **Database:** Google Sheets API (chosen specifically for the owner's familiarity)
*   **Storage & Hosting:** Supabase (images), Cloudflare (domain)

---

## 📸 Screenshots

| Storefront | Admin Dashboard |
|:---:|:---:|
| ![Home](./screenshots/storefront-home.png) | ![Dashboard](./screenshots/admin-dashboard.png) |
| **Product View** | **Inventory Editor** |
| ![Product](./screenshots/product-detail.png) | ![Editor](./screenshots/admin-product-editor.png) |

*(Customer data in screenshots is redacted for privacy.)*

---

## 🤝 Contact

I am a Junior Developer open to full-time roles, freelance work, and collaborations. I love building tools that solve real operational problems.

*   **Email:** [your email]
*   **LinkedIn:** [LinkedIn]
*   **GitHub:** [GitHub]

*(Note: The source code is private as it is actively used in production. I am happy to do a live code walkthrough during an interview.)*