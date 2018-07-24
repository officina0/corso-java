# 060 Comprendere Il Tipo Map Di Java

Una **Map** \(Mappa\) rappresenta il tipo astratto che definisce una struttura dati in grado di memorizzare elementi nella forma di **coppie chiave-valore**. Ogni elemento all’interno di una mappa è identificato da una determinata chiave, la chiave consente non solo di recuperare ed inserire un elemento in una mappa, ma anche di definire un ordinamento per le implementazioni che prevedono questa funzionalità.

## HashMap, HashTable e TreeMap

All’interno del linguaggio Java una Map è definita attraverso l’intefaccia `java.util.Map` e il linguaggio stesso offre diverse implementazioni. **HashMap**, **HashTable** e **TreeMap** sono quelle utilizzate più frequentemente e le caratteristiche che le differenziano sono essenzialmente le seguenti:

* HasMap non è dotata di metodi sincronizzati ma consente elementi con valore `null`;
* HashTable è dotata di metodi sincronizzati ma non consente elementi con valore `null`;
* TreeMap è una mappa che definisce un ordinamento degli elementi basato sul valore delle chiavi.

Le implementazioni di Map devono garantire efficienti operazioni di inserimento e recupero dati, ragion per cui i metodi dell’interfaccia `Map,get()` e `put()` dedicati a queste funzionalità, devono essere realizzati garantendo le migliori performance possibili.

Le implementazioni di HashMap ed HashTable raggiungono questo obiettivo offrendo una complessità di tempo costante **O\(1\)** per entrambi i metodi. TreeMap, dovendo gestire un ordinamento, non è in grado di mantenersi allo stesso livello prestazionale, una TreeMap raggiunge comunque un’ottimale complessità di tempo del tipo **O\(N\*LogN\)**, dove `N` è il numero di elementi contenuti.

Come vedremo, scrivere codice che utilizzi una HashMap, HashTable o TreeMap non presenta particolari difficoltà, l’aspetto più importante è invece rappresentato dalla conoscenza delle basi dell’implementazione interna di una Map. Per imparare ad utilizzare al meglio il tipo Map, abbiamo la necessità di comprendere i concetti di **Hash Function**, **Hash Value** e **Bucket**.

Come sappiamo la classe `Object` definisce due metodi: `equals()` e `hashCode()`. `hashCode()` rappresenta la Hash Function e il valore di ritorno dalla sua implementazione consente di recuperare un elemento da una mappa. E’ importante non dimenticare il contratto che lega `equals()` e `hashCode()`: se eseguiamo l’override di `equals()` lo dobbiamo fare anche di `hashCode()`, tenendo in considerazione che se `equals()` restituisce true su due oggetti, allora l’invocazione di `hashCode()` sui due oggetti deve restituire lo stesso valore.

Come è quindi facile intuire l’Hash Value rappresenta il valore ritornato di hashCode\(\):

public int hashCode\(\){ //implementazione che calcola l'hash value }

## Bucket

Il concetto di **Bucket** è invece meno immediato da comprendere, ma rappresenta il mattone base per l’implementazione della struttura interna di una mappa. Un Bucket è costituito da una _Linked List_, semplificata per l’uso con le mappe e differente da quella contenuta nel `package java.util`, che memorizza coppie chiave-valore attraverso oggetti di una classe denominata `Entry`. `Entry` è dichiarata internamente all’interfaccia Map.

Per comprendere il meccanismo di Hashing nell’accesso al Bucket, consideriamo l’implementazione generale del metodo `get()` per la classe HashMap:

public V get\(Object key\) { if \(key ==null\) //Codice di gestione da eseguire

```text
int hashValue = hash(key.hashCode());

//Utilizzo del valore hash per recuperare  l'entry all'interno del bucket
```

}

Il valore restituito da `hashCode()` non viene utilizzato direttamente, ma passato ad una funzione di `hash()` interna alla classe per recuperare l’effettivo valore per l’accesso al Bucket.

Il motivo dell’uso di un’ulteriore funzione di Hash è legato alla protezione da cattive implementazioni del metodo `hashCode()`. La struttura del Bucket non implementa una corrispondenza 1 a 1 tra coppie chiave-valore \(`Map.Entry`\) e valori hash. Può accadere che due oggetti distinti abbiamo lo stesso valore restituito da `hashCode()`, cosa perfettamente lecita in quanto non viola il contratto `equals()`/`hashCode()`.

Per oggetti di questo tipo avremo una posizione del Bucket non contenente un solo oggetto `Entry`, ma una lista di oggetti `Entry` caratterizzati dallo stesso valore hash. In questo caso l’individuazione dell’oggetto da recuperare avviene tramite la scansione della lista di oggetti `Entry` nella locazione del Bucket individuata dal valore hash. Si utilizza quindi `equals()` per confrontare l’oggetto chiave `key` passato come parametro del metodo `get()`, e le chiavi degli oggetti `Entry` appartenenti alla lista.

Nel successivo capitolo concluderemo la trattazione del Bucket descrivendo i parametri `capacity` e `load factor` di una Mappa.

