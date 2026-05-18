```markdown
# 🦠 COVID-19 Case Reporting System

A comprehensive MuleSoft API integration solution for managing COVID-19 case reporting from local Area Health Offices (AHO) to the World Health Organization (WHO). Built with MuleSoft Anypoint Platform, integrated with MySQL database, and deployed on CloudHub.

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Database Setup](#database-setup)
- [API Endpoints](#api-endpoints)
- [Testing with Postman](#testing-with-postman)
- [Deployment](#deployment)
- [Project Structure](#project-structure)
- [Author](#author)

## 🎯 Overview

This project implements an end-to-end COVID-19 case management system where:
- **Clients** register COVID positive cases with the AHO
- **AHO** reports cases to the **WHO** for global monitoring
- **Vaccination records** are tracked and linked to cases
- **Automatic status management** (POSITIVE → RECOVERED/DEATH/INVALID)
- **Country-wise analytics** for health ministers

## 🏗️ Architecture

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│   CLIENT    │────▶│  MULESOFT    │────▶│   MYSQL     │
│  (Postman)  │     │     API      │     │  DATABASE   │
└─────────────┘     └──────┬───────┘     └─────────────┘
                           │
                    ┌──────▼───────┐
                    │  WHO REPORTS │
                    └──────────────┘
```

## ✨ Features

### Functional Requirements
- ✅ Register COVID positive cases
- ✅ Update case status (RECOVERED/DEATH)
- ✅ Register vaccinated cases
- ✅ Auto-update existing records based on status
- ✅ Automatic INVALID status after 20 days (Scheduler)
- ✅ Maintain statuses: POSITIVE, RECOVERED, DEATH, INVALID
- ✅ Fetch post-vaccination positive cases for monitoring
- ✅ Country-wise case reports

### Non-Functional Requirements
- ✅ Survives Mule runtime restarts
- ✅ High availability configuration
- ✅ Encrypted sensitive data
- ✅ CORS enabled
- ✅ OAuth 2.0 security ready
- ✅ Rate limiting support

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| **Integration Platform** | MuleSoft Anypoint Platform |
| **Runtime** | Mule Runtime 4.4.0 |
| **API Design** | RAML 1.0 |
| **IDE** | Anypoint Studio 7.x |
| **Database** | MySQL 8.0 (freesqldatabase.com) |
| **DB Connector** | Mule Database Connector v1.14.10 |
| **Deployment** | CloudHub |
| **Testing** | Postman |
| **Version Control** | Git / GitHub |
| **Java Version** | Java 11 / 17 |

## 📦 Prerequisites

Before you begin, ensure you have:
- ✅ [Anypoint Studio 7.x](https://www.mulesoft.com/lp/dl/studio) installed
- ✅ [Anypoint Platform Account](https://anypoint.mulesoft.com/login/signup) (Free Trial)
- ✅ [Postman](https://www.postman.com/downloads/) for API testing
- ✅ MySQL Database (Online: [freesqldatabase.com](https://www.freesqldatabase.com/))
- ✅ Java 11 or 17 installed
- ✅ Git installed

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/covid-reporting-system.git
cd covid-reporting-system
```

### 2. Import into Anypoint Studio
1. Open **Anypoint Studio**
2. Navigate to **File → Import → Anypoint Studio → Anypoint Studio Project from File System**
3. Browse and select the project folder
4. Click **Finish**

### 3. Update Database Credentials
Open `src/main/mule/covid-api.xml` and update:
```xml
<db:my-sql-connection 
    host="your-db-host" 
    port="3306" 
    user="your-username" 
    password="your-password" 
    database="your-database"/>
```

### 4. Update Maven Dependencies
Right-click project → **Maven → Update Project** → ✅ Force Update → OK

### 5. Run the Project
Right-click `covid-api.xml` → **Run As → Mule Application**

## 🗄️ Database Setup

### Create Tables in MySQL:

```sql
CREATE TABLE covid_cases (
    id INT AUTO_INCREMENT PRIMARY KEY,
    case_id VARCHAR(50),
    patient_name VARCHAR(100),
    country VARCHAR(50),
    age INT,
    status VARCHAR(20),
    registered_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE vaccinations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    vaccine_id VARCHAR(50),
    patient_id VARCHAR(50),
    vaccine_name VARCHAR(100),
    dose_number INT,
    vaccinated_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE who_reports (
    id INT AUTO_INCREMENT PRIMARY KEY,
    case_id VARCHAR(50),
    country VARCHAR(50),
    status VARCHAR(20),
    reported_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 📡 API Endpoints

### Base URL
```
Local: http://localhost:8081/api
CloudHub: https://covid-api-yourname.us-e2.cloudhub.io/api
```

### Available Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/cases` | Register a new COVID case |
| `GET` | `/cases` | Get all COVID cases |
| `GET` | `/cases/{caseId}` | Get specific case by ID |
| `PUT` | `/cases/{caseId}` | Update case status |
| `POST` | `/vaccinations` | Register vaccination |
| `GET` | `/vaccinations` | Get all vaccinations |
| `GET` | `/monitoring/post-vaccination` | Get positive cases after vaccination |
| `GET` | `/monitoring/country-report?country={country}` | Get country-wise report |

## 🧪 Testing with Postman

### 1️⃣ Register a COVID Case
```http
POST http://localhost:8081/api/cases
Content-Type: application/json

{
  "caseId": "C1001",
  "patientName": "John Doe",
  "country": "India",
  "age": 35,
  "status": "POSITIVE"
}
```

**Response:**
```json
{
  "status": "SUCCESS",
  "message": "Case registered and reported to WHO",
  "caseId": "C1001"
}
```

### 2️⃣ Get All Cases
```http
GET http://localhost:8081/api/cases
```

### 3️⃣ Update Case Status
```http
PUT http://localhost:8081/api/cases/C1001
Content-Type: application/json

{
  "status": "RECOVERED"
}
```

### 4️⃣ Register Vaccination
```http
POST http://localhost:8081/api/vaccinations
Content-Type: application/json

{
  "vaccineId": "V1001",
  "patientId": "C1001",
  "vaccineName": "Covishield",
  "doseNumber": 1
}
```

### 5️⃣ Country Report
```http
GET http://localhost:8081/api/monitoring/country-report?country=India
```

**Response:**
```json
{
  "country": "India",
  "totalCases": 5,
  "positiveCases": 2,
  "recoveredCases": 2,
  "deathCases": 1,
  "invalidCases": 0
}
```

## ☁️ Deployment to CloudHub

### Steps:
1. Right-click project → **Anypoint Platform → Deploy to CloudHub**
2. Sign in with Anypoint credentials
3. Configure:
   - **Application Name:** `covid-api-yourname`
   - **Runtime:** 4.4.0
   - **Workers:** 1-2 (for High Availability)
   - **Worker Size:** 0.1 vCore
   - **Region:** US East
4. Click **Deploy Application**
5. Wait 3-5 minutes for status: **Started** ✅

### Apply Security Policies (Optional):
- **API Manager → Policies:**
  - ✅ OAuth 2.0 Access Token Enforcement
  - ✅ CORS
  - ✅ Rate Limiting (100 req/min)
  - ✅ Client ID Enforcement

## 📁 Project Structure

```
covid-reporting-system/
├── src/
│   └── main/
│       ├── mule/
│       │   └── covid-api.xml          # Main Mule flows
│       ├── resources/
│       │   ├── api/
│       │   │   └── covid.raml          # API specification
│       │   └── log4j2.xml              # Logging config
│       └── java/                       # Java utilities
├── pom.xml                             # Maven dependencies
├── mule-artifact.json                  # Mule artifact config
├── README.md                           # Project documentation
└── .gitignore                          # Git ignore rules
```

## 🎓 Business Logic Highlights

### Case Registration Logic:
1. If case exists AND status ≠ RECOVERED → **UPDATE** existing record
2. If case exists AND status = RECOVERED → **INSERT** new record (new infection)
3. If case doesn't exist → **INSERT** new record
4. Always **report to WHO** after registration

### Auto-INVALID Scheduler:
- Runs **daily**
- Marks cases as **INVALID** if status is POSITIVE and 20+ days old

## 📊 Status Flow

```
   POSITIVE ──┬──▶ RECOVERED
              ├──▶ DEATH
              └──▶ INVALID (auto after 20 days)
```

## 🔐 Security Features

- 🔒 Database credentials externalized
- 🔒 OAuth 2.0 ready
- 🔒 CORS enabled
- 🔒 Rate limiting capable
- 🔒 Encrypted logging support

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Java version error | Use Java 11 or update DB connector to v1.14.10+ |
| DB connection fails | Verify credentials and check firewall |
| Build fails | Run `Maven → Update Project → Force Update` |
| Port 8081 in use | Change port in HTTP Listener config |

## 📝 Future Enhancements

- [ ] Add Process Layer API
- [ ] Add System Layer APIs (separate AHO, WHO, Vaccination)
- [ ] Implement Object Store for caching
- [ ] Add JMS/Kafka for async messaging
- [ ] Build frontend dashboard
- [ ] Add ML-based outbreak prediction
- [ ] Multi-language support

## 👤 Author

**Your Name**
- GitHub: kamlikarjashwanth_2001
- LinkedIn: [Your LinkedIn](https://linkedin.com/in/kamlikar-jashwanth)
- Email: your.email@example.com

## 🙏 Acknowledgments

- MuleSoft Anypoint Platform
- WHO COVID-19 reporting guidelines
- freesqldatabase.com for free MySQL hosting

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

⭐ **If you found this project helpful, please give it a star!** ⭐

**Built with ❤️ using MuleSoft**
```
