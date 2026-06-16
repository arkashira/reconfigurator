# REQUIREMENTS.md

## Project Overview

**Project Name:** reconfigurator  
**Repository:** `arkashira/reconfigurator`  
**Product Type:** Machine‑Learning‑enabled physical‑component reconfiguration tool  
**Target Audience:** Industrial automation engineers, robotics integrators, and manufacturing SMEs who need rapid, data‑driven re‑layout of physical assets (e.g., conveyor belts, robotic arms, assembly lines).  

The tool will ingest sensor data, CAD models, and user constraints, then generate optimized reconfiguration plans that can be executed on physical hardware via an API. It must integrate with existing Axentx ML pipelines and adhere to the company’s data‑centric, production‑grade standards.

---

## 1. Functional Requirements

| ID | Description | Acceptance Criteria |
|----|-------------|---------------------|
| **FR‑1** | **Data Ingestion** | • Accept CAD files (STEP, IGES, STL) via REST endpoint or S3 bucket.<br>• Accept sensor logs (JSON, CSV) and real‑time telemetry streams.<br>• Validate file integrity and schema before processing. |
| **FR‑2** | **Model Integration** | • Load pre‑trained ML models from the Axentx model registry (vLLM or SGLang).<br>• Provide a model‑selection API to choose between “layout‑optimization” and “constraint‑satisfaction” models. |
| **FR‑3** | **Reconfiguration Planning** | • Generate a 3‑D reconfiguration plan (JSON + optional 3‑D view) within 30 s for up to 200 parts.<br>• Plan must satisfy user‑defined constraints (clearance, weight limits, power consumption). |
| **FR‑4** | **Simulation & Validation** | • Simulate the plan using a physics engine (e.g., Bullet or PyBullet) and return a pass/fail report.<br>• Provide a visual simulation preview in the web UI. |
| **FR‑5** | **Execution API** | • Expose an endpoint to send the plan to connected PLCs or robot controllers.<br>• Support rollback on failure. |
| **FR‑6** | **Audit Trail** | • Log every request, model version, plan, and execution outcome in a structured audit table.<br>• Enable query by user, timestamp, or model. |
| **FR‑7** | **User Interface** | • Web UI for uploading assets, setting constraints, viewing plans, and monitoring execution.<br>• Responsive design for desktop and tablet. |
| **FR‑8** | **Versioning & Rollback** | • Store each generated plan with a unique UUID.<br>• Allow users to revert to a previous plan via the UI or API. |
| **FR‑9** | **Integration with Axentx Pipelines** | • Emit events to the Axentx event bus after each successful plan generation.<br>• Consume feedback events to refine the ML model. |
| **FR‑10** | **Documentation & SDK** | • Auto‑generate API docs (OpenAPI 3.0).<br>• Provide a Python SDK for programmatic access. |

---

## 2. Non‑Functional Requirements

| ID | Category | Requirement | Metric / Target |
|----|----------|-------------|-----------------|
| **NFR‑1** | Performance | End‑to‑end plan generation latency | ≤ 30 s for ≤ 200 parts |
| **NFR‑2** | Scalability | Handle 100 concurrent plan requests | 99.9 % success rate |
| **NFR‑3** | Reliability | 99.5 % uptime for the API service | SLA |
| **NFR‑4** | Security | All data in transit encrypted (TLS 1.3) | |
| **NFR‑5** | Security | Role‑based access control (RBAC) for API endpoints | |
| **NFR‑6** | Data Privacy | All user data stored in compliance with GDPR & CCPA | |
| **NFR‑7** | Maintainability | Code coverage ≥ 85 % for core modules | |
| **NFR‑8** | Observability | Structured logging (JSON) and metrics (Prometheus) | |
| **NFR‑9** | Usability | UI response time < 2 s for interactive elements | |
| **NFR‑10** | Extensibility | Plug‑in architecture for new model types | |

---

## 3. Constraints

1. **Model Dependency** – Must use models from the Axentx registry; cannot train new models in this repo.  
2. **Hardware Interface** – Execution API must support only the PLC protocol used by Axentx’s existing hardware stack (Modbus/TCP).  
3. **Data Storage** – All persistent data must reside in the company’s PostgreSQL cluster; no local file persistence.  
4. **Compliance** – Must pass the Axentx internal security audit before release.  
5. **License** – All third‑party libraries must be MIT, Apache‑2.0, or BSD‑3.  
6. **Deployment** – Must run in a Kubernetes cluster with Helm charts provided by the Ops team.  

---

## 4. Assumptions

- Users will provide CAD files that conform to industry standards (STEP, IGES, STL).  
- Sensor data will be in JSON or CSV format and include timestamps.  
- The physical components are already instrumented with the necessary sensors and actuators.  
- The Axentx model registry is reachable with stable network connectivity.  
- The target environment has access to GPU resources for inference (if required).  

---

## 5. Acceptance Test Matrix

| Test ID | Description | Expected Result |
|---------|-------------|-----------------|
| **AT‑1** | Upload a valid STEP file and generate a plan | API returns 200 with plan JSON |
| **AT‑2** | Provide conflicting constraints (e.g., clearance < part size) | API returns 400 with clear error message |
| **AT‑3** | Simulate a generated plan | Simulation passes and UI shows preview |
| **AT‑4** | Execute plan on mock PLC | PLC receives commands, returns success |
| **AT‑5** | Query audit log for a plan | Returns correct audit record |
| **AT‑6** | Perform 100 concurrent plan requests | 99.5 % success, latency ≤ 30 s |

---

## 6. Deliverables

1. **API** – RESTful endpoints with OpenAPI spec.  
2. **Web UI** – React/Vue front‑end with responsive design.  
3. **Python SDK** – `reconfigurator_sdk` package.  
4. **Helm Chart** – Deployment manifests.  
5. **Documentation** – README, developer guide, and user manual.  
6. **Test Suite** – Unit, integration, and performance tests.  

---

## 7. Timeline (High‑Level)

| Phase | Duration | Key Milestones |
|-------|----------|----------------|
| **Discovery** | 1 wk | Requirements finalization |
| **Architecture** | 2 wk | Design docs, tech stack |
| **Core Development** | 6 wk | API, model integration, UI |
| **Testing & QA** | 2 wk | Unit, integration, performance |
| **Deployment Prep** | 1 wk | Helm chart, CI/CD |
| **Launch** | 1 wk | Internal beta, feedback loop |

---

### Signatures

| Role | Name | Date |
|------|------|------|
| Product Owner | Alex Rivera | 2026‑06‑15 |
| Lead Engineer | Maya Chen | 2026‑06‑15 |
| QA Lead | Ravi Patel | 2026‑06‑15 |

---
