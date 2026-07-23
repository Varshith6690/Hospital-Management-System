# Hospital Management System

Full-stack cross-platform hospital management system with Angular web dashboard, React Native patient mobile app, and Node.js/Express backend.

## Overview

A comprehensive Hospital Management System built during full-stack training at UST Global. The system features a unified Express.js + MongoDB backend serving both an Angular web dashboard for hospital staff and a React Native mobile app for patients.

## Key Features

### Web Dashboard (Staff)

- Role-Based Access Control — Owner, Admin, Doctor, Receptionist roles with fine-grained permissions
- Employee Management — Approval workflows, profile change requests, audit logging
- Patient Management — Registration, search, UHID-based records
- Appointment Scheduling — Double-booking prevention, slot management, notifications
- Medical Records — Draft → Verified → Finalized workflow
- Dashboard Analytics — Role-specific statistics and insights
- Permission System — Configurable permissions per designation
- Sidebar Navigation — Dynamic node-based access control

### Mobile App (Patients)

- Self Registration — Patient onboarding with profile management
- Appointment Booking — View available doctors, book/edit/cancel appointments
- Medical Records — Read-only access to finalized medical records
- Real-time Sync — Shared backend ensures data consistency

### Backend (API)

- RESTful API — Comprehensive endpoints for all operations
- JWT Authentication — Dual auth flows (staff + patient)
- Input Validation — express-validator for all requests
- Rate Limiting — Brute-force protection
- Audit Logging — Track all critical operations
- Email Notifications — Appointment confirmations, password resets

## Architecture

```text
┌──────────────────────┐         ┌──────────────────────┐
│   Angular Web App    │         │  React Native Mobile │
│  (Admin Dashboard)   │         │   (Patient App)      │
└──────────┬───────────┘         └──────────┬───────────┘
           │                                │
           │  REST API (HTTP/HTTPS)         │
           │                                │
           └────────────────┬───────────────┘
                            │
                            ▼
                ┌───────────────────────┐
                │   Express.js Server   │
                │   (Node.js Backend)   │
                └───────────┬───────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │    MongoDB    │
                    └───────────────┘
```

## Project Structure

```text
hms-sprint5-deploy/
├── backend/              # Node.js + Express + MongoDB API
├── web-frontend/         # Angular Web Application
├── mobile-app/           # React Native (Expo) Mobile App
├── ARCHITECTURE.md       # System architecture documentation
├── API_DOCUMENTATION.md  # Complete API reference
└── README.md             # This file
```

## Quick Start

### Prerequisites

- Node.js 14+ and npm
- MongoDB 4.4+ (local or cloud)
- Angular CLI 14+
- Expo CLI (for mobile app)

### Installation

#### 1. Clone the Repository

```bash
git clone https://github.com/Varshith6690/Hospital-Management-System.git

cd Hospital-Management-System
```

#### 2. Backend Setup

```bash
cd backend

npm install

cp .env.example .env

# Edit .env with your MongoDB URI and other config

npm run dev
```

Backend runs on:

```text
http://localhost:5000
```

#### 3. Web Frontend Setup

```bash
cd web-frontend

npm install

npm start
```

Web application runs on:

```text
http://localhost:4200
```

#### 4. Mobile App Setup

```bash
cd mobile-app

npm install

npx expo start
```

Scan the QR code using Expo Go on Android or iOS.

## Default Login Credentials

### Web Dashboard (Staff)

- Owner: `owner@hospital.com` / `Owner@123`
- Admin: `admin@hospital.com` / `Admin@123`
- Doctor: `doctor@hospital.com` / `Doctor@123`

### Mobile Application (Patient)

- Register through the mobile application or create a patient account through the web dashboard.

## Documentation

- ./ARCHITECTURE.md
- ./API_DOCUMENTATION.md
- ./backend/DATABASE_SCHEMA.md
- ./backend/README.md
- Web Frontend Documentation
- ./mobile-app/README.md

## Tech Stack

| Layer | Technology |
|---------|------------|
| Frontend (Web) | Angular 14+, TypeScript, RxJS, Angular Material |
| Frontend (Mobile) | React Native, Expo, Zustand, Expo Router |
| Backend | Node.js, Express.js, MongoDB, Mongoose |
| Authentication | JWT (Access + Refresh Tokens) |
| Validation | express-validator |
| Email | Nodemailer |
| Security | bcrypt, helmet, cors, rate-limit |

## Features by Role

| Feature | Owner | Admin | Doctor | Receptionist | Patient (Mobile) |
|----------|----------|----------|----------|----------|----------|
| Manage Employees | Yes | Yes | No | No | No |
| Manage Patients | Yes | Yes | Yes | Yes | No |
| Book Appointments | Yes | Yes | Yes | Yes | Yes |
| Create Medical Records | Yes | Yes | Yes | No | No |
| View Medical Records | Yes | Yes | Yes | No | Own Records |
| Manage Permissions | Yes | No | No | No | No |
| View Audit Logs | Yes | Yes | No | No | No |
| Dashboard Analytics | Yes | Yes | Yes | Yes | No |

## Security Features

- JWT-based stateless authentication
- Bcrypt password hashing (10 salt rounds)
- Rate limiting on authentication endpoints
- Input validation and sanitization
- CORS protection
- Helmet.js security headers
- Audit logging for critical operations
- Role-based and permission-based authorization

## Testing

```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd web-frontend
npm test

# Mobile application tests
cd mobile-app
npm test
```

## Performance

- API Response Time: < 200ms average
- Database Queries: Optimized with indexes
- Bundle Size (Web): ~500KB gzipped
- Mobile App Size: ~15MB (Expo Go)

## Contributing

Contributions are welcome.

Please read:

```text
CONTRIBUTING.md
```

before submitting changes.

## License

This project is licensed under the MIT License.

See:

```text
LICENSE
```

for details.

## Author

**Varshith Jakkula**

Portfolio:  
https://varshith-portfolio-self.vercel.app/

GitHub:  
https://github.com/Varshith6690

LinkedIn:  
https://www.linkedin.com/in/varshith-jakkula-34145a273/

Email:  
21r21a6690@gmail.com

## Acknowledgments

- Built during Full-Stack Development training at UST Global (Mar 2026 – Jun 2026)
- Special thanks to UST mentors and training team

## Project Status

Completed — Fully functional cross-platform HMS with web and mobile clients.

## Project Timeline

- Start Date: March 2026
- End Date: June 2026
- Duration: 4 Months
- Type: Full-Stack Training Project

---

Star this repository if you find it helpful.