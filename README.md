[English](README.md) | [繁體中文](README_zh-TW.md)

# 🚀 AI-Driven Fullstack Project Workflow

This is an enterprise-grade Software Development Life Cycle (SDLC) framework tailored specifically for AI Agents. By leveraging this workflow, users no longer need to write lengthy prompts or manually invoke individual skills. The AI assumes the role of Chief Architect and development team, guiding the user through a complete closed loop from "Market Validation" to "CI/CD Deployment."

---

## 🎯 Core Design Principles
1. **Agent-Led with Human-in-the-loop Gate**: The AI manages the pacing, but **the AI is an engineer, not the CTO**. Critical decisions such as architecture design, DB schema changes, and destructive data migrations must pass a manual "Approve" gate before execution.
2. **Validate Before Building**: Enforces a Problem-Solution Fit validation phase before writing any code, preventing the creation of technically perfect but commercially unviable products.
3. **Extreme Defense & Observability**: Built-in Docker sandboxing, security auditing, advanced observability, and AI cost governance to ensure the system doesn't blow up your budget or leave you blind when debugging.

---

## 🗺️ The 8-Phase SOP

### Phase 0: Problem Framing 🔍 *[Foundation]*
* **Trigger**: The user proposes an initial idea.
* **AI Task**: Before brainstorming features, clarify "Who is the user?", "What is the pain point?", "How is it solved currently?", and "Why are existing solutions inadequate?". This drastically increases the success rate of subsequent commercial validation.

### Phase 1: Concept Clarification
* **Trigger**: Problem framing is complete.
* **AI Task**: Ask 1-2 key questions to clarify the system's core objectives and hero features.

### Phase 2: Market Research & Feature Discovery
* **Trigger**: Core objectives confirmed.
* **AI Task**: Actively use **Web Search** to conduct competitor analysis and compile a "Core Use Case" and "Standard Feature List".
* **💡 Risk Acceptance Mode**: If the system is strictly for internal use, the user can declare **Risk Acceptance Mode** to bypass market validation and jump directly to Phase 3.

### Phase 2.5: Problem-Solution Fit (Validation Loop) 🌟 
* **Trigger**: Feature List confirmed.
* **AI Task**: Do not write code yet. Output a "Hypothesis List" and design an MVP validation strategy (e.g., Landing Page wireframe, Waitlist CTA, Reddit validation post) to prove the idea is worth building.

### Phase 3: Roadmap Planning
* **Trigger**: Validation strategy and core features confirmed.
* **AI Task**: Prioritize features, establish a Roadmap, and clearly define the MVP scope.

### Phase 4: Architecture, Security & Ops 🛡️ *[Enterprise Grade]*
* **Trigger**: MVP Roadmap confirmed.
* **AI Task**:
  1. **Core Architecture & DB Schema**: Recommend frameworks, design the database, and integrate **Data Governance** (PII handling, retention policies, Audit Logs).
  2. **Authorization Model**: Design strict RBAC/ABAC mechanisms to define operational boundaries.
  3. **Scalability & Backpressure**: Configure a Docker sandbox. Introduce Message Queues for async AI tasks with **Job Isolation** (timeout/retry logic) and **Backpressure** (reject requests or downgrade services when queues are overloaded).
  4. **Observability & SLO/SLA**: In addition to JSON Logging, Tracing, and Metrics, define explicit **Service Level Objectives (SLOs)** (e.g., 99.9% uptime, P95 latency < 2s) tied to **Alerting** mechanisms (Slack/Email).
  5. **AI Audit Trail**: Log the AI's critical decision-making processes (generation rationale, tool invocations) for compliance and debugging.
  6. **Cost Governance**: Implement Caching strategies and enforce **Budget Guardrails** (Hard Limits to auto-downgrade models or pause services).
  7. **Rate Limiting**: Enforce API quotas to prevent abuse and free-riding.
  8. **AI Safety Layer**: Integrate Prompt Injection filtering, Tool whitelisting, and PII masking.
  9. **Dependency Risk Management**: Establish fallback strategies for LLM APIs, enforce lockfiles, and mandate regular dependency patching.
  10. **Contract Locking & Versioning**: Lock API specs with OpenAPI. Implement **API Versioning** and **Prompt Versioning**. **The AI is strictly forbidden from altering schemas unprompted**.
  11. **Configuration Management**: Segregate Dev/Staging/Prod configs with default fallbacks. **Hardcoding environment dependencies is strictly prohibited**.

### Phase 5: PRD Generation
* **Trigger**: Architecture approved by a human.
* **AI Task**: Synthesize Phases 1-4 into a highly professional, Markdown-formatted Project Requirement Document (PRD).

### Phase 6: Agile Implementation & Testing 🧪
* **Trigger**: Open a completely new AI chat window for **Context Engineering** (Use the PRD as the System Prompt & Constraints).
* **AI Task**:
  1. Develop strictly inside the Docker Sandbox.
  2. Implement the **Testing Pyramid**: Unit, Integration, and E2E Tests (Playwright/Cypress).
  3. **AI Evaluation**: Integrate Prompt Regression Tests, output quality checks, and Hallucination Baselines.
  4. **Test Data Management**: Create Mock/Seed Data. Using real Production Data in dev/test environments is strictly prohibited.

### Phase 7: Deployment & Documentation 🚀
* **Trigger**: MVP code and tests passed.
* **AI Task**:
  1. **CI/CD Pipeline**: Setup GitHub Actions with Preview Deploys (PR Previews).
  2. **Deployment Strategy**: Define the Staging to Production promotion strategy, implementing **Blue-Green Deployment** or **Canary Release** to lower launch risks.
  3. **Secrets Management**: Enforce the use of Vault/GitHub Secrets. Ensure sensitive data is never exposed in CI/CD logs.
  4. **Disaster Recovery Plan**: Design DB Rollbacks, Feature Flags, and automated backups. **Mandatory Disaster Recovery (DR) Drills** must be executed to ensure backups actually work.
  5. **Documentation Loop**: Auto-generate readable docs including **OpenAPI Specs**, **System Diagrams**, and detailed **Architecture Decision Records (ADR)** to ensure core project knowledge does not reside solely in the AI's memory.

---

## 🛠️ Prerequisites
Ensure your AI Agent has the following skills loaded.
> 📦 **Out of the Box**: This GitHub repository includes a `skills/` directory containing all required skills. Simply copy its contents into your AI Agent's skill library.

* **Orchestration**: `project-spec-generator`, `context-window-management`
* **UX/UI**: `ui-ux-pro-max`, `frontend-design`, `form-cro`
* **Architecture/Backend**: `senior-fullstack`, `database-design`, `backend-dev-guidelines`, `api-patterns`
* **Security**: `cc-skill-security-review`, `backend-security-coder`, `frontend-security-coder`
* **Frontend/Implementation**: `frontend-developer`, `react-best-practices`, `react-patterns`
* **AI/Advanced**: `rag-implementation`, `rag-engineer`, `langgraph`, `agent-evaluation`

---

## 🚀 How to Use (Quick Start)

This framework is not just a document; it is an **Executable AI Prompt System**.

1. **Import Skills**:
   * If you use an Agent (like Antigravity), copy the `skills/` folder into your system's skills directory.
   * If using an AI IDE like **Cursor** or **Windsurf**, include these Markdown skill files in your `.cursorrules` or System Prompt.
   * If using **ChatGPT / Claude**, upload this `README.md` and relevant skill files as context at the start of your chat.

2. **The Trigger Prompt**:
   Once the environment is ready, send this exact prompt to your AI:
   > **"Please read README.md and initiate the [AI-Driven Fullstack Project Workflow]. I want to build a [insert your system/tool idea here]."**

3. **Follow the Flow**: Let the AI take the lead. Answer its guiding questions and enjoy a principal-engineer-level development experience.

---

## ⚠️ Extreme Safety Rules & Precautions

> 💡 **Forensic Lesson (CASCADE-DELETE-RECURSIVE-0426)**: This workflow integrates maximum fail-safes based on a real-world destructive incident.

1. **🐳 Mandatory Docker Sandboxing**: Phase 6 development must happen inside a container. Global host execution is absolutely forbidden.
2. **🚫 PowerShell Wildcard Protection**: 
   * **Always use `-LiteralPath`**: Never use `-Path` for file operations to prevent disastrous regex parsing bugs (e.g., Next.js `[id]` routes being read as wildcards).
   * **No Command Concatenation**: The AI is explicitly forbidden from using `\n` to chain dangerous commands for blind retries.
3. **⛔ Human-in-the-loop Gate**: High-risk operations (Architecture changes, DB Schema modifications, destructive deletions, migrations) MUST be paused for human `Approve`.
4. **💾 Frequent Git Checkpoints**: Force a `git commit` before any major refactor.
5. **🛡️ Security First**: Never write API keys into code. Use `.env` management exclusively.

---

## 🤝 Credits & Collaboration Statement
This open-source framework is the crystallized result of deep collaboration between **Senior Developer (etrnya)** and **AI Agent (Antigravity)**, forged through countless debugging sessions, real-world forensic analyses, and high-level architectural deductions.

We condensed the best practices of SRE, Security, AI Defenses, and Commercial Validation into this guide. Our goal is to help all developers avoid the fatal blind spots of AI-assisted coding and safely build software capable of scaling to millions of users.