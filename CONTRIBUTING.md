# Contributing to Bridge-Base-Solana

Thank you for your interest in contributing to the Hypernode Bridge! This guide will help you get started with development.

---

## 🚀 Quick Start

### Prerequisites

**Required:**
- Node.js >= 18.x
- Rust 1.77+ (stable)
- Solana CLI 1.18+
- Anchor 0.30+
- Foundry (forge, cast, anvil)
- Git

**Optional:**
- Bun (faster alternative to npm)

### Initial Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Hypernode-sol/Bridge-Base-Solana.git
   cd Bridge-Base-Solana
   ```

2. **Install dependencies**
   ```bash
   # Root workspace
   npm install

   # Solana program
   cd solana && bun install

   # Base contracts
   cd ../base && forge install
   ```

3. **Set up environment variables**
   ```bash
   # Copy example files
   cp .env.example .env
   cp base/.env.example base/.env
   ```

4. **Build all components**
   ```bash
   # From root
   npm run build

   # Or individually
   npm run build:solana
   npm run build:base
   npm run build:relayer
   ```

---

## 🏗️ Project Structure

```
Bridge-Base-Solana/
│
├── base/                    # Solidity contracts for Base (EVM)
│   ├── src/                # Contract source files
│   │   ├── Bridge.sol      # Main bridge contract
│   │   ├── Twin.sol        # Execution contract
│   │   └── CrossChainERC20.sol
│   ├── script/             # Deployment scripts
│   ├── test/               # Foundry tests
│   └── Makefile            # Build automation
│
├── solana/                  # Rust/Anchor program for Solana
│   ├── programs/
│   │   ├── bridge/         # Main bridge program
│   │   └── base_relayer/   # Relayer program
│   ├── scripts/            # Transaction scripts (Bun)
│   └── Anchor.toml         # Anchor configuration
│
├── scripts/                 # Deployment and utility scripts
│   └── src/
│       └── internal/sol/   # Solana integration scripts
│
├── clients/                 # Client libraries
│   └── ts/                 # TypeScript client
│
└── relayer/                 # Off-chain relayer (planned)
```

---

## 🔨 Development Workflow

### Working with Base Contracts (Solidity)

```bash
cd base

# Build contracts
forge build

# Run tests
forge test

# Run specific test
forge test --match-test testBridgeFlow

# Test with verbosity
forge test -vvv

# Check coverage
make coverage

# Format code
forge fmt

# Deploy to testnet
make deploy
```

### Working with Solana Program (Rust/Anchor)

```bash
cd solana

# Build program for devnet
bun run program:build devnet-alpha

# Run tests
anchor test

# Deploy program
bun run program:deploy devnet-alpha

# Generate IDL
bun run generate:idl devnet-alpha

# Initialize bridge
bun run tx:initialize devnet-alpha

# Test bridging operations
bun run tx:bridge-sol devnet-alpha
bun run tx:bridge-spl devnet-alpha
```

### Running the Relayer (Coming Soon)

```bash
cd relayer
npm install
npm start
```

---

## 🧪 Testing

### Unit Tests

**Base (Solidity):**
```bash
cd base
forge test
```

**Solana (Rust):**
```bash
cd solana
anchor test
```

### Integration Tests

Run integration tests from the `solana/scripts/` directory:

```bash
cd solana

# Test full bridge flow
bun run tx:bridge-sol devnet-alpha
bun run tx:bridge-spl devnet-alpha
```

### Manual Testing

1. **Deploy contracts to testnets**
   ```bash
   # Deploy Base contracts (Base Sepolia)
   cd base && make deploy

   # Deploy Solana program (Devnet)
   cd solana && bun run program:deploy devnet-alpha
   ```

2. **Initialize the bridge**
   ```bash
   cd solana
   bun run tx:initialize devnet-alpha
   ```

3. **Test token bridging**
   - Send tokens from Solana to Base
   - Verify tokens arrived on Base
   - Bridge back from Base to Solana

---

## 📝 Code Standards

### General Guidelines

1. **All code must be in English**
   - Comments, documentation, variable names, commit messages

2. **Follow existing code style**
   - Solidity: Use forge fmt
   - Rust: Use rustfmt
   - TypeScript: Follow project .prettierrc

3. **Write tests for new features**
   - Aim for >80% code coverage
   - Include both unit and integration tests

4. **Document public APIs**
   - Add NatSpec comments for Solidity functions
   - Add rustdoc comments for Rust functions

### Solidity Style (Base Contracts)

```solidity
// GOOD: Clear, documented, secure
/// @notice Bridges tokens from Base to Solana
/// @param amount The amount of tokens to bridge
/// @param recipient The Solana recipient address
function bridgeToSolana(uint256 amount, bytes32 recipient) external {
    require(amount > 0, "Amount must be positive");
    require(balanceOf(msg.sender) >= amount, "Insufficient balance");

    _burn(msg.sender, amount);
    emit TokensBridged(msg.sender, recipient, amount);
}

// BAD: Unclear, no checks
function bridge(uint256 a, bytes32 r) external {
    _burn(msg.sender, a);
}
```

### Rust Style (Solana Program)

```rust
// GOOD: Clear, safe, documented
/// Bridges SPL tokens from Solana to Base
pub fn bridge_spl_to_base(
    ctx: Context<BridgeSPL>,
    amount: u64,
    base_recipient: [u8; 20],
) -> Result<()> {
    require!(amount > 0, BridgeError::InvalidAmount);

    // Transfer tokens to vault
    token::transfer(
        ctx.accounts.transfer_context(),
        amount,
    )?;

    Ok(())
}

// BAD: Unclear, no validation
pub fn bridge(ctx: Context<B>, a: u64, r: [u8; 20]) -> Result<()> {
    token::transfer(ctx.accounts.t(), a)?;
    Ok(())
}
```

---

## 🔐 Security Guidelines

### Before Submitting Code

- [ ] No private keys or secrets in code
- [ ] All external calls are checked
- [ ] Integer overflow/underflow handled
- [ ] Reentrancy protected (Solidity)
- [ ] Account validation complete (Solana)
- [ ] Error messages are descriptive
- [ ] Gas optimization considered (but not premature)

### Security Best Practices

1. **Trust Minimization**
   - Minimize trusted parties
   - Use cryptographic verification
   - Avoid admin-only functions where possible

2. **Input Validation**
   - Validate all user inputs
   - Check account ownership (Solana)
   - Verify signatures and proofs

3. **Error Handling**
   - Use require/assert appropriately
   - Return descriptive error messages
   - Handle all failure cases

4. **Testing**
   - Test edge cases
   - Test with malicious inputs
   - Fuzz test critical functions

---

## 📤 Submission Process

### 1. Create a Feature Branch

```bash
git checkout -b feature/your-feature-name
```

### 2. Make Your Changes

- Write code following the style guide
- Add tests for new functionality
- Update documentation as needed

### 3. Test Thoroughly

```bash
# Run all tests
npm run test:solana
npm run test:base

# Check formatting
cd base && forge fmt --check
cd solana && cargo fmt --check
```

### 4. Commit Your Changes

Use clear, descriptive commit messages:

```bash
# GOOD
git commit -m "feat: add SPL token bridging to Base

Implements bridge_spl_to_base instruction that locks SPL tokens
in vault and emits message for Base-side minting.

Includes unit tests and integration test script."

# BAD
git commit -m "added feature"
```

### 5. Push and Create PR

```bash
git push origin feature/your-feature-name
```

Then create a Pull Request on GitHub with:
- Clear description of changes
- Reference to related issues
- Test results
- Screenshots/videos if UI-related

---

## 🐛 Bug Reports

When reporting bugs, please include:

1. **Description**: What is the bug?
2. **Steps to Reproduce**: How can we recreate it?
3. **Expected Behavior**: What should happen?
4. **Actual Behavior**: What actually happens?
5. **Environment**: OS, Node version, Rust version, etc.
6. **Logs**: Relevant error messages or stack traces

---

## 💡 Feature Requests

We welcome feature suggestions! Please:

1. **Check existing issues** to avoid duplicates
2. **Describe the use case** clearly
3. **Explain the benefits** to users
4. **Consider implementation** complexity
5. **Discuss in Discord** before major changes

---

## 📚 Additional Resources

- [Solana Documentation](https://docs.solana.com)
- [Anchor Framework](https://www.anchor-lang.com)
- [Foundry Book](https://book.getfoundry.sh)
- [Base Documentation](https://docs.base.org)
- [Hypernode Main Docs](https://github.com/Hypernode-sol/Docs)

---

## 🤝 Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help others learn and grow
- Follow project guidelines
- Report violations to contact@hypernodesolana.org

---

## 📞 Getting Help

- **Discord**: Join our community for real-time help
- **GitHub Issues**: For bug reports and feature requests
- **GitHub Discussions**: For questions and general discussion
- **Email**: contact@hypernodesolana.org

---

Thank you for contributing to Hypernode Bridge! 🚀
