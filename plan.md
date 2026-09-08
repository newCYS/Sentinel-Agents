# plan.md — Project Contract for the Agent Team

> Every agent (Orchestrator, Coding, Security Testing, DevOps, Infrastructure, Documentation)
> reads this file FIRST, before touching any task. It is the single source of truth for
> "what are we building and how." If this file is missing or stale, agents must stop and
> ask for it to be filled in rather than guessing the stack.

## 1. Project Identity
- **Project name:**
- **One-line purpose:**
- **License model:** open-source | closed-source | dual  <!-- REQUIRED: changes what Security/DevOps agents may assume about public exposure -->

## 2. Stack (REQUIRED — no agent should infer this)
- **Type:** MERN | Next.js | Spring Boot | ASP.NET | Django | Laravel | Rails | other: ____
- **Language(s):**
- **Frontend framework:**
- **Backend framework:**
- **Database:** (e.g. PostgreSQL, MongoDB, MySQL)
- **ORM / query layer:** (e.g. Prisma, Mongoose, Hibernate, Entity Framework, raw SQL)
- **Auth mechanism:** (e.g. JWT, session cookies, OAuth provider)
- **Deployment target:** (e.g. Docker + Kubernetes, Vercel, AWS ECS, bare VM)

## 3. Non-negotiable security baseline
This applies regardless of stack. The Security Testing Agent enforces these on every PR:
- No raw string concatenation into SQL queries — parameterized queries / ORM only.
- All user input rendered in HTML must be escaped or use the framework's safe-render path
  (no `dangerouslySetInnerHTML`, no `innerHTML =`, no unescaped template output).
- No secrets, API keys, or credentials committed to the repo (checked against `.env.example`).
- Authentication/authorization checks live server-side; never trust client-side gating alone.
- Dependencies are pinned and scanned (see DevOps Agent).

## 4. Task backlog
| # | Task | Owner agent | Status |
|---|------|-------------|--------|
| 1 | | coding | pending |

## 5. Definition of done (per task)
A task is only "done" when:
1. Coding Agent's change passes existing tests + adds new ones for the change.
2. Security Testing Agent has reviewed the diff and reported zero unresolved BLOCKER/MAJOR findings.
3. DevOps Agent confirms the CI pipeline is green (build, lint, test, dependency scan).
4. Documentation Agent has updated the relevant README/CHANGELOG/docs section.
5. Orchestrator has recorded the outcome back into this file's backlog table.

## 6. Escalation rule
If Security Testing Agent and Coding Agent disagree (e.g. a fix breaks a feature), the
Orchestrator escalates to the human — no agent may silently override a security finding
to make a task "pass."
