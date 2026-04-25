# ResuMai 🎯

> Upload your profile once. Paste any job description. Get a perfectly tailored LaTeX resume — instantly.

ResuMai is a full-stack web app that lets you store your complete professional profile (projects, GitHub repos, achievements, academics, skills, contact info) and then generates a **job-specific, ATS-optimized LaTeX resume** by analyzing any job description you provide.

---

## ✨ Features

- **Profile Builder** — Store your name, email, phone, CGPA, university, GitHub, LinkedIn, skills, projects, achievements, certifications, and work experience — all in one place
- **JD Analyzer** — Paste or upload a job description; the AI extracts required skills, keywords, and priorities
- **Smart Resume Engine** — Matches your profile data against the JD and selects the most relevant items
- **LaTeX Generator** — Produces clean, compilable LaTeX code using battle-tested resume templates
- **Live Preview** — Renders a PDF preview in-browser before you download
- **Resume History** — Every generated resume is saved; revisit, fork, or tweak past versions
- **Multi-template Support** — Classic, modern, and academic LaTeX templates

---

## 🗂️ Folder Structure

```
resumai/
│
├── apps/
│   ├── web/                          # Next.js frontend
│   │   ├── public/
│   │   │   └── templates/            # Static LaTeX template previews (.png)
│   │   │
│   │   ├── src/
│   │   │   ├── app/                  # Next.js App Router
│   │   │   │   ├── layout.tsx
│   │   │   │   ├── page.tsx          # Landing page
│   │   │   │   ├── (auth)/
│   │   │   │   │   ├── login/
│   │   │   │   │   │   └── page.tsx
│   │   │   │   │   └── register/
│   │   │   │   │       └── page.tsx
│   │   │   │   ├── dashboard/
│   │   │   │   │   ├── page.tsx      # Overview: recent resumes, quick actions
│   │   │   │   │   └── layout.tsx
│   │   │   │   ├── profile/
│   │   │   │   │   ├── page.tsx      # Main profile editor
│   │   │   │   │   ├── personal/
│   │   │   │   │   │   └── page.tsx  # Name, email, phone, social links
│   │   │   │   │   ├── academics/
│   │   │   │   │   │   └── page.tsx  # Institution, degree, CGPA, graduation year
│   │   │   │   │   ├── skills/
│   │   │   │   │   │   └── page.tsx  # Tag-based skill input with proficiency
│   │   │   │   │   ├── projects/
│   │   │   │   │   │   └── page.tsx  # Project cards with GitHub link, tech stack, description
│   │   │   │   │   ├── experience/
│   │   │   │   │   │   └── page.tsx  # Internships, jobs — role, company, dates, bullets
│   │   │   │   │   ├── achievements/
│   │   │   │   │   │   └── page.tsx  # Hackathons, competitions, certifications
│   │   │   │   │   └── github/
│   │   │   │   │       └── page.tsx  # OAuth GitHub import + repo selector
│   │   │   │   ├── generate/
│   │   │   │   │   ├── page.tsx      # JD upload + template picker → trigger generation
│   │   │   │   │   └── [id]/
│   │   │   │   │       └── page.tsx  # Result page: LaTeX code + PDF preview + download
│   │   │   │   └── history/
│   │   │   │       └── page.tsx      # Past generated resumes
│   │   │   │
│   │   │   ├── components/
│   │   │   │   ├── ui/               # Reusable primitives (Button, Input, Badge, Card…)
│   │   │   │   ├── profile/
│   │   │   │   │   ├── ProfileSidebar.tsx
│   │   │   │   │   ├── SkillTagInput.tsx
│   │   │   │   │   ├── ProjectCard.tsx
│   │   │   │   │   ├── ExperienceForm.tsx
│   │   │   │   │   ├── AchievementList.tsx
│   │   │   │   │   └── GithubImporter.tsx
│   │   │   │   ├── generate/
│   │   │   │   │   ├── JDUploader.tsx       # Textarea + file drag-and-drop for JD
│   │   │   │   │   ├── TemplatePicker.tsx   # Visual template selector
│   │   │   │   │   ├── GenerateButton.tsx
│   │   │   │   │   ├── LaTeXViewer.tsx      # Syntax-highlighted code block
│   │   │   │   │   └── PDFPreview.tsx       # iframe PDF renderer
│   │   │   │   └── layout/
│   │   │   │       ├── Navbar.tsx
│   │   │   │       ├── Sidebar.tsx
│   │   │   │       └── Footer.tsx
│   │   │   │
│   │   │   ├── lib/
│   │   │   │   ├── api.ts            # API client helpers
│   │   │   │   ├── auth.ts           # NextAuth config
│   │   │   │   ├── github.ts         # GitHub OAuth + Octokit helpers
│   │   │   │   └── utils.ts
│   │   │   │
│   │   │   ├── hooks/
│   │   │   │   ├── useProfile.ts
│   │   │   │   ├── useGenerate.ts
│   │   │   │   └── useGithub.ts
│   │   │   │
│   │   │   ├── store/
│   │   │   │   └── profileStore.ts   # Zustand store for local profile state
│   │   │   │
│   │   │   └── types/
│   │   │       ├── profile.ts
│   │   │       ├── resume.ts
│   │   │       └── api.ts
│   │   │
│   │   ├── .env.local
│   │   ├── next.config.ts
│   │   ├── tailwind.config.ts
│   │   └── tsconfig.json
│   │
│   └── api/                          # Node.js + Express backend (or Next.js API routes)
│       ├── src/
│       │   ├── index.ts              # Express app entry + middleware
│       │   │
│       │   ├── routes/
│       │   │   ├── auth.ts           # /api/auth/** — login, register, session
│       │   │   ├── profile.ts        # /api/profile — CRUD for user profile data
│       │   │   ├── github.ts         # /api/github — repo import, README parsing
│       │   │   ├── generate.ts       # /api/generate — trigger AI resume generation
│       │   │   └── resume.ts         # /api/resume/:id — fetch/download past resumes
│       │   │
│       │   ├── services/
│       │   │   ├── ai/
│       │   │   │   ├── jdParser.ts         # Extracts skills/keywords/requirements from JD
│       │   │   │   ├── profileMatcher.ts   # Ranks and selects user data relevant to JD
│       │   │   │   ├── latexGenerator.ts   # Calls LLM to produce LaTeX from matched data
│       │   │   │   └── prompts/
│       │   │   │       ├── jdParsePrompt.ts
│       │   │   │       ├── matchPrompt.ts
│       │   │   │       └── latexPrompt.ts
│       │   │   ├── github/
│       │   │   │   ├── repoFetcher.ts      # Fetches repos via GitHub API
│       │   │   │   └── readmeParser.ts     # Extracts project descriptions from READMEs
│       │   │   ├── latex/
│       │   │   │   ├── templateEngine.ts   # Injects data into .tex templates
│       │   │   │   ├── compiler.ts         # Runs pdflatex in a sandbox (or Tectonic)
│       │   │   │   └── sanitizer.ts        # Escapes special LaTeX characters
│       │   │   └── storage/
│       │   │       └── fileStore.ts        # Upload / fetch PDF + .tex from S3/R2
│       │   │
│       │   ├── middleware/
│       │   │   ├── auth.ts           # JWT verification
│       │   │   ├── rateLimiter.ts    # Per-user generation rate limit
│       │   │   └── errorHandler.ts
│       │   │
│       │   ├── db/
│       │   │   ├── prisma/
│       │   │   │   └── schema.prisma # Data models (see below)
│       │   │   └── client.ts         # Prisma client singleton
│       │   │
│       │   └── utils/
│       │       ├── logger.ts
│       │       └── validators.ts     # Zod schemas
│       │
│       ├── .env
│       ├── package.json
│       └── tsconfig.json
│
├── packages/
│   ├── templates/                    # Shared LaTeX resume templates
│   │   ├── classic/
│   │   │   ├── template.tex          # Parameterized .tex with {{placeholders}}
│   │   │   └── preview.png
│   │   ├── modern/
│   │   │   ├── template.tex
│   │   │   └── preview.png
│   │   └── academic/
│   │       ├── template.tex
│   │       └── preview.png
│   │
│   └── shared/                      # Shared TypeScript types + validation
│       ├── src/
│       │   ├── types.ts
│       │   └── schemas.ts            # Zod schemas shared between frontend + backend
│       └── package.json
│
├── docker/
│   ├── Dockerfile.web
│   ├── Dockerfile.api
│   └── Dockerfile.latex              # Alpine + TeX Live for PDF compilation
│
├── docker-compose.yml                # Local dev: web + api + postgres + minio
├── .github/
│   └── workflows/
│       ├── ci.yml                    # Lint + type-check + test on PR
│       └── deploy.yml                # Build + push to registry on main
│
├── .gitignore
├── .env.example
├── package.json                      # Monorepo root (pnpm workspaces)
├── pnpm-workspace.yaml
├── turbo.json                        # Turborepo build pipeline
└── README.md
```

---

## 🗃️ Database Schema (Prisma)

```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String
  createdAt DateTime @default(now())
  profile   Profile?
  resumes   Resume[]
}

model Profile {
  id           String        @id @default(cuid())
  userId       String        @unique
  user         User          @relation(fields: [userId], references: [id])
  phone        String?
  linkedinUrl  String?
  githubUrl    String?
  portfolioUrl String?
  cgpa         Float?
  university   String?
  degree       String?
  gradYear     Int?
  skills       Skill[]
  projects     Project[]
  experiences  Experience[]
  achievements Achievement[]
  updatedAt    DateTime      @updatedAt
}

model Skill {
  id          String  @id @default(cuid())
  name        String
  proficiency String?  // beginner | intermediate | advanced
  category    String?  // languages | frameworks | tools | etc.
  profileId   String
  profile     Profile @relation(fields: [profileId], references: [id])
}

model Project {
  id          String   @id @default(cuid())
  title       String
  description String
  techStack   String[]
  githubUrl   String?
  liveUrl     String?
  highlights  String[]
  profileId   String
  profile     Profile  @relation(fields: [profileId], references: [id])
}

model Experience {
  id          String   @id @default(cuid())
  role        String
  company     String
  location    String?
  startDate   DateTime
  endDate     DateTime?
  current     Boolean  @default(false)
  bullets     String[]
  profileId   String
  profile     Profile  @relation(fields: [profileId], references: [id])
}

model Achievement {
  id          String   @id @default(cuid())
  title       String
  description String?
  year        Int?
  profileId   String
  profile     Profile  @relation(fields: [profileId], references: [id])
}

model Resume {
  id           String   @id @default(cuid())
  userId       String
  user         User     @relation(fields: [userId], references: [id])
  jobTitle     String?
  company      String?
  jdText       String   // Original job description
  latexCode    String   // Generated LaTeX
  pdfUrl       String?  // S3/R2 URL
  texUrl       String?  // S3/R2 URL
  templateId   String
  createdAt    DateTime @default(now())
}
```

---

## 🔄 Generation Flow

```
User submits JD
      │
      ▼
jdParser.ts      →  Extracts: required skills, nice-to-haves,
                     role seniority, company name, keywords
      │
      ▼
profileMatcher.ts →  Scores each project/experience/skill
                      against JD requirements, selects top items
      │
      ▼
latexGenerator.ts →  Sends matched profile + JD summary to LLM
                      with a structured LaTeX prompt
      │
      ▼
templateEngine.ts →  Merges LLM output into chosen .tex template
                      with sanitized values
      │
      ▼
compiler.ts       →  Compiles .tex → .pdf using pdflatex / Tectonic
                      inside a Docker sandbox
      │
      ▼
fileStore.ts      →  Uploads .tex + .pdf to S3/R2,
                      stores URLs in Resume table
      │
      ▼
Response to client:  { latexCode, pdfUrl, resumeId }
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js ≥ 20
- pnpm ≥ 9
- Docker + Docker Compose
- OpenAI / Anthropic / Gemini API key
- GitHub OAuth App (for repo import)

### 1. Clone & install

```bash
git clone https://github.com/your-username/resumai.git
cd resumai
pnpm install
```

### 2. Configure environment

```bash
cp .env.example .env
```

Fill in `.env`:

```env
# Database
DATABASE_URL="postgresql://postgres:password@localhost:5432/resumai"

# Auth
NEXTAUTH_SECRET="your-secret"
NEXTAUTH_URL="http://localhost:3000"

# GitHub OAuth
GITHUB_CLIENT_ID="..."
GITHUB_CLIENT_SECRET="..."

# AI Provider (pick one or more)
OPENAI_API_KEY="sk-..."
ANTHROPIC_API_KEY="sk-ant-..."

# File Storage
S3_BUCKET="resumai"
S3_REGION="auto"
S3_ENDPOINT="http://localhost:9000"   # MinIO for local dev
S3_ACCESS_KEY="minioadmin"
S3_SECRET_KEY="minioadmin"
```

### 3. Start local services

```bash
docker-compose up -d   # Starts PostgreSQL + MinIO
```

### 4. Run migrations

```bash
pnpm --filter api db:migrate
pnpm --filter api db:seed      # Optional demo data
```

### 5. Start dev servers

```bash
pnpm dev   # Starts both web (port 3000) and api (port 4000) via Turborepo
```

Open [http://localhost:3000](http://localhost:3000)

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 15, Tailwind CSS, Zustand, React Hook Form |
| Backend | Node.js, Express, tRPC (optional), Zod |
| Database | PostgreSQL, Prisma ORM |
| AI | OpenAI GPT-4o / Claude 3.5 Sonnet / Gemini Pro |
| LaTeX | TeX Live / Tectonic (Dockerized) |
| File Storage | AWS S3 / Cloudflare R2 (MinIO locally) |
| Auth | NextAuth.js (GitHub + Email/Password) |
| Monorepo | pnpm workspaces + Turborepo |
| CI/CD | GitHub Actions |
| Deployment | Vercel (web) + Railway / Fly.io (api + latex worker) |

---

## 📦 Key Scripts

```bash
pnpm dev              # Run all apps in development
pnpm build            # Build all apps
pnpm lint             # ESLint across all packages
pnpm typecheck        # tsc --noEmit across all packages
pnpm test             # Run Vitest test suites

# From apps/api
pnpm db:migrate       # Apply Prisma migrations
pnpm db:studio        # Open Prisma Studio
pnpm db:seed          # Seed demo user + profile
```

---

## 🧩 Adding a New LaTeX Template

1. Create `packages/templates/your-template/template.tex`
2. Use `{{SECTION_NAME}}` placeholders where the engine injects content
3. Add a `preview.png` screenshot (800×1000px)
4. Register the template ID in `apps/api/src/services/latex/templateEngine.ts`
5. The frontend `TemplatePicker` component picks it up automatically

---

## 🔐 Security Notes

- LaTeX compilation runs inside an isolated Docker container with no network access and a CPU/memory cap to prevent TeX injection attacks
- JD and profile text are sanitized for LaTeX special characters (`&`, `%`, `$`, `#`, `_`, `{`, `}`, `~`, `^`, `\`) before being passed to the compiler
- GitHub tokens are never stored — only OAuth short-lived sessions are used
- File download URLs are pre-signed and expire after 1 hour

---

## 📄 License

MIT — see [LICENSE](./LICENSE)

---

## 🙌 Contributing

PRs are welcome. Please open an issue first for large changes. Run `pnpm lint && pnpm typecheck` before submitting.
