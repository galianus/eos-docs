## EOS.IO ソフトウェアロードマップ

当ドキュメントは、大まかな開発計画をまとめたものであり、バージョン1.0.に向けた進捗があった際に更新されます。当ロードマップは、ブロックチェーンソフトウェアのみに適応されるものであり、その他のウォレットやブロック・エクスプローラ等のツールに適応されるものではありません。ウォレット等のその他のツールに関しては、Phase1完了時に個々に専用のチームとロードマップが構築されます。

***当ドキュメントに記載されている全ての内容は情報提供のみを目的としたもので、草稿段階であり、突然変更される可能性があります。 block.one は、当ロードマップに記載されている情報について正確性を保証しません。また、これらの情報はあくまで現段階におけるものであり、暗示、黙示に関わらずいかなる責任や保証も負いません。***

# Phase1 最低限の実行可能なテスト環境 - 2017年 夏 -

このフェーズの目的は、EOS.IO上で開発者がアプリケーションの構築・テストを開始するために必要なAPIを構築することです。 開発者がアプリケーションの開発を始めるためには、以下の機能が実装されている必要があります：

### スタンドアロン・ノード(Dan & Nathan)

A standalone node operates a test blockchain and produces blocks while exposing an API. This node does not need to concern itself with any P2P networking code.

### Native Contracts (Nathan)

The EOS.IO software has a number of native contracts. These are contracts that manage the core operations of the blockchain and exist outside the Web Assembly interface. These contracts include:

1. @eos - manages EOS token transfers
2. @stake - manages locked EOS, voting, and Producer Election
3. @system - manages permissions, messages, and contact code updates

### Virtual Machine API (Dan)

Contracts are compiled to WebAssembly (WASM) and WASM must interface with the blockchain via a defined API. This API is what developers depend upon to build applications and be relatively stable before developers can really start to build on EOS.

### RPC Interface (Arhag, Nathan)

A simple JSON RPC over HTTP interface will be provided that enables developers to broadcast transactions and query application state. This is critical for both publishing and interacting with test applications.

### Command line Tools (Arhag)

Command line tools facilitate integrating the RPC interface with developer build environments.

### Basic Developer Documentation (Josh)

Documents that teach developers how to get started with building on EOS.IO blockchains. This includes documentations of the WASM API, RPC Interface, and Command Line Tools.

# Phase 2 - Minimal Viable Test Network - Fall 2017

Everything in Phase 1 assumes a trusted environment that only runs the developer's own code. Before a test network can be deployed several additional features need to be implemented and tested.

### P2P Network Code (Phil)

This is a plugin that is responsible for synchronizing the blockchain state between two standalone nodes.

### WASM Sanitation & CPU Sandboxing (Brian)

The WASM code needs to be sanitized to check for non-deterministic behavior such as floating point operations and infinite loops.

### Resource Usage Tracking & Rate Limiting (Arhag)

To prevent abuse the resource monitoring and usage tracking rate limits users according to staked EOS.

### Genesis Import Testing (DappHub)

Tools need to be developed to export data from the EOS Token Distribution state and create a genesis configuration file. This will enable anyone participating in the Token Distribution to acquire some initial test EOS (TEOS).

### Interblockchain Communication (Nathan)

This feature involves verifying the Merkle hashing of transactions is proper.

# Phase 3 - Testing & Security Audits - Winter 2017, Spring 2018

During this phase the platform will undergo heavy testing with a focus on finding security issues and bug. At the end of Phase 3 version 1.0 will be tagged.

### Develop Example Applications

Example applications are critical to proving the platform provides the features required by real developers.

### Bounties for Successfully Attacking Network

Attacking the network with spam, virtual machine exploits, and bug crashes, and non-deterministic behavior will be a heavily involved process but necessary to ensure that version 1.0 is stable.

### Language Support

Adding support for additional languages to be compiled to WASM: C++, Rust, etc.

### Documentation & Tutorials

# Phase 4 - Parallel Optimization Summer / Fall 2018

After getting a stable 1.0 product released, we will move toward optimizing the code for parallel execution.

# Phase 5 - Cluster Implementation The Future