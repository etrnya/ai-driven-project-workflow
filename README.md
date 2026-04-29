[English](README.md) | [繁體中文](README_zh-TW.md)

# 🚀 AI-Driven Fullstack Project Workflow

This is an enterprise-grade Software Development Life Cycle (SDLC) framework tailored specifically for AI Agents. By leveraging this workflow, users no longer need to write lengthy prompts or manually invoke individual skills. The AI assumes the role of Chief Architect and development team, guiding the user through a complete closed loop from "Market Validation" to "CI/CD Deployment."

---

## 💡 Why This Exists

This framework was born from two very real, very personal frustrations:

**1. Great wheels that never got used at the right time**
After learning how to use AI Skills (modular capability packs), I discovered that the internet already has a wealth of high-quality, community-built Skills — covering architecture design, security auditing, frontend development, RAG, and more. The problem was: **I could never remember which skill to call, or when, or what trigger phrase to use.** All those great wheels just sat quietly in a folder, never properly utilized.

So my idea was: **build a larger orchestration layer on top of those wheels.** One that understands which phase of development I’m in and automatically invokes the right skill at the right moment — instead of relying on my memory to trigger them manually.

**2. The C-Drive Massacre (CASCADE-DELETE-RECURSIVE-0426)**
While developing with Antigravity, a PowerShell command executed by the AI misinterpreted a wildcard pattern and triggered a mass deletion of local files. That incident made it viscerally clear: **without mandatory sandboxing and frequent backup mechanisms, AI-assisted development is a ticking time bomb.**

This framework converts both of those hard lessons into concrete, enforceable rules — so that anyone, regardless of technical background, can build industrial-grade software with AI in a safe, structured way.

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
* **AI Task**: Prioritize features, establish a Roadmap, and clearly define the MVP scope. Apply the following **Feature Triage Criteria** to prevent MVP scope creep:
  * ❌ **Cut to Backlog**: Any feature estimated at more than 3 days of work, not critical to the core user flow, or only needed in rare edge cases.
  * ✅ **Keep in MVP**: Features the user absolutely cannot do without — removing them would make the product fundamentally unusable.
  * 💡 **Principle**: Do less, do it well. The goal of an MVP is to be *usable*, not *complete*.

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
  12. **Architecture Sanity Check** 🔍: Before a human approves the architecture proposal, **every item below must be manually verified** to prevent AI hallucinations from derailing the entire project:
      * Do the recommended packages/frameworks actually exist? Are the version numbers the latest stable releases?
      * Are the referenced APIs or services still actively maintained, and not deprecated?
      * Does the DB Schema align with the Glossary definitions, with no self-invented terminology?
      * **Verification method**: Ask the AI to provide official documentation links for every package it recommended. If it cannot provide them or links are dead, treat it as a hallucination warning and re-verify.
  13. **Internationalization (i18n)**：**This decision MUST be made at the architecture stage — retrofitting i18n later is extremely costly.** Answer the following explicitly:
      * Does this product need to support multiple languages or regions? (Even if you're only launching in one language now, design the extension path upfront.)
      * If yes: select an i18n framework (e.g., `next-intl`, `i18next`), define the language resource file structure, and ensure **all UI text, error messages, and email templates are strictly prohibited from being hardcoded**.
      * If no: explicitly record in the PRD: "Currently single-language. Future multi-language support will require refactoring the UI string layer."

### Phase 5: PRD Generation
* **Trigger**: Architecture approved by a human.
* **AI Task**: Synthesize Phases 1-4 into a highly professional, Markdown-formatted Project Requirement Document (PRD). The PRD **must include the following two fixed sections**:
  * **Why This Exists**: Explain why this software was built, whose problem it solves, and why existing solutions are insufficient. This is the "north star" section for future users, contributors, and your future self.
  * **Licensing Placeholder**: Pre-fill the intended license (e.g., MIT) and include a reminder that a complete `ATTRIBUTION.md` must be generated in Phase 7.

### Phase 5.5: Glossary (Domain Terminology Dictionary) 📖 *[Alignment Foundation]*
* **Trigger**: PRD confirmed, before writing any code.
* **AI Task**: Produce a standalone `GLOSSARY_V1.md` that defines every domain-specific term in the project with a **single, authoritative** definition. Examples:
  * **Business Concepts** (e.g., the boundary between "Order" vs. "Request")
  * **Data Entities** (e.g., what exactly `User` / `Account` / `Member` each refer to)
  * **System Terms** (e.g., the difference between "Job", "Event", and "Workflow")
* **Mandatory Enforcement**: All subsequent phases — architecture design, API naming, DB Schema, and frontend UI copy — **must strictly conform to this dictionary**. The AI is forbidden from using synonymous but inconsistently named terms across different files.
* **Living Document**: Whenever a new concept is introduced, the Glossary must be updated and its version incremented (e.g., `GLOSSARY_V2.md`) before development continues.

### Phase 6: Agile Implementation & Testing 🧪
* **Trigger**: Once PRD and Glossary are confirmed, **open a brand new AI chat window** and follow this **Context Switch Protocol** to ensure the AI enters development with the correct rules — not guessing from a blank slate:
  1. Paste the **full contents** of `PRD_Vx.md` as the very first message (System Prompt).
  2. Immediately follow with the full `GLOSSARY_Vx.md`, telling the AI: "The following are the only authoritative definitions for all terms in this project. You must strictly follow them throughout development."
  3. If PRD + Glossary together exceed 8,000 words, paste the **condensed summary** instead (have the AI produce a `PRD_SUMMARY_Vx.md` at the end of Phase 5), to avoid overflowing the Context Window.
  4. Close with: "Please begin Phase 6 development according to the specs above. Before making any change, explain your plan and wait for my confirmation."
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
  6. **ATTRIBUTION.md Generation**: Scan all direct dependencies (npm / pip / Cargo, etc.) and produce an `ATTRIBUTION.md` listing each package's name, version, and license. This ensures the project meets all third-party attribution obligations before public release.

### Phase 8: Post-Launch Feedback Loop 🔄 *[Continuous Evolution]*
* **Trigger**: Product is live in production.
* **AI Task**: Shipping is not the finish line — it’s the start of the real test. Establish the following continuous iteration mechanisms:
  1. **User Feedback Collection**: Set up structured feedback channels (e.g., GitHub Issue templates, in-app feedback forms, or periodic user interviews) so real voices are systematically captured — not lost in chat logs.
  2. **Bug Triage & Prioritization**: Weekly, have the AI help sort the reported issues by "number of users affected × severity" to ensure limited development time is spent where it matters most.
  3. **Iteration Loop Back**: New feature requests or major bug fixes must go back through **Phase 3 Roadmap** for re-prioritization before jumping into Phase 6 coding — to prevent feature chaos and accumulating technical debt.
  4. **SLO Monitoring Review**: Monthly, compare actual metrics against the SLOs defined in Phase 4 (uptime, P95 latency, etc.). Persistent failures must trigger an architecture review (back to Phase 4).
  5. **Skill Update Sync**: When AI tools or Skills receive major updates, evaluate whether this framework’s invocation strategy and PRD templates need to be revised accordingly.

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

## 🆘 Stuck? Here's What to Do

Getting stuck at a Phase — not knowing how to answer the AI's questions — is completely normal, even for non-technical users. Here are two ways to handle it:

### 🆙 Option A: Ask for Help (When you don't know how to answer)
Say any of the following directly to the AI:

> **"I don't quite understand this question. Can you explain why it matters and give me an example that someone with no technical background can follow?"**

> **"I don't have the background knowledge to answer this. Can you give me a few options to choose from instead?"**

### ⏩ Option B: Skip It (When you genuinely don't know and it won't block progress)

> **"I don't have an answer to this right now. I'd like to proceed with a sensible default and revisit if needed. Please suggest the most reasonable assumption and explain why."**

> ⚠️ **Skip Rule**: You may only skip once per Phase, and every skipped decision must be noted in the PRD. If you find yourself skipping every Phase, go back to Phase 0 and re-clarify the project goal.

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
6. **📄 File Versioning Convention**: All important AI-generated documents (PRD, Glossary, architecture diagrams, etc.) must follow the naming template below. Each revision increments the version number. Overwriting previous versions is strictly forbidden:
   ```
   {filename}_V{versionSeq}.md
   ```
   Example: `PRD_V1.md` → `PRD_V2.md`, `GLOSSARY_V1.md` → `GLOSSARY_V2.md`

---

## 🤝 Credits & Collaboration Statement
This open-source framework is the crystallized result of deep collaboration between **a complete non-programmer (etrnya)** and **AI Agent (Antigravity)**, forged through countless debugging sessions, real-world forensic analyses, and high-level architectural deductions.

etrnya has never written a formal software project and doesn't understand programming languages — yet through intense collaboration with AI, was still able to drive the design and development of an industrial-grade architecture and workflow. That is precisely the most powerful statement this framework makes: **you don't need to be an engineer to think, decide, and build like one.**

We condensed the best practices of SRE, Security, AI Defenses, and Commercial Validation into this guide. Our goal is to help everyone who feels intimidated by technology avoid the fatal blind spots of AI-assisted coding and safely build software capable of scaling to millions of users.

---

## 📜 Licensing & Attribution

The framework documentation in this project (READMEs, workflow guides, etc.) is released under the **MIT License**.

The AI Skills bundled in the `skills/` directory are sourced from the
**[antigravity-awesome-skills](https://github.com/antigravity-awesome-skills)** curated collection.
Their content is protected under **CC BY 4.0** and **MIT** licenses depending on origin.

> Skills used in this project are sourced from [antigravity-awesome-skills](https://github.com/antigravity-awesome-skills), licensed under CC BY 4.0 and MIT.

For the full list of third-party sources and licenses, see **[ATTRIBUTION.md](./ATTRIBUTION.md)**.