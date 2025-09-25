# Testing Strategy for Memetic Messenger

## Overview

Our testing strategy uses **Tryorama** (Holochain's official testing framework) for backend zome testing and standard web testing tools for the frontend.

## Testing Architecture

### 1. **Backend Testing** (Holochain Zomes)

**Tool**: `@holochain/tryorama` + Node.js test runner (tape/vitest)

**Structure:**
```
tests/
├── src/
│   ├── integration/           # Multi-zome integration tests
│   │   ├── clock-calendar.ts
│   │   ├── identity-contact.ts
│   │   ├── communication-membrane.ts
│   │   └── dashboard-aggregation.ts
│   ├── zomes/                # Individual zome tests
│   │   ├── clock.test.ts
│   │   ├── calendar.test.ts
│   │   ├── identity.test.ts
│   │   ├── address-book.test.ts
│   │   ├── membrane-manager.test.ts
│   │   ├── communication.test.ts
│   │   ├── frames.test.ts
│   │   └── dashboard.test.ts
│   └── scenarios/            # Complex multi-agent scenarios
│       ├── messaging-flow.ts
│       ├── membrane-interaction.ts
│       └── cross-applet-integration.ts
├── package.json
└── tsconfig.json
```

### 2. **Frontend Testing** (Svelte + TypeScript)

**Tools**: Vitest + Testing Library + Playwright

**Structure:**
```
ui/
├── src/
│   └── tests/
│       ├── unit/             # Component unit tests
│       ├── integration/      # Integration with Holochain
│       └── e2e/             # End-to-end user flows
├── playwright.config.ts
└── vitest.config.ts
```

## Test Implementation Examples

### Basic Zome Test (Tryorama)

```typescript
// tests/src/zomes/clock.test.ts
import test from 'tape';
import {
  runScenario,
  Player,
  Scenario,
  addAllAgentsToAllConductors,
} from '@holochain/tryorama';

test('Clock zome - create and retrieve timestamp', async (t) => {
  await runScenario(async (scenario: Scenario) => {
    // Create conductors (agents)
    const [alice]: Player[] = await scenario.addPlayersWithApps([{
      appBundlePath: './workdir/memetic-messenger.happ',
    }]);

    await scenario.shareAllAgents();

    // Test creating a timestamp
    const timestamp = {
      unix_time: Date.now(),
      timezone: 'UTC',
      precision: 'Millisecond',
    };

    const createResult = await alice.cells[0].callZome({
      zome_name: 'clock_coordinator',
      fn_name: 'create_timestamp',
      payload: timestamp,
    });

    t.ok(createResult, 'Timestamp created successfully');

    // Test retrieving the timestamp
    const getResult = await alice.cells[0].callZome({
      zome_name: 'clock_coordinator',
      fn_name: 'get_timestamp',
      payload: createResult.signed_action.hashed.hash,
    });

    t.deepEqual(getResult.entry, timestamp, 'Retrieved timestamp matches created timestamp');
  });
});
```

### Multi-Zome Integration Test

```typescript
// tests/src/integration/clock-calendar.test.ts
import test from 'tape';
import { runScenario, Player, Scenario } from '@holochain/tryorama';

test('Clock-Calendar integration - sync schedules with events', async (t) => {
  await runScenario(async (scenario: Scenario) => {
    const [alice]: Player[] = await scenario.addPlayersWithApps([{
      appBundlePath: './workdir/memetic-messenger.happ',
    }]);

    await scenario.shareAllAgents();

    // Create a schedule in Clock zome
    const schedule = {
      title: 'Daily Standup',
      start_time: {
        unix_time: Date.now(),
        timezone: 'UTC',
        precision: 'Second',
      },
      end_time: null,
      recurrence: {
        frequency: 'Daily',
        interval: 1,
        end_date: null,
      },
      creator: alice.agentPubKey,
    };

    const scheduleResult = await alice.cells[0].callZome({
      zome_name: 'clock_coordinator',
      fn_name: 'create_schedule',
      payload: schedule,
    });

    // Sync with Calendar zome
    const syncResult = await alice.cells[0].callZome({
      zome_name: 'calendar_coordinator',
      fn_name: 'sync_with_schedules',
      payload: {},
    });

    t.ok(syncResult.length > 0, 'Calendar successfully synced with schedules');

    // Verify event was created
    const events = await alice.cells[0].callZome({
      zome_name: 'calendar_coordinator',
      fn_name: 'get_my_events',
      payload: {},
    });

    t.equal(events.length, 1, 'One event created from schedule');
    t.equal(events[0].entry.title, schedule.title, 'Event title matches schedule');
  });
});
```

### Multi-Agent Communication Test

```typescript
// tests/src/scenarios/messaging-flow.test.ts
import test from 'tape';
import { runScenario, Player, Scenario } from '@holochain/tryorama';

test('Multi-agent messaging flow', async (t) => {
  await runScenario(async (scenario: Scenario) => {
    const [alice, bob]: Player[] = await scenario.addPlayersWithApps([
      { appBundlePath: './workdir/memetic-messenger.happ' },
      { appBundlePath: './workdir/memetic-messenger.happ' },
    ]);

    await scenario.shareAllAgents();

    // Alice adds Bob as a contact
    const contact = {
      agent_key: bob.agentPubKey,
      display_name: 'Bob',
      nickname: 'Bobby',
      relationship: 'Friend',
      groups: ['Friends'],
      notes: 'Met at conference',
      added_at: {
        unix_time: Date.now(),
        timezone: 'UTC',
        precision: 'Second',
      },
      last_interaction: null,
    };

    await alice.cells[0].callZome({
      zome_name: 'address_book_coordinator',
      fn_name: 'create_contact',
      payload: contact,
    });

    // Alice sends a direct message to Bob
    const message = {
      content: {
        text: 'Hello Bob!',
        attachments: [],
        mentions: [bob.agentPubKey],
        hashtags: [],
      },
      sender: alice.agentPubKey,
      recipients: [bob.agentPubKey],
      membrane_hash: null,
      thread_root: null,
      reply_to: null,
      timestamp: {
        unix_time: Date.now(),
        timezone: 'UTC',
        precision: 'Second',
      },
      message_type: 'Direct',
    };

    const messageResult = await alice.cells[0].callZome({
      zome_name: 'communication_coordinator',
      fn_name: 'create_message',
      payload: message,
    });

    t.ok(messageResult, 'Message sent successfully');

    // Bob retrieves direct messages from Alice
    const bobMessages = await bob.cells[0].callZome({
      zome_name: 'communication_coordinator',
      fn_name: 'get_direct_messages',
      payload: alice.agentPubKey,
    });

    t.equal(bobMessages.length, 1, 'Bob received one message');
    t.equal(bobMessages[0].entry.content.text, 'Hello Bob!', 'Message content matches');
  });
});
```

### Frontend Component Test

```typescript
// ui/src/tests/unit/MessageComponent.test.ts
import { render, screen } from '@testing-library/svelte';
import { vi } from 'vitest';
import MessageComponent from '../../lib/components/MessageComponent.svelte';

test('MessageComponent displays message correctly', () => {
  const message = {
    content: { text: 'Hello World!' },
    sender: 'alice123',
    timestamp: { unix_time: Date.now(), timezone: 'UTC', precision: 'Second' },
  };

  render(MessageComponent, { message });

  expect(screen.getByText('Hello World!')).toBeInTheDocument();
  expect(screen.getByText('alice123')).toBeInTheDocument();
});
```

## Test Configuration Files

### Tryorama Test Configuration

```json
// tests/package.json
{
  "name": "memetic-messenger-tests",
  "scripts": {
    "test": "npm run test:zomes && npm run test:integration",
    "test:zomes": "tape 'src/zomes/**/*.test.ts'",
    "test:integration": "tape 'src/integration/**/*.test.ts'",
    "test:scenarios": "tape 'src/scenarios/**/*.test.ts'"
  },
  "devDependencies": {
    "@holochain/tryorama": "^0.19.0",
    "@types/tape": "^4.13.0",
    "tape": "^5.6.0",
    "typescript": "^5.0.0",
    "ts-node": "^10.9.0"
  }
}
```

### Frontend Test Configuration

```typescript
// ui/vitest.config.ts
import { sveltekit } from '@sveltejs/kit/vite';
import { defineConfig } from 'vitest/config';

export default defineConfig({
  plugins: [sveltekit()],
  test: {
    environment: 'jsdom',
    include: ['src/**/*.{test,spec}.{js,ts}'],
    setupFiles: ['src/tests/setup.ts'],
  },
});
```

## Testing Workflow

### 1. **Development Testing**
```bash
# Run individual zome tests
npm run test:clock
npm run test:communication

# Run integration tests
npm run test:integration

# Run all backend tests
cd tests && npm test

# Run frontend tests
cd ui && npm run test
```

### 2. **CI/CD Testing**
```bash
# Full test suite
npm run build:happ
npm run test:backend
npm run test:frontend
npm run test:e2e
```

### 3. **Manual Testing**
```bash
# Start development environment
npm run dev

# Start multi-agent scenario
npm run network
```

## Test Coverage Strategy

### **Must Test** (Critical Path)
- ✅ Entry creation and retrieval for all zomes
- ✅ Inter-zome function calls
- ✅ Multi-agent communication
- ✅ Data validation and error handling
- ✅ Permission and access control

### **Should Test** (Important Features)
- 🔄 Complex integration scenarios
- 🔄 Performance under load
- 🔄 Network partition recovery
- 🔄 UI component behavior

### **Nice to Test** (Edge Cases)
- 🎯 Stress testing with many agents
- 🎯 Long-running scenarios
- 🎯 Browser compatibility

---

**Testing Framework**: Tryorama + Tape for backend, Vitest + Testing Library for frontend