# cs362_MassNotificationSystem
## Step 1 : Data Design
### Analysis Class Diagram ครอบคลุม Entities ทั้งหมด

| Class | Description | Attributes | Operations |
| :--- | :----: | :----: | :----: |
| User | คลาสหลักสำหรับผู้ใช้ | UserID, Username | getContactInfo()
| EmergencyMessage | ข้อความแจ้งเตือนภัย | messageId, content, disasterType, CreatedTime | -
| Admin | ผู้ดูแลระบบ | - | CreateEmergencyMessage(), SendEmergencyNotification()
| SystemOperator | ผู้ปฏิบัติงานระบบ | - | ViewMonitoringDashboard(), RetryFailedDelivery()
| Notification | รายการแจ้งเตือนแต่ละครั้งที่ถูกส่ง | notificationId, status, sentTime | send(), trackDeliveryStatus()
| CellBroadcastGateway | ช่องทางการส่งผ่าน Cell Broadcast | - | sendNotification(message)
| SMSGateway | ช่องทางการส่งผ่าน SMS | - | sendNotification(message)
| LINENotificationService | ช่องทางการส่งผ่าน LINE | - | sendNotification(message)
| NotificationService (interface) | อินเทอร์เฟซมาตรฐานสำหรับทุกช่องทางเพื่อให้การส่งเป็นรูปแบบเดียวกัน | - | sendNotification(message)
| NotificationLog | บันทึกประวัติการแจ้งเตือนและการส่ง | logId, status, timestamp | storeLog()
| DeliveryManager | จัดการการจัดส่งข้อความผ่านช่องทางต่างๆ และแก้ไขปัญหา | - | distributeMessageViaMultipleChannels(), retryFailedDelivery()

## Step 2 : Architectural Mapping
### Git Action: โครงสร้างโฟลเดอร์นี้สร้างบน Git Repository จริงของกลุ่ม

## Step 3 : Interface & Controller Contract
### Task 1: Controller (API Specification)
#### Create & Send Notification
- Endpoint : POST /api/messages
- Description : สร้างข้อความและเริ่มกระบวนการส่ง

#### Request Body (JSON)
```json
{
  "messageId": "msg001",
  "content": "ตรวจพบแรงสั่นสะเทือนขนาด 8.5 ให้อพยพขึ้นที่สูงด่วน",
  "disasterType": "สึนามิ",
  "createdTime": "2026-03-21T14:30:00Z"
}
```

