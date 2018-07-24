# 095 Gestione Delle Code

Una limitazione evidente nell’uso delle code è quello del loro essere limitate ma soprattutto quello di non avere il concetto dell’attesa che, spesso, in situazioni di sviluppo reale è addirittura un requisito. Attraverso l’uso di monitor e semafori è senza dubbio possibile realizzare questa attesa ma un comportamento del genere non è direttamente gestito dalla struttura dati ma “esternalizzato” in maniera programmatica.

Sun ha pensato a questa situazione tipica, che si manifesta in molti sistemi a coda reale, come l’acquisto di un biglietto in un sistema elettronico, la gestione delle code alle banche o qualsiasi sistema dove l’attesa è una situazione contemplata. Il package _java.util.concurrent_ vede l’introduzione dell’[interfaccia BlockingQueue](http://java.sun.com/j2se/1.5.0/docs/api/java/util/concurrent/BlockingQueue.html) e delle relative implementazioni concrete:

* ArrayBlockingQueue;
* DelayQueue;
* LinkedBlockingQueue;
* PriorityBlockingQueue;
* SynchronousQueue.

Il comportamento dell’interfaccia \(che estende anche Collection e Queue\) è quello di estendere le funzionalità della coda tradizionali avendo però i metodi `take()` e `put()` \(rimozione ed inserimento\) bloccanti per il thread che li invoca, avendo appunto il comportamento di coda con attesa \(illimitata, diciamo per ora\).

Per meglio capire, creeremo una serie di thread Consumer che consumano i messaggi testuali di una coda e un solo thread Producer che li produce, condividendo la stesssa coda.

Listato 20.1. Simula il problema delle attese in caso di coda piena

package it.html.threads;

import java.util.concurrent.BlockingQueue;  
import java.util.concurrent.LinkedBlockingQueue;

public class MainTest {

public static void main\(String\[\] args\) {  
//Creiamo un’istanza di coda “blocking”  
BlockingQueue queue = new LinkedBlockingQueue\(\);

```text
//Processi consumer  
Consumer c1=new Consumer(queue);  
Consumer c2=new Consumer(queue);  
Consumer c3=new Consumer(queue);  
Consumer c4=new Consumer(queue);  
Consumer c5=new Consumer(queue);  

System.out.println(“Starting Consumers….”);  
c1.start();c2.start();c3.start();c4.start();c5.start();  

//Processo producer  
Producer p = new Producer(queue);  

System.out.println(“Starting Consumers….”);  
p.start();  
```

}  
}

Il main si spiega da solo, è piuttosto semplice intuirne il comportamento.

Listato 20.2. Classe che simula il consumo di messaggi di testo

class Consumer extends Thread{

//teniamo un counter a livello di classe  
static int id;

int code;  
BlockingQueue queue;

public Consumer\(BlockingQueue queue\) {  
this.queue = queue;  
code=++id;  
}

public void run\(\){  
//ciclo infinito  
while\(true\){  
try {  
System.out.println\(“Thread\#”+code+” waiting for the message…”\);  
String message = queue.take\(\);  
System.out.println\(“Thread\#”+code+” –&gt; “+message+” taken!”\);  
//riposa 2 secondi  
sleep\(2000\);  
} catch \(InterruptedException e\) {  
e.printStackTrace\(\);  
}  
}  
}  
}

Il thread Consumer è altrettanto intuitivo: in un ciclo infinito si mette in attesa di ricezione un messaggio con la chiamata `queue.take()`. Questa restituisce un messaggio se presente in coda \(ed è il turno del thread\), altrimenti aspetta. In una implementazione pre Java 1.5 avremmo dovuto codificare un semaforo e trovare anche una soluzione per risolvere l’ordine di arrivo.

Listato 20.3. Classe che simula la produzione di messaggi di testo

class Producer extends Thread{

BlockingQueue queue;

public Producer\(BlockingQueue queue\) {  
this.queue = queue;  
}

public void run\(\){  
int messagecode = 0;  
//ciclo infinito  
while\(true\){  
try {  
System.out.println\(“Producing “+\(++messagecode\)\);  
queue.put\(“MESSAGE@”+messagecode\);  
System.out.println\(messagecode+” in queue”\);  
//riposa un secondo  
sleep\(1000\);  
} catch \(InterruptedException e\) {  
e.printStackTrace\(\);  
}  
}  
}  
}

Il Producer ha ovviamente il comportamento opposto, cioè pone dei messaggi in maniera ciclica e quando la coda è piena attende \(metodo `queue.put()`\). Ovviamente in questo caso eseguendo l’esempio vedrete l’attesa dei processi consumatori che sono in numero maggiore dei produttori, ma invertendo il numero di processi avremmo la situazione inversa \(attesa dei produttori\).

Come potete vedere nel codice, al momento della simulazione dell’attesa \(metodo `sleep()`\) abbiamo usato il metodo tradizionale. Una nuova classe, anzi, un’enumerazione permette di definire l’unità di tempo da utilizzare nella gestione del ciclo di vita del Thread: si tratta di `TimeUnit` e le sue unità definite \(SECONDS, MILLIS, MICROSECONDS, NANOSECONDS\).

Nel nostro esempio avremmo dovuto scrivere \(notate l’import statico\):

import static java.util.concurrent.TimeUnit.\*;  
//..  
SECONDS.sleep\(2\);

