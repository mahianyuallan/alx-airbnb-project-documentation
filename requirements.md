# Airbnb Clone Backend - Requirement Specifications

## 1. User Authentication

**Description:**  
Provides secure registration and login for Guests and Hosts, including role-based access control.

**API Endpoints:**  
- `POST /api/auth/register` – Register a new user  
- `POST /api/auth/login` – Login and obtain JWT token  
- `GET /api/auth/profile` – Retrieve logged-in user profile  
- `PUT /api/auth/profile` – Update user profile  

**Input/Output Specifications:**  
- **Register Input:** `name`, `email`, `password`, `role` (guest/host)  
- **Register Output:** `id`, `name`, `email`, `role`, `token`  
- **Login Input:** `email`, `password`  
- **Login Output:** `token`, `user` object  
- **Profile Output:** user details (including profile photo and preferences)

**Validation Rules:**  
- Email must be unique and valid format  
- Password must be at least 8 characters  
- Role must be either `guest` or `host`

**Performance Criteria:**  
- Response time < 200ms for login/register requests  
- Handle 1000 concurrent authentication requests

---

## 2. Property Management

**Description:**  
Enables Hosts to create, update, and delete property listings, including images and availability.

**API Endpoints:**  
- `POST /api/properties` – Add new property  
- `GET /api/properties` – List/search properties  
- `GET /api/properties/:id` – View property details  
- `PUT /api/properties/:id` – Update property  
- `DELETE /api/properties/:id` – Delete property  

**Input/Output Specifications:**  
- **Add Property Input:** `title`, `description`, `location`, `price`, `amenities`, `availability`, `images`  
- **Add Property Output:** Property object with `id`  
- **Search Properties Input:** Filters: `location`, `price_range`, `guests`, `amenities`  
- **Search Output:** List of matching properties  

**Validation Rules:**  
- Title and description required  
- Price must be positive decimal  
- Availability must be valid date ranges  
- Images must be valid URLs or uploaded files

**Performance Criteria:**  
- Search response < 500ms for 10,000 properties  
- Uploading property with up to 10 images should complete < 2 seconds

---

## 3. Booking System

**Description:**  
Allows Guests to book properties, manage booking status, and handles cancellations.

**API Endpoints:**  
- `POST /api/bookings` – Create booking  
- `GET /api/bookings` – List user bookings  
- `GET /api/bookings/:id` – Booking details  
- `PUT /api/bookings/:id/cancel` – Cancel booking  

**Input/Output Specifications:**  
- **Create Booking Input:** `property_id`, `guest_id`, `start_date`, `end_date`  
- **Create Booking Output:** Booking object with `id`, `status`, `total_price`  
- **Booking List Output:** List of bookings with statuses  

**Validation Rules:**  
- Prevent double bookings (date overlap validation)  
- Start date must be before end date  
- Booking status: `pending`, `confirmed`, `canceled`, `completed`  

**Performance Criteria:**  
- Booking creation < 300ms  
- System must support 500 concurrent booking requests per property  
- Ensure atomic updates to prevent double booking conflicts
