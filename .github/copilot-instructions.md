# GitHub Copilot Instructions for Activepieces

## Project Overview

Activepieces is an open-source workflow automation platform (alternative to Zapier, Make, n8n). It allows users to connect different apps and automate workflows through a visual flow builder. This is a fork of the main activepieces repository.

### Tech Stack

- **Frontend**: React, Angular, Nx monorepo, Tailwind CSS, NgRx
- **Backend**: NestJS, Fastify, TypeScript
- **Database**: PostgreSQL (with PGLite for development)
- **Queue**: BullMQ with Redis
- **AI Integration**: AI SDK with support for OpenAI, Anthropic, Azure, Google, Replicate
- **Build**: Nx monorepo, Bun package manager

## Project Structure

```
activepieces/
├── packages/
│   ├── cli/              # Command-line tools for creating pieces
│   ├── ee/               # Enterprise edition features
│   ├── engine/           # Flow execution engine
│   ├── pieces/           # Integration pieces (connectors)
│   │   └── pieces/       # Individual piece packages
│   ├── react-ui/         # React frontend application
│   ├── server/           # NestJS backend API
│   ├── shared/           # Shared utilities and types
│   └── tests-e2e/        # End-to-end tests
├── tools/                # Build and deployment scripts
├── docs/                 # Documentation
└── deploy/               # Docker and deployment configs
```

## Key Concepts

### Pieces (Integrations)
Pieces are reusable integration modules that connect to external services. Each piece contains:
- **Actions**: Operations that perform tasks (e.g., send email, create record)
- **Triggers**: Events that start flows (e.g., webhook received, schedule)

### Flows
A flow is an automated workflow consisting of:
- **Trigger**: What starts the flow
- **Steps**: Actions to execute in sequence
- **Branches**: Conditional logic

### Flow Engine
The engine executes flows step by step:
1. Receives trigger event
2. Executes actions in order
3. Handles branching logic
4. Manages error handling and retries

## Commands

```bash
# Development
npm run dev              # Start all services (frontend, backend, engine)
npm run dev:backend      # Start backend and engine only
npm run dev:frontend     # Start frontend with backend

# Build
npm run build            # Build all packages

# CLI for pieces
npm run create-piece     # Create a new piece
npm run create-action    # Add action to existing piece
npm run create-trigger   # Add trigger to existing piece
npm run build-piece      # Build a piece
npm run publish-piece    # Publish piece to API

# Testing
npm run test:e2e         # Run end-to-end tests

# i18n
npm run i18n:extract     # Extract translation strings
```

## Creating a New Piece

```typescript
// packages/pieces/pieces/my-piece/src/index.ts
import { createPiece, Piece } from '@activepieces/pieces-framework';

export const myPiece = createPiece({
  displayName: 'My Service',
  description: 'Connect to My Service API',
  auth: myAuth,
  minimumSupportedRelease: '0.5.0',
  logoUrl: 'https://example.com/logo.png',
  authors: ['Your Name'],
  actions: [myAction],
  triggers: [myTrigger],
});
```

### Creating Actions

```typescript
import { createAction } from '@activepieces/pieces-framework';

export const myAction = createAction({
  name: 'my_action',
  displayName: 'My Action',
  description: 'Perform an action',
  props: {
    // Define input properties
  },
  run: async (context) => {
    // Implementation
    return result;
  },
});
```

## Environment Variables

Key environment variables (see `.env.example`):
- `AP_API_KEY` - API key for authentication
- `AP_ENGINE_KEY` - Engine secret key
- `AP_ENCRYPTION_KEY` - Data encryption key
- `AP_JWT_SECRET` - JWT signing secret
- `AP_POSTGRES_URL` - PostgreSQL connection
- `AP_REDIS_URL` - Redis connection
- `AP_FRONTEND_URL` - Frontend URL

## Architecture

### Frontend (React UI)
- Flow builder with drag-and-drop interface
- Dashboard for managing flows and runs
- Settings and user management
- Built with React, Tailwind CSS

### Backend (Server)
- NestJS-based API
- PostgreSQL for persistence
- Redis/BullMQ for job queue
- File storage (S3-compatible)

### Engine
- Executes flow steps
- Handles webhooks and triggers
- Manages piece connections
- Supports AI actions

## Code Style Guidelines

1. **TypeScript**: Strict typing throughout
2. **Nx**: Use Nx generators for new packages
3. **Pieces**: Follow the piece structure conventions
4. **Testing**: Write tests for new pieces and actions
5. **Linting**: Run linting before commits

## Notes for Copilot

1. This is a fork - check upstream for latest changes
2. When creating new pieces, use the CLI commands
3. Pieces are stored in `packages/pieces/pieces/`
4. The engine handles flow execution - don't modify core engine unless necessary
5. Use shared types from `packages/shared/`
6. Follow existing piece patterns when adding integrations
7. AI actions use the AI SDK with multiple provider support

## Resources

- Official docs: https://www.activepieces.com/docs
- Piece development: https://www.activepieces.com/docs/developers/build-piece
- Community: https://discord.gg/activepieces