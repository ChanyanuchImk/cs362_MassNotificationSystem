# cs362_MassNotificationSystem
## Step 1 : Data Design
### Analysis Class Diagram ครอบคลุม Entities ทั้งหมด

| Class | Description | Attributes | Operations |
| :--- | :----: | :----: | :----: |
| User | คลาสหลักสำหรับผู้ใช้ | UserID (PK), Username, Password | getContactInfo()
| EmergencyMessage | ข้อความแจ้งเตือนภัย | messageId (PK), content, disasterType, CreatedTime, severity_level | -
| Admin | ผู้ดูแลระบบ | UserID (FK), Username, Password | CreateEmergencyMessage(), SendEmergencyNotification()
| SystemOperator | ผู้ปฏิบัติงานระบบ | UserID (FK), Username, Password | ViewMonitoringDashboard(), RetryFailedDelivery()
| Notification | รายการแจ้งเตือนแต่ละครั้งที่ถูกส่ง | notificationId (PK), status, sentTime, messageId (FK) | send(), trackDeliveryStatus()
| CellBroadcastGateway | ช่องทางการส่งผ่าน Cell Broadcast | - | sendNotification(message)
| SMSGateway | ช่องทางการส่งผ่าน SMS | - | sendNotification(message)
| LINENotificationService | ช่องทางการส่งผ่าน LINE | - | sendNotification(message)
| NotificationService (interface) | อินเทอร์เฟซมาตรฐานสำหรับทุกช่องทางเพื่อให้การส่งเป็นรูปแบบเดียวกัน | - | sendNotification(message)
| NotificationLog | บันทึกประวัติการแจ้งเตือนและการส่ง | logId (PK), notificationId (FK), status, timestamp | storeLog()
| DeliveryManager | จัดการการจัดส่งข้อความผ่านช่องทางต่างๆ และแก้ไขปัญหา | - | distributeMessageViaMultipleChannels(), retryFailedDelivery()

## Step 2 : Architectural Mapping

```
Controller Layer (handlers)/
├── admin_handler.go
├── message_handler.go
├── monitoring_handler.go
├── notification_handler.go
Service Layer
├── Channel_Service/
|   ├── Cellbroadcast_service.go
|   ├── LINEmessage_service.go
|   ├── SMS_service.go
├── deliverymanager_service.go
├── emergencymessage_service.go
├── notification_service.go
Repository Layer
├── emergencymessage_repository.go
├── notification_repository.go
├── notificationlog_repository.go
├── user_repository.go
```
### Git Action: โครงสร้างโฟลเดอร์นี้สร้างบน Git Repository จริงของกลุ่ม

## Step 3 : Interface & Controller Contract
### Task 1: Controller (API Specification)
### Create Message
- Endpoint : POST /api/messages
- Description : สร้างข้อความ

#### Request Body (JSON)
```json
{
  "messageId": "msg001",
  "content": "ตรวจพบแรงสั่นสะเทือนขนาด 8.5 ให้อพยพขึ้นที่สูงด่วน",
  "disasterType": "สึนามิ",
  "severity_level": "High",
  "createdTime": "2026-03-21T14:30:00Z"
}
```

### Notification
- Endpoint : POST /api/notifications
- Description : ใช้สำหรับสั่ง "ส่ง" และ "ติดตามสถานะ"

#### Request Body (JSON)
```json
{
  "notificationId": "noti0001",
  "status": "sended",
  "sentTime": "2025-04-11T14:30:00Z"
}
```

### Notification Log
- Endpoint : GET /api/logs
- Description : ใช้สำหรับตรวจสอบข้อมูลการส่ง

#### Request Body (JSON)
```json
{
  "logId": "log001",
  "status": "saved",
  "timestamp": "2025-12-11T14:30:00Z"
}
```
### Edit message
- Endpoint : GET/PUT/DELETE /api/messages/{id}
- Description : แก้ไขหรือลบข้อความที่ร่างไว้ก่อนจะส่งจริง

#### Request Body (JSON)
```json
{
  "messageId": "msg001",
  "content": "ตรวจพบแรงสั่นสะเทือนขนาด 8.5 ให้อพยพขึ้นที่สูงด่วน",
  "disasterType": "สึนามิ",
  "severity_level": "High",
  "createdTime": "2026-03-21T14:30:00Z"
}
```

### 
