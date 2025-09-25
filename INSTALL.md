# Installation Guide

## Prerequisites

You already have:
- ✅ Rust 1.87.0
- ✅ Node.js v23.11.0
- ✅ npm 10.9.2

## Holochain Development Environment

### Option 1: Cargo Installation (Currently Running)
The Holochain CLI is being installed via cargo in the background. This may take 10-15 minutes due to compilation.

### Option 2: Nix Installation (Recommended by Holochain)
Run this command **interactively** (requires sudo password):
```bash
bash <(curl https://holochain.github.io/holochain/setup.sh)
```

### Option 3: Manual Installation
If both above fail, you can install individual components:

```bash
# Install Holochain CLI
cargo install holochain_cli

# Install Holochain conductor
cargo install holochain

# Install scaffolding tool (check current docs for exact name)
```

## Verification

Once installed, verify with:
```bash
# Check Holochain CLI
holochain --version

# Check scaffolding tool
hc-scaffold --version
```

## Next Steps

1. ✅ Environment setup (in progress)
2. Find weave.social template examples
3. Create basic project structure
4. Build Clock zome skeleton

## Troubleshooting

If installations fail:
1. Check system requirements (8GB+ RAM, 4+ CPU cores)
2. Ensure stable internet connection
3. Try running commands individually
4. Check Holochain Discord for help: https://discord.gg/holochain

---

*Run this installation interactively in your terminal for best results*