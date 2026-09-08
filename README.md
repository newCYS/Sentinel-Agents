# Agent Orchestration Kit — Security-First Multi-Agent Team

One orchestrator + five specialist subagents (Coding, Security Testing, DevOps,
Infrastructure, Documentation), defined natively for **Claude Code**, **OpenCode**, and
**Codex CLI**. The goal: no single coding agent's output ships without a dedicated,
independent security review — catching SQL injection, XSS, and broken auth before merge,
regardless of stack.

## 1. Fill in `plan.md` first — this is required, not optional
Every agent in every platform reads `plan.md` before doing anything. Section 2 (Stack) must
be filled in — MERN, Next.js, Spring Boot, ASP.NET, or whatever you're actually running —
because the Security Testing and Coding agents behave differently per framework (e.g. what
"safe query" and "safe render" look like). Copy `plan.md` to your project root and fill it
in before installing any agent files.

## 2. Install per platform

### Claude Code
```
cp -r claude-code/agents/*.md  <your-project>/.claude/agents/
cp plan.md <your-project>/plan.md   # fill in first if you haven't
```
Verify with `/agents` inside Claude Code — you should see orchestrator, coding,
security-testing, devops, infrastructure, documentation listed.

### OpenCode
```
cp -r opencode/agent/*.md <your-project>/.opencode/agent/
cp plan.md <your-project>/plan.md
```
The orchestrator is `mode: primary` (selectable as your main agent); the rest are
`mode: subagent` and are invoked automatically by the orchestrator, or manually with
`@coding-agent`, `@security-testing`, etc.

### Codex CLI
```
cp codex/AGENTS.md <your-project>/AGENTS.md
mkdir -p <your-project>/.codex/agents
cp codex/agents/*.toml <your-project>/.codex/agents/
# merge codex/config.toml's [agents] block into <your-project>/.codex/config.toml
cp plan.md <your-project>/plan.md
```
Codex's main session (guided by `AGENTS.md`) plays the orchestrator role and spawns the
five TOML-defined agents via `spawn_agent`.

## 3. Non-negotiable rule across all three
A task is never marked done while the security-testing agent has an open BLOCKER or MAJOR
finding — this is written into every orchestrator/root instruction file. If you ever see an
orchestrator skip that gate, that's a bug in your local edits, not the intended design.

## Files
```
plan.md                          <- fill in stack + backlog, shared by all agents
claude-code/agents/*.md          <- .claude/agents/ format
opencode/agent/*.md              <- .opencode/agent/ format
codex/AGENTS.md                  <- root orchestrator instructions for Codex
codex/config.toml                <- [agents] registration snippet
codex/agents/*.toml              <- Codex-native subagent definitions
```
