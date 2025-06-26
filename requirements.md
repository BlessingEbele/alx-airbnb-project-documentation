# 📄 Airbnb Clone Backend – Requirements Specification

This document outlines the technical and functional requirements for three core backend features: **User Authentication**, **Property Management**, and **Booking System**.

---

## 1️⃣ Feature: User Authentication

### 🔹 Objective
Enable secure registration, login, and authentication for guests, hosts, and admins.

### 🔸 API Endpoints
| Method | Endpoint             | Description               |
|--------|----------------------|---------------------------|
| POST   | /api/auth/register   | Register new user         |
| POST   | /api/auth/login      | Login existing user       |
| GET    | /api/auth/profile    | Retrieve authenticated user |
| PUT    | /api/auth/update     | Update user profile       |

### 🔸 Input/Output Format
**Register**
```json
Input:
{
  "email": "user@example.com",
  "password": "secretpass",
  "role": "guest"
}

Output:
{
  "message": "Registration successful",
  "token": "jwt-token-here"
}
```

**Login**
```json
Input:
{
  "email": "user@example.com",
  "password": "secretpass"
}

Output:
{
  "token": "jwt-token-here",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "guest"
  }
}
```

### 🔸 Validation Rules
- Email must be unique and valid format.
- Password minimum 8 characters, includes letters and numbers.
- Role must be one of `guest`, `host`, or `admin`.

### 🔸 Performance Criteria
- Login and token issuance must complete in under 400ms.
- Token expiration: 1 hour (renewable).

---

## 2️⃣ Feature: Property Management

### 🔹 Objective
Enable hosts to list, update, or delete properties with full details.

### 🔸 API Endpoints
| Method | Endpoint            | Description                     |
|--------|---------------------|---------------------------------|
| POST   | /api/properties     | Create a new property listing   |
| GET    | /api/properties     | List all properties (public)    |
| GET    | /api/properties/:id | View single property details    |
| PUT    | /api/properties/:id | Update property listing         |
| DELETE | /api/properties/:id | Delete property                 |

### 🔸 Input/Output Format
**Add Property**
```json
Input:
{
  "title": "Oceanfront Villa",
  "description": "A cozy beachside escape",
  "price": 150.0,
  "location": "Lagos",
  "amenities": ["Wi-Fi", "Pool", "Air Conditioning"],
  "available_from": "2025-07-01",
  "available_to": "2025-12-31"
}

Output:
{
  "message": "Property listed successfully",
  "property_id": 17
}
```

### 🔸 Validation Rules
- Title and description required.
- Price must be a positive decimal.
- Dates must follow ISO format and `from < to`.
- Authenticated `host` role required for creating listings.

### 🔸 Performance Criteria
- Listings should return paginated (20 per page).
- Filter operations by location, price, and amenities under 500ms.

---

## 3️⃣ Feature: Booking System

### 🔹 Objective
Allow guests to reserve properties for selected dates and process booking lifecycle.

### 🔸 API Endpoints
| Method | Endpoint             | Description                      |
|--------|----------------------|----------------------------------|
| POST   | /api/bookings        | Create a new booking             |
| GET    | /api/bookings        | Get bookings for logged-in user  |
| GET    | /api/bookings/:id    | Get specific booking             |
| PUT    | /api/bookings/:id/cancel | Cancel booking                 |

### 🔸 Input/Output Format
**Create Booking**
```json
Input:
{
  "property_id": 17,
  "start_date": "2025-07-15",
  "end_date": "2025-07-20"
}

Output:
{
  "message": "Booking created",
  "booking_id": 202,
  "status": "pending"
}
```

### 🔸 Validation Rules
- Dates must be valid, non-overlapping with other bookings.
- User must be authenticated and have the `guest` role.
- Booking must be within property availability range.

### 🔸 Performance Criteria
- Booking confirmation in < 500ms.
- Prevent race conditions using transaction locks or date validation.

---

📌 *Note:* All APIs should return proper HTTP status codes and error messages.

---

## ✅ Summary
Each feature above follows RESTful API design, uses JWT authentication, validates inputs thoroughly, and is optimized for performance in real-time use cases.
