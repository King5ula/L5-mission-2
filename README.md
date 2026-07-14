# Turners Car Insurance Backend API Project 🚗💨

Welcome to the backend repository for the Turners Car Insurance Project. This project implements a series of RESTful APIs used during the insurance purchasing process. Built using the **MERN Stack** ecosystem (Node.js and Express), this repository strictly follows **Test-Driven Development (TDD)** practices using Jest and Supertest.

## 👥 Agile Team & Ownership
This project was built following Scrum/Agile frameworks:
* **Task Management:** Conducted via JIRA using *To-Do*, *In-Progress*, and *Done* workflows.
* **Daily Scrums:** 10-minute standups focused on achievements, plans, and technical blockers.
* **My Ownership:** I was personally responsible for designing, testing, and developing **API 2** and **API 3**.

---

## 🛠️ Tech Stack & Tools
* **Runtime Environment:** Node.js
* **Backend Framework:** Express.js
* **Testing Framework:** Jest
* **Integration Testing:** Supertest

---

## 🚀 API Specifications & Business Rules (My Deliverables)

### API 2: Convert "Claim History" to a "Risk Rating" (`POST /api/risk-rating`)
Transforms text-based driver claim histories over the last 3 years into a normalized 1-to-5 safety rating.
* **Input (JSON):** `{ "claim_history": "My only claim was a crash into my house's garage door that left a scratch on my car. There are no other crashes." }`
* **Output (JSON):** `{ "risk_rating": 3 }`
* **Business Rule:** Scans text to count keyword occurrences (`collide`, `crash`, `scratch`, `bump`, `smash`). Each occurrence adds 1 to the rating. The baseline floor rating is 1 (lowest risk) and caps at a ceiling of 5 (highest risk). If input value is not valid, return an error.

### API 3: Convert "Car Value" and "Risk Rating" to a "Quote" (`POST /api/quote`)
Calculates the premium quote based on the car's market value and the driver's risk rating.
* **Input (JSON):** `{ "car_value": 6614, "risk_rating": 3 }`
* **Output (JSON):** `{ "monthly_premium": 16.54, "yearly_premium": 198.42 }`
* **Business Rule:** 
  * Yearly premium = `(car_value * risk_rating) / 100`
  * Monthly premium = `Yearly premium / 12`
  * If input values are missing, negative, or invalid data types, return an error.

---

## 📋 Comprehensive Test Cases (TDD Blueprint)

### API 2 (Risk Rating)

| Test Case # | Input (`claim_history`) | Expected Output | Scenario |
| :--- | :--- | :--- | :--- |
| 1 | `"I had a crash and a scratch."` | `{ "risk_rating": 2 }` | Standard matching keywords |
| 2 | `"No accidents at all."` | `{ "risk_rating": 1 }` | Floor limit checking (Defaults to 1) |
| 3 | `"crash crash crash crash crash crash"` | `{ "risk_rating": 5 }` | Ceiling limit checking (Caps at 5) |
| 4 | `""` | `{ "error": "History required" }` | Missing required input verification |
| 5 | `12345` | `{ "error": "Invalid data type" }` | Number data type injection protection |

### API 3 (Quote Calculator)

| Test Case # | Input (`car_value`, `risk_rating`) | Expected Output | Scenario |
| :--- | :--- | :--- | :--- |
| 1 | `6614`, `3` | `{ "monthly_premium": 16.54, "yearly_premium": 198.42 }` | Sunny day (Standard premium calculation) |
| 2 | `-5000`, `3` | `{ "error": "Invalid car value" }` | Negative car value edge case |
| 3 | `6614`, `6` | `{ "error": "Invalid risk rating" }` | Risk rating exceeding limit boundary |
| 4 | `"6614"`, `3` | `{ "error": "Invalid data type" }` | String passed instead of integer numbers |
| 5 | `6614`, `null` | `{ "error": "Parameters required" }` | Missing parameters validation |

---

## 💻 Getting Started & Installation

Follow these steps to run the application and execute tests on your local machine:

### 1. Clone the repository
```bash
git clone https://github.com/King5ula/L5-mission-2.git
```

### 2. Install dependencies
```bash
npm install
```

### 3. Run automated tests (Jest)
Because we use **TDD**, run the test suites to ensure everything satisfies code compliance rules:
```bash
npm test
```

### 4. Start the API local server
```bash
npm start
```
The server will boot up natively at `http://localhost:3000`.

---

## 🔄 Git Workflow & Code Quality Review
To maintain clean code standards:
1. All changes must be developed on isolated local branches: `git checkout -b feature/api2-api3-logic`.
2. Push changes up and open a **Pull Request (PR)** targeting the `main` branch.
3. At least one team member must review the PR, addressing code smells (such as redundancy, lack of input filtering, or unhandled promise rejections) before merging.
