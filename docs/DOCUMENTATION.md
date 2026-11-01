# 🎓 AI Study Buddy — Comprehensive System Documentation & Technical Blueprint

[![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-009688.svg?style=flat&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18.3+-61DAFB.svg?style=flat&logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9+-3178C6.svg?style=flat&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.4+-38B2AC.svg?style=flat&logo=tailwind-css&logoColor=white)](https://tailwindcss.com)
[![PostgreSQL](https://img.shields.io/badge/Neon_PGVector-Serverless-00E699.svg?style=flat&logo=postgresql&logoColor=black)](https://neon.tech)
[![Python](https://img.shields.io/badge/Python-3.11+-3776AB.svg?style=flat&logo=python&logoColor=white)](https://www.python.org)

An enterprise-grade, privacy-first **Cognitive Scholar & Multi-Modal RAG Platform**. Transforms research papers, university textbooks, lecture recordings, and syllabus monographs into isolated vector memories, diagnostic active-recall quizzes, Socratic audio dialogues, examination papers, and technical schematics.

---

## 🏛️ System Architecture Diagram

Below is the full system architecture generated for the platform, illustrating the five operational tiers and bidirectional data flows:

![AI Study Buddy System Architecture](assets/ai_study_buddy_architecture.jpg)

### Interactive Mermaid Architecture Blueprint

```mermaid
graph TD
    subgraph Client_Layer["🖥️ Client Layer (React 18 + Vite SPA)"]
        UI["Web Dashboard & Workspace"]
        AuthModal["JWT Auth & Session Manager"]
        ChatUI["RAG Chat & Document Citations"]
        QuizUI["Active Recall Quiz & Diagnostic Scoring"]
        TutorUI["Socratic Voice Tutor (Interactive Audio)"]
        ExamUI["Exam Paper & Rubric Generator"]
        SlidesUI["Slide Deck & Presentation Generator"]
        SchematicUI["Visual Schematic & Concept Studio"]
    end

    subgraph Gateway_Layer["⚡ API Gateway & Core Server (FastAPI)"]
        CORS["CORS Middleware (Vercel & Cloudflare safe)"]
        AuthRouter["/api/auth (PBKDF2-HMAC-SHA256 & JWT)"]
        UploadRouter["/api/upload (Multi-Format Ingestion & Deduplication)"]
        RAGRouter["/api/chat & /api/quiz & /api/slides"]
        AudioRouter["/api/audio (Edge-TTS & gTTS Voice Streamer)"]
        ImageRouter["/api/image (OpenRouter Turbo & Pollinations Flux)"]
        ModelsRouter["/api/models (Runtime Provider Switcher)"]
    end

    subgraph Processing_Layer["⚙️ Ingestion & RAG Orchestration"]
        Parser["Document Extractors (PDF, DOCX, TXT, OCR, Audio)"]
        Splitter["RecursiveCharacterTextSplitter (1000 Chunk / 200 Overlap)"]
        Deduplicator["Vector Deduplication Guard"]
        ResilienceEngine["3-Layer Resilience Cascade (Vector DB -> Memory Cache -> Direct DB)"]
    end

    subgraph Storage_Layer["🗄️ Database & Vector Store (Neon Serverless)"]
        UserTable["User Accounts (SQLAlchemy)"]
        SessionTable["Study Sessions & Isolated Workspaces"]
        VectorDB[("Neon PostgreSQL + pgvector (3072-Dim Embeddings)")]
        MemoryCache["In-Memory Failover Store"]
    end

    subgraph Inference_Layer["🧠 Multi-Provider AI Inference Hub"]
        GeminiEmbed["Google Gemini Embedding-001 (3072 dims)"]
        GeminiLLM["Google Gemini 2.5 Flash"]
        NvidiaNIM["NVIDIA NIM (Nemotron 3.5 Bolt)"]
        OpenRouterLLM["OpenRouter (LLaMA 3.3 70B, DeepSeek Chat)"]
        EdgeTTS["Microsoft Edge Neural TTS (Ava, Swara, Neerja)"]
        GeminiSTT["Gemini Multimodal Audio (STT)"]
    end

    UI -->|HTTPS / REST API| CORS
    CORS --> AuthRouter
    CORS --> UploadRouter
    CORS --> RAGRouter
    CORS --> AudioRouter
    CORS --> ImageRouter
    CORS --> ModelsRouter

    UploadRouter --> Parser
    Parser --> Splitter
    Splitter --> Deduplicator
    Deduplicator --> GeminiEmbed
    GeminiEmbed --> VectorDB
    Deduplicator --> MemoryCache

    RAGRouter --> ResilienceEngine
    ResilienceEngine -->|1. Vector Search| VectorDB
    ResilienceEngine -->|2. Fast Cache| MemoryCache
    ResilienceEngine -->|3. Table Scan Fallback| VectorDB

    ResilienceEngine --> GeminiLLM
    ResilienceEngine --> NvidiaNIM
    ResilienceEngine --> OpenRouterLLM

    AudioRouter --> GeminiSTT
    AudioRouter --> EdgeTTS
```

---

## 📋 Architectural Tiers & Component Breakdown

### 1. Client Layer (Frontend)
- **Framework**: React 18.3+ with TypeScript 5.9+ and Vite 7.2.
- **Styling & Design System**: Tailwind CSS with custom academic palette (`#FAF8F5` light canvas, `#050506` dark OLED background, and Lucide React icons).
- **State & Context Management**:
  - `AuthContext`: Tracks JWT authentication tokens, email persistence, and anonymous guest sessions.
  - `ThemeContext`: Handles smooth dark/light mode toggling with instant CSS variable re-binding and audio chime feedback.
- **Key Modules**:
  - `Summary.tsx`: Markdown rendering with executive briefs, chapter taxonomy, and one-click clipboard/file export.
  - `Chat.tsx`: Conversational RAG with real-time vector chunk source and page citations.
  - `Quiz.tsx`: Interactive Bloom's Taxonomy test suite with diagnostic weak-spot detection.
  - `Teacher.tsx`: Voice-enabled Socratic audio tutor with interactive scrub bar, speed modulation (1x, 1.25x, 1.5x), and trilingual support (English, Hindi, Hinglish).
  - `SamplePaper.tsx`: Previous Year Question (PYQ) pattern upload and structured mock exam synthesis with Microsoft Word (`.docx`) download.
  - `SlidesGenerator.tsx`: Topic-driven slide deck outline synthesizer with downloadable Microsoft PowerPoint (`.pptx`) decks.
  - `ImageGenerator.tsx`: High-resolution concept diagram and schematic studio with fullscreen lightbox viewer.
  - `ModelsPage.tsx`: Live latency monitor and active provider switcher.

### 2. API Gateway & Backend Engine
- **Framework**: FastAPI (Python 3.11+) powered by Uvicorn.
- **CORS Handling**: Dynamic origin resolution supporting wildcard credentials (`allow_origin_regex=r"^https?://.*"`) for seamless cross-origin operations between Vercel, Cloudflare, and Render.
- **Lifespan Startup**: Automatically initializes database models and verifies the `pgvector` extension upon container launch.
- **Health Checks**: Zero-downtime health probing at `/health` and `/api/health`.

### 3. Data Ingestion & Deduplication Pipeline
- **Supported File Formats**:
  - **PDF Documents**: Extracted via `pypdf` with page-level delimiter tracking (`[Page X of Y]`).
  - **Microsoft Word (`.docx`)**: Structured text and heading extraction via `python-docx`.
  - **Plain Text / Code**: UTF-8 ingestion for `.txt`, `.md`, `.csv`, `.tsv`, `.json`, `.py`, `.ts`, `.js`.
  - **Images & Schematics**: High-accuracy OCR and visual breakdown via Google Gemini Multimodal Vision API.
  - **Audio Lectures**: Audio-to-text transcription via Google Gemini Multimodal Audio.
- **Smart Vector Deduplication**:
  - Queries existing document signatures in PGVector before chunking.
  - Skips redundant embedding API calls if a document with the same filename was previously vectorized in the active session.
  - Auto-names new study sessions from the first ingested file name.

### 4. Vector Storage & 3-Tier Resilience Engine
- **Vector Database**: Neon Serverless PostgreSQL with `pgvector` extension.
- **Embedding Model**: Google Gemini `models/gemini-embedding-001` producing 3072-dimensional vector representations.
- **3-Layer Retrieval Fallback Cascade**:
  1. **Tier 1 (Vector Distance Search)**: Performs cosine distance ranking across metadata-filtered session embeddings (`cmetadata->>'session_id' = :sid`).
  2. **Tier 2 (In-Memory Hot Store)**: If vector queries experience database timeouts or pool exhaustion, the service immediately retrieves from the local in-memory session cache (`_MEMORY_STORE`).
  3. **Tier 3 (Direct Database Chunk Scan)**: Directly queries raw document chunks from `langchain_pg_embedding` table to guarantee zero-failure RAG generation.

### 5. Multi-Engine Inference Hub & Fallback Cascade
The platform dynamically switches across multiple LLM providers:
1. **Google Gemini 2.5 Flash**: Default high-throughput, low-latency reasoning model.
2. **NVIDIA NIM (Nemotron 3.5 Bolt)**: Free high-speed inference endpoint with accelerated context reasoning.
3. **OpenRouter**: Access to open-weight frontier models including Meta LLaMA 3.3 70B, DeepSeek Chat, and Mistral Small.
4. **AWS Bedrock (Optional)**: Anthropic Claude 3 Haiku enterprise tier.
- **Automatic Fallback**: If the active provider fails, the system automatically falls back sequentially (`Gemini -> NVIDIA -> OpenRouter`).

### 6. Voice Synthesis & Audio Engine
- **Dual-Layer TTS Engine**:
  - **Primary**: Microsoft Edge Neural TTS 7.3.1 (`edge-tts`) producing human-like voice synthesis.
  - **Fallback**: Google Translate TTS (`gTTS`) for zero-latency offline resilience.
- **Multi-Dialect Voice Matrix**:
  - English: `en-US-AvaNeural`
  - Hindi: `hi-IN-SwaraNeural`
  - Hinglish: `en-IN-NeerjaNeural` (natural bilingual Indian academic dialect).

---

## 🗃️ Database Schema & Models

### `users` Table
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `VARCHAR` | Primary Key, UUIDv4 | Unique user identifier |
| `email` | `VARCHAR` | Unique, Indexed, Not Null | User login email |
| `hashed_password` | `VARCHAR` | Not Null | PBKDF2-HMAC-SHA256 password hash |
| `created_at` | `DATETIME` | UTC Timestamp | Account registration timestamp |
| `updated_at` | `DATETIME` | UTC Timestamp | Last update timestamp |

### `study_sessions` Table
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `VARCHAR` | Primary Key, UUIDv4 | Unique session identifier |
| `user_id` | `VARCHAR` | Indexed, Not Null | Owner user identifier |
| `title` | `VARCHAR` | Default: "New Session" | User-editable or auto-named title |
| `created_at` | `DATETIME` | UTC Timestamp | Session initialization timestamp |
| `updated_at` | `DATETIME` | UTC Timestamp | Last modification timestamp |

### `langchain_pg_embedding` (PGVector Internal)
| Column | Type | Description |
| :--- | :--- | :--- |
| `uuid` | `UUID` | Primary key per chunk |
| `collection_id` | `UUID` | Collection link (`study_materials`) |
| `embedding` | `vector(3072)` | Google Gemini 3072-dimensional vector embedding |
| `document` | `TEXT` | Raw text chunk content |
| `cmetadata` | `JSONB` | Metadata holding `{"session_id": "...", "source": "..."}` |

---

## 🔌 API Reference & Endpoints

### Authentication (`/api/auth`)
- `POST /api/auth/register`: Register user account with email & password (min 6 chars).
- `POST /api/auth/login`: Authenticate credentials and receive JWT Bearer token.
- `GET /api/auth/me`: Retrieve current authenticated user profile.

### Sessions (`/api/session`)
- `POST /api/session/create`: Create a new isolated study session (requires JWT).
- `GET /api/session/history`: Fetch all active study sessions for the authenticated user.
- `PUT /api/session/{session_id}`: Rename session title.
- `DELETE /api/session/{session_id}`: Delete session and purge all associated vector embeddings from PGVector.
- `POST /api/session/claim`: Associate guest sessions created prior to login.

### Ingestion & Documents (`/api/upload`)
- `POST /api/upload/`: Upload and process files (multipart/form-data) with deduplication into active session.
- `POST /api/upload/url`: Ingest, scrape, and vectorize web page content.
- `GET /api/quiz/documents/{session_id}`: List all distinct ingested document filenames for a session.

### RAG & Study Tools (`/api/chat`, `/api/quiz`, `/api/slides`, `/api/image`)
- `POST /api/chat/`: Query document memory via RAG with source page citations.
- `POST /api/quiz/summary`: Generate brief executive or detailed hierarchical notes.
- `POST /api/quiz/generate`: Generate calibrated multiple-choice questions (Easy / Medium / Hard).
- `POST /api/quiz/analyze`: Diagnostic scoring and personalized remediation guide.
- `POST /api/quiz/pyq-generator`: Analyze PYQ exam paper patterns and generate customized mock exams.
- `POST /api/quiz/download-paper`: Download structured mock exam paper as `.docx`.
- `POST /api/slides/generate`: Generate and download structured `.pptx` presentation deck.
- `POST /api/image/generate`: Generate educational concept schematic (OpenRouter Qwen Image / Flux / SVG).
- `POST /api/audio/interact`: Socratic voice tutor interaction returning audio base64, transcript, and answer.

### Inference & Model Switcher (`/api/models`)
- `GET /api/models/`: List active models, capabilities, and supported providers.
- `POST /api/models/select`: Select active provider (`gemini`, `nvidia`, `openrouter`) and model at runtime.

---

## 🚀 Local Development & Setup

### 1. Prerequisites
- **Python**: 3.11 or 3.12
- **Node.js**: 18+ (Node 20+ recommended) & npm
- **Database**: PostgreSQL with `pgvector` extension enabled (e.g., [Neon.tech](https://neon.tech))

### 2. Environment Variables Configuration
Create `backend/.env` based on `backend/.env.example`:
```env
PORT=8000
DATABASE_URL=postgresql://user:password@ep-example.neon.tech/neondb?sslmode=require
SECRET_KEY=your_super_secret_jwt_key
CORS_ORIGINS=*

# LLM Selection ("gemini", "nvidia", "openrouter")
LLM_PROVIDER=gemini

# Provider Keys
GEMINI_API_KEY=AIzaSy...
NVIDIA_API_KEY=nvapi-...
OPENROUTER_API_KEY=sk-or-v1-...
```

Create `frontend/.env`:
```env
VITE_API_BASE_URL=http://localhost:8000
```

### 3. Backend Execution
```bash
cd backend
python -m venv venv
# Windows:
venv\Scripts\activate
# Linux/macOS:
source venv/bin/activate

pip install -r requirements.txt
python run.py
```
Backend will start at `http://localhost:8000` with Swagger docs at `http://localhost:8000/docs`.

### 4. Frontend Execution
```bash
cd frontend
npm install
npm run dev
```
Frontend will be accessible at `http://localhost:5173`.

---

## ☁️ Production Deployment

### Backend (Render Web Service)
The repository contains `render.yaml` configured for zero-downtime deployment:
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
- **Health Check Path**: `/health`
- **Runtime**: Python 3.11.9

### Frontend (Vercel)
The repository contains `vercel.json` with SPA routing:
- **Build Command**: `cd frontend && npm install && npm run build`
- **Output Directory**: `frontend/dist`
- **Environment Variable**: `VITE_API_BASE_URL=https://<your-render-service>.onrender.com`

---

## 📄 License
This project is licensed under the MIT License.
