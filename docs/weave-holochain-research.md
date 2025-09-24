# Weave.social & Holochain Research

## Overview

Research findings from weave.social and Holochain documentation to inform Memetic Messenger implementation.

## Weave.social Architecture

### Core Concepts

**Social DNAs & Cells**
- **Social DNAs**: Templates for social interactions and communication patterns
- **Cells**: Instances of Social DNA templates representing specific social agreements/contexts
- Perfect for our Individual/Group/Public communication patterns

**Key Components**
- **Membranes**: Define group membership (maps to our Membrane Manager applet)
- **Capacities**: Functional tools like chat, document editing (our applets)
- **Sources**: Templates and starting points for tools

**Interaction Principles**
- Distributed context where agents interact directly
- No intermediary enforcing rules
- Full data access for contributing agents
- Aligns with our decentralized communication principles

### Weave Interaction Pattern (WIP)

Enables composability through:
- Interoperable tools across different contexts
- Asset linking across different capacities
- Standardized data creation, viewing, and interaction

### Implementation Approach for Memetic Messenger

1. Create Holochain app with weave dependencies
2. Implement Social DNAs for our communication contexts:
   - Individual messaging Social DNA
   - Group communication Social DNA
   - Public broadcasting Social DNA
3. Define asset types, rendering, and interaction rules
4. Build capacities (our applets) that work across these Social DNAs

## Holochain Development

### Architecture Overview

- **Framework for distributed P2P apps**
- **Zome-based modular architecture** (perfect for our applets)
- **Data at the edges** - resilient and serverless
- **Source chains** for personal data + **DHT** for shared data

### Development Structure

**Zomes (Our Applets)**
- Clock zome: Timestamp and scheduling functions
- Calendar zome: Event planning and temporal organization
- Identity zome: Personal identity and presence management
- Address Book zome: Contact management
- Membrane Manager zome: Contextual boundaries management
- Frames zome: Document and media storage
- Communication zome: Messaging and conversation interfaces
- Dashboard zome: Unified timeline and system overview

**Data Handling**
- **Source chains**: Personal data (drafts, preferences, personas)
- **DHT**: Shared data (messages, profiles, social graph)
- **Validation mechanisms**: Data integrity across the network
- **Entry types & links**: Structured data relationships

**Inter-Zome Communication**
- Direct function calls between zomes
- Optional access control
- No HTTP clients needed - native zome-to-zome calls

## Implementation Roadmap

### Phase 1: Foundation
1. Set up Holochain development environment
2. Research existing weave.social Social DNA templates
3. Define core entry types and validation rules

### Phase 2: Core Zomes
1. **Clock zome**: Basic timestamp functionality
2. **Identity zome**: DeepKey integration and persona management
3. **Communication zome**: Basic messaging with Social DNA integration

### Phase 3: Extended Functionality
1. **Calendar/Timeline zomes**: Temporal organization
2. **Address Book/Membrane Manager**: Social graph management
3. **Frames zome**: Media and document handling
4. **Dashboard zome**: Unified interface

### Phase 4: Integration & Polish
1. Frontend integration with Holochain conductor
2. Weave Interaction Pattern compliance
3. Cross-applet composability
4. Progressive Web App optimization

## Key Resources

- [Weave.social Developer Documentation](https://dev.theweave.social/)
- [Holochain Developer Portal](https://developer.holochain.org/)
- [Holochain Discord Community](https://discord.gg/holochain)
- [Weave.social GitHub](https://github.com/lightningrodlabs/we)

## Next Steps

To reach 100% implementation readiness, we need:

1. **Environment Setup**: Install Holochain dev tools and weave dependencies
2. **Template Analysis**: Examine existing weave Social DNA templates
3. **Entry Type Design**: Define specific data structures for our use cases
4. **Zome Skeleton Creation**: Basic zome structure for each applet
5. **Frontend Architecture**: UI framework integration planning

---

*Research conducted: 2025-09-24*