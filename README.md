[README.md](https://github.com/user-attachments/files/25926504/README.md)
# ⚡ Streak Protocol — Bitcoin DeFi Super-app

> **OP_NET Vibecoding Challenge — Week 3: The Breakthrough**  
> Proving Bitcoin Layer 1 can handle an advanced DeFi ecosystem.

---

## What is Streak Protocol?

Streak Protocol is an all-in-one Web3 super-app built on **Bitcoin L1 via OP_NET** featuring:

| Module | Contract | Description |
|--------|----------|-------------|
| 🖼 NFT Marketplace | `Streak_Market` | Trustless OP-721 artifact exchange with floor price tracking |
| 🏦 Lending Platform | `Streak_Lend` | OP-20 lending pool with kinked APY model & collateral |
| 🏛 DAO Hub | `Streak_DAO` | On-chain governance with proposals, voting & treasury |

---

## Architecture

```
streak-protocol/
├── contracts/
│   ├── streak-market/          # OP-721 NFT Exchange
│   │   ├── src/
│   │   │   ├── StreakMarket.ts  # Main contract
│   │   │   └── index.ts        # Entry point
│   │   └── tests/
│   │       └── StreakMarket.test.ts
│   ├── streak-lend/            # Lending Pool
│   │   ├── src/
│   │   │   ├── StreakLend.ts
│   │   │   └── index.ts
│   │   └── tests/
│   │       └── StreakLend.test.ts
│   └── streak-dao/             # Governance DAO
│       ├── src/
│       │   ├── StreakDAO.ts
│       │   └── index.ts
│       └── tests/
│           └── StreakDAO.test.ts
├── frontend/
│   ├── index.html              # Single-file React-free DeFi app
│   ├── package.json
│   └── vercel.json             # Vercel deployment config
├── SECURITY_AUDIT.md           # Full OpnetAudit report
├── asconfig.json               # AssemblyScript build config
├── tsconfig.json               # TypeScript config
└── package.json
```

---

## Smart Contract Details

### Streak_Market
- Trustless listing & buying of OP-721 artifacts
- Floor price tracking (updated on every listing)
- 2.5% protocol fee (basis-point math, SafeMath-only)
- Reentrancy guard on `listItem()`, `buyItem()`, `cancelListing()`
- Checks-Effects-Interactions: listing marked inactive BEFORE transfer

### Streak_Lend
- Deposit OP-20 assets to earn APY
- Borrow against BTC/NFT collateral (max LTV 75%)
- **Kinked interest rate model**: 
  - Base: 2% · Slope1: 10% (below 80% util) · Slope2: 50% (above)
  - Supply APY = BorrowAPY × utilization × (1 - 10% protocol fee)
- Per-block interest accrual with idempotency guard
- Architecture prepared for NFT collateral (Streak_Lend v2)

### Streak_DAO
- Create proposals (10-block voting delay, 144-block voting period)
- Vote FOR / AGAINST / ABSTAIN with token-weighted power
- 4% quorum requirement + majority FOR to succeed
- State machine: PENDING → ACTIVE → SUCCEEDED/DEFEATED → EXECUTED
- Protocol treasury with governance-controlled disbursements

---

## Security

All contracts pass the full OP_NET OpnetAudit checklist:
- ✅ No constructor trap (onDeployment pattern)
- ✅ Reentrancy guards on all mutating methods
- ✅ SafeMath everywhere — no raw arithmetic
- ✅ No floating-point — all math in basis points (u256)
- ✅ No native `Map<>` — uses OP_NET storage primitives
- ✅ CEI order on all state + transfer patterns
- ✅ Full `@method()` ABI declarations

See [SECURITY_AUDIT.md](./SECURITY_AUDIT.md) for full audit report.

---

## Frontend

The frontend is a **production-ready, single-file HTML app** with:
- Obsidian + Cyber-Gold design system
- Glassmorphic navigation with mobile hamburger
- OP_WALLET connect via `@btc-vision/walletconnect`
- NFT Marketplace: responsive 4-column grid, filtering, sorting
- Lending: APY ring charts, LTV health bars, flash loan modal
- DAO Hub: proposal voting bars, treasury overview, DAO registry
- Tooltips on all DeFi terms (APY, Floor Price, LTV, TVL)
- Slide-in toast notifications (pending, success, error, info)
- Full FAQ section + footer help links

---

## Installation & Testing

```bash
# Install dependencies
npm install

# Build all contracts
npm run build:all

# Run all unit tests
npm run test:all

# Run security audit
npm run audit:all

# Start frontend locally
cd frontend && npm run dev
```

---

## Deployment

### Contracts (OP_NET Testnet)
```bash
# Deploy to testnet
npm run deploy:testnet
```

### Frontend (Vercel)
```bash
cd frontend

# One-command deploy for Vibecoding submission:
npx vercel --prod --yes
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Smart Contracts | AssemblyScript (OP_NET) |
| Runtime | `@btc-vision/btc-runtime` |
| Math | `@btc-vision/as-bignum` (u256 SafeMath) |
| Wallet | `@btc-vision/walletconnect` (OP_WALLET) |
| Frontend | HTML5 + CSS3 + Vanilla JS |
| Fonts | Syne (display) · DM Mono · Inter |
| Hosting | Vercel (static) |
| Chain | Bitcoin L1 via OP_NET |

---

## License

MIT — Built for the OP_NET Vibecoding Challenge Week 3
