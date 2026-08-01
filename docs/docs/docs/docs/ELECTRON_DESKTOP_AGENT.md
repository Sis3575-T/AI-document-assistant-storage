# AI Personal Digital Assistant

# Electron Desktop Agent Design Document

## 1. Introduction

The Electron Desktop Agent is a desktop application that connects the user's personal computer with the AI Personal Digital Assistant platform.

It provides secure communication between:

- User device files
- Backend API
- Cloud storage
- AI processing system

The purpose of the desktop agent is to allow users to protect, organize, and synchronize their personal files while maintaining user permission and privacy.

---

# 2. Why Electron is Required

A normal web browser cannot directly access a user's computer files.

Example:

```
Chrome Browser

     |

     X

User Documents Folder
```

The browser blocks this for security reasons.

Electron provides a trusted desktop application:

```
User Computer

Documents
Pictures
Projects

       |

       ↓

AI Vault Desktop Agent

       |

       ↓

Backend Server

       |

       ↓

Cloud Storage
```

---

# 3. Technology Stack

## Desktop Framework

```
Electron
```

## Frontend Interface

```
React
TypeScript
Tailwind CSS
```

## Desktop Logic

```
Node.js
Electron Main Process
```

## Local Database

```
SQLite
```

## Communication

```
REST API
WebSocket
```

---

# 4. Desktop Agent Responsibilities

The application is responsible for:

- User authentication
- Device registration
- Folder permission management
- File scanning
- File monitoring
- Synchronization
- Local caching
- Offline support
- Encryption
- Background operation

---

# 5. Application Architecture


```
                 Electron Application


        ---------------------------------

        |                               |

        ↓                               ↓


 Renderer Process              Main Process

 React UI                      Node.js


        |                               |

        ---------------------------------

                       |

                       ↓

              Local Storage

              SQLite Database

                       |

                       ↓

                 Backend API

```

---

# 6. Project Structure


```
desktop-agent/

│

├── src/

│

├── main/

│   ├── main.ts

│   ├── fileScanner.ts

│   ├── syncManager.ts

│   ├── encryption.ts

│   └── database.ts

│

├── renderer/

│   ├── App.tsx

│   ├── Login.tsx

│   ├── Dashboard.tsx

│   └── Settings.tsx

│

├── services/

│   ├── api.ts

│   ├── upload.ts

│   └── notification.ts

│

└── package.json

```

---

# 7. Application Workflow


## Step 1: User Login


User opens:

```
AI Vault Agent.exe
```

Login:

```
Email
Password
```

The application receives:

```
JWT Token
```

---

## Step 2: Device Registration


After login:


```
Desktop Agent

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

Operating System

Last Active Time

Device ID

```

---

# 8. Folder Permission System


## Purpose

The user decides what files the application can access.


Example:


```
Select Protected Folders:


☑ Documents

☑ Pictures

☐ Videos

☐ Downloads


[Start Protection]

```


The system stores:


```
Folder Path

Permission Status

User ID

Created Date

```

---

# 9. File Scanner


## Purpose

Find files inside selected folders.


Example:


Input:

```
Documents/
```

Scanner returns:


```json
[
 {
  "name":"degree.pdf",
  "type":"pdf",
  "size":"2MB"
 }
]
```


Collected information:


- File name
- File type
- File size
- File location
- Modified date
- File hash

---

# 10. File Monitoring System


The agent continuously watches files.


Example:


Before:

```
CV.pdf

Version 1

```

User edits file:


```
CV.pdf

Version 2

```


Agent detects:


```
File changed

Start synchronization

```

---

# 11. Synchronization System


## Upload Process


```
File Detected

       |

Calculate Hash

       |

Check Server Version

       |

Encrypt File

       |

Upload

       |

Update Database

```


---

# 12. Smart Synchronization


The system does not upload everything again.


Example:


First upload:


```
CV.pdf
Degree.pdf
Contract.pdf

```


After modification:


```
Only CV.pdf uploaded

```

Benefits:

- Saves storage
- Saves internet
- Faster synchronization

---

# 13. Smart Backup Selection


AI determines importance.


Example:


Files:


```
Movie.mp4

Degree.pdf

CV.pdf

Game.exe

```


AI:


```
Important:

✓ Degree.pdf

✓ CV.pdf


Ignore:

✗ Movie.mp4

✗ Game.exe

```

---

# 14. Local Storage


The desktop agent uses SQLite.


Stores:

```
User session

File index

Sync status

Settings

Offline changes

```

Example:


```
local.db


Files Table

Sync Queue Table

Settings Table

```

---

# 15. Offline Mode


When internet is unavailable:


```
User edits file

        |

Save locally

        |

Add to sync queue

        |

Internet returns

        |

Automatic upload

```

---

# 16. Background Service


The application can run:


```
System Startup

        |

        ↓

AI Vault Agent

        |

        ↓

Monitor Files

```


Features:

- Start with Windows
- Minimize to tray
- Background synchronization

---

# 17. Security System


## File Encryption


Before upload:


```
Original File

      |

      ↓

Encryption

      |

      ↓

Cloud Storage

```


---

## Authentication Security


Uses:

- JWT tokens
- Refresh tokens
- Secure storage


---

# 18. Communication With Backend


## API Communication


Example:


```
Electron

POST /sync/upload


Backend

Save File


Response:

Success

```


---

# 19. WebSocket Communication


Used for:


- Real-time notifications
- Sync status
- Security alerts


Example:


```
Backend:

New notification


       ↓


Electron:

Display message

```

---

# 20. Desktop User Interface


Main Screens:


## Login

```
Email

Password

Login

```

---

## Dashboard


Shows:


```
Protected Folders

Storage Usage

Sync Status

Security Status

```

---

## Folder Management


```
Add Folder

Remove Folder

Change Permission

```

---

## Sync Status


Example:


```
Files:

542


Synced:

520


Pending:

22

```

---

# 21. Error Handling


The system handles:


## Upload Failure


```
Retry automatically

```

## Internet Loss


```
Save locally

Sync later

```

## Permission Removed


```
Stop monitoring folder

```

---

# 22. Future Features


Possible improvements:


- Mobile synchronization
- Face authentication
- Voice commands
- Advanced file recovery
- Multiple device management
- Enterprise deployment

---

# 23. Development Steps


## Phase 1

Create Electron application

- Window creation
- React interface
- Login page


## Phase 2

Connect backend

- Authentication
- Device registration


## Phase 3

File management

- Folder selection
- Scanner
- File indexing


## Phase 4

Synchronization

- Upload
- Download
- Version control


## Phase 5

Advanced features

- Offline mode
- Encryption
- AI backup selection


---

# Conclusion

The Electron Desktop Agent is the bridge between the user's physical computer and the AI Personal Digital Assistant platform.

It enables secure file access, synchronization, smart backup, and intelligent personal data protection while respecting user permission.