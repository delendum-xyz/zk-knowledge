<div align="center">
  <h1 align="center">ZKP Knowledge Base</h1>
  <p align="center">An ongoing knowledge base for ZKP domain knowledge.</p>
</div>

Table of Content
=================

* [Hardware Acceleration](#hardware-acceleration)
* [Light Client](#light-client)
* [Arithmetic Fields](#Arithmetic-Fields)
* [Efficient Signatures](#Efficient-Signatures)
* [Proof Aggregation](#proof-aggregation)
* [Vulnerability](#vulnerability)
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

### Selection metrics: 
1. Performance: latency, throughput, and power consumption 
2. Total cost of ownership (capital expenditures and maintenance costs) 
3. NRE cost (non recurring engineering: onboarding difficulties) 
4. Long-term comparative advantage: competitor performance may double in a year with the same MSPR (e.g. Intel, Nvidia GPU) 

### Structure of ZKP Hardware Acceleration: 
- Current popular structure is to combine CPU and FPGA/GPU to perform proof generation. CPU tackles single-threaded pieces at higher frequency and deal with non-determinism 
- MSM and FFT can be parallelized, but arithmetization and commitments may be single threaded (different from the “embarrassingly parallel” PoW mining structure) 
- Most FPGA design tools are proprietary: FPGAs have higher barrier to enter than GPU accelerations 

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

## Arithmetic Fields

### Leading Problem:

- Proof systems encode computation as a set of polynomial equations defined over a field. Operating on a different field will lead to gigantic circuits.
- For pairing based SNARKs, there is a limited selection of pairing-friendly curves such as BN254 and BLS12-381.
- FFTs require factors of two for the curves in order to make the polynomial computations practical.
- Proof recursions require encoding arithmetic statements of a field into equations over a different field.
- Bridge operators and roll-ups need interoperability with non-pairing friendly and not highly 2-adic curves, such as Bitcoin and Ethereum’s ECDSA over secp256k1. ([BN254 precompiled contract](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-197.md))
- Choosing the field is a tradeoff of speed and security.

### Solutions:

- Brute force: decompose elements into limbs and operate on the limbs; Reduce and recompose the elements only when necessary
    - Very expensive: may represent up to 1000X the original constraints in the circuit
    - [xJsnark framework](https://akosba.github.io/papers/xjsnark.pdf)
    - [gnark implementation](https://github.com/ConsenSys/gnark/blob/3d3672148b38c548b6527c5f2cfe1af5ae61a11f/std/math/nonnative/doc.go)
- 2-chains and cycles: use matching base fields to implement one curve inside of the other (assume pairing based schemes)
    - Non-pairing based scheme can use non-pairing friendly or hybrid cycles at a cost, such as using PCS (polynomial commitment scheme) or pasta curves with linear time.
    - [Pasta Curves](https://electriccoin.co/blog/the-pasta-curves-for-halo-2-and-beyond/)
    - [2-chains of elliptic curves](https://eprint.iacr.org/2021/1359.pdf)
- Lower the requirements on the field: Polynomial Commitment Scheme (PCS) may not involve elliptic curves at all to be instantiated on arbitrary fields

### Reference Reading: 

- [A survey of elliptic curves for proof systems](https://eprint.iacr.org/2022/586.pdf)
- [ECFFT: Fast Polynomial Algorithms over all Finite Fields](https://arxiv.org/abs/2107.08473)

## Efficient Signatures 

### Leading Problems:

- Signatures are widely used in projects and can contribute to slow computation speed and poor user experience
- Different types of signatures depend on different field or curve arithmetic and need different speed-up methods

### Suggestions:

1. Use “ZK native” signatures if possible (i.e. [Picnic](https://microsoft.github.io/Picnic/))
2. Use “native” curves if possible (i.e. [JubJub](https://github.com/zkcrypto/jubjub))
3. In non-native curve arithmetic, for example ED25519, combine the limb products with the same weight mod 255 (in bits) 
    1. minimize the number of splits by adding intermediate products with the same weight first. For example, when we have 2 intermediate projects of weight 2^51, we can split the sum into 52 bits 
    2. If we have an intermediate product with weight of 2^255 or more, we can change its weight to 1 and multiply by 19, since 2^255 = 19 mod p_ed25519
4. Use CRT related method like Aztec’s approach to have the prover give a purported product and check the identity mod p_native to ignore large limb products 
5. Use a PLONK or STARK can cut down the cost significantly 
6. Check [windowing](https://en.wikipedia.org/wiki/Elliptic_curve_point_multiplication) in the scalar multiplication code 
7. Leverage the constant generator in one of the multiplications in EdDSA 
8. [GLV Endomorphism](https://link.springer.com/chapter/10.1007/3-540-44647-8_11) can split a curve multiplication into two smaller ones in curves like secp256k1

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

### Reference Reading:

- [Proofs for Inner Pairing Products and Applications](https://eprint.iacr.org/2019/1177)
- [SnarkPack: Practical SNARK Aggregation](https://eprint.iacr.org/2021/529)
- [Recursive Proof Composition without a Trusted Setup](https://eprint.iacr.org/2019/1021)
- [Fractal: Post-Quantum and Transparent Recursive Proofs from Holography](https://eprint.iacr.org/2019/1076)
- [Fast Recursive Arguments with PLONK and FRI](https://github.com/mir-protocol/plonky2/blob/main/plonky2/plonky2.pdf) 

## Vulnerability

### Leading Problem:

- Trail of Bits is publicly disclosing critical vulnerabilities that break the soundness of multiple implementations of zero-knowledge proof systems, including PlonK and Bulletproofs
- These vulnerabilities are caused by insecure implementations of the Fiat-Shamir transformation that allow malicious users to forge proofs for random statements
- The vulnerabilities in one of these proof systems, Bulletproofs, stem from a mistake in the [original academic paper](https://eprint.iacr.org/2019/953.pdf), in which the authors recommend an insecure Fiat-Shamir generation

### Affected Parties:

- The following repositories were affected:
  - [ZenGo’s zk-paillier](https://github.com/ZenGo-X/zk-paillier)
  - [SECBIT Labs’ ckb-zkp](https://github.com/sec-bit/ckb-zkp)
  - [Adjoint, Inc.’s bulletproofs](https://github.com/adjoint-io/bulletproofs)
  - [Dusk Network’s plonk](https://github.com/dusk-network/plonk)
  - [Iden3’s SnarkJS](https://github.com/iden3/snarkjs)
  - [ConsenSys’ gnark](https://github.com/ConsenSys/gnark)

### Reference Reading:

- [Serving up zero-knowledge proofs](https://blog.trailofbits.com/2021/02/19/serving-up-zero-knowledge-proofs/)
- [The Frozen Heart vulnerability in PlonK](https://blog.trailofbits.com/2022/04/18/the-frozen-heart-vulnerability-in-plonk/)
- [The Frozen Heart vulnerability in Bulletproofs](https://blog.trailofbits.com/2022/04/15/the-frozen-heart-vulnerability-in-bulletproofs/)
- [The Frozen Heart vulnerability in Girault’s proof of knowledge](https://blog.trailofbits.com/2022/04/14/the-frozen-heart-vulnerability-in-giraults-proof-of-knowledge/)
- [Coordinated disclosure of vulnerabilities affecting Girault, Bulletproofs, and PlonK](https://blog.trailofbits.com/2022/04/13/part-1-coordinated-disclosure-of-vulnerabilities-affecting-girault-bulletproofs-and-plonk/)

## Identity
- ... 
