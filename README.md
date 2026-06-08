# 🏥 Clinic Management System

<div align="center">

![Java](https://img.shields.io/badge/Java-23-007396?style=for-the-badge&logo=java&logoColor=white)
![SparkJava](https://img.shields.io/badge/SparkJava-2.9.4-orange?style=for-the-badge)
![Oracle DB](https://img.shields.io/badge/Oracle_DB-23c-F80000?style=for-the-badge&logo=oracle&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-3.8.1-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-Frontend-E34F26?style=for-the-badge&logo=html5&logoColor=white)

> A full-stack clinic management solution with role-based portals for **Doctors**, **Receptionists**, and **Admins** — powered by a Java REST API and Oracle Database.

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [System Architecture](#-system-architecture)
- [User Roles & Features](#-user-roles--features)
- [API Reference](#-api-reference)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Database Setup](#-database-setup)
- [Running the Application](#-running-the-application)

---

## 🌟 Overview

The **Clinic Management System** is a complete web-based application designed to digitize and streamline day-to-day clinic operations. It provides dedicated, role-specific dashboards for medical staff — allowing secure login, patient tracking, appointment scheduling, prescription management, and payment handling — all connected to a central Oracle database through a Java REST backend.

```
┌─────────────────────────────────────────────────────────┐
│                  CLINIC MANAGEMENT SYSTEM               │
│                                                         │
│   👨‍⚕️ Doctor      👩‍💼 Receptionist      🔧 Admin       │
│   Dashboard        Dashboard           Dashboard        │
│      │                 │                    │           │
│      └────────┬────────┘                    │           │
│               ▼                             │           │
│      Java REST API (SparkJava)              │           │
│         Port: 4567                          │           │
│               │                             │           │
│               ▼                             │           │
│        Oracle Database ◄────────────────────┘           │
└─────────────────────────────────────────────────────────┘
```

---

## 🏗️ System Architecture

### High-Level Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                        FRONTEND (HTML/CSS/JS)                │
│                                                              │
│  ┌─────────────┐   ┌──────────────────┐   ┌─────────────┐  │
│  │  Portal     │   │  Doctor          │   │ Receptionist│  │
│  │  Login Page │   │  Interface       │   │ Interface   │  │
│  │             │   │  ─────────────── │   │ ─────────── │  │
│  │  • Doctor   │   │  • Dashboard     │   │ • Dashboard │  │
│  │  • Recept.  │   │  • Patient Hx    │   │ • Pat. Hx   │  │
│  │            │   │  • Checkup       │   │ • Add Rec.  │  │
│  └──────┬──────┘   │  • Prescription  │   │ • Appoint.  │  │
│         │          │  • Queue Mgmt    │   │ • Payment   │  │
│         │          └────────┬─────────┘   └──────┬──────┘  │
└─────────┼───────────────────┼────────────────────┼─────────┘
          │                   │                    │
          ▼                   ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│               BACKEND — Java SparkJava (Port 4567)          │
│                                                             │
│  BackendServer.java                                         │
│  ├── setupReceptionistRoutes()                              │
│  │     GET  /patientHistory      POST /registerPatient      │
│  │     GET  /allPatients         POST /registerAppointment  │
│  │     GET  /specialties         POST /makePayment          │
│  │     GET  /doctors             POST /receptionist/login   │
│  │     GET  /searchPatients      POST /receptionist/changePassword │
│  │                                                          │
│  └── setupDoctorRoutes()                                    │
│        POST /doctor/login         POST /createPrescription  │
│        GET  /doctor/info          POST /addCheckupDetails   │
│        GET  /prescriptions        POST /processNextPatient  │
│        GET  /patientPrescriptionHistory                     │
│        GET  /doctor/currentPatientName                      │
│        GET  /doctor/queueCount                              │
│                                                             │
│  ┌──────────────┐  ┌─────────────┐  ┌──────────────────┐  │
│  │ Receptionist │  │   Doctor    │  │     clinicDB      │  │
│  │   .java      │  │   .java     │  │     .java         │  │
│  │  (Business   │  │ (Business   │  │ (DB Connection)   │  │
│  │   Logic)     │  │  Logic)     │  │  Oracle JDBC      │  │
│  └──────────────┘  └─────────────┘  └──────────────────┘  │
└─────────────────────┬───────────────────────────────────────┘
                      │  JDBC (ojdbc17)
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              Oracle Database (localhost:1521/orcl)           │
│                                                             │
│  Tables: PATIENTS · APPOINTMENTS · PAYMENTS                  │
│          DOCTORS · DOCTOR_LOGIN · RECEPTIONIST_LOGIN        │
└─────────────────────────────────────────────────────────────┘
```

### Request Flow

```
Browser                  BackendServer              Database
   │                          │                        │
   │──── HTTP Request ────────►│                        │
   │                          │──── SQL Query ─────────►│
   │                          │◄─── ResultSet ──────────│
   │◄─── JSON Response ───────│                        │
   │                          │                        │
```

---

## 👥 User Roles & Features

### 🔐 Portal Login

The entry point presents two role cards for secure login:

```
╔═══════════════════════════════════════════╗
║           Choose Your Role                ║
╠══════════════════╦════════════════════════╣
║                  ║                        ║
║   👨‍⚕️  Doctor    ║   👩‍💼  Receptionist    ║
║                  ║                        ║
║  [Login Page]    ║  [Login Page]          ║
║                  ║                        ║
╚══════════════════╩════════════════════════╝
```

Both portals support: **Login · Forgot Password · Reset Password**

---

### 👨‍⚕️ Doctor Interface

```
┌────────────────────────────────────────────────────────┐
│  SIDEBAR              │  MAIN CONTENT AREA             │
│                       │                                │
│  🏠 Dashboard         │  ┌──────────────────────────┐ │
│  📋 Patient History   │  │   Current Queue Patient  │ │
│  🩺 Checkup Details   │  │   ┌────────────────────┐ │ │
│  💊 Create Prescription│  │   │ Name: John Doe     │ │ │
│  ➡️  Process Patient  │  │   │ Queue Count: 5     │ │ │
│                       │  │   └────────────────────┘ │ │
│  ─────────────────    │  │                          │ │
│  👤 View Profile      │  │   [Process Next Patient] │ │
│  🔑 Change Password   │  └──────────────────────────┘ │
│  🚪 Logout            │                                │
└───────────────────────┴────────────────────────────────┘
```

| Feature | Description |
|---|---|
| **Dashboard** | View current patient queue count and active patient |
| **Patient History** | Search prescription history by patient name |
| **Checkup Details** | Record blood pressure, sugar level, temperature, heart rate |
| **Create Prescription** | Write diagnosis, medication, dosage, and instructions |
| **Process Patient** | FIFO queue system — call in the next waiting patient |
| **View Profile** | See doctor name, email, and specialization |
| **Change Password** | Securely update login credentials |

---

### 👩‍💼 Receptionist Interface

```
┌────────────────────────────────────────────────────────┐
│  SIDEBAR              │  MAIN CONTENT AREA             │
│                       │                                │
│  🏠 Dashboard         │  ┌─────────────────────────┐  │
│  📁 Patient History   │  │ Search Patient Records  │  │
│  ➕ Add Record        │  │ ┌─────────────────────┐ │  │
│  📅 Appointment       │  │ │ Name | Date | Status│ │  │
│  💳 Payment           │  │ │ Ali  | 2026 | Paid  │ │  │
│                       │  │ └─────────────────────┘ │  │
│  ─────────────────    │  └─────────────────────────┘  │
│  👤 View Profile      │                                │
│  🔑 Change Password   │  ┌─────────────────────────┐  │
│  🚪 Logout            │  │   🧾 Receipt Generator  │  │
└───────────────────────┴──┴─────────────────────────┴──┘
```

| Feature | Description |
|---|---|
| **Patient History** | Search past visits, appointment dates, and payment status |
| **Add Record** | Register new patients (name, age, sex, contact) |
| **Appointment** | Book appointments by selecting specialty → doctor → date |
| **Payment** | Process and record patient payments |
| **Receipt** | Generate printable payment receipts |
| **View Profile** | See receptionist info |
| **Change Password** | Securely update credentials |

---

### 🔧 Admin Interface

```
┌────────────────────────────────────────────────────────┐
│  Admin Dashboard                                        │
│                                                         │
│  ┌─────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ 📅 Attend.  │  │ 👥 Employee  │  │ 💰 Payroll   │  │
│  │  Tracking   │  │  Details     │  │  Management  │  │
│  └─────────────┘  └──────────────┘  └──────────────┘  │
│                                                         │
│  ┌─────────────┐  ┌──────────────┐                     │
│  │ ⭐ Perform. │  │ 🕐 Shift     │                     │
│  │  Reviews    │  │  Scheduling  │                     │
│  └─────────────┘  └──────────────┘                     │
└─────────────────────────────────────────────────────────┘
```

| Feature | Description |
|---|---|
| **Attendance** | View and manage employee attendance records |
| **Employee Details** | Add and view employee information |
| **Payroll** | Manage salaries and process payroll |
| **Performance** | Review and track employee performance |
| **Shift Scheduling** | Assign and manage staff shifts |

---

## 📡 API Reference

> Base URL: `http://localhost:4567`

### 🟢 Receptionist Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/patientHistory?name={name}` | Get patient visit history by name |
| `GET` | `/allPatients` | Retrieve all registered patients |
| `GET` | `/searchPatients?q={query}` | Search patients by query |
| `GET` | `/specialties` | List all doctor specializations |
| `GET` | `/doctors?specialization={spec}` | Get doctors by specialization |
| `POST` | `/registerPatient` | Register a new patient |
| `POST` | `/registerAppointment` | Book a new appointment |
| `POST` | `/makePayment` | Process a patient payment |
| `POST` | `/receptionist/login` | Receptionist login |
| `GET` | `/receptionist/info?receptionist_id={id}` | Get receptionist profile |
| `POST` | `/receptionist/changePassword` | Update receptionist password |

**Register Patient — Request Body:**
```json
{
  "name": "Ali Hassan",
  "age": "32",
  "sex": "Male",
  "contact_info": "ali@email.com"
}
```

**Register Appointment — Request Body:**
```json
{
  "patient_id": "101",
  "doctor_id": "5",
  "appointment_date": "2026-06-15"
}
```

---

### 🔵 Doctor Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/doctor/login` | Doctor login |
| `GET` | `/doctor/info?doctor_id={id}` | Get doctor profile |
| `POST` | `/doctor/changePassword` | Update doctor password |
| `GET` | `/patientPrescriptionHistory?name={name}&doctorId={id}` | Patient prescription history |
| `GET` | `/prescriptions?patient_id={id}&doctor_id={id}` | Get prescriptions |
| `POST` | `/createPrescription` | Create a new prescription |
| `POST` | `/addCheckupDetails` | Add patient checkup vitals |
| `POST` | `/doctor/processNextPatient` | Process next patient in queue |
| `GET` | `/doctor/currentPatientName?doctorId={id}` | Get current queue patient |
| `GET` | `/doctor/queueCount?doctorId={id}` | Get waiting queue count |

**Create Prescription — Request Body:**
```json
{
  "doctor_id": 5,
  "patient_id": 101,
  "diagnosis": "Hypertension",
  "medication_details": "Amlodipine 5mg",
  "dosage": "Once daily",
  "instructions": "Take after meal"
}
```

**Add Checkup — Request Body:**
```json
{
  "patient_id": 101,
  "appointment_id": 202,
  "blood_pressure": "120/80",
  "sugar_level": "95 mg/dL",
  "temperature": "98.6°F",
  "heart_rate": "72 bpm"
}
```

---

## 🛠️ Tech Stack

```
┌─────────────────────────────────────────────────────┐
│                    TECH STACK                        │
│                                                     │
│  BACKEND                    FRONTEND                │
│  ─────────                  ────────                │
│  • Java 23                  • HTML5                 │
│  • SparkJava 2.9.4          • CSS3                  │
│  • Oracle JDBC (ojdbc17)    • Vanilla JavaScript    │
│  • org.json                 • Google Fonts          │
│  • SLF4J Simple             • Embedded MP4 bg       │
│                                                     │
│  BUILD & TEST               DATABASE                │
│  ────────────               ────────                │
│  • Maven 3.8.1              • Oracle DB 23c         │
│  • JUnit 4.11               • JDBC (Port 1521)      │
│  • Exec Maven Plugin        • Schema: orcl          │
└─────────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
CLINIC_MANAGEMENT_SYSTEM/
│
├── clinic-management-system/          ← Java Backend (Maven)
│   ├── pom.xml                        ← Maven dependencies
│   ├── project.java                   ← Standalone script
│   └── src/
│       └── main/java/project/dsa/
│           ├── BackendServer.java      ← REST API server (SparkJava)
│           ├── Doctor.java            ← Doctor business logic
│           ├── Receptionist.java      ← Receptionist business logic
│           └── clinicDB.java          ← Oracle DB connection
│
└── Clinic-UI/                         ← HTML/CSS/JS Frontend
    │
    ├── Portal_Login/
    │   └── Portal_Selection_Page/
    │       ├── Portal Page.html        ← Role selection (Doctor/Receptionist)
    │       ├── Doctor_Login/
    │       │   ├── Portal_Page_Doc.html
    │       │   ├── Forget password.html
    │       │   └── Reset password.html
    │       └── Receptionist_Login/
    │           ├── Portal_Page_Rec.html
    │           ├── Forget password.html
    │           └── Reset password.html
    │
    ├── Doctor_interface/
    │   ├── interface.html              ← Doctor dashboard
    │   └── Sidebar_rec/
    │       ├── Patient Priscription History.html
    │       ├── Checkup Details.html
    │       ├── Create Prescription.html
    │       ├── Payment.html
    │       └── Receipt/Receipt.html
    │
    ├── Receptionist_interface/
    │   ├── interface.html              ← Receptionist dashboard
    │   └── Sidebar_rec/
    │       ├── Patient History.html
    │       ├── Patient Record.html
    │       ├── Appointment.html
    │       ├── Payment.html
    │       └── Receipt/Receipt.html
    │
    └── Admin_interface/
        └── Sidebar/
            ├── Sidebar_attendance.html
            ├── Sidebar_employeedetails.html
            ├── Sidebar_payroll.html
            ├── Sidebar_performance.html
            └── Sidebar_shift.html
```

---

## 🚀 Getting Started

### Prerequisites

Make sure the following are installed on your system:

| Tool | Version | Download |
|---|---|---|
| Java JDK | 23+ | [oracle.com/java](https://www.oracle.com/java/technologies/downloads/) |
| Apache Maven | 3.8+ | [maven.apache.org](https://maven.apache.org/download.cgi) |
| Oracle Database | 21c / 23c | [oracle.com/database](https://www.oracle.com/database/) |

---

## 🗄️ Database Setup

1. Open Oracle SQL*Plus or SQL Developer and connect as `system`.

2. Create the required tables:

```sql
-- Patients Table
CREATE TABLE PATIENTS (
    patient_id   NUMBER PRIMARY KEY,
    name         VARCHAR2(100),
    age          NUMBER,
    sex          VARCHAR2(10),
    contact_info VARCHAR2(200)
);

-- Doctors Table
CREATE TABLE DOCTORS (
    doctor_id      NUMBER PRIMARY KEY,
    name           VARCHAR2(100),
    specialization VARCHAR2(100),
    contact_info   VARCHAR2(200)
);

-- Doctor Login Table
CREATE TABLE DOCTOR_LOGIN (
    login_id  NUMBER PRIMARY KEY,
    doctor_id NUMBER REFERENCES DOCTORS(doctor_id),
    username  VARCHAR2(50),
    password  VARCHAR2(100)
);

-- Receptionist Login Table
CREATE TABLE RECEPTIONIST_LOGIN (
    login_id        NUMBER PRIMARY KEY,
    receptionist_id NUMBER,
    username        VARCHAR2(50),
    password        VARCHAR2(100)
);

-- Appointments Table
CREATE TABLE APPOINTMENTS (
    appointment_id   NUMBER PRIMARY KEY,
    patient_id       NUMBER REFERENCES PATIENTS(patient_id),
    doctor_id        NUMBER REFERENCES DOCTORS(doctor_id),
    appointment_date TIMESTAMP
);

-- Payments Table
CREATE TABLE PAYMENTS (
    payment_id      NUMBER PRIMARY KEY,
    appointment_id  NUMBER REFERENCES APPOINTMENTS(appointment_id),
    amount          NUMBER(10,2),
    payment_status  VARCHAR2(20)
);
```

3. Update database credentials in `clinicDB.java` if needed:

```java
String dbURL  = "jdbc:oracle:thin:@localhost:1521:orcl";
String username = "system";
String password = "your_password";
```

---

## ▶️ Running the Application

### Step 1 — Start the Backend

```bash
# Navigate to the backend directory
cd clinic-management-system

# Install dependencies and compile
mvn clean install

# Start the REST API server (runs on port 4567)
mvn exec:java
```

You should see:
```
Connected to ClinicDB
[main] INFO spark.Spark - Listening on 0.0.0.0:4567
```

### Step 2 — Open the Frontend

Open the portal page in any modern browser:

```
Clinic-UI/Portal_Login/Portal_Selection_Page/Portal Page.html
```

> **Note:** The HTML files currently use absolute file paths referencing a local machine. Update the `href` values in each HTML file to match your local directory path before opening.

### Step 3 — Login

| Role | Credentials |
|---|---|
| Doctor | As configured in `DOCTOR_LOGIN` table |
| Receptionist | As configured in `RECEPTIONIST_LOGIN` table |

---

## 📊 Data Flow Examples

### Booking an Appointment

```
Receptionist UI
    │
    ├─── 1. GET /specialties              → Fetch all specializations
    ├─── 2. GET /doctors?specialization=X → Fetch doctors in specialty
    ├─── 3. GET /patients?query=Ali       → Find patient by name
    └─── 4. POST /registerAppointment     → Book the slot
```

### Doctor Seeing a Patient

```
Doctor UI
    │
    ├─── 1. GET  /doctor/queueCount       → How many patients waiting?
    ├─── 2. POST /doctor/processNextPatient → Pull next from queue
    ├─── 3. POST /addCheckupDetails       → Record vitals
    └─── 4. POST /createPrescription      → Write prescription
```

---

## 📝 Notes

- CORS is enabled for all origins (`*`) — restrict this in production.
- SQL queries currently use string concatenation; consider switching to **PreparedStatements** to prevent SQL injection.
- Passwords are stored in plain text — implement **hashing** (e.g., BCrypt) before deploying to production.
- The Admin interface HTML files are standalone (no API integration); backend endpoints for admin features can be added in `BackendServer.java`.

---
## License

Distributed under the MIT License. See the [LICENSE](LICENSE) file for details.

<div align="center">


</div>
