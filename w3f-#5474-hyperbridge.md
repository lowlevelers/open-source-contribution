---
id: learn-hyperbridge
title: Hyperbridge
sidebar_label: Hyperbridge
description: A brief overview of Hyperbridge, the innovative interoperability solution for crosschain bridges.
keywords: [bridges, cross-chain, bridge-methods]
slug: ../learn-hyperbridge
---

import RPC from "./../../components/RPC-Connection";

Interoperability is the core vision of the Polkadot technology. Through years of blockchain development, there has been many effort into making a secure interoperability solution to communicate between blockchains.

:::info Learn about Parachain and Bridges

To fully follow the material on this page, it is recommended to be familiar with the concepts of the [Parachain](https://wiki.polkadot.network/docs/learn-parachains) and [Bridges](./learn-bridges.md).

:::

<!-- %% Add more information about efforts into making trustless bridges and problems of bridges %% -->

## Coprocessor Model

Ensuring secure cross-chain communication involves the meticulous verification of various aspects, including: [Consensus Mechanisms](https://wiki.polkadot.network/docs/learn-consensus), [Consensus Faults](https://research.polytope.technology/consensus-proofs), [State Proofs](https://research.polytope.technology/state-machine-proofs) and [State Transitions](https://wiki.polkadot.network/docs/learn-parachains#state-transitions).

> What is a coprocessor?
>
> **Coprocessor**, in the context of hardware, can be referred to a microprocessor designed to supplement the capabilities of the primary processor. For example, GPU is a coprocessor of the CPU to be optimized for graphical and simultaneous computation.

Due to the complexity and expensiveness of the onchain verification process, in coprocessor model, the computation is performed off-chain and the outcomes of the execution, along with cryptographic proofs validating their accuracy, are subsequently presented on-chain.

The coprocessor model has been applied in other solutions, particularly, **SNARK coprocessor model**.

<!-- %% Add more information about the state problems of the ZK coprocessor model %% -->

## Hyperbridge

Hyperbridge (short for hyper-scalable bridge) is a cross-chain solution built as an interoperability coprocessor. Hyperbridge is crafted to scale cryptographically secure, consensus, and state-proof-based interoperability across all blockchains.

### Parachain as Coprocessors

By leveraging the cost-effective consensus proofs facilitated by [BEEFY](https://spec.polkadot.network/sect-finality#sect-grandpa-beefy), Hyperbridge affirms the legitimacy of all parachain state transitions safeguarded by the network.

This capability enables the distribution of the validation workload for consensus, state proofs, and state transition re-execution across various designated [Parachain Cores](https://github.com/polkadot-fellows/RFCs/blob/6f29561a4747bbfd95307ce75cd949dfff359e39/text/0001-agile-coretime.md). Hence, Polkadot is utilized by the Hyperbridge as a verifiable computation layer to provide the "full node security" in cross-chain bridges.

<!-- Explain more about full node security -->

### Interoperable State Machine Protocol (ISMP)

<!-- Give more information about the ISMP. -->

### Underlying technologies

Underlying technologies of the Hyperbridge is integrated with:

- PLONK verifier
- [BEEFY consensus](https://spec.polkadot.network/sect-finality#sect-grandpa-beefy)
- The Barretenberg backend.

## Terminology

### State Machine

### State Transitions

State transitions represent the changes in the state of a blockchain as a result of valid transactions. When engaging in cross-chain communication, it is crucial to verify that state transitions are executed correctly and securely. This includes confirming that assets or data are transferred accurately and that the integrity of the overall system is maintained.

### State Proofs

### State Machine

## Learn More

The information provided here is subject to change; keep up to date using the following resources:

- [Introducing Hyperbridge: An Interoperability Coprocessor](https://blog.polytope.technology/introducing-hyperbridge-interoperability-coprocessor) - Article by Seun Lanlege, Polytope Lab founder.
- [Digital Services as State Machines](https://polkadot-blockchain-academy.github.io/pba-book/blockchain-contracts/services-as-state-machines/page.html) - Lecture about state machine from Polkadot Blockchain Academy
- [Polkadot Wiki - Bridges](https://wiki.polkadot.network/docs/learn-bridges) - Learn about bridges in Polkadot
- [Polkadot Wiki - Parachain](https://wiki.polkadot.network/docs/learn-parachains) - Learn about parachain in Polkadot
- [Hyperbridge Source Code](https://github.com/polytope-labs/hyperbridge) - Public codebase repository of hyperbridge.
- [Interoperable State Machine Protocol (ISMP) Book](https://ismp.polytope.technology/) - Guidebook of the ISMP
- [The Puzzle of Blockchain Interoperability](https://twitter.com/stakenode_dev/status/1744653040764817675)
- [RFC-1: Agile Coretime](https://github.com/polkadot-fellows/RFCs/blob/6f29561a4747bbfd95307ce75cd949dfff359e39/text/0001-agile-coretime.md) - Agile periodic-sale-based model for assigning Coretime on the Polkadot Ubiquitous Computer.
- [PLONK verifier for Solidity](https://github.com/matter-labs/solidity_plonk_verifier) - PLONK verifier written for Solidity by Matter Labs
