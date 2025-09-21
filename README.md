# 🚀 AI Analyst for Startup Evaluation

> An AI-powered platform that evaluates startups by synthesizing founder materials (pitch decks, call transcripts) and public data to generate concise, actionable investment insights.



## 🎯 Vision

Investors face a deluge of unstructured startup data—pitch decks, call transcripts, founder updates—making manual analysis slow, inconsistent, and error-prone. This project builds an **AI Analyst** using Google's Gemini models to automate the evaluation process, cut through the noise, and produce scalable, data-driven investment insights.

## ✨ Core Capabilities

### 📄 **Ingest & Structure**
Process various documents (PDFs, TXT files) via the `ingestion-service` to extract and structure key deal information into a consistent format.

### 💾 **Persist**
The `deal-management-service` saves, retrieves, and manages structured startup data in a Firestore database.

### 📊 **Benchmark**
Compare a startup's key metrics (Valuation, Revenue, Hiring Velocity) against industry peers from a BigQuery dataset.

### ❗ **Risk Analysis**
Automatically identify and flag potential red flags, such as missing metrics, high burn rates, or extreme valuations.

### 🧠 **Synthesize & Recommend**
Generate a final investment recommendation (`Fund`, `Review`, `Pass`) based on a weighted analysis of the team, product, market, and financials.

## 🏛️ Architecture

This platform uses a modular **microservice architecture**, where each core function is an independent, container-ready service. This design makes the platform scalable, maintainable, and resilient.

### Services Overview

| Service | Port | Purpose |
|---------|------|---------|
| **Frontend Service** | 8080 | Vanilla HTML/CSS/JS single-page application for user interface |
| **Ingestion Service** | 8000 | Accepts file uploads, uses Gemini to extract structured data |
| **Deal Management Service** | 8001 | CRUD operations on startup data using Google Firestore |
| **Benchmarking Service** | 8002 | Queries BigQuery for peer data and generates comparative analysis |
| **Synthesis Service** | 8003 | Generates final AI-powered investment recommendations |

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Backend Framework** | Python / FastAPI | High-performance, asynchronous microservices |
| **AI & Machine Learning** | Google Gemini (via Vertex AI) | Core LLM for document parsing, reasoning, and synthesis |
| **Databases** | Google Firestore, Google BigQuery | NoSQL for deal data and data warehouse for peer metrics |
| **Frontend** | HTML, TailwindCSS, JavaScript | Dynamic and responsive user interface |
| **Shared Code** | `core-library` (Python Package) | Centralized Pydantic schemas and prompts |
| **Deployment (Planned)** | Docker, Google Cloud Run | Containerizing and deploying services |

## 🚀 Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- **Python 3.10+**
- **Google Cloud SDK**: Authenticated with a project that has Vertex AI, Firestore, and BigQuery APIs enabled
- **Firebase Service Account Key**: A `serviceAccountKey.json` file for the `deal-management-service`
- **(Optional) `uv`**: A fast Python package installer. If not used, you can use `pip`

### 1. Clone the Repository

```bash
git clone https://github.com/YourUsername/ai-analyst-platform.git
cd ai-analyst-platform
```

### 2. Google Cloud Authentication

Ensure your local environment is authenticated to Google Cloud:

```bash
gcloud auth application-default login
gcloud config set project YOUR_GOOGLE_CLOUD_PROJECT_ID
```

### 3. Service Account Key Setup

1. Go to your Firebase project settings → Service accounts
2. Generate a new private key
3. Save the downloaded JSON file as `serviceAccountKey.json` inside the `services/deal-management-service/` directory

### 4. Install Dependencies

Each service has its own dependencies. You'll need to open a separate terminal for each service you intend to run.

For each service directory (`ingestion-service`, `deal-management-service`, `benchmarking-service`, `synthesis-service`):

```bash
# Navigate to a service directory (e.g., ingestion-service)
cd services/ingestion-service

# Create a virtual environment (recommended)
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies from requirements.txt
pip install -r requirements.txt

# Install the shared core-library in editable mode
pip install -e ../../core-library
```

> **Note**: Repeat the installation process for all services in their respective terminals.

## ▶️ Running the Application

Each microservice and the frontend must be run in its own terminal window.

### Start All Services

#### 1. Ingestion Service (Port 8000)
*Main entry point for the frontend*
```bash
cd services/ingestion-service
uvicorn main:app --reload --port 8000
```

#### 2. Deal Management Service (Port 8001)
```bash
cd services/deal-management-service
uvicorn main:app --reload --port 8001
```

#### 3. Benchmarking Service (Port 8002)
```bash
cd services/benchmarking-service
uvicorn main:app --reload --port 8002
```

#### 4. Synthesis Service (Port 8003)
```bash
cd services/synthesis-service
uvicorn main:app --reload --port 8003
```

#### 5. Frontend Service (Port 8080)
```bash
cd services/frontend-services
python -m http.server 8080
```

### Access the Application

Once all services are running, open your browser and navigate to:
**`http://localhost:8080`**

## 🧪 Testing

Each FastAPI service includes auto-generated interactive API documentation (Swagger UI).

1. Run the service you wish to test (e.g., `ingestion-service`)
2. Open your web browser to the service's `/docs` endpoint:
   - Ingestion Service: **`http://127.0.0.1:8000/docs`**
   - Deal Management: **`http://127.0.0.1:8001/docs`**
   - Benchmarking: **`http://127.0.0.1:8002/docs`**
   - Synthesis: **`http://127.0.0.1:8003/docs`**
3. Use the interactive interface to send requests and view live responses

## 📁 Project Structure

```
ai-analyst-platform/
├── services/                           # Microservices directory
│   ├── ingestion-service/              # Document processing & data extraction (Port 8000)
│   ├── deal-management-service/        # Manages deals in Firestore (Port 8001)
│   ├── benchmarking-service/           # Compares startup to peers from BigQuery (Port 8002)
│   ├── synthesis-service/              # Generates final scores & recommendations (Port 8003)
│   └── frontend-services/              # User interface (HTML, CSS, JS) (Port 8080)
├── core-library/                       # Shared Python library
│   ├── core/
│   │   ├── prompts.py                 # Centralized Gemini prompt templates
│   │   └── schemas.py                 # Pydantic models for consistent data structures
│   └── setup.py                       # Makes the library installable
├── .gitignore                         # Git ignore file
└── README.md                          # This file
```
