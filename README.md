# AI Meeting Task Extractor

A NestJS backend prototype that connects meeting transcripts to actionable GitHub and Jira issues using Gemini and Fireflies.

## Workflow

1. An authenticated user stores the relevant GitHub or Jira integration settings.
2. The meeting service requests a Fireflies bot and persists a pending meeting.
3. A transcript-ready webhook triggers transcript retrieval and Gemini task extraction.
4. Extracted tasks are sent to the configured issue tracker and persisted with external references.
5. Meeting status is recorded as processing, completed, or failed.

## Backend components

- **NestJS / TypeScript:** modular services, REST endpoints, validation, and exception handling.
- **Prisma / PostgreSQL:** users, meetings, and issue records.
- **Authentication:** JWT and role guards, with Redis support and request throttling.
- **Integrations:** Gemini task extraction, Fireflies transcripts, GitHub issues, and Jira issues.
- **API documentation:** Swagger / OpenAPI; application prefix `/api/v1`.

## Explore the code

| Area | Source |
| --- | --- |
| Task extraction | `src/ai/ai.service.ts` |
| Meeting creation and access | `src/meeting/meeting.service.ts` |
| Transcript workflow | `src/webhook/webhook.service.ts` |
| Database models | `prisma/schema.prisma` |
| Guards and configuration | `src/app.module.ts` |

## Local development

Use a Node.js version compatible with the installed dependencies and a local PostgreSQL and Redis instance. Review `package.json` before installing.

```sh
git clone https://github.com/abdelrahman-abusleem/ai-meeting-task-extractor.git
cd ai-meeting-task-extractor
npm ci
```

Copy `.env.example` to `.env`, supply your own local configuration, and review `src/config`, `prisma.config.ts`, and the integration services for additional required variables, including `GEMINI_API_KEY`. The example environment file comes from the authentication starter and does not yet document every integration variable.

After configuring the database and integrations:

```sh
npx prisma generate
npm run start:dev
```

Prepare the database schema using the repository migrations and your local Prisma configuration. The existing Docker Compose file provisions MySQL and Redis from the starter; it does not currently provide the PostgreSQL database used by the Prisma schema.

## Validation commands

```sh
npm run build
npm run test:unit
npm run test:e2e
```

These are repository scripts, not a claim that the current build or tests pass. External API calls need your own service credentials and development accounts.

## Development status

This is a prototype. The webhook currently matches the latest pending meeting rather than an explicit correlation identifier; concurrent meeting processing needs further work before production use. A complete environment example, PostgreSQL Compose setup, and reproducible integration tests are next steps.

## Attribution

The repository includes authentication-starter work credited to Anil Ahir in `package.json` and retains the existing MIT license. The meeting and AI modules extend that foundation; see Git history for individual contributions.
