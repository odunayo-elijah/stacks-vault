# StacksVault: Bitcoin-Secured NFT Finance Protocol

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Stacks](https://img.shields.io/badge/Stacks-Clarity-orange.svg)
![Version](https://img.shields.io/badge/version-1.0.0-green.svg)

## 🏗️ Overview

**StacksVault** is a next-generation digital asset management platform that combines Bitcoin's immutable security with Stacks' programmability, enabling institutional-grade NFT operations with integrated DeFi capabilities. Built entirely in Clarity smart contracts, StacksVault provides a comprehensive suite for NFT minting, trading, staking, and fractional ownership.

### 🚀 Core Innovations

- **Bitcoin-Secured Settlements**: Leverages Bitcoin's finality for ultimate security
- **Collateralized Minting System**: Ensures asset-backed value through mandatory over-collateralization
- **Automated Yield Generation**: Time-locked staking mechanisms with 8% APY
- **Fractional Ownership**: Enables liquidity for high-value assets through share tokenization
- **Protocol-Owned Liquidity**: Market stability through treasury-backed operations
- **Emergency Circuit Breaker**: Admin controls for protocol safety

## 🏛️ Architecture

### Smart Contract Design

- **Non-Custodial**: Users maintain full sovereignty over their assets
- **Audit-Ready**: Clean, well-documented code with comprehensive error handling
- **DAO-Compatible**: Governance-ready infrastructure for community control
- **Mathematically Precise**: Basis point calculations for accurate fee distribution

### Security Features

- ✅ Over-collateralization requirements (150% minimum)
- ✅ Input validation on all public functions
- ✅ Arithmetic overflow protection
- ✅ Emergency pause functionality
- ✅ Role-based access control

## 📋 Prerequisites

- [Clarinet](https://docs.hiro.so/clarinet) v1.7.0+
- [Node.js](https://nodejs.org/) v16.0.0+
- [Stacks CLI](https://docs.hiro.so/stacks-cli) (optional, for mainnet deployment)

## 🛠️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/odunayo-elijah/stacks-vault.git
cd stacks-vault
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Run Tests

```bash
npm test
# or
clarinet test
```

### 4. Check Contract Syntax

```bash
clarinet check
```

## 📖 Contract Interface

### Core NFT Operations

#### `mint-asset`

Mint a new NFT with mandatory collateral backing.

```clarity
(mint-asset (metadata-uri (string-ascii 256)) (collateral-amount uint))
```

**Parameters:**

- `metadata-uri`: IPFS or HTTP URI for asset metadata (1-256 characters)
- `collateral-amount`: Base collateral amount (will be multiplied by collateral ratio)

**Returns:** `(response uint uint)` - New token ID on success

**Example:**

```clarity
(contract-call? .stacks-vault mint-asset "ipfs://QmYourHash" u1000000)
;; Requires 1.5M µSTX collateral (150% of 1M base amount)
```

#### `transfer-asset`

Transfer NFT ownership (only if not staked).

```clarity
(transfer-asset (token-id uint) (new-owner principal))
```

### Marketplace Functions

#### `create-listing`

List an NFT for sale on the marketplace.

```clarity
(create-listing (token-id uint) (sale-price uint))
```

#### `execute-purchase`

Purchase a listed NFT (includes 2.5% protocol fee).

```clarity
(execute-purchase (token-id uint))
```

### Staking & Yield Generation

#### `stake-for-yield`

Stake an NFT to earn 8% annual yield.

```clarity
(stake-for-yield (token-id uint))
```

#### `unstake-asset`

Unstake NFT and claim all pending rewards.

```clarity
(unstake-asset (token-id uint))
```

### Fractional Ownership

#### `transfer-fractional-shares`

Transfer fractional ownership shares between users.

```clarity
(transfer-fractional-shares (token-id uint) (recipient principal) (share-amount uint))
```

### Read-Only Functions

#### `get-asset-details`

Retrieve complete asset information.

```clarity
(get-asset-details (token-id uint))
```

#### `calculate-pending-rewards`

Calculate current staking rewards for a token.

```clarity
(calculate-pending-rewards (token-id uint))
```

#### `get-protocol-stats`

Get overall protocol statistics.

```clarity
(get-protocol-stats)
```

## 💰 Economic Model

### Fee Structure

- **Marketplace Fee**: 2.5% on all sales (250 basis points)
- **Collateral Requirement**: 150% over-collateralization
- **Staking Yield**: 8% annual percentage yield (APY)

### Collateral Mechanics

When minting an NFT with base value of 1,000,000 µSTX:

- Required collateral: 1,500,000 µSTX (150%)
- Collateral remains locked while NFT exists
- Returned upon asset burning/destruction

### Yield Calculation

```
Annual Yield Rate: 8% (800 basis points)
Stacks Blocks per Year: ~52,560
Reward per Block = (Collateral × 800) / (52,560 × 10,000)
```

## 🔧 Configuration Parameters

| Parameter | Default Value | Description |
|-----------|---------------|-------------|
| `minimum-collateral-ratio` | 150% | Over-collateralization requirement |
| `protocol-fee-rate` | 2.5% | Marketplace transaction fee |
| `annual-yield-rate` | 8% | Staking rewards APY |
| `emergency-pause` | false | Protocol circuit breaker |

## 🧪 Testing

### Run Full Test Suite

```bash
npm test
```

### Run Specific Tests

```bash
clarinet test --filter mint-asset
clarinet test --filter marketplace
clarinet test --filter staking
```

### Test Coverage Areas

- ✅ Asset minting with collateral validation
- ✅ Ownership transfers and access controls
- ✅ Marketplace listing and purchasing flows
- ✅ Staking rewards calculation and distribution
- ✅ Fractional ownership transfers
- ✅ Administrative functions and emergency controls
- ✅ Edge cases and error conditions

## 🚀 Deployment

### Testnet Deployment

```bash
clarinet deploy --testnet
```

### Mainnet Deployment

```bash
clarinet deploy --mainnet
```

### Post-Deployment Verification

1. Verify contract deployment on [Stacks Explorer](https://explorer.stacks.co/)
2. Test core functions with small amounts
3. Monitor protocol statistics via read-only functions

## 🔐 Security Considerations

### Audited Components

- ✅ Arithmetic operations (overflow protection)
- ✅ Access control mechanisms
- ✅ State transition validation
- ✅ Emergency pause functionality

### Best Practices Implemented

- **Input Validation**: All public functions validate parameters
- **Safe Math**: Protected arithmetic with overflow checks
- **Access Control**: Owner-only functions for critical operations
- **State Consistency**: Atomic operations prevent inconsistent states

### Recommended Security Practices

1. **Start Small**: Begin with minimal collateral amounts
2. **Monitor Treasury**: Regularly check protocol treasury balance
3. **Emergency Procedures**: Understand pause/unpause mechanisms
4. **Regular Audits**: Periodic security assessments recommended

## 📊 Error Codes Reference

| Code | Error | Description |
|------|-------|-------------|
| u100 | ERR-UNAUTHORIZED | Insufficient permissions for operation |
| u101 | ERR-INSUFFICIENT-FUNDS | Insufficient STX balance |
| u102 | ERR-INVALID-TOKEN | Token ID doesn't exist or invalid |
| u103 | ERR-INVALID-PRICE | Price must be greater than zero |
| u104 | ERR-LISTING-INACTIVE | Marketplace listing is not active |
| u105 | ERR-INSUFFICIENT-COLLATERAL | Below minimum collateral requirement |
| u106 | ERR-ALREADY-STAKED | Token is currently staked |
| u107 | ERR-NOT-STAKED | Token is not currently staked |
| u108 | ERR-INVALID-RECIPIENT | Invalid recipient address |
| u109 | ERR-ARITHMETIC-OVERFLOW | Mathematical operation overflow |
| u110 | ERR-INVALID-SHARES | Invalid share amount for fractional transfer |

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. **Fork the Repository**
2. **Create Feature Branch**: `git checkout -b feature/amazing-feature`
3. **Write Tests**: Ensure new features have comprehensive tests
4. **Run Test Suite**: `npm test` must pass
5. **Submit Pull Request**: With detailed description of changes

### Development Guidelines

- Follow Clarity best practices and conventions
- Add comprehensive documentation for new functions
- Include both positive and negative test cases
- Update README for any interface changes

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Links & Resources

- **Documentation**: [Stacks Documentation](https://docs.stacks.co/)
- **Clarity Language**: [Clarity Reference](https://docs.stacks.co/clarity)
- **Testnet Faucet**: [Stacks Testnet Faucet](https://explorer.stacks.co/sandbox/faucet)
- **Block Explorer**: [Stacks Explorer](https://explorer.stacks.co/)
