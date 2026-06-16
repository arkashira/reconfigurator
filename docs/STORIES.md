# STORIES.md

## Product: reconfigurator  
**Goal** – Deliver a machine‑learning powered tool that lets users remodel and reconfigure physical components (e.g., furniture, modular electronics, smart‑home devices) through an intuitive UI, while ensuring safety, performance, and seamless integration with existing Axentx infrastructure.

---

## Epics & Backlog

| Epic | Priority | Description |
|------|----------|-------------|
| **E1 – Core ML Engine** | ★★★★★ | Build the inference engine that predicts optimal reconfiguration plans. |
| **E2 – Component Integration** | ★★★★☆ | Connect the tool to physical components via APIs and firmware. |
| **E3 – User Interface** | ★★★★☆ | Provide a web/mobile UI for design, simulation, and deployment. |
| **E4 – Safety & Validation** | ★★★★☆ | Ensure plans are safe, compliant, and validated before execution. |
| **E5 – Deployment & Monitoring** | ★★★★☆ | Deploy reconfiguration commands to devices and monitor outcomes. |
| **E6 – Analytics & Feedback Loop** | ★★★☆☆ | Capture usage data, performance metrics, and feed back into training. |
| **E7 – Documentation & Support** | ★★☆☆☆ | Create docs, tutorials, and support channels. |

---

## User Story Backlog

> **Format**:  
> *As a <role>, I want <goal>, so that <benefit>.*  
> **Acceptance Criteria** – list of conditions that must be met for the story to be considered done.

### Epic E1 – Core ML Engine

| Story | Acceptance Criteria |
|-------|---------------------|
| **S1.1** | *As a data scientist, I want the ML model to ingest 3D CAD data, so that it can generate accurate reconfiguration plans.* |
| | - Model accepts OBJ/STL files up to 10 MB. |
| | - Output is a JSON plan with component IDs, positions, and constraints. |
| | - Latency < 2 s for typical 1 k component models. |
| **S1.2** | *As a product owner, I want the engine to support multiple languages, so that it can be used globally.* |
| | - Model weights are language‑agnostic; only UI text changes. |
| | - Localization files are loaded at runtime. |
| **S1.3** | *As a QA engineer, I want unit tests covering 95 % of inference code, so that regressions are caught early.* |
| | - 95 % coverage reported by coverage tool. |
| | - CI pipeline runs tests on every PR. |

### Epic E2 – Component Integration

| Story | Acceptance Criteria |
|-------|---------------------|
| **S2.1** | *As an integration engineer, I want a REST API that accepts reconfiguration plans, so that devices can receive commands.* |
| | - Endpoint `/api/v1/plan` accepts JSON, returns 200 OK. |
| | - Authentication via OAuth2 JWT. |
| **S2.2** | *As a firmware developer, I want the device to acknowledge plan execution status, so that the app can show real‑time progress.* |
| | - Device sends `plan_status` events over MQTT. |
| | - Status includes `queued`, `running`, `completed`, `failed`. |
| **S2.3** | *As a system admin, I want device health checks, so that I can pre‑empt failures.* |
| | - Health endpoint `/health` returns 200 with JSON `{status:"ok"}`. |
| | - Alerts sent to Ops Slack channel on failure. |

### Epic E3 – User Interface

| Story | Acceptance Criteria |
|-------|---------------------|
| **S3.1** | *As a designer, I want a drag‑and‑drop 3D canvas, so that I can visualize component placement.* |
| | - Canvas supports WebGL rendering of STL files. |
| | - Dragging updates underlying JSON model in real time. |
| **S3.2** | *As a user, I want a “Simulate” button, so that I can preview the reconfiguration before execution.* |
| | - Simulation runs in the browser using the ML engine via WebAssembly. |
| | - Results displayed as a 3D animation. |
| **S3.3** | *As a mobile user, I want a responsive UI, so that I can use the tool on tablets.* |
| | - UI scales to 768 px width without loss of functionality. |
| | - Touch gestures for pan/zoom on the canvas. |

### Epic E4 – Safety & Validation

| Story | Acceptance Criteria |
|-------|---------------------|
| **S4.1** | *As a safety officer, I want the engine to flag unsafe plans, so that no hazardous configurations are deployed.* |
| | - Engine returns `unsafe:true` with list of violations. |
| | - Violations include collision, over‑stress, and power limits. |
| **S4.2** | *As a compliance manager, I want audit logs of all plans, so that I can trace decisions.* |
| | - Each plan stored with timestamp, user ID, and validation status. |
| | - Logs searchable via Kibana. |
| **S4.3** | *As a QA engineer, I want automated safety tests, so that we can run them nightly.* |
| | - Test suite simulates 100 random plans and checks for safety flags. |
| | - CI pipeline fails if any plan passes as unsafe. |

### Epic E5 – Deployment & Monitoring

| Story | Acceptance Criteria |
|-------|---------------------|
| **S5.1** | *As a dev‑ops engineer, I want CI/CD to deploy the reconfigurator to Kubernetes, so that scaling is automated.* |
| | - Helm chart available in `charts/reconfigurator`. |
| | - Deployment triggers on merge to `main`. |
| **S5.2** | *As a user, I want real‑time status updates, so that I know when the reconfiguration completes.* |
| | - WebSocket endpoint `/ws/status` streams updates. |
| | - UI shows progress bar and final status. |
| **S5.3** | *As a monitoring engineer, I want metrics exposed via Prometheus, so that we can alert on failures.* |
| | - Metrics include `reconfig_success_total`, `reconfig_latency_seconds`. |
| | - Alert rule for `reconfig_success_total` < 95 % in last 5 min. |

### Epic E6 – Analytics & Feedback Loop

| Story | Acceptance Criteria |
|-------|---------------------|
| **S6.1** | *As a data scientist, I want usage telemetry, so that we can improve the model.* |
| | - Telemetry sent to `telemetry.axentx.com`. |
| | - Data includes plan size, execution time, success/failure. |
| **S6.2** | *As a product manager, I want a dashboard showing key metrics, so that I can track adoption.* |
| | - Dashboard built in Grafana with panels for active users, plan count, success rate. |
| **S6.3** | *As a ML engineer, I want a retraining pipeline, so that the model learns from new data.* |
| | - Pipeline ingests telemetry, retrains nightly, and deploys new weights. |
| | - A/B test new model against baseline. |

### Epic E7 – Documentation & Support

| Story | Acceptance Criteria |
|-------|---------------------|
| **S7.1** | *As a new user, I want a quick‑start guide, so that I can begin using the tool in 5 min.* |
| | - Markdown guide in `docs/quickstart.md`. |
| | - Includes screenshots and sample plan. |
| **S7.2** | *As a developer, I want API docs, so that I can integrate with my own devices.* |
| | - Swagger UI available at `/docs`. |
| | - Example cURL requests provided. |
| **S7.3** | *As a support engineer, I want a knowledge base, so that I can resolve common issues.* |
| | - KB articles stored in `docs/kb/`. |
| | - Searchable via website. |

---

## MVP Release Order

1. **S1.1** – Core ML engine ingesting CAD data.  
2. **S2.1** – REST API for plan submission.  
3. **S3.1** – Drag‑and‑drop 3D canvas.  
4. **S4.1** – Safety flagging.  
5. **S5.1** – CI/CD to Kubernetes.  
6. **S5.2** – Real‑time status updates.  
7. **S6.1** – Telemetry collection.  
8. **S7.1** – Quick‑start guide.

Subsequent stories will refine UI, add safety tests, deploy monitoring, and expand analytics. All stories are traceable to the product backlog and will be reviewed in the next sprint planning session.
