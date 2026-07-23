# HMS Database Schema

## Overview

MongoDB database design for the Hospital Management System.

---

## Collections

### Users

```json
{
  "_id": "ObjectId",
  "email": "string",
  "username": "string",
  "password": "hashed string",
  "employee": "ObjectId",
  "designation": "OWNER|ADMIN|DOCTOR|RECEPTIONIST",
  "isActive": true,
  "lastLogin": "Date",
  "refreshTokens": [],
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

Indexes:

- email
- username
- employee

---

### Employees

```json
{
  "_id": "ObjectId",
  "employeeCode": "string",
  "name": "string",
  "email": "string",
  "phone": "string",
  "department": "string",
  "designation": "string",
  "qualification": "string",
  "joiningDate": "Date",
  "status": "ACTIVE",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

Indexes:

- employeeCode
- email
- status

---

### Patients

```json
{
  "_id": "ObjectId",
  "UHID": "string",
  "firstName": "string",
  "lastName": "string",
  "email": "string",
  "phone": "string",
  "dateOfBirth": "Date",
  "gender": "string",
  "address": "string",
  "bloodGroup": "string",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

Indexes:

- UHID
- email
- phone

---

### Appointments

```json
{
  "_id": "ObjectId",
  "appointmentCode": "string",
  "patient": "ObjectId",
  "doctor": "ObjectId",
  "receptionist": "ObjectId",
  "appointmentDate": "Date",
  "status": "SCHEDULED",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

Indexes:

- appointmentCode
- patient
- doctor
- appointmentDate

---

### Medical Records

```json
{
  "_id": "ObjectId",
  "recordCode": "string",
  "appointment": "ObjectId",
  "patient": "ObjectId",
  "doctor": "ObjectId",
  "diagnosis": "string",
  "prescription": "string",
  "status": "DRAFT",
  "createdAt": "Date",
  "updatedAt": "Date"
}
```

Indexes:

- recordCode
- appointment
- patient

---

### Permissions

```json
{
  "_id": "ObjectId",
  "designation": "OWNER",
  "codes": ["CREATE_EMPLOYEE", "VIEW_PATIENT"]
}
```

Indexes:

- designation

---

### Nodes

```json
{
  "_id": "ObjectId",
  "name": "Dashboard",
  "path": "/dashboard",
  "icon": "dashboard",
  "allowedDesignations": ["OWNER", "ADMIN"]
}
```

Indexes:

- path

---

### Audit Logs

```json
{
  "_id": "ObjectId",
  "user": "ObjectId",
  "action": "CREATE_PATIENT",
  "resource": "Patient",
  "resourceId": "ObjectId",
  "status": "SUCCESS",
  "createdAt": "Date"
}
```

Indexes:

- user
- createdAt
- resource

---

## Entity Relationships

```text
Users
  │
  └── 1:1 Employees

Patients
  │
  └── 1:N Appointments

Employees (Doctor)
  │
  └── 1:N Appointments

Appointments
  │
  └── 1:1 Medical Records

Users
  │
  └── 1:N Audit Logs
```

---

## Permission Model

```text
Role
  │
  ▼
Permission
  │
  ▼
Node Access
  │
  ▼
Action Access
```

Examples:

- CREATE_PATIENT
- UPDATE_PATIENT
- DELETE_PATIENT
- CREATE_EMPLOYEE
- UPDATE_EMPLOYEE
- CREATE_APPOINTMENT
- UPDATE_APPOINTMENT
- VIEW_MEDICAL_RECORDS
- APPROVE_EMPLOYEE
- VIEW_AUDIT_LOGS

---

## Core Relationships

```text
User
 └── Employee

Employee
 ├── Appointment (Doctor)
 └── Medical Record (Doctor)

Patient
 ├── Appointment
 └── Medical Record

Appointment
 └── Medical Record

User
 └── Audit Log
```

---

## Notes

- MongoDB ObjectIds are used as references.
- Passwords are stored as bcrypt hashes.
- JWT is used for authentication.
- Audit logs are maintained for traceability.
- UTC timestamps are used across collections.
- Soft deletion is implemented via status/isActive fields where applicable.