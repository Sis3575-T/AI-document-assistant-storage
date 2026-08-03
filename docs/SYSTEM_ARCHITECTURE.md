# AI Personal Digital Assistant

# System Architecture Document

## 1. Introduction

This document describes the complete technical architecture of the AI Personal Digital Assistant system.

The system is designed as a distributed platform consisting of:

- Web Application
- Mobile Application
- Backend API
- AI Processing Service
- Database System
- Storage System

The architecture allows users to securely manage personal data while using artificial intelligence for understanding and automation.

---

# 2. High-Level Architecture

The system follows a multi-service architecture.

```
                          USER

                            |
           --------------------------------
           |                              |
           ↓                              ↓

   Web Application                 Mobile Application

   React + TypeScript              React Native + Expo

           |                              |
           |                              |
           --------------------------------

                          |

                          ↓

                   Backend API

                Node.js + Express

                          |

         ---------------------------------

         |                               |

         ↓                               ↓


  PostgreSQL Database              Storage Service


         |

         ↓


     AI Service

 Python + FastAPI

         |

 -----------------------------

 |             |             |

OCR        Gemini AI     Embeddings

```

---

# 3. System Components


# 3.1 Frontend Application

## Purpose

The frontend provides the user interface for interacting with the platform.

## Technology

- React
- TypeScript
- Tailwind CSS
- React Query
- Zustand/Redux


## Responsibilities

The frontend handles:

- User registration
- Login
- Dashboard
- File explorer
- Document search
- AI chat interface
- Notification display
- Settings management


## Main Pages

```
frontend/

src/

pages/

├── Login.tsx

├── Register.tsx

├── Dashboard.tsx

├── Files.tsx

├── AIChat.tsx

├── Notifications.tsx

└── Settings.tsx

```

---

# 3.2 Mobile Application

## Purpose

The mobile application provides on-the-go access to the AI Vault system, including document scanning via camera, offline access, push notifications, and biometric authentication.

## Technology

- React Native
- Expo
- TypeScript
- Expo Router
- Zustand
- React Query
- expo-camera
- expo-image-picker
- expo-notifications
- expo-secure-store
- expo-local-authentication


## Responsibilities

The mobile app handles:

- User authentication (including biometric)
- Document scanning via camera/OCR
- File upload and management
- Offline access to cached documents
- Push notifications
- AI chat interface
- Settings management


## App Structure

```
apps/mobile/

├── app/                  # Expo Router screens (file-based routing)
│   ├── (auth)/           # Auth screens (login, register)
│   ├── (tabs)/           # Main tab screens
│   ├── files/            # File management screens
│   ├── chat/             # AI chat screens
│   └── _layout.tsx       # Root layout
├── components/           # Reusable UI components
│   ├── ui/               # Primitives (Button, Input, Card)
│   ├── layout/           # Header, TabBar, Drawer
│   └── features/         # Feature-specific components
├── stores/               # Zustand state stores
├── services/             # API clients and native module wrappers
├── hooks/                # Custom React hooks
├── types/                # TypeScript type definitions
├── utils/                # Helper functions
├── constants/            # App constants, theme, config
├── assets/               # Images, fonts, icons
├── app.json              # Expo config
├── eas.json              # EAS Build config
├── tailwind.config.js    # NativeWind config
├── tsconfig.json
└── package.json

```

---

# 3.3 Backend API


## Purpose

The backend is the central controller of the application.

## Technology

- Node.js
- Express.js
- TypeScript
- Prisma ORM


## Responsibilities

The backend manages:

- Authentication
- Users
- Files
- Permissions
- Devices
- Notifications
- AI requests
- Security


## Backend Structure


```
backend/

src/

├── controllers/

├── routes/

├── middleware/

├── services/

├── database/

├── utils/

└── server.ts

```

---

# 4. Mobile Application Workflow


## Step 1: User Authentication


User opens:

```
AI Vault Mobile App
```

Login options:

```
Email/Password
Biometric (Face ID / Fingerprint)
```

The application receives:

```
JWT Token
Refresh Token
```

---

## Step 2: Device Registration


After login:

```
Mobile App
       |
       ↓
Backend
       |
       ↓
Create Device Record
```

Stored information:

```
Device Name
Operating System (iOS/Android)
Last Active Time
Device ID
Push Token
```

---

# 5. Document Scanning & Upload


## Camera Scanning

```
User opens camera
       |
Scan document
       |
Auto-crop & enhance
       |
OCR Processing (on-device or cloud)
       |
Preview & confirm
       |
Upload to backend
```

## Gallery Import

```
Select from gallery
       |
Image processing
       |
OCR extraction
       |
Upload to backend
```

---

# 6. Backend Architecture


## Request Flow


```
User Action
      |
Mobile App / Web App
      |
API Request
      |
Express Router
      |
Controller
      |
Service Layer
      |
Database
```

---

# 7. Database Architecture


## Database Technology

PostgreSQL


## ORM

Prisma


## Main Entities


```
Users
   |
   |
Devices
   |
   |
Files
   |
   |
Documents
   |
   |
AI Analysis
   |
   |
Notifications
```

---

# 8. AI Service Architecture


## Purpose

The AI service processes documents and provides intelligent features.


## Technology

- Python
- FastAPI
- LangChain
- Gemini API
- Tesseract OCR


Architecture:


```
                  Document

                     |

                     ↓

               AI Processing API

                     |

        -----------------------------

        |             |             |

       OCR        AI Model     Embedding

        |             |             |

        -----------------------------

                     |

                     ↓

               Stored Knowledge

```

---

# 9. Document Processing Pipeline


## Upload Process


```
User Uploads File

         |

         ↓

File Storage

         |

         ↓

OCR Extraction

         |

         ↓

Text Processing

         |

         ↓

AI Analysis

         |

         ↓

Database Storage
```

---

# 10. AI Chat Architecture


User:

```
"What is my passport expiry date?"
```


Process:


```
Question

   |
AI Chat Service

   |
Search Document Knowledge

   |
Find Relevant Data

   |
Gemini AI

   |
Answer
```

---

# 11. Storage Architecture


## Development Storage


Use:

- Local Storage
- MinIO


## Production Storage


Options:

- AWS S3
- Google Cloud Storage
- Azure Blob Storage


Storage flow:


```
File

 |
Encryption

 |
Storage

 |
Metadata saved in PostgreSQL
```

---

# 12. Security Architecture


## Authentication


```
User Login

     |
Password Hash

     |
JWT Token

     |
Access Granted
```


---

## File Security


Files are protected using:


- Encryption
- Access permissions
- Secure URLs
- Audit logging


---

# 13. Notification Architecture


Notification types:

- Expiry warning
- Security alert
- Backup completed
- File shared


Flow:


```
System Event

      |
Notification Service

      |
-------------------

Email

Push

In-App
```

---

# 14. Communication Between Services


## Frontend → Backend

Protocol:

```
REST API
HTTPS
```


---

## Mobile App → Backend

Protocol:

```
REST API
HTTPS
WebSocket for real-time updates
```


---

## Backend → AI Service

Protocol:

```
HTTP API

JSON
```

---

# 15. Deployment Architecture


Production:


```
                  Users

                    |

                    ↓

              Frontend Server (Web)

                    |
         -------------------------
         |                       |
         ↓                       ↓
Backend Server            Mobile App (App Store/Play Store)
         |
         ---------------------
         |                   |
     PostgreSQL          AI Server
                             |
                          Gemini API

                    |
              Cloud Storage
```

---

# 16. Scalability Plan


Future improvements:


## Microservices

Separate:

- Authentication service
- File service
- AI service
- Notification service


## Queue System

Use:

- Redis
- BullMQ


For:

- Large file processing
- AI tasks
- Notifications


---

# 17. Final Architecture Summary


The system contains:


| Component | Technology |
|-|-|
| Web Application | React + TypeScript |
| Mobile Application | React Native + Expo |
| Backend | Node.js + Express |
| Database | PostgreSQL |
| ORM | Prisma |
| AI Service | Python FastAPI |
| OCR | Tesseract |
| AI Model | Gemini API |
| Storage | Cloud/Object Storage |
| Authentication | JWT |


---

# Conclusion

This architecture allows the AI Personal Digital Assistant to work as a complete personal data management platform.

The combination of React Native, AI, secure storage, and intelligent document processing makes the system different from traditional cloud storage applications.