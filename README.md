<div align="center">
  <h1 align="center">ZKP Knowledge Base</h1>
  <p align="center">An ongoing knowledge base for ZKP domian knowledge.</p>
</div>

Table of Content
=================

* [Hardware Acceleration](#hardware-acceleration)
* [Light Client](#light-client)
* [Regulations](#regulations)
* [Security Vulnerability](#security-vulnerability)
* [Identity](#identity)

## Hardware Acceleration

### Leading Problem:

- Proof generation is time-consuming, high-latency, and not efficient enough on traditional CPUs
- As L2 rollups and other applications of ZKP grow, we need to consider hardware as a potential breakthrough of performance improvement

### Business Models:

- zk-mining: Plug-n-play FPGA accelerator cards (revenue through hardware sale)
- zk-Rollup: public, private and on-premise cloud (revenue through hourly premiere)
- Software auxiliary: SDK for dApps and API (license to program FPGA in developer-friendly way

### 3 Potentials Scenarios

1. Permissioned networks, e.g. StarkNet 
    - Most lucrative, but not following the trend of further decentralization
2. Permissionless networks where provers compete on cost, e.g. Mina's Snarketplace
    - This is a race to the bottom like Bitcoin mining. None should be making a lot of profit unless their prover efficiency is vastly superior 
3. Permissionless networks where provers compete on latency, e.g. Polygon Hermes & Zero 
    - This has the most potential, but may not be a dependable revenue stream, since it can go to zero if a competitor improves their latency 

## Light Client 

### Leading Problem:

- Full node is resource-draining: downloading and verifying the whole chain of blocks takes time and resources
- Not retail friendly to be on mobile and perform quick actions like sending transactions and verifying balances

### Technology Highlight:

- trust-minimized interaction with full nodes and do not interact directly with the blockchain
- much lighter on resources and storage but require higher network bandwidth
- linear time based on chain length: verify from the root to the target height (may be from a trusted height, therefore constant length)
- Process overview:
    - retrieve a chain of headers instead of full block
    - verify the signatures of the intermediary signed headers and resolve disputes through checking with full nodes iteratively
- Superlight client: requires downloading only a logarithmic number of block headers while storing only a single block header between executions

## Regulations
- ... 

## Security Vulnerability
- ... 

## Identity
- ... 
