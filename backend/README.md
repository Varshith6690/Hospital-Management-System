# HMS Backend API

Node.js + Express.js + MongoDB REST API for Hospital Management System

## Overview

Production-grade RESTful API serving both Angular web dashboard (staff) and React Native mobile app (patients) with comprehensive authentication, authorization, and audit logging.

## Features

- Dual Authentication — Separate JWT flows for staff and patients
- Role-Based Access Control — 4 roles (Owner, Admin, Doctor, Receptionist)
- Permission System — Fine-grained action-level permissions
- Node-Based Authorization — Dynamic sidebar/module access
- Input Validation — express-validator on all endpoints
- Rate Limiting — Brute-force protection on auth routes
- Audit Logging — Track all critical operations
- Email Notifications — Appointment confirmations, password resets
- Scalable Architecture — Stateless, horizontally scalable

## Architecture

```text
Express Server
    │
    ├── Routes (API endpoints)
    ├── Controllers (Business logic)
    ├── Middlewares (Auth, validation, rate-limit)
    ├── Models (Mongoose schemas)
    ├── Validators (Input validation rules)
    ├── Utils (Helper functions)
    └── Config (Environment, database)
```

## Folder Structure

```text
backend/
├── src/
│   ├── api/              # API utilities
│   ├── config/           # Configuration files
│   ├── constants/        # Constants (permissions, roles, etc.)
│   ├── controllers/      # Route controllers
│   ├── middlewares/      # Express middlewares
│   ├── models/           # Mongoose models
│   ├── routes/           # API routes
│   ├── utils/            # Helper functions
│   └── validators/       # Input validation
├── app.js                # Express app setup
├── server.js             # Server entry point
├── package.json
├── .env.example          # Environment variables template
├── DATABASE_SCHEMA.md    # Database schema docs
└── README.md             # This file
```

## Quick Start

### Prerequisites

- Node.js 14+
- MongoDB 4.4+ (local or MongoDB Atlas)
- npm or yarn

### Installation

```bash
# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Edit .env with your configuration
# Required: MONGO_URI, JWT_SECRET, EMAIL credentials
```

### Environment Variables

Create `.env` file in `backend/` folder:

```env
# Server
PORT=5000
NODE_ENV=development

# Database
MONGO_URI=mongodb://localhost:27017/hms

# JWT
JWT_SECRET=your-super-secret-jwt-key-change-this
JWT_ACCESS_EXPIRY=15m
JWT_REFRESH_EXPIRY=7d

# Email (Nodemailer)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password

# Frontend URLs (for CORS)
WEB_URL=http://localhost:4200
MOBILE_URL=exp://192.168.1.100:8081
```

### Run Development Server

```bash
npm run dev
```

Server runs on:

```text
http://localhost:5000
```

### Run Production Server

```bash
npm start
```

## API Documentation

See:

- ../API_DOCUMENTATION.md

Base URL:

```text
http://localhost:5000/api
```

### Key Endpoints

| Module | Base Path | Description |
|----------|----------|----------|
| Auth | `/auth` | Staff login, register, password reset |
| Patient Auth | `/patient-auth` | Patient login, register, password reset |
| Employees | `/admin` | Employee CRUD, approvals |
| Patients | `/patient` | Patient CRUD, search |
| Appointments | `/appointment` | Appointment booking, management |
| Medical Records | `/medical-record` | Medical records CRUD, workflow |
| Dashboard | `/dashboard` | Statistics, analytics |
| Permissions | `/permission` | Permission management |
| Nodes | `/node` | Sidebar navigation management |

## Authentication

### Staff Login

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "doctor@hospital.com",
  "password": "Doctor@123"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGc...",
    "refreshToken": "eyJhbGc...",
    "user": {}
  }
}
```

### Patient Login

```http
POST /api/patient-auth/login
Content-Type: application/json

{
  "email": "patient@example.com",
  "password": "Patient@123"
}
```

### Using JWT Token

```http
GET /api/employee/me
Authorization: Bearer eyJhbGc...
```

## Database

### MongoDB Collections

- `users` — Staff user accounts
- `employees` — Employee records
- `patients` — Patient records
- `appointments` — Appointment bookings
- `medicalRecords` — Medical records
- `permissions` — Role-permission mappings
- `nodes` — Sidebar navigation nodes
- `auditLogs` — Audit trail

See:

- ./DATABASE_SCHEMA.md

for complete schema.

## Security

- Password Hashing — bcrypt with 10 salt rounds
- JWT Tokens — Access (15 minutes) + Refresh (7 days)
- Rate Limiting
  - Login: 5 attempts per 15 minutes
  - Password reset: 3 attempts per hour
- Input Validation — All inputs sanitized
- CORS — Configured for web + mobile origins
- Helmet.js — Security headers
- Audit Logging — Track all mutations

## Authorization Layers

### 1. Authentication Middleware

Verifies JWT token validity.

### 2. Node Authorization

Checks if the user role can access the module (sidebar node).

### 3. Permission Check

Verifies the user has specific permission for the action.

Example:

```javascript
router.post(
  "/create-employee",
  auth,
  authorizeNode("/dashboard/employees"),
  requirePermission("CREATE_EMPLOYEE"),
  validate,
  controller.createEmployee
);
```

## Logging

### Audit Logs

All critical operations logged with:

- User ID
- Action performed
- Resource affected
- Before/after state
- IP address
- Timestamp

### Server Logs

```bash
# Development
npm run dev

# Production
npm start
```

## Testing

```bash
# Run tests
npm test

# Coverage
npm run test:coverage

# Specific file
npm test -- src/controllers/__tests__/authController.test.js
```

## Deployment

### Vercel Deployment

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Production Deploy
vercel --prod
```

See:

```text
vercel.json
```

for deployment configuration.

### Environment Variables (Production)

Set the following in Vercel:

- MONGO_URI
- JWT_SECRET
- EMAIL_HOST
- EMAIL_USER
- EMAIL_PASS
- WEB_URL
- MOBILE_URL

## Performance

- Average Response Time: < 200ms
- Database Queries: Optimized with indexes
- Concurrent Requests: Supports 1000+ req/sec
- Horizontal Scaling: Stateless architecture

## Tech Stack

- Runtime: Node.js 14+
- Framework: Express.js
- Database: MongoDB + Mongoose ODM
- Authentication: JWT (jsonwebtoken)
- Validation: express-validator
- Email: Nodemailer
- Security: bcrypt, helmet, cors, express-rate-limit
- Dev Tools: nodemon, dotenv

## API Conventions

### Success Response

```json
{
  "success": true,
  "message": "Operation successful",
  "data": {}
}
```

### Error Response

```json
{
  "success": false,
  "message": "Error description",
  "statusCode": 400,
  "errors": []
}
```

### HTTP Status Codes

- 200 — Success
- 201 — Created
- 400 — Bad Request
- 401 — Unauthorized
- 403 — Forbidden
- 404 — Not Found
- 429 — Too Many Requests
- 500 — Internal Server Error

## Scripts

```bash
npm run dev       # Start development server
npm start         # Start production server
npm test          # Run tests
npm run lint      # Lint code
npm run format    # Format code
```

## Troubleshooting

### MongoDB Connection Error

```bash
mongod --version

mongo
```

Update:

```env
MONGO_URI=
```

in `.env`.

### Port Already in Use

```bash
PORT=5001
```

or

```bash
lsof -ti:5000 | xargs kill -9
```

### Email Not Sending

- Use Gmail App Passwords if 2FA is enabled.
- Verify EMAIL_* environment variables.

## Development

### Adding a New Endpoint

1. Create route in `src/routes/`
2. Create controller in `src/controllers/`
3. Add validation in `src/validators/`
4. Add middleware if required
5. Update API documentation

## Documentation

- ../README.md
- ../ARCHITECTURE.md
- API Documentation
- ./DATABASE_SCHEMA.md

## License

MIT

## Author

**Varshith Jakkula**

GitHub:  
https://github.com/Varshith6690

Portfolio:  
https://varshith-portfolio-self.vercel.app/

Email:  
21r21a6690@gmail.com