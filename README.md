<p align="center">
  <img src="https://img.shields.io/badge/CareConnect-Healthcare%20Platform-00BFA6?style=for-the-badge&logoColor=white&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmc[...]
</p>

<h1 align="center">🏥 CareConnect</h1>

<p align="center">
  <strong>An enterprise-grade healthcare management platform built with microservices architecture</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Spring%20Boot-3.3-6DB33F?style=flat-square&logo=springboot&logoColor=white" alt="Spring Boot">
  <img src="https://img.shields.io/badge/Node.js-22-339933?style=flat-square&logo=nodedotjs&logoColor=white" alt="Node.js">
  <img src="https://img.shields.io/badge/Next.js-15-000000?style=flat-square&logo=nextdotjs&logoColor=white" alt="Next.js">
  <img src="https://img.shields.io/badge/PostgreSQL-16-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/MongoDB-7-47A248?style=flat-square&logo=mongodb&logoColor=white" alt="MongoDB">
  <img src="https://img.shields.io/badge/Kafka-7.6-231F20?style=flat-square&logo=apachekafka&logoColor=white" alt="Kafka">
  <img src="https://img.shields.io/badge/Docker-Containerized-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/AWS-Cloud-FF9900?style=flat-square&logo=amazonaws&logoColor=white" alt="AWS">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License">
</p>

<p align="center">
  <a href="#-features">Features</a> •
  <a href="#%EF%B8%8F-architecture">Architecture</a> •
  <a href="#-tech-stack">Tech Stack</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="#-project-structure">Project Structure</a> •
  <a href="#-api-documentation">API Docs</a> •
  <a href="#-testing">Testing</a> •
  <a href="#-deployment">Deployment</a>
</p>

---

## 📖 About

**CareConnect** is a full-stack healthcare management platform that enables patients to find doctors across 28 medical specialties, book physical and virtual appointments, manage daily medications, and access secure medical records in one place.

The platform follows a **dual-backend microservices architecture** — using **Spring Boot + PostgreSQL** for transactional reliability (auth, appointments, billing) and **Node.js + MongoDB** for flexible services such as medication tracking, records, notifications, and search.

---

## ✨ Features

### 🔐 Authentication & Security
- Multi-step registration with real-time validation
- Flexible login (username / email / phone)
- JWT-based authentication with refresh tokens
- Password reset via OTP verification
- Auto-logout after 5 minutes of inactivity
- Role-Based Access Control (Patient, Doctor, Admin)

### 👨‍⚕️ Doctor Directory
- 28 medical specialties with filterable icon cards
- Doctor profiles with ratings, experience, working hours
- Smart search across name, specialty, and hospital
- ML-powered doctor recommendation engine

### 📅 Appointment System
- Book physical and virtual consultations
- 30-minute time slot grid (9 AM – 4:30 PM)
- Double-booking prevention with database constraints
- Automatic reminders (24h and 1h before)
- Upcoming/past appointment management with cancellation

### 💊 Medication Management
- Add medications with name, dosage, purpose, and time
- Time-sorted medication list
- Scheduled medication reminders via notifications
- Adherence tracking

### 📋 Medical Records
- Secure file upload with MinIO (S3-compatible storage)
- Records listing with download and delete
- ML-powered document classification (Phase 4)

### 🤖 AI & ML Features (Phase 4)
- **Symptom Checker** — XGBoost-based disease prediction from symptoms
- **Health Risk Predictor** — Neural network for diabetes/heart disease risk
- **Doctor Recommendations** — Collaborative filtering engine
- **No-Show Predictor** — XGBoost model for appointment attendance
- **Review Sentiment Analysis** — DistilBERT-powered sentiment scoring
- **AI Health Chatbot** — RAG pipeline with LangChain + ChromaDB + Llama 3

### 🔔 Notifications
- Email notifications (appointment confirmations, reminders)
- Real-time in-app notifications via WebSocket (Socket.IO)
- Event-driven architecture via Kafka

---

## 🏗️ Architecture

```
                         ┌──────────────────┐
                         │    Frontend      │
                         │  Next.js 15 +    │
                         │  React + Tailwind│
                         └────────┬─────────┘
                                  │ HTTPS
                         ┌────────▼─────────┐
                         │   API Gateway    │
                         │ Spring Cloud GW  │
                         └────────┬─────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
   ┌──────────▼──────┐ ┌─────────▼───────┐ ┌─────────▼───────┐
   │  Auth Service   │ │ Doctor Service  │ │ Appointment Svc │
   │  (Spring Boot)  │ │ (Spring Boot)   │ │ (Spring Boot)   │
   │  + PostgreSQL   │ │ + PostgreSQL    │ │ + PostgreSQL    │
   └──────────┬──────┘ └─────────┬───────┘ └─────────┬───────┘
              │                   │                   │
              └───────────────────┼───────────────────┘
                                  │ Kafka Events
                    ┌─────────────▼──────────────┐
                    │       Apache Kafka          │
                    │    (Event Streaming)        │
                    └─────────────┬──────────────┘
              ┌───────────────────┼───────────────────┐
              │                   │                   │
   ┌──────────▼──────┐ ┌─────────▼───────┐ ┌─────────▼───────┐
   │ Medication Svc  │ │ Notification Svc│ │  Search Service │
   │ (Express.js)    │ │ (Express.js)    │ │  (Express.js)   │
   │ + MongoDB       │ │ + Socket.IO     │ │  + Elasticsearch│
   └─────────────────┘ └─────────────────┘ └─────────────────┘
              │
   ┌──────────▼──────┐    ┌──────────────────┐
   │ Records Service │    │  ML Service      │
   │ (Express.js)    │    │  (FastAPI)       │
   │ + MongoDB+MinIO │    │  + LangChain+RAG │
   └─────────────────┘    └──────────────────┘
```

### Design Principles
- **SOLID Principles** applied across all services
- **14 Design Patterns**: MVC, Repository, DTO, Builder, Factory, Singleton, Observer, Strategy, Adapter, Facade, Proxy, Chain of Responsibility, Template Method, Decorator
- **Event-Driven Architecture** via Apache Kafka for loose coupling
- **Database-per-Service** pattern for data isolation

---

## 🛠 Tech Stack

### Backend — Reliability Layer (ACID Transactions)
| Technology | Version | Purpose |
|-----------|---------|---------|
| Java | 21 (LTS) | Primary language |
| Spring Boot | 3.3 | REST APIs, DI, auto-configuration |
| Spring Security | 6.x | JWT authentication, RBAC |
| Spring Data JPA | 3.x | ORM, repository pattern |
| PostgreSQL | 16 | Relational database (users, doctors, appointments) |
| Flyway | 10.x | Database migrations |
| Redis | 7 | Caching, sessions, OTP storage |
| Spring Cloud Gateway | 4.x | API gateway, routing, rate limiting |

### Backend — Flexibility Layer (Schema-Free)
| Technology | Version | Purpose |
|-----------|---------|---------|
| Node.js | 22 (LTS) | Runtime for flexibility services |
| Express.js | 5.x | REST APIs |
| MongoDB | 7 | Document database (medications, records) |
| Mongoose | 8.x | MongoDB ODM |
| Socket.IO | 4.x | Real-time WebSocket notifications |
| MinIO | Latest | S3-compatible file storage |
| Elasticsearch | 8.13 | Full-text search engine |

### Event Streaming
| Technology | Version | Purpose |
|-----------|---------|---------|
| Apache Kafka | 7.6 (Confluent) | Async event streaming between services |
| Zookeeper | 7.6 | Kafka cluster coordination |

### Frontend
| Technology | Version | Purpose |
|-----------|---------|---------|
| React | 19 | UI component library |
| Next.js | 15 | SSR/SSG framework, App Router |
| TypeScript | 5.x | Type-safe JavaScript |
| Tailwind CSS | 4.x | Utility-first CSS framework |
| Shadcn/UI | Latest | Accessible component library |
| Zustand | 5.x | Lightweight state management |
| TanStack Query | 5.x | Server state management |
| React Hook Form | 7.x | Form handling |
| Zod | 3.x | Schema validation |

### AI / ML
| Technology | Version | Purpose |
|-----------|---------|---------|
| Python | 3.12 | ML model development |
| FastAPI | Latest | ML model serving |
| scikit-learn | 1.x | Classical ML models |
| TensorFlow/Keras | 2.x | Neural networks |
| LangChain | Latest | RAG pipeline orchestration |
| ChromaDB | Latest | Vector database for embeddings |
| HuggingFace | Latest | Pre-trained NLP models (DistilBERT, spaCy) |
| Groq API | Free tier | Llama 3 inference (no local GPU required) |

### DevOps & Infrastructure
| Technology | Purpose |
|-----------|---------|
| Docker + Docker Compose | Containerization (12+ containers) |
| GitHub Actions | CI/CD pipeline |
| AWS (EC2, RDS, S3) | Cloud deployment |
| Vercel | Frontend hosting |
| Render | Backend hosting (free tier) |
| Prometheus + Grafana | Monitoring & observability |

### Testing
| Technology | Purpose |
|-----------|---------|
| JUnit 5 + Mockito | Java unit & integration tests |
| Testcontainers | Integration tests with real databases |
| Jest + Supertest | Node.js unit & API tests |
| Cypress | E2E browser testing |
| Playwright | Cross-browser E2E testing |
| Postman + Newman | API collection testing & CI automation |

---

## 🚀 Getting Started

### Prerequisites

- **Java 21** — [Eclipse Temurin](https://adoptium.net/)
- **Node.js 22** — [nodejs.org](https://nodejs.org/)
- **Python 3.12** — [python.org](https://www.python.org/)
- **Docker Desktop** — [docker.com](https://www.docker.com/products/docker-desktop/)
- **Maven 3.9+** — [maven.apache.org](https://maven.apache.org/)
- **Git** — [git-scm.com](https://git-scm.com/)

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/CareConnect.git
cd CareConnect
```

### 2. Start Infrastructure (Databases, Kafka, Redis)

```bash
cd infrastructure/docker
docker compose up -d
```

This starts:
- PostgreSQL (port 5432)
- MongoDB (port 27017)
- Redis (port 6379)
- Kafka + Zookeeper (port 9092)
- Elasticsearch (port 9200)
- MinIO (port 9000/9001)
- pgAdmin (port 5050)
- Mongo Express (port 8082)

### 3. Start Backend Services

**Spring Boot services:**
```bash
# Auth Service
cd backend-spring/auth-service
mvn spring-boot:run

# Doctor Service (port 8082)
cd backend-spring/doctor-service
mvn spring-boot:run

# Appointment Service (port 8083)
cd backend-spring/appointment-service
mvn spring-boot:run
```

**Node.js services:**
```bash
# Medication Service
cd backend-node/medication-service
npm install && npm run dev

# Records Service
cd backend-node/records-service
npm install && npm run dev

# Notification Service
cd backend-node/notification-service
npm install && npm run dev
```

### 4. Start Frontend

```bash
cd frontend
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

### 5. Start ML Services (Phase 4)

```bash
cd ml-services/prediction-service
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

---

## 📁 Project Structure

```
CareConnect/
├── backend-spring/                    # Java microservices
│   ├── auth-service/                  # Authentication + User management
│   ├── doctor-service/                # Doctor directory + Ratings
│   ├── appointment-service/           # Appointment booking + Reminders
│   └── api-gateway/                   # Spring Cloud Gateway
│
├── backend-node/                      # Node.js microservices
│   ├── medication-service/            # Medication CRUD + Reminders
│   ├── records-service/               # Medical records + File storage
│   ├── notification-service/          # Email + Push + In-app notifications
│   └── search-service/               # Elasticsearch indexing + Search
│
├── frontend/                          # Next.js 15 application
│   ├── app/                           # App Router pages
│   ├── components/                    # Reusable UI components
│   ├── lib/                           # API clients, utilities
│   └── stores/                        # Zustand state stores
│
├── ml-services/                       # Python ML services
│   ├── prediction-service/            # FastAPI ML model serving
│   ├── chatbot-service/               # RAG + LangChain chatbot
│   ├── data/                          # Training datasets
│   └── notebooks/                     # Jupyter EDA notebooks
│
├── infrastructure/
│   ├── docker/
│   │   ├── docker-compose.yml         # Full stack Docker setup
│   │   └── init-scripts/              # Database schema + seed data
│   └── k8s/                           # Kubernetes manifests (optional)
│
├── .github/
│   └── workflows/
│       ├── ci.yml                     # Build + Test on every push
│       └── deploy.yml                 # Deploy to cloud
│
└── docs/                              # Architecture diagrams, API specs
```

---

## 📡 API Documentation

All REST APIs are documented with **Swagger/OpenAPI**.

| Service | Swagger UI | Base URL |
|---------|-----------|----------|
| Auth Service | `http://localhost:8081/swagger-ui.html` | `/api/v1/auth` |
| Doctor Service | `http://localhost:8082/swagger-ui.html` | `/api/v1/doctors` |
| Appointment Service | `http://localhost:8083/swagger-ui.html` | `/api/v1/appointments` |
| Medication Service | `http://localhost:3001/api-docs` | `/api/v1/medications` |
| Records Service | `http://localhost:3002/api-docs` | `/api/v1/records` |
| ML Service | `http://localhost:8000/docs` | `/api/ml` |

### Key API Endpoints

```
AUTH
  POST   /api/v1/auth/register/step1     Register (personal details)
  POST   /api/v1/auth/register/step2     Set username
  POST   /api/v1/auth/login              Login (username/email/phone)
  POST   /api/v1/auth/forgot-password    Request password reset OTP
  POST   /api/v1/auth/reset-password     Reset password with OTP
  POST   /api/v1/auth/logout             Logout (invalidate token)

DOCTORS
  GET    /api/v1/doctors                 List all doctors
  GET    /api/v1/doctors/:id             Get doctor profile
  GET    /api/v1/doctors/specialty/:name  Filter by specialty
  GET    /api/v1/doctors/search?q=       Search doctors
  GET    /api/v1/doctors/top-rated       Top rated doctors

APPOINTMENTS
  POST   /api/v1/appointments            Book appointment
  GET    /api/v1/appointments/upcoming   Upcoming appointments
  GET    /api/v1/appointments/past       Past appointments
  DELETE /api/v1/appointments/:id        Cancel appointment

MEDICATIONS
  GET    /api/v1/medications             List user medications
  POST   /api/v1/medications             Add medication
  DELETE /api/v1/medications/:id         Remove medication

RECORDS
  GET    /api/v1/records                 List medical records
  POST   /api/v1/records/upload          Upload record
  GET    /api/v1/records/:id/download    Download record
  DELETE /api/v1/records/:id             Delete record

ML / AI
  POST   /api/ml/symptom-check           Symptom-based disease prediction
  POST   /api/ml/health-risk             Health risk assessment
  GET    /api/ml/recommend-doctors       Doctor recommendations
  POST   /api/ml/chat                    AI health chatbot
```

---

## 🧪 Testing

```bash
# Java unit + integration tests
cd backend-spring/auth-service
mvn test

# Node.js tests
cd backend-node/medication-service
npm test

# E2E tests
cd frontend
npx cypress run

# API collection tests
npx newman run postman/CareConnect.postman_collection.json

# Python ML tests
cd ml-services/prediction-service
pytest
```

---

## 🚢 Deployment

### Docker (Local)
```bash
docker compose -f infrastructure/docker/docker-compose.yml up -d
```

### AWS
- **Backend:** EC2 instances with systemd services
- **Database:** RDS PostgreSQL + MongoDB Atlas
- **Frontend:** Vercel (auto-deploy from GitHub)
- **Storage:** S3 for medical records
- **Monitoring:** CloudWatch + Prometheus + Grafana

---

## 📊 Kafka Event Topics

```
user.registered          → Welcome email, search indexing
user.login               → Audit logging
appointment.booked       → Confirmation email, billing, analytics
appointment.cancelled    → Refund processing, analytics
appointment.reminder     → Push/email reminder
medication.added         → Search indexing
medication.reminder      → Push notification
record.uploaded          → ML classification, search indexing
search.index.update      → Elasticsearch sync
```

---

## 🗄 Database Schema

### PostgreSQL (Relational — ACID)
- `users` — Patient/Doctor/Admin accounts
- `specialties` — 28 medical specialties
- `doctors` — Doctor profiles with ratings
- `doctor_working_hours` — Weekly schedules
- `doctor_ratings` — Patient reviews
- `appointments` — Booking records with status
- `invoices` — Billing records

### MongoDB (Document — Flexible)
- `medications` — Medication reminders with flexible dosage formats
- `medical_records` — Record metadata with file references
- `notifications` — User notification history
- `chat_messages` — AI chatbot conversation history

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'feat: add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Commit Convention
```
feat:     New feature
fix:      Bug fix
docs:     Documentation update
style:    Code style (formatting, no logic change)
refactor: Code restructuring
test:     Adding/updating tests
chore:    Build/config changes
```

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [Spring Boot](https://spring.io/projects/spring-boot) — Enterprise Java framework
- [Next.js](https://nextjs.org/) — React framework for production
- [Apache Kafka](https://kafka.apache.org/) — Distributed event streaming
- [LangChain](https://python.langchain.com/) — LLM application framework
- [Groq](https://groq.com/) — Free Llama 3 API inference
- [Docker](https://www.docker.com/) — Containerization platform

---

<p align="center">
  Built with ❤️ as a full-stack learning project
</p>
<p align="center">
  <strong>⭐ Star this repo if you find it useful!</strong>
</p>
