# White Paper Tecnico di EOS.IO 

**26 giugno 2017**

**Sommario:** Il software di EOS.IO introduce una nuova architettura blockchain progettata per abilitare la scalabilità verticale e orizzontale delle applicazioni decentralizzate. Ciò si ottiene creando un asset simile ad un sistema operativo su cui possano essere sviluppate le applicazioni. Il software fornisce account, autenticazione, database, comunicazione asincrona e la pianificazione di applicazioni su centinaia di core di CPU o cluster. La tecnologia risultante è un'architettura blockchain scalabile e che si adatta a milioni di transazioni al secondo, elimina le commissioni dell'utente e consente una distribuzione rapida e semplice delle applicazioni decentralizzate.

**NOTA BENE: I TOKEN CRITTOGRAFICI DI CUI QUESTO WHITE PAPER FA RIFERIMENTO SONO I TOKEN CRITTOGRAFICI LANCIATI SULLA BLOCKCHAIN CHE UTILIZZA IL SOFTWARE EOS.IO. NON FANNO RIFERIMENTO AI TOKEN ERC-20 CHE SONO DISTRIBUITI SULLA BLOCKCHAIN DI ETHEREUM IN RELAZIONE ALLA DISTRIBUZIONE DEI TOKEN DI EOS.**

Copyright © 2017 block.one

Chiunque può utilizzare, riprodurre o distribuire qualsiasi materiale in questo white paper per uso non commerciale ed educativo (ad esempio, a pagamento o per scopi commerciali) senza permesso, a condizione che la fonte originale e l'avviso del copyright vengano citati.

**DISCLAIMER:** Questo White Paper Tecnico di EOS.IO è fornito a solo scopo informativo. block.one non garantisce l'accuratezza o le conclusioni tratte da questo white paper, inoltre questo white paper è fornito "così com'è". block.one non produce e nega espressamente tutte le dichiarazioni e garanzie, espresse, implicite, statutarie o di altro tipo, incluse, a titolo esemplificativo ma non esaustivo le seguenti: (i) garanzie di commerciabilità, idoneità per uno scopo particolare, adeguatezza, utilizzo, titolo o non violazione; (ii) che il contenuto di questo white paper sia esente da errori; e (iii) che tali contenuti non violino i diritti di terzi. block.one e le sue affiliate non hanno alcuna responsabilità per danni di qualsiasi tipo derivanti dall'uso, riferimento o affidamento su questo white paper o qualsiasi concetto qui contenuto, anche se avvisati della possibilità di tali danni. In nessun caso block.one oi suoi affiliati saranno ritenuti responsabili verso qualsiasi persona o entità per eventuali danni, perdite, responsabilità, costi o spese di qualsiasi tipo, diretti o indiretti, consequenziali, compensativi, incidentali, effettivi, esemplari, punitivi o speciali per l'uso, il riferimento a, o fare affidamento su questo white paper o qualsiasi contenuto qui compreso, inclusi, a titolo esemplificativo, eventuali perdite di affari, ricavi, profitti, dati, utilizzo, benevolenza o altre perdite intangibili.

- [Contesto](#background)
- [Requisiti per le applicazioni Blockchain](#requirements-for-blockchain-applications) 
  - [Supporto di Milioni di Utenti](#support-millions-of-users)
  - [Utilizzo Gratuito](#free-usage)
  - [Aggiornamenti Semplici e Correzione di Bug](#easy-upgrades-and-bug-recovery)
  - [Bassa Latenza](#low-latency)
  - [Prestazioni Sequenziali](#sequential-performance)
  - [Prestazioni Parallele](#parallel-performance)
- [Algoritmi di Consensus (DPOS)](#consensus-algorithm-dpos) 
  - [Conferma della transazione](#transaction-confirmation)
  - [Transazione come Proof of Stake (TaPoS)](#transaction-as-proof-of-stake-tapos)
- [Account](#accounts) 
  - [Messaggi & Gestori](#messages--handlers)
  - [Gestione dei Permessi Basata sui Ruoli](#role-based-permission-management) 
    - [Livelli di Autorizzazione Denominati](#named-permission-levels)
    - [Gruppi di Gestori di Messaggi Denominati](#named-message-handler-groups)
    - [Mappatura delle Autorizzazioni](#permission-mapping)
    - [Valutazione delle Autorizzazioni](#evaluating-permissions) 
      - [Gruppi di Autorizzazione Predefiniti](#default-permission-groups)
      - [Valutazione Parallela delle Autorizzazioni](#parallel-evaluation-of-permissions)
  - [Messaggi con Ritardo Obbligatorio](#messages-with-mandatory-delay)
  - [Recupero da Chiavi Rubate](#recovery-from-stolen-keys)
- [Esecuzione Parallela Deterministica delle Applicazioni](#deterministic-parallel-execution-of-applications) 
  - [Riduzione della Latenza delle Comunicazioni](#minimizing-communication-latency)
  - [Gestori di Messaggi di Sola Lettura](#read-only-message-handlers)
  - [Transazioni Atomiche con Multipli Account](#atomic-transactions-with-multiple-accounts)
  - [Valutazione Parziale dello stato della Blockchain](#partial-evaluation-of-blockchain-state)
  - [Miglior Sforzo Soggettivo di Pianificazione](#subjective-best-effort-scheduling)
- [Modello di Token e Utilizzo delle Risorse](#token-model-and-resource-usage) 
  - [Misure Oggettive e Soggettive](#objective-and-subjective-measurements)
  - [Pagamento Ricevitore](#receiver-pays)
  - [Capacità di Delegazione](#delegating-capacity)
  - [Separazione dei Costi di Transazione dal Valore dei Token](#separating-transaction-costs-from-token-value)
  - [Costi di Archiviazione dello Stato](#state-storage-costs)
  - [Ricompense di blocco](#block-rewards)
  - [Applicazioni a Beneficio della Comunità](#community-benefit-applications)
- [Governanza](#governance) 
  - [Congelamento dei Conti](#freezing-accounts)
  - [Modifica del Codice dell'Account](#changing-account-code)
  - [Costituzione](#constitution)
  - [Aggiornamento del Protocollo & Costituzione](#upgrading-the-protocol--constitution) 
    - [Modifiche di Emergenza](#emergency-changes)
- [Script & Macchine virtuali](#scripts--virtual-machines) 
  - [Messaggi Definiti dallo Schema](#schema-defined-messages)
  - [Database Definito dallo Schema](#schema-defined-database)
  - [Separazione dell'Autenticazione dall'Applicazione](#separating-authentication-from-application)
  - [Architettura Indipendente della Macchina Virtuale](#virtual-machine-independent-architecture) 
    - [Web Assembly (WASM)](#web-assembly-wasm)
    - [Macchina Virtuale di Ethereum (EVM)](#ethereum-virtual-machine-evm)
- [Comunicazione Inter-Blockchain](#inter-blockchain-communication) 
  - [Prova di Merkle per la Validazione del Light Client (LVC)](#merkle-proofs-for-light-client-validation-lcv)
  - [Latenza della Comunicazione Interchain](#latency-of-interchain-communication)
  - [Prova di Completezza](#proof-of-completeness)
- [Conclusione](#conclusion)

# Contesto

La tecnologia Blockchain è stata introdotta nel 2008 con il lancio di Bitcoin e da allora imprenditori e sviluppatori hanno tentato di generalizzare la tecnologia in modo da supportare una sempre più ampia gamma di applicazioni su un'unica piattaforma blockchain.

Sebbene un certo numero di piattaforme blockchain ha faticato a supportare applicazioni decentralizzate funzionali, specifiche applicazioni basate su blockchain come l'exchange decentralizzato BitShares (2014) e la piattaforma di social media chiamata Steem (2016) sono diventate massivamente utilizzate con decine di migliaia di utenti attivi quotidianamente. Hanno ottenuto questo risultato aumentando le prestazioni fino a migliaia di transazioni al secondo, riducendo la latenza a 1,5 secondi, eliminando le tasse e fornendo un'esperienza utente simile a quelle attualmente fornite dai servizi centralizzati già esistenti.

Le piattaforme di blockchain esistenti sono gravate da tariffe elevate e da una capacità computazionale limitata che impedisce l'adozione diffusa della blockchain.

# Requisiti per le applicazioni Blockchain

Per ottenere un uso diffuso, le applicazioni sulla blockchain richiedono una piattaforma sufficientemente flessibile che possa soddisfare i seguenti requisiti:

## Supporto di Milioni di Utenti

Per rivoluzionare imprese come Ebay, Uber, AirBnB e Facebook si richiede una tecnologia blockchain in grado di gestire decine di milioni di utenti attivi giornalmente. In alcuni casi, le applicazioni potrebbero non funzionare a meno che non venga raggiunta una massa critica di utenti e quindi è fondamentale avere una piattaforma in grado di gestire una grande quantità di utenti.

## Utilizzo Gratuito

Gli sviluppatori di applicazioni hanno bisogno della flessibilità di offrire agli utenti servizi gratuiti; gli utenti non dovrebbero pagare per utilizzare la piattaforma o per beneficiare dei suoi servizi. Una piattaforma blockchain gratuita ha una probabilità di ottenere una diffusione di adozione molto alta. Gli sviluppatori e le aziende possono quindi creare strategie di monetizzazione efficaci.

## Aggiornamenti Semplici e Correzione di Bug

Le aziende che creano applicazioni basate su una blockchain necessitano la flessibilità di migliorare le loro applicazioni con funzionalità nuove.

Tutti i software non banali sono soggetti a bug, anche con la verifica più rigorosa. La piattaforma deve essere abbastanza robusta da poter correggere bug quando questi si verificano inevitabilmente.

## Bassa Latenza

Una buona esperienza utente richiede feedback affidabili con un ritardo di non più di pochi secondi. Ritardi più lunghi causano frustrazione negli utenti e rendono le applicazioni basate su una blockchain meno competitive in confronto alle alternative esistenti non basate su blockchain.

## Prestazioni Sequenziali

Ci sono alcune applicazioni che non possono essere implementate con algoritmi paralleli a causa di passi sequenzialmente dipendenti. Applicazioni come gli exchange richiedono prestazioni sequenziali sufficienti a gestire elevati volumi e pertanto è necessaria una piattaforma con prestazioni sequenziali molto veloce.

## Prestazioni Parallele

Le applicazioni su larga scala devono dividere il carico di lavoro su multiple CPU e più computer.

# Algoritmi di Consensus (DPOS)

Il software EOS.IO utilizza l'unico algoritmo di consensus decentralizzato in grado di soddisfare i requisiti prestazionali delle applicazioni sulla blockchain,  Delegated Proof of Stake (DPOS) </ 0>. In base a questo algoritmo, coloro che detengono i token su una blockchain che adotta il software EOS.IO possono selezionare produttori di blocchi tramite un sistema di approvazione di votazione continua e chiunque può scegliere di partecipare alla produzione di blocchi, inoltre verrà data l'opportunità di produrre blocchi proporzionali ai voti totali che hanno ricevuto rispetto a tutti gli altri produttori. Per le blockchain private la gestione potrebbe utilizzare i token per aggiungere e rimuovere il personale informatico.</p> 

Il software EOS.IO consente di produrre blocchi esattamente ogni 3 secondi e un produttore è autorizzato a produrre un blocco in qualsiasi momento. Se il blocco non viene prodotto all'orario pianificato, il blocco per quella fascia oraria viene trascurato. Quando uno o più blocchi vengono trascurati, vi è un divario di 6 o più secondi nella blockchain.

Utilizzando il software EOS.IO i blocchi sono prodotti a turni (c.d. rounds) di 21. All'inizio di ogni turno vengono scelti 21 produttori di blocchi unici. I primi 20 per approvazione totale vengono automaticamente scelti ad ogni turno e l'ultimo produttore viene scelto in proporzione al numero di voti rispetto agli altri. I produttori selezionati vengono mescolati usando un numero pseudocasuale derivato dal tempo di blocco. Questo mescolamento è fatto per garantire che tutti i produttori mantengano una connettività bilanciata verso tutti gli altri produttori.

Se un produttore salta un blocco e non ha prodotto alcun blocco nelle ultime 24 ore, non viene più considerato fino a quando non notifica alla blockchain la propria intenzione di iniziare a produrre nuovamente altri blocchi. Ciò garantisce che la rete funzioni senza intoppi riducendo al minimo il numero di blocchi persi non programmando quelli che si sono dimostrati inaffidabili.

In condizioni normali, una blockchain DPOS non è impattada da alcun fork perché i produttori di blocchi non competono ma cooperano per produrre i blocchi. Nel caso in cui vi sia un fork, il consenso (c.d. consensus) passerà automaticamente alla catena più lunga. Questa metrica funziona perché la velocità con cui i blocchi vengono aggiunti ad un fork della chain di una blockchain è direttamente correlata alla percentuale di produttori di blocchi che condividono lo stesso consenso (c.d. consensus). In altre parole, un fork della blockchain con più produttori su di essa crescerà più velocemente di una con meno produttori. Inoltre, nessun produttore di blocchi dovrebbe produrre blocchi su due fork allo stesso tempo. Se un produttore di blocchi viene sorpreso a fare ciò, tale produttore di blocchi verrà probabilmente rimosso. È inoltre possibile utilizzare prove crittografiche della doppia produzione per rimuovere in modo automatico gli autori di questi abusi.

## Conferma della transazione

Le tipiche blockchain DPOS hanno una partecipazione da parte dei produttori di blocchi del 100%. Una transazione può essere considerata confermata con una certezza del 99,9% dopo una media di 1,5 secondi dal momento della trasmissione.

Esistono casi straordinari in cui un bug del software, una congestione della rete Internet o un produttore di blocchi dannoso creino due o più fork. Per avere la certezza assoluta che una transazione sia irreversibile, un nodo può scegliere di attendere la conferma di 15 dei 21 produttori di blocchi. Basandosi su una configurazione tipica del software EOS.IO, questa operazione in circostanze normali richiederà circa 45 secondi. Di default, tutti i nodi considerano irreversibile un blocco confermato da 15 dei 21 produttori e non passeranno a un fork che esclude tale blocco indipendentemente dalla lunghezza.

È possibile che un nodo avvisi gli utenti che esiste un'alta probabilità che si trovino su un fork minoritario entro 9 secondi dall'avvio di un fork. Dopo 2 blocchi mancati consecutivamente c'è una probabilità del 95% che un nodo si trovi su un fork minoritario. Con 3 blocchi mancati consecutivamente c'è una certezza del 99% di essere su un fork minoritario. È possibile generare un modello predittivo affidabile che utilizzi le informazioni su quali nodi sono stati persi, sui tassi di partecipazione recenti e su altri fattori in modo da avvisare rapidamente gli operatori che c'è qualcosa non va.

La risposta di tale avviso dipende interamente dalla natura delle transazioni commerciali, ma la risposta più semplice è di attendere le 15/21 conferme fino a quando l'avviso non si fermi.

## Transazione come Proof of Stake (TaPoS)

Il software EOS.IO richiede che ogni transazione includa l'hash di un'intestazione di un blocco recente. Questo hash ha due scopi:

1. impedisce la riproduzione di una transazione sui fork che non includono il blocco di riferimento; e
2. segnala alla rete che un particolare utente e la sua quota di partecipazione (c.d. stake) si trovano su un fork specifico.

Col tempo, tutti gli utenti confermeranno direttamente la blockchain rendendo difficile falsificare le chain contraffatte poiché la contraffazione non sarebbe in grado di migrare le transazioni dalla chain legittima.

# Account

Il software EOS.IO consente a tutti gli account di essere referenziati da un nome unico e leggibile di 2 fino a 32 caratteri. Il nome è scelto dal creatore dell'account. Tutti i conti devono essere finanziati con un saldo nel conto minimo questo avviene nel momento in cui vengono creati in modo da coprire il costo di archiviazione dei dati dell'account. I nomi degli account hanno anche il supporto dei namespace in modo tale che il proprietario dell'account @domain sia l'unico che può creare l'account @user.domain.

In un contesto decentralizzato, gli sviluppatori di applicazioni pagheranno il costo nominale della creazione dell'account per poter iscrivere un nuovo utente. Le imprese tradizionali già spendono ingenti somme di denaro per ogni cliente, questo avviene sotto forma di pubblicità, servizi gratuiti, ecc. Il costo del finanziamento di un nuovo account nella blockchain dovrebbe essere insignificante in confronto. Fortunatamente, non è necessario creare account per gli utenti già registrati in un'altra applicazione.

## Messaggi & Gestori

Ogni account può inviare messaggi strutturati ad altri account e può definire script per gestire i messaggi quando vengono ricevuti. Il software EOS.IO assegna ad ogni account il proprio database privato a cui possono accedere solo i propri gestori (c.d. handlers) di messaggi. Gli script di gestione messaggi possono anche inviare messaggi ad altri account. La combinazione tra i messaggi e i gestori automatici di messaggi definiscono il modo in cui EOS.IO gestisce i contratti intelligenti.

## Gestione dei Permessi Basata sui Ruoli

La gestione delle autorizzazioni include il determinare se un messaggio è correttamente autorizzato o meno. La forma più semplice di gestione dei permessi consiste nel controllare che una transazione abbia le firme richieste, ma questo implica che le firme richieste siano già note. Generalmente l'autorità è legata a individui o gruppi di individui ed è spesso compartimentata. Il software EOS.IO fornisce un sistema di gestione dei permessi dichiarativo che offre agli account un controllo minuzioso e ad alto livello sul chi può fare cosa e quando lo si può fare.

È fondamentale che l'autenticazione e la gestione dei permessi siano standardizzati e separati dalla logica aziendale dell'applicazione. Ciò consente di sviluppare strumenti per gestire le autorizzazioni in modo generico e offre anche opportunità significative per l'ottimizzazione delle prestazioni.

Ogni account può essere controllato da una combinazione di altri account e chiavi private. Ciò crea una struttura autoritaria gerarchica che riflette il modo in cui le autorizzazioni sono organizzate e semplifica il controllo multiutente sui fondi. Il controllo multiutente è il singolo e più grande contributore alla sicurezza e, se usato correttamente, può eliminare notevolmente il rischio di furti dovuti a hacking.

Il software EOS.IO consente agli account di definire quale combinazione di chiavi e/o account può inviare un determinato tipo di messaggio a un'altro account. Ad esempio, è possibile avere una chiave per un social media account di un utente e un'altra per l'accesso all'exchange. È anche possibile concedere ad altri account il permesso di agire per conto dell'account di un utente senza assegnargli le chiavi.

### Livelli di Autorizzazione Denominati

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Utilizzando il software EOS.IO, ciascun account può definire livelli di autorizzazione denominati che possono essere derivate da autorizzazioni denominate di livello superiore. Ogni livello di autorizzazione nominato definisce un'autorità; un'autorità è un controllo a più firme costituito da chiavi e/o livelli di autorizzazione denominati di altri account. Ad esempio, è possibile impostare il livello di autorizzazione "Amico" di un account affinché l'account sia controllato in modo equo da tutti gli "amici" dell'account.

Un altro esempio è la blockchain di Steem che ha tre livelli di autorizzazione nominati e codificati: proprietario, attivo e pubblicazione. L'autorizzazione alla pubblicazione può solo eseguire azioni sociali come il voto e la pubblicazione, mentre l'autorizzazione attiva può fare tutto tranne cambiare il proprietario. L'autorizzazione del proprietario è pensata per la conservazione a lungo termine ed è in grado di gestire qualunque cosa. Il software EOS.IO generalizza questo concetto consentendo a ciascun titolare dell'account di definire la propria gerarchia e il proprio raggruppamento di azioni.

### Gruppi di Gestori di Messaggi Denominati

Il software EOS.IO consente a ciascun account di organizzare i propri gestori di messaggi in gruppi denominati e nidificati. Questi gruppi di gestori di messaggi denominati possono essere referenziati da altri account quando configurano i loro livelli di autorizzazione.

Il gruppo di gestori di messaggi di livello più alto rappresenta il nome dell'account, mentre il livello più basso è il tipo di messaggio singolo ricevuto dall'account. Questi gruppi possono essere referenziati in questo modo: **@accountname.groupa.subgroupb.MessageType**.

In base a questo modello, è possibile che un contratto di exchange raggruppi la creazione e l'annullamento dell'ordine separatamente dal deposito e dal prelievo. Questo raggruppamento in base al contratto di exchange è una comodità per gli utenti dell'exchange.

### Mappatura delle Autorizzazioni

Il software EOS.IO consente a ciascun account di definire una mappatura tra un gruppo di gestori di messaggi denominati di qualsiasi account e il proprio Livello di Autorizzazione Denominato. Ad esempio, un titolare del conto può mappare il titolare del conto di un applicazione social media al gruppo di autorizzazioni "Amico" del titolare del conto. Con questa mappatura, qualsiasi "amico" può postare come possessore dell'account sui social media del titolare dell'account. Anche se pubblicherebbero come titolari dell account, utilizzerebbero comunque le proprie chiavi per firmare il messaggio. Ciò significa che è sempre possibile identificare quali amici hanno utilizzato l'account e in che modo.

### Valutazione delle Autorizzazioni

Quando si consegna un messaggio di tipo "**Action**", da **@alice** a **@bob** il software EOS.IO prima di tutto verificherà se **@alice** ha definito una mappatura delle autorizzazioni per **@bob.groupa.subgroup.Action**. Se non viene trovato nulla, sarà controllata una mappatura per **@bob.groupa.subgroup**, **@bob.groupa**, e infine **@bob**. Se non viene trovata alcuna ulteriore corrispondenza, la mappatura ipotizzata si troverà nel gruppo di autorizzazioni denominato **@alice.active**.

Una volta identificata una mappatura, l'autorizzazione di firma viene convalidata utilizzando il processo di firma multipla e l'autorizzazione associata all'autorizzazione denominata. Se ciò dovesse fallire, allora passerà al permesso principale e, in definitiva, all'autorizzazione del proprietario, **@alice.owner**.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Gruppi di Autorizzazione Predefiniti

La tecnologia EOS.IO consente inoltre a tutti gli account di avere un gruppo "proprietario" che può fare tutto, e un gruppo "attivo" che può fare tutto tranne cambiare il gruppo proprietario. Tutti gli altri gruppi di autorizzazione sono derivati da quelli "attivi".

#### Valutazione Parallela delle Autorizzazioni

Il processo di valutazione dell'autorizzazione è "di sola lettura" e le modifiche alle autorizzazioni effettuate dalle transazioni non hanno effetto fino alla fine di un blocco. Ciò significa che tutte le chiavi e la valutazione delle autorizzazioni per tutte le transazioni possono essere eseguite in parallelo. Inoltre, ciò significa che è possibile una rapida convalida dell'autorizzazione evitando l'avviamento della costosa logica di applicazione che dovrebbe poi essere ripristinata. Infine, ciò vuol dire che le autorizzazioni delle transazioni ricevute possono essere valutate come transazioni in sospeso e non devono essere rivalutate man mano che vengono applicate.

Tutto sommato, la verifica dell'autorizzazione rappresenta una percentuale significativa del calcolo richiesto per convalidare le transazioni. Rendendo questo un processo di sola lettura e parallelizzabile consente un notevole aumento delle prestazioni.

Quando si riproduce la blockchain per rigenerare lo stato deterministico dal registro dei messaggi, non è necessario rivalutare le autorizzazioni. Il fatto che una transazione sia inclusa in un blocco valido noto è sufficiente per saltare questo passaggio. Questo riduce drasticamente il carico computazionale associato alla riproduzione di una blockchain in continua crescita.

## Messaggi con Ritardo Obbligatorio

Il tempo è un componente fondamentale della sicurezza. Nella maggior parte dei casi, non è possibile sapere se una chiave privata è stata rubata fino a quando non è stata utilizzata. La sicurezza basata sul tempo è ancora più importante quando le persone hanno applicazioni che richiedono che le chiavi vengano conservate su computer connessi a Internet per l'uso quotidiano. Il software EOS.IO consente agli sviluppatori di applicazioni di indicare che alcuni messaggi devono attendere un periodo di tempo minimo dopo essere stati inclusi in un blocco prima che possano essere applicati. Durante questo periodo possono essere cancellati.

Gli utenti possono quindi ricevere notifiche tramite e-mail o Sms quando uno di questi messaggi viene trasmesso. Se non l'hanno autorizzato, possono utilizzare la procedura di recupero dell'account per recuperare il proprio account e ritirare il messaggio.

Il ritardo richiesto dipende dal grado di sensibilità di un'operazione. Pagare un caffè può non avere ritardi ed essere irreversibile in pochi secondi, mentre l'acquisto di una casa può richiedere un periodo di compensazione di 72 ore. Il trasferimento di un intero account a un nuovo controllo può richiedere fino a 30 giorni. I ritardi esatti scelti dipendono dagli sviluppatori e dagli utenti dell'applicazione in questione.

## Recupero da Chiavi Rubate

Il software EOS.IO offre agli utenti un modo per ripristinare il controllo del proprio account quando le loro chiavi vengono rubate. Il proprietario di un account può utilizzare qualsiasi chiave del proprietario che fosse attiva negli ultimi 30 giorni e approvata dal partner di recupero dell'account designato per poter reimpostare la chiave del proprietario sul proprio account. Il partner per il recupero dell'account non può reimpostare il controllo dell'account senza l'aiuto del proprietario.

Non c'è nulla che l'hacker possa ottenere tentando di passare attraverso il processo di recupero perché già "controllano" l'account. Inoltre, se avessero seguito il processo, il partner per il recupero avrebbe probabilmente richiesto l'identificazione e l'autenticazione a più fattori (telefono ed e-mail). Ciò probabilmente comprometterebbe o non farebbe ottenere nulla all'hacker durante la procedura.

Questo processo è anche molto diverso da una semplice disposizione multi-firma. Con una transazione multi-firma, c'è un'altra società che prende parte a ogni transazione che viene eseguita, ma con la procedura di recupero l'agente è solo una parte del processo di ripristino e non ha alcun potere sulle transazioni giornaliere. Questo riduce drasticamente costi e responsabilità legali per tutti i soggetti coinvolti.

# Esecuzione Parallela Deterministica delle Applicazioni

Il consensus o consenso della blockchain dipende dal comportamento deterministico (riproducibile). Ciò significa che tutte le esecuzioni parallele devono essere libere dall'uso di mutex o altri chiusure primitive (c.d. locking primitives). Senza chiusure ci deve essere un modo per garantire che tutti gli account possano solo leggere e scrivere il proprio database privato. Significa anche che ciascun account elabora i messaggi in sequenza e che il parallelismo sarà a livello di account.

In una blockchain basata sul software EOS.IO, è compito del produttore di blocchi di organizzare il recapito dei messaggi in thread indipendenti in modo che possano essere valutati in parallelo. Lo stato di ciascun account dipende solo dai messaggi consegnati a quest'ultimo. L'agenda (c.d. schedule) è l'output di un produttore di blocchi e verrà eseguito in modo deterministico, ma il processo per generare l'agenda non deve essere deterministico. Ciò significa che i produttori di blocchi possono utilizzare algoritmi paralleli per pianificare le transazioni.

Parte dell'esecuzione parallela significa che quando uno script genera un nuovo messaggio non viene consegnato immediatamente, ma è pianificato per essere consegnato nel ciclo successivo. Il motivo per cui non può essere consegnato immediatamente è perché il ricevitore potrebbe modificare attivamente il proprio stato in un altro thread.

## Riduzione della Latenza delle Comunicazioni

La latenza è il tempo impiegato da un'account per inviare un messaggio a un altro account per poi ricevere una risposta. L'obiettivo è di consentire a due account di scambiare messaggi tra di loro all'interno di un singolo blocco senza dover attendere 3 secondi tra ogni messaggio. Per abilitare ciò, il software EOS.IO divide ciascun blocco in cicli. Ogni ciclo è diviso in thread e ogni thread contiene un elenco di transazioni. Ogni transazione contiene un insieme di messaggi da consegnare. La sua struttura può essere visualizzata come un albero in cui i layers alternati vengono elaborati in sequenza e in parallelo.

        Blocco
    
          Cicli (sequenziali)
    
            Threads (paralleli)
    
              Transazioni (sequenziali)
    
                Messaggi (sequenziali)
    
                  Ricevitore e Account Notificati (paralleli)
    

Le transazioni generate in un ciclo possono essere consegnate in qualsiasi ciclo o blocco successivo. I produttori di blocchi continueranno ad aggiungere cicli a un blocco fino a quando non sarà trascorso il tempo massimo di "wall time" o non ci saranno nuove transazioni generate da consegnare.

È possibile utilizzare l'analisi statica di un blocco per verificare che all'interno di un determinato ciclo non vi siano due thread contenenti transazioni che modificano lo stesso account. Finché tale invariante viene mantenuto, è possibile elaborare un blocco eseguendo tutti i thread in parallelo.

## Gestori di Messaggi di Sola Lettura

Alcuni account potrebbero essere in grado di elaborare un messaggio su base pass/fail senza modificare il loro stato interno. In tal caso, questi gestori possono essere eseguiti in parallelo a condizione che solo i gestori di messaggi di sola lettura per un determinato account siano inclusi in uno o più thread all'interno di un particolare ciclo.

## Transazioni Atomiche con Multipli Account

A volte è desiderabile garantire che i messaggi vengano consegnati e accettati da più account atomicamente. In questo caso entrambi i messaggi vengono inseriti in una transazione e ad entrambi gli account verrà assegnato lo stesso thread e i messaggi verranno applicati in sequenza. Questa situazione non è ideale per le prestazioni e quando si tratta "di fatturare" gli utenti per l'utilizzo, verranno fatturati dal numero di account univoci a cui fa riferimento una transazione.

Per motivi di prestazioni e costi, è meglio ridurre al minimo le operazioni atomiche che coinvolgono due o più account molto utilizzati.

## Valutazione Parziale dello stato della Blockchain

La scalabilità della tecnologia blockchain richiede che i componenti siano modulari. Nessuno dovrebbe eseguire qualcosa, soprattutto se hanno solo bisogno di utilizzare un piccolo sottoinsieme delle applicazioni.

Uno sviluppatore di un exchange esegue i nodi completi allo scopo di mostrare lo stato degli scambi ai propri utenti. Questo exchange non ha bisogno dello stato associato a applicazioni come i social media. Il software EOS.IO consente a qualsiasi nodo completo di selezionare qualsiasi sottoinsieme di applicazioni da eseguire. I messaggi consegnati ad altre applicazioni vengono ignorati in modo sicuro perché lo stato di un'applicazione deriva interamente dai messaggi che vengono consegnati.

Ciò ha alcune implicazioni significative sulla comunicazione con altri account. In particolare non si può presumere che lo stato dell'altro account sia accessibile sulla stessa macchina. Ciò significa che mentre si è tentati di abilitare i "blocchi" che consentono a un account di chiamare in modo sincrono un altro account, questo modello di progettazione si interrompe se l'altro account non è residente in memoria.

Tutte le comunicazioni di stato tra gli account devono essere passate tramite i messaggi inclusi nella blockchain.

## Miglior Sforzo Soggettivo di Pianificazione

Il software EOS.IO non può obbligare i produttori di blocchi a consegnare alcun messaggio a nessun altro account. Ogni produttore di blocchi rende la propria misurazione soggettiva della complessità computazionale e del tempo richiesto per elaborare una transazione. Questo vale sia che una transazione sia generata da un utente o che sia generata automaticamente da uno script.

In una blockchain operativa che adotta il software EOS.IO, a livello di rete tutte le transazioni vengono fatturate a un costo di bandwidth fisso, indipendentemente dal fatto che siano stati eseguiti 0,01 ms o 10 ms per eseguirlo. Tuttavia, ogni singolo produttore di blocchi che utilizza il software può calcolare l'utilizzo delle risorse utilizzando il proprio algoritmo e le proprie misure. Quando un produttore di blocchi conclude che una transazione o un conto ha consumato una quantità sproporzionata della capacità computazionale possono semplicemente rifiutare la transazione quando producono il proprio blocco; tuttavia, elaboreranno ancora la transazione se altri produttori di blocchi lo ritengono valido.

In generale, purché anche solo un produttore di blocchi consideri una transazione valida e ai limiti dell'utilizzo delle risorse, tutti gli altri produttori di blocchi lo accetteranno, ma potrebbe essere necessario fino a 1 minuto necessari alla transazione per trovare quel produttore.

In alcuni casi, un produttore può creare un blocco che include transazioni di un ordine di grandezza al di fuori degli intervalli accettabili. In questo caso il produttore del blocco successivo può scegliere di rifiutare il blocco e il legame verrà interrotto dal terzo produttore. Ciò non è diverso da quello che succederebbe se un blocco di grande dimensioni causasse ritardi nella propagazione della rete. La comunità noterebbe un modello di abuso e avrebbe rimosso i voti dal produttore disonesto.

Questa valutazione soggettiva del costo computazionale libera la blockchain dal bisogno di misurare in modo preciso e deterministico quanto tempo ci vuole per eseguire. Con questo design non è necessario contare con precisione le istruzioni ciò aumenta notevolmente le opportunità di ottimizzazione senza infrangere il consensus/consenso.

# Modello di Token e Utilizzo delle Risorse

**NOTA BENE: I TOKEN CRITTOGRAFICI DI CUI QUESTO WHITE PAPER FA RIFERIMENTO SONO I TOKEN CRITTOGRAFICI LANCIATI SULLA BLOCKCHAIN CHE UTILIZZA IL SOFTWARE EOS.IO. NON FANNO RIFERIMENTO AI TOKEN ERC-20 CHE SONO DISTRIBUITI SULLA BLOCKCHAIN DI ETHEREUM CONNESSI ALLA DISTRIBUZIONE DEI TOKEN DI EOS. **

Tutte le blockchain sono limitate in termini di risorse e richiedono un sistema per prevenire gli abusi. Con una blockchain che utilizza il software EOS.IO, vi sono tre ampie classi di risorse che vengono utilizzate dalle applicazioni:

1. Larghezza di banda e archiviazione dei registri (disco);
2. Computazione e Backlog Computazionale (CPU); e
3. Archiviazione di Stato (RAM).

Il bandwidth e il calcolo computazionale hanno due componenti, l'uso istantaneo e l'uso a lungo termine. Una blockchain mantiene un registro di tutti i messaggi e questo registro viene infine archiviato e scaricato da tutti i nodi completi. Con il registro dei messaggi è possibile ricostruire lo stato di tutte le applicazioni.

Il debito computazionale è formato da calcoli che devono essere eseguiti per rigenerare lo stato dal registro messaggi. Se il debito computazionale diventa troppo grande allora diventa necessario fare uno snapshot dello stato della blockchain e scartare la storia della blockchain. Se il debito computazionale cresce troppo rapidamente, potrebbero essere necessari 6 mesi per ripetere il valore di 1 anno di transazioni. È quindi fondamentale che il debito computazionale sia gestito con cura.

L'archiviazione di stato della blockchain è un'informazione accessibile dalla logica dell'applicazione. Include informazioni quali ordini e saldi contabili. Se lo stato non viene mai letto dall'applicazione, non dovrebbe essere memorizzato. Ad esempio, i contenuti dei post di un blog e i commenti non vengono letti dalla logica dell'applicazione, quindi non devono essere archiviati nello stato della blockchain. Nel frattempo l'esistenza di un post/commento, il numero di voti e altre proprietà vengono memorizzate come parte dello stato della blockchain.

I produttori di blocchi pubblicano la capacità a loro disponibile per la bandwidth, per il calcolo e per lo stato. Il software EOS.IO consente a ciascun account di utilizzare una percentuale della capacità disponibile proporzionale alla quantità di token detenuti in un contratto di staking di 3 giorni. Ad esempio, se viene lanciata una blockchain basata sul software EOS.IO e se un account detiene l'1% del totale dei token distribuibili in base a tale blockchain, allora tale account ha il potenziale di utilizzare l'1% della capacità di archiviazione di stato.

L'adozione del software EOS.IO su una blockchain in esecuzione significa che la bandwidth e la capacità computazionale sono allocate su una base di riserva frazionaria poiché sono transitori (la capacità non utilizzata non può essere salvata per un uso futuro). L'algoritmo utilizzato dal software EOS.IO è simile all'algoritmo utilizzato da Steem per il limite di frequenza della bandwidth usata.

## Misure Oggettive e Soggettive

Come discusso in precedenza, la strumentazione dell'uso computazionale ha un impatto significativo sulle prestazioni e sull'ottimizzazione; pertanto, tutti i vincoli di utilizzo delle risorse sono in definitiva soggettivi e l'applicazione viene eseguita dai produttori di blocchi in base ai loro singoli algoritmi e le loro stime.

Detto questo, ci sono alcune cose che sono triviali da misurare obiettivamente. Il numero di messaggi consegnati e la dimensione dei dati memorizzati nel database interno sono semplici e possono essere misurati oggettivamente. Il software EOS.IO consente ai produttori di blocchi di applicare lo stesso algoritmo su queste misure oggettive, ma si può scegliere di applicare algoritmi soggettivi più rigidi rispetto alle misurazioni soggettive.

## Pagamento Ricevitore

Tradizionalmente, è l'azienda che paga per lo spazio di un ufficio oltre che alla potenza di calcolo e altri costi necessari per gestire l'attività. Il cliente acquista prodotti specifici dall'azienda e le entrate derivanti da tali vendite di prodotti vengono utilizzate per coprire i costi operativi dell'attività. Allo stesso modo, nessun sito Web obbliga i suoi visitatori a effettuare micropagamenti per visitare il sito in modo da coprire i costi di hosting. Pertanto, le applicazioni decentralizzate non dovrebbero costringere i propri clienti a pagare direttamente la blockchain per l'uso della stessa.

Una blockchain lanciata che utilizza il software EOS.IO non richiede che i suoi utenti paghino la blockchain direttamente per il suo utilizzo e pertanto non limita o impedisce a un'azienda di determinare la propria strategia di monetizzazione per i propri prodotti.

## Capacità di Delegazione

Un detentore di token su una blockchain lanciata che adotta il software EOS.IO che potrebbe non avere la necessità immediata di consumare tutta o parte della bandwidth disponibile, può dare o affittare tale bandwidth non consumata ad altri; i produttori di blocchi che eseguono il software EOS.IO su tale blockchain riconosceranno questa delega di capacità e assegneranno di conseguenza la bandwidth.

## Separazione dei Costi di Transazione dal Valore dei Token

Uno dei principali vantaggi del software EOS.IO è che la quantità della bandwidth disponibile per un'applicazione è completamente indipendente dal prezzo dei token. Se il proprietario di un'applicazione detiene un numero rilevante di token su una blockchain che adotta il software EOS.IO, l'applicazione può essere eseguita a tempo indeterminato all'interno di uno stato fisso e dell'utilizzo della bandwidth. In tal caso, gli sviluppatori e gli utenti non sono influenzati da alcuna volatilità dei prezzi del mercato dei token e quindi non dipendono da un feed dei prezzi. In altre parole, una blockchain che adotta il software EOS.IO consente ai produttori di blocchi di poter aumentare naturalmente la bandwidth, il calcolo e l'archiviazione disponibili per il token indipendentemente dal valore di quest'ultimo.

Una blockchain che utilizza il software EOS.IO premia i produttori di blocchi con token ogni volta che producono un blocco. Il valore dei token influirà sulla quantità della bandwidth, dello spazio di archiviazione e del calcolo che un produttore può permettersi di acquistare; questo modello sfrutta naturalmente i valori dei token in aumento in modo da aumentare le prestazioni della rete.

## Costi di Archiviazione dello Stato

Mentre la bandwidth e il calcolo computazionale possono essere delegati, l'archiviazione dello stato dell'applicazione richiederà allo sviluppatore di applicazioni di conservare i token fino a quando tale stato non viene eliminato. Se lo stato non viene mai cancellato, i token vengono effettivamente rimossi dalla circolazione.

Ogni account di un qualunque utente richiede una certa quantità di spazio di archiviazione; pertanto, ogni account deve mantenere un saldo minimo. Con l'aumentare della capacità di archiviazione della rete, questo equilibrio minimo richiesto diminuirà.

## Ricompense di blocco

Una blockchain che adotta il software EOS.IO assegnerà nuovi token a un produttore di blocchi ogni volta che viene prodotto un blocco. In queste circostanze, il numero di token creati è determinato dalla mediana della retribuzione desiderata pubblicata da tutti i produttori di blocchi. Il software EOS.IO può essere configurato per imporre un limite alle ricompense del produttore in modo tale che l'aumento annuo totale della disponibilità di token non superi il 5%.

## Applicazioni a Beneficio della Comunità

Oltre a eleggere i produttori di blocchi, in base a una blockchain basata sul software EOS.IO, gli utenti possono scegliere 3 applicazioni a beneficio della comunità, note anche come smart contract. Queste 3 applicazioni riceveranno token fino a una percentuale configurata della disponibilità di token all'anno meno i token che sono stati pagati ai produttori di blocchi. Questi smart contract riceveranno token proporzionali ai voti che ogni applicazione ha ricevuto dai titolari dei token. Le applicazioni elettive o gli smart contract possono essere sostituiti da nuove applicazioni o da smart contract da parte dei titolari di token.

# Governanza

La governanza è il processo attraverso il quale le persone raggiungono il consenso su questioni soggettive che non possono essere acquisite interamente da algoritmi software. Una blockchain basata su software EOS.IO implementa un processo di governanza che affronta l'influenza esistente dei produttori di blocchi. In assenza di un processo di governanza definito, le blockchain precedenti si basavano su processi di governanza ad hoc, informali e spesso controversi che hanno prodotto esiti imprevedibili.

Una blockchain basata sul software EOS.IO riconosce che il potere ha origine dai titolari di token che delegano tale potere ai produttori di blocchi. Ai produttori di blocchi viene concessa un'autorizzazione limitata e controllata che consente di bloccare gli account, di aggiornare le applicazioni difettose e di proporre modifiche difficoltose riguardanti il protocollo sottostante.

Incorporato nel software EOS.IO vi è l'elezione dei produttori di blocchi. Prima che qualsiasi cambiamento possa essere apportato alla blockchain questi produttori di blocchi devono approvarlo. Se i produttori di blocchi rifiutano di apportare modifiche desiderate dai titolari dei token, questi produttori di blocchi possono essere rimossi. Se i produttori di blocchi apportano modifiche senza il permesso dei titolari di token, tutti gli altri validatori del node completo non in produzione (exchanges, ecc.) rifiuteranno la modifica.

## Congelamento dei Conti

A volte un contatto intelligente si comporta in modo aberrante o imprevedibile e non riesce a funzionare come previsto; altre volte un'applicazione o un account possono scoprire un exploit che gli consente di consumare una quantità irragionevole di risorse. Quando si verificano inevitabilmente tali problemi, i produttori di blocchi hanno il potere di correggere tali situazioni.

I produttori di blocchi di tutte le blockchain hanno il potere di selezionare quali transazioni sono incluse in blocchi che danno loro la possibilità di congelare o meglio bloccare gli account. Una blockchain che utilizza il software EOS.IO formalizza questa autorità sottoponendo il processo di congelamento di un account a un voto di 17/21 dei produttori attivi. Se i produttori abusano del potere, questi possono essere rimossi e l'account verrà sbloccato.

## Modifica del Codice dell'Account

Una volta fallite tutte le possibilità e un'applicazione "inarrestabile" agisce in modo imprevedibile, una blockchain che utilizza il software EOS.IO consente ai produttori di blocchi di sostituire il codice dell'account senza effettuare l'hard fork dell'intera blockchain. Simile al processo di congelamento di un account, questa sostituzione del codice richiede un voto di 17/21 dai produttori di blocchi eletti.

## Costituzione

Il software EOS.IO consente alle blockchain di stabilire un contratto di servizio peer-to-peer o un contratto vincolante tra gli utenti che lo firmano, definito "costituzione". Il contenuto di questa costituzione definisce gli obblighi tra gli utenti che non possono essere interamente applicati mediante codice, inoltre facilita la risoluzione delle controversie stabilendo la giurisdizione e la scelta della legge insieme ad altre regole reciprocamente accettate. Ogni transazione trasmessa sulla rete deve incorporare l'hash della costituzione come parte della firma e quindi vincola esplicitamente il firmatario del contratto.

La costituzione definisce anche l'intento comunemente leggibile del protocollo del codice sorgente. Questo intento viene utilizzato per identificare la differenza tra un bug e una funzionalità quando si verificano errori e guida la comunità su quali correzioni sono giuste o improprie.

## Aggiornamento del Protocollo & Costituzione

Il software EOS.IO definisce un processo mediante il quale il protocollo come definito dal codice sorgente canonico e dalla sua costituzione, può essere aggiornato utilizzando il seguente processo:

1. I produttori di blocchi propongono una modifica della costituzione e ottengono 17/21 approvazioni.
2. I produttori di blocchi mantengono 17/21 approvazioni per 30 giorni consecutivi.
3. Tutti gli utenti sono tenuti a firmare le transazioni utilizzando l'hash della nuova costituzione.
4. I produttori di blocchi adottano le modifiche al codice sorgente per riflettere il cambiamento nella costituzione e lo propongono alla blockchain usando l'hash di un commit git.
5. I produttori di blocchi mantengono 17/21 approvazioni per 30 giorni consecutivi.
6. Le modifiche al codice vanno in vigore 7 giorni dopo, dando a tutti i nodi completi 1 settimana per effettuare l'aggiornamento dopo la ratifica del codice sorgente.
7. Tutti i nodi che non si aggiornano al nuovo codice si spengono automaticamente.

Per impostazione predefinita del software EOS.IO, il processo di aggiornamento della blockchain per aggiungere nuove funzionalità richiede dai 2 ai 3 mesi, mentre per aggiornamenti che correggono bug non critici e che non richiedono modifiche alla costituzione possono necessitare da 1 a 2 mesi.

### Modifiche di Emergenza

I produttori di blocchi possono accelerare il processo se è necessaria una modifica del software volta a correggere un bug nocivo o un exploit di sicurezza che sta danneggiando attivamente gli utenti. In generale, potrebbe essere contro la costituzione effettuare aggiornamenti accelerati che introducono nuove funzionalità o che correggono bug innocui.

# Script & Macchine virtuali

Il software EOS.IO sarà prima di tutto una piattaforma per il coordinamento della consegna di messaggi autenticati agli account. I dettagli del linguaggio di scripting e della macchina virtuale sono dettagli specifici dell'implementazione che sono per lo più indipendenti dal design della tecnologia di EOS.IO. Qualsiasi linguaggio o macchina virtuale che sia deterministica e che sia in una sandbox adeguata con prestazioni sufficienti può essere integrata con l'API del software EOS.IO.

## Messaggi Definiti dallo Schema

Tutti i messaggi inviati tra account sono definiti da uno schema che fa parte dello stato del consensus della blockchain. Questo schema consente una conversione perfetta tra la rappresentazione binaria e JSON dei messaggi.

## Database Definito dallo Schema

Anche lo stato del database viene definito utilizzando uno schema simile. Ciò garantisce che tutti i dati memorizzati da tutte le applicazioni siano in un formato che può essere interpretato come JSON comunemente leggibile, ma memorizzato e manipolato con la stessa efficienza del binario.

## Separazione dell'Autenticazione dall'Applicazione

Per massimizzare le opportunità di parallelizzazione e ridurre al minimo il debito computazionale associato alla rigenerazione dello stato dell'applicazione dal log delle transazioni, il software EOS.IO separa la logica di convalida in tre sezioni:

1. Convalida che un messaggio sia internamente coerente;
2. Convalida che tutte le condizioni preliminari siano valide; e
3. Modifica dello stato dell'applicazione.

La convalida della coerenza interna di un messaggio è di sola lettura e non richiede l'accesso allo stato della blockchain. Ciò significa che può essere eseguito con il massimo parallelismo. La convalida delle precondizioni, come il saldo richiesto, è di sola lettura e pertanto può anche questo beneficiare del parallelismo. Solo la modifica dello stato dell'applicazione richiede l'accesso in scrittura e deve essere elaborata in sequenza per ogni applicazione.

L'autenticazione è il processo di sola lettura che verifica se un messaggio può essere applicato. L'applicazione sta effettivamente effettuando il lavoro. In tempo reale è necessario eseguire entrambi i calcoli, tuttavia una volta inclusa una transazione nella blockchain non è più necessario eseguire le operazioni di autenticazione.

## Architettura Indipendente della Macchina Virtuale

È intenzione della blockchain basata sul software EOS.IO che sia possibile supportare più macchine virtuali e nuove macchine virtuali aggiunte in futuro a seconda delle necessità. Per questo motivo, questo documento non discuterà i dettagli di un particolare linguaggio di programmazione o macchina virtuale. Detto questo, vi sono due macchine virtuali che vengono attualmente valutate per l'uso di una blockchain basata sul software EOS.IO.

### Web Assembly (WASM)

Web Assembly è uno standard Web emergente per la creazione di applicazioni Web ad alte prestazioni. Con alcune piccole modifiche, il Web Assembly può essere reso deterministico e in modalità sandbox. Il vantaggio di Web Assembly è il supporto diffuso dell'industria e che consente di sviluppare contratti in linguaggi familiari come C o C ++.

Gli sviluppatori di Ethereum hanno già iniziato a modificare il Web Assembly per fornire sandboxing e determinismo idonei con il loro [Ethereum flavored Web Assembly (WASM)](https://github.com/ewasm/design). Questo approccio può essere facilmente adattato e integrato con il software EOS.IO.

### Macchina Virtuale di Ethereum (EVM)

Questa macchina virtuale è stata utilizzata per la maggior parte degli smart contract esistenti e potrebbe essere adattata per funzionare all'interno di una blockchain EOS.IO. È concepibile che i contratti EVM possano essere eseguiti all'interno della propria sandbox in una blockchain basata sul software EOS.IO e che con alcuni contratti di adattamento EVM si possa comunicare con altre applicazioni blockchain del software EOS.IO.

# Comunicazione Inter-Blockchain

Il software EOS.IO è progettato per facilitare la comunicazione inter-blockchain. Ciò si ottiene rendendo semplice la dimostrazione dell'esistenza del messaggio e la dimostrazione della sequenza dei messaggi. Queste dimostrazioni combinate con un'architettura su applicazione progettata per il passaggio dei messaggi, consentono di nascondere i dettagli della comunicazione tra blockchain (inter-blockchain) e la convalida delle prove dagli sviluppatori di applicazioni.

<img align="right" src="http://eos.io/wpimg/Diagram1.jpg" width="362.84px" height="500px" />

## Prova di Merkle per la Validazione del Light Client (LVC)

L'integrazione con altre blockchain è molto più semplice se i client non devono elaborare tutte le transazioni. Dopotutto, un exchange si preoccupa solo dei trasferimenti IN e OUT dall'exchange e nulla più. Sarebbe anche ideale se la catena di scambio (c.d. exchange chain) potesse utilizzare prove di deposito merkle leggere piuttosto che doversi fidare interamente dei propri produttori di blocchi. Quantomeno i produttori di blocchi di una catena vorrebbero mantenere il più piccolo overhead possibile durante la sincronizzazione con un'altra blockchain.

L'obiettivo di LCV è di abilitare la generazione di prove di esistenza (c.d. proof of existence) relativamente leggere che possano essere convalidate da chiunque tenga traccia di un set di dati relativamente leggero. In questo caso l'obiettivo è dimostrare che una particolare transazione è stata inclusa in un particolare blocco e che il blocco è incluso nella cronologia verificata di una determinata blockchain.

Bitcoin supporta la convalida delle transazioni, presupponendo che tutti i nodi abbiano accesso alla cronologia completa delle intestazioni dei blocchi che ammontano a 4MB di intestazioni dei blocchi all'anno. A 10 transazioni al secondo, una prova valida richiede circa 512 byte. Questo funziona correttamente per una blockchain con un intervallo di 10 minuti, ma non è più considerato "leggero" per una blockchain con un intervallo di 3 secondi.

Il software EOS.IO consente prove leggere per chiunque abbia un'intestazione di blocco irreversibile dopo il momento in cui è stata inclusa la transazione. Utilizzando la struttura collegata ad hash mostrata, è possibile provare l'esistenza di qualsiasi transazione con una prova inferiore a 1024 byte di dimensione. Se si presuppone che i nodi di convalida mantengano il controllo di tutte le intestazioni di blocco nel giorno passato (2 MB di dati), la dimostrazione di queste transazioni richiederà solo delle prove di 200 byte.

C'è un piccolo overhead incrementale associato alla produzione di blocchi con un hash-linking appropriato per abilitare queste prove, il che significa che non c'è motivo per non generare blocchi in questo modo.

Quando arriva il momento di convalidare le prove su altre catene vi è una grande varietà di ottimizzazioni tempo/ spazio/ bandwidth che possono essere effettuate. Tracciare tutte le intestazioni dei blocchi (420 MB/anno) manterrà le dimensioni delle prove marginali. Tracciare solo le intestazioni recenti può offrire un compromesso tra archiviazione a lungo termine e dimensioni della prova. In alternativa, una blockchain può usare un approccio di valutazione semplice in cui ricorda gli hash intermedi delle prove passate. Le nuove prove devono includere solo i collegamenti allo sparse tree conosciuto. L'approccio esatto utilizzato dipenderà necessariamente dalla percentuale di blocchi estranei che includono le transazioni a cui fa riferimento la prova di merkle.

Dopo una certa densità di interconnessione diventa più efficiente avere semplicemente una catena che contiene l'intera cronologia dei blocchi di un'altra catena ed eliminare in modo assoluto la necessità delle prove. Per motivi di prestazioni, è ideale ridurre al minimo la frequenza delle prove tra catene (c.d. inter-chain proofs).

## Latenza della Comunicazione Interchain

Quando si comunica con una blockchain esterna, i produttori di blocchi devono attendere fino a quando non vi è la certezza del 100% che una transazione sia stata confermata in modo irreversibile dall'altra blockchain prima di accettarla come input valido. Utilizzando una blockchain basata sul software EOS.IO e DPOS con 3 secondi per ogni blocco e con 21 produttori, sono necessari circa 45 secondi. Se i produttori di blocchi di una catena non aspettano l'irreversibilità, sarebbe come se un exchange accettasse un deposito che è stato successivamente annullato e che potrebbe influire sulla validità del consensus della blockchain.

## Prova di Completezza

Quando si usano le prove di merkle da blockchain esterne, c'è una differenza significativa tra il sapere che tutte le transazioni processate sono valide e il sapere che nessuna transazione è stata saltata o omessa. Sebbene sia impossibile dimostrare che tutte le transazioni più recenti siano conosciute, è possibile dimostrare che non ci sono stati vuoti nella cronologia delle transazioni. Il software EOS.IO facilita ciò assegnando un numero di sequenza a ogni messaggio consegnato a ogni account. Un utente può utilizzare questi numeri di sequenza per dimostrare che tutti i messaggi destinati a un determinato account sono stati elaborati e che sono stati elaborati in ordine.

# Conclusione

Il software EOS.IO è progettato in base all'esperienza di concetti collaudati e di pratiche ottimali e rappresenta un progresso fondamentale nella tecnologia della blockchain. Il software fa parte di un modello olistico per una sistema blockchain scalabile a livello globale in cui le applicazioni decentralizzate possono essere facilmente distribuite e governate.