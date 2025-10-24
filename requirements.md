# 🏠 Airbnb Clone Backend — Requirement Specifications

## 📘 Overview

This document defines the **technical and functional requirements** for the key backend features of the Airbnb Clone system.  
It serves as a guide for developers, testers, and project managers to ensure that the backend system is implemented correctly, securely, and efficiently.

---

## 📚 Table of Contents

1. [Feature 1: User Authentication & Authorization](#feature-1-user-authentication--authorization)
2. [Feature 2: Property Management](#feature-2-property-management)
3. [Feature 3: Booking System](#feature-3-booking-system)
4. [Non-Functional Requirements](#non-functional-requirements)
5. [API Design Standards](#api-design-standards)

---

## 🧩 Feature 1: User Authentication & Authorization

### Functional Description

Handles secure user registration, login, and session management. Supports both **guests** and **hosts** with different access levels.

### Core Functionalities

- Register new users (guest or host).
- Login and generate JWT tokens.
- Authenticate protected routes using middleware.
- Role-based access control (RBAC) for admin, host, and guest.

### API Endpoints

| Method  | Endpoint                | Description                      | Auth Required |
| ------- | ----------------------- | -------------------------------- | ------------- |
| `POST`  | `/api/v1/auth/register` | Register a new user (guest/host) | ❌            |
| `POST`  | `/api/v1/auth/login`    | Authenticate user and issue JWT  | ❌            |
| `GET`   | `/api/v1/auth/profile`  | Retrieve user profile info       | ✅            |
| `PATCH` | `/api/v1/auth/profile`  | Update user profile              | ✅            |

### Input / Output Specification

**Example Request — Registration**

```json
POST /api/v1/auth/register
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "Secure@123",
  "role": "host"
}
```

**Example Response**

```json
{
  "message": "User registered successfully",
  "user": {
    "id": "uuid",
    "name": "John Doe",
    "email": "john@example.com",
    "role": "host"
  },
  "token": "eyJhbGciOi..."
}
```

### Validation Rules

| Field      | Rule                                                            |
| ---------- | --------------------------------------------------------------- |
| `email`    | Must be unique, valid email format                              |
| `password` | Minimum 8 chars, at least 1 uppercase, 1 number, 1 special char |
| `name`     | Required, alphabetic characters only                            |
| `role`     | Must be either “guest” or “host”                                |

### Performance Criteria

- Authentication requests must complete within **≤ 300 ms** under normal load.
- JWT tokens expire after **24 hours**.
- Passwords are **hashed using bcrypt** with at least 10 salt rounds.

---

## 🏡 Feature 2: Property Management

### Functional Description

Allows **hosts** to create, update, delete, and manage property listings.
**Guests** can view listings and search using filters.

### Core Functionalities

- Hosts can add new listings with title, description, location, price, and amenities.
- Guests can search and filter properties.
- Admin can manage and moderate listings.

### API Endpoints

| Method   | Endpoint                 | Description                       | Auth Required |
| -------- | ------------------------ | --------------------------------- | ------------- |
| `POST`   | `/api/v1/properties`     | Create new property               | ✅ Host       |
| `GET`    | `/api/v1/properties`     | Get all properties (with filters) | ❌            |
| `GET`    | `/api/v1/properties/:id` | Get single property details       | ❌            |
| `PATCH`  | `/api/v1/properties/:id` | Update property info              | ✅ Host       |
| `DELETE` | `/api/v1/properties/:id` | Delete property                   | ✅ Host/Admin |

### Input / Output Specification

**Example Request — Add Property**

```json
POST /api/v1/properties
{
  "title": "Luxury Beachfront Villa",
  "description": "Modern villa with ocean view.",
  "price_per_night": 150,
  "location": "Mombasa, Kenya",
  "amenities": ["Wi-Fi", "Pool", "AC"],
  "availability": true
}
```

**Example Response**

```json
{
  "message": "Property created successfully",
  "property": {
    "id": "uuid",
    "title": "Luxury Beachfront Villa",
    "price_per_night": 150,
    "host_id": "uuid"
  }
}
```

### Validation Rules

| Field             | Rule                         |
| ----------------- | ---------------------------- |
| `title`           | Required, max 100 characters |
| `price_per_night` | Required, numeric, positive  |
| `location`        | Required                     |
| `amenities`       | Array of strings             |
| `host_id`         | Must be a valid host account |

### Performance Criteria

- Filter and pagination queries should execute within **500 ms**.
- Use **indexing** on `price`, `location`, and `host_id` for faster lookups.
- Property images stored in **Cloudinary** or AWS S3.

---

## 📅 Feature 3: Booking System

### Functional Description

Manages the process of guests booking properties, preventing double bookings, and tracking booking status.

### Core Functionalities

- Guests can create bookings for available dates.
- Hosts can confirm or cancel bookings.
- Prevent overlapping bookings for the same property.
- Payments processed securely via Stripe or PayPal.

### API Endpoints

| Method  | Endpoint                      | Description                | Auth Required |
| ------- | ----------------------------- | -------------------------- | ------------- |
| `POST`  | `/api/v1/bookings`            | Create new booking         | ✅ Guest      |
| `GET`   | `/api/v1/bookings`            | Get user bookings          | ✅ Guest/Host |
| `PATCH` | `/api/v1/bookings/:id/cancel` | Cancel a booking           | ✅ Guest/Host |
| `GET`   | `/api/v1/bookings/:id`        | Get single booking details | ✅            |

### Input / Output Specification

**Example Request — Create Booking**

```json
POST /api/v1/bookings
{
  "property_id": "uuid",
  "check_in": "2025-12-01",
  "check_out": "2025-12-05",
  "guests": 2
}
```

**Example Response**

```json
{
  "message": "Booking successful",
  "booking": {
    "id": "uuid",
    "status": "confirmed",
    "total_amount": 600
  }
}
```

### Validation Rules

| Field         | Rule                                    |
| ------------- | --------------------------------------- |
| `property_id` | Must exist and be available             |
| `check_in`    | Must be before `check_out`              |
| `check_out`   | Must not overlap existing bookings      |
| `guests`      | Positive integer, <= max guests allowed |

### Performance Criteria

- Booking creation should verify availability in **≤ 400 ms**.
- Use **transactions** to maintain consistency.
- Prevent race conditions using database **row-level locks**.

---

## 🧠 Non-Functional Requirements

| Category           | Description                                                         |
| ------------------ | ------------------------------------------------------------------- |
| **Security**       | All passwords hashed with bcrypt, JWT used for auth, HTTPS enforced |
| **Scalability**    | API must support horizontal scaling                                 |
| **Error Handling** | Consistent JSON error responses                                     |
| **Logging**        | Use Winston or Morgan for API logs                                  |
| **Testing**        | Unit and integration tests using Jest or Pytest                     |
| **Rate Limiting**  | 100 requests/min per user (via express-rate-limit)                  |

---

## 🌐 API Design Standards

- **Naming Convention:** RESTful structure (`/api/v1/resources`)
- **Response Format:** Always JSON
- **Status Codes:**

  - `200 OK` – Successful request
  - `201 Created` – Resource created successfully
  - `400 Bad Request` – Validation or input error
  - `401 Unauthorized` – Missing or invalid credentials
  - `404 Not Found` – Resource not found
  - `500 Internal Server Error` – Unexpected server error

- **Pagination:** Query params `?page=1&limit=10`
- **Sorting/Filtering:** Query params for flexibility (`?sort=price&location=Mombasa`)

---
