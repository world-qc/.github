# World Quantum Computer (WQC)

WQC is an open-source protocol for decentralized quantum computation: consumer hardware runs useful circuits and proves the work with zk-STARKs.

A public **testnet is planned for Q4 2026**. The repositories miners need to join it are public now.

## Start here

**[wqc-miner](https://github.com/world-qc/wqc-miner)** is the operator entry point. It generates keys, holds network settings, and supervises the worker processes. Setup for the testnet is in [wqc-miner `docs/TESTNET.md`](https://github.com/world-qc/wqc-miner/blob/main/docs/TESTNET.md).

Protocol specification, whitepaper, and the HTTP API reference live in **[wqc-docs](https://github.com/world-qc/wqc-docs)** ([rendered API reference](https://world-qc.github.io/wqc-docs/)).

## Public repositories

| Repository | Role |
| --- | --- |
| [wqc-miner](https://github.com/world-qc/wqc-miner) | Launcher and local admin UI for worker nodes |
| [wqc-node](https://github.com/world-qc/wqc-node) | libp2p swarm agent: bid, execute a slice, return a proof |
| [wqc-core](https://github.com/world-qc/wqc-core) | Quantum circuit executor (tensor-network MPS + zk-STARK) |
| [wqc-stark-engine](https://github.com/world-qc/wqc-stark-engine) | STARK prover and verifier used by `wqc-core` |
| [wqc-keygen](https://github.com/world-qc/wqc-keygen) | Ed25519 key utility for worker nodes |
| [wqc-docs](https://github.com/world-qc/wqc-docs) | Specifications, whitepaper, and API reference |

Worker code is [GPL-3.0](https://www.gnu.org/licenses/gpl-3.0). Documentation is [GFDL-1.3](https://www.gnu.org/licenses/fdl-1.3.html).

## Network-side components

The live control plane is mapped in [wqc-docs `spec/architecture-current.md`](https://github.com/world-qc/wqc-docs/blob/main/spec/architecture-current.md). Source for these components ships with mainnet. The map is public; the repositories stay closed until then, for security and to keep the network implementation from being copied ahead of launch.

| Component | Role |
| --- | --- |
| wqc-orchestrator | HTTP client API, slicing, bid lottery, dispatch, quorum, leaf-PCS nomination, consensus verify, compose enqueue, manifest seal |
| wqc-p2p-proxy | libp2p sidecar: gossip and streams on the public P2P port; control frames to the orchestrator over a Unix socket |
| wqc-composer | Redis worker that builds the root STARK from leaf artifacts in CAS |
| wqc-snark-wrap | Groth16 wrap of the root proof when on-chain validity proofs are enabled |
| wqc-contracts | L2 Solidity: `$WQC` token, optimistic settlement, and thin-wrap settlement |

Miners run the public worker stack (`wqc-miner`, `wqc-node`, `wqc-core`). The control plane stays with the network. On the public testnet the off-chain Redis ledger is the source of truth; on-chain settlement is the mainnet path.

## Security

Report vulnerabilities privately. See the [security policy](https://github.com/world-qc/.github/blob/main/SECURITY.md).
