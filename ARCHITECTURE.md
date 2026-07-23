# HMS Architecture Overview

## System Architecture

```text
┌─────────────────────────────────────────────────────────────────┐
│                     Hospital Management System                  │
└─────────────────────────────────────────────────────────────────┘

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
         ┌──────────────────┼──────────────────┐
         │                  │                  │
         ▼                  ▼                  ▼
    ┌─────────┐         ┌─────────┐      ┌──────────┐
    │ MongoDB │         │  Redis  │      │   SMTP   │
    │Database │         │ Cache   │      │  Email   │
    └─────────┘         └─────────┘      └──────────┘
```

---

## Repository Structure

```text
hms-sprint5-deploy
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── HMS_Back_end/
│   ├── src/
│   │   ├── api/
│   │   ├── config/
│   │   ├── constants/
│   │   ├── controllers/
│   │   ├── middlewares/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── validators/
│   │   └── utils/
│   │
│   ├── package.json
│   ├── vercel.json
│   └── README.md
│
├── HMS_Front_end/
│   ├── src/
│   │   ├── app/
│   │   │   ├── core/
│   │   │   ├── features/
│   │   │   └── shared/
│   │   └── environments/
│   │
│   ├── angular.json
│   ├── package.json
│   └── vercel.json
│
└── PatientApp/
    ├── src/
    │   ├── app/
    │   ├── screens/
    │   ├── components/
    │   ├── services/
    │   ├── hooks/
    │   ├── store/
    │   ├── utils/
    │   └── styles/
    │
    ├── assets/
    ├── app.json
    └── package.json
```

---

## Technology Stack

### Web Application

- Angular
- TypeScript
- Angular Router
- Angular HttpClient
- CSS

### Mobile Application

- React Native
- Expo
- TypeScript
- Expo Router
- Zustand
- SecureStore

### Backend

- Node.js
- Express.js
- MongoDB
- Mongoose
- JWT Authentication
- Express Validators
- Rate Limiting

### DevOps

- GitHub
- GitHub Actions
- Vercel
- Expo

---

## Backend Request Flow

```text
Client Request
      │
      ▼
Route
      │
      ▼
Middleware
(Auth → Authorization → Validation)
      │
      ▼
Controller
      │
      ▼
Service / Utility
      │
      ▼
Mongoose Model
      │
      ▼
MongoDB
      │
      ▼
JSON Response
```

---

## Authentication Flow

```text
Login Request
      │
      ▼
Validate Credentials
      │
      ▼
Generate Access Token
Generate Refresh Token
      │
      ▼
Store Tokens
      │
      ▼
Authorization Header
Bearer <token>
      │
      ▼
Protected API Access
```

---

## Authorization Flow

```text
User Request
      │
      ▼
Verify JWT
      │
      ▼
Resolve Role
      │
      ▼
Resolve Permissions
      │
      ▼
Check Node Access
      │
      ▼
Allow / Deny Request
```

---

## Core Backend Modules

### Authentication

```text
/auth
```

Responsibilities:

- Login
- Logout
- Refresh Tokens
- Password Reset

### Employees

```text
/employee
/admin
```

Responsibilities:

- Employee Management
- Role Assignment
- Approval Process

### Patients

```text
/patient
/patient-self
```

Responsibilities:

- Patient Registration
- Profile Management
- Patient Search

### Appointments

```text
/appointment
```

Responsibilities:

- Booking
- Rescheduling
- Cancellation
- Availability Management

### Medical Records

```text
/medical-record
```

Responsibilities:

- Record Creation
- Record Verification
- Record Retrieval

### Permissions

```text
/permission
/node
```

Responsibilities:

- Role Permissions
- Navigation Access
- Authorization Control

### Dashboard

```text
/dashboard
```

Responsibilities:

- Statistics
- Reports
- Analytics

---

## Security Features

- JWT Authentication
- Refresh Tokens
- Password Hashing (bcrypt)
- Input Validation
- Role-Based Access Control
- Permission-Based Authorization
- Rate Limiting
- Audit Logging
- Secure HTTP Headers
- CORS Protection

---

## Scalability Considerations

- Stateless Authentication
- MongoDB Indexing
- Redis Caching
- Horizontal Scaling
- Modular Controllers
- Service Separation
- Environment-Based Configuration

---

## Deployment Architecture

```text
Angular Frontend
        │
        ▼
     Vercel

React Native App
        │
        ▼
 Expo Build/EAS

Node.js Backend
        │
        ▼
     Vercel

MongoDB Atlas
        │
        ▼
   Cloud Database
```
``