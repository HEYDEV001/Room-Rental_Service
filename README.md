# Room-Rental_Service

> **Raj PG Services** — A fully responsive, static-frontend web application for showcasing and browsing premium Paying Guest (PG) accommodation in Indore, Madhya Pradesh.

 **Live Demo:** [room-rental-service-sooty.vercel.app](https://raj-pg-service.vercel.app/)
&nbsp;&nbsp;|&nbsp;&nbsp;
 **Repository:** [github.com/HEYDEV001/Room-Rental_Service](https://github.com/HEYDEV001/Room-Rental_Service)

---

##  Project Objective

The goal of this project is to build a professional, conversion-focused website for **Raj PG Services** — a real-world paying guest accommodation business operating across three locations in Indore. The platform bridges the gap between prospective tenants and the PG owner by:

- Presenting available rooms with real photos, pricing, amenities, and ratings in a clean, scannable UI
- Enabling visitors to filter, search, and sort rooms without any backend or page reload
- Providing an enquiry/contact form with real-time client-side validation so potential tenants can express interest directly
- Delivering a complete, production-grade user experience on any device — desktop, tablet, or mobile

The project is intentionally built as a **zero-backend, zero-dependency static site** (HTML + CSS + Vanilla JS) so it can be deployed instantly on platforms like Vercel with no build step, no server costs, and no maintenance overhead.

---

##  Project Structure

```
Room-Rental_Service/
└── raj-pg-improved/          # Main application folder
    ├── index.html             # Home page — Hero, Featured Rooms, How It Works, Testimonials
    ├── listings.html          # Browse Rooms page — Full room catalogue with sidebar filters
    ├── detail.html            # Room Detail page — Individual room view with full info
    ├── contact.html           # Contact / Enquiry page — Validated enquiry form
    │
    ├── css/
    │   ├── base.css           # CSS reset, design tokens (variables), typography, utility classes
    │   ├── components.css     # Reusable UI components — cards, badges, buttons, pills, testimonials
    │   ├── layout.css         # Page-level layout — navbar, footer, grid systems, sidebar layout
    │   ├── pages.css          # Page-specific styles — hero, trust strip, CTA banner, steps
    │   └── listings-enhanced.css  # Listings-specific styles — filter drawer, chips, view toggle
    │
    ├── js/
    │   ├── utils.js           # Shared helpers — room card renderer, star generator, scroll-to-top, debounce
    │   ├── nav.js             # Navbar scroll effect and hamburger menu toggle
    │   ├── listings.js        # Core listings render logic
    │   ├── listings-enhanced.js   # Advanced filter engine — live filtering, sort, chips, mobile drawer
    │   └── contact.js         # Enquiry form validation with real-time feedback
    │
    ├── data/
    │   └── rooms.js           # Central room data store — 8 room objects with all metadata
    │
    └── images/                # Real property photographs (WhatsApp exports) + placeholder SVG
```

**Architecture at a glance:** Data lives in `data/rooms.js` as a plain JS array. `utils.js` exposes a shared `renderRoomCard()` helper used by both the home page and the listings page, ensuring DRY rendering. All filtering, sorting, and search logic runs entirely in the browser via `listings-enhanced.js`.

---

##  Key Features

###  Home Page (index.html)
- **Hero section** with a contextual search widget — filter by location, stay duration, and room type before navigating to listings
- **Live stats strip** — 32+ rooms, 3 locations, 200+ residents, 4.8★ average rating
- **Trust strip** — scrollable icons highlighting CCTV, RO Water, Parking, Housekeeping, and Electricity inclusion
- **Featured rooms grid** — dynamically rendered from `rooms.js` via JavaScript (first 6 rooms), clickable cards leading to the detail page
- **"How It Works"** — 3-step process section for first-time visitors
- **Testimonials** — 3 resident testimonials with avatar, name, and designation
- **CTA banner** — dual call-to-action guiding users to listings or the contact form

###  Listings Page (listings.html)
- **Real-time text search** — filters rooms by name, location, type, or amenities as the user types, with a one-click clear button
- **Multi-dimensional sidebar filters:**
  - Location (Pardesipura / Near Aurobindo)
  - Room type (Single / Double / Triple / Budget)
  - Price slider (₹2,500 – ₹3,500) with dynamic fill animation
  - Amenity checkboxes (CCTV, RO Water, Wi-Fi, Parking, Housekeeping)
  - Minimum rating selector
- **Active filter chips** — each applied filter renders as a dismissible chip; clicking ✕ on a chip individually removes that filter and re-runs the query
- **Sort control** — sort by Recommended, Price: Low to High, Price: High to Low, or Top Rated
- **Grid / List view toggle** — switches the room grid between card layout and list layout
- **Animated "No Results" state** with reset and contact shortcuts
- **Mobile filter drawer** — sidebar filters are cloned into a slide-up bottom drawer on small screens, with Apply and Reset actions, keeping the mobile experience clean

###  Room Detail Page (detail.html)
- Displays full details for a room selected from the grid (room ID passed via URL query parameter)
- Shows the room gallery, amenity list, description, price, rating, and availability badge
- Direct call-to-action linking to the contact page

###  Contact Page (contact.html)
- Enquiry form with fields: Name, Email, Phone, Preferred Location, Room Type, Move-In Date, and Message
- **Real-time field validation** on blur and on subsequent input:
  - Name: minimum 2 characters
  - Email: standard regex pattern
  - Phone: Indian 10-digit format with optional `+91` prefix
  - Required dropdowns validated on submit
- Inline error messages that appear and disappear dynamically
- **Success state** — form hides and a confirmation panel slides in on valid submission, with a "Send Another Enquiry" button to reset

###  Design & UX
- Custom CSS design system with CSS variables for consistent theming (navy accent `#1B3A6B`, warm backgrounds, smooth shadows)
- **Playfair Display** for headings, **Lato** for body — imported via Google Fonts
- Smooth scroll behaviour, cubic-bezier transitions throughout
- Sticky navbar with a scroll-triggered shadow effect
- Hamburger menu for mobile with an animated slide-down overlay
- Lazy-loaded images with an SVG placeholder fallback on error
- Scroll-to-top button that appears after 400px of scroll

---

##  Challenges Faced & Optimisations Made

### 1. No Backend — Dynamic UI with Pure Vanilla JS
**Challenge:** The project needed dynamic room listing, filtering, and sorting without any server, database, or framework.

**Optimisation:** All room data was centralised in a single `data/rooms.js` file as a structured JS object array. A shared `renderRoomCard()` function in `utils.js` ensures that both the home page (featured grid) and the listings page produce identical card markup from the same data source — eliminating duplication and making future room additions a single-file edit.

---

### 2. Multi-Filter State Management Without a Framework
**Challenge:** Simultaneous filtering by location, type, price, amenities, rating, and search — with each filter independently removable via chips — is complex state management for plain JS.

**Optimisation:** A single `applyFilters()` function in `listings-enhanced.js` reads all filter inputs at once and recomputes `filteredRooms` from the master `rooms` array on every change. This "compute from source" pattern avoids stale state bugs. Active filter state is represented by the DOM itself (checkbox `.checked`, slider `.value`, etc.), avoiding a separate state object that could fall out of sync.

---

### 3. Mobile Filter Experience
**Challenge:** A sidebar with 5+ filter categories is unusable on small screens if just hidden behind a media query.

**Optimisation:** A dedicated mobile filter drawer was implemented using a DOM cloning approach — on mobile trigger, the sidebar's inner HTML is deep-cloned into a bottom drawer (`filter-drawer`), the drawer's inputs are synced to current filter state, and on "Apply" the drawer's values are written back to the sidebar before `applyFilters()` runs. This means the sidebar and drawer always stay in sync without maintaining two separate sets of event listeners.

---

### 4. CSS Architecture at Scale
**Challenge:** A multi-page site with shared components risks monolithic CSS files that are hard to maintain and can cause specificity conflicts.

**Optimisation:** CSS was split into five purposeful layers — `base.css` (reset + variables), `components.css` (reusable UI atoms), `layout.css` (page scaffolding), `pages.css` (section-specific rules), and `listings-enhanced.css` (feature-specific overrides). This separation makes each file independently editable and keeps specificity flat. Design tokens (`--accent`, `--radius`, `--transition`, etc.) defined in `:root` ensure visual consistency without hard-coded values scattered across files.

---

### 5. Image Reliability with Real-World Photos
**Challenge:** The project uses actual WhatsApp-exported property photos with long, space-containing filenames. Broken images would significantly hurt perceived quality.

**Optimisation:** All `<img>` tags include a native `onerror` handler that swaps to a local `placeholder.svg` if any image fails to load. Images also carry `loading="lazy"` attributes to prevent them from blocking the initial render.

---

### 6. Form Validation UX
**Challenge:** A simple "submit and show all errors at once" pattern creates a frustrating experience for users filling out a multi-field enquiry form.

**Optimisation:** `contact.js` implements a two-stage validation strategy — fields show errors on `blur` (when the user leaves the field), and errors are cleared immediately on `input` once the user starts correcting them. This means the form provides feedback at exactly the right moment: not prematurely while typing, and not only after a failed submit.

---

##  Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 (semantic elements, ARIA labels) |
| Styling | Vanilla CSS3 (custom properties, flexbox, grid, media queries) |
| Scripting | Vanilla JavaScript (ES6+, DOM API, no frameworks) |
| Fonts | Google Fonts (Playfair Display, Lato) |
| Data | Plain JS module (`rooms.js`) |
| Deployment | Vercel (static hosting) |

---

##  Getting Started

No build tools or package managers required.

```bash
# 1. Clone the repository
git clone https://github.com/HEYDEV001/Room-Rental_Service.git

# 2. Navigate into the project folder
cd Room-Rental_Service/raj-pg-improved

# 3. Open in your browser
#    Option A — open index.html directly in a browser
#    Option B — use a local dev server (recommended to avoid CORS on images)
```

Then visit `http://localhost:8080` (or `http://localhost:3000` with `serve`).

---

##  Business Details

| Detail | Info |
|---|---|
| Business Name | Raj PG Services |
| City | Indore, Madhya Pradesh |
| Locations | Pardesipura (2 properties) · Near Aurobindo Hospital (1 property) |
| Price Range | ₹2,500 – ₹3,500 / month (utilities included) |
| Total Rooms | 32+ |
| Contact | +91 9826705696 |
| Instagram | [@ydv_raj_0143](https://www.instagram.com/ydv_raj_0143) |

---

