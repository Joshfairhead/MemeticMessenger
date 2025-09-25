# Weave.social Integration Patterns Analysis

## Project Structure Pattern

Based on examination of Notebooks and KanDo tools, weave.social tools follow this structure:

```
project/
├── dnas/
│   └── [app-name]/
│       ├── zomes/
│       │   ├── coordinator/    # Business logic zome
│       │   └── integrity/      # Data validation zome
│       └── workdir/           # Build artifacts
├── ui/                        # Frontend code
├── applet/                    # Weave-specific integration
├── we_dev/                    # Weave development configs
├── package.json               # NPM workspace config
├── Cargo.toml                 # Rust workspace config
└── flake.nix                  # Nix environment config
```

## Key Dependencies & Integration

**Package.json patterns:**
```json
{
  "scripts": {
    "dev": "concurrently \"npm:ui\" \"npm:agents\"",
    "tool-dev": "npm run start:ui & npm run agents",
    "network": "hc s run --piped -n 2 workdir/[app].happ",
    "package": "npm run build:happ && hc web-app create --happ workdir/[app].happ"
  },
  "devDependencies": {
    "@theweave/cli": "latest",
    "@holochain/hc-spin": "latest",
    "concurrently": "latest"
  }
}
```

**Weave Integration:**
- Uses `@theweave/cli` for agent management
- Supports multi-agent development scenarios
- Bootstrap and signal ports configuration
- WASM zome compilation

## DNA Architecture

**Standard Holochain Pattern:**
- **Integrity Zome**: Defines entry types, validation rules, link types
- **Coordinator Zome**: Contains the API functions, business logic

**For Memetic Messenger:**
- Each applet (Clock, Calendar, Identity, etc.) = separate coordinator zome
- Shared integrity zome for common entry types
- OR separate integrity zome per applet for modularity

## Development Workflow

1. **Environment**: Nix shell + Holochain CLI
2. **Build**: `npm run build:happ` creates `.happ` file
3. **Test**: Uses @holochain/tryorama for backend tests
4. **Package**: Creates `.webhapp` for Holochain Launcher
5. **Deploy**: Integrates with Weave agent system

## Next Steps for Implementation

✅ **Understood**: Project structure and integration patterns
🔄 **Next**: Define entry type schemas based on this structure