# AI Personal Digital Assistant

# API Documentation

## 1. Introduction

This document defines the backend API structure for the AI Personal Digital Assistant system.

The API connects:

- Web Application
- Mobile Application
- AI Service
- Database
- Storage System

Technology:

```
Node.js + Express + TypeScript
```

API Style:

```
REST API
```

Communication:

```
HTTPS + JSON
```

---

# 2. Base URL

Development:

```
http://localhost:5000/api
```

Production:

```
https://api.aiassistant.com/api
```

---

# 3. Authentication

## Authentication Method

The system uses:

```
JWT Authentication
```

Flow:

```
Login

↓

Receive Access Token

↓

Send Token With Requests

↓

Access Protected APIs
```

Header:

```
Authorization: Bearer TOKEN
```

---

# 4. Authentication APIs


# 4.1 Register User

## Endpoint

```
POST /auth/register
```

Purpose:

Create a new account.


Request:

```json
{
  "fullName": "Sisay Temesgen",
  "email": "user@gmail.com",
  "password": "password123"
}
```


Response:

```json
{
  "message": "Account created successfully",
  "userId": "uuid"
}
```

---

# 4.2 Login User

## Endpoint

```
POST /auth/login
```


Request:

```json
{
  "email": "user@gmail.com",
  "password": "password123"
}
```


Response:

```json
{
  "accessToken": "jwt_token",
  "user": {
    "id": "uuid",
    "name": "User"
  }
}
```

---

# 4.3 Logout

## Endpoint

```
POST /auth/logout
```

Purpose:

Invalidate current session.

---

# 4.4 Password Reset

## Endpoint

```
POST /auth/reset-password
```

---

# 5. User APIs


# 5.1 Get Profile

```
GET /users/profile
```

Response:

```json
{
 "name":"User",
 "email":"user@gmail.com",
 "language":"English"
}
```

---

# 5.2 Update Profile

```
PUT /users/profile
```

---

# 6. Device APIs

Used by Mobile Application.


# 6.1 Register Device


```
POST /devices/register
```


Request:

```json
{
 "deviceName":"HP Laptop",
 "os":"Windows 11"
}
```


Response:

```json
{
 "deviceId":"uuid"
}
```

---

# 6.2 Device Status


```
GET /devices
```


Response:

```json
[
 {
 "name":"Laptop",
 "lastConnected":"2026-08-01"
 }
]
```

---

# 7. File Management APIs


# 7.1 Upload File


```
POST /files/upload
```


Content Type:

```
multipart/form-data
```


Data:

```
file
folderId
deviceId
```


Response:

```json
{
 "message":"File uploaded",
 "fileId":"uuid"
}
```

---

# 7.2 Get Files


```
GET /files
```


Query:

```
folderId
category
search
```


Response:

```json
[
 {
  "name":"degree.pdf",
  "type":"PDF",
  "size":"2MB"
 }
]
```

---

# 7.3 Download File


```
GET /files/:id/download
```


Purpose:

Download user file.


---

# 7.4 Delete File


```
DELETE /files/:id
```


---

# 7.5 Rename File


```
PUT /files/:id/rename
```


Request:

```json
{
"name":"new-name.pdf"
}
```

---

# 8. Folder APIs


# Create Folder


```
POST /folders
```


Request:

```json
{
"name":"Certificates"
}
```


---

# Get Folder Structure


```
GET /folders
```


Response:

```json
{
"name":"Documents",
"children":[]
}
```

---

# 9. Mobile Synchronization APIs


Used by Mobile Application.


# 9.1 Scan File Information


```
POST /sync/scan
```


Request:

```json
{
"deviceId":"uuid",
"files":[
 {
  "name":"CV.pdf",
  "size":20000
 }
]
}
```


---

# 9.2 Upload Changed Files


```
POST /sync/upload
```


Purpose:

Upload only changed files.


---

# 9.3 Sync Status


```
GET /sync/status
```


Response:

```json
{
"synced":100,
"pending":5
}
```

---

# 10. AI Document APIs


# 10.1 Analyze Document


```
POST /ai/analyze/:fileId
```


Process:

```
File

↓

OCR

↓

AI Analysis

↓

Save Result
```


Response:

```json
{
"type":"Passport",
"expiryDate":"2030-05-20"
}
```

---

# 10.2 Get Document Information


```
GET /ai/document/:fileId
```


Response:

```json
{
"category":"Identity",
"summary":"Passport document"
}
```

---

# 10.3 Generate Summary


```
POST /ai/summary/:fileId
```

---

# 11. AI Chat APIs


# 11.1 Create Chat


```
POST /ai/chat
```


Request:

```json
{
"title":"My Documents"
}
```

---

# 11.2 Ask Question


```
POST /ai/chat/message
```


Request:

```json
{
"chatId":"uuid",
"message":"What is my passport expiry date?"
}
```


Response:

```json
{
"answer":
"Your passport expires in 2030."
}
```

---

# 12. AI Search APIs


# Semantic Search


```
GET /search
```


Query:

```
?q=university certificate
```


Response:

```json
[
{
"name":"Degree.pdf",
"confidence":95
}
]
```

---

# 13. Notification APIs


# Get Notifications


```
GET /notifications
```


---

# Mark Notification Read


```
PUT /notifications/:id/read
```


---

# 14. Expiry Management APIs


# Get Expiring Documents


```
GET /expiry
```


Response:

```json
[
{
"file":"passport.pdf",
"expires":"2030-05-20"
}
]
```

---

# 15. Sharing APIs


# Create Share Link


```
POST /share
```


Request:

```json
{
"fileId":"uuid",
"expireHours":24
}
```


Response:

```json
{
"url":"secure-link"
}
```

---

# Access Shared File


```
GET /share/:token
```

---

# 16. Security APIs


# Security Events


```
GET /security/events
```


Response:

```json
[
{
"type":"LOGIN",
"time":"2026-08-01"
}
]
```

---

# 17. Admin APIs


# Get Users


```
GET /admin/users
```


---

# System Statistics


```
GET /admin/statistics
```


Example:

```json
{
"users":100,
"files":50000
}
```

---

# 18. Error Response Format


All errors use:


```json
{
"success":false,
"message":"Error description"
}
```


Examples:

```
401 Unauthorized

403 Forbidden

404 Not Found

500 Server Error

```

---

# 19. API Security Rules


The API must use:


- JWT authentication
- Rate limiting
- Input validation
- File type checking
- Permission verification
- HTTPS communication


---

# 20. Future API Extensions


Future endpoints:


```
/voice

/mobile

/blockchain

/face-auth

/enterprise

```

---

# Conclusion

This API structure provides communication between all parts of the AI Personal Digital Assistant system.

It supports:

- File management
- Desktop synchronization
- AI processing
- Document chat
- Smart search
- Notifications
- Security management
