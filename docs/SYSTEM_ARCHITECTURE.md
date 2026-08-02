# AI Personal Digital Assistant

# System Architecture Document

## 1. Introduction

This document describes the complete technical architecture of the AI Personal Digital Assistant system.

The system is designed as a distributed platform consisting of:

- Web Application
- Desktop Agent
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

   Web Application                 Desktop Agent

   React + TypeScript              Electron

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

# 3.2 Backend API


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

# 3.3 Desktop Agent


## Purpose

The desktop application connects the user's computer with the AI Vault system.

## Technology

- Electron
- React
- Node.js


## Why Electron?

A browser cannot directly access personal computer files.

Electron allows the application to:

- Read selected folders
- Monitor changes
- Synchronize files
- Work offline


## Desktop Structure


```
desktop-agent/


src/

├── main/

│    ├── main.ts

│    ├── scanner.ts

│    ├── sync.ts

│    └── security.ts


├── renderer/

│    └── React UI


└── storage/

```

---

# 4. Desktop Agent Workflow


## Step 1: User Permission


```
User opens AI Vault Agent

        |

Select folders

        |

Allow access

```


---

## Step 2: File Scanning


```
Documents Folder

      |

Scanner Service

      |

Collect:

- Name
- Size
- Type
- Location
- Modified Date

```

---

## Step 3: Synchronization


```
Detected File

       |

Check Changes

       |

Encrypt File

       |

Upload

       |

Backend

```

---

# 5. Backend Architecture


## Request Flow


```
User Action

      |

Frontend

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

# 6. Database Architecture


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

# 7. AI Service Architecture


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

# 8. Document Processing Pipeline


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

# 9. AI Chat Architecture


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

# 10. Storage Architecture


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

# 11. Security Architecture


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

# 12. Notification Architecture


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

# 13. Communication Between Services


## Frontend → Backend

Protocol:

```
REST API
HTTPS
```


---

## Backend → AI Service


Protocol:

```
HTTP API

JSON
```


---

## Desktop → Backend


Protocol:

```
Secure API

WebSocket for sync

```

---

# 14. Deployment Architecture


Production:


```
                 Users

                   |

                   ↓

              Frontend Server

                   |

                   ↓

              Backend Server

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

# 15. Scalability Plan


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

# 16. Final Architecture Summary


The system contains:


| Component | Technology |
|-|-|
| Web Application | React + TypeScript |
| Desktop Agent | Electron |
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

The combination of Electron, AI, secure storage, and intelligent document processing makes the system different from traditional cloud storage applications.