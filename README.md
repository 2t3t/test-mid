# RESTful API Design - Meeting Room Booking System

## 🔑 Auth API
- **POST /api/auth/login**
  - Login เข้าสู่ระบบ

- **POST /api/auth/register**
  - Register สมัครสมาชิกใหม่

## 👤 Users API
- **GET /api/users/profile**
  - ดูข้อมูลตัวเอง (ต้อง login ก่อน)

## 🏢 Rooms API
- **GET /api/rooms**
  - ดึงรายการห้องทั้งหมด (รองรับ filter เช่น capacity, location)
  - Roles: User, Staff

- **GET /api/rooms/:id**
  - ดึงข้อมูลห้องประชุมตาม room_id
  - Roles: User, Staff

- **POST /api/rooms**
  - สร้างห้องประชุมใหม่
  - Roles: Staff

- **PUT /api/rooms/:id**
  - แก้ไขข้อมูลห้องประชุม
  - Roles: Staff

- **DELETE /api/rooms/:id**
  - ลบห้องประชุม
  - Roles: Staff

## 📅 Bookings API
- **GET /api/bookings**
  - ดึงรายการการจองทั้งหมด
  - Roles: Staff

- **GET /api/bookings/my**
  - ดึงการจองของผู้ใช้ที่ล็อกอินอยู่
  - Roles: User

- **POST /api/bookings**
  - สร้างการจองใหม่ (ตรวจสอบห้องว่าง, เวลาไม่ซ้อนกัน)
  - Roles: User

- **DELETE /api/bookings/:id**
  - ยกเลิกการจองของตัวเอง
  - Roles: User

- **PUT /api/bookings/:id/approve**
  - อนุมัติการจอง
  - Roles: Staff

---

## 🔗 ตัวอย่าง Flow
1. User → `POST /api/auth/login` → เข้าสู่ระบบ  
2. User → `POST /api/bookings` → ระบบตรวจสอบห้องว่าง → ถ้าว่าง → สร้าง booking (status = pending)  
3. Staff → `PUT /api/bookings/:id/approve` → เปลี่ยนสถานะ booking เป็น approved  
4. User → `DELETE /api/bookings/:id` → ยกเลิกการจอง (status = cancelled)  
5. User → `GET /api/users/profile` → ดูข้อมูลตัวเอง  

---

## 🧾 ตัวอย่าง Request/Response

### POST /api/bookings
**Request Body**
```json
{
  "room_id": 101,
  "booking_date": "2026-01-15",
  "start_time": "09:00",
  "end_time": "12:00",
  "purpose": "ประชุมโครงการ",
  "attendees": 8
}


{
  "id": 1234,
  "room_id": 101,
  "room_name": "ห้อง A301",
  "user_id": 567,
  "booking_date": "2026-01-15",
  "start_time": "09:00",
  "end_time": "12:00",
  "purpose": "ประชุมโครงการ",
  "status": "pending",
  "created_at": "2026-01-08T10:30:00Z"
}

