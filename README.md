# AI Analyst for Startup Evaluation

An AI-powered platform that evaluates startups by synthesizing founder materials (pitch decks, call transcripts) and public data to generate concise, actionable investment insights.

🚀 Vision
---
Investors face a deluge of unstructured startup data—pitch decks, call transcripts, founder updates—making manual analysis slow, inconsistent, and error-prone. This project builds an **AI Analyst** using Google's Gemini models to automate the evaluation process, cut through the noise, and produce scalable, data-driven investment insights.

## ✨ Core Capabilities

*   **📄 Ingest & Structure**: Process various documents (PDFs, TXT files) via the `ingestion-service` to extract and structure key deal information into a consistent format.
*   **💾 Persist**: The `deal-management-service` saves, retrieves, and manages structured startup data in a Firestore database.
*   **📊 Benchmark**: Compare a startup's key metrics (Valuation, Revenue, Hiring Velocity) against industry peers from a BigQuery dataset.
*   **❗ Risk Analysis**: Automatically identify and flag potential red flags, such as missing metrics, high burn rates, or extreme valuations.
*   **🧠 Synthesize & Recommend**: Generate a final investment recommendation (`Fund`, `Review`, `Pass`) based on a weighted analysis of the team, product, market, and financials.

🏛️ Architecture
---
This platform uses a modular **microservice architecture**, where each core function is an independent, container-ready service. This design makes the platform scalable, maintainable, and resilient.

1.  **Frontend Service**: A vanilla HTML/CSS/JS single-page application that serves as the user interface for uploading documents and viewing analysis results.
2.  **Ingestion Service**: A FastAPI service that accepts file uploads, uses Gemini to extract structured data, and then calls the Deal Management Service to save the results.
3.  **Deal Management Service**: A FastAPI service responsible for CRUD operations on startup data, using Google Firestore as its database.
4.  **Benchmarking Service**: A FastAPI service that queries a BigQuery database for peer data and uses Gemini to generate comparative analysis and identify risks.
5.  **Synthesis Service**: A FastAPI service that takes structured data and investor-defined weights to generate a final, AI-powered investment recommendation.

🛠️ Technology Stack
| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Backend Framework** | Python / FastAPI | For creating high-performance, asynchronous microservices. |
| **AI & Machine Learning** | Google Gemini (via Vertex AI) | The core LLM for document parsing, reasoning, and synthesis. |
| **Databases** | Google Firestore, Google BigQuery | NoSQL for deal data and a data warehouse for peer metrics. |
| **Frontend** | HTML, TailwindCSS, JavaScript | A dynamic and responsive user interface. |
| **Shared Code** | `core-library` (Python Package) | Centralizes Pydantic schemas and prompts for consistency. |
| **Deployment (Planned)**| Docker, Google Cloud Run | Containerizing and deploying services in a serverless environment. |

🏁 Getting Started
---
Follow these instructions to set up and run the project locally.

### Prerequisites

*   **Python 3.10+**
*   **Google Cloud SDK**: Authenticated with a project that has Vertex AI, Firestore, and BigQuery APIs enabled.
*   **Firebase Service Account Key**: A `serviceAccountKey.json` file for the `deal-management-service`.
*   **(Optional) `uv`**: A fast Python package installer. If not used, you can use `pip`.

### 1. Clone the Repository

```bash
git clone https://github.com/YourUsername/ai-analyst-platform.git
cd ai-analyst-platform
```

### 2. Google Cloud Authentication

Ensure your local environment is authenticated to Google Cloud. This allows the services to use the Vertex AI and BigQuery SDKs.

```bash
gcloud auth application-default login
gcloud config set project YOUR_GOOGLE_CLOUD_PROJECT_ID
```

### 3. Service Account Key

1.  Go to your Firebase project settings -> Service accounts.
2.  Generate a new private key.
3.  Save the downloaded JSON file as `serviceAccountKey.json` inside the `services/deal-management-service/` directory.

### 4. Install Dependencies

Each service has its own dependencies. You will need to open a separate terminal for each service you intend to run.

For each service directory (`ingestion-service`, `deal-management-service`, `benchmarking-service`, `synthesis-service`):

```bash
# Navigate to a service directory (e.g., ingestion-service)
cd services/ingestion-service

# Create a virtual environment (recommended)
python -m venv .venv
source .venv/bin/activate  # On Windows use `.venv\Scripts\activate`

# Install dependencies from requirements.txt
pip install -r requirements.txt

# Install the shared core-library in editable mode
pip install -e ../../core-library
```

> **Note**: Repeat the installation process for `deal-management-service`, `benchmarking-service`, and `synthesis-service` in their respective terminals.

## ▶️ How to Run the Project

Each microservice and the frontend must be run in its own terminal window.

#### 1. Run the Ingestion Service (Port 8000)

This is the main entry point for the frontend.

```bash
# In the services/ingestion-service directory
uvicorn main:app --reload --port 8000
```

#### 2. Run the Deal Management Service (Port 8001)

```bash
# In the services/deal-management-service directory
uvicorn main:app --reload --port 8001
```

#### 3. Run the Benchmarking Service (Port 8002)

```bash
# In the services/benchmarking-service directory
uvicorn main:app --reload --port 8002
```

#### 4. Run the Synthesis Service (Port 8003)

```bash
# In the services/synthesis-service directory
uvicorn main:app --reload --port 8003
```

#### 5. Run the Frontend (Port 8080)

The simplest way to serve the frontend is with Python's built-in HTTP server.

```bash
# In the services/frontend-services directory
python -m http.server 8080
```

Once all services are running, open your browser and navigate to **`http://localhost:8080`**.

## 🧪 How to Test

Each FastAPI service includes auto-generated interactive API documentation (Swagger UI).

1.  Run the service you wish to test (e.g., `ingestion-service`).
2.  Open your web browser to the service's `/docs` endpoint (e.g., **`http://127.0.0.1:8000/docs`**).
3.  Use the interactive interface to send requests and view live responses.

📁 Project Structure/ai-analyst-platform/
|
|--- services/                  # Each microservice is a separate application
|    |--- ingestion-service/       # Handles document processing and data extraction (Port 8000)
|    |--- deal-management-service/ # Manages deals in Firestore (Port 8001)
|    |--- benchmarking-service/    # Compares startup to peers from BigQuery (Port 8002)
|    |--- synthesis-service/       # Generates final scores and recommendations (Port 8003)
|    `--- frontend-services/       # The user interface (HTML, CSS, JS) (Port 8080)
|
|--- core-library/              # A shared Python library for all backend services
|    |--- core/
|    |    |--- prompts.py       # Centralized Gemini prompt templates
|    |    `--- schemas.py       # Pydantic models for consistent data structures
|    `--- setup.py             # Makes the library installable
|
|--- .gitignore                 # Specifies intentionally untracked files to ignore
|
`--- README.md                  # This file

