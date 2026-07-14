# NPHIES-Clinical-Data-Mapping
A practical clinical-to-FHIR data mapping project demonstrating NPHIES compliance, ICD-10-AM coding, and RCM optimization in Saudi Arabia.

# 🏥 NPHIES Clinical Data Mapping: Outpatient Encounter

![NPHIES](https://img.shields.io/badge/Platform-NPHIES-green)
![Standard](https://img.shields.io/badge/Standard-HL7_FHIR_R4-blue)
![Coding](https://img.shields.io/badge/Coding-ICD--10--AM-orange)

## 📌 Project Overview
This repository demonstrates a practical, hands-on approach to mapping clinical documentation to the **NPHIES (National Platform for Health and Insurance Exchange Services)** standards in Saudi Arabia. 

The primary goal of this project is to showcase how accurate clinical-to-technical data mapping bridges the gap between medical practitioners and billing systems. By adhering to **HL7 FHIR** standards and **ICD-10-AM** coding, this mapping prevents automated medical coding rejections, ensures interoperability, and optimizes the Revenue Cycle Management (RCM) process.

---

## 🩺 1. The Clinical Scenario (Mock Case)
To provide a realistic context, this mapping is based on a common outpatient scenario that frequently faces billing scrutiny if not documented and mapped correctly:

* **Encounter Type:** Outpatient / Ambulatory
* **Patient Presentation:** A 45-year-old male presenting with acute lower back pain radiating to the right leg.
* **Clinical Diagnosis:** Lumbar Disc Herniation with radiculopathy.
    * *Standard Code (ICD-10-AM):* **M51.2**
* **Procedure/Service Provided:** Clinical assessment and advanced manual therapy.
    * *Mock Service Code:* **97140**

---

## 🏗️ 2. Clinical-to-FHIR Data Mapping Architecture
Translating raw clinical narrative into structured FHIR resources required by the NPHIES platform logic.

| Clinical Element | FHIR Resource | Standard System | Value & Business Logic |
| :--- | :--- | :--- | :--- |
| **Patient ID** | `Patient` | National ID / Iqama | Passed as a unique `identifier` to ensure real-time insurance eligibility verification. |
| **Encounter Type** | `Encounter` | `v3-ActCode` | Value: **AMB** (Ambulatory). Directs the pricing engine to outpatient logic. |
| **Diagnosis** | `Condition` | `ICD-10-AM` | Code: **M51.2**. `clinicalStatus` is strictly set to **Active** to justify the medical procedure. |
| **Procedure/Service** | `Claim.item` | `ACHI` / `CPT` | Code: **97140**. Defines the `quantity` and `unitPrice` for the billing system. |
| **Medical Necessity** | `Claim.diagnosis` | Internal Reference | The procedure (97140) is programmatically linked to the diagnosis (M51.2) to prevent automated insurance denials. |

---

## 💻 3. FHIR JSON Implementation
To see the actual code representation of the clinical diagnosis as expected by the NPHIES integration engine, please check the `resources` directory.

👉 **[View the Condition Resource JSON](./resources/condition-lumbar-disc-01.json)**

### 🛠️ Technical Note:
The implemented JSON explicitly maps the clinical diagnosis to the ICD-10-AM coding system and maintains the `clinicalStatus` as **Active**. This logic is crucial for justifying the associated medical procedure in the final Claim resource, thereby acting as a primary defense against automated denials.
