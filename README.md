<div align="center">
    <img alt="Logo" src="docs/icon.png" width="100" />
</div>
<h1 align="center">
    Ai Canvas - Cursor for 3D Modeling
</h1>
<p align="center">
   turn your roughest sketches into stunning 3D worlds by Ai drawing
</p>


![2D Canvas](docs/canvas.jpeg)

![3D World](docs/world.jpeg)

## How It Works

1. **Sketch**: Draw freely on the 2D canvas
2. **Enhance**: Use the "Improve Drawing" button to refine sketches into detailed, polished drawings
3. **Transform**: Click "Make 3D" to convert your drawing into a 3D model
4. **Build**: Add your 3D models to the world by switching to the 3D World tab
5. **Iterate**: Edit and refine your 3D models by sketching or by writing a text prompt
6. **Export**: Export your 3D world with 1 click in a standard format (.glTF) to integrate with your pre-existing tooling
##Digram
flowchart TB

  subgraph "Frontend Layer"
    React["Next.js + React App"]:::frontend
    TLDraw["TLDraw Canvas (2D Sketch UI)"]:::frontend
    ThreeJS["Three.js 3D Viewport"]:::frontend
    Zustand["Zustand Store"]:::frontend
    Network["Network Module (REST + SSE)"]:::frontend
  end

  subgraph "API Layer"
    FastAPI["FastAPI Server"]:::api
    Routes["API Routes (/improve-drawing, /make-3d, /export)"]:::api
    Config["Config & Dependency Injection"]:::api
  end

  subgraph "Asynchronous Processing Layer"
    CeleryApp["Celery Broker & Backend (Redis)"]:::worker
    Tasks["Worker Processes (AI Task Definitions)"]:::worker
  end

  subgraph "Messaging Bus"
    Redis["Redis Instance (Broker + Pub/Sub)"]:::messaging
  end

  subgraph "External AI Services"
    Claude["Claude API"]:::external
    Gemini["Gemini API"]:::external
    Cerebras["Cerebras API"]:::external
    PiAPI["PiAPI"]:::external
  end

  %% Data Flows
  React -->|"REST Call"| FastAPI
  Network --> FastAPI
  FastAPI -->|"Enqueue Task"| CeleryApp
  CeleryApp --> Tasks
  Tasks -->|..invoke..| Claude
  Tasks -->|..invoke..| Gemini
  Tasks -->|..invoke..| Cerebras
  Tasks -->|..invoke..| PiAPI
  Tasks -->|-- Pub/Sub --| Redis
  FastAPI -->|-- SSE Subscribe --| Redis
  Redis -->|-- SSE Stream --| Network
  FastAPI -->|"HTTP Response"| Network
  Network --> React

  %% UI State Flows
  React --> Zustand
  Zustand --> TLDraw
  Zustand --> ThreeJS

  %% Click Events
  click React "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/page.tsx"
  click React "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/layout.tsx"
  click TLDraw "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/tldraw.css"
  click TLDraw "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/components/TldrawLogo.tsx"
  click ThreeJS "https://github.com/ankitsingh-git2/ai-canvas/tree/main/frontend/app/components/three/"
  click Zustand "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/store/appStore.ts"
  click Network "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/lib/improveDrawing.tsx"
  click Network "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/lib/vibe3DCode.tsx"
  click Network "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/lib/blobToBase64.ts"
  click Network "https://github.com/ankitsingh-git2/ai-canvas/blob/main/frontend/app/lib/downloadDataUrlAsFile.ts"
  click FastAPI "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/main.py"
  click FastAPI "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/run.py"
  click Routes "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/api/routes.py"
  click Routes "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/api/models.py"
  click Config "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/core/config.py"
  click Config "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/core/redis.py"
  click CeleryApp "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/core/celery_app.py"
  click Tasks "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/tasks/cerebras_tasks.py"
  click Tasks "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/tasks/claude_tasks.py"
  click Tasks "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/tasks/gemini_tasks.py"
  click Tasks "https://github.com/ankitsingh-git2/ai-canvas/blob/main/backend/app/tasks/tasks.py"

  %% Styles
  classDef frontend fill:#D0E8FF,stroke:#333,stroke-width:1px
  classDef api fill:#D5F5E3,stroke:#333,stroke-width:1px
  classDef worker fill:#FFE5B4,stroke:#333,stroke-width:1px
  classDef messaging fill:#FFF3A3,stroke:#333,stroke-width:1px
  classDef external fill:#D3D3D3,stroke:#333,stroke-width:1px

## Quick Start


### Prerequisites

- Node.js 18+
- Python 3.10+
- API keys for Claude, Gemini, Cerebras, and PiAPI

### Frontend Setup

```bash
cd frontend

npm install

npm run dev
```

### Backend Setup

```bash
cd backend

# remember to add api keys
cp .env.example .env

docker compose up
```

## Architecture

### Frontend

- **Next.js & React**: Responsive, user-friendly UI
- **Three.js**: Rendering interactive 3D models
- **TLDraw**: Powerful 2D drawing canvas
- **Zustand**: State management

### Backend

- **FastAPI**: High-performance API framework
- **Celery**: Asynchronous task queue for AI operations
- **Redis**: Pub/Sub for real-time updates and task result storage
- **SSE (Server-Sent Events)**: Real-time progress updates



