# Technical Specifications

## Overview

This document defines the technical implementation details for the Memetic Messenger platform, building upon the design framework outlined in [design-specifications.md](./design-specifications.md).

## Architecture Questions

To build this platform in production, we need to clarify several technical implementation details:

### 1. Data Layer ✓
- **Decision**: Holochain DHT/DAG - content addressable, local-first distributed architecture
- **Implementation**: Built on weave.social Holochain framework
- **Benefits**:
  - Natural DAG structure for event history
  - Content addressing for data integrity
  - Local-first with P2P synchronization
  - No central servers or databases needed

### 2. Communication Protocol ✓
- **Decision**: Holochain zome-based applets with shared DHT space
- **Implementation**:
  - Each applet is a zome (or smaller module)
  - All operate within weave.social DHT space
  - Inter-zome function calls for direct communication
  - Pub/sub event system for reactive updates
- **Architecture**: Applets are perspectives on shared data:
  - **Clock**: Time data perspective
  - **Calendar**: Timeline perspective
  - **Identity**: Personal timeline + profile perspective
  - **Address Book**: Social graph perspective from identity viewpoint
  - **Membrane Manager**: Groupoid perspective on social graph
  - **Files/Communication/Dashboard**: Various data perspectives

### 3. Identity & Authentication ✓
- **Decision**: Multi-layered identity through Holochain + AD4M framework
- **Identity Hierarchy**:
  1. **DeepKey**: Foundational human identity (persistent across all systems)
  2. **Agent Keys**: Cryptographic identities derived from DeepKey
  3. **Personas**: Agent-side data/expressions (stored on source chain)
  4. **Profiles**: App-side presentation fields (reconstructable across apps)
- **Implementation**:
  - DeepKey provides consistent identity foundation
  - Agent keys access AD4M framework for expressions/languages/perspectives
  - Personas enable different "voices" (personal, professional, etc.)
  - Profiles are app-specific presentations of persona data
- **Benefits**: Identity remains with progenitor (Josh) while allowing flexible agent expressions
- **Scope**: Pure Holochain for simplicity (identity bridging via AD4M is future consideration)

### 4. Networking Architecture ✓
- **Decision**: Pure Holochain DHT networking
- **Implementation**:
  - Standard Holochain conductor and DHT protocols
  - No additional infrastructure layers needed
  - Entry through any weave.social DNA or ecosystem app
- **Bootstrap**: Users can join through any existing entry point in the ecosystem
- **Scope**: Desktop/web focus initially (mobile connectivity out of scope)

### 5. Data Synchronization ✓
- **Decision**: Holochain's eventual consistency model with agent-relative time
- **Implementation**:
  - Event sequencing based on agent perspective (time is relative to agent)
  - Cross-device sync via DeepKey → Agent Key relationship
  - Shared source chain access for multiple devices under same identity
  - CRDT-like behavior for concurrent modifications
- **Conflict Resolution**:
  - Rely on Holochain's CRDT mechanisms
  - Consider single-writer constraints if conflicts become problematic
- **Data Integrity**: Handled by Holochain DHT with local islands/hotspots that can resync
- **Scope**: Holochain's eventual consistency handles synchronization needs

### 6. Platform & Deployment ✓
- **Decision**: Progressive Web App on Holoports with WASM compilation
- **Frontend**:
  - Use Holochain's built-in UI framework if available, otherwise consider Yew (Rust → WASM)
  - Progressive Web App for cross-platform compatibility
- **Deployment**:
  - Runs on Holoports and served over web
  - No desktop/mobile installation required
- **Development Strategy**:
  - Build incrementally based on dependencies (Clock → Calendar → Timeline, etc.)
  - All applets likely needed for proper messaging (to replace OS-level contacts, etc.)
- **Integration**: Leverage existing weave.social functionality where it suits our needs

## Implementation Priority

Based on dependencies and core messaging needs:
1. **Clock** (timestamps)
2. **Timeline/Calendar** (extends Clock naturally)
3. **Identity** (required for all communication)
4. **Social Graph/Address Book** (contact management)
5. **Communication** (core messaging)
6. **Membrane Manager** (contextual boundaries)
7. **Files** (media/attachments)
8. **Dashboard** (unified view)

---

*This document will be expanded based on architectural decisions and implementation choices.*