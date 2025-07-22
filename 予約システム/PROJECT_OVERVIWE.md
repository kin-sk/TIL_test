# Project Overview: Dental Clinic Booking System

## 1. Project Goal

This project is a web-based booking system for a dental clinic. It allows patients to book appointments online and enables administrators to manage bookings and clinic information. A key feature is the ability for patients to request a pickup service.

## 2. Technology Stack

| Technology | Role |
| :--- | :--- |
| **Next.js** | React framework for frontend and backend. Enables SSR, SSG, and SEO optimization. |
| **TypeScript** | Superset of JavaScript for static typing, improving code quality and maintainability. |
| **Chakra UI** | UI component library for building accessible and responsive user interfaces quickly. |
| **Firebase** | Backend-as-a-Service (BaaS) for database, authentication, and serverless functions. |
| **Vercel** | Hosting platform for deploying the Next.js application, with CI/CD integration. |

## 3. Core Features

### For Patients (Public Site)
-   View clinic information, services, and access details.
-   Check appointment availability on a calendar.
-   Book an appointment by selecting a menu, date, and time.
-   **Request a pickup service during booking.**
-   (Optional) Register/Login to view booking history and manage appointments.

### For Administrators (Admin Dashboard)
-   Secure login to the admin panel.
-   Dashboard to view today's appointments and pickup requests.
-   Full CRUD (Create, Read, Update, Delete) functionality for bookings.
-   Manage clinic settings (e.g., business hours, holidays).
-   Manage treatment menus (e.g., name, price, duration).

## 4. Directory Structure

/
├── app/ # Next.js App Router.
│ ├── (public)/ # Routes for the public-facing site.
│ ├── (admin)/ # Routes for the admin dashboard (requires auth).
│ └── api/ # API Routes (Route Handlers).
├── components/ # Reusable React components.
│ ├── ui/ # Generic UI parts (e.g., Button, Input).
│ └── features/ # Feature-specific components (e.g., BookingCalendar).
├── lib/ # Helper functions and external service integrations.
│ └── firebase.ts # Firebase initialization and helper functions.
└── types/ # Global TypeScript type definitions.

## 5. Database Schema (Firebase Firestore)

The database consists of the following collections:

### `shopSettings` (Singleton document, id: `main`)
Stores the clinic's general settings.
- `shopName` (string)
- `address` (string)
- `openingHours` (array)
- `holidays` (array)
- `bookingInterval` (number)

### `menus`
Stores information about available treatments.
- `name` (string)
- `description` (string)
- `price` (number)
- `duration` (number)
- `isActive` (boolean)

### `users` (Document ID = Firebase Auth UID)
Stores information for registered patients.
- `email` (string)
- `name` (string)
- `phoneNumber` (string)

### `bookings`
Stores individual appointment data. **This is the most critical collection.**
- `startTime` (timestamp): **Primary key for querying availability.**
- `endTime` (timestamp)
- `menuId` (string)
- `menuName` (string)
- `userId` (string | null)
- `customerInfo` (map): `{name, email, phone}`
- `needsPickup` (boolean): **`true` if pickup service is requested.**
- `status` (string): e.g., "confirmed", "cancelled"
- `createdAt` (timestamp)

## 6. Development Workflow

-   **Issue-driven**: All development tasks are managed as GitHub Issues.
-   **Git Branching**: Use `feature/issue-number-name` branches based on the `develop` branch.
-   **Commit Messages**: Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification.