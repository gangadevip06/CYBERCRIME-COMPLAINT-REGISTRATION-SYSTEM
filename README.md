# 🛡️ Cybercrime Complaint Registration System

A secure web-based **Cybercrime Complaint Registration System** built with **Java and Spring Boot** that enables citizens to register cybercrime complaints, upload digital evidence, track complaint status, and allows administrators and police officers to manage and investigate complaints.

The system follows a **role-based architecture** with separate modules for **Citizens, Admins, and Police Officers**, while using file-based storage for user and complaint information and Excel-based storage for police officer records.

---

## 🚀 Features

### 👤 Citizen Module

* User registration and login
* Secure user authentication
* User profile
* Register cybercrime complaints
* Automatic complaint ID generation
* Upload supporting evidence
* View registered complaints
* Track complaint status using Complaint ID
* View complaint history
* User details stored in files

### 🔐 Admin Module

* Secure admin authentication
* Add police officers
* Delete police officers
* View police officer details
* View all complaints
* Search complaints by Complaint ID
* Assign complaints to police officers
* View uploaded evidence
* Filter complaints based on a custom number of days
* Supports complaint filtering from **1–30 days**
* Manage complaint status
* Monitor complaint investigation

### 👮 Police Officer Module

* Secure officer login
* View assigned complaints
* Search complaints using Complaint ID
* View complaint details
* View user-uploaded evidence
* Filter complaints by **1–30 days**
* Update complaint status
* Handle assigned investigations

---

## 🔄 System Workflow

```text
                         ┌───────────────────┐
                         │      LOGIN        │
                         └─────────┬─────────┘
                                   │
             ┌─────────────────────┼─────────────────────┐
             │                     │                     │
             ▼                     ▼                     ▼
       ┌───────────┐         ┌───────────┐         ┌────────────┐
       │  CITIZEN  │         │   ADMIN   │         │  OFFICER   │
       └─────┬─────┘         └─────┬─────┘         └──────┬─────┘
             │                     │                      │
             ▼                     ▼                      ▼
       Register Complaint    Manage Officers       Assigned Cases
             │                     │                      │
             ▼                     ▼                      ▼
       Upload Evidence       View Complaints       Search Complaint
             │                     │                      │
             ▼                     ▼                      ▼
       Generate Complaint ID Assign Complaint       View Evidence
             │                     │                      │
             ▼                     └──────────┬───────────┘
       Track Complaint                       ▼
             │                         Update Status
             └─────────────────────────────┘
```

---

## 🏗️ Architecture

The application follows a layered Spring Boot architecture:

```text
┌─────────────────────────────┐
│          Frontend           │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│       REST Controller       │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│          Service            │
│       Business Logic        │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│      File Repository        │
│      / Data Storage         │
└──────────────┬──────────────┘
               │
       ┌───────┼────────┐
       ▼       ▼        ▼
    Users  Complaints  Evidence
               │
               ▼
        Police Officers
          (Excel File)
```

---

## 🛠️ Technology Stack

| Technology              | Purpose                         |
| ----------------------- | ------------------------------- |
| **Java**                | Core programming language       |
| **Spring Boot**         | Backend web framework           |
| **Spring Web**          | REST API development            |
| **Maven**               | Build and dependency management |
| **HTML/CSS/JavaScript** | Frontend                        |
| **File I/O**            | File-based data storage         |
| **Apache POI**          | Excel file handling             |
| **BCrypt**              | Password hashing                |
| **REST API**            | Frontend-backend communication  |

---

## 📂 Data Storage

The project uses file-based persistence instead of a traditional database.

```text
data/
│
├── users/
│   └── users.json
│
├── complaints/
│   └── complaints.json
│
├── officers/
│   └── officers.xlsx
│
└── evidence/
    ├── CMP10001/
    ├── CMP10002/
    └── CMP10003/
```

### User Data

User registration details are stored in files.

### Complaint Data

Complaint information is stored separately and associated with the registered user.

### Police Officer Data

Police officer details are maintained in an `.xlsx` Excel file.

### Evidence

Evidence uploaded by citizens is stored and associated with the corresponding Complaint ID.

Authorized Admins and Police Officers can access the uploaded evidence.

---

## 🔎 Complaint Management

Every registered complaint receives a unique Complaint ID.

Example:

```text
Complaint ID: CMP10001
```

The Complaint ID can be used to:

* Search a complaint
* Track complaint status
* Identify uploaded evidence
* Assign complaints to officers

### Complaint Status

```text
OPEN
  ↓
ASSIGNED
  ↓
UNDER INVESTIGATION
  ↓
RESOLVED
  ↓
CLOSED
```

---

## 📅 Dynamic Complaint Filtering

Instead of fixed **5-day** or **30-day** filters, administrators and police officers can enter any value from **1 to 30 days**.

Example:

```text
Enter number of days: 10
```

The system retrieves complaints registered within the selected period.

### Validation

```text
Minimum: 1 day
Maximum: 30 days
```

Invalid input is rejected with an appropriate error message.

---

## 📎 Evidence Management

Citizens can upload supporting evidence while registering a complaint.

The evidence is linked to the Complaint ID.

Example:

```text
CMP10001/
├── screenshot.png
├── transaction.pdf
└── email.txt
```

The uploaded evidence can be accessed by:

* 🔐 Admin
* 👮 Assigned Police Officer

This allows authorized personnel to review supporting evidence during complaint investigation.

---

## 🔐 Security

The application implements role-based access for:

```text
Citizen
Admin
Police Officer
```

### Security Features

* Separate login for each role
* Admin-only police officer management
* Police officers cannot self-register
* Password hashing using BCrypt
* Authentication before protected operations
* Authorization based on user role
* Evidence access restricted to authorized users
* Input validation
* Exception handling
* No hardcoded sample complaints

---

## 👥 Role Permissions

| Feature                 | Citizen | Admin | Officer |
| ----------------------- | :-----: | :---: | :-----: |
| Register Account        |    ✅    |   ❌   |    ❌    |
| Login                   |    ✅    |   ✅   |    ✅    |
| Register Complaint      |    ✅    |   ❌   |    ❌    |
| Upload Evidence         |    ✅    |   ❌   |    ❌    |
| Track Complaint         |    ✅    |   ❌   |    ❌    |
| View All Complaints     |    ❌    |   ✅   |    ✅    |
| Search Complaint        |    ❌    |   ✅   |    ✅    |
| Add Officer             |    ❌    |   ✅   |    ❌    |
| Delete Officer          |    ❌    |   ✅   |    ❌    |
| Assign Complaint        |    ❌    |   ✅   |    ❌    |
| View Evidence           |    ❌    |   ✅   |    ✅    |
| Update Complaint Status |    ❌    |   ✅   |    ✅    |
| Date-Based Filtering    |    ❌    |   ✅   |    ✅    |

---

## 📁 Project Structure

```text
cybercrime-complaint-system/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── cybercrime/
│   │   │           ├── controller/
│   │   │           ├── service/
│   │   │           ├── model/
│   │   │           ├── repository/
│   │   │           ├── exception/
│   │   │           ├── security/
│   │   │           └── util/
│   │   │
│   │   └── resources/
│   │       ├── static/
│   │       ├── templates/
│   │       └── application.properties
│   │
│   └── test/
│
├── data/
│   ├── users/
│   ├── complaints/
│   ├── officers/
│   └── evidence/
│
├── pom.xml
└── README.md
```

---

## 🧠 Java Concepts Demonstrated

This project demonstrates several core and advanced Java concepts.

### Object-Oriented Programming

* Encapsulation
* Inheritance
* Polymorphism
* Abstraction

### String Handling

Used for:

* Complaint ID processing
* User input validation
* Searching
* Data formatting

### I/O Streams

Used for:

* Reading user data
* Writing complaint data
* Managing evidence files
* File-based persistence

### Exception Handling

Custom and built-in exceptions are used for:

* Invalid login
* Invalid Complaint ID
* Missing complaint
* Invalid date range
* Unauthorized access
* File errors

### Collections

Collections such as:

```java
List
Map
Set
```

are used for managing users, complaints, officers, and related data.

### Multithreading

Multithreading can be used for background operations such as:

* File processing
* Logging
* Complaint monitoring
* Background tasks

---

## 🌐 REST API

The backend provides REST endpoints for communication between the frontend and Spring Boot application.

Example endpoints:

```text
POST   /api/users/register
POST   /api/users/login

POST   /api/complaints
GET    /api/complaints/{id}

POST   /api/officers
DELETE /api/officers/{id}

GET    /api/admin/complaints
GET    /api/officer/complaints
```

The exact endpoints may vary according to the implementation.

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

### 2. Navigate to the Project

```bash
cd cybercrime-complaint-system
```

### 3. Check Java

```bash
java -version
```

Java 17 or later is recommended.

### 4. Check Maven

```bash
mvn -version
```

### 5. Build the Project

```bash
mvn clean install
```

### 6. Run the Application

```bash
mvn spring-boot:run
```

Alternatively, run the main Spring Boot application class from your IDE.

---

## 🧪 Application Testing

### Citizen Test

```text
1. Create a citizen account
2. Login
3. Open User Dashboard
4. Register a complaint
5. Upload evidence
6. Submit complaint
7. Copy generated Complaint ID
8. Track complaint status
```

### Admin Test

```text
1. Login as Admin
2. Add Police Officer
3. View complaints
4. Search Complaint ID
5. Enter number of days (1–30)
6. View evidence
7. Assign complaint to officer
```

### Police Officer Test

```text
1. Login using officer credentials
2. View assigned complaints
3. Search Complaint ID
4. Open complaint
5. View evidence
6. Update complaint status
```

---

## 🎯 Project Objectives

* Provide a centralized platform for cybercrime complaint registration.
* Allow citizens to submit complaints digitally.
* Provide secure evidence upload and access.
* Enable complaint tracking using a unique Complaint ID.
* Allow administrators to manage police officers.
* Assign complaints to appropriate police officers.
* Enable officers to investigate assigned complaints.
* Provide flexible 1–30 day complaint filtering.
* Demonstrate Java and Spring Boot concepts in a real-world application.
* Maintain application data using file-based storage.

---

## 📚 Course Outcomes Covered

| CO      | Concepts                                          |
| ------- | ------------------------------------------------- |
| **CO1** | Object-Oriented Programming                       |
| **CO2** | String Handling, I/O Streams & Exception Handling |
| **CO3** | Multithreading & Collections                      |
| **CO4** | Maven, REST API & Project Management              |
| **CO5** | Spring Boot Enterprise/Web Applications           |

---

## 🔮 Future Enhancements

* JWT-based authentication
* Email notifications
* SMS notifications
* PDF complaint reports
* Advanced audit logging
* Complaint analytics dashboard
* Digital evidence integrity verification
* AI-based complaint classification
* Database integration
* Cloud-based evidence storage
* Real-time investigation notifications

---

## 👩‍💻 Project Information

**Project:** Cybercrime Complaint Registration System
**Domain:** Cybersecurity / Web Application
**Backend:** Java + Spring Boot
**Build Tool:** Maven
**Storage:** File-Based + Excel
**Architecture:** Layered Architecture
**Application Type:** Web Application / REST API

---

## ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.

---

## 📄 License

This project is developed for **educational and academic purposes**.

