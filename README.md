---
# Domain Aware Incident Integration (ServiceNow Scoped App)

## Overview

This project demonstrates how to build a **domain-aware integration** in ServiceNow.
It accepts incidents from external systems via a **Scripted REST API**, then routes them to the correct **domain** based on the authenticated user.

This is useful for **multi-tenant ServiceNow implementations** (e.g., Managed Service Providers), where each customer must have isolated data using **Domain Separation**.
---

## Features

- Scoped application (`x_yourname_domain_integration`)
- **Scripted REST API** (`POST /incident`)
- Auto-assigns incidents to correct `sys_domain` based on the **user** executing the webservice
- Tested with **Postman collection** for easy simulation
- Works with **Domain Separation plugin** enabled

---

## Architecture

**External System** (Postman / API client) → **ServiceNow Scripted REST API** → **Look up User** → **Incident (sys_domain assigned)**

---

## Setup Instructions

### 1. Install Scoped App

1. Clone repo or import update set into your PDI. This is the repo to clone: https://github.com/anurampalli/Domain-Aware-Incident-Integration.git
2. Ensure **Domain Support – Domain Extensions** plugin is active.

### 2. Configure Domain Mapping

- Add records in core_compqny table (CompanyA, CompanyB).
- Add records in domain table

  - CompanyA → DomainA
  - CompanyB → DomainB

### 3. Use REST API

Endpoint:

```
POST https://<your-instance>.service-now.com/api/x_1778869_domain_0/remote_incidents/create
```

Headers:

```http
Content-Type: application/json
Accept: application/json
```

Authentication:

- OAuth 2.0

---

## Example Requests

### Create Incident for CompanyA

```json
{
  "short_description": "Server CPU usage high"
}
```

### Create Incident for CompanyB

```json
{
 "short_description": "Email outage in region"
}
```

---

## Example Response

```json
{
    "result": {
        "status": "success",
        "incident_number": "INC0012271",
        "company": "CompanyB",
        "domain": "TOP/DomainB"
    }
}
```

---

## Screenshots

- Domain Table  
  ![Domain Table](screenshots/domain.png)
- Postman Request  
  ![Postman Request](postman_collection/updated_postman_request_and_response.png)
- ACL  
  ![ACL](acl/Scoped_Integration_Users_1.png)
- ACL  
  ![ACL](acl/Scoped_Integration_Users_2.png)
- Create Users
  ![Domain Specific Users](screenshots/create_users_for_each_domain.png)
- Domain Separation Incident Table
  ![Incident Table](screenshots/user_based_domain_seperation.png)
- **I did it! Domain Seperation, Integration, with OAuth too!**
  ![I did it!](i_did_it.jpg)
---

## Learning Outcomes

- How to integrate ServiceNow with external systems using REST APIs
- How to enforce **domain separation** in integrations
- How to simulate **multi-tenant incident management** in a single instance based on authenticated user
- How to package ServiceNow work into a **scoped app** for reusability

---

## Next Steps

- Extend app to support **Problem/Change** records
- Create **ATF (Automated Test Framework)** tests for regression

---
