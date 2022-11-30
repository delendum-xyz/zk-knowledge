---
layout: default
---

<div align="center">
  <h1 align="center">ZKP Knowledge Base</h1>
  <p align="center">An ongoing knowledge base for ZKP domain knowledge.</p>
</div>

## Historical Overview

### Foundations of zkSNARKs

This is an overview of foundational papers relevant to zkSNARKs. It’s mostly based on [presentations](https://twitter.com/stonecoldpat0/status/1017778014841602048/photo/1) by Jens Groth at the IC3 Bootcamp at Cornell University in July 2018 and at the 2nd [ZKProof Standards workshop](https://www.youtube.com/watch?v=X-z3JYlFdzs) in Berkeley, CA in April 2019.

Jens talks about three recurring motifs:

1. Language - what statements can we prove
2. Security - what are our assumptions, unconditional soundness vs. unconditional zero knowledge
3. Efficiency - prover, verifier computation, interaction, setup size, succinctness

The following papers in theoretical computer science/cryptography set the boundaries of what we are working with (note that some papers deal with more than one aspect of ZKPs):

#### Theory

[GMR85] Goldwasser, Micali, Rackoff [[The Knowledge Complexity of Interactive Proof Systems]](https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Proof%20Systems/The_Knowledge_Complexity_Of_Interactive_Proof_Systems.pdf)

- The paper which introduced the notion of interactive proofs, which constitute the most basic primitive underlying all modern zero-knowledge proof and argument systems
- GMR also define zero knowledge within the context of IP, along with notions such as completeness and soundness, which are fundamental for the theory of IP
- As a first use case, the others present zero knowledge interactive proofs for quadratic (non)residuosity

#### Language

[GMW91] Goldreich, Micali, Wigderson [[Proofs that Yield Nothing But their Validity or All Languages in NP have Zero-Knowledge Proofs]](https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Zero%20Knowledge/Proofs_That_Yield_Nothing_But_Their_Validity_or_All_Languages_in_NP_Have_Zero-Knowledge_Proof_Systems.pdf)

- look at graph 3-coloring problem
- prove that NP-complete languages have proof systems

#### Security

[GMW91] Goldreich, Micali, Wigderson [[Proofs that Yield Nothing But their Validity or All Languages in NP have Zero-Knowledge Proofs]](https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Zero%20Knowledge/Proofs_That_Yield_Nothing_But_Their_Validity_or_All_Languages_in_NP_Have_Zero-Knowledge_Proof_Systems.pdf)

- one-way functions suffice for computational zero-knowledge

[BC86] Brassard, Crépeau [[All-or-Nothing Disclosure of Secrets]](http://crypto.cs.mcgill.ca/~crepeau/PDF/ASPUBLISHED/BCR86.pdf)

- you can get perfect (unconditional) zero-knowledge as opposed to computational zero-knowledge

[BCC88] Brassard, Chaum, Crépeau [[Minimum Disclosure Proofs of Knowledge]](http://crypto.cs.mcgill.ca/~crepeau/PDF/ASPUBLISHED/BCC88.pdf)

- look at zero-knowledge against unbounded adversaries

#### Efficiency

[BFM88] Blum, de Santis, Micali, Persiano [[Non-interactive Zero-knowledge]](https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Zero%20Knowledge/Noninteractive_Zero-Knowkedge.pdf) 

- CRS (common reference string) for non-interactive ZKPs

[Kilian92] Kilian [[A note on efficient zero-knowledge proofs and arguments]](https://people.csail.mit.edu/vinodv/6892-Fall2013/efficientargs.pdf)

- how to get succinct interactive proofs

#### Pairing-based cryptography

Later on we have some ideas from pairing-based cryptography:

[BGN05] Dan Boneh, Eu-Jin Goh, Kobbi Nissim [[Evaluating 2-DNF Formulas on Ciphertexts]](https://crypto.stanford.edu/~dabo/papers/2dnf.pdf)

- pairing-based double-homomorphic encryption scheme

#### Non-interactive zero-knowledge

From there, we can ask, how to get perfect non-interactive zero-knowledge?

[GOS06] Jens Groth, Rafail Ostrovsky, Amit Sahai [[Perfect Non-Interactive Zero Knowledge for NP]](https://eprint.iacr.org/2005/290.pdf)

- efficient, uses circuit-SAT

[GS08] Jens Groth, Amit Sahai [[Efficient Non-Interactive Proof Systems for Bilinear Groups]](https://eprint.iacr.org/2007/155.pdf)

- efficient, uses a practical language (R1CS)

[Gro10] Jens Groth [[Short pairing-based non-interactive zero-knowledge arguments]](https://www.iacr.org/archive/asiacrypt2010/6477323/6477323.pdf)

- preprocessing zk-SNARKs
- power knowledge of exponent(KoE) assumption
- universal CRS, works for any circuit

#### Succinctness

And then, how to get succinctness?

[GW11] Gentry, Wichs [[Separating Succinct Non-Interactive Arguments From All Falsifiable Assumptions]](https://eprint.iacr.org/2010/610.pdf)

- SNARG = Succinct non-interactive argument
- justifies need for KoE assumption

[BCCT12] Bitansky, Canetti, Chiesa, Tromer [[From extractable collision resistance to succinct non-Interactive arguments of knowledge, and back again]](https://eprint.iacr.org/2011/443.pdf)

- zk-SNARK
- verifier time polynomial in statement size (ease of verification)

[GGPR13] Gennaro, Gentry, Parno, Raykova [[Quadratic Span Programs and Succinct NIZKs without PCPs]](https://eprint.iacr.org/2012/215.pdf)

- quadratic span programs, quadratic arithmetic programs
- specialized CRS, tailored to specific circuit

This is a graph of various constructions in this [video](https://www.youtube.com/watch?v=X-z3JYlFdzs&t=668s) at 34:00

#### Implementations

And at this point we begin to see implementations:

[PHGR13] Parno, Gentry, Howell, Raykova [[Pinocchio: Nearly Verifiable Practical Computation]](https://eprint.iacr.org/2013/279.pdf)

Relating back to the three motifs, Jens talks about the following areas for improvement with regard to each of the three motifs:

1. Language: Pairing-friendly languages, interoperability between languages

2. Security: Setup - multi-crs, mpc-generated, updatable; Formal verification

3. Efficiency: Asymmetric pairings, commit-and-prove

#### Reference Texts:

- Introduction to Modern Cryptography by Katz, Lindell
- Algebra by M. Artin
- Abstract Algebra by Dummit and Foote
- Introduction To Commutative Algebra, by M. Atiyah and I. Macdonald
- Arithmetic of Elliptic Curves by J. Silverman

## ZKP Compiler Pipeline Design

### Motivation Question

- The motivating question in defensive ZKP compiler pipeline design is:
  - If most general-purpose applications of ZKPs will require general-purpose programmable privacy, what compiler stack can allow developers to safely write programs and also produce efficient outputs?

- This is the motivating question simply because the things that are many are the programs. Eventually, we’ll have a few proof systems and maybe a few different compiler stacks and those might have some bugs and we’ll need to iron out those bugs and formally verify those parts - but the proof systems and compiler stacks will be relatively small in number compared to the actual programs. 
  - Consider comparable examples, such as the Ethereum Virtual Machine (EVM) and the Solidity compiler. Early on in the history of the EVM and Solidity, the EVM and the Solidity compiler both had a lot of bugs, and often, bugs in programs were if not equally likely then at least non-trivially likely to be due to a bug in the compiler or in the EVM as opposed to a bug in the program caused by a developer not thinking about their model or writing the Solidity code correctly. 
  - But, over time, as the EVM and Solidity “solidified” and became better understood and more formally modeled in different ways, the frequency of those bugs has declined, in the case of the EVM perhaps to zero and in the case of Solidity to a very low number and to mostly minor ones. Now most of the problems, if you go on [Rekt](https://rekt.news/), most bugs nowadays are not bugs in the Solidity compiler or bugs in the EVM but rather bugs in code written by developers - sometimes mistakes caused by the model of the program itself being unclear or wrong (with respect to what the developer probably intended), sometimes mistakes in the use of the Solidity language to express that model, but in both cases the result of an incongruity at the developer level. If you’re interested in preventing bugs, you have to focus on this layer because most of the bugs are written by developers.

- This holds doubly so for programs involving zero-knowledge proof systems (ZKPs) and privacy, because most developers aren’t going to be intricately familiar with the implementation details of zero-knowledge proof systems, and if the compilation stack requires that they understand these details in order to write programs safely, their programs are going to have a lot of bugs. It’s really hard to understand cryptography, especially if you don’t have a deep background in it and especially if you’re writing a complex application at the same time. 
- Of course, if you have a very safe compiler stack but it’s not fast enough and the performance costs are too high for actual applications, no one will be any safer because no one will use it. So, the question can be rephrased a wee bit: 
  - **What compiler stacks can provide the most protection to developers, while also being efficient enough that they will actually be used?**

### General Constraints

- There are some different constraints in circuit / ZKP compiler design than in the design of ordinary compilers. 
  - For one, there are some very specific “threshold latency constraints”, where execution times under a few seconds will be fine but execution times over that will render many applications impossible. 
  - In most cases, at least if you want privacy, users have to be creating the proofs, and when users are creating the proofs there’s a really big user experience difference between 3 seconds latency and 9 seconds latency, even though this is only a constant factor of 3. 
  - With normal compilers, you might be able to tolerate such small factor differences in the interest of making it easier to write programs (see: Javascript) - if you’re running a spreadsheet tax calculation program, for example, a difference of a factor of 3 may not actually matter that much - the person would just do something else while the program is running as long as they aren’t running it too frequently. 
  - With normal compilers you also tend to care about user interaction latency, which has deep implications in compiler design. For example, it means that languages with automatic memory management tend to do incremental garbage collection, which limits garbage collector latency, as opposed to simpler garbage collection algorithms which can cause unpredictable latency spikes. 
  - This doesn’t apply when writing circuits to be used in ZKP systems, because all of the interactions are **non-interactive with respect to one individual program** - the compiler should instead optimize for making the total prover execution time as low as possible.

- Another class of parties who could potentially write bugs are the circuit compiler developers - us! - so it behooves the ecosystem to come up with at least some reusable complements of the compiler stack that can be shared by multiple parties and jointly verified. To that end, the subsequent parts of this post craft a high-level taxonomy of current approaches to circuit compilation and describe their trade-offs.

- This taxonomy categorizes compilation approaches in three broad classes, which are defined and analyzed in order:
  1. DSL approach
  2. ISA/VM approach
  3. Direct approach

#### DSL Approach

- The first approach and the approach that quite logically the ecosystem started with is herein termed the “DSL approach”. 
- DSL stands for “domain-specific language”, and the DSL approach is to take something like [R1CS](https://learn.0xparc.org/materials/additional-learning-resources/r1cs%20explainer/) or [PLONK](https://eprint.iacr.org/2019/953.pdf) or [AIR](https://medium.com/starkware/arithmetization-i-15c046390862) and build a language on top of it, crafting abstractions iteratively in order to simplify the job of the programmer. 
- DSL instructions in this sort of a system typically act like constraint-writing macros - some variables can be tracked, there’s some sort of simplified compilation going on, but there’s no intermediate instruction set or abstract machine, and instructions in the DSL translate pretty directly to constraints. Examples of this approach include:
    - [Zokrates](https://zokrates.github.io/introduction.html)
    - [Circom](https://docs.circom.io/)
    - [Snarky](https://github.com/o1-labs/snarky)
    - [Leo](https://leo-lang.org/)
    - [Juvix Circuits](https://github.com/anoma/juvix-circuits)
    - [Cairo](https://eprint.iacr.org/2021/1063.pdf) (although over time it has gotten closer to an ISA/VM approach)
    - probably many others

- While these languages are quite different, written with different syntaxes and targeting different proof systems, they are all following the same kind of general approach of starting with a low-level constraint system and iteratively adding abstractions on top. With the DSL approach, each program is compiled to its own circuit.

- Advantages of the DSL approach:
  - It’s relatively easy to write a DSL because you can iteratively build abstractions on top of constraints - you don’t need to have a whole separately-defined instruction set or architecture
  - Because you have this low-level control, you can use a DSL as an easier way of writing hand-rolled circuit optimisations - it’s easy to reason about performance when the developer has a good understanding of exactly what constraints each instruction is translated into

- Disadvantages of the DSL approach:
  - DSLs aren’t as capable of high-level abstraction, at least not without more complex compiler passes and semantics more removed from the underlying constraint systems
  - If there are a lot of different DSLs for different proof systems, developers are always going to need the semantics of some proof-system-specific language in order to write circuits, and the semantics of these DSLs are quite different than the semantics of the languages most developers already know
  - If developers are writing programs that include some circuit components and some non-circuit components, those are two different semantics and languages, and the conversions in-between are a likely location for the introduction of bugs
  - Most DSLs don’t built in much of the way of a type system for reasoning about the behavior about programs, so developers who wish to verify that their programs satisfy certain correctness criteria will need to use external tools in order to accomplish this

#### ISA/VM Approach

- The next compilation approach is termed ISA (instruction set architecture) / VM (virtual machine) compilation. This approach is more involved. 
  - ISA/VM compilation requires clearly defining an intermediate virtual machine and instruction set architecture which then intermediates in the compilation pipeline between the high-level languages in which developers are writing programs and the low-level zero-knowledge proof systems with which proofs are being made. 
  - Although it is possible to write VM instructions directly, in this approach it is generally assumed that developers won’t (and these VMs are not optimized for developer ergonomics).

- This can be done with an existing instruction set. 
  - For example, the Risc0 team has done this with the RISC-V microarchitecture - originally designed for CPUs with no thought given to ZKP systems - but Risc0 has written a [STARK prover and verifier for RISC5](https://github.com/risc0/risc0) which is compatible with the existing specification of the RISC-V ISA.
- This can also be done with a custom-built VM and instruction set designed specifically to be efficient for a particular proof system, and potentially for recursive verification in the VM or some other specific application you might want to support efficiently. 
  - For example, the [Miden VM](https://github.com/maticnetwork/miden) is designed to be more efficient to prove and verify in a STARK than an architecture such as RISC5 (which was not designed for this) might be, and the [Triton VM](https://github.com/TritonVM/triton-vm) has been designed specifically to make recursive proof verification in the VM relatively inexpensive.
- All of these approaches use one circuit for all programs, and support many higher-level languages - the higher-level languages just need to compile to the specified instruction set - and all of these examples are [von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture)-style machines - so there’s a stack, operations on the stack, some form of memory, storage, I/O, and maybe some co-processors or built-in operations.

- Advantages of the ISA/VM approach:
  - Developers can use higher-level languages which they are either already familiar with or are easier to learn than low-level circuit DSLs
  - In the case of using an existing microarchitecture, such as RISC-V, you can reuse tons of existing tooling - anything which compiles to RISC-V, which is anything which compiles to [LLVM](https://llvm.org/), which is almost everything you would care about - can be used with the Risc0 circuit, prover, and verifier
  - Even in the case of a new instruction set architecture, such as with Miden VM and Triton VM, compilation techniques to these kinds of stack-based VMs for both imperative and functional languages are pretty well-known already, and compiler authors can reuse a lot of existing work that is not specific to ZKPs or circuits

- Disadvantages of the ISA/VM approach:
  - Although it’s unclear exactly what this cost is, there is probably going to be some cost of abstraction when compiling high-level programs to some intermediate instruction set - e.g. you’re doing stack operations, which can probably be checked more directly in a purpose-built circuit for any given program. There’s reason to expect that this will be especially true and that there will be a higher performance penalty if the instruction set architecture wasn’t built specifically for ZKPs (such as RISC-V).
  - Arguably, this approach offers more power than applications actually need, and is going to be correspondingly harder to verify correctness of because it’s more powerful. There’s a lot of noise about Turing-completeness, but no one really wants to be able to run ZKP computations that don’t terminate because it would be impossible to create a proof in practice. This is different than in the interactive world - most interactive programs, say a spreadsheet, never terminate because they take in user input in a loop and take action according to the users’ input, then wait for more input until the user (or OS) tells the program to exit. Circuit execution, on the other hand, always terminates.
  - If you want to verify the correctness of the compiler in the ISA/VM approach, you have to verify both the correctness of the translation from the higher-level language semantics into the intermediate instructions, and the correctness of the circuit which implements the intermediate ISA VM - although if you’re using existing or more powerful higher-level languages, there’s probably a lot of existing work that can be reused here.

#### Direct Approach

- The third approach, for lack of a better word, is just referred to here as direct.
The basic idea here is to somehow directly compile higher-level programs to circuits. 

  - These higher-level programs could be functional, based on the lambda calculus, or some kind of imperative language, but the direct approach involves transforming this high-level language directly to a circuit without an intermediate instruction set. 
  - The best candidate for this sort of translation, at least for functional languages, is based on a paper called [“Compiling to Categories”](http://conal.net/papers/compiling-to-categories/) by Conal Elliot, which works by taking the kind of simple lambda calculus core that you can translate a functional language to, translating the semantics of lambda calculus operations to the semantics of a category, translating the categorical semantics into the category of polynomial operations, then translating the polynomial operations into a specific circuit. 
  - You can find a more detailed description in the GEB project repository [here](https://github.com/anoma/geb/tree/main/geb). Similar to the DSL approach, with the direct approach each program generates a unique circuit. Other kinds of direct compilation may be possible - arguably, there’s a sort of continuum of DSLs where **they approach higher and higher-level semantics** and get more and more like this kind of direct compilation.

- Advantages of direct compilation:
  - Developers can use a very high-level language and at least in principle you can get very high performance because you don’t lose any information when you compile this way (e.g. you don’t have to represent things in terms of intermediate stack operations)
  - This approach is similar to some existing work in hardware circuit synthesis done outside of the context of ZKP systems but in the context of the end of Moore’s law and compiler pipelines that want to produce physical circuits and extremely parallel programs
  - At least with “Compiling to Categories”, compilation has a very clear mathematical model that is easy to reason about and verify

- Disadvantages of direct compilation:
  - More complexity in the compiler pipeline, at least compared to the simple DSL approach
  - Most existing tooling cannot be reused, since the core and compilation techniques are new and non-standard
  - As in general with any approach that involves a higher degree of abstraction, it may be more difficult for developers to reason about the performance of their programs

### Takeaways

- So many compiler options! It’s all a little exhausting - and prompts the question: what might be expected to happen, and where, accordingly, can efforts best be spent right now? 

  - The three approaches outlined here have different trade-offs, particularly in terms of **potential performance, low-level control, degree of abstraction, and difficulty of creating appropriate tooling**. 
  - Due to these trade-offs, we’re likely to see a hybridization of approaches, where **critical functions and gadgets** (e.g. hash functions, Merkle proof verification, signature checks, recursive verification, other cryptography) are written directly in lower-level **DSLs (1) or even in raw constraints**, but these will be provided in standard libraries and then tied together with **“business logic”** written by developers in a higher-level language compiled using either the **ISA/VM (2) or direct compilation (3)** approaches. 
  - Hybridization is likely because it gives you the best of both worlds - you can spend your “optimisation time” (the time of people writing hand-optimized circuits) on the critical “hot spots” of your code which really need to be optimized and whose performance mostly determines the overall performance of your program, and use a less efficient but more general compilation pipeline for custom per-program “business logic” written by regular developers.

- This is also similar to the structure of cryptographic systems today - if you look at how people implement cryptographic protocols, there’s usually hardware support (after standardization) for specific optimized primitives (hash functions, encryption algorithms), which are tied together in protocols themselves written in higher-level languages which are easier to develop in and reason about.

- Finally, a minor exhortation: agreement on and convergence to **specific standard primitives and intermediate interfaces** can help facilitate deduplication of work across the ecosystem and an easier learning environment for developers. 
  - A good example of a moderately successful standardization effort in the space already is the [IBC protocol](https://ibcprotocol.org/), which is now being adopted by many different chains and ecosystems. Anoma is trying to help a little bit in this direction with [VampIR](https://github.com/anoma/vamp-ir) proof-system-agnostic intermediate polynomial representation, and we’d be very happy to collaborate or learn about other standards efforts.
  - If you are developing other standards efforts, please consider adding to this section or let us know at research@delendum.xyz

### Programming Languages 

| Name  | Ecosystem | Type | GitHub | Documentation | 
| ------------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Cairo     | StarkNet    | STARK-provable programs for general computation  | https://github.com/starkware-libs/cairo-lang | [https://cairo-lang.org/docs/](https://cairo-lang.org/docs/) | 
| ZoKrates     | Python subset   | R1CS SNARKs Frontend | https://github.com/Zokrates/ZoKrates | [https://zokrates.github.io](https://zokrates.github.io) |
| Leo      | Aleo     | Functional, statically-typed  | https://github.com/AleoHQ/leo | [https://developer.aleo.org/developer/language/layout/](https://developer.aleo.org/developer/language/layout/) |
| Circom | Typed JS | Circuit compiler   | https://github.com/iden3/circom | [https://docs.circom.io](https://docs.circom.io) |
| Noir | Aztec | Private contract language  | https://github.com/noir-lang/noir | [https://noir-lang.github.io/book/index.html](https://noir-lang.github.io/book/index.html)
| Snarky | Mina | R1CS SNARKs OCaml frontend | [https://github.com/o1-labs/snarky](https://github.com/o1-labs/snarky) | / | 
| Zinc | zkSync | Turing-complete smart contract | [https://github.com/matter-labs/zinc](https://github.com/matter-labs/zinc) | / | 
| Juxiv | Anoma | Functional | https://github.com/anoma/juvix | [https://juvix.readthedocs.io/en/latest/index.html](https://juvix.readthedocs.io/en/latest/index.html) | 
| ZKPDL | / | High-level | https://github.com/brownie/cashlib | [http://cs.brown.edu/research/brownie/usenix10.pdf](http://cs.brown.edu/research/brownie/usenix10.pdf) |
| zkVM | / | Stack machine with a string of bytecode representing ZkVM instructions | https://github.com/stellar/slingshot/tree/main/zkvm | [https://github.com/stellar/slingshot/files/3164245/zkvm-whitepaper-2019-05-09.pdf](https://github.com/stellar/slingshot/files/3164245/zkvm-whitepaper-2019-05-09.pdf) | 
| lurk | Protocol Labs | Lurk is a statically scoped dialect of Lisp, influenced by Scheme and Common Lisp | https://github.com/lurk-lang/lurk-rs | [https://github.com/lurk-lang/lurk/blob/master/spec/v0-1.md](https://github.com/lurk-lang/lurk/blob/master/spec/v0-1.md) |

### Programming Libraries 

| Name  | Host Language | Features | GitHub |
| ------------- |:-------------:|:-------------:|:-------------:|
| Libsnark     | C++    | General-purpose proof systems, gadget libraries | [https://github.com/scipr-lab/libsnark](https://github.com/scipr-lab/libsnark) |
| Bulletproofs | Rust | Single-party proofs, online multi-party computation, R1CS  | [https://github.com/dalek-cryptography/bulletproofs](https://github.com/dalek-cryptography/bulletproofs) |
| Bellman     | Rust   | Circuit traits, primitive structures, basic gadget implementations | [https://github.com/zkcrypto/bellman](https://github.com/zkcrypto/bellman) |
| gnark      | Go     | High level API with frontend and backend to design circuits | [https://github.com/ConsenSys/gnark](https://github.com/ConsenSys/gnark) | 
| Arkworks | Rust | R1CS, curves, Groth16, finite field, curves | [https://github.com/arkworks-rs](https://github.com/arkworks-rs) | 
| Circomlib | Javascript | Circom templates | [https://github.com/iden3/circomlib](https://github.com/iden3/circomlib) |
| libSTARK | C++ | ZK-STARK library | [https://github.com/elibensasson/libSTARK](https://github.com/elibensasson/libSTARK) | 
| plonky2 | rust | SNARK implementation based on techniques from PLONK and FRI | [https://github.com/mir-protocol/plonky2](https://github.com/mir-protocol/plonky2) |
| plonk | rust | Pure Rust implementation of the PLONK ZKProof System | [https://github.com/dusk-network/plonk](https://github.com/dusk-network/plonk) |
| Spartan | rust | High-speed zkSNARKs without trusted setup | [https://github.com/microsoft/Spartan](https://github.com/microsoft/Spartan) |
| DIZK | Java | Distributed polynomial interpolation, Lagrange polynomials, multi-scalar multiplication | [https://github.com/scipr-lab/dizk](https://github.com/scipr-lab/dizk) | 
| wasmsnark | Javascript | Generate zkSnark proofs and verify from web browser | [https://github.com/iden3/wasmsnark](https://github.com/iden3/wasmsnark) | 
| jellyfish | rust | Rust Implementation of the PLONK ZKP System and Extensions | [https://github.com/EspressoSystems/jellyfish](https://github.com/EspressoSystems/jellyfish) |
| libiop | C++ | IOP-based zkSNARKs | [https://github.com/scipr-lab/libiop](https://github.com/scipr-lab/libiop) | 
| Nova | rust | Recursive SNARKs without trusted setup | [https://github.com/microsoft/Nova](https://github.com/microsoft/Nova) |

## Hardware Acceleration

### Leading Problem

- Proof generation is time-consuming, high-latency, and not efficient enough on traditional CPUs
- As L2 rollups and other applications of ZKP grow, we need to consider hardware as a potential breakthrough of performance improvement

### Business Models

- zk-mining: Plug-n-play FPGA accelerator cards (revenue through hardware sale)
- zk-Rollup: public, private and on-premise cloud (revenue through hourly premiere)
- Software auxiliary: SDK for dApps and API (license to program FPGA in developer-friendly way

### 3 Potential Scenarios

1. Permissioned networks, e.g. StarkNet 
2. Permissionless networks where provers compete on cost, e.g. Mina's Snarketplace
3. Permissionless networks where provers compete on latency, e.g. Polygon Hermes & Zero 

### Selection metrics
1. Performance: latency, throughput, and power consumption 
2. Total cost of ownership (capital expenditures and maintenance costs) 
3. NRE cost (non recurring engineering: onboarding difficulties) 
4. Long-term comparative advantage: competitor performance may double in a year with the same MSPR (e.g. Intel, Nvidia GPU) 

### Structure of ZKP Hardware Acceleration
- Current popular structure is to combine CPU and FPGA/GPU to perform proof generation. CPU tackles single-threaded pieces at higher frequency and deal with non-determinism 
- MSM and FFT can be parallelized, but arithmetization and commitments may be single threaded (different from the “embarrassingly parallel” PoW mining structure) 
- Most FPGA design tools are proprietary: FPGAs have higher barrier to enter than GPU accelerations 

## Light Client 

### Leading Problem

- Full node is resource-draining: downloading and verifying the whole chain of blocks takes time and resources
- Not retail friendly to be on mobile and perform quick actions like sending transactions and verifying balances

### Technology Highlight

- Trust-minimized interaction with full nodes and do not interact directly with the blockchain
- Much lighter on resources and storage but require higher network bandwidth
- Linear time based on chain length: verify from the root to the target height (may be from a trusted height, therefore constant length)
- Process overview:
    - retrieve a chain of headers instead of full block
    - verify the signatures of the intermediary signed headers and resolve disputes through checking with full nodes iteratively
- Superlight client: requires downloading only a logarithmic number of block headers while storing only a single block header between executions

### Commit-and-proof Schemes

| Name                                           | Mechanism                                                                     | Prover Complexity | Verifier Complexity | Trusted Setup | Reference                                                                                                                                                  |
| ---------------------------------------------- | ----------------------------------------------------------------------------- | ----------------- | ------------------- | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Recursive SNARKs                               | Cycle of elliptic curves                                                      | High              | Low                 | Yes           | [https://minaprotocol.com/wp-content/uploads/technicalWhitepaper.pdf](https://minaprotocol.com/wp-content/uploads/technicalWhitepaper.pdf)                 |
| KVC (Key-Value Commitment)                     | Constant size key-value map                                                   | Med-high          | Medium              | No            | [https://eprint.iacr.org/2020/1161.pdf](https://eprint.iacr.org/2020/1161.pdf)                                                                             |
| AMT (authenticated multipoint evaluation tree) | authenticate a polynomial multipoint evaluation at the first n roots of unity | Medium            | Medium              | No            | [https://people.csail.mit.edu/devadas/pubs/scalable\_thresh.pdf](https://people.csail.mit.edu/devadas/pubs/scalable_thresh.pdf)                            |
| KZG Polynomial Commitment                      | Elliptic curve pairings with BLS12-381                                        | High              | Low                 | Yes           | [https://www.iacr.org/archive/asiacrypt2010/6477178/6477178.pdf](https://www.iacr.org/archive/asiacrypt2010/6477178/6477178.pdf)                           |
| Verkle Tree                                    | (with anonymous revocation)                                                   | High              | Low                 | No            | [https://math.mit.edu/research/highschool/primes/materials/2018/Kuszmaul.pdf](https://math.mit.edu/research/highschool/primes/materials/2018/Kuszmaul.pdf) |
| Vector Commitment                              | RSA and computational Diffie-Hellman over bilinear groups                     | High              | Low                 | Yes           | [https://eprint.iacr.org/2011/495.pdf](https://eprint.iacr.org/2011/495.pdf)                                                                               |
| One-to-many prover VSS                         | Prover batching with sumcheck and Fiat-Shamir                                 | Medium            | Low                 | No            | [https://www.usenix.org/system/files/sec22summer\_zhang-jiaheng.pdf](https://www.usenix.org/system/files/sec22summer_zhang-jiaheng.pdf)                    |


 

## Arithmetic Fields

### Leading Problem

- Proof systems encode computation as a set of polynomial equations defined over a field. Operating on a different field will lead to gigantic circuits.
- For pairing based SNARKs, there is a limited selection of pairing-friendly curves such as BN254 and BLS12-381.
- FFTs require factors of two for the curves in order to make the polynomial computations practical.
- Proof recursions require encoding arithmetic statements of a field into equations over a different field.
- Bridge operators and roll-ups need interoperability with non-pairing friendly and not highly 2-adic curves, such as Bitcoin and Ethereum’s ECDSA over secp256k1. ([BN254 precompiled contract](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-197.md))
- Choosing the field is a tradeoff of speed and security.

### Solutions

- Brute force: decompose elements into limbs and operate on the limbs; Reduce and recompose the elements only when necessary
    - Very expensive: may represent up to 1000X the original constraints in the circuit
    - [xJsnark framework](https://akosba.github.io/papers/xjsnark.pdf)
    - [gnark implementation](https://github.com/ConsenSys/gnark/blob/3d3672148b38c548b6527c5f2cfe1af5ae61a11f/std/math/nonnative/doc.go)
- 2-chains and cycles: use matching base fields to implement one curve inside of the other (assume pairing based schemes)
    - Non-pairing based scheme can use non-pairing friendly or hybrid cycles at a cost, such as using PCS (polynomial commitment scheme) or pasta curves with linear time.
    - [Pasta Curves](https://electriccoin.co/blog/the-pasta-curves-for-halo-2-and-beyond/)
    - [2-chains of elliptic curves](https://eprint.iacr.org/2021/1359.pdf)
- Lower the requirements on the field: Polynomial Commitment Scheme (PCS) may not involve elliptic curves at all to be instantiated on arbitrary fields

### Reference Reading

- [A survey of elliptic curves for proof systems](https://eprint.iacr.org/2022/586.pdf)
- [ECFFT: Fast Polynomial Algorithms over all Finite Fields](https://arxiv.org/abs/2107.08473)

## Efficient Signatures 

### Leading Problems

- Signature verification is a problem that arises in many ZK related projects, from Zcash to zk-rollups
- Different types of signatures depend on different field or curve arithmetic and need different speed-up methods
- Here we will discuss some different options for signature schemes, and strategies for efficient implementations

#### Specialized ZK signatures

- Given a ZKP system, a simple signature scheme can be constructed as follows. The signer generates a random secret key sk, and hashes it to get the associated public key pk. They then prove, in zero knowledge, that they know some sk such that hash(sk) = pk. [Picnic](https://microsoft.github.io/Picnic/) is a variant of this idea, using a block cipher rather than a hash function
- Since the statement being proved involves just a single evaluation of a hash or block cipher, these schemes can be highly efficient, particularly if using arithmetic-friendly primitives such as LowMC or Poseidon
- The caveat here is that the proof must be generated by the user who owns the secret key. If the signature is part of a larger circuit, such as Zcash’s spend circuit, this may be undesirable, as a user cannot outsource proving to another entity without revealing their secret key to that entity

#### Native curves

- If we wish to support delegated proving, it may be best to use a conventional signature scheme such as ECDSA, and include its verification steps in our circuit. This way the user can generate just a signature, and a separate prover can prove that they witnessed a valid signature
- To make this efficient, we can choose an elliptic curve such that the base field of the curve matches the “native” field of our argument system. An example of this is the [Baby Jubjub](https://eips.ethereum.org/EIPS/eip-2494) curve, whose base field matches the scalar field of alt_bn128. If we use an argument system such as Groth16 with alt_bn128 as our curve, Baby Jubjub arithmetic can be done “natively”
- However, using a curve like Baby Jubjub isn’t always an option. Sometimes we need compatibility with existing tools which support conventional curves. Polygon Zero, for example, must support Ethereum’s existing signature scheme, which is ECDSA over secp256k1. In these cases, we must simulate arithmetic in another field using bignum techniques

#### Bignum techniques

- For example, if we use standard multiplication algorithms for ED25519 field multiplication and range check each of the 5x5 limb products, we can reduce the cost by:
- Since 2^255 = 19 mod p_ed25519, you could combine limb products with the same weight mod 255 (in bits). The combined products will have a few more bits but it should be cheaper overall
- If we split each intermediate product into bits and use binary adders, we can minimize the number of splits by adding intermediate products with the same weight "natively" first. For example, if two intermediate products have a weight of 2^51 (the base), we can split the sum into 52 bits, which is just over half the cost of doing two 51-bit splits. When we have an intermediate product with a weight of 2^255 or more, we can change its weight to 1 and multiply it by 19, since 2^255 = 19 mod p_ed25519. That could help to further reduce the number of splits

##### CRT inspired methods

- Alternatively, there's the Aztec approach of having the prover give a purported product, then checking the identity mod p_native (almost free) and then mod, say, 2^306 (can ignore limb products with weights of 306+)
- We're going to publish a couple other methods soon, which should be cheaper than the ones above

##### Other tricks

- These methods can be generalized to arbitrary low-degree formulas rather than simple multiplications. Checking a whole elliptic curve formula directly can be much more efficient
- Finally, we would expect the cost to come down significantly if you used a PLONK or STARK implementation that supported lookup-based range checks, instead of range checking via binary decomposition

#### Edge cases

- We can use incomplete formulae if we’re careful

#### Conventional optimizations

- In the scalar multiplication code, we would suggest looking into windowing
- If we’re checking an ECDSA or EdDSA signature, we can leverage the fact that one of the multiplications has a fixed base, enabling various preprocessing tricks
- Curve-specific: secp256k1 supports the [GLV endomorphism](https://link.springer.com/chapter/10.1007/3-540-44647-8_11), for example, which allows us to split a curve multiplication into two smaller ones, opening the door to multi-scalar multiplication methods such as Yao’s

### Suggestions

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

### Leading Problem

When comparing zk-SNARKs, we are usually primarily interested in three
different metrics: prover time, verifier time, and proof length -
some may also want to add quantum security and setup trust assumptions
to that list.

Within the context of blockchains, there is one further criterion which
I would argue to be as important as those three, and we'll give it the
clunky name *aggregation-friendliness*.

A zk-SNARK with universal setup can be said to be aggregation-friendly
if (informally) there exists some m so that it is easy to merge m proofs
$$P_{1},...,P_{m}$$ into one proof $$P_{1,...,m}$$ so that $$P_{1,...,m}$$ is
not much bigger than either $$P_{i}$$, and certainly much smaller than the
two of them together. Specifically there should exist a deterministic
polynomial-time algorithm $$\mathfrak{A}$$ (which we call the aggregator)
and $$\mathfrak{V}_{\mathfrak{A}}$$ (which we call the aggregation
verifier) such that for any $$P_{1},...,P_{m}$$:

-   The aggregation time $$|\mathfrak{A}(P_{1},...,P_{m})|$$ is
    (quasi-)linear in m

-   The proof size $$|P_{1,...,m}|$$ is sublinear in m and quasilinear in
    $$\max_{i}(\|P_{i}\|)$$

-   $$|\mathfrak{V}_{\mathfrak{A}}(P_{1,...,m})|$$ is quasilinear in
    $$|\mathfrak{V}({argmax}_{i}(|P_{i}|))|$$, where
    $$\mathfrak{V}$$ is the verifier of the given zk-SNARK

So why is aggregation-friendliness so important to us? In blockchains,
especially for scalability purposes, SNARKs are used to prove the
validity of a bunch of statements at once. Now, if we want to prove a
block of n transactions of, say for simplicity, the same size m, with a
single zk-SNARK, the prover time will typically be O($$nm\log nm$$). Often
both proof length and verifier time are also non-constant in n. Bigger n
therefore leads to significantly bigger costs, and furthermore, this
doesn't really scale with hardware parallelization or multiple provers.

On the other hand, imagine if the same proof system were
aggregation-friendly, and let us for now ignore network limitations. You
could have n different provers, each proving one transaction in time
O($$m\log m$$), and then aggregating them in time sublinear in n,
therefore generating a block proof much more quickly. Hopefully the
resulting proof would also not be much bigger than the original one or
much more difficult to verify.

To hammer the point home, assume we have two schemes,
$$S_{1}:=(\mathcal{P}_{1}, \mathcal{V}_{1}), S_{1}:=(\mathcal{P}_{2}, \mathcal{V}_{2})$$.
$$\mathcal{P}_{1}$$ is more efficient that $$\mathcal{P}_{2}$$, with the
respective proving times for a statement of length m being
$$t_{1} = 5m\log_{2}m, t_{2} = 10m\log^{2}_{2}m$$. However, while $$S_{2}$$
is aggregation-friendly, $$S_{1}$$ is not. There exists an aggregator
$$\mathcal{A}$$ for $$S_{2}$$ which can aggregate n proofs of a length-m
statement in time $$4\log^{2}_{2}n$$. In total, the proving time for
$$P_{1}$$ will be $$5nm\log_{2}nm$$. On the other hand, we can run $$P_{2}$$
in parallel for each of the statements, and then run $$\mathcal{A}$$ to
aggregate them into a single proof, for a total time of

$$10m\log^{2}_{2}m\cdot 4\log^{2}_{2}n = 40m\log^{2}_{2}m\log^{2}_{2}n$$

Let's look at how $$t_{1}$$ and $$t_{2}$$ compare for some different values
of n and m:

-   For n = 2, m = 2, we have $$t_{1}=40, t_{2}=80$$

-   For n = 8, m = 4, we have $$t_{1} = 800, t_{2} = 5760$$

-   For n = 4096, m = 16, we have
    $$t_{1} = 5242880, t_{2} = 1474560 \simeq 0.28\cdot t_{1}$$

Here we see that, although $$\mathcal{P}_{1}$$ performs significantly
better than $$\mathcal{P}_{2}+\mathcal{A}$$ for small n, it's a very
different story for big n, and should make it clear why
aggregation-friendliness is important.

This post will focus on some different approaches to zk-SNARK proof
aggregation. At a high level, there are three axes which divide them
into categories:

-   **Recursive/Batching**: Recursive proofs are proofs of proofs. We
    refer to non-recursive aggregation methods as \"batching\"

-   **Sequential/Parallel**: Can the scheme be executed in parallel?

-   **Algebraic/Arithmetization-based**: This distinction will become
    clear once we look at some examples.

### Popular zk-SNARK Aggregation Constructions

#### Halo

-   Recursive

-   Sequential

-   Algebraic

As so much of cryptography, zero-knowledge and otherwise, Halo is based
on elliptic curves. Specifically, it uses a polynomial commitment scheme
(PCS), which was previously used in another well-known zk-SNARK scheme,
namely [Bulletproofs](https://eprint.iacr.org/2017/1066.pdf): Given an elliptic curve E (where we write
$$E(\mathbb{F})$$ for the group defined by the points on E in the field
$$\mathbb{F}$$ over which we work) and random points
$$P_{i}\in E(\mathbb{F})$$, one can commit to a polynomial p(x) =
$$\sum_{i}a_{i}x^{i}$$ with the element $$\sum_{i}a_{i}\cdot P_{i}$$, where
$$\cdot$$ denotes scalar multiplication in the additive group
$$E(\mathbb{F})$$. This commitment is also often written as
$$<\textbf{a},\textbf{P}>$$ as if it were an inner product.

The \"inner product argument\" (IPA) is a protocol between the prover
and the verifier where the prover tries to convince the verifier that he
knows the polynomial corresponding to his commitment. Roughly speaking,
he does this by iteratively chopping his tuples **a** and **P** in two
equally long tuples and then putting them together so as to get two new
tuples of half the length. Eventually, after a logarithmic number of
rounds, the prover is left with a single equation, where, almost if and
only if he has acted honestly throughout will he be able to satisfy the
verifier's final challenge.

The problem is that, although the number of rounds is nice and small,
the verifier still needs to compute two \"inner products\" of
\"vectors\" of the original length. One may think that this ruins the
protocol: the verifier's work is now linear rather than logarithmic.
However, this is where the recursive proof composition (aggregation)
comes in:

The aggregation technique largely relies on an ad hoc polynomial
g$$_{u_{1},...,u_{k}}$$(x), which is defined in terms of the verifier's
challenges $$u_{i}$$ to the prover in the IPA. As it turns out,
g$$_{u_{1},...,u_{k}}$$(x) can be used to compute both of the \"inner
products\", where we will focus on one of them: At the end of the IPA,
the prover has a single elliptic curve point P. As it turns out, by the
construction of g$$_{u_{1},...,u_{k}}$$(x), the commitment to it defined
by the above scheme yields exactly P! The other \"inner product\" can
also be defined as an evaluation of g$$_{u_{1},...,u_{k}}$$(x). This does
not help us directly, as evaluating the polynomial still takes linear
time, however, the prover can now create such polynomials for many
statements, commit to them, and then at the end prove that a linear
combination of them corresponds to a desired value - this uses a
property of the PCS which we have not mentioned thus far, namely that
it's additively homomorphic (specifically, it's based on Pedersen
commitments).

To recap, the verifier previously had to do a linear-time operation for
each statement. By invoking a certain polynomial with some nice
properties, the prover can defer the final opening proof until he has
computed and committed to arbitrarily many such polynomials. Halo people
often talk about the \"inner\" and the \"outer\" circuit to reflect the
distinction between the point of view of the prover (who finishes most
of each proof before computing an aggregated proof for all together) and
the verifier (for whom the proof really boils down to the linear-time
computation she has to perform).

For specifics, we refer to the original [Halo paper](https://eprint.iacr.org/2019/1021). The Halo approach is clearly algebraic in its nature: It depends on the nice homomorphic
properties of Pedersen commitments, as well as a particular polynomial.
Moreover, it is sequential: The prover iteratively adds commitments
$$G_{1}, G_{2},...$$ to his inner proof, before opening a linear
combination of them at the end of the protocol. Finally, it is
recursion-based: The prover proves the validity of the previous inner
proof to the current one.

Halo 2 is used by several protocols, and its main selling point is the
aggregation property. Notably, Mina protocol uses Halo to create a
constant-sized blockchain, in the sense that the validity of the current
state is proven simply by verifying the validity of the proof of the
previous state together with the transactions in the last block.
Finally, a proof is computed, representing everything that has happened
up to that state.

Electric Coin Company, the developer of the scheme, recently
incorporated Halo 2 as part of their upgrade to the Orchard shielded
pools.

Halo 2 is also used by the zkEVM rollup project Scroll, in order to
aggregate multiple blocks into one final proof and thereby amortize the
L1 verification costs between them.

#### Plonky2

-   Recursive

-   Batching

-   Arithmetization-based

Plonky2 emphasizes the modular nature of zk-SNARKs:
We need some arithmetization scheme, a PCS, a zero test, and a
consistency check between the arithmetization polynomials and the
constraint polynomial.

The goal of the scheme is to make recursive proving extremely fast, i.e.
for two (or more) Plonky2 proofs, proving the validity of them with a
single proof should be a small computation.

As an aside, why not simply use PLONK? While an elliptic curve E defined
over some finite field $$\mathbb{F}_{q}$$ can have q points over
$$\mathbb{F}_{q}$$, those curve/field combinations are not appropriate
within the setting of cryptography. Instead, say E($$\mathbb{F}_{q})$$ has
r elements for some prime r. Then the arithmetic of the group is defined
over $$\mathbb{F}_{r}$$, a different field altogether. This means that for
a proving scheme for $$\mathbb{F}_{q}$$-arithmetic satisfiability, the
verifier has to perform arithmetic over $$\mathbb{F}_{r}$$. This means
that the recursive proof must use a circuit defined over
$$\mathbb{F}_{r}$$, the recursive proof of that a different field, and so
on. Halo in fact heavily relies on the existence of 2-cycles of curves,
namely tuples (E,q), (E',r) such that q and r are primes and
$$\#E(\mathbb{F}_{q})=r, \#E'(\mathbb{F}_{r})=q$$.

Back to Plonky2. Because of the aforementioned issues with elliptic
curves, the scheme essentially takes advantage of the flexibility of the
so-called Plonkish arithmetization, while using a PCS which doesn't rely
on elliptic curves. Specifically, Plonky2 replaces the KZG-based proof
with FRI. We will only briefly describe FRI here:

For any univariate polynomial f(x) $$\in \mathbb{F}[x]$$, we can consider
the polynomials $$f_{even}$$(x) and $$f_{odd}(x)$$ defined by the
coefficients of the even and odd powers of x in f(x), respectively. Note
also that the degree of both of those polynomials is at most half the
degree of f and that $$f(x) = f_{even}(x^{2}) + x\cdot f_{odd}(x^{2})$$

Roughly speaking, we can iteratively commit to polynomials\
$$f(x) = f^{(0)}(x),...,f^{(\log deg f)}(x)$$ such that deg $$f^{(i)} \leq$$
deg $$f^{(i+1)}/2$$ by evaluating them over certain appropriate domains
and Merkle-committing to the evaluations.

For the verifier, and hence the construction of recursive proofs, the
main computational task lies in computing hashes to verify Merkle paths.
The high-level idea is that while for vanilla PLONK complex arithmetic
expressions (such as hash functions) lead to deep circuits, it's
possible to implement custom arithmetic gates which themselves compute
more complex expressions than addition and multiplication.

Plonky2 implements custom gates which ensure that the verifier circuit
is shallow, as well as some other engineering optimizations, in order to
make recursive proving as fast as possible. Plonky2 was created by the formerly Mir Protocol, now Polygon Zero team. 

#### Nova

-   Recursive

-   Sequential

-   Arithmetization-based + Algebraic

Unlike the two preceding schemes, Nova is not universal; it can only be
used to incrementally verify computations $$y^{(i)}=f^{(i)}(x)$$. The
authors themselves consider it as a \"Halo taken to the extreme\".
Unlike Halo, each step of Nova does not output a zk-SNARK, but a more
minimal construction, and the authors have dubbed this technique
incrementally verifiable computation (IVC). Nova is indeed faster than
Halo, but this also has the consequence that not any arbitrary output is
suitable within every context (as is the case for Halo), and the idea of
IVC is really, as the name suggests, to prove the validity of n
repetitions of the same computation on the same input and not care about
any individual step i $$<$$ n.

Nova uses a modified R1CS arithmetization which allows the prover to
\"fold\" two instances (one representing the recursive \"proof\", one
representing the current step of the computation) into one. The prover
ultimately proves the validity of the final folded R1CS instance with a
zk-SNARK, which, thanks to the properties of the IVC scheme, proves the
validity of all n invocations of f.

Although not appropriate for most applications we're interested in
within a blockchain context, we have included Nova because the idea of
IVC is nevertheless interesting: Take some half-baked proofs, which by
themselves are neither zero-knowledge nor succinct, then aggregate and
construct a single proof at the end.

#### SnarkPack

-   Batching

-   Parallel

-   Algebraic

The schemes we have seen up to this point have been zk-SNARKs optimized
for aggregation, i.e. the aggregation techniques are essentially baked
into the respective protocols. [SnarkPack](https://eprint.iacr.org/2021/529) takes a different approach: it is
constructed as an addition to an already existing zk-SNARK, namely
Groth16. In this sense, SnarkPack is the first (and only) true
*aggregation scheme* covered here.

Groth16 uses the popular KZG polynomial commitment scheme, and the
verification of the proof includes checking an equation of the form
$$e(A,B) = Y + e(C,D)$$ for an elliptic curve pairing
$$e: E_{1}(\mathbb{F})\times E_{2}(\mathbb{F})\rightarrow E_{pairing}(\mathbb{F})$$
and points\
$$A,C\in E_{1}(\mathbb{F}), B,D \in E_{2}(\mathbb{F})$$, and
$$Y\in E_{pairing}(\mathbb{F})$$ ($$E(\mathbb{F})$$ here denotes the group
of points (x,y) on the elliptic curve E with $$x,y\in \mathbb{F}$$). An
elliptic curve is a bilinear non-degenerate map, where the bilinearity
is most important to us. To see why, imagine for a moment Y=0,
$$A_{1}, A_{2}, C_{1}, C_{2}\in E_{1}(\mathbb{F})$$, and we want to check
the validity of the equation $$e(A_{i},B) = e(C_{i},D), i=1,2$$ 

One way of verifying this equation is to pick a random r $$\in\mathbb{F}$$ and
check 

$$\label{eq:ec_1}
    e(A_{1},B) + re(A_{2},B) = e(C_{1},D) + re(C_{2},D)$$ 
    
Now, bilinearity of e implies that the left side of the equation can be
rewritten as $$e(A_{1}+rA_{2},B)$$ and likewise for the right side, so
that we have reduced to $$e(A_{1}+rA_{2},B) = e(C_{1}+rC_{2},D)$$
Importantly, we only have to compute two elliptic curve pairings rather
than four (pairings are expensive).

For m proofs we can imagine the same approach yielding even greater
benefits. Now, the above toy example does not fully apply to Groth16; in
particular Y cannot be assumed to be 0, and B does not remain constant
for different i.

For m Groth16 proofs $$\pi_{i}=(A_{i},B_{i},C_{i})$$, the SnarkPack
aggregation scheme computes $$Y_{i}=Y(\pi_{i})$$ as according to Groth16,
and samples the random element r as in our toy example. 

Denote by

$$C^{(m)} := \sum_{i=0}^{m-1}r^{i}C_{i}{\;and\;} Y^{(m)} := \sum_{i=0}^{m-1}r^{i}Y_{i}.$$

The aggregated equation to be checked is of the form

$$\sum_{i=0}^{m-1}r^{i}e(A_{i},B_{i}) = Y^{(m)}+C^{(m)}$$ 

This looks very linear, and therefore not all that nice, however, the prover can
use the IPA technique also used in Halo, in order to ensure that the
proof size as well as the verifier time are logarithmic in m.

### Some Potentially Interesting Approaches

#### PlonkPack

-   Batching

-   Parallel

-   Algebraic

**Since writing this section, We have come across the brand new paper
[aPlonk](https://eprint.iacr.org/2022/1352.pdf), which is essentially SnarkPack for PLONK. Nevertheless, the idea here is slightly different, and so it should still be worth exploring.**

SnarkPack is Groth16-based, and therefore not relevant for applications
relying on universal SNARKs, such as blockchain execution layers. There
are some problems with adopting that kind of approach to KZG-based
universal zk-SNARKs: For one, most of the ones we have are IOPs,
in practice meaning that they rely on random challenges from the
verifier. Each proof will have different challenges contained within it,
and this is a problem for aggregation schemes relying on structural
properties of e.g. the elliptic curve pairing in KZG.

That said, we can think of aggregation within different contexts:

1.  In SnarkPack, one can simply take n proofs and aggregate them.

    Note: for SnarkPack, specifically, the circuit has to be the same
    for all because Groth16 is non-universal. We are focused on schemes
    for aggregating proofs of arbitrary circuits into one.

2.  One might imagine a synchronous scheme where n provers with n
    statements to prove, build an aggregated proof together. After each
    prover has completed a given round, the verifier sends the same
    random challenge to all of them

(1) is the SnarkPack approach, and it is ideal in the sense that a
scheme like this offers the most flexibility and universality.

(2) will generally entail significant communication costs, however we can
imagine each prover simply as a core on a GPU. In this case, the
approach may well make sense (this is the approach taken by the
aPlonk authors as well). This is also very much relevant to today's
problems, as all current zk-rollups have one prover per block, i.e.
there's not a decentralized network of provers each proving the validity
of individual transactions, and there likely won't be for a long time.

For an approach based on homomorphism similar to SnarkPack, then, it's
natural to look at PLONK, which within our framework can be seen as a
universal counterpart to Groth16 - the main innovation of PLONK
arguably lays in its arithmetization, which we consider to be irrelevant
for this kind of aggregation scheme.

As we have seen, in PLONK, the goal is to prove the validity of some
equation of the form: $$P(x) \equiv 0 { mod } Z_{H}(x)$$ For
m such polynomials $$P_{1}(x),...,P_{m}(x)$$. For linearly independent
elements $$\alpha_{i}$$, roughly speaking, for each $$P_{i}(x)$$ the
corresponding such equation holds if and only if:
$$\sum_{i}\alpha_{i}P_{i}(x)\equiv 0 { mod } Z_{H}(x)$$ In
practice it's not that easy, as we do not get these nice, linear
relations. Specifically, for vanilla PLONK, the equation to be proved is
of the form: 

$$\label{eq:plonk}
    \omega_{L}(x)q_{L}(x) + \omega_{R}(x)q_{R}(x) \\ + \omega_{L}(x)\omega_{R}(x)q_{M}(x) + \omega_{O}(x)q_{O}(x) + q_{C}(x)\equiv 0 {\,mod\,} Z_{H}(x)$$

where the $$q_{I}$$(x) define the circuit and the $$\omega_{J}$$(x)
constitute the witness. Assuming, then, that all those polynomials are
defined such that we have $$\alpha_{i}q_{I,i}(x)$$ and
$$\alpha_{i}\omega_{J,i}(x)$$, the aggregated polynomial is no longer
equal to 0 modulo the vanishing polynomial. Specifically, the left side
of the previous equation now looks like 

$$\begin{split}
    \alpha_{i}\omega_{L,i}(x)\alpha_{i}q_{L,i}(x) + \alpha_{i}\omega_{R,i}(x)\alpha_{i}q_{R,i}(x) \\ +\, \alpha_{i}\omega_{L,i}(x)\alpha_{i}\omega_{R,i}(x)\alpha_{i}q_{M,i}(x) + \alpha_{i}\omega_{O,i}(x)\alpha_{i}q_{O,i}(x) + \alpha_{i}q_{C,i}(x) = \\
    \alpha_{i}^{2}\omega_{L,i}(x)q_{L,i}(x) + \alpha_{i}^{2}\omega_{R,i}(x)q_{R,i}(x) \\ +\, \alpha_{i}^{3}\omega_{L,i}(x)\omega_{R,i}(x)q_{M,i}(x) +
    \alpha_{i}^{2}\omega_{O,i}(x)q_{O,i}(x) + \alpha_{i}q_{C,i}(x)
    \end{split}$$ 
    
where the problem is that we have different powers of
$$\alpha_{i}$$ for the different terms. This is likely not an unsolvable
problem; for example, the \"circuit aggregator\" could simply multiply
the various $$q_{I,i}(x)$$ with powers of $$\alpha_{i}$$, so that this
ultimately ends up as an equation of the form

$$\sum_{i}\alpha_{i}^{k}P_{i}(x)\equiv 0\, { mod }\, Z_{H}(x)$$
for some k $$>$$ 1.

This does raise another issue, namely that the set $$\{a_{i}^{k}\}_{i,k}$$
cannot be assumed to be linearly independent. Setting
$$\alpha_{i}:=L_{i}(y)$$ for the Lagrange polynomials $$L_{i}(y)$$ defined
on some set of size m easily solves this, as the set
$$\{L_{i}^{k}(y)\}_{i,k}$$ is linearly independent. It might also provide
some geometric intuition.

We are fairly certain that this approach will work with some tweaking. Of course, PLONK is not as simple as proving this one equation directly, and one does encounter
some issues down the road.

#### Multilinear Polynomials

-   Recursive

-   Parallel

-   Arithmetization-based

We observe that multilinear polynomials will become increasingly
popular for arithmetization, in particular with the recent release of
[HyperPlonk](https://eprint.iacr.org/2022/1355), which has some exciting properties such as the possibility for high-degree custom gates. The main benefit of multilinear polynomials is the O(n) interpolation, as
opposed to O(n$$\log$$n) for univariate polynomials. We interpolate over
the Boolean hypercube $$\{0,1\}^{t}$$, and for a function
$$f:\{0,1\}^{t}\rightarrow \mathbb{F}$$, its multilinear extension is
defined as:


$${f}(\textbf{x}) = \sum_{\textbf{w}\in\{0,1\}^{t}}f(\textbf{w})\cdot {Eq}(\textbf{x}, \textbf{w})$$


where for two length-t vectors **x**,
**y**:

$${Eq}(\textbf{x},\textbf{y}) = \prod_{i=1}^{t}(x_{i}y_{i}-(1-x_{i})(1-y_{i}))$$

We see that if x and y are binary, Eq(x,y) = 1 iff x
= y, 0 otherwise, hence the name.

There is an isomorphism between the additive group of all univariate
polynomials of max degree $$2^{t}-1$$ with coefficients in $$\mathbb{F}$$
and the group of all multilinear polynomials with t variables. We call
this the *power-to-subscript* map, and it sends a power $$x^{k}$$ to the
monomial $$x_{0}^{i_{0}}...x_{t-1}^{i_{t-1}}$$, where $$i_{0}...i_{t-1}$$ is
the binary decomposition of k. The inverse is

$$x_{0}^{i_{0}}...x_{t-1}^{i_{t-1}}\mapsto x^{i_{0}\cdot 2^{0}}...x^{i_{t-1}\cdot2^{t-1}}$$.

**Multilinear to univariate interpolation** 

This raises one (not directly related to aggregation) question: Is it possible to do O(n)
univariate interpolation by first doing multilinear interpolation over
the Boolean hypercube and then choosing a fitting map to turn into a
univariate polynomial? At this point, we may think that the answer is
obviously a positive one, since we have the power-to-subscript map and
its inverse, however that does not work; evaluations on the hypercube
are not mapped to the corresponding evaluations in $$\mathbb{F}$$ (Thanks
to Benedikt Bünz for pointing this out).

Sadly, there is reason to think this doesn't exist. First of all,
someone would have probably already thought of this, since O(n)
univariate interpolation would be a massive deal.

More concretely, the missing piece to our puzzle is the so-called
pullback 

$$\varphi^{*}$$ for $$\varphi: \mathbb{F}\rightarrow\{0,1\}^{t}, n = \sum_{j} i_{j}2^{j}\mapsto (i_{0},...,i_{t-1})$$

For $$f(x_{0},...,x_{t-1})\in\mathbb{F}[x_{0},...,x_{t-1}]$$,
$$\varphi^{*}f(x)$$ is defined as f($$\varphi(x))$$, so $$\varphi^{*}f$$ is
exactly the univariate polynomial we'd want to map f to.

The problem, then, is to find a closed form of the map $$\varphi$$. If we
can do this, we have solved the problem. More likely it does not exist.

**Better recursive proofs** 

In HyperPlonk, the authors use KZG commitments. For a Plonky-like approach, we would like something hash-based such as FRI. There does in fact exist a FRI analogue for
multivariate polynomials. The problem occurs where we need to check the consistency of the witness with the final prover message of the sumcheck protocol.

Specifically, the prover uses sumcheck in order to check that an
equation of the kind $$\sum_{\textbf{x}\in\{0,1\}^{t}}P(\textbf{x}) = 0$$,
where $$P(\textbf{x})=C(\omega(\textbf{x}))$$ for the constraint
polynomial C and the witness $$\omega$$ (we'll treat $$\omega$$ as a single
polynomial for simplicity). At the end of the protocol he sends a field
element $$m_{final}$$ which should correspond to P($$r_{1},...,r_{t})$$ for
random verifier challenges $$r_{i}$$.

The issue here is that a Merkle commitment corresponding to the values
of $$\omega$$ evaluated over, say, $$\{0,1\}^{t}$$, cannot simply be opened
at $$(r_{1},...,r_{t})$$. Actually, this is not quite true; one of the
nice facts about multilinear polynomials is that if we have the
evaluations of $$\omega$$ over $$\{0,1\}^{t}$$ we can use this to compute
$$\omega(r_{1},...,r_{t})$$ for any random such point. The drawback is
that this takes $$O(2^{t})$$ time, so it's linear in the witness size. It
also likely requires access to nearly all the leaves of the tree, which
kind of defeats the point.

This leaves us with a few open questions:

1.  Is there an efficient way for the prover to prove that he has
    computed $$\omega(r_{1},...,r_{t})$$ correctly, for example using the
    formula mentioned? (see for details)

2.  If not, is there some other recursion-friendly PCS which could be
    used?

As it turns out, we have likely found a solution to 1 (see [this
video](https://www.youtube.com/watch?v=tv5-gFgQWr0)).

This seems to be a promising direction, due to the linear proving time
and the increased flexibility with respect to custom gates. It also
ought to be mentioned that the use of the sumcheck protocol adds a
logarithmic term to the proof, which is more annoying for KZG-based than
FRI-based proofs, as the latter are already polylogarithmic (bigger than
the sumcheck part) whereas the former are constant-sized (much smaller)
by default.

### Reference Reading

- [Proofs for Inner Pairing Products and Applications](https://eprint.iacr.org/2019/1177)
- [SnarkPack: Practical SNARK Aggregation](https://eprint.iacr.org/2021/529)
- [Recursive Proof Composition without a Trusted Setup](https://eprint.iacr.org/2019/1021)
- [Fractal: Post-Quantum and Transparent Recursive Proofs from Holography](https://eprint.iacr.org/2019/1076)
- [Fast Recursive Arguments with PLONK and FRI](https://github.com/mir-protocol/plonky2/blob/main/plonky2/plonky2.pdf) 

## Vulnerabilities

- Zero-knowledge proof systems require the following properties to be defined as a zero-knowledge proof:
  - Completeness: If a statement is true, an honest verifier will be convinced of this by an honest prover
  - Soundness: If a statement is false, then there is a negligible probably that a cheating prover can prove the validity of the statement to an honest verifier
  - Zero-knowledge property: If a statement is true, then the verifier learns nothing about the statement other than the fact that the statement is true

- As such, if one of the above properties are broken, we no longer have a valid zero-knowledge proof system
- This section organizes known vulnerabilities in implementations of zero-knowledge proof systems by the above properties and an additional section mainly pertaining to more general cryptographic vulnerabilities

### Vulnerabilities that break completeness

- Correctness of G_k, generated in the ownership proof, is not enforced in the balance proof in Lelantus
  - [Lelantus Cryptographic Audit](https://firo.org/about/research/papers/lelantus-cryptography-audit-abdk.pdf)

### Vulnerabilities that break soundness

#### The Fiat-Shamir transformation

- Trail of Bits is publicly disclosing critical vulnerabilities that break the soundness of multiple implementations of zero-knowledge proof systems, including PlonK and Bulletproofs
- These vulnerabilities are caused by insecure implementations of the Fiat-Shamir transformation that allow malicious users to forge proofs for random statements
- The vulnerabilities in one of these proof systems, Bulletproofs, stem from a mistake in the [original academic paper](https://eprint.iacr.org/2019/953.pdf), in which the authors recommend an insecure Fiat-Shamir generation
- Addendum: Challenges should also be generated in such a way they are independently random.
   - See [Fiat-Shamir challenges in range_prover are not always independently random](https://github.com/firoorg/firo/issues/890)

##### Affected Parties

- The following repositories were affected:
  - [ZenGo’s zk-paillier](https://github.com/ZenGo-X/zk-paillier)
  - [SECBIT Labs’ ckb-zkp](https://github.com/sec-bit/ckb-zkp)
  - [Adjoint, Inc.’s bulletproofs](https://github.com/adjoint-io/bulletproofs)
  - [Dusk Network’s plonk](https://github.com/dusk-network/plonk)
  - [Iden3’s SnarkJS](https://github.com/iden3/snarkjs)
  - [ConsenSys’ gnark](https://github.com/ConsenSys/gnark)

##### Solution

- The Fiat-Shamir hash computation must include all public values from the zero-knowledge proof statement and all public values computed in the proof (i.e., all random “commitment” values)

##### Reference Reading

- [Serving up zero-knowledge proofs](https://blog.trailofbits.com/2021/02/19/serving-up-zero-knowledge-proofs/)
- [The Frozen Heart vulnerability in PlonK](https://blog.trailofbits.com/2022/04/18/the-frozen-heart-vulnerability-in-plonk/)
- [The Frozen Heart vulnerability in Bulletproofs](https://blog.trailofbits.com/2022/04/15/the-frozen-heart-vulnerability-in-bulletproofs/)
- [The Frozen Heart vulnerability in Girault’s proof of knowledge](https://blog.trailofbits.com/2022/04/14/the-frozen-heart-vulnerability-in-giraults-proof-of-knowledge/)
- [Coordinated disclosure of vulnerabilities affecting Girault, Bulletproofs, and PlonK](https://blog.trailofbits.com/2022/04/13/part-1-coordinated-disclosure-of-vulnerabilities-affecting-girault-bulletproofs-and-plonk/)

#### Creating Fake ZK-SNARK proofs

- In certain ZK-SNARK protocols, a trusted setup ceremony is devised in order to produce parameters for use in the proof generation of a particular statement
- However, there are extra parameters, deemed as toxic waste, that meant to be destroyed after the ceremony has been performed. If  the toxic waste is not properly disposed of, a cheating prover can generate fake proofs and mislead and honest verifier

##### Reference Reading

- [Creating fake ZK-SNARK proofs](https://medium.com/qed-it/how-toxic-is-the-waste-in-a-zksnark-trusted-setup-9b250d59bdb4)
- [Zcash Counterfeit Vulnerability](https://nvd.nist.gov/vuln/detail/cve-2019-7167)

### Vulnerabilities that break the zero-knowledge property

#### Honest verifier zero-knowledge proof

- Honest verifier zero-knowledge proofs (HVZKP) assume an honest verifier. This means that in the presence of malicious verifiers, non-interactive protocols should always be used 
- These also exchange fewer messages between prover and verifier. A malicious verifier can employ different attacks depending on the proof system

##### Reference Reading

- [Using HVZKP in the wrong context](https://www.zkdocs.com/docs/zkdocs/security-of-zkps/when-to-use-hvzk/)
- [UC non-interactive, proactive, threshold ECDSA with identifiable aborts (2020)](https://eprint.iacr.org/2021/060.pdf)

### General Vulnerabilities affecting zero-knowledge enabled systems

#### Reference Reading

- [Disclosure of recent vulnerabilities in Aztec 2.0](https://hackmd.io/@aztec-network/disclosure-of-recent-vulnerabilities)
- [SELFDESTRUCT main via delegatecall in ZkSync](https://docs.zksync.io/dev/security/ZKSYNC1-2021-01/)


## Licensing

| License                              | Permissions                                                         | Conditions                                                                                                                     | Projects                                                                                                                                                                                                                                                                                                    |
| ------------------------------------ | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MIT                                  | Commercial use, distribution, modification, private use             | License and copyright notice                                                                                                   | [Scroll](https://github.com/michaelrhodes/scroll/blob/master/LICENSE), [Libsnark](https://github.com/scipr-lab/libsnark/blob/master/LICENSE), [Plonky2](https://github.com/mir-protocol/plonky2/tree/main/insertion)                                                                                                                                                                |
| GNU GPLv3                            | Commercial use, distribution, modification, patent use, private use | Disclose source, license and copyright notice, state changes                                                                   | [Aleo](https://github.com/AleoHQ/aleo/blob/main/LICENSE.md), [Tornado Cash](https://github.com/tornadocash/tornado-core/blob/master/LICENSE). [Aztec](https://github.com/AztecProtocol/AZTEC/blob/develop/LICENSE)                                                                                          |
| Mozilla Public License               | Commercial use, distribution, modification, patent use, private use | Disclose source, license and copyright notice                                                                                  |   /                                                                                                                                                                                                                                                                                                          |
| Apache License                       | Commercial use, distribution, modification, patent use, private use | License and copyright notice, state changes                                                                                    | [O(1) Labs](https://github.com/o1-labs/proof-systems/blob/master/LICENSE), [StarkEx](https://github.com/starkware-libs/starkex-contracts/blob/master/LICENSE), [Halo2](https://github.com/zcash/halo2/blob/main/LICENSE-APACHE), [zkSync](https://github.com/matter-labs/zksync/blob/master/LICENSE-APACHE), [Plonky2](https://github.com/mir-protocol/plonky2/tree/main/insertion) |
| BSD License                          | Commercial use, distribution, modification, patent use, private use | Disclose source, license and copyright notice                                                                                  |                        /                                                                                                                                                                                                                                                                                     |
| BSL                                  | Non-production use, distribution, modification, private use         | Disclose source, license and copyright notice                                                                                  | [Uniswap v3](https://github.com/Uniswap/v3-core/blob/main/LICENSE), Aave                                                                                                                                                                                                                                    |
| BOSL (Bootstrap Open Source License) | Commercial use, distribution, modification, private use             | Open-source the improvements, improvements available under BOSL after 12 months, disclose source, license and copyright notice | Zcash ([halo2’s](https://github.com/zcash/orchard/blob/main/LICENSE-BOSL) initial launch)                                                                                                                                                                                                                   |
| Polaris Prover License               | Non-commercial use                                                  | No transfer of rights, state changes                                                                                           | [StarkWare Prover ](https://starkware.co/starkware-polaris-prover-license/)                                                                                                                                                                                                                                 |

## Verifiable Delay Functions (VDF)

### Leading Problems

1. randomness is hard to generate on chain due to non-determinism, but we still want to be able to verify the randomness 
2. fairness of leader election is hard to ensure as the attacker may manipulate the randomness in election 

### VDF Requirements

1. Anyone has to compute **sequential** steps to distinguish the output. No one can have a speed advantage. 
2. The result has to be **efficiently verifiable** in a short time (typically in log(t))

### Techniques

- injective rational maps (First attempt in [original VDF paper](https://eprint.iacr.org/2018/601.pdf)): “weak VDF” requires large parallel processing
- Finite group of unknown order ([Pietrazak](https://eprint.iacr.org/2018/627.pdf) and [Wesolowski](https://eprint.iacr.org/2018/623.pdf)): use a trapdoor or Rivest-Shamir-Wagner time-lock puzzle

### Applications

- Chia blockchain: use VDF for consensus algorithms
- Protocol Labs + Ethereum Foundation: co-funding grants for research of viability of building optimized ASICs for running a VDF

### Great Resources

- [https://vdfresearch.org/](https://vdfresearch.org/)
- [https://blog.trailofbits.com/2018/10/12/introduction-to-verifiable-delay-functions-vdfs/](https://blog.trailofbits.com/2018/10/12/introduction-to-verifiable-delay-functions-vdfs/)

## Formal Verification

### Leading Problem

- How do we formally verify that a set of constraints used by a zero knowledge proof system has the desired characteristics, such as soundness, completeness, functional correctness, and zero knowledge?

- Zero knowledge proof systems often use a mathematical constraint system such as R1CS or AIR to encode a computation. The zero knowledge proof is a probabilistic cryptographic proof that the computation was done correctly

- Formal verification of a constraint system used in a zero knowledge proof requires 
   - (1) a formal specification of the computation that the constraint system is intended to encode
   - (2) a formal model of the semantics of the constraint system
   - (3) the specific set of constraints representing the computation
   - (4) the theorem prover, and
   - (5) the mechanized proof of a theorem relating (1) and (3)

### Techniques

- To prove a general statement with certainty, it must be a purely mathematical statement which can be known to be true by reasoning alone, without needing to perform any tests. The reasoning must start from consistent assumptions (axioms) and proceed by truth-preserving inferences
   - Example: given that a bachelor is defined as an unmarried man, and a married man by definition has a spouse, it can be proven with certainty that no bachelor has a spouse.

- A proof theory codifies a consistent set of rules for performing truth-preserving inference. Proof theories can be used to construct proofs manually, with pencil and paper
   - Example: [sequent calculus for classical logic](http://logitext.mit.edu/logitext.fcgi/tutorial)

- Computer programs called proof assistants reduce labor and mitigate the risk of human error by helping the user to construct machine-checkable proofs
   - Examples: [Coq](https://coq.inria.fr/), [Agda](https://agda.readthedocs.io/en/v2.6.0.1/getting-started/what-is-agda.html), [Coq](https://coq.inria.fr/), [Agda](https://agda.readthedocs.io/en/v2.6.0.1/getting-started/what-is-agda.html), [Isabelle](https://isabelle.in.tum.de/), [ACL2](https://www.cs.utexas.edu/users/moore/acl2/manuals/latest/?topic=ACL2____TOP), [PVS](https://pvs.csl.sri.com/), [Imandra](https://docs.imandra.ai/imandra-docs/), and [Lean](https://leanprover.github.io/)

#### Using a proof assistant

- To use a proof assistant to prove a statement about a program, there are two main approaches:

  - (1) Write the program in the proof assistant language and apply the proving facilities to the program directly
  - (2) Use a transpiler to turn a program written in another language into an object which the proof assistant can reason about

- Of these approaches, (1) seems preferable for the greater confidence provided by the proof being about exactly the (source) program being executed, as opposed to output of a transpiler which is assumed to have the same meaning as the source program
- What motivates approach (2) is when (for whatever reason) the proof assistant language is not suitable as a language for developing the application in

#### Without using a proof assistant

- There are also ways to prove a statement about a program without (directly) using a proof assistant:

  - Use a verified compiler (e.g. [CompCert](https://compcert.org/)), which turns a source program into an object program which provably has certain properties by virtue of (proven) facts about the verified compiler
     - Note that there is a distinction between a verified compiler and a verifying compiler.  CompCert itself is proved to generate binary that will always be semantically equivalent to the C input.  A verifying compiler generates a binary along with a proof of correctness that the binary is semantically equivalent to the source.
     - When a program is compiled by a verifying or verified compiler, we say that the compiler output is correct by construction (provided that the input is correct).
  - Use an automatic proof search algorithm, which takes as input statements to be proven and outputs proofs of those statements if those statements are true and the proof search algorithm finds proofs
  - Use a static analyzer, which takes as input a program and automatically checks for various kinds of issues using predetermined algorithms

- Both of these approaches have limitations:

  - A verified compiler is limited in what statements it can prove about the resulting program: typically, just that the resulting program has the same meaning or behavior as the source program

  - An automatic proof search algorithm is limited in what statements it can prove by the sophistication of the algorithm and the computational power applied to it. Also, due to Gödel's incompleteness theorem, there cannot exist a proof search algorithm which would find a proof of any given true statement

  - A static analyzer is generally not capable of reasoning about the meaning of a program to see if it’s correct; it is only able to recognize patterns which always or often indicate some kind of issue

### Formal Verification for ZK Circuits

- Formal verification for probabilistic proof systems, inclusive of ZK proof systems, encompasses two main problem spaces:

  1. Proving the intended properties of a general-purpose proving system, such as soundness, completeness, and zero knowledge (E.g., Bulletproofs, Halo 2, etc.)
  2. Proving the intended properties of a circuit, namely that it correctly encodes the intended relation

- Problem (1) is generally considered a very difficult problem and has not been done for any significant proving system

- Problem (2) can be done with a formal specification of the relation and a model of thcircuit semantics.  Usually it requires proving functional correctness of functions defined within the relation as well as the existence of witness variables for every argument to the function


#### Denotational design

- Denotational design provides a helpful way of thinking about both problem spaces (general and application-specific). The circuit denotes a set: namely, the set of public inputs for which the circuit is satisfiable

- The goal of application specific circuit verification is to prove that the circuit denotes the intended relation
- The goal of general purpose proving system verification is to prove that it has the intended properties with respect to the denotational semantics of circuits:

  1. Soundness means that if the verifier accepts a proof, then with high probability, the public input used to generate the proof (i.e., the statement being proven) is in the set denoted by the circuit (i.e., the statement is true)
  2. Completeness means that if a public input (i.e., a statement) is in the set denoted by the circuit (i.e., the statement is true), then the proving algorithm successfully outputs a proof which the verifier accepts

 - Example: consider a ZK proving system which one would use to show that one has a solution to a Sudoku puzzle, without revealing the solution
   - The statements to be proven are of the form "X (a Sudoku puzzle) is solvable"
   - The relation we intend is the set of solvable Sudoku puzzles
   - Soundness means that if the verifier accepts a proof that X is solvable, then with high probability, X is solvable
   - Completeness means that if X is solvable, then the prover creates a proof that X is solvable which the verifier accepts
   - Zero knowledge means that having a proof that X is solvable does not reduce the computational difficulty of finding a solution to X
   - To see this example worked out more formally, see [the OSL whitepaper](https://eprint.iacr.org/2022/1003).

- If you know that your circuit denotes the relation you intend, and you know that your general purpose proof system is sound and complete in the above senses, then you know that your application-specific proving system (i.e., the circuit plus the general proof system) has the intended soundness and completeness properties for that application

- This suggests that, given a formally verified general-purpose proving system, and a verifying compiler from statements to circuits, one can solve the problem of proving correctness of application-specific proving systems without application-specific correctness proofs

- Suppose that 

  1. one can write the statement to be expressed by a circuit in a machine-readable, human-readable notation, where it is self-evident that the statement being written has the intended meaning or denotation
  2. one has a verifying compiler which turns that statement into a circuit which provably has the same denotation as the source statement
  3. circuit can be executed on a formally verified general-purpose probabilistic proving system

- Then one can generate formally verified application-specific probabilistic proving systems without any additional proof writing for an additional application. This seems like a promising way forward towards a sustainable and cost effective approach to formal verification for ZK circuits

### Synthesizing formally verified programs

- Here is a summary of some of the ways in which the ecosystem supports efficient execution of verified code:

  - The proof assistants [Coq](https://coq.inria.fr/) and [Agda](https://github.com/agda/agda/) do not provide for compilation of programs written in those languages to an efficiently executable form
  - The language [ATS](http://www.ats-lang.org/) provides proof facilities and purports to allow for programming with the efficiency of C and C++
  - There are various means for transpiling code written in a mainstream language such as C or Haskell into a proof assistant, which allows for theorems to be proven about the extracted model of the source program
  - You can synthesize an efficient binary program using Coq (e.g., using [Fiat](https://github.com/mit-plv/fiat-crypto))
  - The proof assistant ACL2 defines a subset of Common Lisp with a full formal logic.  When a definition is executable, it can be compiled into efficient code, and because the language is a formal logic, you can define and prove theorems about the code
  - There is a verifying compiler project, [ATC](https://kestrel.edu/research/atc), from ACL2 to C
  - Imandra defines a subset of OCaml with a full formal logic and a theorem prover


#### Current limitations of formal methods on modern hardware

- In the context of modern computing, most computationally intensive tasks deal with vector math and other embarassingly parallel problems which are done most efficiently on specialized hardware such as GPUs, FPGAs, and ASICs
- This is generally true of the problem of constructing proofs in probabilistic proof systems. Provers for these proof systems would be most efficient if implemented on specialized hardware, but in practice, they are usually implemented on CPUs, due to the greater ease of programming on CPUs and the greater availability of those skill sets in the labor market
- For creating a formally verified implementation of a probabilistic proof system which executes efficiently, it seems that the right goal is not to optimize for speed of execution on a CPU, but to target specialized hardware such as FPGAs, GPUs, or ASICs
- Unfortunately, tools for compiling formally verified programs to run on FPGAs, GPUs, or ASICs are more or less nonexistent as far as we know

### The State of Current Progress

- Decades of research exists on formal verification strategies for arithmetic circuits in the context of hardware verification
   - See, e.g., [Drechsler et al (2022)](https://link.springer.com/chapter/10.1007/978-981-16-7182-1_36) and [Yu et al (2016)](https://ieeexplore.ieee.org/document/7442835)
   - This work has limited industral applications, e.g., the AAMP5 (see [Kern and Greenstreet (1997), page 43](https://cse.usf.edu/~haozheng/lib/verification/general/survey-FV.pdf))
   - This line of research is not directly applicable to formal verification of arithmetic circuits for zk-SNARKs, because 
     arithmetic circuits in hardware and arithmetic circuits in zk-SNARKs are not quite the same things
- Orbis Labs is working on:
   - A [verifying Halo 2 circuit compiler](https://github.com/Orbis-Tertius/coq-arithmetization) for [Σ¹₁ formulas](https://eprint.iacr.org/2022/777)
      - Expected to be working in Q4 2022 or Q1 2023
   - [Orbis Specification Language (OSL)](https://eprint.iacr.org/2022/1003/), which provides a high level spec language which we can compile to Σ¹₁ formulas
   - A toolchain (Miya) for developing formally verified, hardware accelerated probabilistic proof systems
      - A theory of interaction combinator arithmetization, towards compiling
        formally verified code into circuits
      - No timeline on this; still in basic research
- [Kestrel Institute](https://www.kestrel.edu) is a research lab that has proved functional correctness of a number of R1CS gadgets using the ACL2 theorem prover.  (Kestrel also does a lot of other FV work, including on Yul, Solidity, Ethereum, and Bitcoin)
   - They [formalized and proved](https://www.cs.utexas.edu/users/moore/acl2/manuals/latest/index.html?topic=ZKSEMAPHORE____SEMAPHORE) the functional correctness of parts of the Ethereum Semaphore R1CS
   - They [formalized and proved](https://www.cs.utexas.edu/users/moore/acl2/manuals/latest/index.html?topic=ZCASH____ZCASH) the functional correctness of parts of the Zcash Sapling protocol.  An overview of the process:
     - Used the [ACL2](https://www.cs.utexas.edu/users/moore/acl2/) proof assistant to formalize specs of parts of the Zcash Sapling protocol
     - Formalized rank 1 constraint systems [(R1CS) in ACL2](https://www.cs.utexas.edu/users/moore/acl2/manuals/latest/index.html?topic=R1CS____R1CS)
     - Used an extraction tool to fetch the R1CS gadgets from the Bellman implementation, and then used the [Axe](https://www.kestrel.edu/research/axe/) toolkit to lift the R1CS into logic
     - Proved in ACL2 that those R1CS gadgets are semantically equivalent to their specs, implying soundness and completeness
- [Aleo](https://www.aleo.org/) is developing programming languages such as [Leo](https://leo-lang.org/) that compile to constraint systems such as R1CS
     - Aleo aims to create a verifying compiler for Leo, with theorems of correct compilation generated and checked using the ACL2 theorem prover
     - Aleo has also done post-hoc verification of R1CS gadgets using Kestrel Institute's [Axe](https://www.kestrel.edu/research/axe/) toolkit
- [Nomadic Labs](https://www.nomadic-labs.com/) is a consulting firm that does a lot of work on Tezos and they built the Zcash Sapling protocol for shielded transactions into the Tezos blockchain as of the Edo upgrade.   Kestrel Institute formally verified some of the R1CSes used in that protocol. (Nomadic Labs also does a lot of other FV work)
- Anoma team is working on the [Juvix language](https://github.com/anoma/juvix) as a first step toward creating more robust and reliable alternatives for formally verified smart contracts than existing languages
- Andrew Miller and Bolton Bailey are working on a [formal verification of a variety of SNARK proof systems](https://github.com/BoltonBailey/formal-snarks-project), using the Lean Theorem Prover, in the Algebraic Group Model
- [Alex Ozdemir](https://cs.stanford.edu/~aozdemir/research/) from Stanford is working on adding a [finite field solver](https://github.com/alex-ozdemir/CVC4/tree/ff) in cvc5 SMT Solver
- Lucas Clemente Vella and Leonardo Alt are working on [SMT solver](https://github.com/lvella/polynomial-solver/blob/master/docs/SMT-2022-extended-abstract/full-text.pdf) of polynomial equations over finite fields
- Veridise is working on:
    - [Medjai](https://github.com/Veridise/Medjai), a symbolic evaluator for Cairo, intended for use in automatic proof search
    - [Picus](https://github.com/Veridise/Picus), a symbolic VM for R1CS, intended for use in automatic proof search
    - [V](https://github.com/Veridise/V), a specification language intended for use in expressing statements to be proven
      by automatic proof search
- [Ecne](https://0xparc.org/blog/ecne) is a special-purpose automatic proof search tool which can prove
  that an R1CS constraint system defines a function (total or partial)
   - In other words, it proves that for any input values on which the system is satisfiable, there
     is a unique combination of output values on which the system is satisfied
   - This proves, for an R1CS which is intended to be satisfiable on all possible inputs (denoting a function
     as opposed to a partial function), that there are enough constraints, in the sense that adding constraints
     could not change the denotation of the circuit if the denotation remains a partial function
   - This does not imply soundness
   - This approach has been proven to be useful in flushing out bugs in circuits
- [Starkware](https://starkware.co/) is writing [Lean](https://leanprover.github.io/)
  [proofs](https://github.com/starkware-libs/formal-proofs) to check that
  circuits expressed as Cairo programs conform to their specs

### How to analyze each research effort / development project

- You could analyze each research effort / development project in terms of
   - the language used for specification
   - the language used to model the constraint system
   - which theorem prover they are using
   - what sorts of theorems are they proving, like soundness, completeness, functional completeness, and zero knowledge
- Other interesting attributes of project concerning FV for constraint systems are:
   - whether the tooling and the formal verification are open source.  It is hard to have confidence in a theorem when the components that went into proving that theorem are not available for inspection
   - what is the trusted core, i.e., what software in the stack is not formally verified, and what are the possible consequences if it has bugs

### Future Research Directions

 - A lot of work needs to be done. There is not enough emphasis placed on formal verification in the security industry
 - Based on the observations and arguments presented in this blog post, we think the following will be some interesting directions for future research and development:
   - Build foundations for formally verifying zero knowledge proof systems:
   - Generalizable proof techniques for proving the desired properties formally
   - Reusable verified abstractions for proof systems, e.g., a polynomial commitment scheme library
- Can we formally verify systems that are designed to automatically make ZK circuits more efficient? 
   - For example: systems that choose a different circuit so that the setup MPC is more parallelizable or that allow a prover who learns part of the witness in advance to partially evaluate a circuit and use this information to compute proofs faster
- Improved specification languages and verified translators between specification languages
- Understand how to create formally verified programs to run on vectorized hardware, e.g., FPGAs, GPUs, and/or ASICs

### Terminology

- There are different ways to axiomatize a problem to prove it.  Some categories are denotational semantics, axiomatic semantics, and operational semantics.  Operational semantics is particularly useful for proving things about programs
- If you write a specification of a computation in a high-level formal language and compile it to a constraint system using a verified or verifying compiler, that is called correct by construction.  If you take an existing constraint system and you try to prove properties about it (up to and including soundness and completeness) with respect to a specification in a high-level formal language, that is called post-hoc verification

## Proof Systems

### Background - Commitment Schemes
- Allows one party to commit to some data by publishing a commitment
- Later they can reveal the data and convince anyone it is the same as what they committed to.
- Ideally, a commitment scheme should be:
    - **Hiding** - It does not reveal the input that is committed
    - **Binding** - It is impossible (or impractically difficult) to open a commitment to a different value that it was created with

#### Pedersen Commitments
- Let $$G$$ and $$H$$ be two public generators for a large group where the discrete log is hard.
- For an input, $$x$$, and hidden random value, $$r$$, the Pedersen commitment is $$comm(x) = xG + rH$$
- The commitment is opened by revealing $$x$$ and $$r$$
- These have some cool properties compared with hash commitments:
    - **Additively Homomorphic:** $$comm(a) + comm(b) = comm(a+b)$$
    - **Batchable:** $$x_1G_1 + x_2G_2 + ... + rH = \vec{x}\vec{G} + rH$$
- Low-level tech: discrete log group
- Assumptions: discrete log
- Resources:
    - Lecture by Dan Boneh: [https://youtu.be/IkNZWJFcfcU](https://youtu.be/IkNZWJFcfcU)
    - Overview by Bill Buchanan: [https://youtu.be/J9SOk9dIOCk](https://youtu.be/J9SOk9dIOCk)

#### Kate Commitments (KZG)
- A type of polynomial commitment scheme that employs structured reference string (SRS) and require trusted setup, thus producing toxic waste
- The verifier asks "what is the value of the polynomial at z"
- The prover responds with $$y$$ and a proof $$h(s)$$ using:
    - the polynomial $$f()$$
    - the commitment - $$f(s)$$ where $$s$$ is a secret point (toxic waste)
    - the proof - $$h(s)$$
    - the coordinates $$z$$, $$y$$
- The verifier is able to convince themselves this is true even if they don’t know $$f()$$
- Require pairing-friendly curves to multiple elliptic curve points
- Low-level tech: pairing group
- Assumptions: discrete log + secure bilinear pairing
- Used for:
    - zk-SNARKs
    - [Data Availability Sampling](https://ethresear.ch/t/an-alternative-low-degreeness-proof-using-polynomial-commitment-schemes/6649)
- Libraries:
    - [https://github.com/proxima-one/kzg](https://github.com/proxima-one/kzg) (Rust)
    - [https://github.com/sifraitech/kzg](https://github.com/sifraitech/kzg) (Rust)
    - [https://github.com/protolambda/go-kzg](https://github.com/protolambda/go-kzg) (Go)
    - [https://github.com/benjaminion/c-kzg](https://github.com/benjaminion/c-kzg) (C)
    - [https://github.com/mratsim/constantine/tree/master/research/kzg_poly_commit](https://github.com/mratsim/constantine/tree/master/research/kzg_poly_commit) (Nim)
    - [https://github.com/Nashatyrev/jc-kzg](https://github.com/Nashatyrev/jc-kzg) (Java)
- Resources:
    - General overview: [https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html](https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html)
    - Trusted setup:
        - [https://vitalik.ca/general/2022/03/14/trustedsetup.html](https://vitalik.ca/general/2022/03/14/trustedsetup.html)
        - [https://github.com/ethereum/kzg-ceremony-specs](https://github.com/ethereum/kzg-ceremony-specs)
    - Math simply put: [https://twitter.com/bkiepuszewski/status/1518163771788824576](https://twitter.com/bkiepuszewski/status/1518163771788824576)

#### Inner Product Arguments (IPA)
- A modification of Pedersen commitments to allow polynomial commitments
- Does not require trusted setup and pairing-friendly curves
- Larger proof size compared with KZG
- Low-level tech: discrete log group
- Assumptions: discrete log
- Used for:
    - Halo2
    - [Kimchi](https://o1-labs.github.io/proof-systems/plonk/inner_product.html)
- Libraries:
    - [https://github.com/arnaucube/ipa-rs](https://github.com/arnaucube/ipa-rs) (Rust)
    - [https://github.com/arkworks-rs/poly-commit/tree/master/src/ipa_pc](https://github.com/arkworks-rs/poly-commit/tree/master/src/ipa_pc) (Rust)
- Resources:
    - General overview: [https://dankradfeist.de/ethereum/2021/07/27/inner-product-arguments.html](https://dankradfeist.de/ethereum/2021/07/27/inner-product-arguments.html)
    - Talk: [https://youtu.be/dD_0Vn4BhmI](https://youtu.be/dD_0Vn4BhmI)

#### Fast Reed-Solomon Interactive Oracle Proof of Proximity (FRI)
- Based around [Reed-Solomon](https://youtu.be/1pQJkt7-R4Q) erasure coding
- Only requires hash-based (symmetrical) cryptography (quantum-resistant)
- No need for a trusted setup
- Much larger proof sizes
- Used for:
    - STARKs
    - Plonky2
- Resources:
    - General overview: [https://aszepieniec.github.io/stark-anatomy/fri.html](https://aszepieniec.github.io/stark-anatomy/fri.html)
    - Vitalik's deep dive: [https://vitalik.ca/general/2017/11/22/starks_part_2.html](https://vitalik.ca/general/2017/11/22/starks_part_2.html)
- Assumptions: collision-resistant hash function

#### Diophantine Arguments of Knowledge (DARK)
- Based on unknown order groups
- Requires trusted setup if using RSA groups, not if using Class Groups
- Much slower verification time compared with others (O(n))
- Resources:
    - Paper: [https://eprint.iacr.org/2019/1229.pdf](https://eprint.iacr.org/2019/1229.pdf)


#### Commitment Schemes Comparison

| Scheme         | Pedersen                                         | KZG                                                                                                     | IPA                                                         | FRI                                    | DARK                                                            |
|----------------|--------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|----------------------------------------|-----------------------------------------------------------------|
| Type           | Scalar                                           | Polynomial                                                                                              | Polynomial                                                  | Polynomial                             | Polynomial                                                      |
| Low-level tech | Discrete log group                               | Pairing group                                                                                           | Discrete log group                                          | Hash function                          | Unknown order group                                             |
| Setup          | $$G,H$$ generator points and random scalar $$r$$ | $$G1$$, $$G2$$ groups, $$g1$$, $$g2$$ generators, $$e$$ pairing function, $$s_k$$ secret value in $$F$$ | $$G$$ elliptic curve, $$g^n$$ independent elements in $$G$$ | $$H$$ hash function, $$w$$ unity root  | $$N$$ unknown order, $$g$$ random in $$N$$, $$q$$ large integer |
| Commitment     | $$aG+rH$$                                        | $$(a_0s^0+...a_ns^n)g_1$$                                                                               | $$a_0g_0+...+a_ng_n$$                                       | $$H(f(w^0), ..., f(w^n))$$             | $$(a_0q^0+...a_nq^n)g$$                                         |
| Proof size     | $$O(N)$$                                         | $$O(1)$$                                                                                                | $$O(log(N))$$                                               | $$O(log^2(N))$$                        | $$O(log(N))$$                                                   |
| Verify time    | $$O(N)$$                                         | $$O(1)$$                                                                                                | $$O(log(N))$$                                               | $$O(log^2(N))$$                        | $$O(N)$$                                                        |
| Prove time     | $$O(N)$$                                         | $$O(N)$$                                                                                                | $$O(N)$$                                                    | $$O(N*log^2(N))$$                      | $$O(N)$$                                                        |

### Bulletproofs
- Short NIZK that requires no trusted setup.
- Low-level tech: Pedersen commitments.
- Uses its own constraint system, which is easily convertible to R1CS.
- Assumptions: discrete log.

#### Performance:
- Prover time: $$O(N * log(N))$$
- Verifier time: $$O(N)$$
- Proof size: $$O(log(N))$$
- Works best with `dalek` curve having a Ristretto group (a compressed group of Ed25519 points).

#### Application scope:
- Range proofs (take only 600 bytes)
- Inner product proofs
- Verifiable secret shuffle
- Intermediary checks in MPC protocols
- Aggregated and distributed (with many private inputs) proofs

#### Used in:
- Confidential TX for Bitcoin:
    - [https://elementsproject.org/features/confidential-transactions](https://elementsproject.org/features/confidential-transactions)
- Monero
    - [https://medium.com/digitalassetresearch/monero-becomes-bulletproof-f98c6408babf](https://medium.com/digitalassetresearch/monero-becomes-bulletproof-f98c6408babf)
- Stellar Shielded Tx (Cloak)
    - [https://github.com/stellar/slingshot/blob/main/spacesuit/spec.md](https://github.com/stellar/slingshot/blob/main/spacesuit/spec.md)

#### Implementations:
- [https://github.com/dalek-cryptography/bulletproofs](https://github.com/dalek-cryptography/bulletproofs) (Rust)
- [https://github.com/adjoint-io/bulletproofs](https://github.com/adjoint-io/bulletproofs) (Haskell)
- [https://github.com/bbuenz/BulletProofLib](https://github.com/bbuenz/BulletProofLib) (Java)

#### Examples:
- [https://github.com/lovesh/bulletproofs-r1cs-gadgets](https://github.com/lovesh/bulletproofs-r1cs-gadgets)
- [https://github.com/MarcKloter/bulletproofs_gadgets](https://github.com/MarcKloter/bulletproofs_gadgets)

#### Resources:
- [https://crypto.stanford.edu/bulletproofs/](https://crypto.stanford.edu/bulletproofs/)
- [https://youtu.be/gZjDKgR4dw8?t=1045](https://youtu.be/gZjDKgR4dw8?t=1045)


### Maurer (sigma) Proofs
- Short proof that needs no trusted setup.
- Dlog-based variant of the interactive Sigma protocols.
- Require a constant number (3) of public-key operations.
- Can be made non-interactive using Fiat-Shamir transform.
- Generalize Schnorr proofs to arbitrary linear functions.
- Multiple Sigma proofs can be combined to form a new proof (composition):
    - if you have two Sigma protocols for proving statements `A` and `B` you can compose them into a protocol for proving `A or B`, `A and B`, `eq(A,B)`, `all(n,A)`, `eq-all(n,A)`;
    - `n` must be known at compile time.
- Assumptions:
    - discrete log,
    - honest verifier or Fiat-Shamir.

#### Application scope
- Discrete log (dlog) proofs, prove knowledge of x such that g^x = y for publicly known values g, y.
- One-of-many dlogs, discrete log equality (dleq).
- Range proofs.
- Knowledge of message and randomness in a Pedersen commitment, equality of message in 2 Pedersen commitments.
- Pedersen commitment and ciphertext ([twisted ElGamal](https://github.com/yuchen1024/Twisted_ElGamal_PKE/blob/master/doc/Twisted_ElGamal_Report.pdf)) holds the same message.

#### Used in:
- Verifiable Random Functions
    - [https://medium.com/asecuritysite-when-bob-met-alice/verifiable-random-functions-4563d6eb17ab](https://medium.com/asecuritysite-when-bob-met-alice/verifiable-random-functions-4563d6eb17ab)
- Signal [Algebraic MACs](https://smeiklej.com/files/ccs14.pdf) for or group chats:
    - [https://signal.org/blog/signal-private-group-system/](https://signal.org/blog/signal-private-group-system/)
- Dleq proofs in the adaptor signatures
    - [https://github.com/LLFourn/one-time-VES/blob/master/main.pdf](https://github.com/LLFourn/one-time-VES/blob/master/main.pdf)
- ElGamal encryption in the [Cryptography for #metoo](https://petsymposium.org/2019/files/papers/issue3/popets-2019-0054.pdf)

#### Libraries:
- [https://github.com/LLFourn/secp256kfun/tree/master/sigma_fun](https://github.com/LLFourn/secp256kfun/tree/master/sigma_fun) (Rust)
- [https://docs.rs/zkp/0.7.0/zkp](https://docs.rs/zkp/0.7.0/zkp) (Rust)
- [https://github.com/xlab-si/emmy](https://github.com/xlab-si/emmy) (Go)
- [https://pkg.go.dev/go.dedis.ch/kyber/v4/proof](https://pkg.go.dev/go.dedis.ch/kyber/v4/proof) (Go)
- [https://github.com/cryptobiu/libscapi](https://github.com/cryptobiu/libscapi) (C++)
- [https://github.com/spring-epfl/zksk](https://github.com/spring-epfl/zksk) (Python)

#### Resources
- Overview: [https://medium.com/@loveshharchandani/zero-knowledge-proofs-with-sigma-protocols-91e94858a1fb](https://medium.com/@loveshharchandani/zero-knowledge-proofs-with-sigma-protocols-91e94858a1fb)
- Blog post about Maurer generalization of dlog-based Sigma protocols: [https://cronokirby.com/posts/2022/08/the-paper-that-keeps-showing-up/](https://cronokirby.com/posts/2022/08/the-paper-that-keeps-showing-up/)
- Math: [https://www.win.tue.nl/~berry/CryptographicProtocols/LectureNotes.pdf](https://www.win.tue.nl/~berry/CryptographicProtocols/LectureNotes.pdf) (pp. 47-61)
- Forum discussion: [https://community.zkproof.org/t/standardizing-sigma-protocols/471](https://community.zkproof.org/t/standardizing-sigma-protocols/471)


### Halo 2
- Combines an efficient accumulation scheme with [PLONKish](https://twitter.com/feministPLT/status/1413815927704014850) arithmetization and needs no trusted setup.
- Superchargable with pre-computed lookup tables and custom gates.
- Flourishing developer ecosystem actively developed tooling.
- Low-level tech: IPA commitments.
- Assumptions: discrete log.

#### Performance:
- Allows to choose (codewords rate):
    - fewer rows => fast prover,
    - fewer columns and constraints => fast verifier.
- Prover time: $$O(N * log(N))$$
- Verifier time: $$O(1)$$ > PLONK+KZG > Groth'16
- Proof size: $$O(log(N)$$

#### Application scope:
- Arbitrary verifiable computation;
- Recursive proof composition;
- Circuit-optimized hashing based on lookup-based [Sinsemilla](https://zcash.github.io/halo2/design/gadgets/sinsemilla.html) hash function.

#### Used in:
- [Zcash Orchard Shielded Protocol](https://electriccoin.co/blog/announcing-zip-224-bringing-halo-2-to-zcash/) [\[ZIP 224\]](https://github.com/zcash/zips/issues/435)
- [Scroll zkEVM](https://scroll.io/blog/zkEVM#zkEVM_Design_highlight_111)
- [Orbis ZK-Rollup](https://github.com/Orbis-Tertius) on Cardano
- [Dark Fi L1](https://dark.fi/)

#### Libraries:
- [https://github.com/zcash/halo2](https://github.com/zcash/halo2) - original (IPA)
- [privacy-scaling-explorations/halo2wrong](https://github.com/privacy-scaling-explorations/halo2wrong) - replaces IPA to KZG
- [https://github.com/Orbis-Tertius/halo2](https://github.com/Orbis-Tertius/halo2) - replaces IPA to FRI (WIP)

#### Examples:
- [https://github.com/nikkolasg/halo2-circuits](https://github.com/nikkolasg/halo2-circuits)
- [https://github.com/icemelon/halo2-tutorial](https://github.com/icemelon/halo2-tutorial)
- [https://github.com/weijiekoh/halo2_circuits](https://github.com/weijiekoh/halo2_circuits)
- [https://github.com/akinovak/halo2-semaphore](https://github.com/akinovak/halo2-semaphore)
- [https://github.com/timoth-y/halo2-encryption](https://github.com/timoth-y/halo2-encryption)

#### Resources:

- General overview: [https://electriccoin.co/blog/explaining-halo-2/](https://electriccoin.co/blog/explaining-halo-2/)
- Talk: [https://youtu.be/KdkVTEHUxgo?t=399](https://youtu.be/KdkVTEHUxgo?t=399)
- Math: [https://vitalik.ca/general/2021/11/05/halo.html](https://vitalik.ca/general/2021/11/05/halo.html)
- Awesome: [https://github.com/adria0/awesome-halo2](https://github.com/adria0/awesome-halo2)
- Ecosystem showcase: [https://youtu.be/JJi2TT2Ahp0](https://youtu.be/JJi2TT2Ahp0)

### Plonky2

- Combines FRI with PLONKish arithmetization and needs *no trusted setup*
- Extensively optimized for modern processors (both x86-64 and ARM64) using:
    - [Ed448-Goldilocks](https://eprint.iacr.org/2015/625.pdf), whose modulus allows working on 64-bit fields
    - [SIMD](http://ftp.cvut.cz/kernel/people/geoff/cell/ps3-linux-docs/CellProgrammingTutorial/BasicsOfSIMDProgramming.html) operations for efficient sponge hashing
- Leverages custom gates to significantly optimize non-native arithmetic (e.g. field division)
- Uses Poseidon sponge as the underlying hash function. Supports GMiMC, which will result in more efficiency, but might be less secure
- Assumptions: collision-resistant hash function

#### Performance:

- Prover time: $$O(log^2(N))$$ - 300ms/20s (depending on the codewords rate)
- Verifier time: $$O(log^2(N))$$
- Proof size: $$O(N*log^2(N))$$ - 500kb/43kb (depending on the codewords rate)

#### Application scope:

- Recursive proof composition
- Circuit optimization using TurboPLONK's custom gates

#### Used in:

- [Polygon Zero](https://blog.polygon.technology/introducing-plonky2)
- [Maru Network](https://proxima-one.github.io/maru/introduction.html)

#### Libraries
- [https://github.com/mir-protocol/plonky2](https://github.com/mir-protocol/plonky2)

#### Examples:

- [https://github.com/qope/plonky2-examples](https://github.com/qope/plonky2-examples)
- [https://github.com/recmo/proto-ecdsa-plonky2](https://github.com/recmo/proto-ecdsa-plonky2)
- [https://github.com/mir-protocol/plonky2-semaphore](https://github.com/mir-protocol/plonky2-semaphore)
- [https://github.com/timoth-y/plonky2-encryption](https://github.com/timoth-y/plonky2-encryption)

#### Resources

- [https://github.com/mir-protocol/plonky2/blob/main/plonky2/plonky2.pdf](https://github.com/mir-protocol/plonky2/blob/main/plonky2/plonky2.pdf)
- [https://proxima-one.github.io/maru/background-knowledge/plonky2.html](https://proxima-one.github.io/maru/background-knowledge/plonky2.html)


### MPC-in-the-head

#### Background - MPC:
- MPC enables parties to carry out distributed computation on their private inputs.
- each party produces a computation transcript (its own view).
- **Important observation #1:** if the final result of MPC is wrong, then there's an inconsistent views somewhere.

#### Background - Secret Sharing:
- Secret Sharing allows splitting some secret among multiple parties, such that all of them need to cooperate for using it.
- **Important observation #2:** if only a subset of shares is revealed, then the secret remains unknown.

#### Overview:
- The prover simulates an MPC protocol "in their head", which computes $$f(x)$$ on a private input $$x$$ shared among $$n$$ virtual parties.
- The verifier can then ask to reveal a subset of views, which isn't enough to recover private inputs, but gives some confidence that the probability of not seeing the inconsistency is small.
- It's not small enough, so we need to repeat this interaction multiple times (~300).
- This can then be made non-interactive by doing this in parallel and applying Fiat-Shamir transform.
- The underlying MPC protocol used for proof generation isn't important, only views are;
- This allows treating simulated MPC as a black box and skip computing, thus avoiding a lot of overhead.
- Can be efficiently applied for boolean circuits, unlike most other NIZKs that are only feasible with arithmetic circuits.
- Can be extended with RAM - shared memory with private addresses ([paper](https://eprint.iacr.org/2022/313.pdf))
- Above innovations allow to easily compile arbitrary program into the verifiably computation circuit.

#### Performance:
- Prover time: $$O(n)$$ - much less than with SNARKs
- Verifier time: $$O(n)$$
- Proof size: $$O(n)$$
- The overhead of doing computation verifiably is much lower ($$1/{n_{reps}*n_{parties}}$$ => ~1/600) than that of SNARKs (~1/millions).
- Additions are free, but multiplications needed to be simulated many times.
- With pre-processing it's possible to reduce size and verifier complexity at the prover's expense.

#### Application scope:
- Complex programs that are poorly translatable to arithmetic circuits, e.g. SHA hashes.
- ML and other types of approximate calculations, that do floating point arithmetic (can be done bit-by-bit in boolean circuits).
- Delegated proving:
    - You want to create a SNARK of some statement, but don't want to compute it yourself nor to reveal the witness to a third party.
    - One thing you could do is create a MPCith proof and then the delegated party would create a new proof which verifies this one.
- Optimized confidential transactions using delegated proving
    - The idea being that you would minimize latency for the transaction creator, where they create the proof as fast as possible, and then another entity could compress the proof with other techniques.

#### Libraries:
- [https://github.com/trailofbits/reverie/tree/stacked-ikos](https://github.com/trailofbits/reverie/tree/stacked-ikos) (Rust)
- [https://github.com/cronokirby/boo-hoo](https://github.com/cronokirby/boo-hoo) (Rust)
- [https://github.com/cronokirby/Rem-Boo](https://github.com/cronokirby/Rem-Boo) (Rust)

#### Resources:
- Podcast episodes:
    - [https://cronokirby.substack.com/p/mpc-in-the-head-special](https://cronokirby.substack.com/p/mpc-in-the-head-special)
    - [https://cronokirby.substack.com/p/mpc-in-the-head-2-thoughts-about](https://cronokirby.substack.com/p/mpc-in-the-head-2-thoughts-about)
- Great intro paper (ZKBoo): [https://eprint.iacr.org/2016/163.pdf](https://eprint.iacr.org/2016/163.pdf)
- Security analysis: [https://eprint.iacr.org/2021/437.pdf](https://eprint.iacr.org/2021/437.pdf)
