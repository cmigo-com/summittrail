# Maqka Summit Adventures and Tours — Booking Platform

A custom-built hiking/trekking website with a public site, a client dashboard, and an
admin dashboard for managing treks and bookings. Payments are manual (bank transfer / cash) —
admin marks bookings as paid once payment is confirmed.

## Stack
- **Backend:** Node.js + Express + SQLite (via `better-sqlite3`), JWT auth
- **Frontend:** React + Vite, React Router

## Project structure
```
maqka-summit/
  backend/     Express API + SQLite database
  frontend/    React client (public site, client dashboard, admin dashboard)
```

## 1. Backend setup

```bash
cd backend
npm install
cp .env.example .env
# edit .env: set JWT_SECRET, ADMIN_EMAIL, ADMIN_PASSWORD
npm run seed     # creates the admin user + sample Mt Kenya treks
npm run dev      # starts API on http://localhost:4000
```

Default admin login (unless you changed `.env` before seeding):
- Email: whatever you set as `ADMIN_EMAIL` (default `admin@maqkasummit.com`)
- Password: whatever you set as `ADMIN_PASSWORD` (default `ChangeMe123!`)

**Change these before deploying anywhere real.**

## 2. Frontend setup

```bash
cd frontend
npm install
npm run dev      # starts React app on http://localhost:5173
```

The Vite dev server proxies `/api/*` requests to `http://localhost:4000`, so run the backend
first. Open http://localhost:5173 in your browser.

## What's included

**Public site**
- Home page with hero, "why choose us," featured treks
- Trek listing + detail pages pulling from the database (title, route, duration,
  difficulty, price, highlights, upcoming departure dates)
- Booking form on each trek page (creates a "pending" booking; no online payment,
  just bank transfer / cash noted on the booking)
- Register / Login

**Client dashboard** (`/dashboard`)
- List of the logged-in client's own bookings with trek, dates, guest count, total price,
  status, payment status, and any notes/admin replies

**Admin dashboard** (`/admin`, admin role only)
- **Overview** — booking counts, paid revenue, client count, upcoming departures
- **Bookings** — table of all bookings; change status (pending/confirmed/paid/completed/cancelled),
  change payment status (unpaid/partial/paid), add an admin note visible to the client, cancel
  a booking (auto-releases the reserved slots)
- **Treks** — create/edit/deactivate treks, add new departure dates with slot counts
- **Clients** — list of registered clients

## Next steps / ideas to extend this
- Add photo galleries per trek (you have great Mt Kenya shots already — Point Lenana,
  the giant groundsels, Shipton's Camp, etc.)
- Add an "altitude sickness & safety" content page — you already have solid source material
  for this from your own prep materials
- Email notifications (booking received, booking confirmed, payment received) via a service
  like Resend or Nodemailer + Gmail SMTP
- Packing list / gear checklist per trek, downloadable as PDF
- Reviews/testimonials from past trekkers
- Multi-image upload for admin (currently image_url is a single URL/path field — swap in
  an upload service like Cloudinary or S3 when ready)
- Guide/porter assignment per departure, if you want to track staffing per trip
- Move from SQLite to Postgres when you're ready to deploy at scale (the SQL is simple
  enough that migration is straightforward)

## Deployment notes
- Backend: any Node host (Render, Railway, a VPS). Point it at a persistent disk for the
  SQLite file, or migrate to Postgres.
- Frontend: `npm run build` in `frontend/` produces a static `dist/` folder deployable to
  Netlify, Vercel, or any static host — just make sure `/api` requests are proxied/rewritten
  to your backend's URL in production.
