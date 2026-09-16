# DocSage — Research Paper Assistant

Upload research papers, ask questions about them, and get answers grounded in the source text with citations back to the exact passage. Built as a portfolio project demonstrating full-stack fundamentals (React/TypeScript, Python/FastAPI, PostgreSQL, Docker, CI/CD) with a genuinely agentic RAG layer on top — not a single-LLM-call demo.

**Live demo:** [docsage.vercel.app](https://docsage.vercel.app/) (frontend only — backend isn't deployed yet)

**Status:** Phase 0 — foundations. This is currently an empty-but-running skeleton: no RAG, no chat, no auth yet.

**Planned future modes** (built one at a time, after the research-paper assistant is complete): a legal consulting-agreement reviewer, and a technical-documentation Q&A tool. Same underlying RAG/agent engine, different document domain and prompting.

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Frontend | React + TypeScript + Next.js | Most-requested combination across the job search that motivated this project |
| Frontend testing | Playwright | Explicitly named in multiple real job postings |
| Backend | Python + FastAPI | Matches existing production experience; most-requested backend pairing after JS |
| Database | PostgreSQL | Universally expected; real schema design experience |
| Vector store | Chroma | Self-hostable and free — closes the RAG/vector-DB gap without a paid dependency |
| Agent orchestration | Claude Agent SDK / MCP | Direct continuity with prior agent-orchestration training |
| Containerization | Docker + docker-compose | Universally expected; enables the observability layer below |
| CI/CD | GitHub Actions | Free, visible on the repo as a badge |
| Observability | Prometheus + Grafana | Self-hosted, free-tier viable |
| Deployment | Vercel (frontend) + Railway/Render (backend) | Free tiers sufficient at this scale; genuinely live URL |

## Architecture

```
┌─────────────────┐      ┌──────────────────┐      ┌─────────────────┐
│  Next.js Client  │─────▶│  FastAPI Backend  │─────▶│   PostgreSQL    │
│  (React + TS)    │◀─────│                   │◀─────│  (metadata,     │
│  Mobile-responsive│      │  - Upload/parse   │      │   chat history) │
└─────────────────┘      │  - Chunk/embed    │      └─────────────────┘
                          │  - RAG retrieval  │
                          │  - Agent loop     │      ┌─────────────────┐
                          └─────────┬─────────┘─────▶│  Chroma Vector  │
                                    │                 │  Store          │
                                    ▼                 └─────────────────┘
                          ┌──────────────────┐
                          │  Claude API +     │
                          │  Agent SDK / MCP  │
                          └──────────────────┘

Deployment: Docker containers, GitHub Actions CI, Prometheus/Grafana monitoring
```

## Local development

Requires Docker and Docker Compose.

```bash
docker compose up --build
```

- Frontend: [http://localhost:3000](http://localhost:3000)
- Backend health check: [http://localhost:8000/health](http://localhost:8000/health)

Or run each side independently — see [`frontend/README.md`](frontend/README.md) and the backend section below.

### Backend only

```bash
cd backend
pip install -r requirements.txt
uvicorn app.main:app --reload
```

## Roadmap

- [x] **Phase 0 — Foundations:** repo, README, license, Next.js shell, FastAPI shell + health check, docker-compose, frontend deployed to Vercel
- [ ] **Phase 1 — Document Ingestion:** upload/parse, PostgreSQL schema, REST endpoints
- [ ] **Phase 2 — RAG Pipeline:** chunking, embeddings in Chroma, retrieval, cited answers
- [ ] **Phase 3 — Agentic Layer:** multi-step agent loop (re-retrieve on low confidence, compare across documents)
- [ ] **Phase 4 — Real Frontend:** streaming chat UI, citation display, accessibility, mobile
- [ ] **Phase 5 — Testing & CI:** Playwright e2e, backend unit tests, GitHub Actions
- [ ] **Phase 6 — Deployment & Observability:** live backend deploy, Prometheus/Grafana dashboard
- [ ] **Phase 7 — Polish & Documentation:** README rewrite, demo GIF, portfolio link

## Why these choices

- **Chroma over a paid vector DB (e.g. Pinecone):** keeps the project self-hostable and free while still demonstrating real vector-search experience.
- More notes will land here as decisions get made in each phase — this section is meant to read as engineering judgment, not just a changelog.

## License

MIT — see [LICENSE](LICENSE).
