# desktable
Below are the four deliverable documents for DeskTable. They are written as
GitHub-flavored Markdown and can be placed directly in the repository root or docs/
directory.
README.md
DeskTable — AI STEM Tutor
DeskTable is an interactive AI tutor that teaches STEM concepts live on a digital
chalkboard. Students can draw, solve problems, speak to the tutor, and receive real-time
Socratic guidance while the system observes their notebook or desk through a vision
pipeline.
Instead of simply returning answers, DeskTable guides students through reasoning steps,
highlights work on the whiteboard, explains concepts with synthesized voice, and adapts
hints based on handwritten work and verbal responses.
Key Features
● Interactive Chalkboard / Whiteboard — Draw equations, diagrams, graphs, and
annotations using Excalidraw and Fabric.js.
● Socratic AI Tutor — OpenAI-powered reasoning that asks guiding questions rather
than immediately giving final answers.
● Live Voice Interaction — Students can speak naturally; the tutor responds through
streamed text-to-speech audio.
● Desk Vision — MediaPipe and face-api.js support desk, notebook, gesture, and
attention-aware learning experiences.
● Real-Time Collaboration — Socket.IO synchronizes canvas actions, tutor drawing
steps, voice events, and AI responses.
● Lesson Progress — Lessons, attempts, session history, and mastery progress are
persisted through PostgreSQL and SQLAlchemy.
● Subscription Tiers — Pricing, entitlement, and access-tier management for free,
student, and premium plans.
● Admin Observability — System metrics, usage monitoring, and operational health
endpoints.
Tech Stack
Layer Technology Purpose
Backend Python 3.11, FastAPI REST APIs, dependency injection,
async services
Database PostgreSQL or SQLite, SQLAlchemy
2.0 Async
Persistent application data
Migrations Alembic Versioned database schema
migrations
Realtime python-socketio, Redis WebSocket events, rooms, pub/sub
coordination
AI OpenAI API Socratic tutoring, reasoning,
explanations
Vision MediaPipe, face-api.js Desk/notebook perception and
student tracking
Voice Browser speech APIs, backend TTS
service
Speech input and streamed tutor
audio
Frontend Next.js 14 App Router, React 18,
TypeScript
Application UI and routing
State Zustand Lightweight client-side state
management
Canvas Excalidraw, Fabric.js Interactive drawing and whiteboard
rendering
Math KaTeX Mathematical expression rendering
Animation GSAP Chalk-style drawing and tutor
animation effects
Testing PyTest Backend unit and integration tests
Project Structure
desktable/
├── backend/
│ ├── app/
│ │ ├── main.py # FastAPI application entrypoint
│ │ ├── core/
│ │ │ ├── config.py # Settings and environment configuration
│ │ │ ├── security.py # JWT, password hashing, auth utilities
│ │ │ └── dependencies.py # Shared FastAPI dependencies
│ │ ├── db/
│ │ │ ├── base.py # SQLAlchemy declarative base
│ │ │ ├── session.py # Async engine and session factory
│ │ │ └── redis.py # Redis connection management
│ │ ├── modules/
│ │ │ ├── auth/ # Login, registration, JWT management
│ │ │ ├── lessons/ # Lessons, progress, history
│ │ │ ├── ai/ # Socratic reasoning and OpenAI integration
│ │ │ ├── tts/ # Text-to-speech streaming
│ │ │ ├── vision/ # Vision ingestion and analysis
│ │ │ ├── subscription/ # Plans, entitlements, billing hooks
│ │ │ ├── admin/ # Metrics and system monitoring
│ │ │ └── websocket/ # Socket.IO events and agent coordination
│ │ ├── schemas/ # Pydantic request/response models
│ │ └── utils/ # Shared helpers
│ ├── alembic/
│ │ ├── versions/ # Migration files
│ │ └── env.py
│ ├── tests/
│ ├── alembic.ini
│ ├── pyproject.toml
│ ├── requirements.txt
│ └── .env.example
├── frontend/
│ ├── app/ # Next.js App Router pages and layouts
│ ├── components/
│ │ ├── whiteboard/ # Excalidraw / Fabric.js canvas components
│ │ ├── tutor/ # Chat, hints, explanations, voice UI
│ │ ├── vision/ # Camera and MediaPipe integrations
│ │ └── shared/ # Buttons, cards, modals, layout primitives
│ ├── lib/
│ │ ├── api.ts # Typed REST API client
│ │ ├── socket.ts # Socket.IO client
│ │ ├── tts.ts # Audio playback and streaming helpers
│ │ └── vision.ts # MediaPipe / face-api.js helpers
│ ├── stores/
│ │ ├── authStore.ts
│ │ ├── lessonStore.ts
│ │ ├── canvasStore.ts
│ │ ├── tutorStore.ts
│ │ └── visionStore.ts
│ ├── styles/
│ ├── package.json
│ ├── tailwind.config.ts
│ ├── tsconfig.json
│ └── .env.local.example
├── docker-compose.yml
├── Dockerfile.backend
├── Dockerfile.frontend
├── .github/
│ └── workflows/
├── docs/
│ ├── ARCHITECTURE.md
│ ├── API_REFERENCE.md
│ └── DEPLOYMENT.md
└── README.md
Prerequisites
Requirement Minimum Version Notes
Node.js 18+ Required for Next.js 14 frontend
npm 9+ Installed with Node.js
Python 3.11+ Required for FastAPI backend
PostgreSQL 14+ Recommended for local and production use
SQLite 3+ Acceptable for lightweight local development
Redis 6+ Required for Socket.IO coordination and
caching
OpenAI API key — Required for AI tutor functionality
Quickstart
1. Clone the repository
git clone https://github.com/your-org/desktable.git
cd desktable
2. Configure backend environment
cd backend
cp .env.example .env
Update .env with your local database, Redis, JWT, and OpenAI settings.
3. Install backend dependencies
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
4. Run database migrations
alembic upgrade head
5. Start the backend
uvicorn app.main:app --reload
By default, the API is available at:
http://localhost:8000
Interactive API documentation:
http://localhost:8000/docs
http://localhost:8000/redoc
6. Configure frontend environment
cd ../frontend
cp .env.local.example .env.local
Next.js exposes browser-accessible variables only when they are prefixed with
NEXT_PUBLIC_; server-only secrets should remain unprefixed.[nextjs]
7. Install frontend dependencies
npm install
8. Start the frontend
npm run dev
Open:
http://localhost:3000
Environment Variables
Backend — backend/.env
Variable Requi
red
Example Descript
ion
APP_NAME No DeskTable Applicati
on
display
name
ENVIRONMENT Yes local local,
stagin
g, or
produc
tion
DEBUG No true Enables
verbose
logging
and
debug
behavior
API_V1_PREFIX Yes /api/v1 Base
path for
REST
endpoint
s
DATABASE_URL Yes postgresql+asyncpg://postgres:postg
res@localhost:5432/desktable
Async
SQLAlch
emy
databas
e URL
SQLITE_DATABASE
_URL
No sqlite+aiosqlite:///./desktable.db Optional
SQLite
fallback
REDIS_URL Yes redis://localhost:6379/0 Redis
connecti
on URL
JWT_SECRET_KEY Yes change-me-in-production Secret
used to
sign
JWTs
JWT_ALGORITHM Yes HS256 JWT
signing
algorith
m
ACCESS_TOKEN_EX
PIRE_MINUTES
Yes 60 Access-t
oken
lifetime
REFRESH_TOKEN_E
XPIRE_DAYS
Yes 30 Refreshtoken
lifetime
OPENAI_API_KEY Yes sk-... OpenAI
API
credenti
al
OPENAI_MODEL No gpt-4o Default
reasonin
g model
OPENAI_EMBEDDIN
G_MODEL
No text-embedding-3-small Embeddi
ng
model
for
lesson
context
TTS_PROVIDER No openai TTS
provider
identifier
TTS_VOICE No alloy Default
tutor
voice
VISION_MAX_FPS No 5 Maximu
m
process
ed vision
frames
per
second
CORS_ORIGINS Yes http://localhost:3000 Commaseparate
d
allowed
origins
SOCKETIO_PATH Yes /ws/socket.io Socket.I
O
request
path
Frontend — frontend/.env.local
Variable Require
d
Example Description
NEXT_PUBLIC_API_BAS
E_URL
Yes http://localhost:8000
/api/v1
REST API base
URL
NEXT_PUBLIC_SOCKET_
URL
Yes http://localhost:8000 Socket.IO server
origin
NEXT_PUBLIC_SOCKET_
PATH
Yes /ws/socket.io Socket.IO path
NEXT_PUBLIC_APP_NAM
E
No DeskTable UI display name
NEXT_PUBLIC_ENABLE_
VISION
No true Enables camera
and vision features
NEXT_PUBLIC_ENABLE_
VOICE
No true Enables
microphone and
TTS features
NEXT_PUBLIC_SENTRY_
DSN
No https://... Optional frontend
error tracking
Running Tests
Backend tests use PyTest:
cd backend
pytest
Run tests with coverage:
pytest --cov=app --cov-report=term-missing
Useful Commands
Command Location Description
uvicorn app.main:app --reload backend/ Start FastAPI in development
mode
alembic upgrade head backend/ Apply all pending migrations
alembic revision
--autogenerate -m "message"
backend/ Create a new migration
pytest backend/ Run backend tests
npm run dev frontend/ Start Next.js development
server
npm run build frontend/ Create a production frontend
build
npm run lint frontend/ Run frontend lint checks
docker compose up --build Repository
root
Start full local Docker stack
Security Notes
● Never commit .env, .env.local, API keys, JWT secrets, or database credentials.
● Use strong, unique values for JWT_SECRET_KEY in every environment.
● Restrict CORS_ORIGINS to known frontend origins.
● Use HTTPS in production so WebSockets, microphone access, and camera access
work securely.
● Rotate OpenAI credentials and database passwords through your
secret-management system.
License
Specify your project license here, for example:
MIT License
ARCHITECTURE.md
DeskTable Architecture
DeskTable uses a decoupled Next.js frontend and FastAPI backend, connected through
REST APIs for transactional operations and Socket.IO for low-latency realtime tutoring. The
backend coordinates OpenAI reasoning, vision analysis, TTS streaming, Redis-backed
session state, and persistent lesson data.
High-Level Architecture
flowchart TB
subgraph Client["Browser / Student Device"]
UI["Next.js 14 App Router UI"]
Canvas["Excalidraw + Fabric.js Whiteboard"]
Mic["Microphone / Speech Input"]
Camera["Camera / MediaPipe / face-api.js"]
Audio["TTS Audio Player"]
Stores["Zustand Stores"]
end
subgraph Edge["FastAPI ASGI Application"]
REST["REST API /api/v1"]
WS["Socket.IO /ws/socket.io"]
Auth["Auth Module"]
Lessons["Lessons Module"]
AI["AI Module"]
TTS["TTS Module"]
Vision["Vision Module"]
Subs["Subscription Module"]
Admin["Admin Module"]
end
subgraph Services["External Services"]
OpenAI["OpenAI API"]
Redis["Redis"]
DB[("PostgreSQL / SQLite")]
end
UI --> REST
UI --> WS
Canvas --> WS
Mic --> WS
Camera --> WS
Audio --> UI
Stores --> UI
UI --> Canvas
UI --> Mic
UI --> Camera
REST --> Auth
REST --> Lessons
REST --> AI
REST --> TTS
REST --> Vision
REST --> Subs
REST --> Admin
WS --> AI
WS --> TTS
WS --> Vision
WS --> Lessons
AI --> OpenAI
AI --> Redis
TTS --> OpenAI
Vision --> Redis
Lessons --> DB
Auth --> DB
Subs --> DB
Admin --> Redis
Core Design Principles
● Realtime-first tutoring — Canvas strokes, voice events, AI responses, and tutor
drawing actions are streamed through Socket.IO.
● Socratic by default — The AI module is designed to produce questions, partial
hints, and step-by-step guidance rather than direct answers.
● Separation of concerns — Each domain module owns its routes, schemas,
services, and persistence concerns.
● Async throughout — FastAPI, SQLAlchemy, Redis, OpenAI calls, and Socket.IO
handlers use async I/O.
● Ephemeral versus durable state — Active tutoring context lives in Redis;
completed lessons and progress live in the relational database.
● Progressive disclosure — The tutor reveals concepts gradually based on student
responses, canvas activity, and vision context.
Realtime Interaction Pipeline
sequenceDiagram
autonumber
participant S as Student Browser
participant C as Whiteboard Canvas
participant WS as Socket.IO Client
participant API as FastAPI Socket Server
participant AI as AI Service
participant OAI as OpenAI API
participant VIS as Vision Service
participant TTS as TTS Service
participant R as Redis
S->>C: Draws equation or diagram
C->>WS: canvas_draw event
WS->>API: Emit canvas_draw
API->>R: Store latest canvas context / room state
S->>WS: user_voice_input
WS->>API: Emit user_voice_input
API->>AI: Build tutoring context
AI->>OAI: Request Socratic response
alt Vision enabled
S->>WS: vision_frame
WS->>API: Emit vision_frame
API->>VIS: Analyze handwriting / desk context
VIS->>R: Cache frame analysis
VIS-->>API: Vision insight
API->>AI: Enrich prompt with vision context
end
OAI-->>AI: Streaming model response
AI-->>API: Reasoning chunks and drawing plan
API-->>WS: ai_response_chunk
API-->>WS: socratic_hint
API-->>WS: draw_step
WS-->>C: Render tutor drawing animation
API->>TTS: Convert explanation to speech
TTS-->>API: tts_audio_chunk
API-->>WS: tts_audio_chunk
WS-->>S: Play streamed tutor audio
Pipeline Stages
Stage Component Responsibility
Capture Excalidraw / Fabric.js Captures strokes, shapes, equations, and
annotations
Transport Socket.IO client Sends canvas, voice, and vision events to the
backend
Session
coordination
FastAPI WebSocket
module
Validates JWT/session, joins rooms, routes
events
Context
assembly
AI module Combines lesson metadata, canvas state, chat
history, and vision insights
Reasoning OpenAI API Produces streamed Socratic explanations and
structured drawing steps
Perception Vision module Interprets notebook/desk frames and attention
signals
Expression TTS module Streams tutor explanation audio to the browser
Persistence SQLAlchemy Saves lesson sessions, messages, progress,
and completed artifacts
Ephemeral state Redis Stores active room state, rate limits, streams,
and transient AI context
Socket.IO Integration
The Socket.IO ASGI application is mounted alongside FastAPI at /ws/socket.io. A
typical integration wraps the FastAPI app in a socketio.ASGIApp and mounts it under
/ws, with the Socket.IO path configured as /ws/socket.io.[fastapi-practices.github]
# backend/app/main.py
import socketio
from fastapi import FastAPI
from app.modules.websocket.events import register_socket_events
app = FastAPI(title="DeskTable API")
sio = socketio.AsyncServer(
async_mode="asgi",
cors_allowed_origins=settings.CORS_ORIGINS,
)
socket_app = socketio.ASGIApp(
socketio_server=sio,
other_asgi_app=app,
socketio_path="/ws/socket.io",
)
register_socket_events(sio)
app.mount("/ws", socket_app)
The frontend connects using the matching path:
// frontend/lib/socket.ts
import { io } from "socket.io-client";
export const socket = io(process.env.NEXT_PUBLIC_SOCKET_URL!, {
path: process.env.NEXT_PUBLIC_SOCKET_PATH ?? "/ws/socket.io",
transports: ["websocket"],
autoConnect: false,
withCredentials: true,
});
Frontend State Management
Zustand provides focused client-side stores. Each store owns a narrow slice of UI state and
communicates with services through typed API and Socket.IO clients.
Store Owns Updated By
authStore Access token, refresh token,
current user, auth status
REST auth endpoints
lessonStore Active lesson, lesson list, progress,
history
REST lesson endpoints and
Socket.IO session events
canvasStore Canvas elements, selected tools,
undo/redo stack, tutor drawing
queue
Local canvas actions and
draw_step events
tutorStore Chat transcript, streaming
response, hints, tutor status
ai_response_chunk,
socratic_hint,
tts_audio_chunk
visionStore Camera permission, tracking
status, latest analysis, privacy
mode
Local MediaPipe pipeline and
vision_analysis events
subscriptionS
tore
Current plan, entitlements, usage
limits
REST subscription endpoints
Example Zustand Store
// frontend/stores/tutorStore.ts
import { create } from "zustand";
type TutorMessage = {
id: string;
role: "student" | "tutor";
content: string;
createdAt: string;
};
type TutorState = {
messages: TutorMessage[];
isThinking: boolean;
currentHint: string | null;
appendChunk: (chunk: string) => void;
setHint: (hint: string | null) => void;
setThinking: (thinking: boolean) => void;
reset: () => void;
};
export const useTutorStore = create<TutorState>((set) => ({
messages: [],
isThinking: false,
currentHint: null,
appendChunk: (chunk) =>
set((state) => {
const last = state.messages[state.messages.length - 1];
if (!last || last.role !== "tutor") {
return {
messages: [
...state.messages,
{
id: crypto.randomUUID(),
role: "tutor",
content: chunk,
createdAt: new Date().toISOString(),
},
],
};
}
const updated = [...state.messages];
updated[updated.length - 1] = {
...last,
content: last.content + chunk,
};
return { messages: updated };
}),
setHint: (currentHint) => set({ currentHint }),
setThinking: (isThinking) => set({ isThinking }),
reset: () => set({ messages: [], isThinking: false, currentHint: null }),
}));
Backend State Model
State Type Store Lifetime Examples
Identity PostgreSQL /
SQLite
Long-lived Users, credentials, roles
Learning
record
PostgreSQL /
SQLite
Long-lived Lessons, sessions, messages,
progress
Billing state PostgreSQL /
SQLite
Long-lived Plans, subscriptions, entitlements
Active room Redis Minutes to
hours
Room membership, connection IDs,
canvas version
Tutoring
context
Redis Session-scope
d
Recent messages, current problem,
hint level
Stream state Redis Seconds to
minutes
Active AI stream, TTS sequence,
partial transcription
Rate limits Redis Short-lived AI request limits, vision-frame limits
AI Tutoring Flow
flowchart LR
A[Student Action] --> B{Input Type}
B -- Canvas --> C[Canvas Context Extractor]
B -- Voice --> D[Speech / Transcript Handler]
B -- Vision --> E[Vision Analyzer]
B -- Text --> F[Chat Message Handler]
C --> G[Context Builder]
D --> G
E --> G
F --> G
G --> H[Socratic Prompt Chain]
H --> I[OpenAI Streaming Completion]
I --> J{Response Type}
J -- Explanation --> K[ai_response_chunk]
J -- Guided question --> L[socratic_hint]
J -- Board action --> M[draw_step]
J -- Narration --> N[TTS stream]
Socratic Prompt Chain
The AI module builds prompts in layers:
1. System layer — Defines the tutor persona, STEM subject, safety constraints, and
Socratic behavior.
2. Lesson layer — Supplies topic, learning objective, difficulty, prior progress, and
curriculum alignment.
3. Canvas layer — Supplies a compact representation of current drawings, equations,
labels, and selected objects.
4. Vision layer — Supplies privacy-safe observations such as detected handwriting
regions, work-in-progress signals, or attention cues.
5. Conversation layer — Supplies recent student/tutor turns and prior hints.
6. Output contract — Requests separate fields for explanation, next question, hint
level, and optional whiteboard drawing actions.
Example response contract:
{
"explanation": "Let's isolate the unknown variable step by step.",
"socratic_question": "What operation can we apply to both sides to remove the +7?",
"hint_level": 1,
"draw_steps": [
{
"type": "text",
"content": "x + 7 = 12",
"position": { "x": 120, "y": 100 }
},
{
"type": "arrow",
"from": { "x": 180, "y": 130 },
"to": { "x": 240, "y": 130 },
"label": "- 7"
}
],
"tts_text": "We have x plus seven equals twelve. What could we subtract from both sides?"
}
Data Flow Summary
Flow Trigger Path Result
Canvas
collaboration
Student draws Canvas → canvas_draw
→ Socket.IO → Redis →
peers
All participants see
synchronized board
state
Socratic
tutoring
Student asks or
submits work
Socket.IO → AI service →
OpenAI → Socket.IO
Streamed explanation,
hint, and drawing steps
Voice
explanation
AI produces
narration
AI → TTS service →
Socket.IO → browser audio
Tutor speaks while
drawing
Vision-assiste
d help
Camera frame
captured
Browser → vision_frame
→ Vision service → AI
Context-aware
feedback
Lesson
completion
Tutoring session
ends
Socket.IO/REST → Lessons
service → Database
Persisted transcript,
progress, and artifacts
Access
enforcement
API or
WebSocket
request
Auth + subscription modules Allowed or denied
based on identity and
plan
Scalability Considerations
● Run multiple FastAPI/Socket.IO workers behind a load balancer with sticky sessions
or use the Redis message queue for cross-worker Socket.IO events.
● Use Redis pub/sub or Redis Streams for fan-out of AI and TTS events across
horizontally scaled instances.
● Process vision frames at a bounded frame rate and prefer client-side preprocessing
to reduce bandwidth.
● Cache lesson context and repeated AI prompts where appropriate, while avoiding
caching personal student data longer than necessary.
● Use database read replicas for analytics-heavy admin queries.
● Offload long-running AI generation to background workers when response time or
reliability requirements demand it.
API_REFERENCE.md
DeskTable API Reference
All REST endpoints are versioned under:
/api/v1
Unless stated otherwise:
● Requests and responses use application/json.
● Authentication uses a bearer access token.
● Timestamps are ISO 8601 UTC strings.
● IDs are UUID strings.
Authentication Header
Authorization: Bearer <access_token>
Standard Error Format
{
"detail": {
"code": "lesson_not_found",
"message": "The requested lesson does not exist.",
"request_id": "b3f2c7d4-8e91-4f2a-9d44-6d7a1b9f1234"
}
}
HTTP Status Meaning
200 Success
201 Resource created
204 Success with no response body
400 Invalid request
401 Missing or invalid authentication
403 Authenticated but not permitted
404 Resource not found
409 Conflict, such as duplicate
resource
422 Validation error
429 Rate limit exceeded
500 Internal server error
Auth — /api/v1/auth
Handles registration, login, token refresh, profile access, and logout.
Register User
POST /api/v1/auth/register
Request
{
"email": "student@example.com",
"password": "StrongPassw0rd!",
"full_name": "Ada Lovelace",
"grade_level": "grade_10"
}
Response — 201 Created
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"grade_level": "grade_10",
"created_at": "2026-10-14T09:00:00Z"
}
Login
POST /api/v1/auth/login
Request
{
"email": "student@example.com",
"password": "StrongPassw0rd!"
}
Response — 200 OK
{
"access_token": "eyJhbGciOiJIUzI1NiIs...",
"refresh_token": "eyJhbGciOiJIUzI1NiIs...",
"token_type": "bearer",
"expires_in": 3600,
"user": {
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace"
}
}
Refresh Token
POST /api/v1/auth/refresh
Request
{
"refresh_token": "eyJhbGciOiJIUzI1NiIs..."
}
Response — 200 OK
{
"access_token": "eyJhbGciOiJIUzI1NiIs...",
"token_type": "bearer",
"expires_in": 3600
}
Get Current User
GET /api/v1/auth/me
Response — 200 OK
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"grade_level": "grade_10",
"subscription_tier": "premium",
"created_at": "2026-10-14T09:00:00Z"
}
Logout
POST /api/v1/auth/logout
Request
{
"refresh_token": "eyJhbGciOiJIUzI1NiIs..."
}
Response — 204 No Content
Lessons — /api/v1/lessons
Manages lesson planning, tutoring sessions, progress, and history.
List Lessons
GET /api/v1/lessons
Query Parameters
Parameter Type Required Description
subject string No math, physics, chemistry, biology,
computer_science
status string No draft, published, archived
difficul
ty
string No beginner, intermediate, advanced
page integer No Page number, default 1
page_siz
e
integer No Items per page, default 20, maximum 100
Response — 200 OK
{
"items": [
{
"id": "1c3f4b7e-0d21-4c7a-8f10-9d8e7c6b5a43",
"title": "Solving Linear Equations",
"subject": "math",
"difficulty": "beginner",
"summary": "Learn to isolate variables using inverse operations.",
"estimated_minutes": 25,
"status": "published"
}
],
"page": 1,
"page_size": 20,
"total": 42
}
Create Lesson
POST /api/v1/lessons
Request
{
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"summary": "Understand force, mass, and acceleration relationships.",
"learning_objectives": [
"Interpret F = ma",
"Solve for acceleration",
"Apply units correctly"
],
"estimated_minutes": 30
}
Response — 201 Created
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"status": "draft",
"created_at": "2026-10-14T09:12:00Z"
}
Get Lesson
GET /api/v1/lessons/{lesson_id}
Response — 200 OK
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"summary": "Understand force, mass, and acceleration relationships.",
"learning_objectives": [
"Interpret F = ma",
"Solve for acceleration",
"Apply units correctly"
],
"estimated_minutes": 30,
"status": "published"
}
Update Lesson
PATCH /api/v1/lessons/{lesson_id}
Request
{
"title": "Newton's Second Law and Free-Body Diagrams",
"status": "published"
}
Response — 200 OK
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law and Free-Body Diagrams",
"status": "published",
"updated_at": "2026-10-14T09:20:00Z"
}
Delete Lesson
DELETE /api/v1/lessons/{lesson_id}
Response — 204 No Content
Start Tutoring Session
POST /api/v1/lessons/{lesson_id}/sessions
Request
{
"mode": "socratic",
"enable_voice": true,
"enable_vision": true
}
Response — 201 Created
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"status": "active",
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"started_at": "2026-10-14T09:25:00Z"
}
Get Session
GET /api/v1/lessons/sessions/{session_id}
Response — 200 OK
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"status": "active",
"message_count": 8,
"started_at": "2026-10-14T09:25:00Z",
"ended_at": null
}
Complete Session
POST /api/v1/lessons/sessions/{session_id}/complete
Request
{
"outcome": "completed",
"mastery_score": 0.82,
"notes": "Student successfully solved three linear equations."
}
Response — 200 OK
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"status": "completed",
"outcome": "completed",
"mastery_score": 0.82,
"ended_at": "2026-10-14T09:55:00Z"
}
Get Student Progress
GET /api/v1/lessons/progress
Response — 200 OK
{
"overall_mastery": 0.74,
"subjects": [
{
"subject": "math",
"mastery": 0.81,
"lessons_completed": 12
},
{
"subject": "physics",
"mastery": 0.66,
"lessons_completed": 7
}
]
}
AI — /api/v1/ai
Provides direct AI endpoints for non-streaming requests, tutor context creation, and
explanation generation. Most live tutoring traffic should use Socket.IO.
Create Tutor Context
POST /api/v1/ai/context
Request
{
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"problem": "Solve for x: 2x + 5 = 17",
"student_level": "beginner"
}
Response — 201 Created
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"expires_at": "2026-10-14T10:25:00Z"
}
Generate Explanation
POST /api/v1/ai/explain
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"question": "Why do we subtract 5 from both sides?",
"style": "socratic"
}
Response — 200 OK
{
"explanation": "Subtracting 5 keeps the equation balanced while isolating the term
containing x.",
"socratic_question": "What happens to the left side if we subtract 5 from both sides?",
"hint_level": 1,
"tokens_used": 412
}
Generate Hints
POST /api/v1/ai/hints
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"current_answer": "x = 5",
"hint_level": 2
}
Response — 200 OK
{
"hints": [
"First move the constant to the right-hand side.",
"Then divide both sides by the coefficient of x."
],
"next_question": "What should you divide both sides by?"
}
Evaluate Student Work
POST /api/v1/ai/evaluate
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"student_work": "2x + 5 = 17\n2x = 12\nx = 6",
"expected_answer": "x = 6"
}
Response — 200 OK
{
"correct": true,
"score": 1.0,
"feedback": "Correct. Your steps are clear and mathematically valid.",
"misconceptions": []
}
TTS — /api/v1/tts
Converts tutor text into audio. Live sessions normally receive audio through
tts_audio_chunk Socket.IO events, while REST is useful for previews and offline lesson
audio.
Synthesize Speech
POST /api/v1/tts/synthesize
Request
{
"text": "Let's isolate x by subtracting five from both sides.",
"voice": "alloy",
"format": "mp3"
}
Response — 200 OK
Content-Type: audio/mpeg
Returns binary audio content.
List Voices
GET /api/v1/tts/voices
Response — 200 OK
{
"voices": [
{
"id": "alloy",
"name": "Alloy",
"language": "en-US",
"gender": "neutral"
},
{
"id": "nova",
"name": "Nova",
"language": "en-US",
"gender": "female"
}
]
}
Create Streaming TTS Session
POST /api/v1/tts/stream
Request
{
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"text": "We can solve this by using inverse operations.",
"voice": "alloy"
}
Response — 202 Accepted
{
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"status": "streaming",
"transport": "websocket"
}
Audio chunks are delivered through the Socket.IO event tts_audio_chunk.
Vision — /api/v1/vision
Supports image analysis, vision-session configuration, and privacy controls. Camera frames
are normally streamed through Socket.IO; REST endpoints handle configuration, snapshots,
and results.
Create Vision Session
POST /api/v1/vision/sessions
Request
{
"lesson_session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"modalities": ["notebook", "handwriting", "attention"],
"max_fps": 5
}
Response — 201 Created
{
"id": "vis_4e3d2c1b-0a99-4f88-8e77-6d5c4b3a2f10",
"status": "active",
"max_fps": 5,
"privacy_mode": "standard"
}
Analyze Snapshot
POST /api/v1/vision/analyze
Request
Content-Type: multipart/form-data
Field Type Required Description
image file Yes JPEG, PNG, or WebP image
session_
id
string Yes Vision session ID
modaliti
es
string No Comma-separated analysis
modes
Response — 200 OK
{
"analysis_id": "an_1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c5d",
"handwriting_detected": true,
"equation_candidates": ["2x + 5 = 17"],
"work_in_progress": true,
"attention_score": 0.91,
"suggestions": [
"Ask the student to label the operation applied to both sides."
]
}
Update Privacy Mode
PATCH /api/v1/vision/sessions/{vision_session_id}
Request
{
"privacy_mode": "strict",
"store_frames": false
}
Response — 200 OK
{
"id": "vis_4e3d2c1b-0a99-4f88-8e77-6d5c4b3a2f10",
"privacy_mode": "strict",
"store_frames": false
}
Delete Vision Session
DELETE /api/v1/vision/sessions/{vision_session_id}
Response — 204 No Content
Subscription — /api/v1/subscription
Manages available plans, current entitlements, and subscription changes.
List Plans
GET /api/v1/subscription/plans
Response — 200 OK
{
"plans": [
{
"id": "free",
"name": "Free",
"price_monthly": 0,
"currency": "USD",
"features": [
"3 tutoring sessions per week",
"Basic whiteboard",
"Text-based AI hints"
]
},
{
"id": "student",
"name": "Student",
"price_monthly": 9.99,
"currency": "USD",
"features": [
"Unlimited tutoring sessions",
"Voice tutor",
"Desk vision",
"Lesson history"
]
},
{
"id": "premium",
"name": "Premium",
"price_monthly": 19.99,
"currency": "USD",
"features": [
"All Student features",
"Advanced subjects",
"Priority AI capacity",
"Parent progress reports"
]
}
]
}
Get Current Subscription
GET /api/v1/subscription/me
Response — 200 OK
{
"plan_id": "student",
"status": "active",
"current_period_end": "2026-11-14T00:00:00Z",
"entitlements": {
"voice_tutor": true,
"desk_vision": true,
"advanced_subjects": false,
"max_sessions_per_month": 100
}
}
Change Plan
POST /api/v1/subscription/change-plan
Request
{
"plan_id": "premium"
}
Response — 202 Accepted
{
"subscription_id": "sub_8f7e6d5c-4b3a-4928-8172-6f5e4d3c2b1a",
"status": "pending",
"requested_plan": "premium"
}
Cancel Subscription
POST /api/v1/subscription/cancel
Response — 200 OK
{
"subscription_id": "sub_8f7e6d5c-4b3a-4928-8172-6f5e4d3c2b1a",
"status": "canceling",
"effective_at": "2026-11-14T00:00:00Z"
}
Admin — /api/v1/admin
Administrative endpoints require an admin role.
System Health
GET /api/v1/admin/health
Response — 200 OK
{
"status": "healthy",
"database": "healthy",
"redis": "healthy",
"openai": "healthy",
"socketio": "healthy",
"version": "1.0.0"
}
Platform Metrics
GET /api/v1/admin/metrics
Query Parameters
Parameter Type Required Description
from string No ISO 8601 start
timestamp
to string No ISO 8601 end timestamp
granulari
ty
string No hour, day, week
Response — 200 OK
{
"active_users": 1284,
"active_sessions": 187,
"lessons_completed": 431,
"ai_requests": 15230,
"tts_minutes": 942.5,
"vision_frames_processed": 284100,
"error_rate": 0.008
}
List Users
GET /api/v1/admin/users
Query Parameters
Parameter Type Required Description
email string No Filter by email
subscription_
tier
string No Filter by plan
page integer No Page number
page_size integer No Page size
Response — 200 OK
{
"items": [
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"subscription_tier": "premium",
"is_active": true
}
],
"total": 1
}
WebSocket Protocol
Namespace / endpoint:
/ws/socket.io
The Socket.IO server is mounted as an ASGI sub-application alongside FastAPI, allowing
HTTP and WebSocket traffic to share one deployment.[fastapi-practices.github]
Connection
import { io } from "socket.io-client";
const socket = io("http://localhost:8000", {
path: "/ws/socket.io",
auth: {
token: accessToken,
},
transports: ["websocket"],
});
Authentication Payload
Field Type Required Description
token string Yes JWT access token
session_id string No Active tutoring session ID
client_vers
ion
string No Frontend build/version identifier
Common Error Event
Server → Client: socket_error
{
"code": "unauthorized",
"message": "Invalid or expired access token.",
"retryable": false
}
Client-to-Server Events
join_room
Joins a lesson or tutoring room.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"role": "student"
}
Response: room_joined
leave_room
Leaves the current room.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8"
}
Response: room_left
canvas_draw
Sends a drawing action from the student whiteboard.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "draw_01",
"type": "stroke",
"tool": "pen",
"color": "#111111",
"width": 3,
"points": [
{ "x": 120, "y": 240 },
{ "x": 132, "y": 238 },
{ "x": 150, "y": 230 }
],
"timestamp": "2026-10-14T09:30:12.480Z"
}
Server behavior
● Validates the room and user.
● Stores the latest canvas version in Redis.
● Broadcasts canvas_draw to other room participants.
● Optionally forwards canvas context to the AI service.
canvas_clear
Clears the board.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "clear_01"
}
Response: canvas_cleared
canvas_undo
Requests an undo action.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "undo_01"
}
Response: canvas_undo
vision_frame
Sends a camera or desk frame for analysis.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"frame_id": "frame_042",
"format": "jpeg",
"encoding": "base64",
"width": 640,
"height": 480,
"data": "<base64-encoded-image>",
"modalities": ["handwriting", "attention"]
}
Server behavior
● Applies rate limits and privacy policy.
● Sends the frame to the vision pipeline.
● Emits vision_analysis and may enrich the next AI prompt.
user_voice_input
Sends transcribed student speech or raw audio metadata.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"input_id": "voice_001",
"transcript": "I do not understand how to isolate x.",
"audio_duration_ms": 2400,
"language": "en-US"
}
Server behavior
● Adds the utterance to tutoring context.
● Requests a Socratic response from OpenAI.
● Streams ai_response_chunk, socratic_hint, draw_step, and
tts_audio_chunk.
tutor_request
Requests AI help with explicit text.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"message": "Can you explain the next step?",
"mode": "socratic",
"include_canvas": true,
"include_vision": true
}
Response: ai_response_started, ai_response_chunk, ai_response_completed
hint_request
Requests a hint at a specific level.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "hint_001",
"hint_level": 2,
"problem": "Solve for x: 2x + 5 = 17"
}
Response: socratic_hint
tts_request
Requests spoken narration for a specific explanation.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "tts_001",
"text": "Subtract five from both sides to isolate the term with x.",
"voice": "alloy"
}
Response: tts_started, tts_audio_chunk, tts_completed
typing
Indicates that a participant is typing.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"is_typing": true
}
Response: participant_typing
Server-to-Client Events
room_joined
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"role": "student",
"participants": [
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"role": "student"
}
]
}
room_left
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10"
}
participant_joined
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant": {
"id": "f1e2d3c4-b5a6-4789-8123-456789abcdef",
"role": "tutor"
}
}
participant_left
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "f1e2d3c4-b5a6-4789-8123-456789abcdef"
}
participant_typing
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"is_typing": true
}
canvas_draw
Broadcasts a drawing action to room participants.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "draw_01",
"origin": "student",
"type": "stroke",
"tool": "pen",
"color": "#111111",
"width": 3,
"points": [
{ "x": 120, "y": 240 },
{ "x": 132, "y": 238 }
]
}
canvas_cleared
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"origin": "tutor"
}
canvas_undo
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "undo_01",
"origin": "student"
}
ai_response_started
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"model": "gpt-4o",
"mode": "socratic"
}
ai_response_chunk
Streams a partial tutor response.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"chunk_index": 3,
"delta": "Subtract 5 from both sides.",
"done": false
}
ai_response_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"full_response": "We can isolate x by subtracting 5 from both sides, then dividing by 2.",
"tokens_used": 388,
"done": true
}
socratic_hint
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "hint_001",
"hint_level": 2,
"hint": "Move the constant term to the other side first.",
"question": "What operation removes +5 from the left-hand side?"
}
draw_step
Instructs the whiteboard to render a tutor-generated action.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"step_index": 1,
"action": {
"type": "text",
"content": "2x + 5 - 5 = 17 - 5",
"position": { "x": 140, "y": 180 },
"style": {
"font": "chalk",
"color": "#ffffff"
}
},
"animation": {
"type": "chalk_write",
"duration_ms": 900
}
}
vision_analysis
Returns privacy-safe insights from a vision frame.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"frame_id": "frame_042",
"handwriting_detected": true,
"equation_candidates": ["2x + 5 = 17"],
"work_in_progress": true,
"attention_score": 0.91,
"suggestions": [
"Ask the student what operation removes +5."
]
}
tts_started
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"request_id": "tts_001",
"voice": "alloy",
"format": "mp3"
}
tts_audio_chunk
Streams encoded audio for browser playback.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"sequence": 4,
"format": "mp3",
"encoding": "base64",
"data": "<base64-audio-chunk>",
"done": false
}
tts_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"duration_ms": 4200,
"done": true
}
lesson_progress_updated
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"progress": 0.65,
"mastery_score": 0.78
}
session_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"outcome": "completed",
"ended_at": "2026-10-14T09:55:00Z"
}
DEPLOYMENT.md
DeskTable Deployment Guide
This guide covers Docker-based deployment, database migrations, environment
configuration, reverse-proxy setup, and production hardening for DeskTable.
Deployment Topology
flowchart LR
User["Student Browser"] --> LB["HTTPS Load Balancer / Reverse Proxy"]
LB --> FE["Next.js Frontend Container"]
LB --> BE["FastAPI + Socket.IO Backend Container"]
FE --> BE
BE --> DB[("PostgreSQL")]
BE --> RD[("Redis")]
BE --> OAI["OpenAI API"]
subgraph Optional["Optional Production Services"]
OBJ["Object Storage"]
MON["Monitoring / Logging"]
CDN["CDN"]
end
FE --> CDN
BE --> OBJ
BE --> MON
Docker Compose
Example docker-compose.yml
version: "3.9"
services:
db:
image: postgres:16-alpine
container_name: desktable-db
restart: unless-stopped
environment:
POSTGRES_USER: ${POSTGRES_USER}
POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
POSTGRES_DB: ${POSTGRES_DB}
volumes:
- postgres_data:/var/lib/postgresql/data
healthcheck:
test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
interval: 10s
timeout: 5s
retries: 5
networks:
- desktable
redis:
image: redis:7-alpine
container_name: desktable-redis
restart: unless-stopped
command: ["redis-server", "--appendonly", "yes"]
volumes:
- redis_data:/data
healthcheck:
test: ["CMD", "redis-cli", "ping"]
interval: 10s
timeout: 5s
retries: 5
networks:
- desktable
backend:
build:
context: ./backend
dockerfile: ../Dockerfile.backend
container_name: desktable-backend
restart: unless-stopped
depends_on:
db:
condition: service_healthy
redis:
condition: service_healthy
environment:
ENVIRONMENT: production
DEBUG: "false"
API_V1_PREFIX: /api/v1
DATABASE_URL:
postgresql+asyncpg://${POSTGRES_USER}:${POSTGRES_PASSWORD}@db:5432/${PO
STGRES_DB}
REDIS_URL: redis://redis:6379/0
JWT_SECRET_KEY: ${JWT_SECRET_KEY}
JWT_ALGORITHM: HS256
ACCESS_TOKEN_EXPIRE_MINUTES: 60
REFRESH_TOKEN_EXPIRE_DAYS: 30
OPENAI_API_KEY: ${OPENAI_API_KEY}
OPENAI_MODEL: ${OPENAI_MODEL}
CORS_ORIGINS: ${CORS_ORIGINS}
SOCKETIO_PATH: /ws/socket.io
ports:
- "8000:8000"
networks:
- desktable
frontend:
build:
context: ./frontend
dockerfile: ../Dockerfile.frontend
args:
NEXT_PUBLIC_API_BASE_URL: ${NEXT_PUBLIC_API_BASE_URL}
NEXT_PUBLIC_SOCKET_URL: ${NEXT_PUBLIC_SOCKET_URL}
NEXT_PUBLIC_SOCKET_PATH: ${NEXT_PUBLIC_SOCKET_PATH}
container_name: desktable-frontend
restart: unless-stopped
depends_on:
- backend
ports:
- "3000:3000"
networks:
- desktable
volumes:
postgres_data:
redis_data:
networks:
desktable:
driver: bridge
Example .env.production
POSTGRES_USER=desktable
POSTGRES_PASSWORD=replace-with-strong-password
POSTGRES_DB=desktable
JWT_SECRET_KEY=replace-with-64-char-random-secret
OPENAI_API_KEY=sk-replace-with-secret
OPENAI_MODEL=gpt-4o
CORS_ORIGINS=https://desktable.example.com
NEXT_PUBLIC_API_BASE_URL=https://desktable.example.com/api/v1
NEXT_PUBLIC_SOCKET_URL=https://desktable.example.com
NEXT_PUBLIC_SOCKET_PATH=/ws/socket.io
Next.js exposes only variables prefixed with NEXT_PUBLIC_ to browser code; all other
variables remain server-side.[nextjs]
Dockerfiles
Dockerfile.backend
FROM python:3.11-slim AS base
ENV PYTHONDONTWRITEBYTECODE=1 \
PYTHONUNBUFFERED=1 \
PIP_NO_CACHE_DIR=1
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends \
build-essential \
curl \
&& rm -rf /var/lib/apt/lists/*
COPY requirements.txt .
RUN pip install --upgrade pip && \
pip install -r requirements.txt
COPY . .
RUN useradd --create-home appuser && \
chown -R appuser:appuser /app
USER appuser
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
Dockerfile.frontend
FROM node:18-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
FROM node:18-alpine AS builder
WORKDIR /app
ARG NEXT_PUBLIC_API_BASE_URL
ARG NEXT_PUBLIC_SOCKET_URL
ARG NEXT_PUBLIC_SOCKET_PATH
ENV NEXT_PUBLIC_API_BASE_URL=$NEXT_PUBLIC_API_BASE_URL \
NEXT_PUBLIC_SOCKET_URL=$NEXT_PUBLIC_SOCKET_URL \
NEXT_PUBLIC_SOCKET_PATH=$NEXT_PUBLIC_SOCKET_PATH \
NEXT_TELEMETRY_DISABLED=1
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build
FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production \
NEXT_TELEMETRY_DISABLED=1
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/package-lock.json ./package-lock.json
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
RUN addgroup -g 1001 nodejs && \
adduser -u 1001 -G nodejs -s /bin/sh -D nextjs && \
chown -R nextjs:nodejs /app
USER nextjs
EXPOSE 3000
CMD ["npm", "run", "start"]
Build and Run
Development Stack
docker compose up --build
Services:
Service URL
Frontend http://localhost:3000
Backend API http://localhost:8000
OpenAPI docs http://localhost:8000
/docs
PostgreSQL localhost:5432
Redis localhost:6379
Production Build
docker compose -f docker-compose.yml --env-file .env.production up -d --build
View Logs
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f db
docker compose logs -f redis
Stop Stack
docker compose down
Stop and Remove Volumes
Warning: This deletes persisted PostgreSQL and Redis data.
docker compose down -v
Database Migrations
Alembic migrations should run before the backend accepts traffic.
Run Migrations in a Container
docker compose exec backend alembic upgrade head
Create a New Migration
docker compose exec backend alembic revision --autogenerate -m "add lesson progress
table"
Review the generated file in:
backend/alembic/versions/
Then apply it:
docker compose exec backend alembic upgrade head
Check Current Revision
docker compose exec backend alembic current
View Migration History
docker compose exec backend alembic history
Roll Back One Revision
docker compose exec backend alembic downgrade -1
Reverse Proxy Configuration
Use Nginx, Caddy, Traefik, or a cloud load balancer to terminate TLS and route traffic.
Nginx Example
upstream desktable_frontend {
server frontend:3000;
}
upstream desktable_backend {
server backend:8000;
}
server {
listen 80;
server_name desktable.example.com;
return 301 https://$host$request_uri;
}
server {
listen 443 ssl http2;
server_name desktable.example.com;
ssl_certificate /etc/letsencrypt/live/desktable.example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/desktable.example.com/privkey.pem;
location / {
proxy_pass http://desktable_frontend;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
}
location /api/ {
proxy_pass http://desktable_backend;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
}
location /ws/ {
proxy_pass http://desktable_backend;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_read_timeout 3600s;
proxy_send_timeout 3600s;
}
}
CORS Configuration
Set CORS_ORIGINS to the exact frontend origins permitted to call the API.
CORS_ORIGINS=https://desktable.example.com,https://app.desktable.example.com
Avoid * in production. The backend should allow credentials only for trusted origins,
especially because Socket.IO connections may use authentication cookies or bearer-token
payloads.
SSL and Secure Media
Use HTTPS in production for:
● REST API traffic.
● Socket.IO WebSocket upgrades.
● Browser microphone access.
● Browser camera access via MediaPipe and face-api.js.
Modern browsers generally require a secure context for camera and microphone access.
Serve DeskTable through HTTPS and configure the reverse proxy to preserve WebSocket
upgrade headers.
Required Nginx WebSocket settings:
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
Production Checklist
● Use PostgreSQL rather than SQLite.
● Set DEBUG=false.
● Set ENVIRONMENT=production.
● Use a long, random JWT_SECRET_KEY.
● Store secrets in a managed secret manager.
● Restrict CORS_ORIGINS to production domains.
● Run alembic upgrade head before deploying new backend versions.
● Enable HTTPS and redirect HTTP to HTTPS.
● Configure WebSocket proxy timeouts.
● Enable Redis persistence or use a managed Redis service.
● Configure database backups and restore tests.
● Set OpenAI usage and spend limits.
● Apply per-user and per-room rate limits for AI, TTS, and vision events.
● Disable or minimize vision-frame storage by default.
● Add structured logging, tracing, and error tracking.
● Monitor database connections, Redis memory, CPU, latency, and Socket.IO
connection counts.
● Run database migrations during a controlled deployment window.
● Use health checks for backend, frontend, database, and Redis.
● Configure log retention and PII-redaction policies.
Health Checks
Expose a backend health endpoint such as:
GET /api/v1/admin/health
Expected response:
{
"status": "healthy",
"database": "healthy",
"redis": "healthy",
"openai": "healthy",
"socketio": "healthy"
}
Use container-level health checks as well:
healthcheck:
test: ["CMD", "curl", "-f", "http://localhost:8000/api/v1/admin/health"]
interval: 30s
timeout: 5s
retries: 3
Scaling
Component Scaling Strategy
Frontend Run multiple stateless Next.js containers behind a load balancer or use a
managed hosting platform
Backend
API
Scale FastAPI replicas horizontally
Socket.IO Use Redis adapter/message queue and sticky sessions, or route
WebSocket connections consistently
PostgreSQL Use managed PostgreSQL with read replicas for reporting workloads
Redis Use managed Redis with persistence and eviction policy appropriate to
session data
AI calls Queue and throttle requests; use streaming to reduce perceived latency
Vision Limit frame rate, compress frames, and process client-side where possible
Rollback Procedure
1. Stop new deployments.
2. Identify the last stable backend image tag.
3. Redeploy the previous backend image.
4. If the database schema changed, roll back only if the migration is reversible and safe:
docker compose exec backend alembic downgrade -1
5. Verify:
docker compose ps
docker compose logs -f backend
curl -f https://desktable.example.com/api/v1/admin/health
6. Restore the previous frontend image if the API contract changed.
Below are the four deliverable documents for DeskTable. They are written as
GitHub-flavored Markdown and can be placed directly in the repository root or docs/
directory.
README.md
DeskTable — AI STEM Tutor
DeskTable is an interactive AI tutor that teaches STEM concepts live on a digital
chalkboard. Students can draw, solve problems, speak to the tutor, and receive real-time
Socratic guidance while the system observes their notebook or desk through a vision
pipeline.
Instead of simply returning answers, DeskTable guides students through reasoning steps,
highlights work on the whiteboard, explains concepts with synthesized voice, and adapts
hints based on handwritten work and verbal responses.
Key Features
● Interactive Chalkboard / Whiteboard — Draw equations, diagrams, graphs, and
annotations using Excalidraw and Fabric.js.
● Socratic AI Tutor — OpenAI-powered reasoning that asks guiding questions rather
than immediately giving final answers.
● Live Voice Interaction — Students can speak naturally; the tutor responds through
streamed text-to-speech audio.
● Desk Vision — MediaPipe and face-api.js support desk, notebook, gesture, and
attention-aware learning experiences.
● Real-Time Collaboration — Socket.IO synchronizes canvas actions, tutor drawing
steps, voice events, and AI responses.
● Lesson Progress — Lessons, attempts, session history, and mastery progress are
persisted through PostgreSQL and SQLAlchemy.
● Subscription Tiers — Pricing, entitlement, and access-tier management for free,
student, and premium plans.
● Admin Observability — System metrics, usage monitoring, and operational health
endpoints.
Tech Stack
Layer Technology Purpose
Backend Python 3.11, FastAPI REST APIs, dependency injection,
async services
Database PostgreSQL or SQLite, SQLAlchemy
2.0 Async
Persistent application data
Migrations Alembic Versioned database schema
migrations
Realtime python-socketio, Redis WebSocket events, rooms, pub/sub
coordination
AI OpenAI API Socratic tutoring, reasoning,
explanations
Vision MediaPipe, face-api.js Desk/notebook perception and
student tracking
Voice Browser speech APIs, backend TTS
service
Speech input and streamed tutor
audio
Frontend Next.js 14 App Router, React 18,
TypeScript
Application UI and routing
State Zustand Lightweight client-side state
management
Canvas Excalidraw, Fabric.js Interactive drawing and whiteboard
rendering
Math KaTeX Mathematical expression rendering
Animation GSAP Chalk-style drawing and tutor
animation effects
Testing PyTest Backend unit and integration tests
Project Structure
desktable/
├── backend/
│ ├── app/
│ │ ├── main.py # FastAPI application entrypoint
│ │ ├── core/
│ │ │ ├── config.py # Settings and environment configuration
│ │ │ ├── security.py # JWT, password hashing, auth utilities
│ │ │ └── dependencies.py # Shared FastAPI dependencies
│ │ ├── db/
│ │ │ ├── base.py # SQLAlchemy declarative base
│ │ │ ├── session.py # Async engine and session factory
│ │ │ └── redis.py # Redis connection management
│ │ ├── modules/
│ │ │ ├── auth/ # Login, registration, JWT management
│ │ │ ├── lessons/ # Lessons, progress, history
│ │ │ ├── ai/ # Socratic reasoning and OpenAI integration
│ │ │ ├── tts/ # Text-to-speech streaming
│ │ │ ├── vision/ # Vision ingestion and analysis
│ │ │ ├── subscription/ # Plans, entitlements, billing hooks
│ │ │ ├── admin/ # Metrics and system monitoring
│ │ │ └── websocket/ # Socket.IO events and agent coordination
│ │ ├── schemas/ # Pydantic request/response models
│ │ └── utils/ # Shared helpers
│ ├── alembic/
│ │ ├── versions/ # Migration files
│ │ └── env.py
│ ├── tests/
│ ├── alembic.ini
│ ├── pyproject.toml
│ ├── requirements.txt
│ └── .env.example
├── frontend/
│ ├── app/ # Next.js App Router pages and layouts
│ ├── components/
│ │ ├── whiteboard/ # Excalidraw / Fabric.js canvas components
│ │ ├── tutor/ # Chat, hints, explanations, voice UI
│ │ ├── vision/ # Camera and MediaPipe integrations
│ │ └── shared/ # Buttons, cards, modals, layout primitives
│ ├── lib/
│ │ ├── api.ts # Typed REST API client
│ │ ├── socket.ts # Socket.IO client
│ │ ├── tts.ts # Audio playback and streaming helpers
│ │ └── vision.ts # MediaPipe / face-api.js helpers
│ ├── stores/
│ │ ├── authStore.ts
│ │ ├── lessonStore.ts
│ │ ├── canvasStore.ts
│ │ ├── tutorStore.ts
│ │ └── visionStore.ts
│ ├── styles/
│ ├── package.json
│ ├── tailwind.config.ts
│ ├── tsconfig.json
│ └── .env.local.example
├── docker-compose.yml
├── Dockerfile.backend
├── Dockerfile.frontend
├── .github/
│ └── workflows/
├── docs/
│ ├── ARCHITECTURE.md
│ ├── API_REFERENCE.md
│ └── DEPLOYMENT.md
└── README.md
Prerequisites
Requirement Minimum Version Notes
Node.js 18+ Required for Next.js 14 frontend
npm 9+ Installed with Node.js
Python 3.11+ Required for FastAPI backend
PostgreSQL 14+ Recommended for local and production use
SQLite 3+ Acceptable for lightweight local development
Redis 6+ Required for Socket.IO coordination and
caching
OpenAI API key — Required for AI tutor functionality
Quickstart
1. Clone the repository
git clone https://github.com/your-org/desktable.git
cd desktable
2. Configure backend environment
cd backend
cp .env.example .env
Update .env with your local database, Redis, JWT, and OpenAI settings.
3. Install backend dependencies
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
4. Run database migrations
alembic upgrade head
5. Start the backend
uvicorn app.main:app --reload
By default, the API is available at:
http://localhost:8000
Interactive API documentation:
http://localhost:8000/docs
http://localhost:8000/redoc
6. Configure frontend environment
cd ../frontend
cp .env.local.example .env.local
Next.js exposes browser-accessible variables only when they are prefixed with
NEXT_PUBLIC_; server-only secrets should remain unprefixed.[nextjs]
7. Install frontend dependencies
npm install
8. Start the frontend
npm run dev
Open:
http://localhost:3000
Environment Variables
Backend — backend/.env
Variable Requi
red
Example Descript
ion
APP_NAME No DeskTable Applicati
on
display
name
ENVIRONMENT Yes local local,
stagin
g, or
produc
tion
DEBUG No true Enables
verbose
logging
and
debug
behavior
API_V1_PREFIX Yes /api/v1 Base
path for
REST
endpoint
s
DATABASE_URL Yes postgresql+asyncpg://postgres:postg
res@localhost:5432/desktable
Async
SQLAlch
emy
databas
e URL
SQLITE_DATABASE
_URL
No sqlite+aiosqlite:///./desktable.db Optional
SQLite
fallback
REDIS_URL Yes redis://localhost:6379/0 Redis
connecti
on URL
JWT_SECRET_KEY Yes change-me-in-production Secret
used to
sign
JWTs
JWT_ALGORITHM Yes HS256 JWT
signing
algorith
m
ACCESS_TOKEN_EX
PIRE_MINUTES
Yes 60 Access-t
oken
lifetime
REFRESH_TOKEN_E
XPIRE_DAYS
Yes 30 Refreshtoken
lifetime
OPENAI_API_KEY Yes sk-... OpenAI
API
credenti
al
OPENAI_MODEL No gpt-4o Default
reasonin
g model
OPENAI_EMBEDDIN
G_MODEL
No text-embedding-3-small Embeddi
ng
model
for
lesson
context
TTS_PROVIDER No openai TTS
provider
identifier
TTS_VOICE No alloy Default
tutor
voice
VISION_MAX_FPS No 5 Maximu
m
process
ed vision
frames
per
second
CORS_ORIGINS Yes http://localhost:3000 Commaseparate
d
allowed
origins
SOCKETIO_PATH Yes /ws/socket.io Socket.I
O
request
path
Frontend — frontend/.env.local
Variable Require
d
Example Description
NEXT_PUBLIC_API_BAS
E_URL
Yes http://localhost:8000
/api/v1
REST API base
URL
NEXT_PUBLIC_SOCKET_
URL
Yes http://localhost:8000 Socket.IO server
origin
NEXT_PUBLIC_SOCKET_
PATH
Yes /ws/socket.io Socket.IO path
NEXT_PUBLIC_APP_NAM
E
No DeskTable UI display name
NEXT_PUBLIC_ENABLE_
VISION
No true Enables camera
and vision features
NEXT_PUBLIC_ENABLE_
VOICE
No true Enables
microphone and
TTS features
NEXT_PUBLIC_SENTRY_
DSN
No https://... Optional frontend
error tracking
Running Tests
Backend tests use PyTest:
cd backend
pytest
Run tests with coverage:
pytest --cov=app --cov-report=term-missing
Useful Commands
Command Location Description
uvicorn app.main:app --reload backend/ Start FastAPI in development
mode
alembic upgrade head backend/ Apply all pending migrations
alembic revision
--autogenerate -m "message"
backend/ Create a new migration
pytest backend/ Run backend tests
npm run dev frontend/ Start Next.js development
server
npm run build frontend/ Create a production frontend
build
npm run lint frontend/ Run frontend lint checks
docker compose up --build Repository
root
Start full local Docker stack
Security Notes
● Never commit .env, .env.local, API keys, JWT secrets, or database credentials.
● Use strong, unique values for JWT_SECRET_KEY in every environment.
● Restrict CORS_ORIGINS to known frontend origins.
● Use HTTPS in production so WebSockets, microphone access, and camera access
work securely.
● Rotate OpenAI credentials and database passwords through your
secret-management system.
License
Specify your project license here, for example:
MIT License
ARCHITECTURE.md
DeskTable Architecture
DeskTable uses a decoupled Next.js frontend and FastAPI backend, connected through
REST APIs for transactional operations and Socket.IO for low-latency realtime tutoring. The
backend coordinates OpenAI reasoning, vision analysis, TTS streaming, Redis-backed
session state, and persistent lesson data.
High-Level Architecture
flowchart TB
subgraph Client["Browser / Student Device"]
UI["Next.js 14 App Router UI"]
Canvas["Excalidraw + Fabric.js Whiteboard"]
Mic["Microphone / Speech Input"]
Camera["Camera / MediaPipe / face-api.js"]
Audio["TTS Audio Player"]
Stores["Zustand Stores"]
end
subgraph Edge["FastAPI ASGI Application"]
REST["REST API /api/v1"]
WS["Socket.IO /ws/socket.io"]
Auth["Auth Module"]
Lessons["Lessons Module"]
AI["AI Module"]
TTS["TTS Module"]
Vision["Vision Module"]
Subs["Subscription Module"]
Admin["Admin Module"]
end
subgraph Services["External Services"]
OpenAI["OpenAI API"]
Redis["Redis"]
DB[("PostgreSQL / SQLite")]
end
UI --> REST
UI --> WS
Canvas --> WS
Mic --> WS
Camera --> WS
Audio --> UI
Stores --> UI
UI --> Canvas
UI --> Mic
UI --> Camera
REST --> Auth
REST --> Lessons
REST --> AI
REST --> TTS
REST --> Vision
REST --> Subs
REST --> Admin
WS --> AI
WS --> TTS
WS --> Vision
WS --> Lessons
AI --> OpenAI
AI --> Redis
TTS --> OpenAI
Vision --> Redis
Lessons --> DB
Auth --> DB
Subs --> DB
Admin --> Redis
Core Design Principles
● Realtime-first tutoring — Canvas strokes, voice events, AI responses, and tutor
drawing actions are streamed through Socket.IO.
● Socratic by default — The AI module is designed to produce questions, partial
hints, and step-by-step guidance rather than direct answers.
● Separation of concerns — Each domain module owns its routes, schemas,
services, and persistence concerns.
● Async throughout — FastAPI, SQLAlchemy, Redis, OpenAI calls, and Socket.IO
handlers use async I/O.
● Ephemeral versus durable state — Active tutoring context lives in Redis;
completed lessons and progress live in the relational database.
● Progressive disclosure — The tutor reveals concepts gradually based on student
responses, canvas activity, and vision context.
Realtime Interaction Pipeline
sequenceDiagram
autonumber
participant S as Student Browser
participant C as Whiteboard Canvas
participant WS as Socket.IO Client
participant API as FastAPI Socket Server
participant AI as AI Service
participant OAI as OpenAI API
participant VIS as Vision Service
participant TTS as TTS Service
participant R as Redis
S->>C: Draws equation or diagram
C->>WS: canvas_draw event
WS->>API: Emit canvas_draw
API->>R: Store latest canvas context / room state
S->>WS: user_voice_input
WS->>API: Emit user_voice_input
API->>AI: Build tutoring context
AI->>OAI: Request Socratic response
alt Vision enabled
S->>WS: vision_frame
WS->>API: Emit vision_frame
API->>VIS: Analyze handwriting / desk context
VIS->>R: Cache frame analysis
VIS-->>API: Vision insight
API->>AI: Enrich prompt with vision context
end
OAI-->>AI: Streaming model response
AI-->>API: Reasoning chunks and drawing plan
API-->>WS: ai_response_chunk
API-->>WS: socratic_hint
API-->>WS: draw_step
WS-->>C: Render tutor drawing animation
API->>TTS: Convert explanation to speech
TTS-->>API: tts_audio_chunk
API-->>WS: tts_audio_chunk
WS-->>S: Play streamed tutor audio
Pipeline Stages
Stage Component Responsibility
Capture Excalidraw / Fabric.js Captures strokes, shapes, equations, and
annotations
Transport Socket.IO client Sends canvas, voice, and vision events to the
backend
Session
coordination
FastAPI WebSocket
module
Validates JWT/session, joins rooms, routes
events
Context
assembly
AI module Combines lesson metadata, canvas state, chat
history, and vision insights
Reasoning OpenAI API Produces streamed Socratic explanations and
structured drawing steps
Perception Vision module Interprets notebook/desk frames and attention
signals
Expression TTS module Streams tutor explanation audio to the browser
Persistence SQLAlchemy Saves lesson sessions, messages, progress,
and completed artifacts
Ephemeral state Redis Stores active room state, rate limits, streams,
and transient AI context
Socket.IO Integration
The Socket.IO ASGI application is mounted alongside FastAPI at /ws/socket.io. A
typical integration wraps the FastAPI app in a socketio.ASGIApp and mounts it under
/ws, with the Socket.IO path configured as /ws/socket.io.[fastapi-practices.github]
# backend/app/main.py
import socketio
from fastapi import FastAPI
from app.modules.websocket.events import register_socket_events
app = FastAPI(title="DeskTable API")
sio = socketio.AsyncServer(
async_mode="asgi",
cors_allowed_origins=settings.CORS_ORIGINS,
)
socket_app = socketio.ASGIApp(
socketio_server=sio,
other_asgi_app=app,
socketio_path="/ws/socket.io",
)
register_socket_events(sio)
app.mount("/ws", socket_app)
The frontend connects using the matching path:
// frontend/lib/socket.ts
import { io } from "socket.io-client";
export const socket = io(process.env.NEXT_PUBLIC_SOCKET_URL!, {
path: process.env.NEXT_PUBLIC_SOCKET_PATH ?? "/ws/socket.io",
transports: ["websocket"],
autoConnect: false,
withCredentials: true,
});
Frontend State Management
Zustand provides focused client-side stores. Each store owns a narrow slice of UI state and
communicates with services through typed API and Socket.IO clients.
Store Owns Updated By
authStore Access token, refresh token,
current user, auth status
REST auth endpoints
lessonStore Active lesson, lesson list, progress,
history
REST lesson endpoints and
Socket.IO session events
canvasStore Canvas elements, selected tools,
undo/redo stack, tutor drawing
queue
Local canvas actions and
draw_step events
tutorStore Chat transcript, streaming
response, hints, tutor status
ai_response_chunk,
socratic_hint,
tts_audio_chunk
visionStore Camera permission, tracking
status, latest analysis, privacy
mode
Local MediaPipe pipeline and
vision_analysis events
subscriptionS
tore
Current plan, entitlements, usage
limits
REST subscription endpoints
Example Zustand Store
// frontend/stores/tutorStore.ts
import { create } from "zustand";
type TutorMessage = {
id: string;
role: "student" | "tutor";
content: string;
createdAt: string;
};
type TutorState = {
messages: TutorMessage[];
isThinking: boolean;
currentHint: string | null;
appendChunk: (chunk: string) => void;
setHint: (hint: string | null) => void;
setThinking: (thinking: boolean) => void;
reset: () => void;
};
export const useTutorStore = create<TutorState>((set) => ({
messages: [],
isThinking: false,
currentHint: null,
appendChunk: (chunk) =>
set((state) => {
const last = state.messages[state.messages.length - 1];
if (!last || last.role !== "tutor") {
return {
messages: [
...state.messages,
{
id: crypto.randomUUID(),
role: "tutor",
content: chunk,
createdAt: new Date().toISOString(),
},
],
};
}
const updated = [...state.messages];
updated[updated.length - 1] = {
...last,
content: last.content + chunk,
};
return { messages: updated };
}),
setHint: (currentHint) => set({ currentHint }),
setThinking: (isThinking) => set({ isThinking }),
reset: () => set({ messages: [], isThinking: false, currentHint: null }),
}));
Backend State Model
State Type Store Lifetime Examples
Identity PostgreSQL /
SQLite
Long-lived Users, credentials, roles
Learning
record
PostgreSQL /
SQLite
Long-lived Lessons, sessions, messages,
progress
Billing state PostgreSQL /
SQLite
Long-lived Plans, subscriptions, entitlements
Active room Redis Minutes to
hours
Room membership, connection IDs,
canvas version
Tutoring
context
Redis Session-scope
d
Recent messages, current problem,
hint level
Stream state Redis Seconds to
minutes
Active AI stream, TTS sequence,
partial transcription
Rate limits Redis Short-lived AI request limits, vision-frame limits
AI Tutoring Flow
flowchart LR
A[Student Action] --> B{Input Type}
B -- Canvas --> C[Canvas Context Extractor]
B -- Voice --> D[Speech / Transcript Handler]
B -- Vision --> E[Vision Analyzer]
B -- Text --> F[Chat Message Handler]
C --> G[Context Builder]
D --> G
E --> G
F --> G
G --> H[Socratic Prompt Chain]
H --> I[OpenAI Streaming Completion]
I --> J{Response Type}
J -- Explanation --> K[ai_response_chunk]
J -- Guided question --> L[socratic_hint]
J -- Board action --> M[draw_step]
J -- Narration --> N[TTS stream]
Socratic Prompt Chain
The AI module builds prompts in layers:
1. System layer — Defines the tutor persona, STEM subject, safety constraints, and
Socratic behavior.
2. Lesson layer — Supplies topic, learning objective, difficulty, prior progress, and
curriculum alignment.
3. Canvas layer — Supplies a compact representation of current drawings, equations,
labels, and selected objects.
4. Vision layer — Supplies privacy-safe observations such as detected handwriting
regions, work-in-progress signals, or attention cues.
5. Conversation layer — Supplies recent student/tutor turns and prior hints.
6. Output contract — Requests separate fields for explanation, next question, hint
level, and optional whiteboard drawing actions.
Example response contract:
{
"explanation": "Let's isolate the unknown variable step by step.",
"socratic_question": "What operation can we apply to both sides to remove the +7?",
"hint_level": 1,
"draw_steps": [
{
"type": "text",
"content": "x + 7 = 12",
"position": { "x": 120, "y": 100 }
},
{
"type": "arrow",
"from": { "x": 180, "y": 130 },
"to": { "x": 240, "y": 130 },
"label": "- 7"
}
],
"tts_text": "We have x plus seven equals twelve. What could we subtract from both sides?"
}
Data Flow Summary
Flow Trigger Path Result
Canvas
collaboration
Student draws Canvas → canvas_draw
→ Socket.IO → Redis →
peers
All participants see
synchronized board
state
Socratic
tutoring
Student asks or
submits work
Socket.IO → AI service →
OpenAI → Socket.IO
Streamed explanation,
hint, and drawing steps
Voice
explanation
AI produces
narration
AI → TTS service →
Socket.IO → browser audio
Tutor speaks while
drawing
Vision-assiste
d help
Camera frame
captured
Browser → vision_frame
→ Vision service → AI
Context-aware
feedback
Lesson
completion
Tutoring session
ends
Socket.IO/REST → Lessons
service → Database
Persisted transcript,
progress, and artifacts
Access
enforcement
API or
WebSocket
request
Auth + subscription modules Allowed or denied
based on identity and
plan
Scalability Considerations
● Run multiple FastAPI/Socket.IO workers behind a load balancer with sticky sessions
or use the Redis message queue for cross-worker Socket.IO events.
● Use Redis pub/sub or Redis Streams for fan-out of AI and TTS events across
horizontally scaled instances.
● Process vision frames at a bounded frame rate and prefer client-side preprocessing
to reduce bandwidth.
● Cache lesson context and repeated AI prompts where appropriate, while avoiding
caching personal student data longer than necessary.
● Use database read replicas for analytics-heavy admin queries.
● Offload long-running AI generation to background workers when response time or
reliability requirements demand it.
API_REFERENCE.md
DeskTable API Reference
All REST endpoints are versioned under:
/api/v1
Unless stated otherwise:
● Requests and responses use application/json.
● Authentication uses a bearer access token.
● Timestamps are ISO 8601 UTC strings.
● IDs are UUID strings.
Authentication Header
Authorization: Bearer <access_token>
Standard Error Format
{
"detail": {
"code": "lesson_not_found",
"message": "The requested lesson does not exist.",
"request_id": "b3f2c7d4-8e91-4f2a-9d44-6d7a1b9f1234"
}
}
HTTP Status Meaning
200 Success
201 Resource created
204 Success with no response body
400 Invalid request
401 Missing or invalid authentication
403 Authenticated but not permitted
404 Resource not found
409 Conflict, such as duplicate
resource
422 Validation error
429 Rate limit exceeded
500 Internal server error
Auth — /api/v1/auth
Handles registration, login, token refresh, profile access, and logout.
Register User
POST /api/v1/auth/register
Request
{
"email": "student@example.com",
"password": "StrongPassw0rd!",
"full_name": "Ada Lovelace",
"grade_level": "grade_10"
}
Response — 201 Created
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"grade_level": "grade_10",
"created_at": "2026-10-14T09:00:00Z"
}
Login
POST /api/v1/auth/login
Request
{
"email": "student@example.com",
"password": "StrongPassw0rd!"
}
Response — 200 OK
{
"access_token": "eyJhbGciOiJIUzI1NiIs...",
"refresh_token": "eyJhbGciOiJIUzI1NiIs...",
"token_type": "bearer",
"expires_in": 3600,
"user": {
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace"
}
}
Refresh Token
POST /api/v1/auth/refresh
Request
{
"refresh_token": "eyJhbGciOiJIUzI1NiIs..."
}
Response — 200 OK
{
"access_token": "eyJhbGciOiJIUzI1NiIs...",
"token_type": "bearer",
"expires_in": 3600
}
Get Current User
GET /api/v1/auth/me
Response — 200 OK
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"grade_level": "grade_10",
"subscription_tier": "premium",
"created_at": "2026-10-14T09:00:00Z"
}
Logout
POST /api/v1/auth/logout
Request
{
"refresh_token": "eyJhbGciOiJIUzI1NiIs..."
}
Response — 204 No Content
Lessons — /api/v1/lessons
Manages lesson planning, tutoring sessions, progress, and history.
List Lessons
GET /api/v1/lessons
Query Parameters
Parameter Type Required Description
subject string No math, physics, chemistry, biology,
computer_science
status string No draft, published, archived
difficul
ty
string No beginner, intermediate, advanced
page integer No Page number, default 1
page_siz
e
integer No Items per page, default 20, maximum 100
Response — 200 OK
{
"items": [
{
"id": "1c3f4b7e-0d21-4c7a-8f10-9d8e7c6b5a43",
"title": "Solving Linear Equations",
"subject": "math",
"difficulty": "beginner",
"summary": "Learn to isolate variables using inverse operations.",
"estimated_minutes": 25,
"status": "published"
}
],
"page": 1,
"page_size": 20,
"total": 42
}
Create Lesson
POST /api/v1/lessons
Request
{
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"summary": "Understand force, mass, and acceleration relationships.",
"learning_objectives": [
"Interpret F = ma",
"Solve for acceleration",
"Apply units correctly"
],
"estimated_minutes": 30
}
Response — 201 Created
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"status": "draft",
"created_at": "2026-10-14T09:12:00Z"
}
Get Lesson
GET /api/v1/lessons/{lesson_id}
Response — 200 OK
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"summary": "Understand force, mass, and acceleration relationships.",
"learning_objectives": [
"Interpret F = ma",
"Solve for acceleration",
"Apply units correctly"
],
"estimated_minutes": 30,
"status": "published"
}
Update Lesson
PATCH /api/v1/lessons/{lesson_id}
Request
{
"title": "Newton's Second Law and Free-Body Diagrams",
"status": "published"
}
Response — 200 OK
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law and Free-Body Diagrams",
"status": "published",
"updated_at": "2026-10-14T09:20:00Z"
}
Delete Lesson
DELETE /api/v1/lessons/{lesson_id}
Response — 204 No Content
Start Tutoring Session
POST /api/v1/lessons/{lesson_id}/sessions
Request
{
"mode": "socratic",
"enable_voice": true,
"enable_vision": true
}
Response — 201 Created
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"status": "active",
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"started_at": "2026-10-14T09:25:00Z"
}
Get Session
GET /api/v1/lessons/sessions/{session_id}
Response — 200 OK
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"status": "active",
"message_count": 8,
"started_at": "2026-10-14T09:25:00Z",
"ended_at": null
}
Complete Session
POST /api/v1/lessons/sessions/{session_id}/complete
Request
{
"outcome": "completed",
"mastery_score": 0.82,
"notes": "Student successfully solved three linear equations."
}
Response — 200 OK
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"status": "completed",
"outcome": "completed",
"mastery_score": 0.82,
"ended_at": "2026-10-14T09:55:00Z"
}
Get Student Progress
GET /api/v1/lessons/progress
Response — 200 OK
{
"overall_mastery": 0.74,
"subjects": [
{
"subject": "math",
"mastery": 0.81,
"lessons_completed": 12
},
{
"subject": "physics",
"mastery": 0.66,
"lessons_completed": 7
}
]
}
AI — /api/v1/ai
Provides direct AI endpoints for non-streaming requests, tutor context creation, and
explanation generation. Most live tutoring traffic should use Socket.IO.
Create Tutor Context
POST /api/v1/ai/context
Request
{
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"problem": "Solve for x: 2x + 5 = 17",
"student_level": "beginner"
}
Response — 201 Created
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"expires_at": "2026-10-14T10:25:00Z"
}
Generate Explanation
POST /api/v1/ai/explain
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"question": "Why do we subtract 5 from both sides?",
"style": "socratic"
}
Response — 200 OK
{
"explanation": "Subtracting 5 keeps the equation balanced while isolating the term
containing x.",
"socratic_question": "What happens to the left side if we subtract 5 from both sides?",
"hint_level": 1,
"tokens_used": 412
}
Generate Hints
POST /api/v1/ai/hints
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"current_answer": "x = 5",
"hint_level": 2
}
Response — 200 OK
{
"hints": [
"First move the constant to the right-hand side.",
"Then divide both sides by the coefficient of x."
],
"next_question": "What should you divide both sides by?"
}
Evaluate Student Work
POST /api/v1/ai/evaluate
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"student_work": "2x + 5 = 17\n2x = 12\nx = 6",
"expected_answer": "x = 6"
}
Response — 200 OK
{
"correct": true,
"score": 1.0,
"feedback": "Correct. Your steps are clear and mathematically valid.",
"misconceptions": []
}
TTS — /api/v1/tts
Converts tutor text into audio. Live sessions normally receive audio through
tts_audio_chunk Socket.IO events, while REST is useful for previews and offline lesson
audio.
Synthesize Speech
POST /api/v1/tts/synthesize
Request
{
"text": "Let's isolate x by subtracting five from both sides.",
"voice": "alloy",
"format": "mp3"
}
Response — 200 OK
Content-Type: audio/mpeg
Returns binary audio content.
List Voices
GET /api/v1/tts/voices
Response — 200 OK
{
"voices": [
{
"id": "alloy",
"name": "Alloy",
"language": "en-US",
"gender": "neutral"
},
{
"id": "nova",
"name": "Nova",
"language": "en-US",
"gender": "female"
}
]
}
Create Streaming TTS Session
POST /api/v1/tts/stream
Request
{
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"text": "We can solve this by using inverse operations.",
"voice": "alloy"
}
Response — 202 Accepted
{
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"status": "streaming",
"transport": "websocket"
}
Audio chunks are delivered through the Socket.IO event tts_audio_chunk.
Vision — /api/v1/vision
Supports image analysis, vision-session configuration, and privacy controls. Camera frames
are normally streamed through Socket.IO; REST endpoints handle configuration, snapshots,
and results.
Create Vision Session
POST /api/v1/vision/sessions
Request
{
"lesson_session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"modalities": ["notebook", "handwriting", "attention"],
"max_fps": 5
}
Response — 201 Created
{
"id": "vis_4e3d2c1b-0a99-4f88-8e77-6d5c4b3a2f10",
"status": "active",
"max_fps": 5,
"privacy_mode": "standard"
}
Analyze Snapshot
POST /api/v1/vision/analyze
Request
Content-Type: multipart/form-data
Field Type Required Description
image file Yes JPEG, PNG, or WebP image
session_
id
string Yes Vision session ID
modaliti
es
string No Comma-separated analysis
modes
Response — 200 OK
{
"analysis_id": "an_1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c5d",
"handwriting_detected": true,
"equation_candidates": ["2x + 5 = 17"],
"work_in_progress": true,
"attention_score": 0.91,
"suggestions": [
"Ask the student to label the operation applied to both sides."
]
}
Update Privacy Mode
PATCH /api/v1/vision/sessions/{vision_session_id}
Request
{
"privacy_mode": "strict",
"store_frames": false
}
Response — 200 OK
{
"id": "vis_4e3d2c1b-0a99-4f88-8e77-6d5c4b3a2f10",
"privacy_mode": "strict",
"store_frames": false
}
Delete Vision Session
DELETE /api/v1/vision/sessions/{vision_session_id}
Response — 204 No Content
Subscription — /api/v1/subscription
Manages available plans, current entitlements, and subscription changes.
List Plans
GET /api/v1/subscription/plans
Response — 200 OK
{
"plans": [
{
"id": "free",
"name": "Free",
"price_monthly": 0,
"currency": "USD",
"features": [
"3 tutoring sessions per week",
"Basic whiteboard",
"Text-based AI hints"
]
},
{
"id": "student",
"name": "Student",
"price_monthly": 9.99,
"currency": "USD",
"features": [
"Unlimited tutoring sessions",
"Voice tutor",
"Desk vision",
"Lesson history"
]
},
{
"id": "premium",
"name": "Premium",
"price_monthly": 19.99,
"currency": "USD",
"features": [
"All Student features",
"Advanced subjects",
"Priority AI capacity",
"Parent progress reports"
]
}
]
}
Get Current Subscription
GET /api/v1/subscription/me
Response — 200 OK
{
"plan_id": "student",
"status": "active",
"current_period_end": "2026-11-14T00:00:00Z",
"entitlements": {
"voice_tutor": true,
"desk_vision": true,
"advanced_subjects": false,
"max_sessions_per_month": 100
}
}
Change Plan
POST /api/v1/subscription/change-plan
Request
{
"plan_id": "premium"
}
Response — 202 Accepted
{
"subscription_id": "sub_8f7e6d5c-4b3a-4928-8172-6f5e4d3c2b1a",
"status": "pending",
"requested_plan": "premium"
}
Cancel Subscription
POST /api/v1/subscription/cancel
Response — 200 OK
{
"subscription_id": "sub_8f7e6d5c-4b3a-4928-8172-6f5e4d3c2b1a",
"status": "canceling",
"effective_at": "2026-11-14T00:00:00Z"
}
Admin — /api/v1/admin
Administrative endpoints require an admin role.
System Health
GET /api/v1/admin/health
Response — 200 OK
{
"status": "healthy",
"database": "healthy",
"redis": "healthy",
"openai": "healthy",
"socketio": "healthy",
"version": "1.0.0"
}
Platform Metrics
GET /api/v1/admin/metrics
Query Parameters
Parameter Type Required Description
from string No ISO 8601 start
timestamp
to string No ISO 8601 end timestamp
granulari
ty
string No hour, day, week
Response — 200 OK
{
"active_users": 1284,
"active_sessions": 187,
"lessons_completed": 431,
"ai_requests": 15230,
"tts_minutes": 942.5,
"vision_frames_processed": 284100,
"error_rate": 0.008
}
List Users
GET /api/v1/admin/users
Query Parameters
Parameter Type Required Description
email string No Filter by email
subscription_
tier
string No Filter by plan
page integer No Page number
page_size integer No Page size
Response — 200 OK
{
"items": [
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"subscription_tier": "premium",
"is_active": true
}
],
"total": 1
}
WebSocket Protocol
Namespace / endpoint:
/ws/socket.io
The Socket.IO server is mounted as an ASGI sub-application alongside FastAPI, allowing
HTTP and WebSocket traffic to share one deployment.[fastapi-practices.github]
Connection
import { io } from "socket.io-client";
const socket = io("http://localhost:8000", {
path: "/ws/socket.io",
auth: {
token: accessToken,
},
transports: ["websocket"],
});
Authentication Payload
Field Type Required Description
token string Yes JWT access token
session_id string No Active tutoring session ID
client_vers
ion
string No Frontend build/version identifier
Common Error Event
Server → Client: socket_error
{
"code": "unauthorized",
"message": "Invalid or expired access token.",
"retryable": false
}
Client-to-Server Events
join_room
Joins a lesson or tutoring room.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"role": "student"
}
Response: room_joined
leave_room
Leaves the current room.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8"
}
Response: room_left
canvas_draw
Sends a drawing action from the student whiteboard.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "draw_01",
"type": "stroke",
"tool": "pen",
"color": "#111111",
"width": 3,
"points": [
{ "x": 120, "y": 240 },
{ "x": 132, "y": 238 },
{ "x": 150, "y": 230 }
],
"timestamp": "2026-10-14T09:30:12.480Z"
}
Server behavior
● Validates the room and user.
● Stores the latest canvas version in Redis.
● Broadcasts canvas_draw to other room participants.
● Optionally forwards canvas context to the AI service.
canvas_clear
Clears the board.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "clear_01"
}
Response: canvas_cleared
canvas_undo
Requests an undo action.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "undo_01"
}
Response: canvas_undo
vision_frame
Sends a camera or desk frame for analysis.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"frame_id": "frame_042",
"format": "jpeg",
"encoding": "base64",
"width": 640,
"height": 480,
"data": "<base64-encoded-image>",
"modalities": ["handwriting", "attention"]
}
Server behavior
● Applies rate limits and privacy policy.
● Sends the frame to the vision pipeline.
● Emits vision_analysis and may enrich the next AI prompt.
user_voice_input
Sends transcribed student speech or raw audio metadata.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"input_id": "voice_001",
"transcript": "I do not understand how to isolate x.",
"audio_duration_ms": 2400,
"language": "en-US"
}
Server behavior
● Adds the utterance to tutoring context.
● Requests a Socratic response from OpenAI.
● Streams ai_response_chunk, socratic_hint, draw_step, and
tts_audio_chunk.
tutor_request
Requests AI help with explicit text.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"message": "Can you explain the next step?",
"mode": "socratic",
"include_canvas": true,
"include_vision": true
}
Response: ai_response_started, ai_response_chunk, ai_response_completed
hint_request
Requests a hint at a specific level.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "hint_001",
"hint_level": 2,
"problem": "Solve for x: 2x + 5 = 17"
}
Response: socratic_hint
tts_request
Requests spoken narration for a specific explanation.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "tts_001",
"text": "Subtract five from both sides to isolate the term with x.",
"voice": "alloy"
}
Response: tts_started, tts_audio_chunk, tts_completed
typing
Indicates that a participant is typing.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"is_typing": true
}
Response: participant_typing
Server-to-Client Events
room_joined
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"role": "student",
"participants": [
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"role": "student"
}
]
}
room_left
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10"
}
participant_joined
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant": {
"id": "f1e2d3c4-b5a6-4789-8123-456789abcdef",
"role": "tutor"
}
}
participant_left
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "f1e2d3c4-b5a6-4789-8123-456789abcdef"
}
participant_typing
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"is_typing": true
}
canvas_draw
Broadcasts a drawing action to room participants.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "draw_01",
"origin": "student",
"type": "stroke",
"tool": "pen",
"color": "#111111",
"width": 3,
"points": [
{ "x": 120, "y": 240 },
{ "x": 132, "y": 238 }
]
}
canvas_cleared
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"origin": "tutor"
}
canvas_undo
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "undo_01",
"origin": "student"
}
ai_response_started
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"model": "gpt-4o",
"mode": "socratic"
}
ai_response_chunk
Streams a partial tutor response.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"chunk_index": 3,
"delta": "Subtract 5 from both sides.",
"done": false
}
ai_response_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"full_response": "We can isolate x by subtracting 5 from both sides, then dividing by 2.",
"tokens_used": 388,
"done": true
}
socratic_hint
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "hint_001",
"hint_level": 2,
"hint": "Move the constant term to the other side first.",
"question": "What operation removes +5 from the left-hand side?"
}
draw_step
Instructs the whiteboard to render a tutor-generated action.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"step_index": 1,
"action": {
"type": "text",
"content": "2x + 5 - 5 = 17 - 5",
"position": { "x": 140, "y": 180 },
"style": {
"font": "chalk",
"color": "#ffffff"
}
},
"animation": {
"type": "chalk_write",
"duration_ms": 900
}
}
vision_analysis
Returns privacy-safe insights from a vision frame.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"frame_id": "frame_042",
"handwriting_detected": true,
"equation_candidates": ["2x + 5 = 17"],
"work_in_progress": true,
"attention_score": 0.91,
"suggestions": [
"Ask the student what operation removes +5."
]
}
tts_started
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"request_id": "tts_001",
"voice": "alloy",
"format": "mp3"
}
tts_audio_chunk
Streams encoded audio for browser playback.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"sequence": 4,
"format": "mp3",
"encoding": "base64",
"data": "<base64-audio-chunk>",
"done": false
}
tts_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"duration_ms": 4200,
"done": true
}
lesson_progress_updated
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"progress": 0.65,
"mastery_score": 0.78
}
session_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"outcome": "completed",
"ended_at": "2026-10-14T09:55:00Z"
}
DEPLOYMENT.md
DeskTable Deployment Guide
This guide covers Docker-based deployment, database migrations, environment
configuration, reverse-proxy setup, and production hardening for DeskTable.
Deployment Topology
flowchart LR
User["Student Browser"] --> LB["HTTPS Load Balancer / Reverse Proxy"]
LB --> FE["Next.js Frontend Container"]
LB --> BE["FastAPI + Socket.IO Backend Container"]
FE --> BE
BE --> DB[("PostgreSQL")]
BE --> RD[("Redis")]
BE --> OAI["OpenAI API"]
subgraph Optional["Optional Production Services"]
OBJ["Object Storage"]
MON["Monitoring / Logging"]
CDN["CDN"]
end
FE --> CDN
BE --> OBJ
BE --> MON
Docker Compose
Example docker-compose.yml
version: "3.9"
services:
db:
image: postgres:16-alpine
container_name: desktable-db
restart: unless-stopped
environment:
POSTGRES_USER: ${POSTGRES_USER}
POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
POSTGRES_DB: ${POSTGRES_DB}
volumes:
- postgres_data:/var/lib/postgresql/data
healthcheck:
test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
interval: 10s
timeout: 5s
retries: 5
networks:
- desktable
redis:
image: redis:7-alpine
container_name: desktable-redis
restart: unless-stopped
command: ["redis-server", "--appendonly", "yes"]
volumes:
- redis_data:/data
healthcheck:
test: ["CMD", "redis-cli", "ping"]
interval: 10s
timeout: 5s
retries: 5
networks:
- desktable
backend:
build:
context: ./backend
dockerfile: ../Dockerfile.backend
container_name: desktable-backend
restart: unless-stopped
depends_on:
db:
condition: service_healthy
redis:
condition: service_healthy
environment:
ENVIRONMENT: production
DEBUG: "false"
API_V1_PREFIX: /api/v1
DATABASE_URL:
postgresql+asyncpg://${POSTGRES_USER}:${POSTGRES_PASSWORD}@db:5432/${PO
STGRES_DB}
REDIS_URL: redis://redis:6379/0
JWT_SECRET_KEY: ${JWT_SECRET_KEY}
JWT_ALGORITHM: HS256
ACCESS_TOKEN_EXPIRE_MINUTES: 60
REFRESH_TOKEN_EXPIRE_DAYS: 30
OPENAI_API_KEY: ${OPENAI_API_KEY}
OPENAI_MODEL: ${OPENAI_MODEL}
CORS_ORIGINS: ${CORS_ORIGINS}
SOCKETIO_PATH: /ws/socket.io
ports:
- "8000:8000"
networks:
- desktable
frontend:
build:
context: ./frontend
dockerfile: ../Dockerfile.frontend
args:
NEXT_PUBLIC_API_BASE_URL: ${NEXT_PUBLIC_API_BASE_URL}
NEXT_PUBLIC_SOCKET_URL: ${NEXT_PUBLIC_SOCKET_URL}
NEXT_PUBLIC_SOCKET_PATH: ${NEXT_PUBLIC_SOCKET_PATH}
container_name: desktable-frontend
restart: unless-stopped
depends_on:
- backend
ports:
- "3000:3000"
networks:
- desktable
volumes:
postgres_data:
redis_data:
networks:
desktable:
driver: bridge
Example .env.production
POSTGRES_USER=desktable
POSTGRES_PASSWORD=replace-with-strong-password
POSTGRES_DB=desktable
JWT_SECRET_KEY=replace-with-64-char-random-secret
OPENAI_API_KEY=sk-replace-with-secret
OPENAI_MODEL=gpt-4o
CORS_ORIGINS=https://desktable.example.com
NEXT_PUBLIC_API_BASE_URL=https://desktable.example.com/api/v1
NEXT_PUBLIC_SOCKET_URL=https://desktable.example.com
NEXT_PUBLIC_SOCKET_PATH=/ws/socket.io
Next.js exposes only variables prefixed with NEXT_PUBLIC_ to browser code; all other
variables remain server-side.[nextjs]
Dockerfiles
Dockerfile.backend
FROM python:3.11-slim AS base
ENV PYTHONDONTWRITEBYTECODE=1 \
PYTHONUNBUFFERED=1 \
PIP_NO_CACHE_DIR=1
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends \
build-essential \
curl \
&& rm -rf /var/lib/apt/lists/*
COPY requirements.txt .
RUN pip install --upgrade pip && \
pip install -r requirements.txt
COPY . .
RUN useradd --create-home appuser && \
chown -R appuser:appuser /app
USER appuser
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
Dockerfile.frontend
FROM node:18-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
FROM node:18-alpine AS builder
WORKDIR /app
ARG NEXT_PUBLIC_API_BASE_URL
ARG NEXT_PUBLIC_SOCKET_URL
ARG NEXT_PUBLIC_SOCKET_PATH
ENV NEXT_PUBLIC_API_BASE_URL=$NEXT_PUBLIC_API_BASE_URL \
NEXT_PUBLIC_SOCKET_URL=$NEXT_PUBLIC_SOCKET_URL \
NEXT_PUBLIC_SOCKET_PATH=$NEXT_PUBLIC_SOCKET_PATH \
NEXT_TELEMETRY_DISABLED=1
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build
FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production \
NEXT_TELEMETRY_DISABLED=1
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/package-lock.json ./package-lock.json
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
RUN addgroup -g 1001 nodejs && \
adduser -u 1001 -G nodejs -s /bin/sh -D nextjs && \
chown -R nextjs:nodejs /app
USER nextjs
EXPOSE 3000
CMD ["npm", "run", "start"]
Build and Run
Development Stack
docker compose up --build
Services:
Service URL
Frontend http://localhost:3000
Backend API http://localhost:8000
OpenAPI docs http://localhost:8000
/docs
PostgreSQL localhost:5432
Redis localhost:6379
Production Build
docker compose -f docker-compose.yml --env-file .env.production up -d --build
View Logs
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f db
docker compose logs -f redis
Stop Stack
docker compose down
Stop and Remove Volumes
Warning: This deletes persisted PostgreSQL and Redis data.
docker compose down -v
Database Migrations
Alembic migrations should run before the backend accepts traffic.
Run Migrations in a Container
docker compose exec backend alembic upgrade head
Create a New Migration
docker compose exec backend alembic revision --autogenerate -m "add lesson progress
table"
Review the generated file in:
backend/alembic/versions/
Then apply it:
docker compose exec backend alembic upgrade head
Check Current Revision
docker compose exec backend alembic current
View Migration History
docker compose exec backend alembic history
Roll Back One Revision
docker compose exec backend alembic downgrade -1
Reverse Proxy Configuration
Use Nginx, Caddy, Traefik, or a cloud load balancer to terminate TLS and route traffic.
Nginx Example
upstream desktable_frontend {
server frontend:3000;
}
upstream desktable_backend {
server backend:8000;
}
server {
listen 80;
server_name desktable.example.com;
return 301 https://$host$request_uri;
}
server {
listen 443 ssl http2;
server_name desktable.example.com;
ssl_certificate /etc/letsencrypt/live/desktable.example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/desktable.example.com/privkey.pem;
location / {
proxy_pass http://desktable_frontend;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
}
location /api/ {
proxy_pass http://desktable_backend;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
}
location /ws/ {
proxy_pass http://desktable_backend;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_read_timeout 3600s;
proxy_send_timeout 3600s;
}
}
CORS Configuration
Set CORS_ORIGINS to the exact frontend origins permitted to call the API.
CORS_ORIGINS=https://desktable.example.com,https://app.desktable.example.com
Avoid * in production. The backend should allow credentials only for trusted origins,
especially because Socket.IO connections may use authentication cookies or bearer-token
payloads.
SSL and Secure Media
Use HTTPS in production for:
● REST API traffic.
● Socket.IO WebSocket upgrades.
● Browser microphone access.
● Browser camera access via MediaPipe and face-api.js.
Modern browsers generally require a secure context for camera and microphone access.
Serve DeskTable through HTTPS and configure the reverse proxy to preserve WebSocket
upgrade headers.
Required Nginx WebSocket settings:
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
Production Checklist
● Use PostgreSQL rather than SQLite.
● Set DEBUG=false.
● Set ENVIRONMENT=production.
● Use a long, random JWT_SECRET_KEY.
● Store secrets in a managed secret manager.
● Restrict CORS_ORIGINS to production domains.
● Run alembic upgrade head before deploying new backend versions.
● Enable HTTPS and redirect HTTP to HTTPS.
● Configure WebSocket proxy timeouts.
● Enable Redis persistence or use a managed Redis service.
● Configure database backups and restore tests.
● Set OpenAI usage and spend limits.
● Apply per-user and per-room rate limits for AI, TTS, and vision events.
● Disable or minimize vision-frame storage by default.
● Add structured logging, tracing, and error tracking.
● Monitor database connections, Redis memory, CPU, latency, and Socket.IO
connection counts.
● Run database migrations during a controlled deployment window.
● Use health checks for backend, frontend, database, and Redis.
● Configure log retention and PII-redaction policies.
Health Checks
Expose a backend health endpoint such as:
GET /api/v1/admin/health
Expected response:
{
"status": "healthy",
"database": "healthy",
"redis": "healthy",
"openai": "healthy",
"socketio": "healthy"
}
Use container-level health checks as well:
healthcheck:
test: ["CMD", "curl", "-f", "http://localhost:8000/api/v1/admin/health"]
interval: 30s
timeout: 5s
retries: 3
Scaling
Component Scaling Strategy
Frontend Run multiple stateless Next.js containers behind a load balancer or use a
managed hosting platform
Backend
API
Scale FastAPI replicas horizontally
Socket.IO Use Redis adapter/message queue and sticky sessions, or route
WebSocket connections consistently
PostgreSQL Use managed PostgreSQL with read replicas for reporting workloads
Redis Use managed Redis with persistence and eviction policy appropriate to
session data
AI calls Queue and throttle requests; use streaming to reduce perceived latency
Vision Limit frame rate, compress frames, and process client-side where possible
Rollback Procedure
1. Stop new deployments.
2. Identify the last stable backend image tag.
3. Redeploy the previous backend image.
4. If the database schema changed, roll back only if the migration is reversible and safe:
docker compose exec backend alembic downgrade -1
5. Verify:
docker compose ps
docker compose logs -f backend
curl -f https://desktable.example.com/api/v1/admin/health
6. Restore the previous frontend image if the API contract changed.
Below are the four deliverable documents for DeskTable. They are written as
GitHub-flavored Markdown and can be placed directly in the repository root or docs/
directory.
README.md
DeskTable — AI STEM Tutor
DeskTable is an interactive AI tutor that teaches STEM concepts live on a digital
chalkboard. Students can draw, solve problems, speak to the tutor, and receive real-time
Socratic guidance while the system observes their notebook or desk through a vision
pipeline.
Instead of simply returning answers, DeskTable guides students through reasoning steps,
highlights work on the whiteboard, explains concepts with synthesized voice, and adapts
hints based on handwritten work and verbal responses.
Key Features
● Interactive Chalkboard / Whiteboard — Draw equations, diagrams, graphs, and
annotations using Excalidraw and Fabric.js.
● Socratic AI Tutor — OpenAI-powered reasoning that asks guiding questions rather
than immediately giving final answers.
● Live Voice Interaction — Students can speak naturally; the tutor responds through
streamed text-to-speech audio.
● Desk Vision — MediaPipe and face-api.js support desk, notebook, gesture, and
attention-aware learning experiences.
● Real-Time Collaboration — Socket.IO synchronizes canvas actions, tutor drawing
steps, voice events, and AI responses.
● Lesson Progress — Lessons, attempts, session history, and mastery progress are
persisted through PostgreSQL and SQLAlchemy.
● Subscription Tiers — Pricing, entitlement, and access-tier management for free,
student, and premium plans.
● Admin Observability — System metrics, usage monitoring, and operational health
endpoints.
Tech Stack
Layer Technology Purpose
Backend Python 3.11, FastAPI REST APIs, dependency injection,
async services
Database PostgreSQL or SQLite, SQLAlchemy
2.0 Async
Persistent application data
Migrations Alembic Versioned database schema
migrations
Realtime python-socketio, Redis WebSocket events, rooms, pub/sub
coordination
AI OpenAI API Socratic tutoring, reasoning,
explanations
Vision MediaPipe, face-api.js Desk/notebook perception and
student tracking
Voice Browser speech APIs, backend TTS
service
Speech input and streamed tutor
audio
Frontend Next.js 14 App Router, React 18,
TypeScript
Application UI and routing
State Zustand Lightweight client-side state
management
Canvas Excalidraw, Fabric.js Interactive drawing and whiteboard
rendering
Math KaTeX Mathematical expression rendering
Animation GSAP Chalk-style drawing and tutor
animation effects
Testing PyTest Backend unit and integration tests
Project Structure
desktable/
├── backend/
│ ├── app/
│ │ ├── main.py # FastAPI application entrypoint
│ │ ├── core/
│ │ │ ├── config.py # Settings and environment configuration
│ │ │ ├── security.py # JWT, password hashing, auth utilities
│ │ │ └── dependencies.py # Shared FastAPI dependencies
│ │ ├── db/
│ │ │ ├── base.py # SQLAlchemy declarative base
│ │ │ ├── session.py # Async engine and session factory
│ │ │ └── redis.py # Redis connection management
│ │ ├── modules/
│ │ │ ├── auth/ # Login, registration, JWT management
│ │ │ ├── lessons/ # Lessons, progress, history
│ │ │ ├── ai/ # Socratic reasoning and OpenAI integration
│ │ │ ├── tts/ # Text-to-speech streaming
│ │ │ ├── vision/ # Vision ingestion and analysis
│ │ │ ├── subscription/ # Plans, entitlements, billing hooks
│ │ │ ├── admin/ # Metrics and system monitoring
│ │ │ └── websocket/ # Socket.IO events and agent coordination
│ │ ├── schemas/ # Pydantic request/response models
│ │ └── utils/ # Shared helpers
│ ├── alembic/
│ │ ├── versions/ # Migration files
│ │ └── env.py
│ ├── tests/
│ ├── alembic.ini
│ ├── pyproject.toml
│ ├── requirements.txt
│ └── .env.example
├── frontend/
│ ├── app/ # Next.js App Router pages and layouts
│ ├── components/
│ │ ├── whiteboard/ # Excalidraw / Fabric.js canvas components
│ │ ├── tutor/ # Chat, hints, explanations, voice UI
│ │ ├── vision/ # Camera and MediaPipe integrations
│ │ └── shared/ # Buttons, cards, modals, layout primitives
│ ├── lib/
│ │ ├── api.ts # Typed REST API client
│ │ ├── socket.ts # Socket.IO client
│ │ ├── tts.ts # Audio playback and streaming helpers
│ │ └── vision.ts # MediaPipe / face-api.js helpers
│ ├── stores/
│ │ ├── authStore.ts
│ │ ├── lessonStore.ts
│ │ ├── canvasStore.ts
│ │ ├── tutorStore.ts
│ │ └── visionStore.ts
│ ├── styles/
│ ├── package.json
│ ├── tailwind.config.ts
│ ├── tsconfig.json
│ └── .env.local.example
├── docker-compose.yml
├── Dockerfile.backend
├── Dockerfile.frontend
├── .github/
│ └── workflows/
├── docs/
│ ├── ARCHITECTURE.md
│ ├── API_REFERENCE.md
│ └── DEPLOYMENT.md
└── README.md
Prerequisites
Requirement Minimum Version Notes
Node.js 18+ Required for Next.js 14 frontend
npm 9+ Installed with Node.js
Python 3.11+ Required for FastAPI backend
PostgreSQL 14+ Recommended for local and production use
SQLite 3+ Acceptable for lightweight local development
Redis 6+ Required for Socket.IO coordination and
caching
OpenAI API key — Required for AI tutor functionality
Quickstart
1. Clone the repository
git clone https://github.com/your-org/desktable.git
cd desktable
2. Configure backend environment
cd backend
cp .env.example .env
Update .env with your local database, Redis, JWT, and OpenAI settings.
3. Install backend dependencies
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
4. Run database migrations
alembic upgrade head
5. Start the backend
uvicorn app.main:app --reload
By default, the API is available at:
http://localhost:8000
Interactive API documentation:
http://localhost:8000/docs
http://localhost:8000/redoc
6. Configure frontend environment
cd ../frontend
cp .env.local.example .env.local
Next.js exposes browser-accessible variables only when they are prefixed with
NEXT_PUBLIC_; server-only secrets should remain unprefixed.[nextjs]
7. Install frontend dependencies
npm install
8. Start the frontend
npm run dev
Open:
http://localhost:3000
Environment Variables
Backend — backend/.env
Variable Requi
red
Example Descript
ion
APP_NAME No DeskTable Applicati
on
display
name
ENVIRONMENT Yes local local,
stagin
g, or
produc
tion
DEBUG No true Enables
verbose
logging
and
debug
behavior
API_V1_PREFIX Yes /api/v1 Base
path for
REST
endpoint
s
DATABASE_URL Yes postgresql+asyncpg://postgres:postg
res@localhost:5432/desktable
Async
SQLAlch
emy
databas
e URL
SQLITE_DATABASE
_URL
No sqlite+aiosqlite:///./desktable.db Optional
SQLite
fallback
REDIS_URL Yes redis://localhost:6379/0 Redis
connecti
on URL
JWT_SECRET_KEY Yes change-me-in-production Secret
used to
sign
JWTs
JWT_ALGORITHM Yes HS256 JWT
signing
algorith
m
ACCESS_TOKEN_EX
PIRE_MINUTES
Yes 60 Access-t
oken
lifetime
REFRESH_TOKEN_E
XPIRE_DAYS
Yes 30 Refreshtoken
lifetime
OPENAI_API_KEY Yes sk-... OpenAI
API
credenti
al
OPENAI_MODEL No gpt-4o Default
reasonin
g model
OPENAI_EMBEDDIN
G_MODEL
No text-embedding-3-small Embeddi
ng
model
for
lesson
context
TTS_PROVIDER No openai TTS
provider
identifier
TTS_VOICE No alloy Default
tutor
voice
VISION_MAX_FPS No 5 Maximu
m
process
ed vision
frames
per
second
CORS_ORIGINS Yes http://localhost:3000 Commaseparate
d
allowed
origins
SOCKETIO_PATH Yes /ws/socket.io Socket.I
O
request
path
Frontend — frontend/.env.local
Variable Require
d
Example Description
NEXT_PUBLIC_API_BAS
E_URL
Yes http://localhost:8000
/api/v1
REST API base
URL
NEXT_PUBLIC_SOCKET_
URL
Yes http://localhost:8000 Socket.IO server
origin
NEXT_PUBLIC_SOCKET_
PATH
Yes /ws/socket.io Socket.IO path
NEXT_PUBLIC_APP_NAM
E
No DeskTable UI display name
NEXT_PUBLIC_ENABLE_
VISION
No true Enables camera
and vision features
NEXT_PUBLIC_ENABLE_
VOICE
No true Enables
microphone and
TTS features
NEXT_PUBLIC_SENTRY_
DSN
No https://... Optional frontend
error tracking
Running Tests
Backend tests use PyTest:
cd backend
pytest
Run tests with coverage:
pytest --cov=app --cov-report=term-missing
Useful Commands
Command Location Description
uvicorn app.main:app --reload backend/ Start FastAPI in development
mode
alembic upgrade head backend/ Apply all pending migrations
alembic revision
--autogenerate -m "message"
backend/ Create a new migration
pytest backend/ Run backend tests
npm run dev frontend/ Start Next.js development
server
npm run build frontend/ Create a production frontend
build
npm run lint frontend/ Run frontend lint checks
docker compose up --build Repository
root
Start full local Docker stack
Security Notes
● Never commit .env, .env.local, API keys, JWT secrets, or database credentials.
● Use strong, unique values for JWT_SECRET_KEY in every environment.
● Restrict CORS_ORIGINS to known frontend origins.
● Use HTTPS in production so WebSockets, microphone access, and camera access
work securely.
● Rotate OpenAI credentials and database passwords through your
secret-management system.
License
Specify your project license here, for example:
MIT License
ARCHITECTURE.md
DeskTable Architecture
DeskTable uses a decoupled Next.js frontend and FastAPI backend, connected through
REST APIs for transactional operations and Socket.IO for low-latency realtime tutoring. The
backend coordinates OpenAI reasoning, vision analysis, TTS streaming, Redis-backed
session state, and persistent lesson data.
High-Level Architecture
flowchart TB
subgraph Client["Browser / Student Device"]
UI["Next.js 14 App Router UI"]
Canvas["Excalidraw + Fabric.js Whiteboard"]
Mic["Microphone / Speech Input"]
Camera["Camera / MediaPipe / face-api.js"]
Audio["TTS Audio Player"]
Stores["Zustand Stores"]
end
subgraph Edge["FastAPI ASGI Application"]
REST["REST API /api/v1"]
WS["Socket.IO /ws/socket.io"]
Auth["Auth Module"]
Lessons["Lessons Module"]
AI["AI Module"]
TTS["TTS Module"]
Vision["Vision Module"]
Subs["Subscription Module"]
Admin["Admin Module"]
end
subgraph Services["External Services"]
OpenAI["OpenAI API"]
Redis["Redis"]
DB[("PostgreSQL / SQLite")]
end
UI --> REST
UI --> WS
Canvas --> WS
Mic --> WS
Camera --> WS
Audio --> UI
Stores --> UI
UI --> Canvas
UI --> Mic
UI --> Camera
REST --> Auth
REST --> Lessons
REST --> AI
REST --> TTS
REST --> Vision
REST --> Subs
REST --> Admin
WS --> AI
WS --> TTS
WS --> Vision
WS --> Lessons
AI --> OpenAI
AI --> Redis
TTS --> OpenAI
Vision --> Redis
Lessons --> DB
Auth --> DB
Subs --> DB
Admin --> Redis
Core Design Principles
● Realtime-first tutoring — Canvas strokes, voice events, AI responses, and tutor
drawing actions are streamed through Socket.IO.
● Socratic by default — The AI module is designed to produce questions, partial
hints, and step-by-step guidance rather than direct answers.
● Separation of concerns — Each domain module owns its routes, schemas,
services, and persistence concerns.
● Async throughout — FastAPI, SQLAlchemy, Redis, OpenAI calls, and Socket.IO
handlers use async I/O.
● Ephemeral versus durable state — Active tutoring context lives in Redis;
completed lessons and progress live in the relational database.
● Progressive disclosure — The tutor reveals concepts gradually based on student
responses, canvas activity, and vision context.
Realtime Interaction Pipeline
sequenceDiagram
autonumber
participant S as Student Browser
participant C as Whiteboard Canvas
participant WS as Socket.IO Client
participant API as FastAPI Socket Server
participant AI as AI Service
participant OAI as OpenAI API
participant VIS as Vision Service
participant TTS as TTS Service
participant R as Redis
S->>C: Draws equation or diagram
C->>WS: canvas_draw event
WS->>API: Emit canvas_draw
API->>R: Store latest canvas context / room state
S->>WS: user_voice_input
WS->>API: Emit user_voice_input
API->>AI: Build tutoring context
AI->>OAI: Request Socratic response
alt Vision enabled
S->>WS: vision_frame
WS->>API: Emit vision_frame
API->>VIS: Analyze handwriting / desk context
VIS->>R: Cache frame analysis
VIS-->>API: Vision insight
API->>AI: Enrich prompt with vision context
end
OAI-->>AI: Streaming model response
AI-->>API: Reasoning chunks and drawing plan
API-->>WS: ai_response_chunk
API-->>WS: socratic_hint
API-->>WS: draw_step
WS-->>C: Render tutor drawing animation
API->>TTS: Convert explanation to speech
TTS-->>API: tts_audio_chunk
API-->>WS: tts_audio_chunk
WS-->>S: Play streamed tutor audio
Pipeline Stages
Stage Component Responsibility
Capture Excalidraw / Fabric.js Captures strokes, shapes, equations, and
annotations
Transport Socket.IO client Sends canvas, voice, and vision events to the
backend
Session
coordination
FastAPI WebSocket
module
Validates JWT/session, joins rooms, routes
events
Context
assembly
AI module Combines lesson metadata, canvas state, chat
history, and vision insights
Reasoning OpenAI API Produces streamed Socratic explanations and
structured drawing steps
Perception Vision module Interprets notebook/desk frames and attention
signals
Expression TTS module Streams tutor explanation audio to the browser
Persistence SQLAlchemy Saves lesson sessions, messages, progress,
and completed artifacts
Ephemeral state Redis Stores active room state, rate limits, streams,
and transient AI context
Socket.IO Integration
The Socket.IO ASGI application is mounted alongside FastAPI at /ws/socket.io. A
typical integration wraps the FastAPI app in a socketio.ASGIApp and mounts it under
/ws, with the Socket.IO path configured as /ws/socket.io.[fastapi-practices.github]
# backend/app/main.py
import socketio
from fastapi import FastAPI
from app.modules.websocket.events import register_socket_events
app = FastAPI(title="DeskTable API")
sio = socketio.AsyncServer(
async_mode="asgi",
cors_allowed_origins=settings.CORS_ORIGINS,
)
socket_app = socketio.ASGIApp(
socketio_server=sio,
other_asgi_app=app,
socketio_path="/ws/socket.io",
)
register_socket_events(sio)
app.mount("/ws", socket_app)
The frontend connects using the matching path:
// frontend/lib/socket.ts
import { io } from "socket.io-client";
export const socket = io(process.env.NEXT_PUBLIC_SOCKET_URL!, {
path: process.env.NEXT_PUBLIC_SOCKET_PATH ?? "/ws/socket.io",
transports: ["websocket"],
autoConnect: false,
withCredentials: true,
});
Frontend State Management
Zustand provides focused client-side stores. Each store owns a narrow slice of UI state and
communicates with services through typed API and Socket.IO clients.
Store Owns Updated By
authStore Access token, refresh token,
current user, auth status
REST auth endpoints
lessonStore Active lesson, lesson list, progress,
history
REST lesson endpoints and
Socket.IO session events
canvasStore Canvas elements, selected tools,
undo/redo stack, tutor drawing
queue
Local canvas actions and
draw_step events
tutorStore Chat transcript, streaming
response, hints, tutor status
ai_response_chunk,
socratic_hint,
tts_audio_chunk
visionStore Camera permission, tracking
status, latest analysis, privacy
mode
Local MediaPipe pipeline and
vision_analysis events
subscriptionS
tore
Current plan, entitlements, usage
limits
REST subscription endpoints
Example Zustand Store
// frontend/stores/tutorStore.ts
import { create } from "zustand";
type TutorMessage = {
id: string;
role: "student" | "tutor";
content: string;
createdAt: string;
};
type TutorState = {
messages: TutorMessage[];
isThinking: boolean;
currentHint: string | null;
appendChunk: (chunk: string) => void;
setHint: (hint: string | null) => void;
setThinking: (thinking: boolean) => void;
reset: () => void;
};
export const useTutorStore = create<TutorState>((set) => ({
messages: [],
isThinking: false,
currentHint: null,
appendChunk: (chunk) =>
set((state) => {
const last = state.messages[state.messages.length - 1];
if (!last || last.role !== "tutor") {
return {
messages: [
...state.messages,
{
id: crypto.randomUUID(),
role: "tutor",
content: chunk,
createdAt: new Date().toISOString(),
},
],
};
}
const updated = [...state.messages];
updated[updated.length - 1] = {
...last,
content: last.content + chunk,
};
return { messages: updated };
}),
setHint: (currentHint) => set({ currentHint }),
setThinking: (isThinking) => set({ isThinking }),
reset: () => set({ messages: [], isThinking: false, currentHint: null }),
}));
Backend State Model
State Type Store Lifetime Examples
Identity PostgreSQL /
SQLite
Long-lived Users, credentials, roles
Learning
record
PostgreSQL /
SQLite
Long-lived Lessons, sessions, messages,
progress
Billing state PostgreSQL /
SQLite
Long-lived Plans, subscriptions, entitlements
Active room Redis Minutes to
hours
Room membership, connection IDs,
canvas version
Tutoring
context
Redis Session-scope
d
Recent messages, current problem,
hint level
Stream state Redis Seconds to
minutes
Active AI stream, TTS sequence,
partial transcription
Rate limits Redis Short-lived AI request limits, vision-frame limits
AI Tutoring Flow
flowchart LR
A[Student Action] --> B{Input Type}
B -- Canvas --> C[Canvas Context Extractor]
B -- Voice --> D[Speech / Transcript Handler]
B -- Vision --> E[Vision Analyzer]
B -- Text --> F[Chat Message Handler]
C --> G[Context Builder]
D --> G
E --> G
F --> G
G --> H[Socratic Prompt Chain]
H --> I[OpenAI Streaming Completion]
I --> J{Response Type}
J -- Explanation --> K[ai_response_chunk]
J -- Guided question --> L[socratic_hint]
J -- Board action --> M[draw_step]
J -- Narration --> N[TTS stream]
Socratic Prompt Chain
The AI module builds prompts in layers:
1. System layer — Defines the tutor persona, STEM subject, safety constraints, and
Socratic behavior.
2. Lesson layer — Supplies topic, learning objective, difficulty, prior progress, and
curriculum alignment.
3. Canvas layer — Supplies a compact representation of current drawings, equations,
labels, and selected objects.
4. Vision layer — Supplies privacy-safe observations such as detected handwriting
regions, work-in-progress signals, or attention cues.
5. Conversation layer — Supplies recent student/tutor turns and prior hints.
6. Output contract — Requests separate fields for explanation, next question, hint
level, and optional whiteboard drawing actions.
Example response contract:
{
"explanation": "Let's isolate the unknown variable step by step.",
"socratic_question": "What operation can we apply to both sides to remove the +7?",
"hint_level": 1,
"draw_steps": [
{
"type": "text",
"content": "x + 7 = 12",
"position": { "x": 120, "y": 100 }
},
{
"type": "arrow",
"from": { "x": 180, "y": 130 },
"to": { "x": 240, "y": 130 },
"label": "- 7"
}
],
"tts_text": "We have x plus seven equals twelve. What could we subtract from both sides?"
}
Data Flow Summary
Flow Trigger Path Result
Canvas
collaboration
Student draws Canvas → canvas_draw
→ Socket.IO → Redis →
peers
All participants see
synchronized board
state
Socratic
tutoring
Student asks or
submits work
Socket.IO → AI service →
OpenAI → Socket.IO
Streamed explanation,
hint, and drawing steps
Voice
explanation
AI produces
narration
AI → TTS service →
Socket.IO → browser audio
Tutor speaks while
drawing
Vision-assiste
d help
Camera frame
captured
Browser → vision_frame
→ Vision service → AI
Context-aware
feedback
Lesson
completion
Tutoring session
ends
Socket.IO/REST → Lessons
service → Database
Persisted transcript,
progress, and artifacts
Access
enforcement
API or
WebSocket
request
Auth + subscription modules Allowed or denied
based on identity and
plan
Scalability Considerations
● Run multiple FastAPI/Socket.IO workers behind a load balancer with sticky sessions
or use the Redis message queue for cross-worker Socket.IO events.
● Use Redis pub/sub or Redis Streams for fan-out of AI and TTS events across
horizontally scaled instances.
● Process vision frames at a bounded frame rate and prefer client-side preprocessing
to reduce bandwidth.
● Cache lesson context and repeated AI prompts where appropriate, while avoiding
caching personal student data longer than necessary.
● Use database read replicas for analytics-heavy admin queries.
● Offload long-running AI generation to background workers when response time or
reliability requirements demand it.
API_REFERENCE.md
DeskTable API Reference
All REST endpoints are versioned under:
/api/v1
Unless stated otherwise:
● Requests and responses use application/json.
● Authentication uses a bearer access token.
● Timestamps are ISO 8601 UTC strings.
● IDs are UUID strings.
Authentication Header
Authorization: Bearer <access_token>
Standard Error Format
{
"detail": {
"code": "lesson_not_found",
"message": "The requested lesson does not exist.",
"request_id": "b3f2c7d4-8e91-4f2a-9d44-6d7a1b9f1234"
}
}
HTTP Status Meaning
200 Success
201 Resource created
204 Success with no response body
400 Invalid request
401 Missing or invalid authentication
403 Authenticated but not permitted
404 Resource not found
409 Conflict, such as duplicate
resource
422 Validation error
429 Rate limit exceeded
500 Internal server error
Auth — /api/v1/auth
Handles registration, login, token refresh, profile access, and logout.
Register User
POST /api/v1/auth/register
Request
{
"email": "student@example.com",
"password": "StrongPassw0rd!",
"full_name": "Ada Lovelace",
"grade_level": "grade_10"
}
Response — 201 Created
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"grade_level": "grade_10",
"created_at": "2026-10-14T09:00:00Z"
}
Login
POST /api/v1/auth/login
Request
{
"email": "student@example.com",
"password": "StrongPassw0rd!"
}
Response — 200 OK
{
"access_token": "eyJhbGciOiJIUzI1NiIs...",
"refresh_token": "eyJhbGciOiJIUzI1NiIs...",
"token_type": "bearer",
"expires_in": 3600,
"user": {
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace"
}
}
Refresh Token
POST /api/v1/auth/refresh
Request
{
"refresh_token": "eyJhbGciOiJIUzI1NiIs..."
}
Response — 200 OK
{
"access_token": "eyJhbGciOiJIUzI1NiIs...",
"token_type": "bearer",
"expires_in": 3600
}
Get Current User
GET /api/v1/auth/me
Response — 200 OK
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"grade_level": "grade_10",
"subscription_tier": "premium",
"created_at": "2026-10-14T09:00:00Z"
}
Logout
POST /api/v1/auth/logout
Request
{
"refresh_token": "eyJhbGciOiJIUzI1NiIs..."
}
Response — 204 No Content
Lessons — /api/v1/lessons
Manages lesson planning, tutoring sessions, progress, and history.
List Lessons
GET /api/v1/lessons
Query Parameters
Parameter Type Required Description
subject string No math, physics, chemistry, biology,
computer_science
status string No draft, published, archived
difficul
ty
string No beginner, intermediate, advanced
page integer No Page number, default 1
page_siz
e
integer No Items per page, default 20, maximum 100
Response — 200 OK
{
"items": [
{
"id": "1c3f4b7e-0d21-4c7a-8f10-9d8e7c6b5a43",
"title": "Solving Linear Equations",
"subject": "math",
"difficulty": "beginner",
"summary": "Learn to isolate variables using inverse operations.",
"estimated_minutes": 25,
"status": "published"
}
],
"page": 1,
"page_size": 20,
"total": 42
}
Create Lesson
POST /api/v1/lessons
Request
{
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"summary": "Understand force, mass, and acceleration relationships.",
"learning_objectives": [
"Interpret F = ma",
"Solve for acceleration",
"Apply units correctly"
],
"estimated_minutes": 30
}
Response — 201 Created
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"status": "draft",
"created_at": "2026-10-14T09:12:00Z"
}
Get Lesson
GET /api/v1/lessons/{lesson_id}
Response — 200 OK
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law",
"subject": "physics",
"difficulty": "intermediate",
"summary": "Understand force, mass, and acceleration relationships.",
"learning_objectives": [
"Interpret F = ma",
"Solve for acceleration",
"Apply units correctly"
],
"estimated_minutes": 30,
"status": "published"
}
Update Lesson
PATCH /api/v1/lessons/{lesson_id}
Request
{
"title": "Newton's Second Law and Free-Body Diagrams",
"status": "published"
}
Response — 200 OK
{
"id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"title": "Newton's Second Law and Free-Body Diagrams",
"status": "published",
"updated_at": "2026-10-14T09:20:00Z"
}
Delete Lesson
DELETE /api/v1/lessons/{lesson_id}
Response — 204 No Content
Start Tutoring Session
POST /api/v1/lessons/{lesson_id}/sessions
Request
{
"mode": "socratic",
"enable_voice": true,
"enable_vision": true
}
Response — 201 Created
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"status": "active",
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"started_at": "2026-10-14T09:25:00Z"
}
Get Session
GET /api/v1/lessons/sessions/{session_id}
Response — 200 OK
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"status": "active",
"message_count": 8,
"started_at": "2026-10-14T09:25:00Z",
"ended_at": null
}
Complete Session
POST /api/v1/lessons/sessions/{session_id}/complete
Request
{
"outcome": "completed",
"mastery_score": 0.82,
"notes": "Student successfully solved three linear equations."
}
Response — 200 OK
{
"id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"status": "completed",
"outcome": "completed",
"mastery_score": 0.82,
"ended_at": "2026-10-14T09:55:00Z"
}
Get Student Progress
GET /api/v1/lessons/progress
Response — 200 OK
{
"overall_mastery": 0.74,
"subjects": [
{
"subject": "math",
"mastery": 0.81,
"lessons_completed": 12
},
{
"subject": "physics",
"mastery": 0.66,
"lessons_completed": 7
}
]
}
AI — /api/v1/ai
Provides direct AI endpoints for non-streaming requests, tutor context creation, and
explanation generation. Most live tutoring traffic should use Socket.IO.
Create Tutor Context
POST /api/v1/ai/context
Request
{
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"problem": "Solve for x: 2x + 5 = 17",
"student_level": "beginner"
}
Response — 201 Created
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"expires_at": "2026-10-14T10:25:00Z"
}
Generate Explanation
POST /api/v1/ai/explain
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"question": "Why do we subtract 5 from both sides?",
"style": "socratic"
}
Response — 200 OK
{
"explanation": "Subtracting 5 keeps the equation balanced while isolating the term
containing x.",
"socratic_question": "What happens to the left side if we subtract 5 from both sides?",
"hint_level": 1,
"tokens_used": 412
}
Generate Hints
POST /api/v1/ai/hints
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"current_answer": "x = 5",
"hint_level": 2
}
Response — 200 OK
{
"hints": [
"First move the constant to the right-hand side.",
"Then divide both sides by the coefficient of x."
],
"next_question": "What should you divide both sides by?"
}
Evaluate Student Work
POST /api/v1/ai/evaluate
Request
{
"context_id": "ctx_7f2b9d4a-1c83-4f6e-9a20-5b7c8d9e0f11",
"student_work": "2x + 5 = 17\n2x = 12\nx = 6",
"expected_answer": "x = 6"
}
Response — 200 OK
{
"correct": true,
"score": 1.0,
"feedback": "Correct. Your steps are clear and mathematically valid.",
"misconceptions": []
}
TTS — /api/v1/tts
Converts tutor text into audio. Live sessions normally receive audio through
tts_audio_chunk Socket.IO events, while REST is useful for previews and offline lesson
audio.
Synthesize Speech
POST /api/v1/tts/synthesize
Request
{
"text": "Let's isolate x by subtracting five from both sides.",
"voice": "alloy",
"format": "mp3"
}
Response — 200 OK
Content-Type: audio/mpeg
Returns binary audio content.
List Voices
GET /api/v1/tts/voices
Response — 200 OK
{
"voices": [
{
"id": "alloy",
"name": "Alloy",
"language": "en-US",
"gender": "neutral"
},
{
"id": "nova",
"name": "Nova",
"language": "en-US",
"gender": "female"
}
]
}
Create Streaming TTS Session
POST /api/v1/tts/stream
Request
{
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"text": "We can solve this by using inverse operations.",
"voice": "alloy"
}
Response — 202 Accepted
{
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"status": "streaming",
"transport": "websocket"
}
Audio chunks are delivered through the Socket.IO event tts_audio_chunk.
Vision — /api/v1/vision
Supports image analysis, vision-session configuration, and privacy controls. Camera frames
are normally streamed through Socket.IO; REST endpoints handle configuration, snapshots,
and results.
Create Vision Session
POST /api/v1/vision/sessions
Request
{
"lesson_session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"modalities": ["notebook", "handwriting", "attention"],
"max_fps": 5
}
Response — 201 Created
{
"id": "vis_4e3d2c1b-0a99-4f88-8e77-6d5c4b3a2f10",
"status": "active",
"max_fps": 5,
"privacy_mode": "standard"
}
Analyze Snapshot
POST /api/v1/vision/analyze
Request
Content-Type: multipart/form-data
Field Type Required Description
image file Yes JPEG, PNG, or WebP image
session_
id
string Yes Vision session ID
modaliti
es
string No Comma-separated analysis
modes
Response — 200 OK
{
"analysis_id": "an_1a2b3c4d-5e6f-4a7b-8c9d-0e1f2a3b4c5d",
"handwriting_detected": true,
"equation_candidates": ["2x + 5 = 17"],
"work_in_progress": true,
"attention_score": 0.91,
"suggestions": [
"Ask the student to label the operation applied to both sides."
]
}
Update Privacy Mode
PATCH /api/v1/vision/sessions/{vision_session_id}
Request
{
"privacy_mode": "strict",
"store_frames": false
}
Response — 200 OK
{
"id": "vis_4e3d2c1b-0a99-4f88-8e77-6d5c4b3a2f10",
"privacy_mode": "strict",
"store_frames": false
}
Delete Vision Session
DELETE /api/v1/vision/sessions/{vision_session_id}
Response — 204 No Content
Subscription — /api/v1/subscription
Manages available plans, current entitlements, and subscription changes.
List Plans
GET /api/v1/subscription/plans
Response — 200 OK
{
"plans": [
{
"id": "free",
"name": "Free",
"price_monthly": 0,
"currency": "USD",
"features": [
"3 tutoring sessions per week",
"Basic whiteboard",
"Text-based AI hints"
]
},
{
"id": "student",
"name": "Student",
"price_monthly": 9.99,
"currency": "USD",
"features": [
"Unlimited tutoring sessions",
"Voice tutor",
"Desk vision",
"Lesson history"
]
},
{
"id": "premium",
"name": "Premium",
"price_monthly": 19.99,
"currency": "USD",
"features": [
"All Student features",
"Advanced subjects",
"Priority AI capacity",
"Parent progress reports"
]
}
]
}
Get Current Subscription
GET /api/v1/subscription/me
Response — 200 OK
{
"plan_id": "student",
"status": "active",
"current_period_end": "2026-11-14T00:00:00Z",
"entitlements": {
"voice_tutor": true,
"desk_vision": true,
"advanced_subjects": false,
"max_sessions_per_month": 100
}
}
Change Plan
POST /api/v1/subscription/change-plan
Request
{
"plan_id": "premium"
}
Response — 202 Accepted
{
"subscription_id": "sub_8f7e6d5c-4b3a-4928-8172-6f5e4d3c2b1a",
"status": "pending",
"requested_plan": "premium"
}
Cancel Subscription
POST /api/v1/subscription/cancel
Response — 200 OK
{
"subscription_id": "sub_8f7e6d5c-4b3a-4928-8172-6f5e4d3c2b1a",
"status": "canceling",
"effective_at": "2026-11-14T00:00:00Z"
}
Admin — /api/v1/admin
Administrative endpoints require an admin role.
System Health
GET /api/v1/admin/health
Response — 200 OK
{
"status": "healthy",
"database": "healthy",
"redis": "healthy",
"openai": "healthy",
"socketio": "healthy",
"version": "1.0.0"
}
Platform Metrics
GET /api/v1/admin/metrics
Query Parameters
Parameter Type Required Description
from string No ISO 8601 start
timestamp
to string No ISO 8601 end timestamp
granulari
ty
string No hour, day, week
Response — 200 OK
{
"active_users": 1284,
"active_sessions": 187,
"lessons_completed": 431,
"ai_requests": 15230,
"tts_minutes": 942.5,
"vision_frames_processed": 284100,
"error_rate": 0.008
}
List Users
GET /api/v1/admin/users
Query Parameters
Parameter Type Required Description
email string No Filter by email
subscription_
tier
string No Filter by plan
page integer No Page number
page_size integer No Page size
Response — 200 OK
{
"items": [
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"email": "student@example.com",
"full_name": "Ada Lovelace",
"subscription_tier": "premium",
"is_active": true
}
],
"total": 1
}
WebSocket Protocol
Namespace / endpoint:
/ws/socket.io
The Socket.IO server is mounted as an ASGI sub-application alongside FastAPI, allowing
HTTP and WebSocket traffic to share one deployment.[fastapi-practices.github]
Connection
import { io } from "socket.io-client";
const socket = io("http://localhost:8000", {
path: "/ws/socket.io",
auth: {
token: accessToken,
},
transports: ["websocket"],
});
Authentication Payload
Field Type Required Description
token string Yes JWT access token
session_id string No Active tutoring session ID
client_vers
ion
string No Frontend build/version identifier
Common Error Event
Server → Client: socket_error
{
"code": "unauthorized",
"message": "Invalid or expired access token.",
"retryable": false
}
Client-to-Server Events
join_room
Joins a lesson or tutoring room.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"role": "student"
}
Response: room_joined
leave_room
Leaves the current room.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8"
}
Response: room_left
canvas_draw
Sends a drawing action from the student whiteboard.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "draw_01",
"type": "stroke",
"tool": "pen",
"color": "#111111",
"width": 3,
"points": [
{ "x": 120, "y": 240 },
{ "x": 132, "y": 238 },
{ "x": 150, "y": 230 }
],
"timestamp": "2026-10-14T09:30:12.480Z"
}
Server behavior
● Validates the room and user.
● Stores the latest canvas version in Redis.
● Broadcasts canvas_draw to other room participants.
● Optionally forwards canvas context to the AI service.
canvas_clear
Clears the board.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "clear_01"
}
Response: canvas_cleared
canvas_undo
Requests an undo action.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "undo_01"
}
Response: canvas_undo
vision_frame
Sends a camera or desk frame for analysis.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"frame_id": "frame_042",
"format": "jpeg",
"encoding": "base64",
"width": 640,
"height": 480,
"data": "<base64-encoded-image>",
"modalities": ["handwriting", "attention"]
}
Server behavior
● Applies rate limits and privacy policy.
● Sends the frame to the vision pipeline.
● Emits vision_analysis and may enrich the next AI prompt.
user_voice_input
Sends transcribed student speech or raw audio metadata.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"input_id": "voice_001",
"transcript": "I do not understand how to isolate x.",
"audio_duration_ms": 2400,
"language": "en-US"
}
Server behavior
● Adds the utterance to tutoring context.
● Requests a Socratic response from OpenAI.
● Streams ai_response_chunk, socratic_hint, draw_step, and
tts_audio_chunk.
tutor_request
Requests AI help with explicit text.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"message": "Can you explain the next step?",
"mode": "socratic",
"include_canvas": true,
"include_vision": true
}
Response: ai_response_started, ai_response_chunk, ai_response_completed
hint_request
Requests a hint at a specific level.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "hint_001",
"hint_level": 2,
"problem": "Solve for x: 2x + 5 = 17"
}
Response: socratic_hint
tts_request
Requests spoken narration for a specific explanation.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "tts_001",
"text": "Subtract five from both sides to isolate the term with x.",
"voice": "alloy"
}
Response: tts_started, tts_audio_chunk, tts_completed
typing
Indicates that a participant is typing.
Payload
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"is_typing": true
}
Response: participant_typing
Server-to-Client Events
room_joined
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"role": "student",
"participants": [
{
"id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"role": "student"
}
]
}
room_left
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10"
}
participant_joined
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant": {
"id": "f1e2d3c4-b5a6-4789-8123-456789abcdef",
"role": "tutor"
}
}
participant_left
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "f1e2d3c4-b5a6-4789-8123-456789abcdef"
}
participant_typing
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"participant_id": "b6f4d1d8-72e4-4b9a-8f21-4d0e2f1b9a10",
"is_typing": true
}
canvas_draw
Broadcasts a drawing action to room participants.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "draw_01",
"origin": "student",
"type": "stroke",
"tool": "pen",
"color": "#111111",
"width": 3,
"points": [
{ "x": 120, "y": 240 },
{ "x": 132, "y": 238 }
]
}
canvas_cleared
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"origin": "tutor"
}
canvas_undo
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"action_id": "undo_01",
"origin": "student"
}
ai_response_started
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"model": "gpt-4o",
"mode": "socratic"
}
ai_response_chunk
Streams a partial tutor response.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"chunk_index": 3,
"delta": "Subtract 5 from both sides.",
"done": false
}
ai_response_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"full_response": "We can isolate x by subtracting 5 from both sides, then dividing by 2.",
"tokens_used": 388,
"done": true
}
socratic_hint
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "hint_001",
"hint_level": 2,
"hint": "Move the constant term to the other side first.",
"question": "What operation removes +5 from the left-hand side?"
}
draw_step
Instructs the whiteboard to render a tutor-generated action.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"request_id": "req_001",
"step_index": 1,
"action": {
"type": "text",
"content": "2x + 5 - 5 = 17 - 5",
"position": { "x": 140, "y": 180 },
"style": {
"font": "chalk",
"color": "#ffffff"
}
},
"animation": {
"type": "chalk_write",
"duration_ms": 900
}
}
vision_analysis
Returns privacy-safe insights from a vision frame.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"frame_id": "frame_042",
"handwriting_detected": true,
"equation_candidates": ["2x + 5 = 17"],
"work_in_progress": true,
"attention_score": 0.91,
"suggestions": [
"Ask the student what operation removes +5."
]
}
tts_started
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"request_id": "tts_001",
"voice": "alloy",
"format": "mp3"
}
tts_audio_chunk
Streams encoded audio for browser playback.
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"sequence": 4,
"format": "mp3",
"encoding": "base64",
"data": "<base64-audio-chunk>",
"done": false
}
tts_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"stream_id": "tts_9d8c7b6a-5e43-4f21-8a90-1b2c3d4e5f60",
"duration_ms": 4200,
"done": true
}
lesson_progress_updated
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"lesson_id": "8a2c9f31-4d77-4f3e-91c2-7d5b6a4e3f21",
"progress": 0.65,
"mastery_score": 0.78
}
session_completed
{
"room_id": "lesson-8a2c9f31-user-b6f4d1d8",
"session_id": "d91a7c22-5b63-4e0f-9d31-2a8f7c6b5e10",
"outcome": "completed",
"ended_at": "2026-10-14T09:55:00Z"
}
DEPLOYMENT.md
DeskTable Deployment Guide
This guide covers Docker-based deployment, database migrations, environment
configuration, reverse-proxy setup, and production hardening for DeskTable.
Deployment Topology
flowchart LR
User["Student Browser"] --> LB["HTTPS Load Balancer / Reverse Proxy"]
LB --> FE["Next.js Frontend Container"]
LB --> BE["FastAPI + Socket.IO Backend Container"]
FE --> BE
BE --> DB[("PostgreSQL")]
BE --> RD[("Redis")]
BE --> OAI["OpenAI API"]
subgraph Optional["Optional Production Services"]
OBJ["Object Storage"]
MON["Monitoring / Logging"]
CDN["CDN"]
end
FE --> CDN
BE --> OBJ
BE --> MON
Docker Compose
Example docker-compose.yml
version: "3.9"
services:
db:
image: postgres:16-alpine
container_name: desktable-db
restart: unless-stopped
environment:
POSTGRES_USER: ${POSTGRES_USER}
POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
POSTGRES_DB: ${POSTGRES_DB}
volumes:
- postgres_data:/var/lib/postgresql/data
healthcheck:
test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
interval: 10s
timeout: 5s
retries: 5
networks:
- desktable
redis:
image: redis:7-alpine
container_name: desktable-redis
restart: unless-stopped
command: ["redis-server", "--appendonly", "yes"]
volumes:
- redis_data:/data
healthcheck:
test: ["CMD", "redis-cli", "ping"]
interval: 10s
timeout: 5s
retries: 5
networks:
- desktable
backend:
build:
context: ./backend
dockerfile: ../Dockerfile.backend
container_name: desktable-backend
restart: unless-stopped
depends_on:
db:
condition: service_healthy
redis:
condition: service_healthy
environment:
ENVIRONMENT: production
DEBUG: "false"
API_V1_PREFIX: /api/v1
DATABASE_URL:
postgresql+asyncpg://${POSTGRES_USER}:${POSTGRES_PASSWORD}@db:5432/${PO
STGRES_DB}
REDIS_URL: redis://redis:6379/0
JWT_SECRET_KEY: ${JWT_SECRET_KEY}
JWT_ALGORITHM: HS256
ACCESS_TOKEN_EXPIRE_MINUTES: 60
REFRESH_TOKEN_EXPIRE_DAYS: 30
OPENAI_API_KEY: ${OPENAI_API_KEY}
OPENAI_MODEL: ${OPENAI_MODEL}
CORS_ORIGINS: ${CORS_ORIGINS}
SOCKETIO_PATH: /ws/socket.io
ports:
- "8000:8000"
networks:
- desktable
frontend:
build:
context: ./frontend
dockerfile: ../Dockerfile.frontend
args:
NEXT_PUBLIC_API_BASE_URL: ${NEXT_PUBLIC_API_BASE_URL}
NEXT_PUBLIC_SOCKET_URL: ${NEXT_PUBLIC_SOCKET_URL}
NEXT_PUBLIC_SOCKET_PATH: ${NEXT_PUBLIC_SOCKET_PATH}
container_name: desktable-frontend
restart: unless-stopped
depends_on:
- backend
ports:
- "3000:3000"
networks:
- desktable
volumes:
postgres_data:
redis_data:
networks:
desktable:
driver: bridge
Example .env.production
POSTGRES_USER=desktable
POSTGRES_PASSWORD=replace-with-strong-password
POSTGRES_DB=desktable
JWT_SECRET_KEY=replace-with-64-char-random-secret
OPENAI_API_KEY=sk-replace-with-secret
OPENAI_MODEL=gpt-4o
CORS_ORIGINS=https://desktable.example.com
NEXT_PUBLIC_API_BASE_URL=https://desktable.example.com/api/v1
NEXT_PUBLIC_SOCKET_URL=https://desktable.example.com
NEXT_PUBLIC_SOCKET_PATH=/ws/socket.io
Next.js exposes only variables prefixed with NEXT_PUBLIC_ to browser code; all other
variables remain server-side.[nextjs]
Dockerfiles
Dockerfile.backend
FROM python:3.11-slim AS base
ENV PYTHONDONTWRITEBYTECODE=1 \
PYTHONUNBUFFERED=1 \
PIP_NO_CACHE_DIR=1
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends \
build-essential \
curl \
&& rm -rf /var/lib/apt/lists/*
COPY requirements.txt .
RUN pip install --upgrade pip && \
pip install -r requirements.txt
COPY . .
RUN useradd --create-home appuser && \
chown -R appuser:appuser /app
USER appuser
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
Dockerfile.frontend
FROM node:18-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
FROM node:18-alpine AS builder
WORKDIR /app
ARG NEXT_PUBLIC_API_BASE_URL
ARG NEXT_PUBLIC_SOCKET_URL
ARG NEXT_PUBLIC_SOCKET_PATH
ENV NEXT_PUBLIC_API_BASE_URL=$NEXT_PUBLIC_API_BASE_URL \
NEXT_PUBLIC_SOCKET_URL=$NEXT_PUBLIC_SOCKET_URL \
NEXT_PUBLIC_SOCKET_PATH=$NEXT_PUBLIC_SOCKET_PATH \
NEXT_TELEMETRY_DISABLED=1
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build
FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production \
NEXT_TELEMETRY_DISABLED=1
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/package-lock.json ./package-lock.json
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
RUN addgroup -g 1001 nodejs && \
adduser -u 1001 -G nodejs -s /bin/sh -D nextjs && \
chown -R nextjs:nodejs /app
USER nextjs
EXPOSE 3000
CMD ["npm", "run", "start"]
Build and Run
Development Stack
docker compose up --build
Services:
Service URL
Frontend http://localhost:3000
Backend API http://localhost:8000
OpenAPI docs http://localhost:8000
/docs
PostgreSQL localhost:5432
Redis localhost:6379
Production Build
docker compose -f docker-compose.yml --env-file .env.production up -d --build
View Logs
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f db
docker compose logs -f redis
Stop Stack
docker compose down
Stop and Remove Volumes
Warning: This deletes persisted PostgreSQL and Redis data.
docker compose down -v
Database Migrations
Alembic migrations should run before the backend accepts traffic.
Run Migrations in a Container
docker compose exec backend alembic upgrade head
Create a New Migration
docker compose exec backend alembic revision --autogenerate -m "add lesson progress
table"
Review the generated file in:
backend/alembic/versions/
Then apply it:
docker compose exec backend alembic upgrade head
Check Current Revision
docker compose exec backend alembic current
View Migration History
docker compose exec backend alembic history
Roll Back One Revision
docker compose exec backend alembic downgrade -1
Reverse Proxy Configuration
Use Nginx, Caddy, Traefik, or a cloud load balancer to terminate TLS and route traffic.
Nginx Example
upstream desktable_frontend {
server frontend:3000;
}
upstream desktable_backend {
server backend:8000;
}
server {
listen 80;
server_name desktable.example.com;
return 301 https://$host$request_uri;
}
server {
listen 443 ssl http2;
server_name desktable.example.com;
ssl_certificate /etc/letsencrypt/live/desktable.example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/desktable.example.com/privkey.pem;
location / {
proxy_pass http://desktable_frontend;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
}
location /api/ {
proxy_pass http://desktable_backend;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
}
location /ws/ {
proxy_pass http://desktable_backend;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_read_timeout 3600s;
proxy_send_timeout 3600s;
}
}
CORS Configuration
Set CORS_ORIGINS to the exact frontend origins permitted to call the API.
CORS_ORIGINS=https://desktable.example.com,https://app.desktable.example.com
Avoid * in production. The backend should allow credentials only for trusted origins,
especially because Socket.IO connections may use authentication cookies or bearer-token
payloads.
SSL and Secure Media
Use HTTPS in production for:
● REST API traffic.
● Socket.IO WebSocket upgrades.
● Browser microphone access.
● Browser camera access via MediaPipe and face-api.js.
Modern browsers generally require a secure context for camera and microphone access.
Serve DeskTable through HTTPS and configure the reverse proxy to preserve WebSocket
upgrade headers.
Required Nginx WebSocket settings:
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
Production Checklist
● Use PostgreSQL rather than SQLite.
● Set DEBUG=false.
● Set ENVIRONMENT=production.
● Use a long, random JWT_SECRET_KEY.
● Store secrets in a managed secret manager.
● Restrict CORS_ORIGINS to production domains.
● Run alembic upgrade head before deploying new backend versions.
● Enable HTTPS and redirect HTTP to HTTPS.
● Configure WebSocket proxy timeouts.
● Enable Redis persistence or use a managed Redis service.
● Configure database backups and restore tests.
● Set OpenAI usage and spend limits.
● Apply per-user and per-room rate limits for AI, TTS, and vision events.
● Disable or minimize vision-frame storage by default.
● Add structured logging, tracing, and error tracking.
● Monitor database connections, Redis memory, CPU, latency, and Socket.IO
connection counts.
● Run database migrations during a controlled deployment window.
● Use health checks for backend, frontend, database, and Redis.
● Configure log retention and PII-redaction policies.
Health Checks
Expose a backend health endpoint such as:
GET /api/v1/admin/health
Expected response:
{
"status": "healthy",
"database": "healthy",
"redis": "healthy",
"openai": "healthy",
"socketio": "healthy"
}
Use container-level health checks as well:
healthcheck:
test: ["CMD", "curl", "-f", "http://localhost:8000/api/v1/admin/health"]
interval: 30s
timeout: 5s
retries: 3
Scaling
Component Scaling Strategy
Frontend Run multiple stateless Next.js containers behind a load balancer or use a
managed hosting platform
Backend
API
Scale FastAPI replicas horizontally
Socket.IO Use Redis adapter/message queue and sticky sessions, or route
WebSocket connections consistently
PostgreSQL Use managed PostgreSQL with read replicas for reporting workloads
Redis Use managed Redis with persistence and eviction policy appropriate to
session data
AI calls Queue and throttle requests; use streaming to reduce perceived latency
Vision Limit frame rate, compress frames, and process client-side where possible
Rollback Procedure
1. Stop new deployments.
2. Identify the last stable backend image tag.
3. Redeploy the previous backend image.
4. If the database schema changed, roll back only if the migration is reversible and safe:
docker compose exec backend alembic downgrade -1
5. Verify:
docker compose ps
docker compose logs -f backend
curl -f https://desktable.example.com/api/v1/admin/health
6. Restore the previous frontend image if the API contract changed.
