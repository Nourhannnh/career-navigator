# JobAssist — AI Job Application Assistant

Ever stared at a job description and wondered, *"Am I actually qualified for this?"*

JobAssist answers that question in seconds. Paste your CV and a job description, and the AI tells you exactly how well you match the role, what skills you're missing, how to improve your CV, and even writes you a tailored cover letter — all in one place.

---

## What it does

- **Match Score** — Get a 0–100% compatibility score between your CV and the job description
- **Missing Skills** — See exactly what the employer is looking for that your CV doesn't currently show
- **CV Improvement Tips** — Actionable suggestions to make your application stronger
- **Cover Letter Generator** — One click generates a fully tailored cover letter for that specific role
- **Interview Questions** — Predicts the questions the interviewer is likely to ask you
- **Analysis History** — Every analysis is saved so you can revisit and compare them over time
- **Dashboard** — A summary of all your job hunt activity in one view

---

## Tech Stack

| Part | Technology |
|---|---|
| Frontend | React + Vite + TypeScript |
| Backend | Node.js + Express + TypeScript |
| Database | PostgreSQL + Drizzle ORM |
| Auth | Clerk |
| AI | OpenAI GPT |
| Monorepo | pnpm workspaces |

The whole project is written in TypeScript — both the frontend and backend — so everything stays consistent and type-safe.

---

## Project Structure

```
artifacts/
  job-assistant/        # React frontend (the website)
    src/
      pages/
        home.tsx          # Landing page
        dashboard.tsx     # Your stats and recent activity
        analyze.tsx       # New analysis form
        analyses.tsx      # Full history list
        analysis-detail.tsx  # Single result + cover letter + questions
      App.tsx             # Routing and auth setup

  api-server/           # Express backend (the API)
    src/
      routes/
        analyses/         # CV analysis endpoints
        dashboard/        # Stats endpoints
      lib/
        ai.ts             # All AI prompts and logic

lib/
  db/                   # Database schema
  api-spec/             # OpenAPI spec + auto-generated API client
```

---

## Getting Started

### Prerequisites
- Node.js 20+
- pnpm (`npm install -g pnpm`)
- A PostgreSQL database
- A [Clerk](https://clerk.com) account (for auth)
- An OpenAI API key

### Environment Variables

Create a `.env` file in the root with the following:

```env
# Database
DATABASE_URL=your_postgresql_connection_string

# Clerk Auth
CLERK_SECRET_KEY=your_clerk_secret_key
CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key

# OpenAI
OPENAI_API_KEY=your_openai_api_key
```

### Install & Run

```bash
# Install all dependencies
pnpm install

# Push the database schema
pnpm --filter @workspace/db run push

# Start the API server
pnpm --filter @workspace/api-server run dev

# Start the frontend (in a separate terminal)
pnpm --filter @workspace/job-assistant run dev
```

The frontend will be available at `http://localhost:5173` and the API at `http://localhost:8080`.

---

## API Endpoints

| Method | Route | Description |
|---|---|---|
| GET | `/api/healthz` | Health check |
| GET | `/api/analyses` | List your analyses (paginated) |
| POST | `/api/analyses` | Create a new analysis |
| GET | `/api/analyses/:id` | Get a single analysis |
| DELETE | `/api/analyses/:id` | Delete an analysis |
| POST | `/api/analyses/:id/cover-letter` | Generate a cover letter |
| POST | `/api/analyses/:id/interview-questions` | Generate interview questions |
| GET | `/api/dashboard/stats` | Your dashboard statistics |
| GET | `/api/dashboard/recent` | 5 most recent analyses |

All routes except `/api/healthz` require authentication.

---

## How the AI Works

When you submit a CV and job description, the backend sends both to OpenAI with a carefully structured prompt. The AI is instructed to return a JSON object with:

- A numeric match score
- A list of missing skills
- A list of improvement suggestions

For cover letters and interview questions, separate prompts are used that factor in both the CV and the job description to keep everything tailored and relevant.

You can customize the AI behavior by editing `artifacts/api-server/src/lib/ai.ts`.

---

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you'd like to change.

---

## License

MIT
