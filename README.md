# RESTful API Design - Meeting Room Booking System

## Rooms API
### Endpoints
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

---

## Bookings API
### Endpoints
- **GET /api/bookings**
  - ดึงรายการการจองทั้งหมด
  - Roles: Staff

- **GET /api/bookings/me**
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

## Flow ตัวอย่าง
1. **User** → `POST /api/bookings` → ระบบตรวจสอบห้องว่าง → ถ้าว่าง → สร้าง booking (status = `pending`)
2. **Staff** → `PUT /api/bookings/:id/approve` → เปลี่ยนสถานะ booking เป็น `approved`
3. **User** → `DELETE /api/bookings/:id` → ยกเลิกการจอง (status = `cancelled`)

---

## โครงสร้างโค้ด (Express.js Example)
```js
// routes/rooms.route.js
router.get("/", roomController.getRooms);
router.get("/:id", roomController.getRoomById);
router.post("/", authStaff, roomController.createRoom);
router.put("/:id", authStaff, roomController.updateRoom);
router.delete("/:id", authStaff, roomController.deleteRoom);

// routes/bookings.route.js
router.get("/", authStaff, bookingController.getAllBookings);
router.get("/my", authUser, bookingController.getMyBookings);
router.post("/", authUser, bookingController.createBooking);
router.delete("/:id", authUser, bookingController.cancelBooking);
router.put("/:id/approve", authStaff, bookingController.approveBooking);
