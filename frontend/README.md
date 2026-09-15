# DocSage Frontend

Next.js + TypeScript app shell for DocSage. See the [root README](../README.md) for project-level context.

## Local development

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

Or, from the repo root, run the whole stack: `docker compose up`.

## Environment

Copy `.env.example` to `.env.local` and adjust as needed. `NEXT_PUBLIC_API_URL` points at the FastAPI backend.

## Deploy

This app deploys to [Vercel](https://vercel.com/new) with no extra configuration — import the repo, set the root directory to `frontend`, and set `NEXT_PUBLIC_API_URL` to the deployed backend URL.
