Below is a **professional, deployment‑ready, auto‑generated `README.md`** for your GitHub repository.

You can copy/paste this directly into your **README.md** file.  
It is formatted for GitHub, Vercel, and open‑source clarity.

***

# 🌳 Imani Green Health Advocate – Tree Maintenance Scheduler

A modern React + Vite scheduling application designed to help Chicago residents coordinate **Annual Tree Maintenance** visits with Imani Green Health Advocates.  
This application supports **guest scheduling**, **admin dashboards**, and **automatic ICS calendar downloads** using the correct **America/Chicago** timezone.

***

## ✨ Features

### 👥 Guest Scheduler

*   Select **three potential dates**
*   Choose preferred day of the week
*   Pick from 45‑minute time slots
*   Add custom notes for the field crew
*   Automatically download an **ICS file** titled  
    **“Imani Green Health Advocate Annual Tree Maintenance”**

### 🔐 Admin Dashboard

*   Access controlled by a **secret URL token**
*   View all bookings:
    *   Guest name
    *   Email
    *   Preferred day
    *   Time slot
    *   Selected dates
    *   Additional notes

### 📅 ICS Calendar Integration

The app generates **standards‑compliant** ICS files using:

*   **IANA time zone:** `America/Chicago`
*   **Full VTIMEZONE block** (required by Outlook Desktop)
*   **Local timestamps**, not UTC
*   **UID, DTSTAMP, SUMMARY, DESCRIPTION** fields

Timezone behavior follows best practices described in ICS compliance guides, which emphasize:

*   Avoiding abbreviations like `CST` and using IANA zones like `America/Chicago` instead, since Google Calendar, Apple Calendar, and Outlook Web reject non‑IANA TZIDs. [\[schooldigger.com\]](https://www.schooldigger.com/go/IL/zip/60628/search.aspx)
*   Including a full `VTIMEZONE` component to satisfy Outlook Desktop, which requires explicit daylight‑savings definitions to avoid incorrect event shifting. [\[publicscho...review.com\]](https://www.publicschoolreview.com/illinois/chicago/60628)

***

## 🚀 Live Deployment (Vercel)

This project is optimized for **instant deployment** using Vercel’s Static Build system.

### Guest (Public) URL

    https://YOUR-APP.vercel.app/

### Admin (Secret) URL

    https://YOUR-APP.vercel.app/?key=YOUR_ADMIN_SECRET

The admin token is stored inside `vercel.json`:

```json
"env": {
  "ADMIN_SECRET": "greenhealth-47d9f7b0c2e84"
}
```

***

## 📦 Project Structure

    imani-tree-scheduler/
    │
    ├── public/
    │   └── index.html
    │
    ├── src/
    │   ├── App.jsx
    │   ├── main.jsx
    │   ├── index.css
    │   └── components/
    │       ├── ui/
    │       │   ├── button.jsx
    │       │   ├── card.jsx
    │       │   └── card-content.jsx
    │       └── scheduler/
    │           └── TreeMaintenanceScheduler.jsx
    │
    ├── package.json
    ├── vite.config.js
    ├── vercel.json
    └── README.md

***

## 🛠️ Development Setup

### 1. Install dependencies

```bash
npm install
```

### 2. Run the development server

```bash
npm run dev
```

### 3. Build for production

```bash
npm run build
```

### 4. Preview the build locally

```bash
npm run preview
```

***

## 🌐 Deploying to Vercel

1.  Push this repository to GitHub
2.  Visit **<https://vercel.com/new>**
3.  Import your GitHub repo
4.  Deploy 🚀

Vercel automatically reads:

*   `package.json` → build commands
*   `vite.config.js` → bundler config
*   `vercel.json` → routing + environment variables

***

## 🔧 Environment Variables

You can set environment variables directly in Vercel:

    ADMIN_SECRET=greenhealth-47d9f7b0c2e84

Used to grant access to the Admin Dashboard:

```jsx
?key=greenhealth-47d9f7b0c2e84
```

***

Built for **Imani Green Health Advocates** to support community tree care and environmental wellness across Chicago neighborhoods.

***
