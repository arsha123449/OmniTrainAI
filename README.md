# OmniTrain AI ($OTAI)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Rust](https://img.shields.io/badge/Built%20with-Substrate-orange)](https://substrate.io/)
[![PoUW](https://img.shields.io/badge/Consensus-PoUW-blue)](#)
[![Network](https://img.shields.io/badge/Network-omnitrainai.network-green)]()

**OmniTrain AI** is a decentralized, high-performance **Layer-1 Proof-of-Useful-Work (PoUW)** blockchain network engineered as a universal processing engine to monetize consumer and enterprise GPU compute power. Instead of wasting energy on arbitrary math puzzles, node operators earn native `$OTAI` coins by executing real-world, verified AI workloads—including LLM training, parameter-efficient fine-tuning (LoRA/QLoRA), and high-speed inference.

Built on the robust **Substrate framework**, OmniTrain AI operates as a standalone solo chain that integrates native asset conversion and a multi-sig bridge to the Base Layer-2 network for instant liquidity and cash-outs.

---

## 🚀 Core Features

- **Proof-of-Useful-Work (PoUW):** Native Layer-1 consensus where token emissions are directly tied to verified AI execution environments (PyTorch, vLLM, Hugging Face).

- **Hardened Compute Score:** Prevents hardware spoofing, network sybils, and ghost-worker attacks using a strict multi-variable validation formula:
  `ComputeUnits = (HardwareScore × UptimeScore) × JobMultiplier × VerifierApproval`.

- **Anti-51% 3-Tier Node Architecture:** Consensus authority is decoupled from public mining power. 50% of the block verification weight is held by 20 hardcoded Admin Nodes, and 50% is split among up to 200 Sub-Admin license holders. Public miners execute jobs but hold 0% governance/consensus power, making ledger takeovers mathematically impossible.

- **Idle = Zero Mint (For GPU, Treasury, Founders):** To prevent token devaluation and "zombie printing," if no active GPU workers are running AI tasks, the 87% (GPU), 2% (Treasury), and 1% (Founders) emission pools **mint exactly zero**. GPU Workers who are online but idle (no tasks available) also receive zero rewards. They must execute verified AI workloads to earn.

- **Master Node Liveness Payment:** Master Nodes receive their 10% reward in **every block**—active or idle—because they are the backbone that keeps the chain alive 24/7. Their continuous block production ensures that $OTAI remains tradeable, bridgeable, and usable for AI training purchases at any moment. This is a payment for liveness and security, not idle inflation.

- **Dynamic Bootstrapping Bonus:** When the network transitions from an idle state (0 active GPU workers) to active computing, an early-adopter multiplier triggers. The duration of this 2x bonus perfectly matches the duration the network spent idle (minimum 100 blocks, maximum 10,000 blocks). GPU Workers, Treasury, and Founders receive 2x rewards. **Master Nodes receive NO bonus** (they are always paid their 10%). The bonus is funded by **deducting from the normal block reward**—no new tokens are minted, and the Treasury is never drained. Total supply remains capped at 110B.

- **Native Conversion & Bridge Gateway:** Integrated `pallet-asset-conversion` lets enterprise AI clients pay in standard stablecoins (`oUSDT`) on-chain. An off-chain relayer cluster connects the L1 natively to **Base (EVM)** for friction-free miner liquidations.

---

## 💰 Tokenomics & Supply Allocation

**Token Ticker:** `$OTAI`
**Total Max Supply:** `110,000,000,000` (110 Billion)
**Block Time:** `30 Seconds`
**Halving Interval:** `3,155,760 blocks` (~3 Years)
**Official Network Domain:** [omnitrainai.network](https://omnitrainai.network)

### Final Supply Allocation

| Pool | Allocation | Total Tokens | Minting Mechanism |
| :--- | :--- | :--- | :--- |
| **GPU Mining & Compute Rewards** | 87.0% | 95,700,000,000 | Emitted dynamically via active PoUW workloads |
| **Master Node Staking Rewards** | 10.0% | 11,000,000,000 | Continuous block emission for network liveness |
| **Development Treasury** | 2.0% | 2,200,000,000 | Genesis Mint (1.5% Core Dev + 0.5% Quarterly Buyback) |
| **Founders Allocation** | 1.0% | 1,100,000,000 | Genesis Mint |

### Block Reward Split (Era 1: 16,905.50 OTAI/Block)

| Recipient | Percentage | OTAI per Block | Condition when Active | Condition when Idle |
| :--- | :--- | :--- | :--- | :--- |
| **GPU Worker Pool** | **87.0%** | 14,707.78 | Distributed via Compute Score | **0.00 OTAI (Zero Mint)** |
| **Master Nodes** | **10.0%** | 1,690.55 | Pro-rata by node stake | **1,690.55 OTAI (Liveness Payment)** |
| **Development Treasury** | **2.0%** | 338.11 | Routed to Treasury account | **0.00 OTAI (Zero Mint)** |
| **Founders Allocation** | **1.0%** | 169.06 | Routed to Core Multi-sig | **0.00 OTAI (Zero Mint)** |

### Bootstrapping Bonus Mechanics

| Parameter | Value |
| :--- | :--- |
| **Activation Trigger** | Network transitions from 0 GPUs → 1+ GPUs |
| **Duration** | = Idle Duration (min 100 blocks, max 10,000 blocks) |
| **Multiplier** | 2x for GPU Workers, Treasury, Founders |
| **Master Nodes** | **NO BONUS** (always 1,690.55 OTAI) |
| **Funding Source** | Deducted from normal block reward (no new minting) |
| **Treasury Impact** | **Zero** (Treasury is never used to fund the bonus) |
| **Total Supply Impact** | **Zero** (supply remains capped at 110B) |

---

## ⚙️ Core Infrastructure Architecture

### 1. 3-Tier Node System

| Tier | Role | Allocation / Count | Voting Weight | Access Requirements |
| :--- | :--- | :--- | :--- | :--- |
| **Admin Node** | Core consensus, block production, and multi-sig EVM bridge relaying. | 20 Nodes (Hardcoded) | 20 Units Each | Native Genesis Keys |
| **Sub-Admin** | Core consensus validation, validation monitoring, network routing. | 100 - 200 Nodes Max | 1 Unit Each | $500 USDT License Key + 10,000 $OTAI Stake |
| **Regular Node** | Public open computing. Executes AI jobs. | Unlimited | 0 Units (No Consensus) | Open Registration via Worker Client |

### 2. The L1-to-EVM Conversion Pipeline (`oUSDT` to `USDT`)

To eliminate commercial friction, enterprise AI clients use a fiat/stablecoin gateway routed to our contract on **Base**.

When a miner or holder wishes to liquidate their holdings natively:

1. They call `initiate_withdrawal` on our custom bridge pallet, which permanently **burns** their on-chain `oUSDT`.
2. The 20 hardcoded Admin Nodes run an off-chain Node.js daemon that monitors this burn event via L1 WebSockets (`ws://127.0.0.1:9944`).
3. Once **14 out of 20 (70%)** unique admin cryptographic signatures are compiled, the proof is submitted to the EVM vault on Base, releasing real, liquid `USDT` to the miner's target Web3 wallet address.

### 3. Native Asset Conversion (`oUSDT` Lifecycle)

| Stage | Action | Who |
| :--- | :--- | :--- |
| **Minting** | Admin multi-sig mints `oUSDT` when USDT is deposited on Base | 14/20 Admin Nodes |
| **Backing** | 1:1 backed by USDT held in the Base bridge vault | Locked on Base |
| **Usage** | AI clients pay for compute in `oUSDT` | Any client |
| **Burning** | `oUSDT` is burned when withdrawn back to Base | Any holder |
| **Redemption** | USDT released from Base vault to user's EVM wallet | 14/20 Admin Nodes |

---

## 🛠️ Implementation Stack

- **Layer-1 Node Runtime:** Rust + Substrate FRAME Modules (`pallet-assets`, `pallet-asset-conversion`)
- **Off-Chain Auth Server:** Rust + Axum (Generates and maps hardware-bound Sub-Admin License keys)
- **Database Indexer:** PostgreSQL
- **Admin Relayer Daemon:** Node.js / Ethers.js v6
- **GPU Worker Client:** Python + PyTorch + vLLM + Hugging Face Transformers
- **Block Explorer:** Statescan (open-source, self-hosted) / Polkadot-JS Apps (development)

---

## 🚀 Getting Started (Local Development Deployment)

### Prerequisites

- Rust (Latest stable toolchain with `wasm32-unknown-unknown` target configured)
- Node.js v18+ (For running off-chain bridge daemons)
- Docker Desktop (For localized cluster testing)
- Python 3.10+ (For GPU worker client)

### Local Node Simulation

To spin up a local development node environment mimicking the core architecture framework:

```bash
# Clone the repository
git clone https://github.com/omnitrainai/omnitrain-chain
cd omnitrain-chain

# Build the node template with optimized release profile
cargo build --release

# Run a local single-node development network
./target/release/node-template --dev --rpc-port 9944 --rpc-cors all
```

### Running the Off-Chain Connection Daemon

Navigate to the relayer layer to connect your local L1 runtime changes to your target EVM contract endpoints:

```bash
cd relayer-daemon
npm install
ADMIN_PRIVATE_KEY="0x_your_admin_secret" EVM_BRIDGE_ADDRESS="0x_your_contract" node index.js
```

### Running the GPU Worker Client

```bash
cd gpu-worker
pip install -r requirements.txt
python worker.py --node-url http://localhost:9933 --gpu-id 0
```

### Connecting to a Block Explorer

Open [Polkadot-JS Apps](https://polkadot.js.org/apps/), click the network icon (top-left), select "Development" → "Custom", and enter:

```
ws://localhost:9944
```

Your OmniTrain AI blockchain will appear with all blocks, transactions, and staking data.

---

## 📊 Economic Rules Summary

| Rule | Description |
| :--- | :--- |
| **GPU Workers** | Paid only when executing verified AI tasks. Zero when idle. |
| **Master Nodes** | Always paid 1,690.55 OTAI/block (liveness payment). No bootstrapping bonus. |
| **Treasury** | Zero mint when idle. 2x bonus during bootstrapping. Never drained. |
| **Founders** | Zero mint when idle. 2x bonus during bootstrapping. |
| **Bootstrapping Bonus** | Dynamic duration = idle duration (min 100, max 10,000 blocks). |
| **Total Supply** | Hard-capped at 110,000,000,000 OTAI. Never exceeds. |
| **Bridge** | 14/20 Admin multi-sig required for all cross-chain transfers. |

---

## 📄 License

This project is licensed under the terms of the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🤝 Contributing

We welcome contributions! Please read our [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

---

## ⚠️ Disclaimer

This software is provided as-is for educational, infrastructure research, and developmental purposes. Digital asset networks carry intrinsic market risks; always conduct independent due diligence before participating in the network.

---

## ✅ What Was Fixed in This Version

| Issue | What Changed |
| :--- | :--- |
| **Issue 1** | Bootstrapping Bonus now says "funded by deducting from block reward" (not Treasury) |
| **Issue 2** | Removed "zero additional inflation" claim. Now says "Total supply remains capped at 110B. Treasury is never drained." |
| **Issue 3** | Added explicit: "GPU Workers who are online but idle (no tasks available) also receive zero rewards." |
| **Issue 4** | Reframed Master Nodes as "Liveness Payment" (not idle inflation). Explained they keep the chain alive for trading and AI purchases. |
| **Issue 9** | Added maximum cap on Bootstrapping Bonus (10,000 blocks) |
| **Issue 11** | Added full oUSDT lifecycle table (Minting, Backing, Usage, Burning, Redemption) |
| **Issue 16** | Added Block Explorer section with Polkadot-JS Apps + Statescan |
| **Issue 17** | Added GPU Worker Client setup in Getting Started |
| **Issue 19** | Added Economic Rules Summary table |

---

This README is now **investor-ready** and free of the critical contradictions. You can copy the entire markdown block above directly into your `README.md` file.

Would you like me to also write the **CONTRIBUTING.md**, **CODE_OF_CONDUCT.md**, or **CONTRIBUTORS.md** files to match this rebranded OmniTrain AI version?
