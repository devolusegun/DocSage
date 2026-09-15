# Portfolio Project Brief: AI Document Research Assistant

**Purpose of this document:** This is a self-contained brief for building a portfolio project from scratch. It can be handed directly to a Claude Code session to begin implementation, and it also serves as the owner's own learning/commit roadmap. If you're a coding agent reading this: the technical spec, architecture, and phase breakdown are your instructions. The "Learn This First" callouts are for the human developer's own study alongside the build — acknowledge them but they are not tasks for you to complete.

**Context (for whoever picks this up):** This project exists to close specific, real gaps observed across 40+ actual job applications in software engineering, frontend, and AI-agentic roles. The recurring gaps were: frontend depth (accessibility, testing rigor, performance), RAG/vector-database/agent-orchestration experience, Kubernetes/observability tooling, and visible CI/CD discipline. The fundamentals (React, TypeScript, Python, REST APIs, PostgreSQL, Docker, AWS) showed up far more often than any AI-specific requirement, so this project prioritizes rock-solid fundamentals with genuine AI/agentic work layered on top — not an AI-first showcase that's thin on the basics.

---

## Product Vision

**Working name:** DocSage (rename freely)

**What it does:** A user uploads one or more documents (PDF, text, markdown). They chat with an AI assistant that answers questions grounded in those documents, with citations pointing back to the specific source passage. Under the hood, this is a Retrieval-Augmented Generation (RAG) system with a genuinely agentic layer on top — not just a single LLM call, but multi-step reasoning: the assistant can decide to re-search, compare across multiple documents, or ask a clarifying question before answering.

**Who it's for (the portfolio framing):** A believable, real use case — e.g., a legal-consulting-agreement reviewer, a research-paper assistant, or a technical-documentation Q&A tool. Pick one specific, narrow use case rather than a generic "chat with any document" tool — specificity reads as more real to anyone evaluating the project.

**Must be:** live, deployed, mobile-responsive, and usable by anyone with the link — not a localhost-only demo.

---

## Tech Stack & Rationale

| Layer | Choice | Why |
|---|---|---|
| Frontend | React + TypeScript + Next.js | The single most-requested combination across the entire job search |
| Frontend testing | Playwright | Named explicitly in multiple real postings (Adyen, ING) |
| Backend | Python + FastAPI | Matches existing production experience; most-requested backend pairing after JS |
| Database | PostgreSQL | Universally expected; real schema design experience |
| Vector store | Chroma (self-hostable, free) | Closes the RAG/vector-DB gap (CertifyOS, SimilarWeb) without a paid dependency |
| Agent orchestration | Claude Agent SDK / MCP | Direct continuity with existing bootcamp training — genuine, not manufactured |
| Containerization | Docker + docker-compose | Universally expected; enables the observability layer below |
| CI/CD | GitHub Actions | Free, visible on the repo as a badge — cheap credibility signal |
| Observability | Prometheus + Grafana (self-hosted, free tier viable) | Closes a gap that showed up at Adyen, both Workday roles, and Vultr |
| Deployment | Vercel (frontend) + Railway or Render (backend) | Free tiers sufficient at this scale; genuinely live URL |

---

## Architecture Overview

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

---

## Phased Build Plan (approx. 9 weeks, daily commits)

Each phase ends with a working, demoable increment — never a dead-end branch. Commit daily even on small progress; a visible, consistent contribution graph is itself part of the portfolio value.

### Phase 0 — Foundations (Week 1)
**Deliverable:** Empty-but-running full-stack skeleton, deployed.
- Repo setup, README skeleton, license, `.gitignore`
- Next.js + TypeScript app shell, deployed to Vercel (even blank)
- FastAPI shell with a health-check endpoint, containerized
- Docker Compose wiring both together locally
- **Learn this first:** TypeScript fundamentals; FastAPI basics; Git/GitHub workflow conventions.
  *LinkedIn Learning search terms:* "TypeScript Essential Training", "FastAPI Framework", "Git and GitHub Essential Training"

### Phase 1 — Document Ingestion (Week 2)
**Deliverable:** Upload a document, see it stored and listed.
- File upload endpoint (PDF/text), basic parsing/text extraction
- PostgreSQL schema: documents, chunks, chat sessions
- Basic REST endpoints: upload, list, delete
- **Learn this first:** PostgreSQL fundamentals; REST API design principles.
  *LinkedIn Learning search terms:* "PostgreSQL Essential Training", "REST API Design"

### Phase 2 — RAG Pipeline (Weeks 3–4)
**Deliverable:** Ask a question, get an answer grounded in the uploaded document with a citation.
- Chunking strategy (sensible overlap, not naive splitting)
- Embedding generation, storage in Chroma
- Retrieval logic (similarity search, relevance filtering)
- Claude API integration for answer generation with explicit source citation
- **Learn this first:** RAG concepts end to end; embeddings and vector search; prompt engineering for grounded answers.
  *LinkedIn Learning search terms:* "Retrieval Augmented Generation (RAG)", "Vector Databases for AI", "Prompt Engineering"

### Phase 3 — Agentic Layer (Week 5)
**Deliverable:** The assistant can take more than one step before answering — e.g., re-searching when the first retrieval is weak, or comparing across multiple documents.
- Integrate Claude Agent SDK / MCP for multi-step tool use
- Explicit agent loop: retrieve → evaluate confidence → re-retrieve or answer
- **Learn this first:** Agent orchestration patterns; MCP fundamentals (direct continuity with prior bootcamp work — this phase should feel familiar, not new).
  *LinkedIn Learning search terms:* "Building AI Agents", "Model Context Protocol (MCP) Fundamentals"

### Phase 4 — Real Frontend (Week 6)
**Deliverable:** A genuinely usable, accessible, mobile-first chat interface — not a bare form.
- Chat UI with streaming responses, citation display/linking
- Accessibility: real keyboard navigation, ARIA labels, color contrast — not decorative
- Mobile-responsive layout tested on an actual phone, not just DevTools
- **Learn this first:** Web accessibility (WCAG) practically applied; responsive design patterns.
  *LinkedIn Learning search terms:* "Web Accessibility for Developers", "Responsive Web Design"

### Phase 5 — Testing & CI (Week 7)
**Deliverable:** Green CI badge on the README, meaningful test coverage.
- Playwright end-to-end tests covering the core user flow (upload → ask → get cited answer)
- Backend unit tests for chunking/retrieval logic
- GitHub Actions pipeline running tests on every push
- **Learn this first:** Playwright fundamentals; CI/CD with GitHub Actions.
  *LinkedIn Learning search terms:* "Playwright Testing", "GitHub Actions Essential Training"

### Phase 6 — Deployment & Observability (Week 8)
**Deliverable:** Live production URL, with a real (even if minimal) monitoring dashboard.
- Backend deployed to Railway/Render, environment configs separated from code
- Prometheus metrics exposed from the FastAPI app (request counts, latency, error rates)
- Grafana dashboard visualizing those metrics — a screenshot of this belongs in the README
- **Learn this first:** Deployment fundamentals for containerized apps; Prometheus/Grafana basics.
  *LinkedIn Learning search terms:* "Docker for Developers", "Prometheus and Grafana"

### Phase 7 — Polish & Documentation (Week 9)
**Deliverable:** Portfolio-ready.
- README rewrite: what it does, why, architecture diagram, live demo link, tech stack, "what I'd improve next" section
- Short demo GIF or video embedded in the README
- Link prominently from the portfolio site
- Review commit history for clarity (doesn't need squashing, just shouldn't be fifty "fix typo" commits with nothing else)

---

## Engineering Standards (apply throughout, not just at the end)

- **Accessibility is not a phase-4-only concern** — build it in as you go.
- **Every PR/commit should be explainable** — if you can't describe why a piece of code exists and what it does, don't merge it until you can. This project's entire purpose is being defensible in an interview.
- **README-first mentality per feature** — a one-line note on *why* a technical choice was made (e.g., "chose Chroma over Pinecone to keep this self-hostable and free") reads as engineering judgment, not just code output.
- **Commit daily**, even if small — the visible contribution graph is part of the value, not a vanity metric.

---

## Portfolio-Readiness Checklist (Definition of Done)

- [ ] Live URL, works on mobile
- [ ] README has: description, architecture diagram, live link, tech stack, screenshots/GIF, "what I'd improve" section
- [ ] Green CI badge visible
- [ ] Real accessibility effort demonstrated (not just claimed)
- [ ] Vector DB / RAG pipeline genuinely functional, not mocked
- [ ] Agent loop demonstrably multi-step (not a single LLM call dressed up)
- [ ] Prometheus/Grafana dashboard screenshot in README
- [ ] Owner can explain every architectural decision without notes

---

## Notes specifically for a coding-agent session picking this up

- Confirm the specific use-case framing (legal docs, research papers, technical docs, etc.) with the project owner before Phase 1 if it hasn't been chosen yet — don't default silently.
- Work phase by phase in the order above; each phase should end in a working, committed, deployable state — avoid long-lived broken branches.
- Favor free-tier and self-hostable tools throughout (Chroma over paid vector DBs, Railway/Render free tiers, self-hosted Grafana) — cost should stay near zero.
- Keep the owner's daily-learning goal in mind: prefer clear, well-commented implementations over clever-but-opaque ones, since the owner needs to genuinely understand and be able to explain this code later.
