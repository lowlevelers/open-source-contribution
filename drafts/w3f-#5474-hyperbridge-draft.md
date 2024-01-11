# Hyperbridge

## Outline

1. Definition of the Hyperbridge

Hyperbridge an **interoperability coprocessor capable of scaling cryptographically secure, consensus & state proof-based interoperability** to all blockchains.

Secure cross-chain communication requires the verification of consensus, consensus faults, state proofs, and state transitions.

- Consensus:
- Consensus faults:
- State proofs:
- State transitions:

These verification processes are typically too expensive to be performed onchain

1. Solving problems of the technology

Current problems of cross-chain communication solutions: Insecure bridges

- [**On Program Verification via ZkSnark circuit:**](https://crypto.stackexchange.com/questions/59324/on-program-verification-through-zk-snarks/62568#62568) In this model, a “prover” commits using a Polynomial Commitment scheme to the so-called execution trace of the computation in question, and attempts to convince a “verifier” through a Polynomial Interactive Oracle Proof combined with a Fiat-Shamir heuristic that the execution trace indeed describes a valid computation.

- [**Applications for Pinocchio:**](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/pinocchio.pdf)

a built system for efficiently verifying general computations while relying only on cryptographic assumptions. With Pinocchio, the client creates a public evaluation key to describe her computation; this setup is proportional to evaluating the computation once. The worker then evaluates the computation on a particular input and uses the evaluation key to produce a proof of correctness. The proof is only 288 bytes, regardless of the computation performed or the size of the inputs and outputs. Anyone can use a public verification key to check the proof.

- KZG commitment scheme: https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html

=> Proving the execution trace of the program places immense memory (RAM) requirements on the prover as they have to work with very **large execution traces** that describe the computation in question

- Casper Friendly Finality Gadget (FFG): https://arxiv.org/pdf/1710.09437.pdf

https://tokens-economy.gitbook.io/consensus/chain-based-proof-of-stake/proof-of-stake-casper-pos-casper

- Enshrined state machines:

Parachains are enshrined on the Polkadot relay chain, the Ethereum execution layer is enshrined on the Beacon chain, and zk rollups are indirectly enshrined on their settlement layer.

https://research.polytope.technology/state-machine-proofs

1. How does this impact to Polkadot parachain infrastructure?
2. Analyzing technical aspects of the advancement
3. Source code analysis of the Pallet ISMP

# Official Article

## Problems with cross-chain communication

### State Machine and Consensus Clients

Decoupling the **consensus mechanism and state machine in blockchain protocols**. By doing so, we enable the existence of "consensus clients," popularly known as **"light clients"** that only track consensus proofs of state transitions instead of the full state transitions.

Proofs of state transitions are stored in the Patricia Tree of the Relay chain in the Polkadot network. Light client like smoldot only sync the headers and consensus proofs

Duties of the light client

- ❌ Sync blocks
- ❌ Execute blocks
- ✅ Sync headers
- ❔ Maintain Transaction Pool
- ✅ Checks consensus
- ❌ Maintains state

### Insecure bridges

How to establish a secure cross-chain communication?

Secure cross-chain communication requires the verification of consensus, consensus faults, state proofs, and state transitions in the state machine.

1. Consensus

2. Consensus faults:

3. [State Machine Proofs](https://research.polytope.technology/state-machine-proofs)

Existential role of a blockchain is to provide a database that is backed by a sequence of valid state transitions

As we know, in the blockchain state machine, state `S` stands for the full database of blockchain while transition `T` in the Polkadot context is the list of extrinsic executed by the State Transition Function to produce a new state to the state machine `S'` => copy of the full database.

4. State transitions

Acknowledging the aforementioned problems of the overhead of state machine state changes. One solution is to leverage a "state diff" which demonstrate the difference between the old state with the new state.

- Reference [Blockchain from scratch exercise](https://github.com/lowlevelers/lowlevelers.com/issues/24)

To get the complete state of the new database, we can use previous state changes of the state machine `pre_state` and compute it with the `"state diff"` to get the new state root. Then we can use the state root to validate if the computation is valid.

```rs
pre_state + hash(extrinsics) = state_root
```

> Polkadot Parachains and Optimistic L2s on Ethereum both use state proofs to achieve parallel state machines running on a single consensus layer.
>
> State proof = proof of validity = state witness

- Parachain and state proof: In the case of Polkadot, the Polkadot relay chain re-executes parachain blocks with their given state proofs to ensure that only blocks with valid state transitions are finalized by the Polkadot network.
- Ethereum L2: Does not execute roll up blocks but use an offchain operator to re-execute block (See more in [Optimistic Rollup](https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/#scaling-ethereum-with-optimistic-rollups)).

> If an L2 block is posted to Ethereum with a state commitment that doesn't match the actual value calculated after its blocks are executed off-chain, the L2 block can be **challenged**.
>
> This is done by forcing its **re-execution on-chain**, so that its real state root maybe derived. Unfortunately, this is also why **withdrawals from optimistic L2s take at least 7 days**.
>
> The reason for the delay is to mitigate any censorship attacks
> that may arise, and **give honest parties enough time to challenge invalid L2 blocks.**

To scale state verfication in the context of the multi-sharded network, separating the logic of consensus layer and execution layer is neccessary. The execution layer processes the transitions of the single STF and sends proof of validity to the consensus layer for deciding if the state diff made by Parablock can added to the main chain (Relay chain).

Consensus layer is built using the light clients (See [smoldot]() and [beacon chain](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/light-client/sync-protocol.md))

> light clients today are essentially parasites since their state proof requests add extra networking and computing load on full nodes

### No standard of using consensus proofs

### Interop between Layer 2 rollups

## Hyperbridge

Hyperbridge is a cross-chain solution built as an interoperability coprocessor.

> Coprocessor is a microprocessor designed to supplement the capabilities of the primary processor. (In this case, _primary processor_ can be considered as the blockchain).
>
> For example, GPU is a coprocessor of the CPU to be optimized for graphical and simultaneous computation.

Hyperbridge is crafted to scale cryptographically secure, consensus, and state-proof-based interoperability across all blockchains.

> Additionally, we benefit cheap consensus proofs through BEEFY, which attests to the validity of all parachain state transitions secured by the network. Because of this, we can shard the work of validating consensus, state proof and state transition re-execution across multiple so-called parachain cores. This allows us to aggregate the cross-chain messages from multiple connected blockchains which are verified on multiple parachain cores into a single zk consensus & state proof that can be verified cheaply on any chain.

In its mission to revolutionize blockchain interoperability, Hyperbridge integrates three key technologies:

- the PLONK verifier
- BEEFY consensus
- The Barretenberg backend.

Let's explore how each component contributes to Hyperbridge's groundbreaking capabilities:

- PLONK Verifier:
  A High-Tech Security System: Think of the PLONK Verifier as a sophisticated security system within Hyperbridge. It's like an expert detective ensuring every transaction is legitimate without revealing private details. Constantly improving with advancements like UltraPLONK, it plays a crucial role in maintaining the integrity and confidentiality of cross-chain communications.

- ️BEEFY Consensus:
  The Efficient Bridge Builder: BEEFY acts as an efficient bridge between different blockchain networks, akin to a skilled translator who enables networks like Polkadot and Ethereum to trust and understand each other's data.

- Barretenberg Backend:
  The Powerhouse Engine: In the world of Hyperbridge, Barretenberg is like a powerhouse engine, handling complex mathematical computations. This backend ensures that all cryptographic operations within Hyperbridge are fast, secure, and reliable.

## ISMP

## Additional Resources and Examples

- [Introducing Hyperbridge Interoperability Coprocessor](https://blog.polytope.technology/introducing-hyperbridge-interoperability-coprocessor)
- [Polkadot Wiki - Bridges](https://wiki.polkadot.network/docs/learn-bridges) - Learn about bridges in Polkadot
- Hyperbridge source code: https://github.com/polytope-labs/hyperbridge
- ISMP Book: https://ismp.polytope.technology/
- Pallet ISMP: https://ismp.polytope.technology/pallet-ismp
- Twitter Tweet to introduce about Hyperbridge: https://twitter.com/stakenode_dev/status/1744653040764817675

- Gateway - first application built on Hyperbridge: https://gateway.hyperbridge.network/
- Polytype Labs - Consensus Proofs: https://research.polytope.technology/consensus-proofs
