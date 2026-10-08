# ClaimPilot AI — Autonomous Claims Adjudication Suite

ClaimPilot AI is a production-grade autonomous motor insurance claim intake, multi-agent cost reconciliation, and instant document & photo verification engine. It uses a microservice architecture powered by specialized AI agents to automate the end-to-end motor insurance claim adjudication process.

## 🌟 Key Features

- **Multi-Agent Orchestration**: Specialized microservices for document verification, image analysis, and cost estimation.
- **Instant Document Verification**: Automated OCR and verification for RC, DL, and claim forms.
- **Visual Damage Assessment**: AI-powered analysis of vehicle images to assess damage severity.
- **Cost Reconciliation Engine**: Calculates recommended payouts using IRDAI standards via a dedicated Spring Boot/Python engine.
- **High-Throughput Custom Auth**: Scalable JWT-based authentication system backed by PostgreSQL/Supabase.
- **Dynamic Web Interface**: Modern, responsive frontend built with Tailwind CSS and Glassmorphic design principles.

## 🏗️ Architecture

ClaimPilot AI consists of four primary microservices running locally:

1. **Orchestrator & Web Gateway (Port 8000)**: Coordinates communication between agents and serves the web frontend. Built with Python & FastAPI (`orchestrator`).
2. **Document Verification Agent (Port 8001)**: Processes and verifies uploaded documents (RC, DL, forms). Built with Python & FastAPI (`document_agent`).
3. **Vehicle Image Damage Agent (Port 8002)**: Analyzes incident photos for damage assessment. Built with Python & FastAPI (`image_agent`).
4. **Cost Agent (Port 8082)**: Calculates repair costs and determines the recommended settlement. Available as a Java Spring Boot microservice (`cost-agent-spring-ai`) with a deterministic Python fallback.

## 📂 Project Structure

```text
claimcopilot/
├── orchestrator/           # Main API Gateway & Orchestration service (FastAPI)
├── document_agent/         # OCR & Document Verification service (FastAPI)
├── image_agent/            # Computer Vision service for damage assessment (FastAPI)
├── cost-agent-spring-ai/   # Cost calculation & IRDAI compliance engine (Java/Spring Boot)
├── css/ & js/              # Frontend assets for the Web UI
├── index.html              # Main web interface entrypoint
├── run_local.ps1           # Windows PowerShell launcher for all services
├── run_local.bat           # Windows Batch launcher for all services
├── start.sh                # Unix/Linux shell launcher
├── DATA_NEEDS.md           # Database schema and technical specs
└── DB_CONNECTION_GUIDE.md  # Database connection instructions
```

## 🚀 Getting Started

### Prerequisites
- Python 3.9+
- Java 17+ (If compiling/running the Java Spring Boot Cost Agent)
- PostgreSQL / Supabase account (See `DB_CONNECTION_GUIDE.md`)

### Installation & Setup

1. **Clone the repository and set up a virtual environment**:
   ```bash
   python -m venv .venv
   ```
2. **Activate the virtual environment**:
   - **Windows**: `.venv\Scripts\activate`
   - **Linux/Mac**: `source .venv/bin/activate`

3. **Install Dependencies**:
   *(Assuming requirements.txt exists in the agent directories)*
   ```bash
   pip install -r orchestrator/requirements.txt
   pip install -r document_agent/requirements.txt
   pip install -r image_agent/requirements.txt
   ```

### Running Locally

You can launch the entire suite of microservices with a single script.

**For Windows (PowerShell):**
```powershell
.\run_local.ps1
```

**For Windows (Command Prompt):**
```cmd
run_local.bat
```

**For macOS/Linux:**
```bash
./start.sh
```

This will start all four microservices and automatically open the ClaimPilot AI web application in your browser at `http://127.0.0.1:8000`.

## 🗄️ Database & Schema

ClaimPilot AI uses a custom, high-throughput PostgreSQL schema with the following core entities:
- **`users`**: Manages both `claimant` and `admin` (Surveyor) roles with secure bcrypt hashing and stateless JWT bearer tokens.
- **`policies`**: Stores motor insurance policies, vehicle details (Make, Model, IDV), and coverage dates.
- **`claims`**: The core adjudication ledger tracking incident details, estimated costs, recommended payouts, and review status.
- **`claim_documents`**: Verification logs for all OCR processing and document uploads.

For the exact table definitions and JWT payload structure, refer to [DATA_NEEDS.md](./DATA_NEEDS.md).

## 🛑 Stopping the Services

To stop all local services, return to the terminal where you launched the run script and press `Ctrl+C`. Background agents will be automatically terminated.
