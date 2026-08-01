# AI Personal Digital Assistant

# Database Design Document

## 1. Introduction

This document describes the database design for the AI Personal Digital Assistant system.

The database is responsible for storing:

- User information
- Devices
- Files
- Folders
- Documents
- AI analysis results
- Notifications
- Security records
- Sharing permissions
- User preferences

Database technology:

```
PostgreSQL
```

ORM:

```
Prisma
```

---

# 2. Database Design Principles

The database follows:

- Relational database design
- Data normalization
- Secure data access
- Scalable structure
- Audit tracking

---

# 3. Database Architecture

```
                    Users

                      |

                      |

                  Devices

                      |

                      |

                   Files

                      |

        ----------------------------

        |                          |

   Documents                 File Versions

        |

        |

 AI Analysis Results

        |

        |

   AI Knowledge Base


```

---

# 4. Main Database Tables

The system contains the following tables:

```
Users

Profiles

Devices

Folders

Files

FileVersions

DocumentMetadata

DocumentAnalysis

DocumentEmbeddings

Categories

Notifications

ExpiryRecords

SharingLinks

Permissions

AIChats

AIChatMessages

SecurityEvents

AuditLogs

UserSettings

EmergencyKits

Translations

```

---

# 5. Users Table

## Purpose

Stores account information.

Table:

```
users
```


Fields:

| Field | Type | Description |
|-|-|-|
| id | UUID | Primary key |
| full_name | VARCHAR | User name |
| email | VARCHAR | Unique email |
| password_hash | TEXT | Encrypted password |
| role | ENUM | USER/ADMIN |
| created_at | TIMESTAMP | Account creation |
| updated_at | TIMESTAMP | Last update |


Relationship:

```
User

  |

  |

Many Devices

```

---

# 6. User Profile Table


Table:

```
profiles
```


Purpose:

Stores personal information.


Fields:

| Field | Type |
|-|-|
| id | UUID |
| user_id | UUID |
| phone | VARCHAR |
| country | VARCHAR |
| language | VARCHAR |
| timezone | VARCHAR |


Relationship:

```
User

1 ---- 1

Profile

```

---

# 7. Devices Table


Table:

```
devices
```


Purpose:

Stores connected computers.


Fields:

| Field | Type |
|-|-|
| id | UUID |
| user_id | UUID |
| device_name | VARCHAR |
| operating_system | VARCHAR |
| device_token | TEXT |
| last_connected | TIMESTAMP |
| created_at | TIMESTAMP |


Example:


```
User

Laptop

Desktop

```

---

# 8. Folders Table


Table:

```
folders
```


Purpose:

Stores user file structure.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| user_id | UUID |
| parent_id | UUID |
| name | VARCHAR |
| path | TEXT |
| created_at | TIMESTAMP |


Supports:


```
Documents

    |

    ├── Certificates

    └── Contracts

```

---

# 9. Files Table


Table:

```
files
```


Purpose:

Stores file information.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| user_id | UUID |
| folder_id | UUID |
| device_id | UUID |
| filename | VARCHAR |
| file_type | VARCHAR |
| file_size | BIGINT |
| storage_path | TEXT |
| hash | VARCHAR |
| status | ENUM |
| created_at | TIMESTAMP |


Status:


```
ACTIVE

DELETED

SYNCING

FAILED

```

---

# 10. File Versions Table


Table:

```
file_versions
```


Purpose:

Stores previous versions.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| file_id | UUID |
| version_number | INT |
| storage_path | TEXT |
| created_at | TIMESTAMP |


Example:


```
CV.pdf

Version 1

Version 2

Version 3

```

---

# 11. Document Metadata Table


Table:

```
document_metadata
```


Purpose:

Stores extracted information.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| file_id | UUID |
| document_type | VARCHAR |
| title | VARCHAR |
| issue_date | DATE |
| expiry_date | DATE |
| extracted_text | TEXT |


Example:


Passport:


```
Document Type:
Passport


Expiry:
2030-05-20

```

---

# 12. Document Analysis Table


Table:

```
document_analysis
```


Purpose:

Stores AI analysis.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| file_id | UUID |
| category | VARCHAR |
| summary | TEXT |
| confidence | FLOAT |
| analyzed_at | TIMESTAMP |


Example:


```
Category:

Education


Confidence:

95%

```

---

# 13. AI Embeddings Table


Table:

```
document_embeddings
```


Purpose:

Stores vector information for AI search.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| file_id | UUID |
| embedding | VECTOR |
| created_at | TIMESTAMP |


Used for:


- Semantic search
- Chat with documents


---

# 14. Categories Table


Table:


```
categories
```


Default categories:


```
Education

Identity

Work

Finance

Medical

Personal

```

Fields:


| Field | Type |
|-|-|
| id | UUID |
| name | VARCHAR |

---

# 15. Notifications Table


Table:


```
notifications
```


Purpose:

Stores user alerts.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| user_id | UUID |
| title | VARCHAR |
| message | TEXT |
| type | VARCHAR |
| is_read | BOOLEAN |
| created_at | TIMESTAMP |


Examples:


```
Passport expires in 6 months

New file synchronized

Security warning

```

---

# 16. Expiry Records Table


Table:


```
expiry_records
```


Purpose:

Tracks document deadlines.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| file_id | UUID |
| expiry_date | DATE |
| reminder_period | INT |
| notification_enabled | BOOLEAN |


Example:


```
Expiry:

December 2028


Reminder:

6 months before

```

---

# 17. Sharing Links Table


Table:


```
sharing_links
```


Purpose:

Secure file sharing.


Fields:


| Field | Type |
|-|-|
| id | UUID |
| file_id | UUID |
| token | VARCHAR |
| password | VARCHAR |
| expires_at | TIMESTAMP |
| download_limit | INT |


Example:


```
Share certificate

Expires after 24 hours

```

---

# 18. Permissions Table


Table:


```
permissions
```


Fields:


| Field | Type |
|-|-|
| id | UUID |
| sharing_id | UUID |
| permission_type | VARCHAR |


Values:


```
VIEW

DOWNLOAD

EDIT

```

---

# 19. AI Chat Tables


## AI Chats


```
ai_chats
```


Fields:


| Field | Type |
|-|-|
| id | UUID |
| user_id | UUID |
| title | VARCHAR |
| created_at | TIMESTAMP |


---

## Messages


```
ai_chat_messages
```


Fields:


| Field | Type |
|-|-|
| id | UUID |
| chat_id | UUID |
| sender | VARCHAR |
| message | TEXT |
| created_at | TIMESTAMP |


Example:


```
User:

Find my degree


AI:

Degree found

```

---

# 20. Security Events Table


Table:


```
security_events
```


Stores:


- Login attempts
- Suspicious activity
- File access


Fields:


| Field | Type |
|-|-|
| id | UUID |
| user_id | UUID |
| event_type | VARCHAR |
| description | TEXT |
| created_at | TIMESTAMP |

---

# 21. Audit Logs Table


Table:


```
audit_logs
```


Tracks:


```
Who

Did what

When

```

Example:


```
User downloaded passport.pdf

Date:
2026-01-01

```

---

# 22. User Settings Table


Table:


```
user_settings
```


Stores:


- Language
- Notifications
- Privacy settings


---

# 23. Emergency Kit Table


Table:


```
emergency_kits
```


Stores selected important documents.


Example:


```
Passport

ID

Medical Record

```

---

# 24. Translation Table


Table:


```
translations
```


Stores:


- Original text
- Translated text
- Language


Supports:


```
English

Amharic

Afaan Oromo

Tigrinya

```

---

# 25. Main Relationships


## User Relationship


```
User

 |

 |---- Devices

 |

 |---- Files

 |

 |---- Notifications

 |

 |---- AI Chats

 |

 |---- Settings

```

---

## File Relationship


```
File

 |

 |---- Versions

 |

 |---- Metadata

 |

 |---- AI Analysis

 |

 |---- Embeddings

 |

 |---- Sharing Links

```

---

# 26. Prisma Model Structure


Project:


```
backend/

prisma/

└── schema.prisma

```

Contains:


```
model User {}

model File {}

model Device {}

model DocumentAnalysis {}

model Notification {}

```

---

# 27. Database Security

Requirements:


- Encrypt sensitive fields
- Use prepared queries
- Apply user ownership checks
- Hide private file paths
- Maintain audit history


---

# 28. Future Database Improvements


Possible additions:

- Graph database for document relationships
- Redis cache
- Elasticsearch for advanced search
- Object storage metadata service


---

# Conclusion

This database design supports all 25 planned functionalities.

The structure allows the system to grow from a university project into a scalable AI personal data management platform.
