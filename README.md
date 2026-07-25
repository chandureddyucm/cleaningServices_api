# Cleaning Services API

A REST API backend for a cleaning services platform, supporting three types of accounts — **users** (customers), **staff** (cleaners), and **admins**. Built with Express and MongoDB (Mongoose).

## Features

- User registration, profile management, and account enable/disable
- Service catalog management (add, modify, enable/disable services)
- Booking flow: book a service, assign staff, complete a booking, cancel, pay, and leave feedback/rating
- Staff assignment and tracking of assigned/completed services
- Admin reporting on staff and user activity

## Tech Stack

- [Node.js](https://nodejs.org/) / [Express](https://expressjs.com/)
- [MongoDB](https://www.mongodb.com/) via [Mongoose](https://mongoosejs.com/)
- [body-parser](https://www.npmjs.com/package/body-parser), [cors](https://www.npmjs.com/package/cors), [uuid](https://www.npmjs.com/package/uuid)

## Project Structure

```
.
├── controllers/   # Request handlers (business logic) per resource
├── models/        # Mongoose schemas (admin, staff, user, service, bookings)
├── routes/        # Express routers, mounted under /api
└── server.js      # App entry point: DB connection, middleware, route mounting
```

## Getting Started

### Prerequisites

- Node.js and npm
- A MongoDB connection string (Atlas or local)

### Installation

```bash
npm install
```

### Configuration

The app currently connects to MongoDB using a hardcoded connection string in [server.js](server.js). Before running it yourself, replace this with your own MongoDB URI (ideally loaded from an environment variable rather than committed to source, since the current string embeds live database credentials).

### Run

```bash
node server.js
```

The server starts on port `3000` by default (`http://localhost:3000`).

## API Overview

All endpoints are mounted under `/api`.

### User (`/api/user`)

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | `/register-user` | Register a new user |
| POST | `/get-users` | List users |
| POST | `/get-user` | Get a single user |
| POST | `/update-user` | Update user profile |
| POST | `/update-user-password` | Update user password |
| POST | `/toggle-user` | Enable/disable a user account |
| POST | `/feedback-service` | Leave feedback/rating for a completed service |
| POST | `/book-service` | Book a service |
| POST | `/cancel-service` | Cancel a booking |
| POST | `/get-services` | List available services |
| POST | `/pay-service` | Pay for a booked service |

### Staff (`/api/staff`)

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | `/register-staff` | Register a new staff member |
| POST | `/get-staffs` | List staff |
| POST | `/get-staff` | Get a single staff member |
| POST | `/update-staff` | Update staff profile |
| POST | `/update-staff-password` | Update staff password |
| POST | `/toggle-staff` | Enable/disable a staff account |
| POST | `/assign-service` | Assign a booking to staff |
| POST | `/complete-service` | Mark a booking as completed |
| POST | `/get-services` | List services |
| POST | `/get-assigned-services` | List services assigned to staff |

### Admin (`/api/admin`)

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | `/register-admin` | Register a new admin |
| GET | `/get-admins` | List admins |
| POST | `/get-admin` | Get a single admin |
| POST | `/update-admin` | Update admin profile |
| POST | `/update-admin-password` | Update admin password |
| POST | `/toggle-staff` | Enable/disable a staff account |
| POST | `/toggle-user` | Enable/disable a user account |
| POST | `/add-service` | Add a new service to the catalog |
| POST | `/modify-service` | Modify an existing service |
| POST | `/toggle-service` | Enable/disable a service |
| POST | `/view-services` | List services |
| POST | `/get-staff-report` | Get a staff activity report |
| POST | `/get-user-report` | Get a user activity report |

## Data Models

- **User / Staff / Admin** — account details (name, mobile, email, username, gender, password) plus an `is_deleted` soft-delete flag.
- **Service** — name, description, time estimate, cost, and an `is_deleted` flag.
- **Bookings** — links a user, service, and (once assigned) staff member; tracks status, cancellation, payment, notes, feedback, and rating.
