# Airbnb Clone – Technical and Functional Requirements

This document outlines the detailed technical and functional requirements for the core backend features of the Airbnb Clone project.

---

## ✨ 1. User Authentication

### 🎯 Functional Requirements
- Allow users (guest/host/admin) to register and log in.
- Use JWT for authentication.
- Support OAuth (e.g., Google, Facebook) in future iterations.
- Provide profile update functionality.

### 🔌 API Endpoints

| Method | Endpoint              | Description                  | Auth Required |
|--------|-----------------------|------------------------------|---------------|
| POST   | /api/auth/register    | Register a new user          | No            |
| POST   | /api/auth/login       | Log in a user and return JWT | No            |
| GET    | /api/user/profile     | Fetch current user profile   | Yes           |
| PUT    | /api/user/profile     | Update user profile          | Yes           |

### 📥 Input Specifications
- **POST /api/auth/register**
  - `first_name`: string, required
  - `last_name`: string, required
  - `email`: string, required, unique
  - `password`: string, required (min 8 characters)
  - `role`: enum [guest, host], required

- **POST /api/auth/login**
  - `email`: string, required
  - `password`: string, required

### 📤 Output Specifications
- JSON response with `user_id`, `role`, and `access_token`.

### ✅ Validation Rules
- Email must be valid and unique.
- Password must be hashed (e.g., using bcrypt).
- JWT must be securely signed.

### ⚙️ Performance Criteria
- Token generation should complete within 100ms.
- Profile fetch should complete within 300ms.

---

## 🏠 2. Property Management

### 🎯 Functional Requirements
- Allow hosts to create, edit, and delete property listings.
- Users can view listings with filters.

### 🔌 API Endpoints

| Method | Endpoint              | Description                    | Auth Required |
|--------|-----------------------|--------------------------------|---------------|
| POST   | /api/properties       | Create a property              | Host          |
| GET    | /api/properties       | Get all listings with filters  | No            |
| GET    | /api/properties/:id   | Get a specific listing         | No            |
| PUT    | /api/properties/:id   | Edit a property                | Host          |
| DELETE | /api/properties/:id   | Delete a property              | Host          |

### 📥 Input Specifications
- `name`: string, required
- `description`: text, required
- `location`: string, required
- `price_per_night`: decimal, required
- `amenities`: array of strings, optional
- `availability`: date ranges, optional

### 📤 Output Specifications
- JSON response with property details and timestamps.

### ✅ Validation Rules
- Only authenticated hosts can create/edit/delete.
- Price must be a positive decimal.
- Location and description are mandatory.

### ⚙️ Performance Criteria
- Listing creation/edit within 500ms.
- Filtered search responses within 700ms with pagination.

---

## 📅 3. Booking System

### 🎯 Functional Requirements
- Guests can book properties.
- Prevent double bookings.
- Allow booking cancellations.
- Track booking status (pending, confirmed, canceled).

### 🔌 API Endpoints

| Method | Endpoint                | Description                    | Auth Required |
|--------|-------------------------|--------------------------------|---------------|
| POST   | /api/bookings           | Create a new booking           | Guest         |
| GET    | /api/bookings           | Get all user bookings          | Guest/Host    |
| GET    | /api/bookings/:id       | Get specific booking           | Guest/Host    |
| PUT    | /api/bookings/:id       | Update status (cancel, etc.)   | Guest/Host    |

### 📥 Input Specifications
- `property_id`: UUID, required
- `start_date`: date, required
- `end_date`: date, required
- `total_price`: decimal, calculated
- `status`: enum [pending, confirmed, canceled]

### 📤 Output Specifications
- JSON with booking ID, property, user, status, and timestamps.

### ✅ Validation Rules
- Validate date overlaps to prevent double bookings.
- Ensure end_date > start_date.
- Status transitions must be logical (e.g., pending → confirmed).

### ⚙️ Performance Criteria
- Booking creation within 400ms.
- Overlap validation must support high concurrency.

---

## 🧾 Notes
- All endpoints must return appropriate HTTP status codes.
- Secure APIs with middleware (JWT verification, role checks).
- Use pagination for all GET list endpoints.

---
