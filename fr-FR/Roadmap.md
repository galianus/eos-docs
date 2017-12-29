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

# La Phase 2 - Réseau de test minimum viable - Automne 2017

Tout dans la phase 1 suppose un environnement de confiance qui exécute uniquement le code du développeur. Avant qu'un réseau de test puisse être déployé, plusieurs fonctionnalités supplémentaires doivent être implémentées et testées.

### Code de réseau P2P (Phil)

C'est un plugin qui est responsable de la synchronisation de l'état blockchain entre deux nœuds autonomes.

### Assainissement WASM & CPU sandboxing (Brian)

Le code WASM doit être nettoyé pour vérifier les comportements non déterministes tels que les opérations en point flottante et les boucles infinies.

### Suivi de L'utilisation des Ressources & Limitation du Débit (Arhag)

Pour éviter les abus le suivi des ressources et le taux de suivi de l'utilisation limitent les utilisateurs en fonction de l'EOS implanté.

### Tests d'importation Genèse (DappHub)

Des outils doivent être développés pour exporter des informations de l'etat de la distribution des jetons EOS et créer un fichier de configuration genèse. Cela permettra à toute personne participant à la Distribution de Jetons d'acquérir un EOS de test initial(TEOS). 

### La Communication Interblockchain (Nathan)

Cette fonctionnalité implique la vérification du hachage Merkle des transactions est correcte.

# La Phase 3 - Tests & Audits de Sécurité - Hiver 2017, Printemps 2018

Au cours de cette phase, la plate-forme va subir des tests exhaustifs avec une concentration dans la recherche de problèmes et de bugs. Version 1.0 sera marquée à la fin de la Phase 3.

### Développement d'exemples d'applications

D'exemples d'applications sont criticales pour prouver la plate-forme fournit les fonctionnalités requises par les vrais développeurs.

### Récompenses pour attaquer avec succès le réseau

Attaquer le réseau avec spam, des exploits de machines virtuelles, des accidents bug, et comportement non déterministe sera un processus fortement impliqué mais nécessaire pour s'assurer que la version 1.0 est stable.

### Support linguistique

Ajout de support pour langues supplémentaires qui seront compilées en WASM: C++, Rust, etc.

### La Documentation & Tutoriels

# Phase 4 - Parallel Optimization Summer / Fall 2018

After getting a stable 1.0 product released, we will move toward optimizing the code for parallel execution.

# Phase 5 - Cluster Implementation The Future