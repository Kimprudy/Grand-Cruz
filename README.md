
# Grand Cruz Hotel — Automation Portfolio Project

A fully automated hotel management system built as a portfolio project to demonstrate 
end-to-end automation expertise using Make.com, Airtable, Typeform, and React.

---

## Project Overview

Grand Cruz is a fictional hotel property used to simulate a real-world hotel 
management system. The project covers the full guest lifecycle — from reservation 
intake to post-checkout review requests — with automated workflows, email 
communications, housekeeping management, and a live operations dashboard.

---

## Tech Stack

| Tool | Role |
|------|------|
| Make.com | Automation engine — all scenario workflows |
| Airtable | Database — Hotel Operations Hub |
| Typeform | Guest-facing reservation intake form |
| Gmail | Transactional email delivery |
| React | Live hotel operations dashboard |
| GitHub | Version control |
| Vercel | Dashboard deployment |

---

## Airtable Base — Hotel Operations Hub

### Tables
1. **Guests** — Guest profiles, tags, booking history
2. **Reservations** — All reservation records with status tracking
3. **Rooms** — Room inventory with live status
4. **Housekeeping Tasks** — Task assignment and completion tracking
5. **Staff** — Hotel staff profiles and zone assignments
6. **Feedback & Reviews** — Review request tracking and review storage
7. **Daily Operations Log** — Auto-generated daily snapshot of hotel metrics
8. **Guest Communications** — Full audit log of all automated communications

---

## Make.com Scenarios

### Scenario 1 — Guest & Reservation Creation
**Trigger:** Typeform webhook (instant)

Fires when a guest submits the reservation form. Checks if the guest already 
exists in Airtable by email. If they exist, updates their profile and tags them 
as Returning Guest. If new, creates their profile and tags them as New Guest. 
Creates a reservation record in both cases and logs the communication.

---

### Scenario 2 — Pre-Arrival Guest Communication
**Trigger:** Schedule — Every day at 9:00 AM

Searches for reservations with check-in dates 3 days and 1 day from today. 
Routes each reservation to the correct email path and sends a branded HTML 
pre-arrival email. Marks the email as sent to prevent duplicates and logs 
the communication.

---

### Scenario 3 — Housekeeping Task Automation
**Trigger:** Schedule — Every day at 11:00 AM

Finds all reservations with today's checkout date and Checked Out status. 
For each checkout, fetches room details, finds an available housekeeper in 
the same zone, creates a housekeeping task, updates the room status to 
Dirty - Awaiting HK, and sends the housekeeper a branded email notification 
with an Airtable room link.

---

### Scenario 4 — Review Request & Thank You
**Trigger:** Schedule — Every day at 2:00 PM

Finds today's checkouts where Review Requested is false. Routes each guest 
to the correct review platform email based on their Booking Source — 
Booking.com, Expedia, Airbnb, or Google (fallback). Marks Review Requested 
as true, creates a record in Feedback & Reviews, and logs the communication.

---

### Scenario 5 — Reservation Status Management
**Trigger:** Schedule — Every day at 12:00 PM

Handles two status updates:
- **No Shows** — Finds reservations where check-in date has passed and 
  status is still Confirmed. Updates status to No Show.
- **Cancellations** — Finds cancelled reservations where Cancellation Email 
  Sent is false. Sends a branded HTML cancellation email with a Typeform 
  link for rebooking. Marks email as sent and logs the communication.

---

### Scenario 6 — Daily Operations Dashboard
**Trigger:** Schedule — Every day at 7:00 AM

Collects six live metrics from Airtable — arrivals, departures, in-house 
guests, available rooms, pending housekeeping tasks, and completed 
housekeeping tasks. Uses numeric aggregators after each Search Records 
module to prevent bundle multiplication. Creates a single daily log record 
in the Daily Operations Log table and sends one branded HTML report email 
to the hotel manager.

---

## Email Templates

All emails are branded with Grand Cruz dark navy (#1a1a2e) and gold (#c9a84c) 
color scheme.

| Template | Trigger |
|----------|---------|
| Pre-Arrival 3-Day | Scenario 2 — 3 days before check-in |
| Pre-Arrival 1-Day | Scenario 2 — 1 day before check-in |
| Housekeeping Notification | Scenario 3 — on checkout |
| Booking.com Review Request | Scenario 4 — Booking.com guests |
| Expedia Review Request | Scenario 4 — Expedia guests |
| Airbnb Review Request | Scenario 4 — Airbnb guests |
| Google Review Request | Scenario 4 — all other guests |
| Cancellation Confirmation | Scenario 5 — cancelled reservations |
| Daily Operations Report | Scenario 6 — 7:00 AM every day |

---

## React Dashboard

A live hotel operations dashboard that pulls real-time data directly from 
Airtable via the Airtable API.

### Features
- Live stat cards — Arrivals, Departures, In-House Guests, Available Rooms, 
  Pending HK Tasks, Completed HK Tasks
- Today's Arrivals table
- Today's Departures table
- Housekeeping Tasks board with status badges
- Room Status grid with color-coded availability

### Running Locally
```bash
cd grand-cruz-dashboard
npm install
npm start
