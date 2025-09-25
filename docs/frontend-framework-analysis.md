# Frontend Framework Analysis for Memetic Messenger

## Existing Weave.social Tool Frameworks

From my research of existing tools:

1. **KanDo**: Svelte (64.7% of codebase)
2. **Notebooks**: TypeScript (97.8% of codebase) - likely React or vanilla TypeScript
3. **Converge**: Svelte (mentioned in weave docs)
4. **Presence**: Lit Framework (mentioned in weave docs)
5. **Acorn**: React (mentioned in weave docs)

## Framework Options Analysis

### 1. **Svelte** ✅ Recommended
**Pros:**
- Most popular in weave.social ecosystem (KanDo, Converge)
- Excellent performance (compiles to vanilla JS)
- Small bundle size - perfect for PWAs
- Great TypeScript support
- Simple, clean syntax
- Good Holochain community adoption

**Cons:**
- Smaller ecosystem than React
- Less familiar to some developers

### 2. **React**
**Pros:**
- Largest ecosystem and community
- Many Holochain examples available
- Excellent tooling and libraries
- Used by Acorn weave tool

**Cons:**
- Larger bundle size
- More complex setup
- Runtime overhead

### 3. **Lit**
**Pros:**
- Web standards based
- Small footprint
- Used by Presence weave tool
- Good for web components

**Cons:**
- Less popular in Holochain ecosystem
- Learning curve for web components

### 4. **TypeScript + Vanilla**
**Pros:**
- Full control
- Minimal dependencies
- Fast loading

**Cons:**
- More development time
- Need to build everything from scratch

## Decision: **Svelte + TypeScript**

**Reasoning:**
1. **Ecosystem alignment**: Most successful weave.social tools use Svelte
2. **Performance**: Critical for PWA on Holoports
3. **Bundle size**: Essential for good UX
4. **Development speed**: Svelte's simplicity will accelerate development
5. **TypeScript**: Provides type safety for Holochain integration

## Project Structure Template

Based on weave.social patterns + Svelte best practices:

```
memetic-messenger/
├── dnas/
│   └── memetic_messenger/
│       ├── zomes/
│       │   ├── integrity/
│       │   ├── clock_coordinator/
│       │   ├── calendar_coordinator/
│       │   ├── identity_coordinator/
│       │   ├── address_book_coordinator/
│       │   ├── membrane_manager_coordinator/
│       │   ├── communication_coordinator/
│       │   ├── frames_coordinator/
│       │   └── dashboard_coordinator/
│       └── workdir/
├── ui/                        # Svelte frontend
│   ├── src/
│   │   ├── lib/
│   │   │   ├── components/     # Reusable components
│   │   │   ├── stores/         # Svelte stores for state
│   │   │   ├── holochain/      # Holochain client integration
│   │   │   └── types/          # TypeScript type definitions
│   │   ├── routes/             # SvelteKit routing
│   │   ├── app.html
│   │   └── app.ts
│   ├── static/
│   ├── tests/
│   ├── package.json
│   ├── svelte.config.js
│   ├── tsconfig.json
│   └── vite.config.ts
├── applet/                    # Weave integration
│   ├── src/
│   └── package.json
├── we_dev/                    # Weave dev config
├── tests/                     # Backend tests
├── package.json               # Root workspace
├── Cargo.toml                 # Rust workspace
└── flake.nix                  # Nix environment
```

## Key Dependencies

**Frontend (ui/package.json):**
```json
{
  "devDependencies": {
    "@sveltejs/adapter-static": "^3.0.0",
    "@sveltejs/kit": "^2.0.0",
    "@sveltejs/vite-plugin-svelte": "^3.0.0",
    "svelte": "^4.0.0",
    "typescript": "^5.0.0",
    "vite": "^5.0.0",
    "@holochain/client": "^0.16.0",
    "@msgpack/msgpack": "^3.0.0"
  },
  "type": "module"
}
```

**Weave Integration (applet/package.json):**
```json
{
  "devDependencies": {
    "@theweave/cli": "latest",
    "@holochain/hc-spin": "latest"
  }
}
```

## Development Workflow

1. **Holochain Backend**: Rust zomes
2. **Frontend**: Svelte + TypeScript + SvelteKit
3. **Integration**: @holochain/client for zome calls
4. **Building**: Vite for fast development and building
5. **Packaging**: Create .webhapp for Holochain Launcher
6. **Weave**: Integrate with weave agent system

## Next Steps

1. Set up SvelteKit project structure
2. Configure TypeScript and Vite
3. Set up Holochain client integration
4. Create basic component structure for 8 applets
5. Implement routing for different applets

---

**Decision**: Svelte + TypeScript for optimal performance and ecosystem alignment.