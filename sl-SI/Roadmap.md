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

Pogodbe se zbirajo v WebAssembly (WASM) in WASM se mora povezati z "blockchain" preko definiranega API-ja. Ta API je stvar, na katero se razvijalci zanašajo, da gradijo aplikacije in je razmeroma stabilen, preden lahko razvijalci resnično začnejo graditi na EOS.

### RPC vmesnik (Arhag, Nathan)

Zagotovljen bo preprost JSON RPC prek vmesnika HTTP, ki razvijalcem omogoča oddajanje transakcij in poizvedbo o stanju aplikacije. To je ključnega pomena za objavljanje in interakcijo s testnimi aplikacijami.

### Orodja ukazne vrstice (Athag)

Orodja ukazne vrstice olajšajo integracijo vmesnika RPC z okoljem za razvijalce.

### Osnovna dokumentacija za razvijalce (Josh)

Dokumenti, ki poučujejo razvijalce, kako začeti graditi na blokih EOS.IO. To vključuje dokumentacijo API-ja WASM, vmesnika RPC in orodij ukazne vrstice.

# Faza 2 - Minimalno testno omrežje - Jesen 2017

Vse v 1. fazi predpostavlja zaupanja vredno okolje, ki izvaja samo lastno kodo razvijalca. Preden lahko uporabimo preizkusno omrežje, je treba izvesti in preizkusiti več dodatnih funkcij.

### P2P omrežna koda (Phil)

To je vtičnik, ki je odgovoren za sinhronizacijo "blockchain" stanja med dvema samostojnima vozliščema.

### Sanacija WASM & CPU "sandboxing" (Brian)

Kodo WASM je treba sanitirati, da preveri nedeterministično vedenje, kot so operacije s plavajočo vejico in neskončne zanke.

### Sledenje porabe virov & omejitev hitrosti (Arhag)

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