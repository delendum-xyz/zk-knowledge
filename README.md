<div align="center">
  <h1 align="center">ZKP Knowledge Base</h1>
  <p align="center">An ongoing knowledge base for ZKP domain knowledge.</p>
</div>

Table of Content
=================

* [Hardware Acceleration](#hardware-acceleration)
* [Light Client](#light-client)
* [Regulations](#regulations)
* [Security Vulnerability](#security-vulnerability)
* [Proof Aggregation](#proof-aggregation)
* [Field Selection](#field-selection)
* [Identity](#identity)

## Hardware Acceleration

### Leading Problem:

- Proof generation is time-consuming, high-latency, and not efficient enough on traditional CPUs
- As L2 rollups and other applications of ZKP grow, we need to consider hardware as a potential breakthrough of performance improvement

### Business Models:

- zk-mining: Plug-n-play FPGA accelerator cards (revenue through hardware sale)
- zk-Rollup: public, private and on-premise cloud (revenue through hourly premiere)
- Software auxiliary: SDK for dApps and API (license to program FPGA in developer-friendly way

### 3 Potential Scenarios

1. Permissioned networks, e.g. StarkNet 
2. Permissionless networks where provers compete on cost, e.g. Mina's Snarketplace
3. Permissionless networks where provers compete on latency, e.g. Polygon Hermes & Zero 

## Light Client 

### Leading Problem:

- Full node is resource-draining: downloading and verifying the whole chain of blocks takes time and resources
- Not retail friendly to be on mobile and perform quick actions like sending transactions and verifying balances

### Technology Highlight:

- Trust-minimized interaction with full nodes and do not interact directly with the blockchain
- Much lighter on resources and storage but require higher network bandwidth
- Linear time based on chain length: verify from the root to the target height (may be from a trusted height, therefore constant length)
- Process overview:
    - retrieve a chain of headers instead of full block
    - verify the signatures of the intermediary signed headers and resolve disputes through checking with full nodes iteratively
- Superlight client: requires downloading only a logarithmic number of block headers while storing only a single block header between executions

## Regulations
- ... 

## Security Vulnerability
- ... 

## Proof Aggregation

### Leading Problem:

- Proving n statements individually leads to n proofs. Therefore, in the absence of recursion/aggregation, the verification cost grows linearly with the number of statements proven. This is not ideal for a high-throughput blockchain

### Technology Highlight:

- Custom PLONK gates, the bilinearity elliptic curve pairings as used in KZG

### Proof Aggregation Schemes:

- Halo uses recursion to include proof number i-1 in proof i for all i and: 
  - amortizes the verification cost of the final proof for all n proofs
  - necessitates sequentialism: proof i cannot be generated until proof i-1 already exists
- Plonky2 uses custom PLONK gates for the Poseidon hash function to succinctly verify FRI proofs
  - recursive proofs on FRI proofs to prove the validity of multiple proofs in one
  - the final recursive proof proves the validity of all tx proofs
- SnarkPack utilizes the KZG10 polynomial commitment schemes together with the Bulletproof trick to aggregate Groth16 proofs into a single proof 
  - can be verified in base-2 logarithmic time (in the number of proofs)

### Current Research Progress:

### Reference Papers:

- [Proofs for Inner Pairing Products and Applications](https://eprint.iacr.org/2019/1177)
- [SnarkPack: Practical SNARK Aggregation](https://eprint.iacr.org/2021/529)
- [Recursive Proof Composition without a Trusted Setup](https://eprint.iacr.org/2019/1021)
- [Fractal: Post-Quantum and Transparent Recursive Proofs from Holography](https://eprint.iacr.org/2019/1076)
- [Fast Recursive Arguments with PLONK and FRI](https://github.com/mir-protocol/plonky2/blob/main/plonky2/plonky2.pdf) 

## Field Selection

### Leading Problem:

## Identity
- ... 
