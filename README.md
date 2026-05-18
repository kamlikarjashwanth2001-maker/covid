# 📄 Clean & Simple README.md

**Copy this and paste into GitHub:**

---

```markdown
# COVID-19 Case Reporting System

A MuleSoft API integration project for managing COVID-19 cases, vaccinations, and WHO reporting using MySQL database.

## 📌 About

This project implements a COVID-19 case management system where:
- Clients register COVID positive cases
- AHO (Area Health Office) reports to WHO
- Vaccinations are tracked
- Country-wise reports are generated
- Breakthrough infections are monitored

## 🛠️ Tech Stack

- **MuleSoft Anypoint Studio** 7.x
- **Mule Runtime** 4.4.0
- **RAML** 1.0 for API design
- **MySQL** 8.0 (hosted on freesqldatabase.com)
- **Java** 11/17
- **Postman** for testing

## 📊 Database Tables

| Table | Purpose |
|-------|---------|
| `covid_cases` | Stores COVID patient records |
| `vaccinations` | Stores vaccination details |
| `who_reports` | Auto-generated WHO reports |

## 🚀 How to Run

1. **Clone the repo**
   ```bash
   git clone https://github.com/kamlikarjashwanth2001-maker/covid-reporting-system.git
   ```

2. **Import into Anypoint Studio**
   - File → Import → Anypoint Studio Project

3. **Update Maven**
   - Right-click project → Maven → Update Project → Force Update

4. **Run the application**
   - Right-click `covid-api.xml` → Run As → Mule Application

5. **API is live at:** `http://localhost:8081/api`

## 📡 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/cases` | Register a COVID case |
| GET | `/api/cases` | Get all cases |
| GET | `/api/cases/{caseId}` | Get specific case |
| PUT | `/api/cases/{caseId}` | Update case status |
| POST | `/api/vaccinations` | Register vaccination |
| GET | `/api/vaccinations` | Get all vaccinations |
| GET | `/api/monitoring/post-vaccination` | Breakthrough infections |
| GET | `/api/monitoring/country-report?country={country}` | Country report |

## 🧪 Sample Requests

### Register a Case
```http
POST http://localhost:8081/api/cases
Content-Type: application/json

{
  "caseId": "IND001",
  "patientName": "Rahul Sharma",
  "country": "India",
  "age": 32,
  "status": "POSITIVE"
}
```

### Register Vaccination
```http
POST http://localhost:8081/api/vaccinations
Content-Type: application/json

{
  "vaccineId": "VAC001",
  "patientId": "IND001",
  "vaccineName": "Covishield",
  "doseNumber": 1
}
```

### Country Report
```http
GET http://localhost:8081/api/monitoring/country-report?country=India
```

## ✨ Key Features

- ✅ Full CRUD operations for COVID cases
- ✅ Vaccination tracking with multiple doses
- ✅ Smart status management (POSITIVE → RECOVERED/DEATH)
- ✅ Auto-INVALID after 20 days (Scheduler)
- ✅ Breakthrough infection detection
- ✅ Country-wise analytics
- ✅ Automatic WHO reporting

## 👤 Author

**Kamlikar Jashwanth**

- GitHub: [@kamlikarjashwanth2001-maker](https://github.com/kamlikarjashwanth2001-maker)
- LinkedIn: [Kamlikar Jashwanth](https://www.linkedin.com/in/kamlikar-jashwanth)
- Email: kamlikarjashwanth2001@gmail.com

## 📄 License

This project is open-source and available for educational purposes.

---

⭐ If you found this helpful, give it a star!
```

---


---

**Clean, simple, and professional!** 🎯
