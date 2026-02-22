# @devvir/service

Service lifecycle management for microservices. Handles initialization, health checks, and graceful shutdown with minimal boilerplate.

## Quick Start

```typescript
import { defineLifecycle, logger } from '@devvir/service';

const lifecycle = defineLifecycle({
  dependencies: ['mongodb', 'rabbitmq'],

  onInit: async () => {
    // Set up connections, load config, etc.
    const mongo = await connectMongo();
    return { mongo };
  },

  onPing: async () => ({
    messagesProcessed: 1234,
    uptime: process.uptime(),
  }),

  onShutdown: async () => {
    // Close connections gracefully
    await mongo.client.close();
  },
});

await lifecycle.init();
```

## Features

- **Automatic health check server** - Listens on port 3000 by default
- **Graceful shutdown** - Handles SIGTERM/SIGINT signals
- **Dependency tracking** - Automatically monitors declared dependencies
- **Event-driven** - Emit/listen for init, ready, failure, done events
- **No magic** - Simple async/await, easy to debug

## Configuration

See [docs/LIFECYCLE.md](docs/LIFECYCLE.md) for full API reference.

## Modules

- **Lifecycle** - Core service initialization and shutdown orchestration
- **Health Checks** - HTTP /health endpoint with custom status reporting
- **Shutdown** - Signal handling and graceful termination
- **Logger** - Structured logging via Pino

For detailed documentation on each module, see the [docs](docs/) folder.
