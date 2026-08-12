# Enterprise HR Onboarding & SLA Control Tower

An end-to-end automated HR onboarding pipeline built using **Make.com**, **Webhooks**, **Google Sheets**, and **Gmail API**. This system automates candidate record processing, monitors SLA compliance in real time, and dispatches automated candidate communications without manual data entry.

---

## 📸 System Architecture & Overview
[ Incoming Webhook (JSON) ]
│
▼
[ Google Sheets Control Tower ] ──► (SLA Calculations & Process Health)
│
▼
[ Gmail API Dispatcher ] ─────────► (Automated Candidate Onboarding Email)


---

## 🚀 Features & Capabilities

* **Automated Data Ingestion:** Captures incoming candidate payloads dynamically via a custom webhook endpoint.
* **Master Database Integration:** Automatically maps candidate metadata (`Name`, `Department`, `Start Date`, `Email`) to the `Onboarding_Database` tracker.
* **Automated SLA & Health Tracking:** Integrates with sheet formulas (`WORKDAY`, `NETWORKDAYS`) to calculate IT provisioning timelines and flag overall process health (🟢 On Track, 🟡 At Risk, 🔴 Critical).
* **Conditional Communication Dispatch:** Triggers welcome emails via Gmail API based on record criteria.

---

## 🛠️ Tech Stack & Protocols

* **IPaaS / Automation Engine:** Make.com (Scenario ID: `5926094`)
* **Data Transport:** JSON / REST API / Webhooks
* **Database & Analytics:** Google Sheets (`Onboarding_Master_Tracker.xlsx`)
* **Communication Service:** Gmail API (OAuth 2.0 Scope Authentication)

---

## 📄 Sample JSON Payload

Below is the structured JSON format passed to the Make.com webhook:

```json
{
  "records": [
    {
      "EmployeeID": "EMP-2026-001",
      "Name": "Sarah Jenkins",
      "Department": "Engineering",
      "StartDate": "2026-03-01",
      "ITTargetDate": "2026-02-28",
      "ITActualDate": "2026-02-27",
      "ITSlaDelay": 0,
      "ITStatus": "Completed",
      "ReviewTargetDate": "2026-04-01",
      "ReviewActualDate": "2026-03-25",
      "ReviewStatus": "Completed",
      "TimeToProductivity": 24,
      "ProcessHealthStatus": "On Track",
      "VisualSlaFlag": "🟢",
      "ManagerEmail": "manager.eng@company.com",
      "Email": "sarah.jenkins@example.com",
      "SendWelcomeEmail": true
    }
  ]
}
```

## 🔧 Setup & Deployment Instructions
* Database Preparation: Upload the Onboarding_Master_Tracker.xlsx template to Google Drive and open as a Google Sheet.

* Make.com Configuration: https://us2.make.com/public/shared-scenario/rbJrGhnleb7/integration-webhooks-google-sheets-gma

Create a scenario with a Custom Webhook module.

   * Add a Google Sheets (Add a Row) module connected to Onboarding_Database.

   * Add a Gmail (Send an Email) module with re-authenticated OAuth 2.0 scopes.

   * Trigger Execution: Activate the scenario toggle to listen for live incoming requests.
