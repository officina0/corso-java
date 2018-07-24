# 093 Collection Thread Safe

Poniamoci nel caso di programmazione concorrente e diciamo di utilizzare una struttura dati condivisa. Sarebbe interessante vedere le percentuali di utilizzo delle strutture dati che ognuno di noi sceglierebbe. Dico questo perché a parte Vector ed Hashtable le altre strutture dati non sono sincronizzate. Che succede se vogliamo utilizzare un’altra struttura dati che performi in maniera differente rispetto alle precedenti \(una lista concatenata, un albero, ecc\)?

Ovviamente questo è stato un problema piuttosto sentito in Java, e infatti a partire da Java 1.5 tante sono le novità riguardo alla programmazione concorrente, la prima che vediamo in questo capitolo è appunto l’ingresso di decine di nuove strutture dati, in particolare strutture che consentono l’accesso Thread-safe.

In generale vedremo che il modello di programmazione concorrente è cambiato abbastanza, soprattutto con la versione 1.5 del linguaggio. Non si tratta di semplici modifiche cosmetiche ma di veri e propri cambi nel paradigma, soprattutto nel tema della sincronizzazione e di come questa venga gestita dalla macchina virtuale Java. Le modifiche vanno in due direzioni: migliorare il processo di produzione del codice e migliorare le performance, che in tema di concorrenza è un concetto piuttosto complesso vista l’architettura non nativa di Java.

La prima cosa da conoscere è il package _java.util.concurrent_, che di fatto racchiude tutte le migliorie apportate alla programmazione concorrente Java. Discuteremo di buona parte delle sue classi durante le prossime lezioni, in questa ci occupiamo delle implementazioni Thread-safe delle collection.

Riporto di seguito la lista di collezioni Thread-safe che a partire da Java 1.5 potrete utilizzare per i vostri programmi concorrenti:

* ConcurrentHashMap: si tratta dell’implementazione thread safe da utilizzare per una HashMap;
* ConcurrentLinkedQueue: una lista concatenata \(sempre thread-safe\) semplice;
* ConcurrentSkipListMap: implementazione thread-safe di una mappa navigabile \(classe NavigableMap\);
* ConcurrentSkipListSet: implementazione thread-safe di una set navigabile \(classe NavigableSet\);
* CopyOnWriteArrayList: quest’implementazione è utile per liste accedute maggiormente con operazioni di lettura. In pratica la classe si trasforma in thread-safe al momento dell’accesso in scrittura \(implementazione di un ArrayList sincronizzato\);
* CopyOnWriteArraySet: stessa cosa della classe precedente, solo che qui si tratta dell’implementazione di un ArraySet.

Generalmente le classi implementano solo i metodi di scrittura come concorrenti per una questione ovvia di performance, mantenendo peraltro la consistenza inalterata.

Quindi, a partire da Java 1.5, potete utilizzare direttamente un’implementazione concreta delle precedenti senza stare lì a preoccuparvi degli accessi concorrenti agli elementi di una struttura dati.

