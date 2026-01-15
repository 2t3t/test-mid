# RESTful API Design - Meeting Room Booking System

## 🔑 Auth API
- **POST /api/auth/login**
  - Login เข้าสู่ระบบ
- **POST /api/auth/register**
  - Register สมัครสมาชิกใหม่

---

## 👤 Users API
- **GET /api/users/profile**
  - ดูข้อมูลตัวเอง (ต้อง login ก่อน)

---

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

---

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

## 🔗 Flow ตัวอย่าง
1. **User** → `POST /api/auth/login` → เข้าสู่ระบบ  
2. **User** → `POST /api/bookings` → ระบบตรวจสอบห้องว่าง → ถ้าว่าง → สร้าง booking (status = `pending`)  
3. **Staff** → `PUT /api/bookings/:id/approve` → เปลี่ยนสถานะ booking เป็น `approved`  
4. **User** → `DELETE /api/bookings/:id` → ยกเลิกการจอง (status = `cancelled`)  
5. **User** → `GET /api/users/profile` → ดูข้อมูลตัวเอง  

---

## 🛠️ ตัวอย่างโค้ด Router (Express.js)

```js
// routes/auth.route.js
router.post("/login", authController.login);
router.post("/register", authController.register);

// routes/users.route.js
router.get("/profile", authUser, userController.getProfile);

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
