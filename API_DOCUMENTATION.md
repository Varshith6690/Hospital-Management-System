# HMS Backend API Documentation

## Base URL
http://localhost:5000/api


## Overview

The HMS Backend provides a comprehensive REST API for managing hospital operations across web and mobile platforms. All endpoints (except auth) require JWT authentication via the `Authorization: Bearer <token>` header.

---

## Authentication Endpoints

**Base Path:** `/auth`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|-----------------|
| POST | `/login` | User login (staff) | No |
| POST | `/self-register` | Self-register employee | No |
| POST | `/change-password` | Change password | Yes |
| POST | `/forgot-password` | Request password reset | No |
| POST | `/reset-password` | Reset password with token | No |
| POST | `/logout` | User logout | Optional |
| POST | `/refresh` | Refresh access token | No |
| GET | `/me` | Get current user profile | Yes |

---

## Patient Authentication Endpoints (Mobile App)

**Base Path:** `/patient-auth`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|-----------------|
| POST | `/register` | Register new patient | No |
| POST | `/login` | Patient login | No |
| POST | `/forgot-password` | Request password reset | No |
| POST | `/reset-password` | Reset password with token | No |
| PUT | `/change-password` | Change password | Yes (Patient) |
| POST | `/refresh` | Refresh access token | No |
| POST | `/logout` | Patient logout | No |

---

## Employee Management Endpoints

**Base Path:** `/admin` (Admin/Owner only)

| Method | Endpoint | Description | Auth Required | Permissions |
|--------|----------|-------------|-----------------|------------|
| POST | `/create-employee` | Create new employee | Yes | CREATE_EMPLOYEE |
| GET | `/employees` | Get all employees | Yes | View via node |
| GET | `/employees/:employeeCode` | Get employee by code | Yes | View via node |
| GET | `/pending-employees` | Get pending approvals | Yes | View via node |
| PUT | `/approve-employee/:employeeCode` | Approve employee | Yes | APPROVE_EMPLOYEE |
| PUT | `/reject-employee/:employeeCode` | Reject employee | Yes | REJECT_EMPLOYEE |
| PUT | `/update-employee/:employeeCode` | Update employee | Yes | UPDATE_EMPLOYEE |
| DELETE | `/delete-employee/:employeeCode` | Delete employee | Yes | DELETE_EMPLOYEE |
| GET | `/audit-logs` | Get audit logs | Yes | VIEW_AUDIT_LOGS |
| GET | `/profile-change-requests` | Get profile change requests | Yes | View via node |
| PUT | `/approve-profile-change/:requestId` | Approve profile change | Yes | APPROVE_PROFILE_CHANGE |
| PUT | `/reject-profile-change/:requestId` | Reject profile change | Yes | REJECT_PROFILE_CHANGE |

---

## Admin Management Endpoints

**Base Path:** `/owner` (Owner only)

| Method | Endpoint | Description | Auth Required | Permissions |
|--------|----------|-------------|-----------------|------------|
| POST | `/create-admin` | Create new admin | Yes | CREATE_ADMIN |
| GET | `/admins` | Get all admins | Yes | View via node |
| PUT | `/update-admin/:employeeCode` | Update admin | Yes | UPDATE_ADMIN |
| DELETE | `/delete-admin/:employeeCode` | Delete admin | Yes | DELETE_ADMIN |

---

## Patient Management Endpoints

**Base Path:** `/patient`

| Method | Endpoint | Description | Auth Required | Permissions |
|--------|----------|-------------|-----------------|------------|
| POST | `/create-patient` | Create new patient | Yes | CREATE_PATIENT |
| GET | `/` | Get all patients | Yes | View via node |
| GET | `/search` | Search patients | Yes | View via node |
| GET | `/:UHID` | Get patient by UHID | Yes | View via node |
| PUT | `/:UHID` | Update patient | Yes | UPDATE_PATIENT |
| DELETE | `/:UHID` | Delete patient | Yes | DELETE_PATIENT |

---

## Patient Self Service Endpoints (Mobile App)

**Base Path:** `/patient-self`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|-----------------|
| GET | `/me` | Get my profile | Yes (Patient) |
| PUT | `/me` | Update my profile | Yes (Patient) |
| GET | `/doctors` | Get available doctors | Yes (Patient) |
| GET | `/booked-slots` | Get doctor appointment slots | Yes (Patient) |
| GET | `/appointments` | Get my appointments | Yes (Patient) |
| POST | `/appointments` | Book appointment | Yes (Patient) |
| PUT | `/appointments/:appointmentId` | Update my appointment | Yes (Patient) |
| PUT | `/appointments/:appointmentId/cancel` | Cancel my appointment | Yes (Patient) |
| GET | `/medical-records` | Get my medical records | Yes (Patient) |
| GET | `/medical-records/by-appointment/:appointmentId` | Get record by appointment | Yes (Patient) |
| GET | `/medical-records/:medicalRecordId` | Get specific medical record | Yes (Patient) |

---

## Appointment Management Endpoints

**Base Path:** `/appointment`

| Method | Endpoint | Description | Auth Required | Permissions |
|--------|----------|-------------|-----------------|------------|
| POST | `/create-appointment` | Book appointment | Yes | CREATE_APPOINTMENT |
| GET | `/my` | Get my appointments | Yes | VIEW_MY_APPOINTMENTS |
| GET | `/booked-slots` | Get booked appointment slots | Yes | CREATE/UPDATE_APPOINTMENT |
| GET | `/` | Get all appointments | Yes | VIEW_ALL/MY_APPOINTMENTS |
| GET | `/:appointmentId` | Get appointment by ID | Yes | VIEW_ALL/MY_APPOINTMENTS |
| PUT | `/:appointmentId` | Update appointment | Yes | UPDATE_APPOINTMENT |
| PUT | `/:appointmentId/cancel` | Cancel appointment | Yes | CANCEL_APPOINTMENT |
| PUT | `/:appointmentId/unattended` | Mark as unattended | Yes | MARK_APPOINTMENT_UNATTENDED |

---

## Medical Records Endpoints

**Base Path:** `/medical-record`

| Method | Endpoint | Description | Auth Required | Permissions |
|--------|----------|-------------|-----------------|------------|
| POST | `/` | Create medical record | Yes | CREATE_MEDICAL_RECORD_DRAFT / CREATE_AND_FINALIZE_MEDICAL_RECORD |
| GET | `/` | List medical records | Yes | VIEW_ALL/MY_MEDICAL_RECORDS |
| GET | `/by-appointment/:appointmentId` | Get record by appointment | Yes | VIEW_ALL/MY_MEDICAL_RECORDS |
| GET | `/:medicalRecordId` | Get specific record | Yes | VIEW_ALL/MY_MEDICAL_RECORDS |
| PUT | `/:medicalRecordId` | Update medical record | Yes | CREATE_MEDICAL_RECORD_DRAFT / VERIFY_AND_FINALIZE_MEDICAL_RECORD |
| DELETE | `/:medicalRecordId` | Delete medical record | Yes | DELETE_MEDICAL_RECORD |

---

## Employee Profile Endpoints

**Base Path:** `/employee`

| Method | Endpoint | Description | Auth Required | Permissions |
|--------|----------|-------------|-----------------|------------|
| GET | `/me` | Get my profile | Yes | - |
| GET | `/doctors` | Get available doctors | Yes | CREATE/UPDATE_APPOINTMENT |
| PUT | `/update-profile` | Update my profile | Yes | UPDATE_SELF / UPDATE_SELF_DIRECT |

---

## Dashboard Statistics Endpoints

**Base Path:** `/dashboard`

| Method | Endpoint | Description | Auth Required | Role Required |
|--------|----------|-------------|-----------------|------------|
| GET | `/stats` | General dashboard stats | Yes | Any |
| GET | `/admin/stats` | Admin dashboard stats | Yes | OWNER, ADMIN |
| GET | `/doctor/stats` | Doctor dashboard stats | Yes | DOCTOR |
| GET | `/receptionist/stats` | Receptionist dashboard stats | Yes | RECEPTIONIST |
| GET | `/appointments/stats` | Appointment statistics | Yes | OWNER, ADMIN |
| GET | `/patients/stats` | Patient statistics | Yes | OWNER, ADMIN |
| GET | `/employees/stats` | Employee statistics | Yes | OWNER, ADMIN |

---

## Node Management Endpoints (Admin Panel)

**Base Path:** `/node`

| Method | Endpoint | Description | Auth Required | Role Required |
|--------|----------|-------------|-----------------|------------|
| GET | `/` | Get all sidebar nodes | Yes | OWNER |
| POST | `/create-node` | Create new sidebar node | Yes | OWNER |
| PUT | `/update-node/:nodeId` | Update sidebar node | Yes | OWNER |
| DELETE | `/delete-node/:nodeId` | Delete sidebar node | Yes | OWNER |
| GET | `/my-nodes` | Get my visible nodes | Yes | Any |

---

## Permission Management Endpoints

**Base Path:** `/permission`

| Method | Endpoint | Description | Auth Required | Role Required |
|--------|----------|-------------|-----------------|------------|
| GET | `/` | Get all permissions by role | Yes | OWNER |
| PUT | `/update-permissions/:designation` | Update role permissions | Yes | OWNER |
| GET | `/my-permissions` | Get my effective permissions | Yes | Any |

---

## Authentication

### JWT Token Format

Include in request headers:
Authorization: Bearer <your_jwt_token>


### Request/Response Format

All requests and responses use JSON format.

**Success Response:**
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { /* response data */ }
}

**Error Response:**
```json
{
  "success": false,
  "message": "Error description",
  "statusCode": 400,
  "errors": [ ]
}


## Rate Limiting
Login endpoint: 5 requests per 15 minutes per IP
Password reset endpoints: 3 requests per 1 hour per IP

## Key Features
Role-Based Access Control (RBAC) - 4 roles: Owner, Admin, Doctor, Receptionist
Permission-Based Authorization - Fine-grained permissions per action
Node-Based Module Access - Sidebar visibility controlled via nodes
Dual Authentication - Staff (web) and Patient (mobile) separate auth flows
Audit Logging - Track all critical operations
Email Notifications - Appointment confirmations, password resets
Medical Records Workflow - Draft → Verified → Finalized workflow

## Database Models

--> Core Entities:

Users (Staff)
Patients
Employees
Appointments
Medical Records
Permissions
Nodes (Sidebar navigation)
Audit Logs

--> Relationships:

Users -> Permissions (via designation/role)
Patients -> Appointments (1:many)
Appointments -> Medical Records (1:1)
Employees -> Appointments (Doctor - 1:many)