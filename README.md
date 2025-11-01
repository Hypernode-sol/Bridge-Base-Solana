# Bridge-Base-Solana

**Bridge-Base-Solana** is a core component of the **Hypernode Network**, designed to establish a seamless interoperability layer between the **Solana** and **Base** ecosystems.  
It enables secure, efficient, and decentralized communication of assets, messages, and AI-agent events between these two high-performance chains.

---

## 🌉 Overview

The **Bridge-Base-Solana** provides a bi-directional, trust-minimized connection between Solana (SPL-based assets) and Base (ERC-20 / EVM-compatible assets).  
It is designed to support both **token bridging** and **agent-level message relays**, allowing the **Hypernode AI Agents** and sub-networks to operate coherently across different chains.

This bridge is one of the foundational modules in **Phase 1** of the Hypernode ecosystem.

---

## 🧩 Core Features

- **Cross-chain Messaging** — Transfers structured data and messages between Solana and Base.
- **Token Bridging** — Supports wrapped and mirrored asset transfers (SPL ↔ ERC-20).
- **AI Event Relay** — Allows Hypernode Agents to trigger cross-chain actions via smart contracts.
- **Consensus-Aware Validation** — Verifies transaction proofs from both networks before finalization.
- **Integration with Hypernode-Deployer** — Enables automated contract deployment and monitoring.
- **Telemetry Hooks** — Real-time tracking of bridge operations through the Hypernode Telemetry Network.

---

## ⚙️ Architecture

```
+---------------------------------------------------------------+
|                        Hypernode Bridge                       |
+---------------------------------------------------------------+
|                                                               |
|   [Base Network]                [Bridge Layer]        [Solana Network]   |
|   ────────────────              ────────────────       ────────────────   |
|   • ERC-20 Contract    <---->   • Verifier & Relay <---->   • SPL Program |
|   • Event Emitter                • Oracles & Relayers        • Token Mint  |
|                                                               |
|       Base Node Agent     ←→     Hypernode Facilitator     ←→  Solana Node Agent  |
|                                                               |
+---------------------------------------------------------------+
```

The bridge uses **validator relays** running inside the **Hypernode Facilitator layer**, ensuring synchronized state verification across chains.

---

## 🧠 Integration with Hypernode AI Layer

The bridge supports **AI-triggered transactions** and **autonomous task execution** through the **Hypernode MCP (Multi-Chain Protocol)**.

Examples include:
- Automatic liquidity balancing between chains.
- Autonomous arbitrage or yield routing.
- AI agents monitoring token velocity and triggering transfers.
- Reward synchronization between Base and Solana staking modules.

---

## 🔗 Related Repositories

- [Hypernode-AI-Deployer](https://github.com/Hypernode-sol/Hypernode-AI-Deployer)  
  Automated model and agent deployment pipeline.

- [Hypernode-Facilitator](https://github.com/Hypernode-sol/Hypernode-Facilitator)  
  Manages coordination, consensus, and telemetry of bridge events.

- [Network-and-Communication-Infrastructure](https://github.com/Hypernode-sol/Network-and-Communication-Infrastructure)  
  Provides the base layer for network transport and message serialization.

---

## 🚀 Getting Started

### Prerequisites
- Node.js >= 18.x
- pnpm or yarn
- Solana CLI (`solana --version`)
- Base (Ethereum-compatible) wallet or node access
- Hypernode CLI (optional)

### Installation

```bash
git clone https://github.com/Hypernode-sol/Bridge-Base-Solana.git
cd Bridge-Base-Solana

# Install root dependencies
npm install

# Install Solana dependencies
cd solana && bun install

# Install Base dependencies (Foundry)
cd ../base && forge install
```

### Local Development

**Build all components:**

```bash
# From root directory
npm run build

# Or build individually
npm run build:solana    # Build Anchor program
npm run build:base      # Build Solidity contracts
npm run build:relayer   # Build relayer (coming soon)
```

**Run tests:**

```bash
# Test Solana program
cd solana && anchor test

# Test Base contracts
cd base && forge test

# Run with verbose output
forge test -vvv
```

**Deploy to testnets:**

```bash
# Deploy Solana program to devnet
cd solana
bun run program:deploy devnet-alpha

# Deploy Base contracts to Base Sepolia
cd base
make deploy

# Initialize the bridge
cd solana
bun run tx:initialize devnet-alpha
```

**Set up environment:**

```bash
# Copy environment template
cp .env.example .env

# Edit with your configuration
# - Add your private keys (NEVER commit these!)
# - Set RPC endpoints
# - Configure bridge addresses after deployment
```

---

## 🧾 Configuration

Set environment variables in a `.env` file:

```bash
BASE_RPC_URL="https://mainnet.base.org"
SOLANA_RPC_URL="https://api.mainnet-beta.solana.com"
BRIDGE_PRIVATE_KEY="your-private-key"
ORACLE_ENDPOINT="https://hypernode-telemetry.org/api"
```

---

## 🔍 Telemetry & Monitoring

Bridge performance, transfer rates, and cross-chain latencies are continuously monitored by the **Hypernode Telemetry Layer**.

You can visualize bridge metrics and flow on:
> https://hypernodesolana.org/app

---

## 🧱 Directory Structure

```
Bridge-Base-Solana/
│
├── base/                    # Solidity contracts for Base (EVM)
│   ├── src/                # Contract source files
│   │   ├── Bridge.sol      # Main bridge contract
│   │   ├── Twin.sol        # Execution contract
│   │   └── CrossChainERC20.sol  # Wrapped tokens
│   ├── script/             # Deployment scripts (Foundry)
│   ├── test/               # Contract tests
│   └── Makefile            # Build automation
│
├── solana/                  # Rust/Anchor program for Solana
│   ├── programs/
│   │   ├── bridge/         # Main bridge program
│   │   └── base_relayer/   # Relayer program
│   ├── scripts/            # Transaction scripts (Bun/TypeScript)
│   └── Anchor.toml         # Anchor configuration
│
├── scripts/                 # Cross-chain deployment scripts
│   └── src/internal/sol/   # Solana integration utilities
│
├── clients/                 # Client libraries
│   └── ts/                 # TypeScript client SDK
│
├── relayer/                 # Off-chain relayer (planned)
│
├── CONTRIBUTING.md          # Contribution guidelines
├── .env.example             # Environment variables template
└── README.md
```

---

## 🧪 Testing

### Unit Tests

**Test Base contracts (Foundry):**
```bash
cd base

# Run all tests
forge test

# Run specific test
forge test --match-test testBridgeFlow

# Run with verbose output
forge test -vvv

# Check test coverage
make coverage
```

**Test Solana program (Anchor):**
```bash
cd solana

# Run all tests
anchor test

# Run with logs
anchor test --skip-local-validator
```

### Integration Tests

Simulate a full bridge transaction flow:

```bash
cd solana

# Bridge SOL from Solana to Base
bun run tx:bridge-sol devnet-alpha

# Bridge SPL tokens from Solana to Base
bun run tx:bridge-spl devnet-alpha

# Wrap tokens
bun run tx:wrap-token devnet-alpha
```

### Manual Testing

1. Deploy contracts to testnets (see "Deploy to testnets" above)
2. Initialize the bridge on both chains
3. Send test transactions
4. Verify bridging in both directions
5. Check balances and events on both chains

---

## 🧰 Tech Stack

- **Solana Web3.js**
- **Ethers.js**
- **Anchor Framework**
- **TypeScript**
- **pnpm Workspaces**
- **Hypernode SDK**

---

## 🛡️ Security

The bridge implements:
- Double-signature verification (Base & Solana)
- Replay attack prevention
- Event proof verification
- Time-locks for asset release
- Automatic rollback protection via Hypernode Validator Agents

Audits are planned in **Phase 3** of the project roadmap.

---

## 🗺️ Roadmap

| Phase | Description | Status |
|:------|:-------------|:--------|
| 0 | Initial architecture draft | ✅ Completed |
| 1 | Cross-chain token bridge (SPL ↔ ERC-20) | 🚧 In progress |
| 2 | Agent-based message passing | ⏳ Planned |
| 3 | Validator incentivization and staking bridge | ⏳ Planned |
| 4 | Full MCP/x402/ACP integration | ⏳ Planned |

---

## 🤝 Contributing

We welcome contributions! Whether you're fixing bugs, adding features, or improving documentation, your help is appreciated.

### How to Contribute

1. **Fork the repository** on GitHub
2. **Clone your fork** locally
3. **Create a feature branch**: `git checkout -b feature/your-feature-name`
4. **Make your changes** following our code standards
5. **Test thoroughly** with both unit and integration tests
6. **Commit your changes** with clear, descriptive messages
7. **Push to your fork**: `git push origin feature/your-feature-name`
8. **Open a Pull Request** with a detailed description

### Guidelines

- All code must be in **English** (comments, docs, variables)
- Follow existing code style (use `forge fmt` for Solidity, `rustfmt` for Rust)
- Write tests for new features (aim for >80% coverage)
- Update documentation as needed
- Keep commits focused and atomic

See [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed guidelines, code standards, and development workflow.

### Need Help?

- 📝 Open a GitHub Discussion for questions
- 🐛 Report bugs via GitHub Issues
- 📧 Email: contact@hypernodesolana.org

---

## 📜 License

This project is released under the **MIT License**.

---

## 🧭 Part of the Hypernode Ecosystem

> Hypernode transforms idle compute power into a coordinated, incentivized, and intelligent infrastructure — connecting decentralized computation, AI agents, and multi-chain operations into a unified economic layer.

Explore more at  
🌐 [https://hypernodesolana.org](https://hypernodesolana.org)
