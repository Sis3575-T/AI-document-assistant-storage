# AI Personal Digital Assistant

# Mobile Application Design Document

## 1. Introduction

The Mobile Application is a cross-platform (iOS & Android) app that connects the user's mobile device with the AI Personal Digital Assistant platform.

It provides secure communication between:

- User device files and camera
- Backend API
- Cloud storage
- AI processing system

The purpose of the mobile app is to allow users to scan, protect, organize, and synchronize their personal documents while maintaining user permission and privacy on the go.

---

# 2. Why Mobile Application

A mobile application provides native access to device capabilities that a web browser cannot:

- Camera for document scanning
- Biometric authentication (Face ID / Touch ID / Fingerprint)
- Push notifications for real-time alerts
- Offline access to cached documents
- Native file picker and share sheet integration
- Background app refresh for sync

```
User Mobile Device

Camera
Photos
Files

       |
       ↓

AI Vault Mobile App

       |
       ↓

Backend Server

       |
       ↓

Cloud Storage
```

---

# 3. Technology Stack

## Mobile Framework

```
React Native (Expo)
```

## Frontend Interface

```
React Native
TypeScript
Expo Router (file-based navigation)
NativeWind (Tailwind CSS for React Native)
```

## State Management

```
Zustand (global state)
React Query / TanStack Query (server state)
```

## Native Capabilities

```
expo-camera - Document scanning
expo-image-picker - Gallery import
expo-notifications - Push notifications
expo-secure-store - Secure token storage
expo-local-authentication - Biometric auth
expo-file-system - Local file management
expo-background-task - Background sync
```

## Communication

```
REST API (HTTPS)
WebSocket (real-time updates)
```

---

# 4. Mobile Application Responsibilities

The application is responsible for:

- User authentication (email/password + biometric)
- Device registration
- Camera & gallery permission management
- Document scanning with auto-crop & enhancement
- OCR processing (on-device or cloud)
- File upload and management
- Synchronization with backend
- Local caching for offline access
- Push notifications
- Background synchronization
- Encryption for local data
- Secure token storage

---

# 5. Application Architecture

```
                  Mobile Application (React Native + Expo)

        ---------------------------------------------------------

        |                                                       |

        ↓                                                       ↓

  UI Layer (React Native)                              Native Modules
  (Expo Router screens)                            (Camera, Auth, Notifications)

        |                                                       |
        ---------------------------------------------------------
                        |
                        ↓
              State Layer (Zustand + React Query)
                        |
                        ↓
              Service Layer (API Client, Sync Engine)
                        |
                        ↓
              Infrastructure Layer (Secure Storage, Cache)
                        |
                        ↓
                  Backend API
```

---

# 6. Project Structure

```
apps/mobile/

│
├── app/                      # Expo Router screens (file-based routing)
│   ├── (auth)/               # Auth screens (login, register, biometric setup)
│   ├── (tabs)/               # Main tab screens (home, files, chat, settings)
│   ├── files/                # File management screens
│   ├── chat/                 # AI chat screens
│   ├── scan/                 # Document scanning flow
│   └── _layout.tsx           # Root layout with providers
│
├── components/
│   ├── ui/                   # Primitives (Button, Input, Card, Avatar)
│   ├── layout/               # Header, TabBar, Drawer, ScreenContainer
│   └── features/             # Feature-specific components
│
├── stores/                   # Zustand state stores
│   ├── authStore.ts
│   ├── filesStore.ts
│   ├── chatStore.ts
│   ├── syncStore.ts
│   └── settingsStore.ts
│
├── services/                 # API clients and native module wrappers
│   ├── api.ts                # Axios instance with interceptors
│   ├── auth.ts               # Authentication service
│   ├── files.ts              # File operations
│   ├── chat.ts               # AI chat service
│   ├── sync.ts               # Synchronization engine
│   ├── notifications.ts      # Push notification handling
│   ├── camera.ts             # Camera & scanning wrapper
│   ├── biometric.ts          # Biometric authentication
│   └── storage.ts            # Secure storage wrapper
│
├── hooks/                    # Custom React hooks
│   ├── useAuth.ts
│   ├── useFiles.ts
│   ├── useSync.ts
│   ├── useCamera.ts
│   └── useBiometric.ts
│
├── types/                    # TypeScript type definitions
│   ├── api.ts
│   ├── file.ts
│   ├── user.ts
│   └── chat.ts
│
├── utils/                    # Helper functions
│   ├── format.ts
│   ├── validation.ts
│   ├── permissions.ts
│   └── encryption.ts
│
├── constants/                # App constants, theme, config
│   ├── theme.ts
│   ├── config.ts
│   └── endpoints.ts
│
├── assets/                   # Images, fonts, icons
│
├── app.json                  # Expo config
├── eas.json                  # EAS Build config
├── tailwind.config.js        # NativeWind config
├── tsconfig.json
└── package.json
```

---

# 7. Application Workflow

## Step 1: User Login

User opens:

```
AI Vault Mobile App
```

Login options:

```
Email / Password
Biometric (Face ID / Fingerprint) - after initial setup
```

The application receives:

```
JWT Access Token
Refresh Token
```

Tokens stored securely in `expo-secure-store`.

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
Create/Update Device Record
       |
       ↓
Register Push Token
```

Stored information:

```
Device Name
Operating System (iOS/Android)
App Version
Last Active Time
Device ID (UUID)
Push Notification Token
Biometric Enabled (boolean)
```

---

# 8. Permission System

## Purpose

The user decides what device capabilities the application can access.

### Camera Permission

```
Allow AI Vault to access camera for document scanning?
[Don't Allow] [Allow]
```

### Photo Library Permission

```
Allow AI Vault to access photos for importing documents?
[Select Photos...] [Allow Full Access] [Don't Allow]
```

### Notifications Permission

```
AI Vault would like to send you notifications about:
• Document expiry reminders
• Sync status updates
• Security alerts
[Allow] [Don't Allow]
```

### Biometric Permission

```
Enable Face ID / Fingerprint for quick access?
[Not Now] [Enable]
```

The system stores permission status locally and syncs with backend.

---

# 9. Document Scanner

## Purpose

Capture high-quality document images using the device camera.

## Features

- **Auto-detect document edges** - Real-time rectangle detection
- **Auto-capture** - Automatically captures when document is stable
- **Perspective correction** - Transform to flat, rectangular image
- **Image enhancement** - Contrast, brightness, sharpening
- **Multi-page support** - Scan multiple pages into single PDF
- **Flash control** - Auto/on/off for low light
- **Grid overlay** - Alignment guide

## Scanner Flow

```
Open Scanner
      |
Camera Preview (with edge detection)
      |
User aligns document
      |
Auto-capture OR Manual capture
      |
Preview & Edit (crop, rotate, filter)
      |
Add more pages OR Continue
      |
OCR Processing
      |
Save as PDF/Image
      |
Upload to Backend
```

## Output

```json
{
  "pages": [
    {
      "imageUri": "file://...",
      "width": 2480,
      "height": 3508,
      "ocrText": "Extracted text...",
      "detectedLanguage": "en"
    }
  ],
  "documentType": "passport",
  "metadata": {
    "scannedAt": "2026-01-15T10:30:00Z",
    "deviceId": "uuid",
    "pageCount": 1
  }
}
```

---

# 10. OCR Processing

## On-Device OCR (Fast, Private)

- Uses ML Kit / Vision Framework
- Good for quick text extraction
- No internet required
- Limited language support

## Cloud OCR (Accurate, Full-featured)

- Uses Tesseract on AI service
- Better accuracy for complex documents
- Full language support
- Requires internet

## Hybrid Approach

```
Scan Document
      |
On-Device OCR (immediate preview)
      |
Upload to Backend
      |
Cloud OCR (higher accuracy)
      |
Merge results
      |
Store in Database
```

---

# 11. Synchronization System

## Upload Process

```
File Selected / Scanned
       |
Generate File Hash (SHA-256)
       |
Check Server (HEAD request)
       |
If different:
  Encrypt File (AES-256)
  Upload (multipart/resumable)
  Update Local Cache
       |
Update Sync Status
```

## Download Process

```
File Requested
       |
Check Local Cache
       |
If not cached or stale:
  Download (resumable)
  Decrypt
  Store in Cache
       |
Serve to UI
```

## Smart Sync Features

- **Delta sync** - Only changed portions for large files
- **Background sync** - Uses Expo Background Task
- **WiFi-only option** - Respect user data preferences
- **Conflict resolution** - Last-write-wins with user notification
- **Compression** - Automatic for images/PDFs

---

# 12. Local Storage & Offline Mode

## Secure Storage (expo-secure-store)

```
JWT Tokens
Refresh Tokens
Biometric Keys
Encryption Keys
Device ID
```

## AsyncStorage / MMKV (App Data)

```
User Preferences
Recent Files Cache
Sync Queue
Draft Documents
Settings
```

## SQLite (Structured Data) - Optional

```
File Index (for fast search)
Document Metadata
Chat History
Sync Logs
```

## Offline Mode

```
Internet Unavailable
       |
User Actions:
  - View cached files ✓
  - Read chat history ✓
  - Scan documents ✓
  - Queue uploads ✓
       |
Queue stored locally
       |
Internet Returns
       |
Auto-sync queue
       |
Clear synced items
```

---

# 13. Push Notifications

## Notification Types

- **Expiry Warning** - Document expiring soon
- **Sync Complete** - Background sync finished
- **Sync Failed** - Upload/download error
- **Security Alert** - New device login, suspicious activity
- **AI Processing Done** - Document analysis complete
- **Shared Document** - Someone shared a file

## Implementation

```
Backend Event
       |
Notification Service
       |
Expo Push Service (FCM/APNs)
       |
Device Receives
       |
App Handles:
  - Foreground: In-app banner
  - Background: System notification
  - Tapped: Deep link to relevant screen
```

## Deep Linking

```
ai-vault://file/{fileId}
ai-vault://chat/{documentId}
ai-vault://settings/security
ai-vault://scan
```

---

# 14. Biometric Authentication

## Setup Flow

```
User enables biometric in settings
       |
Check device capability
       |
Prompt for system biometric
       |
Generate key pair in Secure Enclave/Keystore
       |
Store public key on backend
       |
Store private key locally (hardware-backed)
```

## Unlock Flow

```
App launched / foregrounded
       |
Check biometric enabled
       |
Prompt Face ID / Fingerprint
       |
Verify with local private key
       |
Decrypt access token
       |
Auto-login
```

## Fallback

- Failed biometric → Email/password
- Too many attempts → Device passcode
- Biometric disabled → Email/password

---

# 15. Background Operations

## Background Sync (Expo Background Task)

```
App backgrounded
       |
Register background task
       |
OS wakes app periodically
       |
Check sync queue
       |
Process pending uploads/downloads
       |
Update badge count
       |
Schedule next wake
```

## Background Fetch (iOS) / WorkManager (Android)

- Minimum interval: 15 minutes (iOS), flexible (Android)
- Requires "Background App Refresh" enabled
- Respects Low Power Mode / Battery Saver

---

# 16. Security System

## File Encryption

Before upload:

```
Original File
      |
      ↓
AES-256-GCM Encryption (per-file key)
      |
      ↓
Key wrapped with user's master key
      |
      ↓
Upload encrypted file + wrapped key
```

## Secure Communication

- All API calls: HTTPS with certificate pinning
- WebSocket: WSS
- Token refresh: Automatic before expiry
- Biometric keys: Hardware-backed (Secure Enclave / StrongBox)

## Data Protection

- iOS: `NSFileProtectionCompleteUntilFirstUserAuthentication`
- Android: `EncryptedSharedPreferences` + `MasterKeys`

---

# 17. Communication With Backend

## API Communication

```
Mobile App
POST /api/files/upload
Headers: Authorization: Bearer <token>
Body: multipart/form-data

Backend
Process file
Store in cloud
Return file metadata

Response:
{
  "file": { "id": "...", "name": "...", "url": "..." }
}
```

## WebSocket Communication

Used for:

- Real-time sync status
- Live collaboration updates
- Security alerts
- AI processing progress

```
Backend: File processing complete
       ↓
WebSocket: { "type": "file.processed", "fileId": "..." }
       ↓
Mobile App: Update UI, show notification
```

---

# 18. Mobile User Interface

## Main Screens (Tab Bar)

### Home (Dashboard)

```
Storage Usage: 2.4 GB / 10 GB
Recent Files: [CV.pdf, Passport.jpg, Contract.pdf]
Expiry Alerts: [Passport - 6 months, License - 3 months]
Quick Actions: [Scan Document, Upload File, AI Chat]
```

### Files

```
My Vault
├── Documents (12)
│   ├── CV.pdf
│   └── Degree.pdf
├── Identity (5)
│   └── Passport.jpg
└── Work (8)
    └── Contract.pdf

[+] New: Scan | Upload | Create Folder
```

### Scan

```
Camera view with edge detection
[Flash] [Grid] [Auto] [Capture]
Bottom: Gallery | Multi-page
```

### Chat

```
AI Assistant
"How can I help with your documents?"

[User] What's my passport expiry?
[AI] Your passport expires on 2030-05-20.
     [View Document] [Renewal Info]

Input: [Message...] [Attach File] [Voice]
```

### Settings

```
Account
  Profile
  Security (Biometric, 2FA, Sessions)
  Notifications
  Sync (WiFi only, Background, Cache size)
  Storage (Usage, Clear cache)
  About
```

---

# 19. Error Handling

## Upload Failure

```
Retry automatically (exponential backoff)
Max 3 retries
Then: Queue for background sync
Notify user if persistent
```

## Network Loss

```
Save to local queue
Show offline indicator
Auto-retry when online
```

## Permission Denied

```
Show rationale
Deep link to system settings
Graceful degradation (e.g., no camera → gallery only)
```

## Biometric Failed

```
Fallback to password
Lock after 5 failures
Require device passcode
```

---

# 20. Performance Optimization

## Image Processing

- Downsample large images before upload
- Use `expo-image-manipulator` for client-side compression
- Progressive JPEG for photos

## List Rendering

- `FlashList` for large file lists
- Windowed rendering
- Memoized components

## Bundle Size

- Expo SDK 52+
- Tree shaking enabled
- Dynamic imports for heavy screens (chat, scan)

## Caching

- React Query: 5 min stale time, 10 min cache
- Images: `expo-image` with disk cache
- Offline: MMKV for fast key-value

---

# 21. Testing Strategy

## Unit Tests

- Services (auth, sync, encryption)
- Utilities (format, validation)
- Hooks (useAuth, useSync)

## Integration Tests

- API client with mock server
- Navigation flows
- Permission handling

## E2E Tests (Detox)

- Login → Scan → Upload → View
- Biometric unlock
- Offline → Online sync
- Push notification handling

## Device Testing

- iOS: iPhone SE, 15, 15 Pro Max
- Android: Pixel 7, Galaxy S23, Tablet
- Low-end devices for performance

---

# 22. Build & Deployment

## Development

```
npm start          # Expo dev server
npm run ios        # iOS simulator
npm run android    # Android emulator
npm run web        # Web preview
```

## EAS Build

```
# Development build (with dev client)
eas build --profile development --platform ios
eas build --profile development --platform android

# Preview (internal testing)
eas build --profile preview --platform all

# Production
eas build --profile production --platform all
```

## App Store Deployment

```
# iOS
eas submit --platform ios

# Android
eas submit --platform android
```

## OTA Updates (Expo Updates)

```
eas update --branch production --message "Bug fixes"
```

---

# 23. Future Features

Possible improvements:

- Widget (iOS) / App Shortcut (Android) for quick scan
- Apple Watch / Wear OS companion
- Voice commands for search
- AR document measurement
- Local AI models (Core ML / TensorFlow Lite)
- Share extension for quick save
- Document signing
- Family sharing

---

# 24. Development Phases

## Phase 1: Foundation

- Expo project setup
- Navigation (Expo Router)
- Authentication (login, register, tokens)
- Basic UI components & theme
- API client with interceptors

## Phase 2: Core Features

- File list & explorer
- Document viewer (PDF, images)
- Upload / download
- Basic sync engine

## Phase 3: Document Intelligence

- Camera scanner with edge detection
- OCR integration
- Multi-page PDF creation
- Gallery import

## Phase 4: AI Features

- AI chat interface
- Document classification
- Smart search
- Expiry detection & notifications

## Phase 5: Platform Polish

- Biometric authentication
- Push notifications
- Offline mode
- Background sync
- Performance optimization
- Accessibility (VoiceOver, TalkBack)

## Phase 6: Production Ready

- EAS build configuration
- App Store / Play Store submission
- Crash reporting (Sentry)
- Analytics (PostHog / Amplitude)
- Automated testing (CI/CD)

---

# Conclusion

The Mobile Application is the primary companion for users to interact with the AI Personal Digital Assistant on the go.

It leverages native mobile capabilities — camera, biometrics, push notifications, offline storage — to provide a seamless, secure, and intelligent document management experience that goes far beyond traditional cloud storage apps.