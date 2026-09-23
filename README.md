# AI Productivity Assistant

> An AI-powered task prioritization web app that helps users decide what to work on first and estimate the time required.

**Lipcoding 2026 · Challenge Project**  
`JavaScript` `GitHub Models API` `AI` `localStorage` `Azure Static Web Apps` `UX`

## Product Idea

When a task list becomes long, the problem is often not capturing tasks — it is deciding **what matters now**. This project explores that problem through a lightweight AI assistant that analyzes tasks and returns priority, estimated time, category, and reasoning.

The product was designed from both a technical and service perspective: reduce decision friction, keep the interaction simple, and make the core flow demonstrable even when an API token is unavailable.

## Target Users

- Students and professionals managing multiple tasks
- Early adopters interested in AI-assisted productivity
- Users comfortable providing their own GitHub Personal Access Token for live AI analysis

## Key Features

- Add, complete, and delete tasks
- AI-based priority and estimated-time analysis
- Task-specific voice input
- Sample task generation
- Demo analysis without an API token
- Persistent task and analysis state with `localStorage`
- GitHub token save/delete controls
- Step-by-step validation feedback inside the app
- Full local-data reset from the privacy/data-management area

## Product & UX Decisions

### User-centered flow

The interface is designed around a simple question: **“What should I do first?”** The analysis result is presented as actionable cards rather than raw model output.

### Demo-first evaluation

Because judges or reviewers may not be able to provide an API token, a demo-analysis path allows the complete interaction flow to be evaluated without credentials.

### Privacy-aware local storage

Task data is stored in the user's browser instead of a separate application database. The data reset action is intentionally placed within the privacy/local-data context so its meaning is clearer to users.

## Architecture

```text
ai-productivity-assistant/
├── index.html
├── staticwebapp.config.json
├── README.md
├── PRD.md
└── AGENTS.md
```

The application uses a deliberately lightweight static architecture. AI requests use the GitHub Models API with `gpt-4o-mini`; deployment is handled through Azure Static Web Apps.

## AI & Token Handling

- Tokens are not hardcoded in the repository.
- Live AI analysis runs only with a token supplied by the user.
- A demo path is available when credentials cannot be provided.
- A production architecture that centrally protects API credentials would require a server-side proxy such as Azure Functions rather than exposing a shared secret to browser code.

## Validation Flow

1. Save/delete token
2. Add a task
3. Complete or remove a task
4. Test voice input
5. Refresh and confirm persisted data
6. Run demo or live AI analysis
7. Confirm priority, estimated time, category, and reasoning cards
8. Refresh and confirm analysis restoration

## Deployment

The project is deployed with Azure Static Web Apps through GitHub Actions.

**Live App:** https://red-smoke-003835800.7.azurestaticapps.net/

## Repository

https://github.com/solbao-dev/ai-productivity-assistant

## What I Learned

This challenge combined implementation with the part of product development I care about most: understanding the user's decision problem and translating it into a focused service experience. It also highlighted the trade-offs between a fast static prototype, credential security, privacy, deployment simplicity, and evaluation usability.

---

Built for **Lipcoding 2026** as part of my **Challenges & Hackathons** journey.