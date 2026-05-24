# ⚖️ Lance — Freelancer Platform with AI Agent Judge

> **A next-generation freelancer marketplace built on the Stellar network.** Lance leverages Soroban smart contracts for secure escrow, Stellar USDC for borderless payments, and integrates advanced AI agents as impartial judges for automated, fair dispute resolution.

---

## 🌟 Overview

Lance reimagines the freelance economy by introducing an **AI Agent Judge** for dispute resolution. Traditional platforms rely on expensive and slow human arbitration when clients and freelancers disagree on the quality of deliverables. Lance solves this by using AI agents (powered by OpenClaw) to impartially review work against original job requirements and make binding decisions.

The platform is underpinned by **Soroban escrow smart contracts**. Clients deposit funds upfront, and the smart contract automatically releases payments based on milestone completion or the verdict of the AI judge if a dispute arises. This ensures trustless, cheap, and fair treatment for all parties.

## ✨ Key Features

- **🔐 Soroban Escrow Smart Contracts**: Job postings hold funds securely on-chain. Payments are guaranteed for completed work, eliminating non-payment risks.
- **🤖 AI Agent Judge**: Impartial dispute resolution powered by OpenClaw. The agent analyzes submissions, reasons about requirements, and provides clear, reasoned verdicts to resolve disagreements.
- **⚡ Milestone-based Payments**: Automatic, trustless payment releases upon approval of project milestones using Stellar USDC.
- **⭐ On-chain Reputation Tracking**: A robust Soroban-based reputation system mapping identity and work history (inspired by decentralized identity concepts but strictly native to Stellar and Soroban).
- **📂 Evidence Submission & Appeal Process**: Built-in mechanisms for submitting proof of work and handling large complex disputes securely.

---

## 🏗️ Architecture

Lance is a full-stack decentralized application consisting of a Next.js frontend, a Rust backend for the AI agent operations, and Soroban smart contracts orchestrating the on-chain logic.

```text
lance/
├── apps/web/          ← Next.js 14 frontend (TypeScript, Tailwind, shadcn/ui)
├── backend/           ← Rust/Axum REST API + OpenClaw AI judge service
├── contracts/
│   ├── escrow/        ← Soroban escrow contract (deposit, release, dispute handling)
│   ├── reputation/    ← Soroban reputation contract (on-chain scores & history)
│   └── job_registry/  ← Soroban job registry (job posting, bidding, deliverables)
├── tests/e2e/         ← Playwright end-to-end tests
├── docs/              ← Architecture docs, ISSUES.md
└── .github/workflows/ ← CI/CD pipelines
```

## 🛠️ Technology Stack

| Layer          | Technology                                                         |
| -------------- | ------------------------------------------------------------------ |
| **Frontend**   | Next.js 14, TypeScript, Tailwind CSS, shadcn/ui                    |
| **Wallet**     | Freighter via `@creit.tech/stellar-wallets-kit`                    |
| **Contracts**  | Rust / Soroban (Stellar Smart Contracts)                           |
| **Escrow**     | Soroban contract integrated with Stellar native multisig           |
| **Reputation** | Custom Soroban contract for decentralized identity & scoring       |
| **Payments**   | Stellar USDC (Circle) for fast, low-cost settlement                |
| **Backend**    | Rust / Axum for high-performance API routing                       |
| **Database**   | PostgreSQL (SQLx) for off-chain relational data                    |
| **AI Judge**   | OpenClaw agent for reasoning & verdict generation                  |
| **CI/CD**      | GitHub Actions                                                     |
| **Deploy**     | Vercel (frontend) · Fly.io (backend) · Stellar Testnet (contracts) |

---

## 🚀 Quick Start

### Prerequisites

- Node.js ≥ 20
- Rust (stable) + `wasm32-unknown-unknown` target
- `stellar` CLI
- Docker (for local Postgres databases)
- [Freighter wallet](https://www.freighter.app/) browser extension

### Install

```bash
# 1. Clone the repository
git clone <repo-url> && cd lance

# 2. Install frontend dependencies
cd apps/web && npm install && cd ../..

# 3. Build backend (ensure Rust compiles correctly)
cargo build -p backend

# 4. Build Soroban contracts
cargo build --target wasm32-unknown-unknown -p escrow
cargo build --target wasm32-unknown-unknown -p reputation
cargo build --target wasm32-unknown-unknown -p job_registry
```

### Environment Setup

```bash
# Setup web frontend environment
cp apps/web/.env.example apps/web/.env.local

# Setup backend environment
cp backend/.env.example backend/.env
# optional per-environment overrides
cp backend/.env.example backend/.env.development
```

Backend env loading precedence:

1. Existing process environment variables
2. `backend/.env.<APP_ENV>` (or `.env.<RUST_ENV>`)
3. `backend/.env`

### Run Locally

```bash
# Terminal 1 — Start Postgres via Docker
docker run --rm -p 5432:5432 -e POSTGRES_PASSWORD=lance postgres:16

# Terminal 2 — Run the Backend API & AI Judge Service
cd backend && cargo run

# Terminal 3 — Run the Next.js Frontend
cd apps/web && npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to start exploring Lance.

---

## 💎 Smart Contracts

Lance operates heavily on-chain to guarantee trustlessness. Our Soroban contracts handle job registries, escrows, and reputation scoring.

```bash
# Run all Soroban contract tests
cargo test -p escrow -p reputation -p job_registry

# Deploy contracts to Stellar Testnet (requires stellar CLI + funded account)
./.github/scripts/deploy-contracts.sh
```

---

## 🧪 Testing

```bash
# Frontend unit tests
cd apps/web && npm test

# Backend Rust tests
cargo test -p backend

# E2E test suite (requires running frontend + backend)
cd tests/e2e && npx playwright test
```

---

## User guides

- [Connecting a Stellar wallet](./docs/user-guide/stellar-wallets.md) — supported wallets, signing flow, network setup, and troubleshooting.
- [Running the stack](./docs/running-the-stack.md) — local startup, health checks, sync status, metrics, and worker operations.

---

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for details on how to get started.

---

## 📄 License

MIT License
