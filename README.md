# invoicemanagementsystem
Invoicing System



📄 Product Requirements Document (PRD)
🧩 Project: FreelanceFlow — Minimal Invoice & Client Manager
1. Overview
1.1 Purpose

FreelanceFlow is a lightweight, privacy-first, invoice and client management application built for freelancers, consultants, and small teams. It enables users to manage clients, generate professional invoices, send them via email, and automatically remind clients of overdue payments—all with no ongoing cost or vendor lock-in.

1.2 Target Audience

Freelancers, solopreneurs, consultants

Small shops and teams (1–50 people)

Users seeking a minimal alternative to SaaS platforms (e.g., QuickBooks, FreshBooks)

2. Goals & Objectives
2.1 MVP Goals

✅ CRUD for Clients and Invoices

✅ PDF invoice generation (download and email)

✅ SMTP email delivery

✅ Auth with roles (Admin, Shop Assistant)

✅ Scheduled daily reminders (Vercel Cron)

✅ Secure and responsive dashboard

✅ Hosted entirely on free-tier services

2.2 Non-Goals

Online payment collection (Stripe/PayPal)

Client portal or login

Multi-language or multi-currency support (future feature)

3. Core Features & Requirements
🔐 3.1 Authentication & Authorization
Requirement	Description
Login	Manual login via email/password (JWT stored in cookie)
Roles	Admin (full access), Shop Assistant (limited access)
Route Protection	Secure pages based on roles
Tech	JWT (with secure HttpOnly cookie), session validation middleware
👤 3.2 Client Management
Requirement	Description
Create/Edit/Delete Clients	Form with inline validation
View Client Detail	Show associated invoices
Store Fields	Name, Company, Email, Phone, Address, Notes
Tech	React Hook Form + Zod/Yup, Tailwind UI
🧾 3.3 Invoice Management
Requirement	Description
Invoice CRUD	Add/edit/delete invoices linked to clients
Multi-line Items	Each invoice supports multiple items (desc, qty, price, tax)
Status Tracking	Sent, Paid, Overdue, Draft
Due Date	Required field
Tax Field	Optional per line item
Tech	React Hook Form, Zod/Yup, Tailwind
🧰 3.4 PDF Generation
Requirement	Description
On-the-fly PDF	Generate PDFs using Puppeteer or pdf-lib
No Storage	PDFs not saved on server or DB
Access	Download via UI or send via email
Branding	Basic logo, sender info, client info, styled invoice
Tech	Puppeteer or pdf-lib in API route
📧 3.5 Emailing Invoices
Requirement	Description
SMTP Integration	Use Gmail/Mailgun/custom SMTP credentials
Attach PDF	Generated invoice attached
Custom Message	Basic templated email with variables (e.g., client name)
Delivery Feedback	Track send success/failure
Tech	Nodemailer with .env-based SMTP credentials
⏰ 3.6 Scheduled Reminders
Requirement	Description
Daily Check	Vercel Cron job runs daily
Find Overdue	Query DB for unpaid invoices past due date
Auto Email	Email reminder to client
Enable/Disable	Toggle per invoice
Tech	Vercel Cron, Serverless API route, DB flag for reminders
📊 3.7 Dashboard
Requirement	Description
Stats	Total invoices, overdue count, unpaid total
Actions	Create invoice, add client, export data
UI	Mobile-responsive, light/dark mode
Tech	Tailwind UI, Next.js App Router
📤 3.8 Data Export / Backup
Requirement	Description
Manual Export	Download all data (clients, invoices) as JSON or CSV
Scheduled Backup (Optional)	Optional daily/weekly export via email or GitHub commit
Tech	API route to generate export, optional Vercel Cron job
4. System Architecture
[Client Browser]
    |
    |--> [Next.js Frontend (App Router)]
    |     - Tailwind UI
    |     - JWT login, session cookies
    |     - Role-based routing
    |
    |--> [API Routes (Vercel Serverless)]
          - /api/auth/login, logout
          - /api/clients (CRUD)
          - /api/invoices (CRUD)
          - /api/pdf/generate
          - /api/email/send
          - /api/reminders/run (Vercel Cron)
          - /api/export (JSON/CSV)
    |
    |--> [Filess.io Database]
          - Users
          - Clients
          - Invoices
          - Logs (optional)
    |
    |--> [SMTP Server]
          - Gmail, Mailgun, or custom
    |
    |--> [Vercel Scheduled Functions]
          - Daily reminder runner

5. Tech Stack Summary
Layer	Stack
Frontend	Next.js (App Router), Tailwind CSS, React Hook Form, Zod/Yup
Backend	Next.js API Routes (Vercel), JWT for auth, Puppeteer/pdf-lib, Nodemailer
Database	Filess.io (PostgreSQL or MySQL)
Hosting	Vercel (Frontend + Backend + Cron)
PDF	Puppeteer or pdf-lib (no storage)
Email	Nodemailer (SMTP via Gmail/Mailgun)
Auth	JWT stored in HttpOnly cookies
Storage	None (PDFs generated on-the-fly)
6. Security & Privacy Considerations

HTTPS enforced (Vercel default)

JWTs stored in secure, HttpOnly cookies

Environment variables for secrets

No persistent storage of sensitive files (PDFs)

SMTP credentials never exposed to client

7. User Roles & Permissions
Role	Permissions
Admin	Full access: Clients, Invoices, Settings, Email, Export
Shop Assistant	Can create/edit Clients and Invoices only (no delete, email, export)
8. UX & UI Requirements

Responsive layout (mobile, tablet, desktop)

Accessible forms with inline validation

Light/Dark mode toggle

Clean, modern design using Tailwind UI components

Toast notifications for success/failure (email, form submission, etc.)

9. Future Features (Not in MVP)

Multi-language / multi-currency support

Stripe/PayPal integration

Client-facing portal

Activity logging

Team collaboration (multi-user per business)

Analytics (monthly revenue, client stats)

10. Milestones / Timeline (Example)
Milestone	Timeline
UI Design Mockups	Week 1
Auth + Roles	Week 1
Client & Invoice CRUD	Week 2
PDF Generation	Week 3
Email via SMTP	Week 3
Dashboard Metrics	Week 4
Daily Cron + Reminders	Week 4
Data Export	Week 5
Testing & Polish	Week 5–6
Launch	End of Week 6
✅ MVP Completion Checklist

 Admin & Shop Assistant auth flow

 CRUD for clients

 CRUD for invoices

 PDF generation API

 SMTP email API

 Vercel cron for reminders

 Responsive dashboard UI

 Manual export to JSON/CSV

 Role-based route protection

 Deployment on Vercel (with free-tier services)

✅ SYSTEM PROMPT — “FreelanceFlow: Minimal Invoice & Client Manager” 🧩 System Overview FreelanceFlow is a minimal, modern, and fully hosted invoice and client management web application tailored for freelancers, consultants, and small businesses (up to 50 users). It aims to provide a lightweight alternative to heavyweight SaaS platforms, enabling users to create, send, and manage invoices in a few clicks—without monthly fees or complicated setup. The system supports client management, invoice generation, PDF export, email delivery, and automated reminders via scheduled background tasks. Designed to be modular, secure, responsive, and developer-friendly, it uses mostly free-tier services like Vercel, Filess.io, and SMTP email. 🚀 Key Features Client Management Add/edit/delete client details View client history and associated invoices Invoice Creation Multi-line item invoices (description, qty, unit price, tax) Due dates, payment status (sent, paid, overdue, etc.) Inline form validation for error-free entry PDF Invoice Generation On-demand PDF creation via API (puppeteer or pdf-lib) Download in browser or auto-send via email (no server storage) Invoice Emailing via SMTP Custom email message templates Attach generated PDFs Track send success/failure Scheduled Reminders Vercel Cron Jobs check for overdue invoices daily Auto-send reminder emails to clients Enable/disable per invoice User Roles & Auth Admin: Full access Shop Assistant: Limited access Auth via manual login (JWT/cookies), roles stored in DB Dashboard UI Key metrics: Total invoices, pending reminders Quick actions: New invoice, view clients Mobile-responsive UI with light/dark mode Data Export / Backup Manual or scheduled export to JSON/CSV Optional GitHub or email backup integration 👥 User Roles RoleCapabilitiesAdminFull access to all records, features, and settingsShop AssistantLimited to creating/editing invoices and clients; no deletion or settings access 🛠️ Suggested Tech Stack Frontend Next.js (App Router) Tailwind CSS or Chakra UI React Hook Form for form logic and validation Zod or Yup for schema validation Backend Next.js API Routes (Serverless via Vercel) Filess.io (PostgreSQL or MySQL) – Free-tier cloud database JWT or Iron-Session for session management Nodemailer for SMTP-based email delivery Puppeteer or pdf-lib for on-the-fly PDF creation Vercel Cron Jobs for automated invoice checks DevOps & Hosting Vercel Free Tier: Frontend + backend hosting + cron .env variables for secrets (SMTP, DB credentials) 🏗️ Basic System Architecture (High-Level) [Client Browser] | |--> [Next.js Frontend (Tailwind UI)] |--> Login (JWT) -> Session Cookies |--> Dashboard, Forms, Views | |--> [API Routes on Vercel Serverless] |--> Auth Routes |--> Client CRUD |--> Invoice CRUD |--> PDF Generation |--> Emailing via SMTP |--> Scheduled Tasks (Reminders) | |--> [Filess.io Database] |--> Users Table |--> Clients Table |--> Invoices Table |--> Logs Table | |--> [Email Provider (SMTP)] |--> Gmail, Mailgun, or custom SMTP | |--> [Vercel Scheduled Functions] |--> Daily Overdue Invoice Check 🧠 Optional Advanced Features (Future Additions) Multi-currency & Language Support Support international clients Analytics Dashboard Monthly revenue charts, client stats Activity Logging Track who edited/created what and when Client Portal Give clients limited access to view/pay invoices Payment Gateway Integration Stripe, PayPal, etc. for collecting payments Multi-user Team Support Multiple assistants per admin account 🌍 Why This Project Matters / Impact Freelancers, small teams, and local shops often don’t need a full-fledged SaaS like QuickBooks or FreshBooks—they just want clean invoices, sent on time, from a tool they control. Most invoicing tools: Lock users into expensive subscriptions Are bloated with unnecessary features Lack customization or offline-friendly capabilities FreelanceFlow is a 100% self-managed, privacy-conscious, and cost-free alternative, offering a full professional workflow (invoices, PDFs, email, reminders) without complexity. It empowers users to own their data, avoid SaaS lock-in, and still look professional. ✅ Final Deliverable (MVP Definition) A production-ready, hosted invoice management system that includes: Clean, mobile-responsive UI Auth with two roles Add/edit/delete clients and invoices PDF generation on demand Email delivery of invoices Daily cron-based reminders Secure route protection Fully hosted (free-tier services) Prompt: Build a minimal but professional invoice and client management app for small businesses using: Next.js, Tailwind, Filess.io, and Vercel Auth (JWT/cookie) with admin/assistant roles Serverless API routes for invoice/client CRUD, PDF generation, email via SMTP Vercel Cron Jobs for reminders No long-term storage of PDFs Clean, responsive UI with dashboard Send emails with PDFs and allow download Export data manually or via scheduled backup Design the system to be modular, privacy-focused, and free to host. Use best practices for security, code organization, and user experience.