# 090 Array Queue E Typed Collections

A partire dalla versione Java 5 c’è l’introduzione di una serie nuova di realizzazioni concrete di Collection e la modifica alla gerarchia delle collezioni con l’aggiunta del tipo Queue. Il tipo Queue \(da non confondersi con le Queue di JMS\) era una di quelle strutture dati mancanti nella gerarchia delle collezioni, spesso utilizzata per la gestione delle politiche FIFO \(First In First Out\). Ora, grazie alla sua introduzione, la gerarchia [si presenta così](http://java.html.it/articoli/leggi/2643/java-collections-framework/):

Figura 15.1. Gerarchia

![Gerarchia](http://www.html.it/guide/img/guida_java_6/Collection_Frameworks.jpg)

Queue è un nuovo tipo che eredita direttamente da Collection e presenta diverse implementazioni pratiche. Ai fini della logica della struttura dati i metodi `offer()` e `poll()` sono quelli fondamentali per l’inserimento e la rimozione di elementi secondo la logica FIFO.

Listato 15.1. Una classe di prova per effettuare test

package it.html.java5;

import java.util.LinkedList;  
import java.util.PriorityQueue;  
import java.util.Queue;

public class QueueTest {

public static void main\(String\[\] args\) {  
System.out.println\(“Ordinamento classico, secondo l’ordine d’arrivo”\);  
Queue queue= new LinkedList\(\);

```text
for (int i=0; i<50; i++){  
  queue.offer(“String #”+i);  
}  

while(!queue.isEmpty()){  
  System.out.println(queue.poll());  
}  
//..
```

Nel primo caso ci troviamo di fronte ad una situazione classica, una coda tipica, ordinata per ordine di arrivo \(quindi FIFO\). La coppia di metodi `offer()`–`poll()` ci garantisce la consistenza della coda e del suo ordine. Eseguendo il codice ci aspettiamo di vedere le stringhe ordinate per identificativo crescente. Si noti che il tipo concreto è `LinkedList` che è stato reimplementato per garantire l’estensione dell’interfaccia Queue.

Listato 15.2. Esempio di utilizzo di PriorityQueue

```text
//..  
System.out.println(“Ordinamento alfabetico, in base al compareTo()”);  
Queue<String> queuePrio= new PriorityQueue<String>();  

for (int i=0; i<50; i++){  
  queuePrio.offer(“String #”+i);  
}  

while(!queuePrio.isEmpty()){  
  System.out.println(queuePrio.poll());  
}  
```

}  
}

La coda a priorità esegue l’estrazione degli elementi secondo la priorità assegnata. Qui lasciamo il comparatore di default \(ma PriorityQueue può accettare un `Comparator<E,T>`\) in quanto lasciamo al metodo `compareTo()` dell’interfaccia Comparable il compito di effettuare la comparazione. L’esecuzione in questo caso darà la lista in ordine alfabetico:

String \#0  
String \#1  
String \#10  
String \#11  
String \#12  
String \#13  
String \#14  
String \#15  
String \#16  
String \#17  
String \#18  
String \#19  
String \#2  
String \#20  
String \#21  
String \#22

La novità più importante è l’introduzione nella gerarchia di questo nuovo tipo. Tra le altre piccole novità introdotte sicuramente dobbiamo citare l’estensione della classe di utilità _java.util.Arrays_ che presenta una serie di utilissimi metodi statici per la gestione di array mono e multidimensionali.

Nuove interfacce \(package java.util\):

* Deque;
* BlockingDeque;
* NavigationSet;
* NavigationMap;
* ConcurrentNavigableMap.

Di fatto queste rappresentano definizioni di nuove strutture dati. La prima è importante ed è direttamente collegata alla coda discussa nel capitolo precedente. Una **Deque**, è una coda a cui è possibile accedere da entrambi gli estremi \(sia in inserimento che in rimozione\). In alcune situazioni può essere il modello di struttura dati che necessitiamo, quindi Sun ha provveduto con l’implementazione all’interno del framework.

Per la gestione di sistemi multithread sarà utile l’utilizzo dell’interfaccia **BlockingDeque**, che estende dalla prima e che effettua l’attesa di spazio disponibile, nel caso di inserimento in coda piena, e l’attesa di un elemento inserito nel caso di coda vuota.

I Set e Map navigabili \(**NavigationSet** e **NavigationMap**\) definiscono il comportamento di un set e una mappa rispettivamente, permettendo delle funzioni di navigazione e attraversamento dei nodi in maniera ordinata. La versione concorrente \(**ConcurrentNavigableMap**\) verrà utilizzata nel caso di possibile accesso concorrente.

Ovviamente, per poter realizzare queste interfacce sono state realizzate le rispettive classi che elenchiamo:

* ArrayDeque;
* LinkedBlockingDeque;
* ConcurrentSkipListSet;
* ConcurrentSkipListMap;
* AbstractMap.SimpleEntry;
* AbstractMap.SimpleImmutableEntry;
* LinkedList;
* TreeSet;
* TreeMap.

Partiamo dalle ultime tre, che non sono dei nuovi tipi ma sono delle reimplementazioni, fatte per essere compatibili con le nuove interfacce. **LinkedList** ora implementa la nuova interfaccia Deque.

**ArrayDeque** invece è una nuova classe che si occupa di gestire il comportamento di una Deque in concreto. Stessa cosa dicasi per **LinkedBlockingDeque**, che si occupa di implementare l’interfaccia BlockingDeque. In generale il naming utilizzato già indica la tipologia di implementazione che quella classe vuole realizzare, come per tutte la gerarchia del Collection framework.

A partire dalla versione di Java 1.6 c’è l’introduzione dei metodi `Array.copyOf()` e `Array.copyOfRange()` che consentono di superare l’utilizzo della vecchia classe di utilità `System.arraycopy()`.

