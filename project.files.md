
# Common Project Files Overview

| File / Path                                           | Purpose                                               | Typical Contents                                    | When to Include           | Notes                          |
|-------------------------------------------------------|-------------------------------------------------------|-----------------------------------------------------|---------------------------|--------------------------------|
| README.md                                             | Main entry point; explains project, usage, and setup. | Description, installation, usage, examples, badges. | Always.                   | Keep concise; link to docs/.   |
| [CHANGELOG.md](CHANGELOG.template.md)                 | Tracks version history and changes.                   | Version sections, Added/Changed/Fixed, issue links. | Always.                   | Follow "Keep a Changelog".     |
| .gitignore                                            | Excludes unnecessary files from Git.                  | Build files, logs, IDE configs.                     | Always.                   | Use GitHub language templates. |
| .gitattributes                                        | Defines file handling and diffs.                      | Line endings, linguist overrides.                   | Cross-platform repos.     | Prevents CRLF issues.          |
| Dockerfile                                            | Container definition.                                 | Base image, build steps, healthcheck.               | Deployable apps/services. | Use multi-stage builds.        |
| docker-compose.yml                                    | Local development stack.                              | DB, cache, queues, services.                        | Microservices or DB apps. | Provide .env.example.          |
| .env.example                                          | Documents environment variables.                      | Placeholder values.                                 | Projects using env vars.  | Never commit actual secrets.   |
| ENVIRONMENT.md                                        | Explains runtime configuration.                       | Env var descriptions.                               | Configurable apps.        | Pair with .env.example.        |
| docs/                                                 | Documentation collection.                             | Architecture, API docs, ADRs.                       | Medium/large projects.    | Organize clearly.              |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.template.md) | High-level system design.                             | Modules, diagrams, flows.                           | Onboarding and planning.  | Update regularly.              |
| docs/adr/                                             | Architecture Decision Records.                        | Decision history.                                   | Long-term projects.       | One decision per ADR.          |
| LOCAL_DEV.md                                          | Developer setup guide.                                | Tools, commands, troubleshooting.                   | Team repos.               | Keep setup under 5 minutes.    |
| DEPLOYMENT_HISTORY.md                                 | Chronological record of all deployments               |                                                     |                           |                                |
| RELEASE_PROCESS.md                                    |                                                       |                                                     |                           |                                |
| FAQ.md                                                | Common questions and how to solve them.               |                                                     |                           |                                |
| ROADMAP.md                                            | Project direction, planned milestones.                |                                                     |                           |                                |


### README.md

README.md is the entry point for GitHub/GitLab — it must stay in the repository root.

### CHANGELOG.md

CHANGELOG.md is also commonly kept at root because it describes the evolution of code, not internal documentation.

### LOCAL_DEV.md

This file answers a single message:
**"If I'm a developer joining this project, how do I run it on my laptop?"**

- It is about **primary workflow for local development**.
- It must be linear, task-oriented, and step-by-step.

Think of LOCAL_DEV.md as a **tutorial**.

Anyone new should follow it from top to bottom without guessing.

### FAQ.md

FAQ.md is:
- not a tutorial
- not linear

It is a collection of answers to recurring problems or **troubleshooting** questions people ask repeatedly.

Its purpose: **Reduce repeated questions in the team.**

FAQ = troubleshooting + tribal knowledge + shortcuts + edge cases.

Typical questions:
- "What to do if the build fails with Java version mismatch?"
- "How do I rebuild only module X?"
- "How do I purge Docker volumes if the database gets corrupted?"
- "How do I run only one service from the composition?"
- "How do I switch environments?"
- "How do I fix IntelliJ indexing issues?"
- "What to do if tests fail due to missing snapshots?"
- "Build fails with UnsupportedClassVersionError. Solution: install Java 21."
- "Docker complains port 5432 is used. How to find and kill the process?"
- "How to reset testcontainers cache?"
- "How to run only one integration test class?"

### Topics that might appear both in LOCAL_DEV.md AND FAQ.md

| Item                                                                  | LOCAL_DEV.md           | FAQ.md                        |
|-----------------------------------------------------------------------|------------------------|-------------------------------|
| How to build the project                                              | YES (main workflow)    | Maybe (if build often breaks) |
| How to run the service in Docker composition                          | YES                    | NO                            |
| How to switch from one environment to another                         | YES (as part of setup) | YES (if tricky/confusing)     |
| What to do if something doesn't work (e.g., wrong Java/Maven version) | NO                     | YES                           |

### RELEASE_PROCESS.md

- It describes the official pipeline
- It explains how artifacts are built and where they go
- It documents how ITO or developers should request a new build
- It helps new teammates understand the whole lifecycle of the artifact

### DEPLOYMENT_HISTORY.md

It stores a clear, chronological record of all deployments — including versions, config files, and JIRA tickets — 
so the team can easily trace what was deployed, when, and why.

It acts as a single source of truth for redeployments, audits, and environment reconstruction months later.

