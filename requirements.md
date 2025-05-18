# Airbnb Clone – Backend Requirement Specifications

This document outlines the functional and technical requirements for core backend features of the Airbnb Clone project.

---

## Feature: User Authentication

### Description
This module handles secure user registration, login, and session management. It supports both Guests and Hosts and uses JWT for session authentication.

### Roles Involved
- Guest
- Host
- Admin

### Functional Requirements
- [FR1] The system shall allow users to register using an email and password.
- [FR2] The system shall validate unique email addresses.
- [FR3] The system shall support login using credentials and return a JWT token.
- [FR4] The system shall protect private routes using JWT verification middleware.

### API Endpoints
| Method | Endpoint             | Description                  |
|--------|----------------------|------------------------------|
| POST   | `/api/auth/register` | Register a new user          |
| POST   | `/api/auth/login`    | Login and return token       |
| GET    | `/api/user/profile`  | Get current user info        |

### Input **(POST /register)**
```json
{
	"email": "user@example.com",
	"password": "securepass",
	"role": "guest"
}
```
### Output
```json
{
	"id": 10,
	"title": "Beachfront Apartment",
	"price": 120.0
}
```
### Validation Rules
- Email must be unique and valid
- Password must be 6+ characters
- Role must be one of: guest, host

### Performance/Non-Functional
- Response time < 500ms
- Passwords are hashed (e.g., bcrypt)
- Error messages for invalid credentials

---
## Feature: Property Listing Management

### Description
Hosts can create, update, and delete property listings, including details such as title, description, location, price, and amenities.

### Roles Involved
- Host

### Functional Requirements
- [FR1] Hosts shall be able to create property listings.
- [FR2] Hosts shall be able to update and delete their own listings.
- [FR3] Guests shall be able to view and search listings.

### API Endpoints
| Method | Endpoint             | Description                  |
|--------|----------------------|------------------------------|
| POST   | `/api/listings/`     | Create new listing           |
| GET    | `/api/listings/`     | Retrieve all listings        |
| GET    | `/api/listings/:id`  | Retrieve listing by ID       |
| PUT    | `/api/listings/:id`  | Update a listing      	   |
| DELETE | `/api/listings/:id`  | Delete a listing   	       |

### Input **(POST /api/listings/)**
```json
{
	"title": "Beachfront Apartment",
	"description": "Nice view with 2 bedrooms",
	"price": 120.0,
	"location": "Accra",
	"amenities": ["Wi-Fi", "Air Conditioning"]
}
```
### Output
```json
{
	"id": 10,
	"title": "Beachfront Apartment",
	"price": 120.0
}
```
### Validation Rules
- Title and location required
- Price must be numeric and > 0
- Only the owner (host) can update or delete their listing

### Performance/Non-Functional
- Support image uploads (future)
- Listing retrieval must support pagination and filtering

---
## Feature: Booking System

### Description
Enables Guests to book available properties, manage booking dates, and prevent overlapping reservations.

### Roles Involved
- Guest  
- Host

### Functional Requirements
- [FR1] Guests can book properties by selecting available dates.  
- [FR2] The system must validate date availability before confirming.  
- [FR3] Hosts can view and cancel bookings for their properties.  
- [FR4] Guests can cancel upcoming bookings.

### API Endpoints
| Method | Endpoint            | Description             |
|--------|---------------------|-------------------------|
| POST   | `/api/bookings/`    | Create new booking      |
| GET    | `/api/bookings/`    | Retrieve user bookings  |
| PUT    | `/api/bookings/:id` | Update/cancel booking   |

### Input (POST /api/bookings)
```json
{
	"property_id": 10,
	"check_in": "2025-06-01",
	"check_out": "2025-06-05"
}
```
### Output
```json
{
	"id": 55,
	"status": "confirmed",
	"check_in": "2025-06-01",
	"check_out": "2025-06-05"
}
```
### Validation Rules
- Dates must be in the future
- Booking must not overlap existing confirmed bookings
- Guests can cancel only their own bookings

### Performance/Non-Functional
- Fast DB query for availability check
- Support cancellation policy rules
- Send notifications on booking success/failure

---