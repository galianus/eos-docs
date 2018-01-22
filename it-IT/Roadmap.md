## Roadmap del Software di EOS.IO

Questo documento delinea un piano di sviluppo di alto livello e sarà aggiornato in considerazione dei progressi compiuti al raggiungimento della versione 1.0. Va notato che questa roadmap è da applicarsi esclusivamente al software della blockchain e non ad altri strumenti e risorse utili come wallet e esploratori di blocchi (c.d. block explorers) che avranno i propri team e le proprie roadmap dedicate una volta raggiunto il completamento della Fase 1.

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

### Pulizia WASM & Sandboxing della CPU (Brian)

Il codice WASM dev'essere ripulito per poter verificare comportamenti non deterministici come operazioni in virgola mobile (FLOP - floating point operations) e loop infiniti.

### Monitoraggio Dell'Uso delle Risorse & Rate Limiting (Arhag)

Per prevenire abusi della rete, il monitoraggio delle risorse e il tasso di controllo dell'utilizzo, limita gli utenti in base agli EOS in riserva (c.d. staked)

### Test D'Importazione di Genesi (DappHub)

È necessario sviluppare strumenti per esportare i dati dallo stato della distribuzione dei Token di EOS e creare un file di configurazione genesi. Ciò consentirà a chiunque partecipi alla distribuzione dei Token di acquisire qualche EOS di test iniziale (TEOS).

### Comunicazione Interblockchain (Nathan)

Questa funzionalità implica che la verifica dell'hash di Merkle delle transazioni sia corretta. 

# Fase 3 - Test & Verifiche di Sicurezza - Inverno 2017, Primavera 2018

Durante questa fase la piattaforma sarà sottoposta a test approfonditi con particolare attenzione alla ricerca di problemi di sicurezza e bug. Alla fine della Fase 3 la versione 1.0 verrà contrassegnata.

### Sviluppo di Esempi di Applicazioni 

Gli esempi di applicazioni sono fondamentali per dimostrare che la piattaforma fornisce le funzionalità richieste dai veri sviluppatori.

### Ricompense per chi Attacca con Successo la Rete

Attaccare la rete con spam, exploit di macchine virtuali, crash causati da bug, e comportamenti non deterministici sarà un processo molto complicato, ma ciò è necessario per assicurare che la versione 1.0 sia stabile.

### Supporto di Linguaggi di Programmazione

Aggiunta del supporto per ulteriori linguaggi di programmazione da compilare in WASM: C++, Rust, etc.

### Documentazione & Tutorials

# Fase 4 - Ottimizzazione Parallela - Estate / Autunno 2018

Dopo aver rilasciato la versione 1.0 stabile, ci avvieremo nell'ottimizzazione del codice per l'esecuzione parallela.

# Fase 5 - Implementazione del Cluster - In Futuro