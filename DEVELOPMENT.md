# Development Guide

## Quick Start

### Prerequisites Installation

**macOS:**
```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install Stellar CLI
# Follow instructions at https://soroban.stellar.org/docs/getting-started/setup
```

**Linux:**
```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install Stellar CLI
# Follow instructions at https://soroban.stellar.org/docs/getting-started/setup
```

**Windows:**
```powershell
# Install Rust
# Download from https://rustup.rs/

# Install Stellar CLI
# Follow instructions at https://soroban.stellar.org/docs/getting-started/setup
```

### Verify Installation

```bash
rustc --version
cargo --version
stellar --version
```

### Project Setup

1. **Clone and navigate:**
   ```bash
   git clone https://github.com/Samuel1-ona/Hunty-contract.git
   cd Hunty-contract
   ```

2. **Build all contracts:**
   ```bash
   # Build hunty-core
   cd contracts/hunty-core
   make build
   
   # Build reward-manager
   cd ../reward-manager
   make build
   
   # Build nft-reward
   cd ../nft-reward
   make build
   ```

3. **Run tests:**
   ```bash
   # From each contract directory
   make test
   ```

## Development Workflow

### Working on a Feature

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make changes:**
   - Edit source files
   - Add tests
   - Update documentation

3. **Test your changes:**
   ```bash
   make test
   make build
   ```

4. **Format code:**
   ```bash
   make fmt
   ```

5. **Commit and push:**
   ```bash
   git add .
   git commit -m "feat: description of changes"
   git push origin feature/your-feature-name
   ```

### Running Tests

**Individual contract tests:**
```bash
cd contracts/hunty-core
cargo test
```

**All tests:**
```bash
cargo test --workspace
```

**With output:**
```bash
cargo test -- --nocapture
```

### Building Contracts

**Build a single contract:**
```bash
cd contracts/hunty-core
make build
```

**Build all contracts:**
```bash
# From project root
for dir in contracts/*/; do
  cd "$dir" && make build && cd ../..
done
```

**Check build output:**
```bash
ls -lh target/wasm32-unknown-unknown/release/*.wasm
```

## Code Organization

### HuntyCore Contract

**File Structure:**
- `lib.rs` - Main contract implementation
- `types.rs` - Data structures (Hunt, Clue, PlayerProgress)
- `storage.rs` - Storage access patterns
- `errors.rs` - Custom error types
- `test.rs` - Test suite

**Key Functions to Implement:**
- `create_hunt()` - Create new hunt
- `add_clue()` - Add clue to hunt
- `register_player()` - Register player for hunt
- `submit_answer()` - Submit and verify answer
- `complete_hunt()` - Mark hunt complete

### RewardManager Contract

**File Structure:**
- `lib.rs` - Main reward distribution logic
- `xlm_handler.rs` - XLM token handling
- `nft_handler.rs` - NFT coordination
- `test.rs` - Test suite

**Key Functions to Implement:**
- `distribute_rewards()` - Main distribution entry
- `handle_xlm_rewards()` - XLM transfer logic
- `handle_nft_rewards()` - NFT minting coordination

### NftReward Contract

**File Structure:**
- `lib.rs` - NFT contract implementation
- `test.rs` - Test suite

**Key Functions to Implement:**
- `mint_reward_nft()` - Mint NFT for reward
- `transfer_nft()` - Transfer NFT to player
- `get_nft_metadata()` - Retrieve NFT info

## Testing Guidelines

### Unit Tests

Test individual functions:
```rust
#[test]
fn test_create_hunt() {
    let env = Env::default();
    // Test implementation
}
```

### Integration Tests

Test cross-contract interactions:
```rust
#[test]
fn test_reward_distribution() {
    // Test HuntyCore -> RewardManager -> NftReward flow
}
```

### Test Coverage

Aim for >80% code coverage. Run:
```bash
cargo test --workspace -- --nocapture
```

## Debugging

### Common Issues

1. **Build errors:**
   - Check Rust version: `rustc --version`
   - Clean and rebuild: `make clean && make build`

2. **Test failures:**
   - Run with output: `cargo test -- --nocapture`
   - Check error messages carefully

3. **Storage issues:**
   - Verify storage keys are unique
   - Check data serialization

### Debug Tools

**Print debugging:**
```rust
env.logs().add("Debug message", &value);
```

**Check storage:**
```rust
// In tests
let stored_value = env.storage().get(&key);
```

## Code Style

### Formatting

Always format before committing:
```bash
make fmt
# or
cargo fmt --all
```

### Naming Conventions

- Functions: `snake_case`
- Types: `PascalCase`
- Constants: `UPPER_SNAKE_CASE`
- Storage keys: `snake_case`

### Documentation

Add doc comments:
```rust
/// Creates a new hunt with the given parameters.
/// 
/// # Arguments
/// * `env` - The environment
/// * `creator` - Address of the hunt creator
/// 
/// # Returns
/// Hunt ID
pub fn create_hunt(env: Env, creator: Address) -> u64 {
    // Implementation
}
```

## Deployment

### Local Testing

```bash
# Deploy to local network
stellar contract deploy --wasm target/wasm32-unknown-unknown/release/hunty_core.wasm
```

### Testnet Deployment

```bash
# Deploy to testnet
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/hunty_core.wasm \
  --network testnet
```

## Resources

- [Soroban Documentation](https://soroban.stellar.org/docs)
- [Stellar SDK Reference](https://docs.rs/soroban-sdk/)
- [Rust Book](https://doc.rust-lang.org/book/)

## Getting Help

- Check [ARCHITECTURE.md](ARCHITECTURE.md) for system design
- Review [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines
- Open an issue on GitHub for questions


