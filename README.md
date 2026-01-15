# 🔑 Auth API
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "123456"
}

Response 200 OK
{
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6..."
}

---

POST /api/auth/register
Content-Type: application/json

{
  "name": "Thanakrit",
  "email": "user@example.com",
  "password": "123456"
}

Response 201 Created
{
  "message": "Register successful",
  "user": {
    "user_id": 1,
    "name": "Thanakrit",
    "email": "user@example.com",
    "role": "user"
  }
}

---

# 👤 Users API
GET /api/users/profile
Authorization: Bearer <token>

Response 200 OK
{
  "user_id": 1,
  "name": "Thanakrit",
  "email": "user@example.com",
  "role": "user"
}

---

# 🏢 Rooms API
GET /api/rooms

Response 200 OK
[
  {
    "room_id": 1,
    "name": "Conference Room A",
    "capacity": 20,
    "location": "Building 1",
    "status": "available"
  },
  {
    "room_id": 2,
    "name": "Meeting Room B",
    "capacity": 10,
    "location": "Building 2",
    "status": "available"
  }
]

---

POST /api/rooms
Authorization: Bearer <staff-token>
Content-Type: application/json

{
  "name": "VIP Room",
  "capacity": 5,
  "location": "Building 3",
  "status": "available"
}

Response 201 Created
{
  "message": "Room created successfully",
  "room": {
    "room_id": 3,
    "name": "VIP Room",
    "capacity": 5,
    "location": "Building 3",
    "status": "available"
  }
}

---

# 📅 Bookings API
POST /api/bookings
Authorization: Bearer <user-token>
Content-Type: application/json

{
  "room_id": 1,
  "start_time": "2026-01-15T13:00:00",
  "end_time": "2026-01-15T15:00:00"
}

Response 201 Created
{
  "message": "Booking created successfully",
  "booking": {
    "booking_id": 101,
    "room_id": 1,
    "user_id": 1,
    "start_time": "2026-01-15T13:00:00",
    "end_time": "2026-01-15T15:00:00",
    "status": "pending"
  }
}

---

GET /api/bookings/my
Authorization: Bearer <user-token>

Response 200 OK
[
  {
    "booking_id": 101,
    "room_id": 1,
    "start_time": "2026-01-15T13:00:00",
    "end_time": "2026-01-15T15:00:00",
    "status": "pending"
  }
]

---

PUT /api/bookings/101/approve
Authorization: Bearer <staff-token>

Response 200 OK
{
  "message": "Booking approved successfully",
  "booking": {
    "booking_id": 101,
    "status": "approved"
  }
}
