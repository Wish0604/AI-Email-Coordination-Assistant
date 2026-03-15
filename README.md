# AI Email Coordination Assistant

Autonomous Email Scheduling with Voice Commands and Adaptive Learning.

An AI-powered digital executive assistant that coordinates meetings through email conversations. It monitors inbox activity, understands scheduling discussions, finds overlapping availability, schedules meetings, sends confirmations, summarizes threads, supports voice commands, and improves decisions over time through adaptive learning.

---

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Core Capabilities](#core-capabilities)
- [System Architecture](#system-architecture)
- [Tech Stack](#tech-stack)
- [End-to-End Workflow](#end-to-end-workflow)
- [Project Structure](#project-structure)
- [Setup and Installation](#setup-and-installation)
- [Configuration](#configuration)
- [API Design (Draft)](#api-design-draft)
- [Database Design (Draft)](#database-design-draft)
- [Voice Command Examples](#voice-command-examples)
- [Adaptive Learning Strategy](#adaptive-learning-strategy)
- [Security and Privacy](#security-and-privacy)
- [Human Override and Safety](#human-override-and-safety)
- [Testing Strategy](#testing-strategy)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

The AI Email Coordination Assistant automates the complete meeting coordination lifecycle:

1. Monitor incoming email messages.
2. Detect scheduling intent in conversations.
3. Extract participants, dates, and time expressions from natural language.
4. Compute overlapping availability across multiple participants and time zones.
5. Create calendar events and send invitations.
6. Respond to email threads as an autonomous assistant identity.
7. Learn user scheduling preferences to improve future decisions.

The result is less manual back-and-forth and significantly faster scheduling.

---

## Problem Statement

Professionals lose time in repetitive meeting coordination tasks:

- Long email threads with unclear availability
- Manual calendar checks and conflict resolution
- Time zone mismatches
- Repeated follow-up messages

This system converts manual scheduling into a reliable, autonomous workflow.

---

## Core Capabilities

### 1) Email Monitoring

- Real-time inbox monitoring (IMAP / SMTP / Gmail API)
- Detection of scheduling and availability requests
- Automatic ingestion and processing of new thread messages

### 2) Intelligent Scheduling Engine

- Extract availability from free-form text (for example: "tomorrow afternoon", "next Monday", "between 2-4 PM")
- Merge availability across participants
- Detect and resolve calendar conflicts
- Suggest alternate slots when overlap is not found
- Auto-create meetings when confidence is high

### 3) Calendar Integration

- Google Calendar event creation
- Automatic invitation dispatch
- Duplicate meeting prevention
- Meeting update and cancellation handling

### 4) Natural Language Understanding

Extracts:

- Date and time
- Participants
- Meeting intent
- Contextual constraints (duration, urgency, timezone hints)

### 5) Email Thread Intelligence

- Full-thread context tracking
- Latest decision extraction
- Thread-level summarization

Example query:

- "What is the latest update on the project?"

Example response:

- "The AI model training completed yesterday and the review meeting is scheduled tomorrow."

### 6) Autonomous Assistant Identity

- Dedicated assistant email address
- Automatic confirmations and follow-ups
- Mandatory AI disclaimer in all outgoing emails

Required disclaimer:

> This message was sent by an experimental AI email assistant.

### 7) Voice Command Interface

- Speech-to-text input
- Voice-based scheduling requests
- Voice-based email and calendar queries
- Text-to-speech responses

### 8) Adaptive User Learning

Learns over time:

- Preferred meeting windows
- Frequent collaborators
- Typical meeting durations
- Historical acceptance/rejection patterns

### 9) Multi-Timezone Support

- Timezone detection and normalization
- Cross-region overlap calculation
- Timezone conflict prevention

### 10) Conflict Resolution and Clarification

- Detect no-overlap conditions
- Propose nearest best alternatives
- Ask clarification questions for ambiguous requests

Example:

- Input: "Let's meet tomorrow."
- Assistant: "Could you please specify a preferred time?"

### 11) Priority-Based Scheduling

- High: executives / clients
- Medium: internal teams
- Low: optional participants

### 12) Human Override

- Edit AI-proposed meetings
- Cancel scheduled meetings
- Pause automation when needed

---

## System Architecture

```text
User (Email / Voice)
        |
Email Gateway + Voice Interface
        |
AI Processing Engine (LLM + NLP)
        |
Scheduling Engine
        |
Thread Intelligence + User Learning
        |
Task Execution Layer
        |
Google Calendar + Email Responses
        |
Database (Preferences + History + Audit)
```

### Mermaid Diagram

```mermaid
flowchart TD
    A[User: Email or Voice] --> B[Email Gateway and Voice Interface]
    B --> C[AI Processing Engine]
    C --> D[Scheduling Engine]
    D --> E[Thread Intelligence and Adaptive Learning]
    E --> F[Task Execution Layer]
    F --> G[Google Calendar API]
    F --> H[Email Provider API]
    E --> I[(PostgreSQL or MongoDB)]
```

---

## Tech Stack

### Frontend

- React.js
- Tailwind CSS
- Web Speech API (voice input/output)

### Backend

- Python
- FastAPI (recommended) or Flask

### AI and NLP

- OpenAI / Gemini / LLaMA for language understanding
- Whisper for speech recognition
- Time parsing libraries (for example: `dateparser`, `duckling`, `chrono`-style parsers)

### Integrations

- Gmail API or IMAP/SMTP connectors
- Google Calendar API

### Database

- PostgreSQL (recommended for relational scheduling data)
- MongoDB (optional for flexible document-style thread storage)

---

## End-to-End Workflow

1. Assistant monitors incoming emails.
2. AI model detects scheduling intent.
3. NLP extracts time expressions and participants.
4. Scheduling engine computes overlap and resolves conflicts.
5. Calendar event is created via Google Calendar API.
6. Invitations and confirmations are sent.
7. Thread summary is updated.
8. User preference model is incrementally updated.

---

## Project Structure

```text
ai-email-coordination-assistant/
  frontend/
    src/
      components/
      pages/
      hooks/
      services/
  backend/
    app/
      api/
      core/
      models/
      services/
      integrations/
      scheduler/
      nlp/
      voice/
      learning/
      utils/
    tests/
  docs/
    architecture/
    api/
    diagrams/
  scripts/
  .env.example
  docker-compose.yml
  README.md
```

---

## Setup and Installation

### Prerequisites

- Python 3.11+
- Node.js 20+
- Google Cloud project with Gmail and Calendar APIs enabled
- OAuth credentials and redirect URIs configured

### 1) Clone Repository

```bash
git clone https://github.com/<your-org>/ai-email-coordination-assistant.git
cd ai-email-coordination-assistant
```

### 2) Backend Setup

```bash
cd backend
python -m venv .venv
# Windows PowerShell
. .venv/Scripts/Activate.ps1
pip install -r requirements.txt
```

### 3) Frontend Setup

```bash
cd ../frontend
npm install
npm run dev
```

### 4) Run Backend

```bash
cd ../backend
uvicorn app.main:app --reload --port 8000
```

### 5) Optional Docker

```bash
docker compose up --build
```

---

## Configuration

Create `.env` files for backend and frontend.

Example backend `.env`:

```env
APP_ENV=development
APP_PORT=8000

OPENAI_API_KEY=
GEMINI_API_KEY=

GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_REDIRECT_URI=
GOOGLE_CALENDAR_ID=primary

GMAIL_WATCH_TOPIC=
GMAIL_CREDENTIALS_PATH=

DATABASE_URL=postgresql://user:password@localhost:5432/email_assistant

ENCRYPTION_KEY=
JWT_SECRET=

ASSISTANT_EMAIL=assistant@yourdomain.com
MANDATORY_AI_DISCLAIMER=This message was sent by an experimental AI email assistant.
```

---

## API Design (Draft)

### Health

- `GET /health` - service status

### Email and Threads

- `POST /emails/webhook` - ingest incoming email event
- `GET /threads/{thread_id}` - retrieve thread with summary
- `POST /threads/{thread_id}/summarize` - regenerate summary

### Scheduling

- `POST /schedule/extract` - parse availability from text
- `POST /schedule/compute` - compute overlap across participants
- `POST /schedule/confirm` - create event and send invites
- `POST /schedule/cancel` - cancel existing event

### Voice

- `POST /voice/transcribe` - speech to text
- `POST /voice/command` - parse and execute voice intent
- `POST /voice/respond` - text to speech output

### Preferences and Learning

- `GET /users/{user_id}/preferences`
- `POST /users/{user_id}/preferences/update`
- `POST /learning/feedback` - explicit user corrections

### Human Override

- `POST /override/pause`
- `POST /override/resume`
- `POST /override/manual-edit`

---

## Database Design (Draft)

### Suggested Core Tables (PostgreSQL)

- `users`
- `participants`
- `threads`
- `emails`
- `meetings`
- `meeting_participants`
- `availability_blocks`
- `user_preferences`
- `learning_events`
- `audit_logs`

### Example Data Captured

- Preferred meeting window (for example, 10:00-16:00)
- Typical meeting duration by meeting type
- Participant priority class
- Accepted vs rejected suggestions

---

## Voice Command Examples

- "Schedule a meeting with Rahul tomorrow at 3 PM"
- "Read my latest email"
- "Am I free tomorrow afternoon?"
- "Move Friday's review meeting to next Monday at 11 AM"

---

## Adaptive Learning Strategy

1. Capture user actions and corrections.
2. Convert actions to explicit preference signals.
3. Score candidate slots using learned features.
4. Apply confidence thresholds for automation.
5. Fall back to clarification when confidence is low.

### Example

If a user consistently accepts meetings between 10 AM and 4 PM, those slots receive higher ranking in future scheduling decisions.

---

## Security and Privacy

- OAuth2-based secure API access
- Encrypted token and credential storage
- TLS-only external API communication
- Scoped permissions for Gmail and Calendar access
- Data minimization for stored email content
- Audit logs for automated actions
- Role-based control for admin and override operations

---

## Human Override and Safety

- Manual edit/cancel at any point
- Automation pause switch
- Clarification prompts on ambiguity
- Transparent AI-generated responses with mandatory disclaimer

---

## Testing Strategy

- Unit tests: intent detection, time extraction, overlap logic
- Integration tests: Gmail/Calendar API flows
- End-to-end tests: full email-to-calendar lifecycle
- Regression tests: timezone edge cases and DST transitions
- Voice tests: transcription quality and command routing

Recommended tools:

- `pytest`
- `httpx` / `pytest-asyncio`
- `Playwright` for frontend and flow automation

---

## Roadmap

- Slack and Microsoft Teams integration
- Meeting analytics and productivity insights
- Automated reminders and follow-up workflows
- Integration with project management tools
- Advanced recommendation ranking models

---

