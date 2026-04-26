# Catalyst — AI Skill Assessment & Personalised Learning Plan Agent

> Submitted for **Catalyst Hackathon** · April 2025

A resume tells you what someone *claims* to know — not how well they actually know it. This agent takes a Job Description and a candidate's resume, **conversationally assesses real proficiency** on each required skill, identifies gaps, and generates a **personalised learning plan** with curated resources and time estimates.

---

## Live Demo

[View on Claude.ai Artifact](#) *(paste the artifact link here)*

---

## Features

- **Skill extraction** — Claude parses the JD and extracts required skills, categorised by importance (`critical` / `important` / `nice-to-have`) and type (`Technical` / `Soft`)
- **Conversational assessment** — for each technical skill, the agent asks one targeted, practical question (not "rate yourself 1–10") and scores the answer 1–10 via LLM evaluation
- **Gap analysis** — computes a priority-weighted gap score: `importance × (10 − proficiency)` to rank what matters most
- **Personalised learning plan** — Claude generates skill-specific roadmaps with curated resources (real courses, books, docs) and realistic week estimates anchored to the candidate's existing background

---

## Architecture

```
JD + Resume → Skill Extractor (Claude) → Conversational Loop (Q&A per skill) → Scores
                                                                              ↓
                                                      Gap Analysis (priority × gap weight)
                                                                              ↓
                                              Learning Plan (resources + time estimates)
```

### Scoring & prioritisation logic

| Stage | Formula |
|---|---|
| Proficiency score | LLM evaluation of candidate's natural-language answer → 1–10 |
| Gap weight | `importance_multiplier × (10 − score)` |
| Plan priority | Skills sorted descending by gap weight |

**Importance multipliers:** `critical = 3`, `important = 2`, `nice-to-have = 1`

Skills scoring ≥ 7 are considered proficient and excluded from the learning plan. Skills below 7 are ranked by gap weight — the highest-priority gaps receive the most detailed resource curation.

### Resource curation

Claude is prompted with the candidate's resume background and the ranked gap list. It generates skill-specific paths with:
- 2–3 resources per skill (course, book, or official docs)
- Free/paid flag
- Realistic week estimates based on the candidate's adjacent knowledge

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React (single-page, no build step) |
| AI | Anthropic Claude API (`claude-sonnet-4-20250514`) |
| Styling | CSS custom properties (Anthropic design system) |
| Deployment | Claude.ai Artifact (zero-infra) |

---

## Local Setup

### Prerequisites

- Node.js 18+
- An Anthropic API key → [console.anthropic.com](https://console.anthropic.com)

### Option A — Standalone HTML (fastest)

```bash
git clone https://github.com/YOUR_USERNAME/catalyst-skill-agent
cd catalyst-skill-agent
# Open index.html in your browser — no build needed
```

Set your API key in `index.html`:
```js
const API_KEY = 'sk-ant-...';  // line ~12
```

### Option B — React + Vite

```bash
git clone https://github.com/YOUR_USERNAME/catalyst-skill-agent
cd catalyst-skill-agent/app
npm install
cp .env.example .env          # add VITE_ANTHROPIC_API_KEY=sk-ant-...
npm run dev                   # → http://localhost:5173
```

> **Note:** The Anthropic API key is used client-side in this prototype. For production, proxy through a backend route.

---

## Sample Input / Output

### Input — Job Description (excerpt)

```
Senior Full-Stack Engineer — FinTech Startup

Requirements:
- 5+ years React and TypeScript
- Node.js and REST API design
- PostgreSQL and Redis
- Docker and Kubernetes
- CI/CD (GitHub Actions)
- OAuth 2.0 / security best practices
- AWS (EC2, S3, RDS, Lambda)
```

### Input — Resume (excerpt)

```
Jane Doe — Software Engineer

- 3 years React + Redux front-end
- Node.js REST APIs
- Basic MySQL
- Git + GitHub
- Docker (beginner, local dev only)
- Personal project on AWS S3 + EC2
```

### Output — Proficiency Scores

| Skill | Score | Assessment |
|---|---|---|
| React | 7/10 | Solid React experience but limited TypeScript exposure |
| Node.js | 6/10 | Can build REST APIs, less experience with advanced patterns |
| PostgreSQL | 3/10 | MySQL background, limited PostgreSQL-specific knowledge |
| Docker / Kubernetes | 2/10 | Docker basics only, no Kubernetes experience |
| AWS | 5/10 | S3 + EC2 only, no RDS/Lambda/IAM depth |
| TypeScript | 4/10 | Aware of types, not writing idiomatic TS |

### Output — Learning Plan (top 2 gaps)

**Docker & Kubernetes** · Critical gap · ~6 weeks
- *Docker & Kubernetes: The Practical Guide* (Udemy) — Paid
- *Kubernetes official interactive tutorial* (kubernetes.io) — Free
- *Play with Kubernetes* (labs.play-with-k8s.com) — Free

**PostgreSQL** · Critical gap · ~3 weeks
- *PostgreSQL Tutorial* (postgresqltutorial.com) — Free
- *The Art of PostgreSQL* (book) — Paid
- *pgexercises.com* — Free

---

## Repo Structure

```
catalyst-skill-agent/
├── index.html          # standalone single-file version (no build)
├── app/
│   ├── src/
│   │   ├── App.jsx         # main React component
│   │   ├── api.js          # Anthropic API calls
│   │   ├── scoring.js      # gap weight logic
│   │   └── prompts.js      # all system prompts
│   ├── package.json
│   └── vite.config.js
├── docs/
│   ├── architecture.png    # architecture diagram export
│   └── sample-output.json  # full sample assessment result
└── README.md
```

---

## Demo Video

[Watch on YouTube / Loom](#) *(paste link here)*

Walkthrough covers:
1. Pasting a real SWE job description and junior resume
2. Live conversational assessment (3 skills)
3. Gap analysis output
4. Reviewing the generated learning plan

---

## Submission Details

| Field | Value |
|---|---|
| Git repository | https://github.com/YOUR_USERNAME/catalyst-skill-agent |
| Git username | YOUR_USERNAME |
| Project site | (artifact / Vercel URL) |
| Demo video | (YouTube / Loom URL) |

---

## Team

Built solo / by YOUR_NAME for Catalyst Hackathon, April 2025.

---

## License

MIT
