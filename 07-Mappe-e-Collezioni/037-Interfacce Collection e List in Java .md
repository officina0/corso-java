Nella programmazione abbiamo spesso bisogno di gestire dinamicamente insiemi di dati all’interno di particolari strutture (**collezioni  
di elementi**),  
desideriamo quindi aggiungere, rimuovere o visualizzare elementi. In Java una buona pratica nella scelta della  
struttura di raggruppamento è partire dalle interfacce di alto livello che descrivono i tipi  
base in modo astratto, scegliendo successivamente la classe che implementa il tipo base, attraverso considerazioni  
che riguardano la velocità di esecuzione e il consumo di memoria. A questi aspetti ci si riferisce  
con i termini di **costo di tempo e di spazio** di un modulo software.

L’interfaccia `java.util.Collection` descrive il tipo astratto collezione tramite metodi di cui  
quelli di uso più frequente sono:

```
boolean add(E e)
boolean remove(Object o)
boolean contains(Object o)
boolean isEmpty()
void clear()
int size()
Iterator<E> iterator()
```

Con `add()` aggiungiamo un elemento alla collezione, `remove()` lo rimuove, mentre con contains() ne testiamo la presenza  
nella collezione. `isEmpty()` consente di verificare se la collezione è vuota (true).  
`clear()` elimina ogni elemento dalla collezione mentre `size()` ne restituisce il totale.

Per iterare  
gli elementi della collezione possiamo far uso di un oggetto che implementa l’interfaccia  
Iterator<E> attraverso l’invocazione di `iterator()`.  
L’interfaccia Iterator ha quattro metodi di cui i tre fondamentali sono:

```
boolean hasNext()
E next()
void remove()
```

`hasNext()` verifica durante l’iterazione e attraverso la restituzione  
del valore true la presenza di un elemento, `next()` ne consente il recupero  
mentre `remove()` la rimozione.

`java.util.Collection` è estesa da `java.util.List<E>` e `java.util.Queue<E>`  
per realizzare i tipi **lista** e **coda**. Una lista mantiene i suoi elementi rispettando l’ordine d’inserimento,  
ogni elemento in lista ha un indice con valore di partenza 0. L’interfaccia `List` aggiunge metodi  
per l’indicizzazione degli elementi:

```
add(int index, E element)
E get(int index)
int indexOf(Object o)
E remove(int index)
```

`add()` aggiunge l’elemento nella posizione specificata dal parametro `index`, get() consente il recupero di un elemento  
in una particolare posizione, `indexOf()` recupera l’indice di un elemento della lista e remove()  
rimuove di un elemento tramite il suo indice.

Le classi più utilizzate che implementano `List` sono `java.util.Vector`, `java.util.Stack` e `java.util.ArrayList`. `Vector` fornisce un’implementazione sincronizzata dei metodi di `List` risultando **Thread Safe**. `ArrayList` è identica a `Vector` ma con metodi non sincronizzati.

`Stack` estende `Vector` aggiungendo i metodi `push()`, `pop()` e `peek()` per il supporto LIFO (_Last in first out_) di un dato Pila. Un’alternativa più completa suggerita dalle **API Java 8** è utilizzare una classe che implementa l’interfaccia  
`java.util.Deque` come `java.util.ArrayDeque<E>` al posto di `Stack`.