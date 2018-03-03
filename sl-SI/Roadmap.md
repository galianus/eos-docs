## EOS.IO Programski načrt

Ta dokument opisuje razvojni načrt iz visoke ravni in bo posodobljen z napredkom v smeri različice 1.0. Treba je opozoriti, da se ta načrt nanaša samo na "blockchain" programsko opremo, ne pa tudi na druga orodja in pripomočke, kot so denarnice in raziskovalci blokov, kateri bodo imeli svoje posvečene ekipe in načrte, ko bo 1. faza končana.

***Vsa vsebina v tem dokumentu je kot osnutek, kateri se lahko kadarkoli spremeni in je namenjen samo za informativne namene. block.one ne zagotavlja natančnosti informacij v tem načrtu in informacije podane so "kot so", brez zagotovil ali jamstev, izrecnih ali implicitnih.***

# Faza 1 - Minimalno preizkusno okolje - Poletje 2017

Cilj te faze je ustvariti API-je, katere bodo razvijalci potrebovali za izgradnjo in preizkušnjo aplikacij na EOS.IO. Da bodo razvijalci lahko začeli s preizkušanjem svojih aplikacij, morajo implementirati naslednje:

### Samostojno vozlišče (Dan & Nathan)

Samostojno vozlišče upravlja testni "blockchain" in proizvaja bloke, medtem ko izpostavlja API. To vozlišče se ne sme nanašati na omrežno kodo P2P.

### Nativne pogodbe (Nathan)

Programska oprema EOS.IO ima številne nativne pogodbe. To so pogodbe, ki upravljajo osnovne operacije "blockchaina" in obstajajo zunaj spletnega vmesnika. Te pogodbe vključujejo:

1. @eos - upravlja prenose žetonov EOS
2. @stake - upravlja zaklenjen EOS, glasovanje in volitve proizvajalcev
3. @system - upravlja dovoljenja, sporočila in posodobitve kode stikov

### Virtualni stroj API (Dan)

Pogodbe se zbirajo v WebAssembly (WASM) in WASM se mora povezati z "blockchain" preko definiranega API-ja. This API is what developers depend upon to build applications and be relatively stable before developers can really start to build on EOS.

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