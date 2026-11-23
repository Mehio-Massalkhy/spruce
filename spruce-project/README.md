# Spruce

Spruce is a mobile housekeeping marketplace built with React Native, Expo and Expo Router for the
Mobile Application Development with Expo assignment. It connects people who need their home cleaned
with vetted cleaning companies and independent cleaners nearby, and gives providers a studio to
manage their public listing and team.

## The problem

Booking a home cleaner is usually a chain of phone calls and unanswered messages. Prices are opaque,
availability is unclear and independent cleaners have no easy way to be discovered next to larger
companies. Spruce makes the whole exchange visible: browse by distance, compare hourly rates and
ratings, check real working hours on a calendar, and book a specific cleaner for a specific number
of hours.

## Users

- **Customers** who want a one-off or regular clean and care about price, availability and trust.
- **Cleaning companies** who list their catalogue of cleaners, each with their own rate, nationality,
  profile and weekly schedule.
- **Independent cleaners** who list themselves, set their own hours and manage their own profile.

## Demo accounts

Login is mock-only and checked in the app. Every account uses the password `cleanhome`.

| Login tab | Email | Who |
| --- | --- | --- |
| Customer | `rami@spruce.app` | Rami Fares, Achrafieh |
| Independent | `rana@spruce.app` | Rana Khalil, Hamra |
| Company | `hello@cedrushome.lb` | Cedrus Home Care, Achrafieh |

The login screen has three separate doors: Customer, Independent and Company. Each tab lists only
its own demo account, and a "Use" button fills the form.

## What you can do

**As a customer**

1. Welcome screen with entry points for customers and providers.
2. Sign in with Zod-validated mock credentials.
3. Browse six providers near Achrafieh, Beirut with search, company/independent filters, distance, hourly
   rate and working-day filters.
4. Open a company to see its catalogue, filtered by price per hour and availability, or open an
   independent cleaner's profile and weekly hours.
5. Pick a day on the availability calendar, choose a start time and hours (2–8), enter an address and
   confirm. The total includes the hourly rate and a $2 service fee.
6. Review upcoming and past bookings, reschedule a booking with the same calendar, or cancel it after
   a confirmation dialog.
7. Sign out from the account tab and return to the welcome screen.

**As a provider**

1. Sign in through the separate Independent or Company tab — the tab bar switches to Studio.
2. Upload a profile picture from the photo library (or keep an illustrated avatar), then edit the
   name, tagline, area, nationality, hourly rate, services and bio.
3. Independent cleaners also edit their weekly working hours; bookings can only start inside those
   windows.
4. Company accounts manage their team catalogue: add, edit and remove cleaners, each with a photo,
   nationality, rate, services, bio, availability toggle and weekly schedule.
5. Preview the public listing exactly as a client sees it.

## Screens and flow

```
Welcome → Login (Customer / Independent / Company)
Customer:  Discover → Provider details → Book (calendar → time → hours → address) → Confirmation
           Discover ↔ Bookings → Booking details (edit / cancel) ↔ Account (sign out)
Provider:  Studio → Edit listing
                  → Add / edit team member
                  → Public listing preview
```

Navigation is handled entirely with Expo Router (nested stack, role-aware JavaScript tabs, dynamic
routes for providers, cleaners and bookings). Back navigation is available on every stacked screen.

## Project structure

```
src/
  app/                 Expo Router routes only
    (app)/(tabs)/      Discover, Bookings, Studio, Account
    (app)/provider/    Provider details and catalogue
    (app)/book/        Calendar + booking flow
    (app)/booking/     Booking details, edit and cancel
    login.tsx          Mock login with Customer / Independent / Company doors
    index.tsx          Welcome screen
  components/
    ui/                Reusable primitives (Button, TextField, Calendar, Toast, Dialog, …)
    cards/             Provider, cleaner and booking cards
  context/             App state: session, catalogue, bookings, AsyncStorage persistence
  lib/                 Types, mock catalogue, validation schemas, schedule helpers, theme
```

## Validation and data

- **Zod** validates login, booking, provider listing and team member forms. Errors are mapped to
  fields; invalid submissions never reach the state layer.
- **AsyncStorage** persists the session, the full provider catalogue (including provider edits and
  team changes) and bookings. Keys are versioned in `src/lib/storage.ts`.
- Uploaded profile photos are cropped, resized to 480 px and stored as compressed base64 alongside
  the listing, so they survive a restart without a file server.
- Every destructive action asks first: cancelling a booking, removing a team member and signing out
  all use the same confirmation dialog.
- Deleting every booking persists an empty list rather than re-seeding, so the empty state survives a
  restart.
- Loading, empty, no-results, success and failure states are implemented throughout. Toasts confirm
  saves and deletions; storage failures surface as inline messages.

## Run locally

```bash
npm install
npx expo start        # then press w for web, or scan the QR code in Expo Go
# or
npm run web
```

Typecheck:

```bash
npm run typecheck
```



