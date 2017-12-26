## Roadmap del Software di EOS.IO

Questo documento delinea un piano di sviluppo di alto livello e sarà aggiornato in considerazione dei progressi compiuti verso la versione 1.0. Va notato che questa roadmap è da applicarsi esclusivamente al software della blockchain e non ad altri strumenti e risorse utili come portafogli e esploratori di blocchi (c.d. block explorers) che avranno i propri team e le proprie roadmap dedicate una volta raggiunto il completamento della Fase 1.

***L'intero contenuto del documento è attualmente sotto forma di bozza e a solo scopo informativo e potrà essere soggetto a variazioni. block.one non garantisce l'accuratezza delle informazioni contenute nella roadmap, inoltre le informazioni vengono fornite "così come sono" senza dichiarazioni o garanzie, esplicite o implicite.***

# Fase 1 - Ambiente di Testing Sostenibile - Estate 2017

L'obbiettivo di questa fase è di stabilire le API di cui gli sviluppatori avranno bisogno per iniziare a costruire e testare le applicazioni su EOS.IO. Affinché gli sviluppatori possano iniziare a testare le loro applicazioni, sarà necessario implementare quanto segue:

### Nodo Autonomo (Dan & Nathan)

Un nodo autonomo gestisce una blockchain di test e produce blocchi durante l'esposizione di un'API. Questo nodo non deve occuparsi di alcun codice di rete P2P.

### Contratti Nativi (Nathan)

Il software EOS.IO ha una serie di contratti nativi. Questi sono contratti che gestiscono le operazioni principali della blockchain e esistono al di fuori dell'interfaccia Web Assembly. Questi contratti includono:

1. @eos - gestisce i trasferimenti di token EOS
2. @stake - gestisce i token EOS bloccati, le votazioni e le Elezioni de Produttore
3. @system - gestisce i permessi, i messaggi e gli aggiornamenti del codice di contatto

### API della Macchina Virtuale (Dan)

I contratti sono compilati in WebAssembly (WASM) e WASM deve interfacciarsi con la blockchain tramite un'API definita. Questa API è ciò su cui gli sviluppatori dipendono per creare applicazioni e renderle relativamente stabili prima che essi possano realmente iniziare a sviluppare su EOS.

### Interfaccia RPC (Arhag, Nathan)

Verrà fornita una semplice interfaccia JSON RPC su HTTP che consente agli sviluppatori di trasmettere transazioni e interrogare lo stato dell'applicazione. Questo è critico sia per la pubblicazione che per l'interazione con le applicazioni di test.

### Strumenti a riga di comando (Arhag)

Gli strumenti a riga di comando facilitano l'integrazione dell'interfaccia RPC con gli ambienti di sviluppo.

### Documentazione di Base per Sviluppatori (Josh)

Documenti che istruiscono gli sviluppatori su come iniziare la procedura di sviluppo per la blockchain di EOS.IO. Ciò include documentazioni dell'API WASM, dell'interfaccia RPC e di strumenti a riga di comando (c.d. Command Line Tools)

# Fase 2 - Test Network Sostenibile - Autunno 2017 

Tutta la Fase 1 presuppone un ambiente affidabile che esegue solo il codice dello sviluppatore. Prima che un network di test possa essere implementato, è necessario implementare e testare diverse funzionalità aggiuntive.

### Codice di Rete P2P (Phill)

Questo plugin è responsabile della sincronizzazione dello stato della blockchain tra due nodi autonomi.

### WASM Sanitation & CPU Sandboxing (Brian)

Il codice WASM dev'essere ripulito per poter verificare comportamenti non deterministici come operazioni in virgola mobile (FLOP - floating point operations) e loop infiniti.

### Monitoraggio Dell'Uso delle Risorse & Rate Limiting (Arhag)

Per prevenire abusi della rete, il monitoraggio delle risorse e il tasso di controllo dell'utilizzo, limita gli utenti in base agli EOS accatastati (c.d. staked)

### Test D'Importazione di Genesi (DappHub)

È necessario sviluppare strumenti per esportare i dati dallo stato della distribuzione dei Token di EOS e creare un file di configurazione genesi. Ciò consentirà a chiunque partecipi alla distribuzione dei Token di acquisire qualche EOS di test iniziale (TEOS).

### Comunicazione Interblockchain (Nathan)

Questa funzionalità implica che la verifica dell'hash di Merkle delle transazioni sia corretta. 

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