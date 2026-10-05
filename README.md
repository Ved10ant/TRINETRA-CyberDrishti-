# TRINETRA CyberDrishti (त्रिनेत्र साइबरदृष्टि) 👁️🛡️

> **Predictive ATM Hotspot Intelligence for Proactive Cybercrime Intervention**  
> *Smart India Hackathon (SIH 2026) | Problem Statement: SIH26184 | Ministry of Home Affairs (MHA), Govt. of India*

---

## 📌 Executive Summary

India receives **8,000+ cybercrime complaints daily** across the National Cybercrime Reporting Portal (NCRP). In financial cyber-frauds (UPI scam, SIM-swap, vishing, impersonation):
- Stolen money moves rapidly through **3 to 6 mule accounts** across states within minutes.
- Cash is withdrawn from an ATM within the **30–90 minute "golden hour"**.
- Traditional law enforcement response averages **4–6 hours**, resulting in a national fund recovery rate of **under 22%** (I4C Annual Report).

**TRINETRA CyberDrishti** flips the paradigm from *reactive investigation* to *proactive real-time intervention*. By intercepting transaction sessions at the network layer and tracing mule terminal accounts, our ML ensemble predicts the target ATM cashout cluster and alerts field officers within **35 seconds** of complaint registration.

---

## ⚡ Key Innovations

1. **Session Intercept Layer (SIL):** Intercepts digital transaction sessions at the network/DNS layer to pinpoint the active terminal mule node in real time.
2. **Mule-Anchored Geospatial Prediction:** Instead of using the victim's location (which yields unusable ~50km radiuses), TRINETRA anchors predictions to the **terminal mule location**, shrinking the search radius to **~5 km**.
3. **ML Ensemble Pipeline:**
   - **Graph Neural Networks (GNN):** Multi-hop mule account network detection and money trail topology mapping.
   - **DBSCAN:** Density-based spatial clustering of historical ATM fraud hotspots.
   - **XGBoost:** Time-to-cashout classification and dynamic threat priority scoring (P1 / P2 / P3).
4. **Sub-35-Second SLA:** Automatic alert generation and dispatch to GIS command dashboards and field officers before cash is liquidated.

---

## 🏗️ System Architecture

```text
                     ┌─────────────────────────────┐
                     │     CITIZEN / VICTIM        │
                     └──────────────┬──────────────┘
                                    │ Files complaint with UTR / Txn ID
                                    ▼
                ┌───────────────────────────────────────┐
                │        NCRP CITIZEN PORTAL            │
                │  Next.js / Tailwind / Interactive UI  │
                └───────────────────┬───────────────────┘
                                    │ POST /api/complaints
                                    ▼
                ┌───────────────────────────────────────┐
                │          SUPABASE POSTGRESQL          │
                │ Real-time triggers & database storage │
                └───────────────┬───────────────────────┘
                                │
        ┌───────────────────────┴───────────────────────┐
        ▼                                               ▼
┌───────────────────────────────┐       ┌───────────────────────────────┐
│     NODE.JS EXPRESS API       │       │    ML SERVICES (FastAPI)      │
│  - Complaint management       │◄─────►│  - GNN Mule Detection         │
│  - Dispatch & tracking        │       │  - DBSCAN ATM Clustering      │
│  - Real-time alert websocket  │       │  - XGBoost Priority Scoring   │
└───────────────┬───────────────┘       └───────────────────────────────┘
                │
                ▼
┌───────────────────────────────────────────────────────────────────────┐
│                LAW ENFORCEMENT & COMMAND DASHBOARD                    │
│  - Real-time GIS Hotspot Map (Leaflet / OpenStreetMap)                │
│  - P1 / P2 / P3 Priority Alert Stream (<35s latency)                 │
│  - Hop-by-Hop Money Trail & Mule Network Graph Visualizer            │
│  - Field Unit Dispatch & Geofence Routing                             │
└───────────────────────────────────────────────────────────────────────┘
```

---

## 📂 Project Structure

```text
SIHhack/
├── backend/                  # Node.js + Express REST API backend
│   ├── src/
│   │   ├── config/           # Database & Supabase configurations
│   │   ├── controllers/      # Business logic (complaints, alerts, chat)
│   │   ├── routes/           # Express API route endpoints
│   │   └── server.js         # Entry point & CORS configuration
│   └── package.json
│
├── frontend/                 # Interactive Law Enforcement Dashboard
│   ├── src/
│   │   ├── components/       # GIS maps, alert badges, analytical widgets
│   │   ├── pages/            # Crime feed, hotspot map, suspect graph
│   │   └── services/         # API clients & Supabase subscriptions
│   └── package.json
│
├── sih-ncrp-main/            # Citizen-facing NCRP complaint intake portal
│   ├── app/                  # Next.js App router (OTP, intake form, tracking)
│   └── package.json
│
├── ml-services/              # Python ML & AI Microservices
│   ├── ml/
│   │   ├── models/           # XGBoost, DBSCAN, and GNN weights
│   │   ├── data/             # Training datasets and synthetic generators
│   │   └── pipelines/        # Feature extraction & inference pipelines
│   └── requirements.txt
│
├── session-intercept/        # Session Intercept Layer (SIL) simulation & telemetry
├── database/                 # SQL schemas, migrations, and seed scripts
├── docker-compose.yml        # Multi-container orchestration specification
└── README.md
```

---

## 🚀 Quickstart & Installation

### Prerequisites
- **Node.js**: v18+ (Node 22 recommended)
- **Python**: v3.10+
- **Docker & Docker Compose** (optional for containerized deployment)
- **Supabase Account** or PostgreSQL database instance

### 1. Clone the Repository
```bash
git clone https://github.com/Ved10ant/TRINETRA-CyberDrishti-.git
cd TRINETRA-CyberDrishti-
```

### 2. Environment Configuration
Create `.env` files in `backend/`, `frontend/`, and `ml-services/` following the templates:

**`backend/.env`**:
```env
PORT=5000
SUPABASE_URL=your_supabase_url
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
ML_SERVICE_URL=http://localhost:8000
```

**`frontend/.env`**:
```env
VITE_API_URL=http://localhost:5000/api
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

### 3. Backend Setup
```bash
cd backend
npm install
npm run dev
```

### 4. Frontend Command Dashboard
```bash
cd frontend
npm install
npm run dev
```

### 5. ML Services
```bash
cd ml-services
python -m venv venv
# Windows:
venv\Scripts\activate
# Linux/macOS:
# source venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

---

## 🛡️ Security & Compliance

- **Zero Credential Exposure:** All `.env` and sensitive configurations are strictly excluded via `.gitignore`.
- **Role-Based Access Control (RBAC):** Distinct roles for Law Enforcement Analysts, Field Officers, and Admin Superusers.
- **Audit Trails:** Every prediction, dispatch event, and complaint lookup is immutably logged for legal evidentiary submission under the Bharatiya Nagarik Suraksha Sanhita (BNSS).

---

## 👥 Team & Acknowledgments

Developed for **Smart India Hackathon 2026**  
**Theme:** Smart Policing / Cybersecurity (Ministry of Home Affairs)  
**Problem Statement ID:** SIH26184
