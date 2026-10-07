# FLOWGUARD

### Predictive Reliability Platform for Microservices

> **Don't wait for your system to fail. Predict it. Simulate it. Stop it.**

**Team CodeX — CURIOUSPARC 2026**

---

## What is FlowGuard?

Modern applications are not single systems. They are networks of services that depend on each other.

A small problem in one service can quickly become a large failure:

```text
Database slows down
       ↓
Payment / Inventory become slow
       ↓
Order Service starts timing out
       ↓
Gateway becomes slow
       ↓
Frontend requests fail
       ↓
Customer checkout breaks
```

**FlowGuard helps detect this chain before it becomes a production incident.**

It combines:

*  **Failure Detection**
*  **Root-Cause Analysis**
*  **Dependency Graphs**
*  **What-If Simulation**
*  **PR Reliability Checks**
*  **Verified Auto-Fix**
*  **AI-powered Incident Briefs**

Instead of asking:

> *"Why did production fail?"*

FlowGuard helps teams ask:

> **"Could this change cause a failure, and how can we prevent it before deployment?"**

---

#  The Problem

Traditional monitoring mainly tells engineers what is happening **after or during an incident**.

For example:

```text
Database latency ↑
       ↓
API latency ↑
       ↓
Timeouts ↑
       ↓
Checkout failures ↑
```

By the time the engineering team understands the dependency chain, users may already be affected.

Chaos testing can expose weaknesses, but it is usually performed separately from the development workflow.

### The gap

| Existing approach        | Limitation                                            |
| ------------------------ | ----------------------------------------------------- |
| Monitoring               | Often identifies problems after they begin            |
| Dashboards               | Show metrics but may not explain the full cascade     |
| Manual chaos testing     | Requires separate setup and execution                 |
| Code review              | Usually focuses on correctness, not system resilience |
| Enterprise observability | Can be expensive and complex for small teams          |

**FlowGuard brings reliability testing closer to the development process.**

---

#  Our Solution

FlowGuard creates a **simulated microservice environment**, builds a dependency graph, injects controlled failures, measures the resulting behavior, and converts the results into a reliability verdict.

### FlowGuard Loop

```text
        ┌───────────────┐
        │    OBSERVE    │
        │ Service Graph │
        └───────┬───────┘
                ↓
        ┌───────────────┐
        │    DETECT     │
        │   Anomalies   │
        └───────┬───────┘
                ↓
        ┌───────────────┐
        │   SIMULATE    │
        │  Faults /     │
        │    What-if    │
        └───────┬───────┘
                ↓
        ┌───────────────┐
        │    ANALYZE    │
        │ Root Cause +  │
        │ Blast Radius  │
        └───────┬───────┘
                ↓
        ┌───────────────┐
        │    VALIDATE   │
        │ PR Reliability│
        └───────┬───────┘
                ↓
        ┌───────────────┐
        │    IMPROVE    │
        │ Verified Fix  │
        └───────┬───────┘
                │
                └──────→ Repeat
```

This creates a continuous **Observe → Detect → Simulate → Analyze → Validate → Improve** loop.

---

#  How FlowGuard Works

## 1. Build the Dependency Graph

FlowGuard represents the application as a graph of services.

Our prototype contains:

```text
Frontend
   ↓
Gateway
   ↓
Order
 ┌─┴───────┐
 ↓         ↓
Inventory  Payment
 ↓         ↓
 └────┬────┘
      ↓
   Database
```

Each service has properties such as:

* latency
* capacity
* load
* timeout
* retry behavior
* circuit breaker
* fallback behavior

This allows FlowGuard to reason about how a failure can propagate.

---

# 2. Detect Anomalies

The intelligence layer continuously evaluates system behavior.

It looks at signals such as:

* latency
* error rate
* service utilization
* dependency behavior
* propagation patterns

When values move beyond defined thresholds, FlowGuard identifies an anomaly.

The system then estimates:

### Root Cause

**Which service is most likely responsible?**

### Propagation Path

**How is the failure travelling through the system?**

### Blast Radius

**Which services and user-facing components can be affected?**

---

# 3. Simulate Failures

FlowGuard can inject controlled faults into the simulated environment.

Current scenarios include:

### 🗄️ Database Latency

```text
Database latency ×3
        ↓
Payment / Inventory affected
        ↓
Order affected
        ↓
Gateway affected
        ↓
Frontend affected
```

###  Service Down

A service is made unavailable to observe how the dependency graph responds.

###  Traffic Spike

External traffic is increased to identify capacity and saturation problems.

---

# 4. Calculate Resilience

FlowGuard converts the measured system behavior into a **Resilience Score from 0–100**.

The score considers:

* error-rate degradation
* latency degradation
* scenario performance

Higher score = more resilient system.

For the current simulator:

```text
Healthy baseline
      ↓
Resilience Score: 84
```

A risky change can significantly reduce the score.

---

# 5. What-If Simulation

Engineers can ask:

> **"What happens if this service becomes slower?"**

or:

> **"What happens if traffic doubles?"**

FlowGuard runs the scenario before the actual incident happens.

Example:

```text
Traffic ×2
     ↓
Queue increases
     ↓
Latency increases
     ↓
Timeout risk increases
     ↓
Resilience decreases
```

This allows engineers to understand system weaknesses before deployment.

---

# 6. PR Resilience Check

FlowGuard moves reliability testing into the development workflow.

A Pull Request can be evaluated against the baseline.

Example:

```text
BASELINE
Resilience = 84
     ↓
PR #12
Resilience = 32
     ↓
❌ REGRESSION DETECTED
```

Instead of simply saying:

> "Your code changed."

FlowGuard can say:

> **"This change reduces system resilience under failure conditions."**

---

# 7. Verified Auto-Fix 

FlowGuard does not blindly trust an automatically generated fix.

The workflow is:

```text
Detect regression
      ↓
Generate candidate fix
      ↓
Apply patch
      ↓
Run fault scenarios again
      ↓
Compare resilience
      ↓
Improved?
   ↙       ↘
 YES       NO
 ↓          ↓
Accept     Reject
```

The fix is accepted **only when the system validates that resilience improves**.

### Example

```text
Before fix:  32
     ↓
Auto-Fix
     ↓
Re-test
     ↓
After fix:   84
     ↓
✅ VERIFIED
```

This is why we call it **Verified Auto-Fix**, rather than simply Auto-Fix.

---

#  AI Incident Brief

FlowGuard also provides a human-readable explanation of incidents.

Instead of forcing an engineer to interpret multiple metrics:

```text
Latency: 628 ms
Error rate: 0.004
Root cause: Database
Affected services: Payment, Inventory, Order...
```

the AI layer converts the analysis into an understandable incident brief.

### Important design principle

The AI layer is used for **explanation**, not for inventing system measurements.

```text
Measured Data
      ↓
Deterministic Intelligence
      ↓
Guardrails
      ↓
AI Explanation
```

If the LLM is unavailable, FlowGuard can fall back to deterministic templates / fixtures so the demo remains functional.

---

#  Intelligence Layer

The intelligence layer combines deterministic system analysis with statistical detection.

It produces:

```text
Anomaly
   ↓
Root Cause
   ↓
Propagation Path
   ↓
Blast Radius
   ↓
Resilience Score
   ↓
Risk / Prediction
```

The current prototype includes:

* anomaly thresholds
* dependency propagation analysis
* root-cause confidence estimation
* blast-radius analysis
* resilience scoring
* analytic prediction

The predictor provides an expected outcome before the measured simulation result is available.

This also allows FlowGuard to compare:

```text
Predicted behavior
        vs
Measured behavior
```

and improve confidence in the reliability analysis.

---

#  Verified Prototype Results

Our simulator currently produces the following reference behavior:

| Scenario                |       Result |
| ----------------------- | -----------: |
| Healthy baseline p95    |      ~468 ms |
| Baseline resilience     |       **84** |
| Database latency ×3     |  ~628 ms p95 |
| Service down            |  ~369 ms p95 |
| Traffic spike ×2        | ~2018 ms p95 |
| Risky PR #12            |       **32** |
| After Verified Auto-Fix |       **84** |

### Example cascade

For the database latency scenario:

```text
Tick 10 → Database
Tick 11 → Payment / Inventory
Tick 12 → Order
Tick 13 → Gateway
Tick 14 → Frontend
```

This demonstrates that a local degradation can propagate through multiple dependencies.

---

#  System Architecture

```text
                    ┌────────────────────┐
                    │      Frontend      │
                    │ Next.js / React    │
                    └─────────┬──────────┘
                              │
                              ▼
                    ┌────────────────────┐
                    │    FastAPI API     │
                    │ REST + WebSocket   │
                    └─────────┬──────────┘
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
          ▼                   ▼                   ▼
   ┌────────────┐      ┌────────────┐      ┌────────────┐
   │ Simulator  │      │Intelligence│      │ PR Engine  │
   │            │      │   Layer    │      │            │
   └─────┬──────┘      └─────┬──────┘      └─────┬──────┘
         │                   │                   │
         └───────────────────┼───────────────────┘
                             ▼
                    ┌────────────────────┐
                    │    AI / Brief      │
                    │ LLM + Guardrails   │
                    └────────────────────┘
```

---

#  Main Components

### Frontend

* Next.js
* React
* Tailwind CSS
* React Flow
* Recharts

Responsible for:

* command center
* service graph
* alerts
* simulations
* pull requests
* resilience visualization

### Backend

* Python
* FastAPI
* WebSockets
* NetworkX

Responsible for:

* APIs
* simulation
* system state
* dependency graph
* real-time updates

### Intelligence

Responsible for:

* anomaly detection
* root-cause analysis
* propagation analysis
* blast radius
* resilience scoring
* prediction

### PR Reliability Engine

Responsible for:

* PR scanning
* reliability rules
* regression detection
* Auto-Fix
* acceptance gate

### AI Layer

Responsible for:

* incident briefs
* PR comments
* human-readable explanations
* guarded LLM responses
* deterministic fallback

---

#  Multi-Tenancy

FlowGuard is designed with a B2B architecture in mind.

Every stored document contains:

```text
tenant_id
project_id
```

Data access is filtered by tenant.

Example:

```text
Company A
 ├── Project 1
 └── Project 2

Company B
 ├── Project 1
 └── Project 2
```

A company sees only its own projects and reliability data.

### Prototype

Tenant identity is provided through:

```text
X-Tenant-ID
```

### Production

The tenant identity should come from a **signed authentication token**, rather than a user-controlled header.

---

#  Demo Flow

The complete demonstration follows this sequence:

```text
1. Start with healthy system
          ↓
2. Show dependency graph
          ↓
3. Inject database latency
          ↓
4. Observe cascade
          ↓
5. Identify root cause
          ↓
6. Show blast radius
          ↓
7. Run What-If analysis
          ↓
8. Analyze PR #12
          ↓
9. Detect resilience regression
          ↓
10. Generate Auto-Fix
          ↓
11. Re-run fault scenarios
          ↓
12. Verify resilience improvement
```

---

#  Testing

Backend tests:

```bash
python -m pytest backend -q
```

Replay fixtures can be regenerated using:

```bash
python scripts/record_fixtures.py
```

The simulator is deterministic enough to provide repeatable reference scenarios for demonstration and testing.

---

#  Quick Start

## Clone the repository

```bash
git clone <repository-url>
cd FLOWGUARD
```

## Configure environment

```bash
cp .env.example .env
```

LLM keys are optional.

Without them, FlowGuard can use its deterministic template / fixture fallback.

---

## Start FlowGuard

```bash
./start.sh
```

This starts:

```text
Backend → :8000
Frontend → :3000
```

---

## Backend Only

```bash
cd backend

python -m venv .venv

# Windows
.venv\Scripts\activate

# Linux / macOS
source .venv/bin/activate

pip install -r requirements.txt

cd ..

python -m uvicorn backend.app.main:app --reload --port 8000
```

---

## Frontend Only

```bash
cd frontend
npm install
npm run dev
```

---

#  API

Main endpoints:

```text
GET  /api/topology
GET  /api/health

POST /api/chaos
POST /api/reset

GET  /api/demo/mode
POST /api/demo/mode

POST /api/whatif

GET  /api/pr
POST /api/pr/{id}/analyze
POST /api/pr/{id}/heal

GET  /api/brief

WS   /ws
```

API documentation:

```text
http://localhost:8000/docs
```

Health check:

```text
http://localhost:8000/api/health
```

---

#  Project Structure

```text
FLOWGUARD/
│
├── backend/
│   └── app/
│       ├── ai/
│       │   ├── brief
│       │   ├── guardrails
│       │   ├── llm
│       │   └── pr_comment
│       │
│       ├── intel/
│       │   └── runtime/
│       │
│       ├── pr/
│       │   ├── scanner
│       │   ├── heal
│       │   ├── gate
│       │   └── prs
│       │
│       ├── sim/
│       ├── main.py
│       ├── api.py
│       ├── schemas.py
│       └── state.py
│
├── frontend/
│
├── fixtures/
│
├── scripts/
│
├── docs/
│
├── reference_sim.py
│
└── start.sh
```

---

#  Team

| Member        | Responsibility                         |
| ------------- | -------------------------------------- |
| **Shardul**   | Frontend, AI prompts, demo flow        |
| **Abhiraj**   | Backend, simulator, APIs, integration  |
| **Sneha**     | Intelligence layer, analytics, testing |
| **Vaishnavi** | PR engine, Auto-Fix, AI integration    |

---

#  Future Scope

FlowGuard's current prototype demonstrates the reliability workflow using a simulated microservice environment.

The production path can extend this architecture toward:

### Real Observability

Integrate real telemetry such as:

```text
OpenTelemetry traces
        ↓
Dependency Graph
        ↓
Reliability Analysis
```

### Preview Environments

Run reliability tests against isolated environments before deployment.

### GitHub Integration

```text
Developer creates PR
        ↓
FlowGuard checks reliability
        ↓
PR receives verdict
        ↓
Developer fixes issue
        ↓
FlowGuard verifies again
```

### Kubernetes

Run fault-injection and validation workflows against Kubernetes-based environments.

### SaaS

Support multiple organizations, projects and repositories at scale.

---

#  Prototype vs Production

| Prototype                     | Production Direction                 |
| ----------------------------- | ------------------------------------ |
| Simulated microservices       | Real services / preview environments |
| Deterministic fault scenarios | Controlled real fault injection      |
| Simulated telemetry           | OpenTelemetry                        |
| Demo PRs                      | GitHub/Git provider integration      |
| Prototype tenant header       | Signed authentication token          |
| Local execution               | Cloud/SaaS deployment                |

**The current prototype should not be interpreted as monitoring a real production system.** It demonstrates and validates the reliability workflow in a controlled simulated environment.

---

#  Why FlowGuard?

FlowGuard changes reliability from:

> **"Something broke. Find out why."**

to:

> **"This change may break the system. Let's test it before deployment."**

It connects:

```text
Code Changes
     ↓
System Behavior
     ↓
Failure Simulation
     ↓
Intelligence
     ↓
Reliability Verdict
     ↓
Verified Improvement
```

### In one sentence:

> **FlowGuard is a predictive reliability platform that simulates cascading failures, explains their impact, checks code changes for resilience regressions, and verifies fixes before they reach production.**

---

#  License

This project was developed as part of **CURIOUSPARC 2026 — Team CodeX**.
