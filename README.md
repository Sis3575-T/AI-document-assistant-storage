# AI Document Assistant

An AI-powered personal digital assistant for managing, protecting, and understanding personal documents.

## Overview

This monorepo contains:

- **Web Application** (React + Vite + TypeScript)
- **Mobile Application** (React Native + Expo)
- **Backend API** (Node.js + Express + TypeScript)
- **AI Service** (Python + FastAPI)
- **Infrastructure** (Kubernetes, Terraform, GitHub Actions)

## Architecture

```
Users
  │
  ├── Web App (React)
  └── Mobile App (React Native + Expo)
        │
        ▼
  Backend API (Express)
        │
  ┌─────┼─────────────┐
  ▼     ▼             ▼
PostgreSQL Redis   AI Service (FastAPI)
                      │
         ┌────────────┼────────────┐
         ▼            ▼            ▼
      Gemini     Tesseract     pgvector
```

## Tech Stack

### Frontend (Web)
- React 18
- Vite
- TypeScript
- Tailwind CSS
- React Query
- Redux Toolkit

### Mobile
- React Native
- Expo SDK 52
- TypeScript
- Expo Router
- Zustand
- NativeWind (Tailwind for RN)
- expo-camera, expo-notifications, expo-secure-store, expo-local-authentication

### Backend
- Node.js
- Express
- TypeScript
- Prisma ORM
- PostgreSQL
- Redis

### AI
- Python
- FastAPI
- Gemini API
- Tesseract OCR
- pgvector

### DevOps
- Kubernetes
- Helm
- Terraform
- GitHub Actions
- Nginx

### Security
- JWT
- Google OAuth
- ClamAV
- AES-256 Encryption

## Getting Started

### Prerequisites
- Node.js 20+
- Python 3.11+
- PostgreSQL 15+
- Redis 7+

### Installation

```bash
# Clone repository
git clone <repo-url>
cd document-assistant

# Install dependencies
npm install

# Set up environment
cp .env.example .env
# Edit .env with your configuration

# Start development
npm run dev
```

### Mobile App

```bash
cd apps/mobile
npm install
npm start  # Starts Expo dev server
```

## Project Structure

```
document-assistant/
├── apps/
│   ├── web/              # React web application
│   ├── mobile/           # React Native mobile app (Expo)
│   └── backend/          # Express API
├── packages/
│   ├── shared/           # Shared TypeScript types
│   └── ui/               # Shared UI components
├── ai-service/           # Python FastAPI AI service
├── infrastructure/       # Kubernetes, Terraform, CI/CD
├── docs/                 # Documentation
└── turbo.json            # Turborepo config
```

## Documentation

- [System Architecture](docs/SYSTEM_ARCHITECTURE.md)
- [Mobile App Design](docs/MOBILE_APP_DESIGN.md)
- [Database Design](docs/docs/DATABASE_DESIGN.md)
- [API Documentation](docs/docs/docs/API_DOCUMENTATION.md)
- [AI Service](docs/docs/docs/docs/docs/AI_SERVICE.md)
- [Security Design](docs/docs/docs/docs/docs/docs/SECURITY_DESIGN.md)
- [Development Roadmap](docs/docs/docs/docs/docs/docs/docs/DEVELOPMENT_ROADMAP_100_TASKS.md)

## Roadmap

### Phase 1 — Foundation & Core Platform
- ✅ Task 1: Project Architecture & Planning
- ✅ Task 2: Project Setup
- ✅ Task 3: Database Design
- ✅ Task 4: Authentication Platform
- ✅ Task 5: User & Profile Management
- ✅ Task 6: Document Storage Platform
- ✅ Task 7: OCR & Document Processing
- ✅ Task 8: AI Integration
- ✅ Task 9: Enterprise Search Platform
- ✅ Task 10: AI RAG Platform

### Phase 2 — Productivity Platform
- ✅ Task 11: Document Intelligence
- ✅ Task 12: Workflow & Automation
- ✅ Task 13: Notification Platform
- ✅ Task 14: Analytics & Dashboard
- ✅ Task 15: Enterprise Collaboration

### Phase 3 — Enterprise Security
- ✅ Task 16: Enterprise Security Platform

### Phase 4 — Enterprise AI
- ✅ Task 17: Enterprise AI Platform

### Phase 5 — Synchronization Platform
- ✅ Task 18: Enterprise Synchronization

### Phase 6 — Platform Ecosystem
- ✅ Task 19: Developer Platform

### Phase 7 — Enterprise DevOps
- ✅ Task 20: Cloud Infrastructure & Deployment

## License

MIT