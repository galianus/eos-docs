## EOS.IO Le plan d’évolution de Software

Ce document décrit le plan de développement à partir d'un niveau élevé et sera mis à jour de tant que que le progrès est fait vers la version 1.0. Il devrait été noté que cette plan d'évolution applique seulement à le Software blockchain et pas à le autres outils et utilitaires comme les portefeuilles et les explorateurs de blocs qui auront leurs propres équipes et des plan d’évolution une fois la phase 1 terminée.

***Tout ce qui est contenu dans ce document est sous forme d'ébauche et peut être modifié à tout moment et fourni à titre d'information seulement. block.one ne garantit pas l'exactitude des informations contenues dans ce plan d' d’évolution et l'information est fourni "comme si" sans représentation ni garantie, expresse ou implicite.***

# La phase 1 - Environnement de test viable minimal- Été 20017

L'objectif de cette phase est d'établir les API dont les développeurs auront besoin pour commencer à créer et tester des applications sur EOS.IO. Afin que les développeurs commencer à tester leurs applications, ils exigeront à mettre en œuvre ce qui suit :

### Nœud Autonome (Dan et Nathan)

Un nœud autonome opérés un blockhain de test et produit des blocs tandis qu'exposant une API. Cette nœud n'a pas besoin de se préoccuper des quelconque réseaux P2P code.

### Contrats Natives (Nathan)

Le Software EOS.IO a un nombre de contrats natives. Ce sont des contrats que manageront les operations principales de le blockchain et qui existe dehors l'interface Web Assemblée. Ces contrats contiennent:

1. @eos - géré les transfers de jetons EOS
2. @stake - gere les jetons EOS verrouilles, vote, et Election du Producteur
3. @system - gère les permissions, les messages et contact de mises à jour du code

### API Machine Virtuelle (Dan)

Les contrats sont compilées à WebAssembly(WASM) et WASM doit s'inerfacer avec le blockchain via une API définie. Cette API est ce qui les développeurs dépendent à créer des applications et être relativement stable avant que les développeurs peut vraiment commencer à construire sur EOS.

### Interface RPC (Arhag, Nathan)

Un simple RPC JSON sur une interface HTTP qui seront fournis que permettra à les développeurs diffuser les transactions et requête l’état de l'application. Ceci est critical pour la publication et l'interaction avec les applications test.

### Outils de ligne de commande (Arhag)

Les outils de ligne de commande facilitent l'integration de l'interface RPC avec les environnements développeur.

### Documentation Basic pour le Développeur (Josh)

Documents qui vont enseigner les développeurs comment commencer avec la construction sur la blockchain EOS.IO. Ceci contient les documentations de WASM,API, interface RPC, et les Outils de Ligne de Commande.

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