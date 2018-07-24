# 100 Locking

Nel capitolo precedente abbiamo introdotto alcuni concetti di sincronizzazione avanzata, mostrando l’uso di una struttura dati molto utile, il semaforo. Il concetto che vedremo qui è quello più a basso livello introdotto con la versione Java 5, il locking.

L’introduzione del locking, attraverso la classe _java.util.concurrent.locks.Lock_ ha lo scopo di mettere a disposizione uno strumento più avanzato e potente rispetto alla classica sincronizzazione, tant’è che ci riferiamo ad esso per definire la sincronizzazione avanzata.

Chi sviluppa programmazione concorrente sa benissimo che l’uso del modificatore di accesso synchronized consente l’accesso in mutua esclusione ad una porzione di codice condivisa per evitare il problema della race condition e dell’interleaving di processi concorrenti. Definendo una sezione o un metodo “synchronized” forziamo l’accesso atomico a quel blocco evitando perciò i problemi di race condition. Questo meccanismo funziona bene ed è tuttora valido ma ha alcune limitazioni che l’uso dei Lock permettono di superare:

* Multi locking;
* Timeout;
* Interruptibility.

Il primo caso è noto in letteratura come “hand over hand” e si riferisce ad esempio quando abbiamo più di una risorsa su cui effettuare il locking, quindi ad esempio prima effettuiamo il locking sulla risorsa A, poi accediamo al lock della risorsa B e liberiamo quindi la A e così via \(tipico problema di alcune strutture dati\).

Quando una classe accede a un blocco synchronized non ha modo di uscirne finché quella sezione non è liberata. In pratica c’è un’attesa indefinita per la risorsa. Il Lock permette di definire un timeout oltre il quale possiamo uscire dall’attesa \(come già abbiamo visto nel capitolo precedente\).

L’**interruptibility** è simile a quanto detto in precedenza, un Lock può essere interrotto e quindi saltare il blocco sincronizzato.

Queste nuove caratteristiche rendono possibile sviluppare evitando le tante insidie che la programmazione concorrente nasconde, grazie a queste nuove strutture dati che ci danno dei potenti strumenti di controllo del flusso concorrente. Vediamo una classe in cui abbiamo codificato dei potenziali blocchi atomici:

Listato 25.1. Classe che codifica blocchi atomici

package it.html.threads;

import java.util.concurrent.TimeUnit;  
import java.util.concurrent.locks.Lock;  
import java.util.concurrent.locks.ReentrantLock;

public class LockableAction {  
private Lock l;

public LockableAction\(\){  
//Creiamo un Reentrant lock, in modo  
//da non creare dei deadlock per cicli di lock interni  
l = new ReentrantLock\(\);  
}

public void doSomethingConcurrent\(\){  
//…svolge qualcosa non concorrente  
//acquisici il lock  
l.lock\(\);  
//…azione 1  
//…azione 2  
//…  
l.unlock\(\);  
}

public void doSomethingConcurrentLimitedWait\(\){  
//…svolge qualcosa non concorrente  
//Prova ad acquisire il lock  
try {  
//superati i 5 secondi salta  
boolean acquired = l.tryLock\(5,TimeUnit.SECONDS\);  
if \(acquired\){  
//…azione 3  
//…azione 4  
//…  
}  
} catch \(InterruptedException e\) {  
e.printStackTrace\(\);  
}finally{  
l.unlock\(\);  
}  
}  
}

La classe ha un Lock, definito concretamente come **ReentrantLock**, che è quello più sicuro in quanto evita problemi nel caso il Lock venga utilizzato internamente, come già accade con i metodi synchronized. I due metodi vengono acceduti concorrentemente da processi esterni che ne condividono un’istanza concreta.

Nel primo metodo simuliamo un blocco synchronized. Se però pensiamo che all’interno di esso possiamo definire altri Lock, allora riusciamo a capire come sia possibile sviluppare logiche più complesse \(esiste anche la classe Condition\). Il secondo metodo sperimenta il lock con timeout \(metodo `tryLock()`\) dove definiamo un timeout di 5 secondi, oltre il quale svolgiamo altre azioni.

L’esempio è servito per mostrare l’uso della classe che concretamente potrà aiutarvi in compiti di concorrenza anche abbastanza complessi. Esistono altre classi e migliorie a partire dalla versione Java 5 che abbiamo omesso \(come ad esempio il package [java.util.concurrent.atomic](http://java.sun.com/j2se/1.5.0/docs/api/java/util/concurrent/atomic/package-summary.html)\), ma i maggiori cambiamenti sono quelli che abbiamo illustrato. In generale, per avere una maggiore comprensione delle nuove caratteristiche, potete riferirvi alla documentazione del package _java.util.concurrent_ dove risiedono tutte le nuove implementazioni.

