## EOS.IO Perangkat lunak Roadmap

Dokumen ini menguraikan rencana pengembangan dari tingkat tinggi dan akan diperbarui karena kemajuan dibuat terhadap versi 1.0. Perlu dicatat bahwa peta jalan ini hanya berlaku untuk perangkat lunak blockchain dan bukan pada alat dan utilitas lain seperti dompet dan penjelajah blok yang akan memiliki tim mereka sendiri dan peta jalan khusus setelah Tahap 1 selesai.

***Segala sesuatu yang terkandung dalam dokumen ini adalah dalam bentuk draft dan dapat berubah sewaktu-waktu dan hanya untuk keperluan informasi saja. block.one tidak menjamin keakuratan informasi yang terdapat dalam peta jalan ini dan informasinya diberikan "sebagaimana adanya" tanpa pernyataan atau jaminan, tersurat maupun tersirat.***

# Tahap 1 - Lingkungan Pengujian Minimal yang Layak - Musim Panas 2017

Tujuan dari tahap ini adalah untuk membangun API yang pengembang perlukan untuk mulai membangun dan menguji aplikasi di EOS.IO. Agar pengembang mulai menguji aplikasi mereka, mereka memerlukan hal-hal berikut untuk diterapkan:

### Node Standalone (Dan & amp; Nathan)

Simpul mandiri mengoperasikan alat uji coba dan menghasilkan blok sambil membuka API. Node ini tidak perlu memperhatikan dirinya sendiri dengan kode jaringan P2P.

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