<div align="center">

<img src="https://img.shields.io/badge/PREPLACE-AI-blueviolet?style=for-the-badge&logoColor=white" />

# PREPLACE AI
### AI-Driven Campus Placement Intelligence Platform

*Built for Shri Mata Vaishno Devi University, Katra*

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![ChromaDB](https://img.shields.io/badge/ChromaDB-Vector_Store-FF6B35?style=for-the-badge)](https://trychroma.com)
[![Gemini](https://img.shields.io/badge/Google_Gemini-AI-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge)]()

</div>

---
## What is PREPLACE?

PREPLACE (**Placement + Replace**) is a full-stack AI-powered campus placement platform that completely replaces the manual, slow, and inconsistent placement process at SMVDU with an intelligent, data-driven system.

- Students upload their resume and instantly get an **AI score out of 100**, strengths, improvement suggestions, and a matched job role — powered by **Google Gemini + ChromaDB vector search**
- Recruiters post jobs, view **AI-ranked candidates**, and manage a structured hiring pipeline
- The TPO/Admin controls users, scoring rules, and has a full audit trail of every action

> No more WhatsApp groups for job announcements. No more manual resume screening. No more guessing why you got rejected.

---



## Features

- **Hybrid AI Scoring** — Google Gemini for qualitative feedback + ChromaDB cosine similarity for deterministic scoring — combined into a single 0–100 score
- **Role Prediction** — Gemini reads the full resume and assigns the best-fit job role (SDE Intern, Data Analyst, ML Engineer, etc.)
- **Keyword Rules** — tiered penalty/boost rules (global → recruiter → listing level) that directly influence the score
- **9-Stage Application Pipeline** — Saved → Applied → Under Review → Shortlisted → Interview Scheduled → Offer Extended → Hired/Rejected/Withdrawn
- **Bidirectional Matching** — match a student to the most relevant jobs, or match a job to the most relevant candidates — both via ChromaDB vector search
- **Public Leaderboard** — top 20 scored resumes visible without login
- **LinkedIn Job Feed** — live jobs fetched based on AI-assigned role, cached per user for 24 hours
- **Audit Logs** — every action logged with actor, target, and timestamp
- **Zero Config Startup** — admin account, 10 scoring templates, and 4 global keyword rules auto-seeded on first run

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 · Vite · Vanilla CSS · Custom dark design system |
| Backend | Python · FastAPI · Uvicorn |
| Database | PostgreSQL · SQLAlchemy ORM |
| Vector Store | ChromaDB (persistent) · `all-MiniLM-L6-v2` SentenceTransformer |
| AI / LLM | Google Gemini API (`google-genai` SDK) |
| PDF Parsing | PyMuPDF · pdfplumber |
| Auth | PBKDF2-SHA256 password hashing · Signed auth tokens |
| Job Feed | LinkedIn Jobs API · Node.js worker |

---

## Project Structure
## Project Structure

```text
PREPLACE/
├── backend/
│   ├── main.py                   # FastAPI app entry point, router registration
│   ├── seed.py                   # Auto-seeds admin, templates, rules on first run
│   ├── database.py               # SQLAlchemy engine, session, startup migrations
│   ├── routers/
│   │   ├── auth.py               # Register, login, token validation
│   │   ├── resumes.py            # Upload, score, delete, leaderboard
│   │   ├── jobs.py               # Job listing CRUD + state transitions
│   │   ├── applications.py       # Apply, withdraw, status updates
│   │   ├── matching.py           # Vector-based job + candidate matching
│   │   ├── admin.py              # User management, templates, penalty rules
│   │   ├── analytics.py          # Recruiter + applicant analytics
│   │   ├── audit.py              # Audit log retrieval
│   │   └── linkedin.py           # LinkedIn job feed (cached)
│   ├── models/                   # SQLAlchemy ORM models
│   ├── schemas/                  # Pydantic request/response schemas
│   ├── services/
│   │   ├── ai_service.py         # Gemini + ChromaDB pipeline
│   │   ├── scoring_service.py    # Hybrid score computation
│   │   ├── matching_service.py   # Bidirectional vector matching
│   │   └── audit_service.py      # Audit log writes
│   ├── .env.example
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── LandingPage.jsx      # Public page + leaderboard
│   │   ├── Dashboard.jsx        # Student view
│   │   ├── RecruiterDashboard.jsx
│   │   ├── AdminDashboard.jsx
│   │   ├── JobDetailsPage.jsx
│   │   └── Shared.jsx           # Toast, loaders, shared components
│   ├── index.html
│   └── package.json
├── linkedin-worker/            # Node.js LinkedIn job fetcher
├── .gitignore
└── README.md
```

## Local Setup

### Prerequisites

Make sure you have these installed:

[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)](https://python.org)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green?logo=node.js)](https://nodejs.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-14+-blue?logo=postgresql)](https://postgresql.org)

---

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/preplace.git
cd preplace
```

---

### 2. Backend Setup

```bash
cd backend

# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

Create your `.env` file:
```bash
cp .env.example .env
```

Open `.env` and fill in your values:
```env
GEMINI_API_KEY=your_gemini_key_here
DATABASE_URL=postgresql://username:password@localhost:5432/preplace
SECRET_KEY=your_secret_key_here
```

Generate a secure secret key:
```bash
python3 -c "import secrets; print(secrets.token_hex(32))"
```

Get a free Gemini API key at → **https://aistudio.google.com/app/apikey**

Create the PostgreSQL database:
```bash
psql -U postgres -c "CREATE DATABASE preplace;"
```

Start the backend:
```bash
uvicorn main:app --reload
```

> On first run, the backend **automatically** seeds the admin account, 10 scoring templates into ChromaDB, and 4 global keyword rules. No manual setup needed.

Backend runs at → **http://localhost:8000**  
API docs at → **http://localhost:8000/docs**

---

### 3. Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

Frontend runs at → **http://localhost:5173**

---

### 4. LinkedIn Worker Setup (optional)

```bash
cd linkedin-worker
npm install
```

This runs automatically when a student visits the LinkedIn Suggestions tab. Requires Node.js on PATH.

---

### Default Credentials (after first run)

| Role | Email | Password |
|---|---|---|
| Admin / TPO | `admin@preplace.smvdu` | `admin@123` |

> Change the admin password immediately after first login.

---

### What Gets Auto-Seeded on First Run

| What | Details |
|---|---|
| Admin account | `admin@preplace.smvdu` / `admin@123` |
| 10 Scoring Templates | SDE Intern, Data Analyst, Frontend Dev, Backend Dev, Full Stack, ML Engineer, DevOps, UI/UX, Cybersecurity, Mobile Dev |
| 4 Keyword Rules | Backend Core, Data Storage, Delivery & Ops, Testing & Quality |

---

## Environment Variables Reference

| Variable | Required | Description |
|---|---|---|
| `GEMINI_API_KEY` | Yes | Google Gemini API key from aistudio.google.com |
| `DATABASE_URL` | Yes | PostgreSQL connection string |
| `SECRET_KEY` | Yes | Random string for signing auth tokens |

---

## Important Notes

- Never commit your `.env` file — it's in `.gitignore`
- If ChromaDB or SentenceTransformers is unavailable, scoring falls back to text overlap — platform stays functional
- CORS is currently wildcard (`*`) — restrict to your domain before deploying to production
- LinkedIn job fetching requires Node.js installed and accessible on PATH

---
---



## License
MIT License
Copyright (c) 2026 Ayush Patel & Sourbh Sharma
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
